# Week 4: Marketing Performance Analysis & Simpson's Paradox

## 📌 Project Overview
This project investigates marketing performance across multiple acquisition channels for 2024 and 2025. The primary objective was to resolve a business paradox: explaining why the top-level aggregate Conversion Rate declined despite individual marketing channels showing operational improvements. The analysis was conducted using SQL querying, Google Sheets data pivoting, and visual modeling.

---

## 🎯 Problem Definition (Checkpoint 1)
- **Primary Objective:** Identify the root cause behind the overall decline in Conversion Rate from 2024 to 2025.
- **Hypothesis:** Evaluate whether the performance of individual marketing channels degraded or if the decline was driven by structural changes in budget and traffic distribution across channels (Budget Mix-Shift).

---

## 💻 SQL Analysis & Data Extraction (Checkpoint 2)
Data extraction, aggregations, and key performance metrics (Sessions, Conversions, Spend, Revenue, Conversion Rate %, ROAS, and CPA) were executed using the following SQLite queries:

```sql
-- 1. İllər üzrə Ümumi Performans Metrikaları
SELECT 
    strftime('%Y', date) AS year,
    SUM(sessions) AS total_sessions,
    SUM(conversions) AS total_conversions,
    ROUND(SUM(spend), 2) AS total_spend,
    ROUND(SUM(revenue), 2) AS total_revenue,
    ROUND((CAST(SUM(conversions) AS FLOAT) / SUM(sessions)) * 100, 2) AS conversion_rate_pct,
    ROUND(CAST(SUM(revenue) AS FLOAT) / SUM(spend), 2) AS roas,
    ROUND(CAST(SUM(spend) AS FLOAT) / SUM(conversions), 2) AS cpa
FROM marketing_events
GROUP BY strftime('%Y', date)
ORDER BY year;

-- 2. Marketinq Kanalları Üzrə Dərin Aqreqasiya (Simpson Paradoksu Analizi)
SELECT 
    strftime('%Y', date) AS year,
    channel,
    SUM(sessions) AS total_sessions,
    SUM(conversions) AS total_conversions,
    ROUND(SUM(spend), 2) AS total_spend,
    ROUND(SUM(revenue), 2) AS total_revenue,
    ROUND((CAST(SUM(conversions) AS FLOAT) / SUM(sessions)) * 100, 2) AS conversion_rate_pct,
    ROUND(CAST(SUM(revenue) AS FLOAT) / SUM(spend), 2) AS roas,
    ROUND(CAST(SUM(spend) AS FLOAT) / SUM(conversions), 2) AS cpa
FROM marketing_events
GROUP BY strftime('%Y', date), channel
ORDER BY year, total_revenue DESC;
📊 Data Transformation & Pivoting (Checkpoint 3)
Using Google Sheets, SQL query results were structured into Pivot Tables to compare side-by-side yearly performance:

Calculated fields were implemented for Conversion Rate (%) (Conversions / Sessions) and ROAS (Revenue / Spend).

Trend analysis confirmed that every single individual channel improved its conversion rate in 2025 compared to 2024.

📈 Visualizations & Interactive Workbook (Checkpoint 4)
Below is the visualization illustrating individual channel conversion rate improvements alongside the budget mix-shift effect:
(https://github.com/Emilq06/DevJoint-Data-Analytics-Internship/tree/main/Week_4_Business%20Case%20Analyze)

Interactive Spreadsheet: (https://docs.google.com/spreadsheets/d/1pDR0-jF0ORLqBNJSJ3uWeX2k5HZMQ0WkWDtzPpMCR4I/edit?usp=sharing)

🔍 Executive Summary (Checkpoint 5)
1. Situation
An initial review of top-level marketing metrics indicated a decrease in the aggregate Conversion Rate from 2024 to 2025, prompting concerns regarding channel efficiency and ad spend allocation.

2. Findings
Individual Channel Growth: Individual performance analysis revealed that all acquisition channels (Organic Search, Email, Paid Search, Paid Social) increased their Conversion Rate (%) and ROAS in 2025.

Simpson's Paradox: The top-level drop was caused by Simpson's Paradox. A significant portion of the budget and traffic was shifted away from high-converting organic/owned channels toward lower-converting paid channels (Paid Social, Paid Search).

💡 Actionable Recommendation (Checkpoint 6)
Recommendation: Reallocate 15–20% of the marketing budget from Paid Social back into high-converting channels (Organic Search and Email optimization).

Rationale & Evidence: Paid Social yields a significantly lower conversion rate (~3.18%) compared to Organic Search (~8.57%) and Email (~6.76%). Rebalancing the budget mix toward higher-performing channels will immediately restore the overall aggregate Conversion Rate and optimize overall ROAS.

🛠️ Tools & Technologies Used
SQL (SQLite): Data extraction, aggregations, CTEs, and metric calculations.

Google Sheets: Data pivoting, calculated fields, and Column Chart visualizations.

GitHub: Documentation, asset management, and version control.
