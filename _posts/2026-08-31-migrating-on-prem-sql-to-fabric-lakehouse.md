---
layout: single
title: "Migrating a Legacy On-Prem SQL Warehouse to a Fabric Lakehouse — The Incremental Way"
excerpt: "A fault-tolerant approach to migrating dimension and fact tables from an on-premises SQL Server to a Fabric Lakehouse Bronze layer using watermark-based incremental loads and idempotent pipelines."
date: 2026-08-31
permalink: /musings/migrating-on-prem-sql-to-fabric-lakehouse/
header:
  teaser: /assets/images/architecture.svg
  overlay_image: /assets/images/architecture.svg
  overlay_filter: 0.65
categories:
  - Musings
  - Microsoft Fabric
  - Data Engineering
tags:
  - Microsoft Fabric
  - Data Engineering
  - Fabric Pipelines
  - Lakehouse
  - Data Factory
  - Medallion Architecture
author_profile: true
share: true
related: true
---

Land it all in the Bronze layer of a medallion Lakehouse — in a way that survives being re-run (idempotent):
1. Dimensions — full reload
2. Facts — incremental load using watermark

---

## The Architecture

A legacy SQL Server warehouse for a physical metals-trading business (trades, shipments, inventory, LME prices, FX, P&L), an on-premises data gateway, and a Fabric trial.

![On-prem SQL to Fabric Bronze architecture](/assets/images/architecture.svg)

---

## The Approach

- **Dimension tables** — full reload via **Overwrite**. Simple, and always consistent.
- **Fact tables** — **Append** only what changed, tracked by a watermark control table in the Fabric Warehouse.
- **Decoupled loads** — a **parent** pipeline plus a reusable **child** pipeline it invokes.

> The reason: separation of concerns. A million/billion-row table timing out shouldn't block the other tables from loading.

---

## The Pipelines

### Parent Pipeline

![Parent pipeline in Fabric Data Factory](/assets/images/parent-pipeline.png)

- Starts with two **array parameters** for dynamic loading of the tables (also makes it easy to add future tables).

![Parent pipeline array parameters](/assets/images/pipeline-parameters.png)

- A **Script** activity to dynamically create the watermark control table if it does not exist.

```sql
IF SCHEMA_ID('ctl') IS NULL EXEC('CREATE SCHEMA ctl');
IF OBJECT_ID('ctl.LoadWatermark') IS NULL
CREATE TABLE ctl.LoadWatermark (
    TableName VARCHAR(128) NOT NULL,
    WatermarkValue BIGINT NOT NULL,
    LoadedAtUtc DATETIME2(6) NOT NULL
);
```

- **The dimensions (full reload)** — a Copy Data activity inside a ForEach that loops over the `DimensionTables` parameter, dynamically copying each table with additional metadata columns — `_ingested_at_utc`, `_source_system`, `_source_table`, `_pipeline_run_id`

![Dimensions ForEach + Copy](/assets/images/dimensions-foreach.svg)
![Dimensions full reload](/assets/images/dimensions-full-reload.png)

- **The large 10M-row FactTrade table** (could be a table of millions of records). It uses the watermark table to do an initial full load and then incrementally append new data. It needs:

![FactTrade High Level](/assets/images/facttrade-high-level.png)

- A **Lookup** activity against the watermark control table in the Fabric Warehouse to get the *old* watermark value.

![Lookup old watermark](/assets/images/get-oldwatermark.png)

```sql
SELECT CAST(ISNULL((SELECT WatermarkValue FROM ctl.LoadWatermark
WHERE TableName='@{variables('varFactTrade')}'), 0) AS BIGINT) AS OldWatermark
```

- Another **Lookup** activity against the source on-prem SQL Server to get the *new* watermark value (usually a date column or an auto-incrementing key).

![Lookup new watermark](/assets/images/get-newwatermark.png)

```sql
SELECT CAST(ISNULL(MAX(TradeKey), 0) AS BIGINT) AS NewWatermark FROM dw.@{variables('varFactTrade')}
```

- A **Copy Data** activity that pulls only the rows between the old and new watermark. If there's nothing new, the query returns no rows and nothing is appended.

![FactTrade incremental copy](/assets/images/facttrade-source-sink.png)

```sql
SELECT * FROM dw.@{variables('varFactTrade')} WHERE TradeKey > @{activity('Get_OldWatermark').output.firstRow.OldWatermark} AND TradeKey <= @{activity('Get_MaxTradeKey').output.firstRow.NewWatermark} AND ?DfDynamicRangePartitionCondition
```

- **Retry-safety (idempotency):** If a copy fails mid-transfer (for whatever reason), a small Spark notebook deletes the partially-loaded rows first, then re-appends cleanly — no duplicates. The watermark only advances *after* a fully successful copy.

> **Note:** On a large table, enable **dynamic-range partitioning** to split the read into parallel slices (instead of one long stream that risks timing out), and set a sensible *degree of copy parallelism*. You **must** add the partition condition `?DfDynamicRangePartitionCondition` to the query, or the pipeline fails at runtime.

- Finally, a **Script** activity to update the watermark control table with the latest value, after the copy succeeds.

![Update watermark](/assets/images/update-watermark.png)

```sql
DELETE FROM ctl.LoadWatermark WHERE TableName='@{variables('varFactTrade')}';
INSERT INTO ctl.LoadWatermark (TableName, WatermarkValue, LoadedAtUtc)
VALUES ('@{variables('varFactTrade')}', @{activity('Get_MaxTradeKey').output.firstRow.NewWatermark}, SYSUTCDATETIME());
```

- **Skip idle runs** — the delete → copy → update activities sit inside an **If Condition** that only fires when the new max key is greater than the stored watermark. A run with nothing new skips the whole branch eliminates unnecessary compute costs, prevents gateway congestion, and completely bypasses Spark cold-start latency when there's no data to process.

- **On failure** — each Copy's fail path posts to a **Teams** channel and then triggers a **Fail** activity, so you're alerted *and* the pipeline still reports failure.

![If Condition](/assets/images/if-condition.png)

### The Child Pipeline

![Child pipeline](/assets/images/child-pipeline.png)

The remaining fact tables all follow the *same* watermark pattern as FactTrade, so instead of copy-pasting those activities per table, I factored them into **one reusable child pipeline**, parameterized by `TableName` and `WatermarkColumn`.

The **parent** drives it: a `ForEach` over the `IncrementalFacts` parameter runs an **Invoke Pipeline** activity that calls the child and contains the following activities:

1. **Lookup** — old watermark from `ctl.LoadWatermark` in the Fabric Warehouse.
2. **Lookup** — new watermark (`MAX(key)`) from the source on-prem SQL Server.
3. **If Condition** — Evaluates if new records exist to avoid idle runs.
4. **Spark Notebook** — Clears out partially transferred drift to ensure idempotency
5. **Copy Data** — Appends the clean delta.
6. **Script** — update `ctl.LoadWatermark` with the new value.

> **Note:** those four activities could have lived directly inside the `OtherFacts_IncrementalLoad` loop, but pulling them into a parameterized child pipeline makes each fact **independently testable and debuggable** — I can run one small table without kicking off the whole the parent.

---

## Alerting on Failure

I added a **Teams Notification** activity onto the `on fail` path of each Copy activity — posting to a Teams channel with the failed table, the error message, the watermark bounds, and the run id.

However, to ensure that the pipeline does not treat the failure as a successful run, I added a Failed activity to the notification success node to re-raise the error so the pipeline terminates as failed.

---

## The Gotchas

These failed runs taught me the most:

- **Fabric Warehouse errs when you use standard `DATETIME2`** datatype when creating a table (*"An integer precision value between 0 and 6 must be specified"*). It wants `DATETIME2(6)`

- **Dynamic-range partitioning with a custom query** requires the `?DfDynamicRangePartitionCondition` token in the `WHERE` clause, or the copy will fail at runtime:

```sql
SELECT * FROM dw.@{variables('varFactTrade')} WHERE TradeKey > @{activity('Get_OldWatermark').output.firstRow.OldWatermark} AND TradeKey <= @{activity('Get_MaxTradeKey').output.firstRow.NewWatermark} AND ?DfDynamicRangePartitionCondition
```

- **A failure alert can hide the failure.** A Teams activity on a Copy's fail path that *succeeds* makes Data Factory mark the pipeline "Succeeded" — add a **Fail activity** after it to re-raise, or failures go green and silent. (See *Alerting on failure* above.)

---

## The Results

- The `LoadWatermark` control table in the Fabric Warehouse (this Warehouse will host the Gold layer later; for now it just holds the migration's control state).

![Watermark control table results](/assets/images/watermark-control-table.png)

- The Bronze layer holds the 9 dimensions and 6 fact Delta tables copied from the on-prem SQL Server.

![Loaded Delta tables in the Bronze layer](/assets/images/lakehouse-result.png)

---

## Production Notes

Adding notebooks into a pipeline brings two side effects worth planning for:

**1. Spark cold-start latency.** Lookup and Copy activities run in seconds, but a notebook needs a Spark session. Inside the `OtherFacts_IncrementalLoad` loop that can mean a fresh session *per table*.

- **Fix:** keep the notebooks on the **Starter Pool** (Fabric's pre-warmed pool sessions start faster than a *custom* pool), and in enable **high-concurrency for pipeline-running notebooks** so the notebook activities **share one session** across the loop instead of spinning up a new one per iteration.

**2. Delta fragmentation from frequent `DELETE`.** Each delete-range leaves small files behind; over time that degrades Bronze read performance.

- **Fix:** a scheduled (weekly/monthly) **maintenance notebook** that runs `OPTIMIZE bronze.<table>` (compacts + V-Orders small files) and `VACUUM bronze.<table>` (purges tombstoned files).

---

## The Takeaway

If you've done data migrations at scale — how do you handle idempotency on an append sink? Curious whether you'd delete-range like I did.
