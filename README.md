# Customer Shopping Behavior Analysis

End-to-end analysis of a retail customer shopping dataset (3,900 transactions) — from raw data cleaning in Python, through SQL-based business analysis in PostgreSQL, to an interactive Power BI dashboard.

![Dashboard Preview](images/dashboard_preview.png)

## Project Overview

This project explores customer purchasing behavior to answer questions retail/e-commerce teams commonly ask: which segments drive revenue, how discounts and subscriptions affect spend, and how purchasing patterns vary by age and category.

**Pipeline:** `CSV → Python (pandas) cleaning → PostgreSQL → SQL analysis → Power BI dashboard`

## Dataset

- **Source:** Retail customer shopping dataset, 3,900 rows × 18 columns
- **Fields include:** customer demographics (age, gender, location), purchase details (item, category, amount, season), engagement signals (subscription status, review rating, discount/promo usage, previous purchases), and fulfillment (shipping type, payment method, purchase frequency)

## What I Did

### 1. Data Cleaning (`notebooks/customer_shopping_analysis.ipynb`)
- Inspected structure and data types, checked for missing values
- Imputed missing `Review Rating` values using the **category-wise median** (more accurate than a single global fill)
- Standardized column names to snake_case
- Engineered new features:
  - `age_group` — binned customers into Young Adult / Adult / Middle-aged / Senior using quartiles
  - `purchase_frequency_days` — mapped categorical purchase frequency (e.g. "Weekly", "Fortnightly") to numeric days for easier analysis
- Identified that `discount_applied` and `promo_code_used` were perfectly correlated and dropped the redundant column
- Loaded the cleaned dataset into a PostgreSQL database using SQLAlchemy

### 2. SQL Analysis (`sql/customer_behavior_sql_queries.sql`)
Ten business questions answered directly in SQL, including:
- Revenue split by gender and by age group
- Customers who used a discount but still spent above the average
- Top 5 products by average review rating
- Standard vs. Express shipping — average spend comparison
- Subscriber vs. non-subscriber spend and total revenue
- Customer segmentation (New / Returning / Loyal) by purchase history
- Top 3 products per category by order volume
- Repeat-buyer subscription likelihood

### 3. Dashboard (`dashboard/customer_behavior_dashboard.pbix`)
An interactive Power BI dashboard with:
- KPI cards: total customers, average purchase amount, average review rating
- Revenue and sales breakdown by category and by age group
- Subscription status distribution
- Filters for gender, category, shipping type, and subscription status

## Key Insights

- **Middle-aged and Young Adult segments** generate the highest revenue and sales volume among the four age groups
- **Clothing** is both the top revenue-generating and best-selling category, followed by Accessories
- Average purchase amount across all customers is **$59.49**, with an average review rating of **3.75**
- Customer segmentation by purchase history separates the base into New, Returning, and Loyal buyers — useful for targeting retention campaigns differently by segment

## Tools & Tech

`Python` (pandas) · `PostgreSQL` · `SQL` (SQLAlchemy) · `Power BI`

## Repository Structure

```
customer-behavior-analysis/
├── data/
│   └── customer_shopping_behavior.csv       # raw dataset
├── notebooks/
│   └── customer_shopping_analysis.ipynb     # cleaning & feature engineering
├── sql/
│   └── customer_behavior_sql_queries.sql    # 10 business questions in SQL
├── dashboard/
│   └── customer_behavior_dashboard.pbix     # Power BI dashboard file
├── images/
│   └── dashboard_preview.png                # dashboard screenshot
├── requirements.txt
└── README.md
```

## How to Reproduce

1. Clone the repo and install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Run `notebooks/customer_shopping_analysis.ipynb` to clean the data and (optionally) load it into PostgreSQL.
   - If loading to PostgreSQL, set these environment variables first instead of hardcoding credentials:
     ```bash
     export PG_USER=postgres
     export PG_PASSWORD=your_password
     export PG_HOST=localhost
     export PG_PORT=5432
     export PG_DATABASE=customer_behavior
     ```
3. Run the queries in `sql/customer_behavior_sql_queries.sql` against the loaded table.
4. Open `dashboard/customer_behavior_dashboard.pbix` in Power BI Desktop to explore the dashboard.

## Notes

This project was built as a learning exercise to practice the full analytics workflow — from raw data to a stakeholder-ready dashboard — rather than any single tool in isolation.
