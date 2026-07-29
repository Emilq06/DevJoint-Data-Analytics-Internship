# Superstore Məlumat Təhlili və Dashboard Layihəsi

Bu layihə staj proqramı çərçivəsində **Superstore** verilənlər bazası üzərində yerinə yetirilmiş məlumatların təmizlənməsi, analizi, şərti qiymətləndirilməsi və vizuallaşdırılması işlərini əhatə edir.

---

## 📌 Layihə Haqqında İcmal
Layihənin əsas məqsədi xam sifariş məlumatlarını təmizləmək, şərti məntiq düsturları tətbiq etmək, əsas performans göstəricilərini (KPI) hesablamaq, dinamik xülasə cədvəlləri qurmaq və interaktiv dashboard hazırlamaqdır.

---

## 📊 Əsas Yerinə Yetirilən Tapşırıqlar (Checkpoints)
- **Məlumatların Təmizlənməsi və Standartlaşdırılması:** Mətndəki artıq boşluqlar təmizləndi, rejestr (böyük/kiçik hərf) standartlaşdırıldı və əskik məlumatlar emal olundu.
- **Dinamik Axtarış və Hesablanmış Sütunlar:** Regionlar üzrə cavabdeh menecer məlumatları inteqrasiya edildi və sifarişlərin mənfəətlilik statusu qiymətləndirildi.
- **KPI Göstəriciləri və Aqreqasiya:** Ümumi satış, mənfəət göstəriciləri və regionlar üzrə sifariş sayları hesablandı.
- **İnteraktiv Dashboard:** Vizual qrafiklər (Column, Line, Pie chart) quruldu və maliyyə hədəflərini vurğulamaq üçün şərti formatlaşdırma (Conditional Formatting) tətbiq edildi.
- **Formulaların Sənədləşdirilməsi:** Layihə boyunca istifadə olunan bütün Excel formulalarının strukturu və izahı sənədləşdirildi.

---

## 🛠️ İstifadə Olunan Excel Formulaları

### 1. Dinamik Axtarış (Lookup Formulas)
- **Regional Manager Axtarışı:**
  ```excel
  =XLOOKUP(M2, People!B:B, People!A:A, "N/A")
  Region adına əsasən 'People' vərəqindən cavabdeh menecerin adını avtomatik çəkir.

2. Məntiqi və Şərti Formulalar
Mənfəətlilik Statusu (Profitability Status):

Excel
=IFS(U2>0, "Profit", U2<0, "Loss", U2=0, "Break Even")
Sifarişləri mənfəət dəyərinə görə Profit, Loss və ya Break Even kimi kateqoriyalaşdırır.

Yüksək Dəyərli Sifariş (High Value Order):

Excel
=IF(R2>1000, "High Value", "Standard")
Satış məbləği 1000-dən böyük olan sifarişləri High Value kimi işarələyir.

3. Şərti Aqreqasiya (Conditional Aggregations)
Region üzrə Ümumi Satış:

Excel
=SUMIFS(Orders!R:R, Orders!M:M, "East")
Yalnız East regionuna aid olan ümumi satış məbləğini cəmləyir.

Regionlar üzrə Mənfəət Xülasəsi:

Excel
=SUMIFS(Orders!U:U, Orders!M:M, "Central")
Hər bir region üzrə mənfəət göstəricilərini cəmləyir.

Çoxşərtli Sifariş Sayı:

Excel
=COUNTIFS(Orders!W:W, "Profit", Orders!X:X, "High Value")
Həm mənfəətli, həm də yüksək dəyərli olan sifarişlərin ümumi sayını hesablayır.

4. Mətn və Formatlaşdırma Funksiyaları
Mətn Təmizlənməsi:

Excel
=TRIM(PROPER(A2))
Mətndəki lüzumsuz boşluqları silir və sözlərin baş hərflərini böyüdür.

📈 Dashboard və Vizuallaşdırma
Column Chart: Regionlar üzrə Satış və Mənfəətin müqayisəsi.

Line Chart: Vaxta görə satış trendlərinin dinamikası.

Pie Chart: Satışların regionlar üzrə faiz bölgüsü.

Conditional Formatting: Summary vərəqində mənfəət hədəfini (Target: 50,000) vurğulamaq üçün tətbiq olunmuşdur (>50k yaşıl, <50k qırmızı).

📁 Repozitoriyanın Strukturu
Plaintext
├── superstore_raw_data.xlsx   # Xam məlumatlar, təmizlənmiş vərəqlər, xülasə və dashboard-u ehtiva edən əsas Excel faylı
└── README.md                          # Layihə icmalı, iş axını və formulaların izahlı sənədləşdirilməsi


## 📊 Checkpoint-lər üzrə Görülən İşlər

- **Checkpoint 1 — Məlumatların Təmizlənməsi və Standartlaşdırılması:** 
  `TRIM` və `PROPER` formulaları ilə mətnlərdəki artıq boşluqlar silindi, baş hərflər böyüdüldü və məlumatlar standart formata gətirildi.

- **Checkpoint 2 — Məlumatların Zənginləşdirilməsi (Calculated Fields):** 
  `XLOOKUP` ilə Regionlar üzrə menecer adları çəkildi, `IFS` ilə mənfəətlilik statusu (`Profit`, `Loss`, `Break Even`) və `IF` ilə sifariş dəyəri (`High Value`, `Standard`) təyin edildi.

- **Checkpoint 3 — Pivot Tables və Məlumatların Qruplaşdırılması:** 
  Məlumat bazası üzərində Pivot cədvəllər qurularaq Regionlar və Kateqoriyalar üzrə Satış və Mənfəət göstəriciləri dinamik aqreqasiya olundu.

- **Checkpoint 4 — KPI və Şərti Cəmləmə (Summary vərəqi):** 
  `SUMIFS` və `COUNTIFS` formulaları vasitəsilə Regionlar üzrə mənfəət göstəriciləri, East regionunun satışı və High Value mənfəətli sifarişlərin sayı hesablandı.

- **Checkpoint 5 — Dashboard və Vizuallaşdırma:** 
  `Summary` vərəqində 3 fərqli qrafik tipi (Column, Line, Pie chart) quruldu və Mənfəət Hədəfini (Target: 50,000) göstərmək üçün Şərti Formatlaşdırma (`>50k` yaşıl, `<50k` qırmızı) tətbiq edildi.

- **Checkpoint 6 — Formulaların Sənədləşdirilməsi:** 
  Layihə boyunca tətbiq edilən bütün Excel formulalarının sintaksisi, məntiqi və təyinatı ətraflı şəkildə sənədləşdirildi.
