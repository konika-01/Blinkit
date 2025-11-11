# Blinkit - SQL Data Cleaning

## Project Overview

This repository contains the SQL data cleaning work performed for the Blinkit dataset. The objective was to clean, standardize, and prepare the raw datasets (orders, customers, products, transactions, refunds, feedback, etc.) for downstream analysis and reporting in Power BI.

## Purpose

* Remove duplicates, fix malformed records, and normalize columns.
* Convert and standardize date/time fields for accurate time-based analysis.
* Impute or remove missing values depending on business logic.
* Enforce consistent categorical labels (e.g.,  `category`).
* Produce a clean, analysis-ready set of tables and sample views used by the Power BI dashboard.

## Dataset (brief)

The original raw tables provided (or extracted) for cleaning included:

* `customers`
* `orders`
* `order_itmes`
* `products`
* `delivery date and time`
* `inventory`
* `feedback`


## Key cleaning steps:
* **Duplicate detection:**

```sql
SELECT order_id, COUNT(*) AS cnt
FROM orders
GROUP BY order_id
HAVING cnt > 1;
```

* **Removing exact duplicate rows (example):**

```sql
CREATE TABLE orders_dedup AS
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY order_id, product_id, order_date ORDER BY order_date) rn
  FROM orders_raw
) t
WHERE rn = 1;
```

* **Date parsing (example):**

```sql
ALTER TABLE orders
ADD COLUMN order_date_parsed DATE;
UPDATE orders
SET order_date_parsed = STR_TO_DATE(order_date, '%d-%m-%Y');
```

* **Null counts (quick check):**

```sql
SELECT
  SUM(CASE WHEN customer_id IS NULL THEN 1 ELSE 0 END) AS missing_customer_id,
  SUM(CASE WHEN order_date IS NULL THEN 1 ELSE 0 END) AS missing_order_date
FROM orders;
```

And more with compelete EDA

---
✍️ Konika Malik.
