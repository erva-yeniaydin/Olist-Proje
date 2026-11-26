# Olist Müşteri Memnuniyetsizliği Tahmini

**FET445 Veri Madenciliği Dersi Final Projesi · Topkapı Üniversitesi**

Bu projede, teslim edilmiş bir e-ticaret siparişinin müşteri tarafından **düşük puanla (1–2 yıldız)** değerlendirilip değerlendirilmeyeceğini sipariş, teslimat, ödeme, ürün ve satıcı bilgilerinden tahmin ediyoruz. Ayrıca memnuniyetsizliği en çok etkileyen faktörleri belirlemeyi amaçlıyoruz.

## Araştırma sorusu

> Teslim edilen bir siparişin müşteri tarafından düşük puanla (1–2 yıldız) değerlendirileceğini sipariş, teslimat, ürün ve satıcı bilgilerinden tahmin edebilir miyiz?

Bu tahmin sayesinde platform, olumsuz yorum gelmeden önce riskli siparişlere müdahale edebilir. Gecikmeye yol açan satıcı ve bölgeleri de önceliklendirebilir.

## Veri seti

[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle)

- 2016–2018 yılları arasında verilmiş yaklaşık 100 bin sipariş
- 9 ilişkisel tablo: siparişler, sipariş kalemleri, ödemeler, yorumlar, müşteriler, satıcılar, ürünler, konum ve kategori çevirisi
- Anonimleştirilmiş veri

Veri, lisans koşulları gereği bu depoya yüklenmez. Kaggle'dan indirip `data/raw/` klasörüne koyun:

```bash
kaggle datasets download -d olistbr/brazilian-ecommerce -p data/raw --unzip
```

## Yöntem

| Aşama | İçerik |
|---|---|
| Veri hazırlığı | Tabloları sipariş düzeyinde birleştirme; teslim süresi, gecikme, kargo/fiyat oranı ve satıcı–müşteri mesafesi gibi öznitelikleri türetme; eksik değer, kodlama ve ölçekleme adımlarını scikit-learn `Pipeline` içinde yapma |
| Hedef değişken | `low_review` (1 = 1–2 yıldız, 0 = 3–5 yıldız) |
| Modeller | Temel model (çoğunluk sınıfı), Lojistik Regresyon, Karar Ağacı, Random Forest |
| Değerlendirme | %80/%20 tabakalı ayırma, 5 katlı tabakalı çapraz doğrulama, PR-AUC (ana metrik), F1, recall, ROC-AUC |
| Dengesizlik | `class_weight="balanced"`, gerekirse yalnızca eğitim katlarında SMOTE, çapraz doğrulamayla seçilen karar eşiği |

## Depo yapısı

```
├── data/
│   ├── raw/            # Kaggle'dan indirilen CSV'ler (git'e eklenmez)
│   └── processed/      # Birleştirilmiş ve temizlenmiş veri
├── notebooks/          # Keşifsel analiz ve model denemeleri
├── src/                # Veri hazırlama ve modelleme kodları
├── reports/            # Proje önerisi, ara rapor ve final raporu (PDF)
├── requirements.txt
└── README.md
```

## Kurulum

```bash
git clone https://github.com/erva-yeniaydin/<depo-adi>.git
cd <depo-adi>
pip install -r requirements.txt
```

## Ekip

| Ad Soyad | Sorumluluk |
|---|---|
| Erva Yeniaydın | Tabloların birleştirilmesi, veri temizliği, lojistik regresyon |
| Sudenaz Ebrar Çoban | Keşifsel veri analizi, öznitelik türetme, karar ağacı |
| Elif Bozdemir | Random Forest, değerlendirme ve eşik seçimi |

## Kaynaklar

1. Olist ve A. Sionek, "Brazilian E-Commerce Public Dataset by Olist," Kaggle, 2018.
2. F. Pedregosa vd., "Scikit-learn: Machine learning in Python," *JMLR*, cilt 12, ss. 2825–2830, 2011.
3. N. V. Chawla vd., "SMOTE: Synthetic minority over-sampling technique," *JAIR*, cilt 16, ss. 321–357, 2002.
