# E-Commerce-Customer-Churn-Analysis
# E-Commerce-Customer-Churn-Analysis

MySQL analysis of e-commerce customer churn — data cleaning, transformation, and 18 business queries to uncover retention insights.

## About This Project

Customer churn is one of the biggest threats to profitability in e-commerce. This project uses MySQL to take a raw, messy customer dataset through cleaning, transformation, and exploratory analysis — with the goal of understanding *why* customers leave and what the business can do about it.

## Problem Statement

* Identify patterns and key factors driving customer churn.
* Analyze customer tenure, payment preferences, satisfaction, and purchase behavior.
* Generate insights to support customer retention and reduce churn.

## Dataset Summary

| Property      | Value            |
| ------------- | ---------------- |
| Database      | `ecomm`          |
| Primary Table | `customer_churn` |
| Total Records | 5,630 raw        |
| Total Columns | 20               |

### Column Groups

| Group                   | Fields                                                                 |
| ----------------------- | ---------------------------------------------------------------------- |
| Identity & Demographics | CustomerID, Gender, MaritalStatus, CityTier                            |
| Access & Preferences    | PreferredLoginDevice, PreferredPaymentMode, PreferredOrderCat          |
| Engagement              | Tenure, HoursSpentOnApp, NumberOfDeviceRegistered, NumberOfAddress     |
| Purchase Activity       | OrderCount, OrderAmountHikeFromlastYear, CouponUsed, DaySinceLastOrder |
| Experience              | SatisfactionScore, CashbackAmount, ComplaintReceived                   |
| Logistics               | WarehouseToHome                                                        |
| Outcome (derived)       | ChurnStatus (Active / Churned)                                         |

## Stage 1 — Cleaning the Data

**Handling missing values**

* Mean (rounded to nearest integer): `WarehouseToHome`, `HoursSpentOnApp`, `OrderAmountHikeFromlastYear`, `DaySinceLastOrder`
* Mode: `Tenure`, `CouponUsed`, `OrderCount`

**Handling Outliers**

* Dropped rows where `WarehouseToHome` > 100 km (unrealistic outliers)

**Dealing with Inconsistencies**

* `PreferredLoginDevice`: "Phone" → "Mobile Phone"
* `PreferredOrderCat`: "Mobile" → "Mobile Phone"
* `PreferredPaymentMode`: "COD" → "Cash on Delivery", "CC" → "Credit Card"

## Stage 2 — Data Transformation

**Column Renaming**

* `PreferedOrderCat` → `PreferredOrderCat`
* `HourSpendOnApp` → `HoursSpentOnApp`

**Creating New Columns**

* `ChurnStatus`: "Churned" / "Active" (from the `Churn` bit flag)
* `ComplaintReceived`: "Yes" / "No" (from the `Complain` bit flag)

**Column Dropping**

* `Churn` and `Complain` — superseded by the readable fields above

## Stage 3 — Data Exploration and Analysis (SQL Queries)

1. Churned vs. active customer count
2. Average tenure & total cashback for churned customers
3. % of churned customers who had complained
4. City tier with most churned Laptop & Accessory buyers
5. Most-used payment mode among active customers
6. Order amount hike total — single, mobile-preferring customers
7. Average devices registered among UPI users
8. City tier with the largest customer base
9. Gender with the highest coupon usage
10. Customer count & max app hours per order category
11. Order totals for Credit Card users at peak satisfaction
12. Average satisfaction score among complainers
13. Order categories favored by heavy coupon users (>5)
14. Top 3 categories by average cashback
15. Payment modes tied to 10-month average tenure & 500+ orders
16. Churn breakdown by delivery distance band
17. Married, City Tier-1 customers ordering above the average
18. Returns table setup + join against churned/complaining customers

## Repository Contents

| File                                   | What It Does                                                                          |
| -------------------------------------- | ------------------------------------------------------------------------------------- |
| `ecomm_raw_dataset.sql`                | Creates the `ecomm` database, `customer_churn` table, and loads all 5,630 raw records |
| `data_cleaning_and_transformation.sql` | Handles missing values, outliers, inconsistent labels, renames, and derived columns   |
| `data_analysis.sql`                    | All 18 business questions, plus the `customer_returns` table setup and join           |

## Built With

| Tool            | Role                                  |
| --------------- | ------------------------------------- |
| MySQL           | Data storage, cleaning, and querying  |
| MySQL Workbench | Script development and execution      |
| SQL (DDL/DML)   | Cleaning, transformation, aggregation |
