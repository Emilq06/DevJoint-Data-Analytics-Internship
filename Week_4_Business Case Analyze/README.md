# Həftə 4: Marketinq Performansının Analizi və Simpson Paradoksu

## 📌 Layihənin Xülasəsi
Bu layihə 2024 və 2025-ci illər üzrə müxtəlif müştəri cəlb etmə (acquisition) kanallarının marketinq performansını təhlil edir. Əsas məqsəd fərdi marketinq kanallarının əməliyyat göstəricilərinin yaxşılaşmasına baxmayaraq, ümumi Konversiya Dərəcəsinin (Conversion Rate) niyə aşağı düşdüyünü — biznes paradoksunu izah etməkdir. Analiz SQL sorğuları, Google Sheets pivot cədvəlləri və vizual modelləşdirmə vasitəsilə aparılmışdır.

---

## 🎯 Problemin Tərifi (Checkpoint 1)
- **Əsas Məqsəd:** 2024-cü ildən 2025-ci ilə qədər Konversiya Dərəcəsinin ümumi azalmasının kök səbəbini müəyyən etmək.
- **Fərziyyə:** Fərdi marketinq kanallarının performansının pisləşdiyini, yoxsa bu azalmanın büdcə və trafik bölgüsündəki struktur dəyişikliklərindən (Budget Mix-Shift) qaynaqlandığını yoxlamaq.

---

## 💻 SQL Analizi və Məlumatların Çıxarılması (Checkpoint 2)
Məlumatların çıxarılması, aqreqasiyası və əsas performans metrikaları (Sessions, Conversions, Spend, Revenue, Conversion Rate %, ROAS və CPA) aşağıdakı SQLite sorğuları istifadə edilərək hesablanmışdır:

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
📊 Məlumatların Transformasiyası və Pivot Cədvəllər (Checkpoint 3)
Google Sheets istifadə edərək, illik performansı yan-yana müqayisə etmək üçün SQL sorğusunun nəticələri Pivot Cədvəllərində (Pivot Tables) strukturlaşdırılmışdır:

Konversiya Dərəcəsi (%) (Conversions / Sessions) və ROAS (Revenue / Spend) üçün hesablanmış sahələr (calculated fields) tətbiq edilmişdir.

Trend analizi təsdiq etdi ki, 2024-cü illə müqayisədə 2025-ci ildə hər bir fərdi kanalın konversiya dərəcəsi artmışdır.

📈 Vizuallaşdırma və İnteraktiv Analiz (Checkpoint 4)
Aşağıda fərdi kanalların konversiya dərəcəsinin artımını və büdcə yerdəyişməsinin (mix-shift) təsirini göstərən qrafik verilmişdir:
https://github.com/Emilq06/DevJoint-Data-Analytics-Internship/tree/main/Week_4_Business%20Case%20Analyze

İnteraktiv Cədvəl: Google Sheets Analizinə Baxın:
https://docs.google.com/spreadsheets/d/1pDR0-jF0ORLqBNJSJ3uWeX2k5HZMQ0WkWDtzPpMCR4I/edit?usp=sharing

🔍 İdarəetmə Xülasəsi (Checkpoint 5)
1. Vəziyyət
Əsas marketinq metrikalarının ilkin icmalı 2024-cü ildən 2025-ci ilə qədər ümumi Konversiya Dərəcəsində azalma olduğunu göstərdi ki, bu da kanalların səmərəliliyi və reklam büdcəsinin ayrılması ilə bağlı narahatlıqlar yaratdı.

2. Tapıntılar
Fərdi Kanal Artımı: Fərdi performans analizi göstərdi ki, bütün cəlb etmə kanalları (Organic Search, Email, Paid Search, Paid Social) 2025-ci ildə öz Konversiya Dərəcələrini (%) və ROAS göstəricilərini artırıb.

Simpson Paradoksu: Ümumi göstəricinin düşməsi Simpson Paradoksu ilə əlaqədardır. Büdcənin və trafikin əhəmiyyətli bir hissəsi yüksək konversiyalı orqanik/daxili kanallardan aşağı konversiyalı ödənişli kanallara (Paid Social, Paid Search) yönəldilmişdir.

💡 Konkret Əməli Tövsiyə (Checkpoint 6)
Tövsiyə: Marketinq büdcəsinin 15-20%-ni Paid Social kanalından alaraq yüksək konversiyalı kanallara (Organic Search və Email) yenidən bölüşdürmək.

Əsaslandırma və Sübut: Paid Social kanalı Organic Search (~8.57%) və Email (~6.76%) ilə müqayisədə əhəmiyyətli dərəcədə aşağı konversiya dərəcəsinə (~3.18%) malikdir. Büdcə payını daha yüksək performanslı kanallara doğru yenidən balanslaşdırmaq dərhal ümumi Konversiya Dərəcəsini bərpa edəcək və ümumi ROAS göstəricisini optimallaşdıracaqdır.

🛠️ İstifadə Olunan Alətlər və Texnologiyalar
SQL (SQLite): Məlumatların çıxarılması, aqreqasiyalar və metrika hesablamaları.

Google Sheets: Məlumatların pivotinqi, hesablanmış sahələr və sütunlu qrafik (Column Chart) vizuallaşdırmaları.

GitHub: Sənədləşdirmə və versiya nəzarəti.



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
