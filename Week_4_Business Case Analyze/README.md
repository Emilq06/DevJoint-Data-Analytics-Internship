# Checkpoint 2: Data Extraction & Aggregation via SQL

## 📌 Task Overview
This checkpoint covers the extraction and aggregation of marketing performance data (Sessions, Conversions, Spend, Revenue, Conversion Rate, and ROAS) for the 2024 vs 2025 H2 datasets using SQL.

## 🛠️ Tools Used
- **SQL / SQLite**
- **DB Browser for SQLite**

## 📊 SQL Query Execution & Output
Below is the execution of the aggregate SQL query in DB Browser for SQLite, displaying the calculated metrics by year and marketing channel:

![SQL Result](sql_result.png)

## 📝 Key Observations
- Data was successfully grouped by year and marketing channel.
- Safe division (`NULLIF` / float casting) was applied to calculate Conversion Rate (%) and ROAS accurately.
- Individual channels show performance growth, while overall aggregate shifts highlight budget reallocation across channels.
