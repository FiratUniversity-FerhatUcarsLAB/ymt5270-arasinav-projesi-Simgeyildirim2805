# YMT5270 Ara Sınav Projesi: Orange ile Veri Analizi ve Makine Öğrenmesi

## Öğrenci Bilgileri
- **Ad Soyad**: Simge YILDIRIM
- **Öğrenci Numarası**: 242137211
- **E-posta**: yildirimsimge8@gmail.com

## Proje Özeti
> *Bu proje kapsamında kalp yetmezliği olan bireylerin ölüm durumu, Orange veri madenciliği platformu kullanılarak çeşitli sınıflandırma algoritmaları ile tahmin edilmiştir. Kullanılan veri seti hfail.csv olup, 299 gözlem ve 13 öznitelik içermektedir. Hedef değişken olan DEATH_EVENT, bireyin yaşayıp yaşamadığını (0 = hayatta, 1 = ölü) göstermektedir. Keşifsel veri analizi aşamasında temel istatistikler hesaplanmış, aykırı değerler ve öznitelikler arası ilişkiler incelenmiştir. Logistic Regression, Random Forest, Naive Bayes ve kNN modelleri uygulanmış ve başarıları Test & Score, Confusion Matrix, Predictions ve ROC Analysis bileşenleri yardımıyla değerlendirilmiştir. En başarılı sonuçlar Random Forest algoritması ile elde edilmiştir.*

## Veri Seti
### Veri Seti Bilgileri
- **Veri Seti Adı**: Heart Failure Clinical Records
- **Kaynak**: *Kaggle - Heart Failure Dataset*
- **Lisans**: *-*
- **Veri Seti Boyutu**: *299 gözlem, 13 öznitelik + 1 hedef değişken (DEATH_EVENT)*

### Veri Seti Tanımı
> *Bu projede kullanılan veri seti, kalp yetmezliği olan bireylerin demografik ve klinik özelliklerini içeren yapay olarak oluşturulmuş, eğitim amaçlı bir veri kümesidir. Veri seti doğrudan bir kurumdan temin edilmemiş olup, benzer çalışmalardan esinlenilerek arkadaşım tarafından oluşturulmuş ve tarafıma analiz amacıyla iletilmiştir. Veri setinde toplam 299 gözlem yer almakta olup her biri, bir bireye ait medikal geçmişi ve sağlık göstergelerini temsil etmektedir. Veri kümesi, 13 öznitelik ve 1 hedef değişkenden oluşmaktadır.
 Öznitelikler arasında yaş, cinsiyet, kan basıncı durumu, serum kreatinin düzeyi, kalbin pompalama gücü (ejeksiyon fraksiyonu), diyabet ve sigara kullanımı gibi tıbbi ve yaşam tarzına dair bilgiler yer almaktadır. Hedef değişken olan DEATH_EVENT, bireyin takip süreci sonunda hayatta olup olmadığını ikili kategorik biçimde ifade etmektedir (0: hayatta, 1: ölmüş). Veri seti üzerinde yapılan ön incelemelerde eksik veriye rastlanmamış, değişken etiketlemeleri Orange platformu tarafından doğru şekilde algılanmıştır. *

### Öznitelik Açıklamaları
| Öznitelik Adı           | Veri Tipi      | Açıklama                          | Örnek Değer |
|-------------------------|----------------|-----------------------------------|-------------|
| age                     | Sayısal        | Yaş                               | 65          |
| anaemia                 | Kategorik      | Kansızlık durumu (0: yok, 1: var) | 0           |
| creatinine_phosphokinase| Sayısal        | Enzim değeri                      | 250         |
| diabetes                | Kategorik      | Diyabet durumu	                   | 1           |
| ejection_fraction       | Sayısal        | Kalp pompalama oranı              | 38          |
| high_blood_pressure     | Kategorik      | Yüksek tansiyon durumu            | 1           |
| platelets               | Sayısal        | Trombosit değeri                  | 250000      |
| serum_creatinine        | Sayısal        | Kreatinin düzeyi	                 | 1.2         |
| serum_sodium            | Sayısal        | Sodyum düzeyi                     | 137         |
| sex                     | Kategorik      | Cinsiyet (0: kadın, 1: erkek)	   | 1           |
| smoking                 | Kategorik      | Sigara kullanımı                  | 0           |
| time                    | Sayısal        | Takip süresi                      | 4           |
| DEATH_EVENT             | Kategorik      | Ölüm durumu (0: hayatta, 1: ölmüş)| 1           |


## Keşifsel Veri Analizi (Explanatory Data Analysis - EDA)
### Temel İstatistikler
> *Orange’ın Feature Statistics bileşeni ile tüm sayısal değişkenlerin ortalama, standart sapma, maksimum ve minimum değerleri incelenmiştir. En büyük varyans serum_creatinine ve ejection_fraction değişkenlerinde gözlemlenmiştir.*

### Veri Ön İşleme
> - *Veri setinde eksik veri bulunmamaktadır.*
> - *Aykırı değerler Box Plot yardımıyla analiz edilmiş, işlem yapılmamıştır.*
> - *Kategorik veriler doğrudan Orange tarafından algılanarak ayrıştırılmıştır.*


### Görselleştirmeler
> *Distributions, Bar Plot ve Box Plot bileşenleri kullanılarak öznitelik dağılımları grafikleştirilmiştir.
Correlation bileşeni ile değişkenler arası ilişki analizi yapılmış, ejection_fraction ve serum_creatinine gibi değişkenlerin hedef değişken ile anlamlı korelasyonlara sahip olduğu görülmüştür.*

#### Görselleştirme 1: Box Plot – serum_creatinine Değişkeni
> *Bu görselleştirmede, serum_creatinine değişkenine ait kutu grafiği kullanılarak değişkenin dağılımı ve aykırı değerleri analiz edilmiştir. Grafikte özellikle üst sınırın üzerinde kalan birkaç gözlem, bu değişkenin bazı bireylerde normal değerlerin oldukça üzerinde çıktığını göstermektedir. Bu durum, böbrek fonksiyonlarının bozulmasıyla ilişkilendirilebileceği için ölümle (DEATH_EVENT) ilişkili olabileceği düşünülmektedir. Aykırı değerlerin varlığı, bu değişkenin sınıflandırma sürecinde belirleyici bir rol oynayabileceğine işaret etmektedir.*

#### Görselleştirme 2: Distributions – ejection_fraction Değişkeni
> *ejection_fraction değişkenine ait dağılım grafiği incelendiğinde, verilerin büyük çoğunluğunun düşük değer aralığında toplandığı görülmektedir. Bu değişken, kalbin pompalama kapasitesini ifade ettiği için düşük değerler kalp fonksiyonlarında yetersizlik anlamına gelir. Dağılım eğrisi, bu değişkenin ölüme etkisi olabilecek kritik bir parametre olduğunu desteklemektedir. Modelleme sürecinde bu tür dağılımsal yapıların dikkate alınması, algoritmaların sınıflandırma başarısını doğrudan etkilemektedir.*

### Öznitelik İlişkileri
> *Veri setinde yer alan sayısal öznitelikler arasındaki ilişkiler, Orange platformunda kullanılan Correlation bileşeni yardımıyla analiz edilmiştir. Bu analizde, Pearson korelasyon katsayısı temel alınarak her bir öznitelik çifti arasındaki doğrusal ilişki derecelendirilmiştir. Korelasyon matrisi üzerinde yapılan değerlendirmelerde:
serum_creatinine ile DEATH_EVENT arasında pozitif yönlü ve anlamlı bir ilişki olduğu gözlemlenmiştir. Bu durum, serum kreatinin düzeyinin artmasının ölüm riskini artırabileceğini göstermektedir.
ejection_fraction ile DEATH_EVENT arasında ise negatif yönlü bir ilişki bulunmuştur. Ejeksiyon fraksiyonunun azalması, bireyin kalp yetmezliği nedeniyle yaşam kaybı yaşama olasılığını artırmaktadır.
Bu ilişkiler, özniteliklerin hedef değişken üzerindeki etkilerini anlamak açısından önemli bir yol gösterici olmuştur. Ayrıca, Scatter Plot bileşeni kullanılarak değişken çiftleri arasındaki dağılımlar iki boyutlu grafiklerle incelenmiş, değişkenlerin ayırt edici gücü görsel olarak da desteklenmiştir.*

## Makine Öğrenmesi Uygulaması
### Kullanılan Yöntem: Sınıflandırma
> *DEATH_EVENT değişkeni, bireyin yaşam durumunu ikili biçimde ifade eden kategorik bir yapıya sahiptir (0 = hayatta, 1 = ölmüş). Bu nedenle, veri yapısına uygun olarak çalışmada sınıflandırma algoritmaları tercih edilmiştir. Hedef değişkenin bu ikili yapısı göz önünde bulundurularak, gözetimli öğrenme yaklaşımı çerçevesinde, ikili sınıflandırma problemleri için yaygın olarak kullanılan algoritmalar uygulanmıştır. Logistic Regression, Random Forest, Naive Bayes ve k-Nearest Neighbors (kNN) algoritmaları ile oluşturulan modellerin DEATH_EVENT değişkenini tahmin etme başarıları karşılaştırmalı olarak değerlendirilmiştir.*

### Modeller ve Parametreler
> | Model             | Parametreler                       |
| ------------------- | ---------------------------------- |
| Logistic Regression | Varsayılan parametreler            |
| Random Forest       | Ağaç sayısı = 10, `balance` kapalı |
| Naive Bayes         | Varsayılan parametreler            |
| kNN                 | k = 5, distance = Euclidean        |


### Model Değerlendirmesi
> *UUygulanan sınıflandırma algoritmalarının performansı, Orange platformundaki Test & Score, Confusion Matrix, Predictions ve ROC Analysis bileşenleri aracılığıyla değerlendirilmiştir. Modeller, 10 katlı çapraz doğrulama yöntemi ile test edilmiştir. 
Bu metrikler doğrultusunda yapılan karşılaştırmada, en yüksek başarıyı sağlayan modelin Random Forest olduğu görülmüştür. Model hem yüksek doğruluk hem de yüksek AUC değeri ile diğer algoritmalara göre daha güvenilir sonuçlar vermiştir. Logistic Regression ise ikinci sırada yer alırken, Naive Bayes ve kNN modelleri görece daha düşük performans sergilemiştir.*

#### Metrikler
| Model               | AUC  | Doğruluk | F1   | Kesinlik | Duyarlılık |
| ------------------- | ---- | -------- | ---- | -------- | ---------- |
| Logistic Regression | 0.85 | 0.83     | 0.78 | 0.80     | 0.77       |
| Random Forest       | 0.89 | 0.87     | 0.83 | 0.85     | 0.82       |
| Naive Bayes         | 0.76 | 0.78     | 0.70 | 0.74     | 0.66       |
| kNN                 | 0.81 | 0.80     | 0.75 | 0.76     | 0.74       |

### Sonuçların Yorumlanması
> *Uygulanan sınıflandırma modelleri değerlendirildiğinde, Random Forest algoritmasının en başarılı sonuçları verdiği görülmüştür. Model, hem yüksek doğruluk oranı (%87), hem de AUC değeri (0.89) ile dikkat çekmiştir. Bunun temel nedeni, Random Forest'ın çok sayıda karar ağacını bir araya getirerek karmaşık örüntüleri öğrenebilme yeteneğine sahip olmasıdır. Ayrıca aşırı öğrenmeyi (overfitting) azaltacak şekilde çeşitliliğe dayanması da önemli bir avantajdır.
Logistic Regression modeli ise daha sade ve yorumlanabilir bir yapıya sahip olmasına rağmen doğruluk açısından Random Forest'ın gerisinde kalmıştır. Ancak modelin istikrarlı sonuçlar üretmesi ve veri yorumlama açısından şeffaf olması, önemli bir güçlü yön olarak değerlendirilebilir.
Naive Bayes, özellikle sınıflar arasında güçlü koşullu bağımlılıklar olmadığında iyi performans göstermektedir. Ancak bu veri setinde, sınıflar arasındaki ilişkililik karmaşık olduğu için model nispeten zayıf kalmıştır. kNN ise örnek sayısının az olduğu veri setlerinde dengesiz sınıf dağılımından etkilenebilmekte, bu da modelin başarısını düşürmektedir.
Alternatif olarak bu çalışmada Support Vector Machines (SVM), Gradient Boosting ya da XGBoost gibi daha gelişmiş algoritmalar denenebilir ve model karşılaştırmaları daha da zenginleştirilebilir. Ayrıca, hiperparametre optimizasyonu yapılması hâlinde var olan modellerin performansları da iyileştirilebilir.*

## Orange İş Akışı
> * İş akışı File bileşeni ile veri setinin platforma yüklenmesiyle başlar. Ardından veri, dört farklı sınıflandırma algoritmasına (Logistic Regression, Random Forest, Naive Bayes ve k-Nearest Neighbors) yönlendirilmiştir.
Modellerin performansı Test & Score bileşeniyle çapraz doğrulama yöntemi kullanılarak değerlendirilmiştir. Buna ek olarak, sınıflandırma sonuçları Confusion Matrix ile görselleştirilmiş, Predictions bileşeni ile her bir gözlem için modelin tahmini ve doğruluğu incelenmiş, ROC Analysis bileşeniyle ise modellerin sınıfları ayırt etme yetenekleri analiz edilmiştir.
İş akışı ayrıca Box Plot, Distributions, Correlation gibi EDA bileşenleriyle desteklenmiştir. Böylece hem keşifsel veri analizi hem de modelleme süreci entegre ve görsel bir yapı içerisinde yürütülmüştür.*

![Orange İş Akışı](goruntuler/iş akışı.png)

## Sonuç ve Öneriler
> *Bu proje kapsamında, kalp yetmezliği hastalarına ait klinik veriler kullanılarak bireylerin yaşam durumlarının tahmini üzerine bir sınıflandırma çalışması gerçekleştirilmiştir. Orange platformu aracılığıyla yürütülen çalışmada dört farklı makine öğrenmesi algoritması karşılaştırılmış, elde edilen performans sonuçları temelinde en etkili modelin Random Forest olduğu belirlenmiştir. Bu model, hem doğruluk oranı hem de AUC değeri açısından diğer algoritmalara kıyasla üstün sonuçlar vermiştir. Logistic Regression modeli ise özellikle yorumlanabilirlik açısından güçlü bir alternatif olarak değerlendirilmiştir.
Çalışmanın bulguları, serum_creatinine ve ejection_fraction gibi belirli klinik değişkenlerin ölüm riski tahmininde anlamlı rol oynadığını göstermiştir. Bu özniteliklerin korelasyon analizlerinde hedef değişken ile olan güçlü ilişkileri, modellerin başarısını doğrudan etkilemiştir.
Gelecek çalışmalarda, model başarısını artırmak için hiperparametre optimizasyonuna gidilmesi ve veri setinin gerçek klinik verilerle genişletilmesi önerilmektedir. Ayrıca, daha gelişmiş sınıflandırma yöntemleri (örneğin Gradient Boosting, SVM, XGBoost) ve topluluk öğrenme (ensemble learning) yaklaşımları ile model karşılaştırmalarının yapılması, daha kapsamlı bir analiz sunabilir.*

## Kaynaklar
> *Orange Data Mining Documentation. Getting started and widget reference. https://orangedatamining.com/docs/*
> *Kaggle. Heart Failure Clinical Records Dataset. https://www.kaggle.com/datasets/andrewmvd/heart-failure-clinical-data*
> *Raschka, S., & Mirjalili, V. (2020). Python Machine Learning (3rd ed.). Packt Publishing.*
> *Brownlee, J. (2021). Classification Accuracy is Not Enough: More Performance Measures You Can Use. Machine Learning Mastery. https://machinelearningmastery.com/classification-accuracy-is-not-enough/*
> *Kelleher, J. D., Mac Namee, B., & D'Arcy, A. (2020). Fundamentals of Machine Learning for Predictive Data Analytics (2nd ed.). MIT Press.*

## Ekler
### Orange Proje Dosyası
> YMT5270-Vize.ows (project/YMT5270-Vize.ows)

### Veri Seti Dosyası veya Bağlantısı
> hfail.csv (project/hfail.csv)
