# 📊 Həftə 3: Power BI İnteraktiv Analitika Dashboard-u

Bu layihə **Star Schema (Ulduz Sxemi)** verillənlər modeli əsasında hazırlanmış tam interaktiv Power BI hesabatıdır. Hesabat satış analitikasını, hədəflərlə faktiki göstəricilərin müqayisəsini və əsas biznes metrikalarını vizual olaraq təhlil etməyə imkan verir.

---

## 🛠 1. Verilənlər Modeli və Əlaqələr (Checkpoint 1 & 2)
* **Model Arxitekturası:** Fakt və ölçü cədvəllərini birləşdirən peşəkar Star Schema modeli qurulmuşdur.
* **Cədvəl Əlaqələri:** 
  * `FactSales` (Satışlar) və `FactSalesTarget` (Satış Hədəfləri) fakt cədvəlləri;
  * `DimProduct`, `DimDate2` və `DimEmployee` ölçü (dimension) cədvəlləri ilə **1-in çoxluğa (1:N)** münasibəti vasitəsilə düzgün əlaqələndirilmişdir.

---

## 📈 2. Vizuallar və KPI Performansı (Checkpoint 3 & 4)
* **İnteraktiv Vizuallar:** Biznes tələblərinə uyğun 4 əsas analitik qrafik (Sütunlu, Xətti, Pasta qrafikləri və s.) qurulmuşdur.
* **Süzgəcləmə (Slicers):** Hesabatı dinamik olaraq illər, tarixlər və kateqoriyalar üzrə filtrləmək üçün Slicer elementləri inteqrasiya edilmişdir.
* **KPI Performansı:** Satış hədəflərinin icrasını izləmək üçün faktiki satışlar (`SalesAmount`) ilə hədəf göstəricilərini (`TargetSalesAmount`) müqayisə edən dinamik **KPI kartı** yerləşdirilmişdir.

---

## 🎨 3. Dashboard UI/UX Dizaynı (Checkpoint 5)
* **Vizual İyerarxiya:** Mühüm rəqəmlər və süzgəclər aydın şəkildə yerləşdirilmiş, vizuallar mərkəzdə simmetrik nizamlanmışdır.
* **Təmiz Interfeys (Clean UI):** Dashboard üçün uyğun Canvas Background (arxa fon) və göz yormayan, ardıcıl rəng palitrası tətbiq edilərək təmiz və peşəkar görünüş təmin edilmişdir.

---

## 📐 4. DAX Formulları və Ölçülər (Checkpoint 6)
* **Dinamik Ölçülər (Calculated Measures):** Hesabatda mənfəət marjasını dinamik hesablamaq və sıfıra bölünmə xətalarının (DIV/0) qarşısını almaq üçün DAX dilində xüsusi ölçü yazılmışdır:

```dax
Profit Margin % = DIVIDE(SUM(FactSales[Profit]), SUM(FactSales[SalesAmount]), 0)

---

### 💻 GitHub-a Yükləmək Üçün Terminal Əmrləri:

Mətni `README.md` faylına yapışdırıb yadda saxladıqdan (`Ctrl + S`) sonra VS Code terminalında bu əmrləri icra et:

```bash
git add .
git commit -m "Docs: Update complete README in Azerbaijani for Week 3 project"
git push origin main
