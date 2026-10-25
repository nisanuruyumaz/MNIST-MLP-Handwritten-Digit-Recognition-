# MNIST-MLP-Handwritten-Digit-Recognition-
MNIST Veri Seti ile MLP Tabanlı El Yazısı Rakam Tanıma Ödevi
# MNIST Veri Seti ile MLP Tabanlı El Yazısı Rakam Tanıma

Bu proje, MNIST el yazısı rakam veri seti kullanılarak PyTorch kütüphanesi ile sıfırdan bir Multi-Layer Perceptron (MLP) modeli geliştirmeyi, eğitmeyi ve eğitilen modelin hem MNIST test veri seti üzerindeki hem de gerçek el yazısı (öğrenci numarası) üzerindeki başarımını değerlendirmeyi amaçlamaktadır.

---

## 1. Model Mimarisi ve Hiperparametreler

Ödev kuralları doğrultusunda CNN kullanılmamış, sadece Tam Bağlantılı (Fully Connected) katmanlardan oluşan en az 2 gizli katmanlı bir MLP yapısı kurulmuştur.

- **Giriş Katmanı:** 784 düğüm ($28 \times 28$ piksel düzleştirilmiş görüntü)
- **1. Gizli Katman (Hidden Layer 1):** 256 düğüm + ReLU Aktivasyon Fonksiyonu
- **2. Gizli Katman (Hidden Layer 2):** 128 düğüm + ReLU Aktivasyon Fonksiyonu
- **Çıkış Katmanı (Output Layer):** 10 düğüm (0–9 arası rakam sınıfları)
- **Kayıp (Loss) Fonksiyonu:** CrossEntropyLoss
- **Optimizasyon Algoritması (Optimizer):** Adam (Learning Rate = 0.001)
- **Epoch Sayısı:** 15
- **Batch Size:** 64
- **Veri Bölünmesi:** 50.000 Eğitim (Train), 10.000 Doğrulama (Validation), 10.000 Test

---

## 2. Deney Sonuçları ve Performans Metrikleri

Elde edilen başarım metrikleri aşağıdaki tabloda özetlenmiştir:

| Metrik | Değer |
| :--- | :--- |
| **Training Loss (Son Epoch)** | ~0.016 |
| **Validation Loss (Son Epoch)** | ~0.137 |
| **Training Accuracy (Son Epoch)** | %99.50 |
| **Validation Accuracy (Son Epoch)** | %97.75 |
| **Nihai Test Accuracy** | **%97.86** |

### Eğitim Süreci Grafikleri

Aşağıdaki grafiklerde 15 epoch boyunca modelin hata (loss) ve doğruluk (accuracy) değişimleri gösterilmiştir:

![Eğitim ve Hata Grafiği](images/test_başarı_grafiği.png)

*Grafik İncelemesi:*
- Modelin eğitim kaybı düzenli olarak düşmüş ve eğitim başarısı %99'un üzerine çıkmıştır.
- Validation kaybı 1. epoch sonrası dengelenmiş ve model aşırı öğrenmeye (overfitting) girmeden %97.86 nihai test başarısına ulaşmıştır.

---

## 3. Kendi El Yazısı Deneyi (Öğrenci No: 24120205069)

Eğitilen modelin gerçek hayat koşullarındaki genelleştirme yeteneğini (generalization) test etmek amacıyla **24120205069** numaralı öğrenci numarasını oluşturan rakamlar (`0, 1, 2, 4, 5, 6, 9`) el yazısıyla kağıda yazılmış ve fotoğraflanmıştır.

Hazırlanan görseller:
1. Grayscale (Gri seviye) formata dönüştürülmüştür.
2. MNIST formatına uyum sağlamak adına arka plan siyah, rakam beyaz olacak şekilde tersine çevrilmiştir (invert).
3. $28 \times 28$ piksel boyutuna getirilerek normalizasyon uygulanmıştır.

---

## 4. Genel Değerlendirme (Distribution Shift & Generalization)

Model, MNIST test veri seti üzerinde **%97.86** gibi yüksek bir başarı oranına ulaşmıştır. Kendi el yazımızla hazırladığımız öğrenci numarası üzerindeki tahmin başarısı ve ortaya çıkan farkların nedenleri şunlardır:

1. **Veri Dağılımı Kayması (Distribution Shift):** MNIST veri setindeki görüntüler standart kırpma, ortalama ve çizgi yumuşatma süreçlerinden geçmiştir. Fotoğrafla çekilen el yazılarında ise çizgi kalınlıkları, ışık gölgelenmeleri ve kağıt pürüzlülüğü gibi gürültüler bulunur. Bu durum modelin performansını doğrudan etkilemektedir.
2. **MLP Mimarisi Sınırlamaları:** Model tam bağlantılı katmanlardan oluştuğu için görüntünün mekansal (spatial) özelliklerini koruyamaz. Rakamın eğik yazılması veya kare alan içerisinde hafifçe merkez kayması yaşaması, konvolüsyonel (CNN) katmanlar bulunmadığı için modelin karar verme mekanizmasını zorlaştırır.
