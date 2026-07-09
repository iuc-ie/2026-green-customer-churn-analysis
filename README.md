# Kozmetik E-Ticarette Makine Öğrenmesi ile Yeşil Müşteri Kaybı (Churn) Davranış Analizi

İstanbul Üniversitesi–Cerrahpaşa, Mühendislik Fakültesi, Endüstri Mühendisliği
Bölümü **Bitirme Projesi** kapsamında geliştirilmiştir.

Bu çalışma, sürdürülebilir (yeşil) kozmetik ürünleri satın alan müşterilerin
kayıp (churn) davranışını makine öğrenmesi ile analiz eder ve bu müşterilerin
sepetlerindeki ürün birlikteliklerini birliktelik kuralı madenciliği ile ortaya
koyar. Analiz iki bağımsız kola ayrılmıştır.

---

## Proje Yapısı

| Dosya | Kol | İçerik |
|-------|-----|--------|
| `A_Kolu.ipynb` | **A Kolu — Churn Modelleme** | Ön işleme, EDA, dengesiz veri yönetimi ve sınıflandırma modelleri ile müşteri kaybı tahmini |
| `B_Kolu.ipynb` | **B Kolu — Birliktelik Analizi** | Apriori / FP-Growth ile sepet analizi ve yeşil ürün odaklı birliktelik kuralları |
| `requirements.txt` | — | Python bağımlılıkları |

---

## A Kolu — Churn Modelleme

Müşteri düzeyinde, **sızıntısız** (data-leakage önlenmiş) bir veri seti üzerinden
churn tahmini yapar. Akış beş adımdan oluşur:

1. **Ön İşleme** — Tautoloji/sızıntı yaratan değişkenlerin (ör. `recency_gun`,
   `son_siparis`) çıkarılması; imputation parametrelerinin yalnızca eğitim
   setinden öğrenilmesi; `stratify` ile katmanlı train/test ayrımı.
2. **Keşifçi Analiz (EDA)** — "Yeşil müşteriler daha düşük churn oranına sahiptir"
   hipotezinin ki-kare testi ve segment bazlı doz–yanıt analizi ile sınanması.
3. **Dengesiz Veri Yönetimi** — Baseline, `class_weight`, SMOTE ve SMOTE-ENN
   stratejilerinin sabit bir model üzerinde karşılaştırılması. Yeniden örnekleme
   **yalnızca eğitim setine** uygulanır.
4. **Modelleme** — Lojistik Regresyon, Random Forest ve XGBoost'un `imblearn`
   pipeline'ı ile adil koşulda karşılaştırılması.
5. **Değerlendirme** — Nihai model (**XGBoost + SMOTE-ENN**), permütasyon tabanlı
   değişken önemi ve sınıflandırma raporu.

Değerlendirme metrikleri: **Precision, Recall, F1, F2, AUC-ROC** (dengesiz veri
nedeniyle Accuracy tercih edilmemiştir; churn'de en kritik metrik Recall'dur).

---

## B Kolu — Birliktelik Kuralı Madenciliği

İşlem (sepet) verisi üzerinden ürün birliktelikleri çıkarır:

- Sepetlerin one-hot (TransactionEncoder) matrisine dönüştürülmesi,
- **Apriori** ve **FP-Growth** algoritmalarının aynı sonucu ürettiğinin ve
  çalışma sürelerinin karşılaştırılması,
- Genel birliktelik kuralları (support, confidence, lift) ve `lift > 1` güçlü
  kuralların türetilmesi,
- **Yeşil ürün odaklı** kurallar: hem öncül hem ardılı tamamen sürdürülebilir
  ürünlerden oluşan kuralların ayrıştırılması ve görselleştirilmesi.

---

## Çalıştırma

Notebook'lar **Google Colab** ortamında geliştirilmiştir ve bazı hücreler
Colab'a özgü dosya yükleme/indirme (`google.colab.files`) ile `/content/...`
yollarını kullanır. Colab dışında çalıştırmak için bu satırların yerel
dosya yollarına uyarlanması gerekir.

```bash
pip install -r requirements.txt
```

Ardından `A_Kolu.ipynb` ve `B_Kolu.ipynb` sırasıyla açılıp yukarıdan aşağıya
çalıştırılır. Her iki kol da `kozmetik_surdurulebilir_churn_veri.xlsx` veri
setini (`Musteri_Tablosu` ve `Islem_Tablosu` sayfaları) kullanır.

---

## Kullanılan Başlıca Kütüphaneler

pandas · numpy · scikit-learn · imbalanced-learn · xgboost · mlxtend · scipy · matplotlib
