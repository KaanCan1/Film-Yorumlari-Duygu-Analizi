# Film Yorumları Duygu Analizi

## Proje Tanımı

Bu proje, Bilgi Mühendisliğine Giriş dersi kapsamında bir grup projesi olarak gerçekleştirilmiştir. Amaç, Türkçe film yorumlarını olumlu (positive) ya da olumsuz (negative) olarak sınıflandıran bir makine öğrenmesi sistemi geliştirmektir.

Proje Python ile yazılmıştır: yorumlar temizlenir, kelime torbası vektörlerine dönüştürülür ve küçük bir Keras sinir ağıyla sınıflandırılır.

## Proje İçeriği ve Yöntem

### Kullanılan Veri Kümesi

- **yorumlar_5000.csv**: 5.000 etiketli Türkçe film yorumu (2.543 positive, 2.457 negative). `ProjeSon.py` modeli bu dosyayla eğitir ve test eder.
- **proje_csv_duzgun_son.csv**: Bu yorumlardan 2.768 satırlık bir alt küme. Kodda kullanılmıyor.

### Veri Ön İşleme

- HTML etiketlerinin ve linklerin silinmesi
- Emojilerin Türkçe kelimelere çevrilmesi (`emoji.demojize`)
- Harf ve rakam dışındaki karakterlerin kaldırılması, küçük harfe çevirme
- NLTK Türkçe stop word (durak kelime) listesiyle gereksiz kelimelerin ayıklanması

### Özellik Çıkarımı

- Etiketler `LabelEncoder` ile 0/1'e çevrilir.
- Veri %80 eğitim, %20 test olarak ayrılır.
- scikit-learn `CountVectorizer` ile 1–3 gramlık kelime torbası vektörleri oluşturulur.

### Modelleme

TensorFlow/Keras ile yoğun katmanlı bir sinir ağı kullanılmıştır:

- 128 → 64 → 32 → 16 nöronlu ReLU katmanları, tek nöronlu sigmoid çıkış
- Her katmanda L2 düzenlileştirme, aralarda dropout
- Adam optimizasyonu, binary cross-entropy kaybı
- En fazla 20 epoch; doğrulama kaybı 4 epoch iyileşmezse erken durdurma

### Değerlendirme ve Görselleştirme

- Test kümesinde doğruluk (accuracy) hesaplanır.
- Eğitim/doğrulama kaybı ve doğruluğu epoch bazında grafikle gösterilir.
- Son olarak kullanıcıdan bir yorum alınır ve olumlu/olumsuz tahmini olasılığıyla birlikte yazdırılır.

## Çalıştırma

```bash
pip install pandas nltk scikit-learn tensorflow emoji matplotlib
python ProjeSon.py
```

`ProjeSon.py` içindeki `pd.read_csv(...)` yolu, `yorumlar_5000.csv` dosyasının bilgisayarınızdaki konumunu gösterecek şekilde düzenlenmelidir.
