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
