# Supply Chain Analytics Pipeline

End-to-end supply chain analytics project built on Databricks with a medallion architecture (Bronze → Silver → Gold).

## Architecture

### Bronze Layer (`workspace.supply_chain_bronze`)
Raw CSV data ingested as Delta tables:
- `financial_transactions` (2,500 rows) — supplier-item transactions with revenue, cost, rebates
- `item_master` (100 rows) — product catalog with category, brand, standard cost
- `supplier_master` (20 rows) — supplier profiles with capacity, quality scores
- `supplier_terms` (460 rows) — supplier-item contracts with rebates
- `mrp` (900 rows) — Maximum Retail Price by item-region-date
- `cdp_price` (460 rows) — Cost Delivered Price by item-supplier-date
- `market_data_idc` (400 rows) — market demand, price, share by item-region
- `revenue_units` (3,600 rows) — monthly revenue units by item-region
- `margin_units` (3,600 rows) — monthly margin units by item-region

### Silver Layer (`workspace.supply_chain_silver`)
Cleaned, deduplicated, FK-validated tables with `_ingested_at` audit columns. 69 data quality checks (all passing) covering row counts, duplicates, nulls, FK integrity, value ranges, and domain enums.

### Gold Layer (`workspace.supply_chain_gold`)
Business-ready aggregation tables:
- `g_item_performance` (100 rows) — item-level KPIs with financial metrics
- `g_supplier_performance` (20 rows) — supplier scorecard with capacity utilization
- `g_monthly_sales_summary` (972 rows) — monthly trends by region and category
- `g_pricing_analysis` (363 rows) — MRP vs CDP vs market price gap analysis

## Notebooks
1. **Supply Chain Data Ingestion** — bronze layer ingestion
2. **Supply Chain Silver Gold Transformations** — silver + gold transformations + DQ checks

## Pipeline
- Daily job at 08:00 UTC on serverless compute
- Task 1: Bronze ingestion → Task 2: Silver/Gold transformations + DQ checks

## Dashboard
AI/BI Dashboard with 4 pages: Executive Overview, Supplier Scorecard, Monthly Sales Trends, Pricing Analysis

## Data Coverage
- 100 items across 7 categories (Accessories, Storage, Monitor, Server, Desktop, Laptop, Networking)
- 20 suppliers across 4 regions (North America, Europe, Asia Pacific, Latin America)
- Date range: Jan 2026 – Sep 2026
- Currency: USD
