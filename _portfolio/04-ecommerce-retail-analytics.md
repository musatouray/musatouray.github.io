---
title: "E-Commerce Retail Analytics"
excerpt: "Production-grade analytics platform with automated pipelines (Airflow), Snowflake warehouse, dbt transformations, CI/CD, and Power BI dashboards."
header:
  teaser: /assets/images/ecommerce_analytics.png
  overlay_image: /assets/images/ecommerce_analytics_dbt_snowflake.png
  overlay_filter: 0.5
tags:
  - dbt
  - Snowflake
  - Power BI
  - Airflow
  - AWS S3
  - Data Engineering
---

## Project Overview

This project transforms raw e-commerce transaction data into actionable customer insights that drive revenue growth, reduce churn, and optimize marketing spend. It's a complete data platform built with the same tools and practices used by data teams at top tech companies—from automated data pipelines to interactive dashboards that answer the questions executives actually ask.

**[View Full Project on GitHub →](https://github.com/musatouray/portfolio-projects.git)**

## Key Questions Answered

| Business Question | Analytics Solution | Impact |
|-------------------|-------------------|--------|
| *Which customers should we prioritize?* | **RFM Segmentation** classifies customers into Champions, Loyal, At-Risk, and Lost segments | Focus retention efforts on high-value customers before they churn |
| *What's a customer worth over time?* | **Customer Lifetime Value** predicts 12-month revenue per customer | Optimize acquisition spend based on projected ROI |
| *Are we retaining customers?* | **Cohort Retention Analysis** tracks monthly cohorts with GRR/NRR metrics | Identify which acquisition channels produce sticky customers |
| *Who's about to leave?* | **Churn Risk Scoring** flags at-risk customers based on behavioral signals | Trigger proactive outreach before customers disappear |
| *What products sell together?* | **Market Basket Analysis** identifies co-purchase patterns | Power cross-sell recommendations and bundle offers |
| *How is the business trending?* | **Time Series Analytics** with dynamic moving averages, MoM and YoY growth | Spot trends early and compare performance across periods |

## Data Layers (Medallion Architecture)

| Layer | Location | Purpose |
|-------|----------|---------|
| **Bronze (RAW)** | DEV | Source data loaded via Airflow from S3 |
| **Silver (STAGING)** | DEV | Cleaned views with type casting and validation |
| **Gold (INTERMEDIATE)** | DEV + PROD | Enriched models with business logic |
| **Gold (MARTS)** | DEV + PROD | Fact and dimension tables for BI |

**Key Design Decisions:**
- Bronze + Silver in DEV only — No data duplication, cost efficient
- Gold layer separated — Dev and Prod environments fully isolated
- Cross-database reference — PROD reads from DEV.STAGING (single source of truth)

## Tech Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| **Orchestration** | Airflow (Docker) | Schedule pipelines, manage dependencies |
| **Storage** | AWS S3 | Stage raw files for loading |
| **Warehouse** | Snowflake | Scalable cloud analytics database |
| **Transformation** | dbt | Version-controlled SQL models with testing |
| **CI/CD** | GitHub Actions | Automated testing and deployment |
| **Visualization** | Power BI + Microsoft Fabric | Interactive dashboards with Git-based deployment |
| **Alerting** | Slack | Pipeline monitoring and notifications |

## Analytics Models

**Customer Intelligence:**
- `fct_rfm_segments` — Monthly customer segmentation snapshots
- `fct_clv_customer` — Lifetime value prediction with behavioral inputs
- `fct_cohort_retention` — Cohort-based retention tracking with GRR/NRR

**Core Analytics:**
- `fct_orders` — Order-level fact table with revenue and delivery metrics
- `fct_order_items` — Line-item detail with product, seller, and margins
- `fct_market_basket` — Product co-occurrence for cross-sell recommendations

**Dimensions:**
- `dim_customers`, `dim_products`, `dim_sellers`, `dim_dates`

## What Makes This Production-Grade

| Capability | Implementation |
|------------|----------------|
| **Automated Pipeline** | Airflow orchestrates daily data generation, loading, and transformation |
| **CI/CD** | GitHub Actions runs tests on every PR, deploys to production on merge |
| **Environment Isolation** | Separate DEV and PROD databases—changes validated before reaching dashboards |
| **Observability** | Slack alerts on pipeline failures, success summaries with row counts |
| **Interactive Dashboards** | Power BI reports deployed via Fabric Git integration |

<p>
    <iframe style="width:100%;" height="383"
        src="https://app.powerbi.com/view?r=eyJrIjoiMWYwMTA1NjMtMmZiYy00YmYzLTgxN2UtMjI3MjFhMzY0MGQ4IiwidCI6ImU3ZmRiMmEyLTUzODAtNDBmMC04MmQ4LWEzYjU0YzFmODE3ZiJ9"
        frameborder="0" allowFullScreen="true">
    </iframe>
</p>
