<p align="center">
  <img src="images/data_cleaning.png" width="600" alt="SQL Data Cleaning Project">
</p>


# 🧹 SQL Data Cleaning Project: E-commerce Sales

A step-by-step data cleaning project in **SQL Server (SSMS)**. I took a messy sales dataset of 1,004 rows and turned it into a clean, analysis-ready table of 1,000 rows.

## 📌 Project Overview
Raw data is rarely ready for analysis. This project shows how I find and fix common data quality problems using SQL, and how I document the decisions I make.

## 📂 Files
| File | Description |
|------|-------------|
| `sales.csv` | Raw (uncleaned) dataset |
| `data_cleaning.sql` | Full SQL cleaning script |
| `SQL_Data_Cleaning_Project.pdf` | Step-by-step presentation (LinkedIn carousel) |

## 🔍 Problems Found in the Raw Data
| Problem | Count |
|---------|-------|
| Duplicate `transaction_id` | 4 rows |
| Negative `quantity` | 318 rows |
| Missing `total_amount` | 247 rows |
| Missing `category` | 167 rows |
| Missing `payment_method` | 138 rows |
| Missing `delivery_status` | 131 rows |
| Missing `price` | 58 rows |
| Missing `customer_address` / `customer_name` | 54 / 53 rows |
| Invalid emails (no `@`) | 103 rows |
| Inconsistent payment labels (`creditcard`, `CC`, `credit`) | 373 rows |
| Invalid / mixed date formats (e.g. `2024-02-30`) | several |

## 🛠️ Cleaning Steps
1. **Remove duplicates**: `ROW_NUMBER()` with a CTE, then delete rows where `row_num > 1`
2. **Check NULL values**: dynamic SQL with `INFORMATION_SCHEMA` to count nulls in every column
3. **Treat NULL values**
   - Text columns filled with clear labels (`Unknown`, `Not Available`, `Not Delivered`)
   - Payment labels standardised to `Credit Card`; missing payment set to `Cash` *(assumption)*
   - Missing `price` filled with the **category-wise average**
4. **Handle negative values**: `ABS()` on `quantity`, then recalculate `total_amount = price * quantity`
5. **Fix dates**: `TRY_CONVERT(DATE, ..., 103)`; impossible dates become `NULL`
6. **Fix emails**: emails without `@` set to `NULL`
7. **Fix datatypes**: `purchase_date` changed from `nvarchar` to `DATE`

## 📊 Result
| Check | Before | After |
|-------|--------|-------|
| Total rows | 1,004 | 1,000 |
| Duplicate IDs | 4 | 0 |
| Negative quantity | 318 | 0 |
| NULL category / price | 167 / 58 | 0 / 0 |
| NULL total_amount | 247 | 0 |
| `purchase_date` type | nvarchar | DATE |

`customer_id` (3 rows) and `email` (102 rows) are left as `NULL` on purpose. I did not create fake values.

## 🧠 SQL Concepts Used
CTE · `ROW_NUMBER()` · Window functions · Dynamic SQL · `STRING_AGG` · `CASE WHEN` · `TRY_CONVERT` · `PERCENTILE_CONT` · `UPDATE` / `DELETE` · `ALTER TABLE` · `INFORMATION_SCHEMA`

## ⚠️ Assumptions & Limitations
- Missing `payment_method` was filled with `Cash`. This is a business assumption.
- Negative quantities were treated as entry errors, but they could also be returns in a real business.
- 75 rows with `quantity = 0` still need review.

## 🚀 Next Steps
- Review `quantity = 0` rows
- Compare mean vs median imputation for `price`
- Build a **Power BI dashboard** on the cleaned data

## 🛠️ Tools
SQL Server Express · SSMS

## 👤 Author
**Amaan Ali**
🔗 LinkedIn: *www.linkedin.com/in/amaan-ali-64a1a832a*
