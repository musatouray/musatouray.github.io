---
title: "E-Commerce Retail Analytics"
excerpt: "Production-grade analytics pipeline featuring advanced SQL patterns including RFM segmentation, cohort analysis, and customer lifetime value."
header:
  teaser: /assets/images/ecommerce_analytics.png
  overlay_image: /assets/images/ecommerce_analytics_dbt_snowflake.png
  overlay_filter: 0.5
tags:
  - dbt
  - Snowflake
  - Power BI
  - SQL
  - Data Engineering
---

This project demonstrates a complete analytics pipeline built with **dbt**, **Snowflake**, and **Power BI**, showcasing production-grade data modeling patterns used by data teams at top companies to drive real business decisions.

## Key Analytics Questions Answered

### Customer Segmentation & Lifetime Value
- **RFM Analysis**: Segment customers into actionable groups (Champions, At-Risk, Lost) based on Recency, Frequency, and Monetary value
- **Customer Lifetime Value (CLV)**: Predict lifetime value of customers and identify high-ROI cohorts
- **Churn Prediction**: Identify customers at risk of churning based on behavioral signals and dormancy periods

### Revenue & Performance Analysis
- **Pareto Analysis (80/20 Rule)**: Identify which 20% of products and customers generate 80% of revenue
- **Funnel & Conversion Analysis**: Track conversion rates from order placement to delivery confirmation
- **Seller Performance Scoring**: Rank sellers using composite scores based on delivery time, reviews, and volume

### Cohort & Trend Analysis
- **Cohort Analysis**: Compare purchasing behavior across monthly acquisition cohorts and track retention curves
- **Time Intelligence**: Analyze MoM/YoY growth with 7-day and 30-day moving averages
- **Geographic Performance**: Identify regional opportunities based on average order value and delivery performance

### Market Basket Analysis
- **Co-purchase Patterns**: Discover products frequently purchased together for cross-sell optimization
- **Product Bundling**: Identify bundling opportunities based on co-occurrence matrices

## Advanced SQL Patterns

| Pattern | Business Value | Key SQL Features |
|---------|----------------|------------------|
| **RFM Analysis** | Customer segmentation | `NTILE()`, `CASE WHEN` scoring |
| **Pareto Analysis** | Focus on high-impact items | `SUM() OVER`, cumulative percentages |
| **Customer Lifetime Value** | Revenue forecasting | Cohort averages, predictive aggregations |
| **Funnel Analysis** | Conversion optimization | `COUNT(CASE WHEN...)`, stage ratios |
| **Cohort Analysis** | Retention tracking | `DATE_TRUNC`, cohort pivots |
| **Market Basket** | Cross-sell opportunities | Self-joins, co-occurrence matrices |
| **Churn Indicators** | Proactive retention | `DATEDIFF`, `LAG()`, behavioral flags |
| **Time Intelligence** | Trend analysis | `LAG()`, moving averages, YoY/MoM |
| **Seller Scoring** | Vendor management | `PERCENT_RANK()`, weighted composites |
| **Geo Performance** | Regional strategy | Location-based aggregations |

## Architecture

The project implements a **Medallion Architecture** with a 2-database pattern for environment isolation:

```
┌──────────────┐      ┌──────────────┐      ┌──────────────┐      ┌──────────────┐
│    Kaggle    │      │  Snowflake   │      │     dbt      │      │   Power BI   │
│   (Source)   │─────▶│  (Warehouse) │─────▶│ (Transform)  │─────▶│  (Visualize) │
└──────────────┘      └──────────────┘      └──────────────┘      └──────────────┘
```

**Key Design Decisions:**
- **Bronze + Silver in DEV only** — No data duplication, cost efficient
- **Gold layer separated** — Dev and Prod environments fully isolated
- **Cross-database reference** — PROD reads from DEV.STAGING (single source of truth)
- **CI/CD with GitHub Actions** — Automated testing and deployment

### Data Layers

| Layer | Purpose |
|-------|---------|
| **Bronze (RAW)** | Raw source data, immutable |
| **Silver (STAGING)** | Cleaned, typed, validated views |
| **Gold (INTERMEDIATE)** | Joined and enriched models |
| **Gold (MARTS)** | Fact and dimension tables for BI |

## Tech Stack

| Component | Tool |
|-----------|------|
| **Warehouse** | Snowflake |
| **Transform** | dbt |
| **Orchestration** | GitHub Actions CI/CD |
| **Visualization** | Power BI |
| **Package Manager** | uv (Python) |

## Data Model

**Core Fact Tables:**
- `fct_orders` — One row per order with metrics
- `fct_order_items` — One row per order item

**Customer Analytics:**
- `fct_rfm_segments` — RFM scores and customer segments
- `fct_cohort_retention` — Cohort retention metrics
- `fct_clv_customer` — Customer lifetime value calculations
- `fct_churn_risk` — Churn risk scores

**Finance & Marketing:**
- `fct_daily_revenue` — Daily revenue aggregates
- `fct_category_performance` — Category metrics by month
- `fct_geo_performance` — Geographic performance by state

**Dimensions:**
- `dim_customers`, `dim_cohorts`, `dim_dates`, `dim_products`, `dim_sellers`

<p>
    <iframe style="width:100%;" height="383"
        src="https://app.powerbi.com/view?r=eyJrIjoiYTg5ZDFjNDctMDhlYi00MTNmLTkyMWEtYmY5ODZkNjQzNjA0IiwidCI6ImU3ZmRiMmEyLTUzODAtNDBmMC04MmQ4LWEzYjU0YzFmODE3ZiJ9"
        frameborder="0" allowFullScreen="true">
    </iframe>
</p>
