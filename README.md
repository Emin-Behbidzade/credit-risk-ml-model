# Kredit Riski ML Modeli

## Dataset
- Mənbə: [Credit Risk Dataset — Kaggle](https://www.kaggle.com/datasets/laotse/credit-risk-dataset)
- 32,581 müştəri, 12 sütun
- Hədəf dəyişən: loan_status (1 = default, 0 = ödəyib)

## Nə etdim

**Məlumat Təmizliyi**
- person_emp_length: 895 boş dəyər median ilə dolduruldu
- loan_int_rate: 3,116 boş dəyər median ilə dolduruldu
- Mətn sütunları LabelEncoder ilə rəqəmə çevrildi

**Niyə mean deyil, median?**
Gəlir və faiz sütunlarının paylanması skewed idi — bir neçə həddindən artıq böyük dəyər mean-i yuxarı çəkir. Median bu halda daha düzgün mərkəzi göstərici verir.

**Model**
- Algoritm: Random Forest (100 ağac)
- Train/Test: 80/20 bölgüsü
- Accuracy: 92.97%

**Niyə Random Forest, Logistic Regression deyil?**
Random Forest non-linear əlaqələri tuta bilir — yəni gəlir ilə kredit faizi arasındakı mürəkkəb əlaqəni Logistic Regression düz xətt ilə modelləşdirə bilmir. Bundan əlavə, Random Forest feature importance-ı avtomatik hesablayır və overfitting-ə daha davamlıdır.

## Nəticələr

| Metrika | Class 0 (ödəyib) | Class 1 (default) |
|---|---|---|
| Precision | 92% | 96% |
| Recall | 99% | 71% |

Recall 71% — yəni həqiqətən default edənlərin 29%-i model tərəfindən qaçırılır. Bank üçün bu real zərər deməkdir.

**Recall-u necə yaxşılaşdırmaq olar?**
`class_weight='balanced'` parametri əlavə etməklə model minority class-a — yəni default edənlərə — daha həssas olur. Bu halda overall accuracy bir az aşağı düşə bilər, amma bank üçün daha kritik olan göstərici — qaçırılan default-ların sayı — azalır.

## Feature Importance — Ən Təsirli Faktorlar
1. loan_percent_income (23%) — kreditin gəlirə nisbəti
2. person_income (15%) — gəlir səviyyəsi
3. loan_int_rate (11%) — faiz dərəcəsi
4. loan_grade (11%) — bankın öz risk qiymətləndirməsi
5. person_home_ownership (10%) — ev sahibliyi statusu

Ən güclü insight: müştərinin gəlirinə nisbətən həddindən artıq kredit alması ən böyük risk siqnalıdır.

## İstifadə Etdiyim Alətlər
- Python (pandas, scikit-learn, matplotlib)
- Power BI — interaktiv dashboard

## Fayllar
- `credit_risk_model.ipynb` — tam notebook
- `data/credit_risk_clean.csv` — təmizlənmiş fayl
- Orijinal fayl yuxarıdakı Kaggle linkindən endirilə bilər
