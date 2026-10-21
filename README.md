# E-Commerce Dimensional Data Pipeline with dbt Core & Snowflake

## Project Overview
This project addresses an operational analytics bottleneck at **Maven Marketplace** (an online e-commerce retailer selling consumer goods). The transactional database records raw customer transactions, orders, order items, and product data; however, the raw schema is unoptimized for business intelligence, aggregated reporting, and analytical querying.

Using **dbt Core** integrated with **Snowflake**, this project restructures raw transactional data into a clean, tested, and documented dimensional model (Star Schema).

---

## Architecture & Tech Stack

* **Data Warehouse:** Snowflake
* **Transformation Tool:** dbt Core (v1.x)
* **Environment Management:** Anaconda / Conda (`dbt_env`)
* **Authentication:** RSA Key Pair Authentication (dbt-to-Snowflake)
* **Data Modeling:** Dimensional Modeling (Star Schema: Fact & Dimension tables)
* **Version Control:** Git & GitHub

---

## Data Pipeline & Architecture

### Source Data Structure (Raw Transactions)
* `customers` – Customer metadata & registration details
* `orders` – Order timestamp and customer association
* `order_items` – Line-item product quantities and orders
* `products` – Product catalog details and unit pricing

### Transformed Data Structure (Analytical Marts)
* `stg_*` (Views): Lightly cleaned, standardized, and type-cast staging views for all raw entities.
* `dim_customer` (Table): Cleaned customer entity enriched with surrogate keys (`md5`) and derived customer loyalty segments.
* `dim_product` (Table): Product catalog entity with clean categories and standardized pricing.
* `fct_sales` (Table): Grain-level sales transaction fact table linked to customer and product dimensions via surrogate keys.
* `agg_sales_by_cat` (Table/View): Roll-up analytical summary measuring total orders, units sold, and total revenue grouped by product category and order year.

---

## Repository Structure

```text
├── macros/
│   └── cents_to_dollars.sql     # Custom Jinja macro for currency conversion
├── models/
│   ├── staging/                 # Staging models (light cleaning & casting)
│   │   ├── sources.yml          # Source definitions & metadata
│   │   ├── stg_customers.sql
│   │   ├── stg_order_items.sql
│   │   ├── stg_orders.sql
│   │   └── stg_products.sql
│   └── marts/                   # Dimensional models (business logic & star schema)
│       ├── schema.yml           # Data quality tests & column documentation
│       ├── agg_sales_by_cat.sql
│       ├── dim_customer.sql
│       ├── dim_product.sql
│       └── fct_sales.sql
├── seeds/                       # Seed CSV files for initial load
│   ├── customers.csv
│   └── orders.csv
├── dbt_project.yml              # dbt project configurations & materializations
├── packages.yml                 # External dbt packages (e.g., dbt-utils)
└── README.md
