# Kidney Disease Classification

Bu proje, veri madenciliği teknikleri kullanılarak klinik laboratuvar sonuçlarından böbrek hastalığı (Kidney Disease) tahmini yapmayı amaçlamaktadır. Proje, sadece standart bir makine öğrenmesi modellemesi sunmakla kalmayıp, **gerçek dünya verilerinin istatistiksel geçerliliğini ve literatürdeki iddiaları sorgulayan analitik bir eleştiri** içermektedir.

---

## 1. Projenin Amacı ve Kapsamı
Klinik teşhis ve karar destek sistemlerinde makine öğrenmesi kullanımı giderek yaygınlaşmaktadır. Bu projenin temel amacı, Bangladeş'teki bir klinikten alınan gerçek hasta kayıtları üzerinde Karar Ağacı tabanlı bir topluluk algoritması olan **Random Forest (Rastgele Orman)** kullanarak sınıflandırma yapmaktır. Ancak süreç ilerledikçe proje, "yapısal olarak temiz görünen her verinin anlamlı bir matematiksel örüntü taşıyıp taşımadığı" sorusuna odaklanan bir veri anatomisi analizine dönüşmüştür.

---

## 2. Veri Seti Özellikleri ve Dağılımı
Çalışmada, *Data in Brief* dergisinde yayımlanan **BD-KDD** klinik veri seti kullanılmıştır. 
* **Örneklem Boyutu:** 988 Hasta kaydı
* **Özellik (Feature) Sayısı:** 26 bağımsız değişken (Yaş, Kan Basıncı, Kan Üresi, Kreatinin, Sodyum vb.)
* **Hedef Değişken (Target):** `Class` (0: Sağlıklı, 1: Böbrek Hastası)

Veri seti, herhangi bir eksik değer (NaN) barındırmayan ve sınıf dengesizliği (class imbalance) problemi olmayan son derece temiz bir yapıya sahiptir.

<img src="/images/sinif_dagilimi.png" alt="Sınıf Dağılımı" width="500">

---

## 3. Keşifçi Veri Analizi (EDA)
Modelleme aşamasına geçilmeden önce verinin iç yapısı incelendiğinde, özellikler ile hastalık durumu arasında anlamlı bir doğrusal veya doğrusal olmayan ilişki kurulamadığı tespit edilmiştir.

### Korelasyon Analizi
Bağımsız değişkenlerin hastalık durumuyla (Class) olan ilişkisi incelendiğinde, maksimum korelasyon katsayısının yalnızca **0.04** seviyesinde kaldığı görülmüştür. Bu, veride modeli yönlendirecek güçlü bir "sinyal" olmadığını gösterir.

<img src="/images/korelasyon_matrisi.png" alt="Korelasyon Matrisi" width="500">

### Sınıfların Örtüşme Durumu (KDE Plot)
Hastalık teşhisinde kritik rol oynaması beklenen 'Kan Üresi (Bu)' gibi değişkenlerin dağılımına bakıldığında, sağlıklı ve hasta bireylerin verilerinin %100'e yakın oranda üst üste bindiği (overlap) kanıtlanmıştır.

<img src="/images/KDE_plot.png" alt="KDE plot" width="500">

---

## 4. Model Kurulumu ve Değerlendirme
Veri seti %80 Eğitim ve %20 Test olarak ayrılmış, doğrusal olmayan karmaşık ilişkileri yakalama gücü yüksek olan **Random Forest Classifier (n_estimators=100)** modeli eğitilmiştir. 

### Karmaşıklık Matrisi (Confusion Matrix)
Modelin test verisi üzerindeki tahminleri incelendiğinde, %50.51'lik bir Doğruluk Oranı (Accuracy) elde edilmiştir. Confusion Matrix, modelin rastgele (yazı-tura) tahminde bulunduğunu açıkça göstermektedir.

<img src="/images/confusion_matrix.png" alt="Confusion Matrix" width="500">

### Özellik Önemi (Feature Importance)
Random Forest'ın bilgi kazancına (Information Gain) göre yaptığı özellik sıralamasında, hiçbir klinik değerin model üzerinde baskın bir karar mekanizması kuramadığı (en yüksek ağırlığın %8'de kaldığı) görülmüştür.

<img src="/images/feature_importance.png" alt="Feature Importance" width="500">

### ROC Eğrisi ve AUC Değeri
Modelin ayırt edicilik gücünü ölçen ROC eğrisi, rastgele tahmin çizgisiyle birebir aynı eksende ilerlemiş ve AUC değeri **0.49** olarak hesaplanmıştır.

<img src="/images/roc_egrisi.png" alt="Roc Egrisi" width="500">

---

## 5. Literatür Karşılaştırması ve Akademik Sonuç
Projenin referans aldığı [orijinal makalede (Rahman et al., 2026)](https://www.sciencedirect.com/science/article/pii/S235234092600291X), araştırmacılar Gaussian Naive Bayes algoritması ile %61.11 başarı elde ettiklerini iddia etmişlerdir. 

Ancak bu projede gerçekleştirilen derinlemesine veri analizi ve Random Forest modellemesi sonucunda; laboratuvar verileri ile hedef hastalık durumu arasında matematiksel olarak hiçbir anlamlı ayrıştırıcı sinyal bulunmadığı ispatlanmıştır. Referans makaledeki %61'lik başarı iddiasının, eğitim verisini ezberleme (overfitting) veya hatalı validasyon yöntemlerinden kaynaklandığı istatistiksel olarak aşikardır. 

**Sonuç olarak:** Bu proje, makine öğrenmesi süreçlerinde verinin yapısal olarak "temiz" (eksiksiz ve sayısallaştırılmış) olmasının, o verinin "öğrenilebilir" bir örüntü taşıdığı anlamına gelmediğini vurgulayan güçlü bir akademik eleştiri sunmaktadır.

---

**Proje Sunum Videosu:** [https://www.youtube.com/watch?v=D8yvRZ5TG7c]
