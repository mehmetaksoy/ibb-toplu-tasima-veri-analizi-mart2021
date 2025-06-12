# İBB Mart 2021 Toplu Taşıma Veri Analizi ve GTFS Entegrasyonu

Bu proje, İstanbul Büyükşehir Belediyesi'nin (İBB) açık veri portalından elde edilen Mart 2021 toplu taşıma kullanım verileri ile Genel Toplu Taşıma Beslemesi (GTFS) spesifikasyonlarından gelen verileri birleştirerek kapsamlı bir veri analizi sunmaktadır. Python programlama dili ve popüler veri bilimi kütüphaneleri kullanılarak, ham verilerin çekilmesinden başlayarak, veri ön işleme, farklı kaynakların birleştirilmesi, zenginleştirilmesi ve kapsamlı Keşifsel Veri Analizi (EDA) adımları detaylı bir şekilde incelenmektedir.

Bu proje, şehir planlaması, toplu taşıma optimizasyonu ve veri odaklı karar verme süreçlerine ilgi duyan herkes için değerli içgörüler sunmayı amaçlamaktadır.

**▶️ Projenin Detaylı Anlatımını İçeren YouTube Videom:** https://youtu.be/aySKa85WX28

## Projenin Temel Amaçları

* İBB açık veri portalından ve GTFS kaynaklarından toplu taşıma verilerini API aracılığıyla çekmek ve hazırlamak.
* Saatlik kullanım verileri ile çeşitli GTFS bileşenlerini (rota, sefer, durak, frekans bilgileri) etkin bir şekilde eşleştirmek ve `df_analysis_mart2021` adında ana analiz veri çerçevesini oluşturmak.
* Veriyi zenginleştirerek ve frekans bilgilerini entegre ederek daha derinlemesine analizlere olanak sağlamak.
* Kapsamlı Keşifsel Veri Analizi (EDA) ile aşağıdaki gibi sorulara yanıt aramak:
    * Zamansal trendler (günlük, haftanın günü bazında kullanım yoğunlukları) nasıl bir dağılım gösteriyor?
    * Farklı ulaşım modlarının (Otobüs, Metro, Vapur vb.) saatlik yoğunluk profilleri nelerdir?
    * Mart 2021'de en yoğun kullanılan toplu taşıma hatları ve bu hatların geçtiği ilçeler hangileridir?
    * Aktarma davranışları ve farklı bilet türlerinin kullanım oranları nasıldır?
    * Pik saatlerdeki (sabah ve akşam) yoğunluklar, sefer sıklıkları ve sefer başına düşen tahmini yolcu sayıları nasıl analiz edilebilir ve görselleştirilebilir?

## Kullanılan Ana Veri Kaynakları

* **İBB Açık Veri Portalı ([data.ibb.gov.tr](https://data.ibb.gov.tr)):**
    * Saatlik Toplu Taşıma Kullanım Verisi (Mart 2021)
    * Kapsamlı GTFS Veri Setleri (agency, routes, trips, stop_times, stops, frequencies vb.)
    * İETT'ye özel GTFS rota bilgileri
* Veriler, not defterinde gösterildiği gibi API çağrıları ile dinamik olarak çekilmiştir.

## Kullanılan Araçlar ve Kütüphaneler

* **Programlama Dili:** Python 3
* **Veri Manipülasyonu ve Analizi:** Pandas, NumPy
* **Veri Görselleştirme:** Matplotlib, Seaborn
* **API Erişimi:** Requests
* **İlerleme Takibi ve Loglama:** Logging modülü
* **Grafik İyileştirmeleri:** `adjustText` (grafiklerde metin etiketlerinin üst üste binmesini engellemek için)
* **Geliştirme Ortamı:** Google Colaboratory

## Proje Adımları ve Analiz Akışı

Bu proje, Colab not defterindeki hücrelerle ve yukarıda bağlantısı verilen YouTube videosundaki zaman damgalarıyla takip edilebilecek şekilde yapılandırılmıştır:

1.  **Kurulum, Ana Veri Yükleme ve Temel GTFS Hazırlığı (Video 00:00):**
    * Gerekli kütüphanelerin import edilmesi ve ayarların yapılması.
    * Google Drive bağlantısının kurulması.
    * `logging` modülü ile loglama altyapısının oluşturulması.
    * İBB Açık Veri Portalı'ndan Mart 2021 saatlik kullanım verisinin (`hourly_transportation_202103.csv`) çekilmesi.
    * Temel GTFS dosyalarının (agency, routes vb. - hem kapsamlı hem de İETT için) indirilmesi ve ön hazırlığı.

2.  **GTFS Verilerinin Saatlik Kullanım Verileriyle Eşleştirilmesi (Ana Hedef: `df_analysis_mart2021` Oluşturma):**
    * **Hazırlık ve Gerekli Kontroller (Video 08:19):** Eşleştirme öncesi veri tipleri, eksik değerler ve temel istatistiklerin kontrolü.
    * **Öncelikli Hedefli Eşleştirme (Kapsamlı GTFS) (Video 11:25):** Saatlik kullanım verilerindeki `HAT_KODU` ve `ARAC_ID` gibi alanlar kullanılarak kapsamlı GTFS (`routes.txt`) ile ilk eşleştirme denemesi. `ROUTE_TYPE` bilgisi (örneğin, 3=Otobüs, 0=Tramvay, 1=Metro) bu aşamada önemli bir rol oynar.
    * **Vapur Hatları İçin Gelişmiş Eşleştirme (Video 16:24):** Vapur hatlarına (`ROUTE_TYPE` = 4) özel, `ROUTE_SHORT_NAME` ve `ROUTE_LONG_NAME` üzerinden daha detaylı bir eşleştirme mantığı.
    * **Kalanlar İçin İETT GTFS İle Eşleştirme (Video 22:04):** Önceki adımlarda eşleşmeyen kayıtların İETT'ye özel GTFS rota bilgileriyle eşleştirilmesi.
    * **Sonuçların Birleştirilmesi ve `df_analysis_mart2021` Oluşturma (Video 27:29):** Farklı eşleştirme stratejilerinden gelen sonuçların birleştirilerek ana analiz veri çerçevesinin (`df_analysis_mart2021`) son haline getirilmesi. Eşleşme durumları ve oranları bu adımda değerlendirilir.

3.  **Veri Zenginleştirme ve Frekans Bilgilerinin Entegrasyonu:**
    * **Ek GTFS (trips, frequencies) İndirme ve Hazırlama (Video 32:20):** GTFS `trips.txt` ve `frequencies.txt` dosyalarının indirilip, sefer ve frekans bilgilerini içerecek şekilde hazırlanması.
    * **Frekans Bilgisinin Ana Analiz DataFrame'ine Eklenmesi (Video 37:04):** Hazırlanan frekans bilgilerinin (`df_frequencies_processed`), `df_analysis_mart2021` veri çerçevesine saat aralıkları ve `trip_id` üzerinden entegre edilmesi. Bu, özellikle pik saat analizleri için önemlidir.

4.  **Kapsamlı Keşifsel Veri Analizi (EDA):**
    * **EDA Başlangıç - Zamansal Analizler ve Mod Bazlı İlk Bakış (Video 42:34):**
        * Mart 2021 boyunca günlük toplam geçiş sayılarının trendi.
        * Haftanın günlerine göre ortalama geçiş sayıları ve yoğunluk dağılımları.
        * Farklı ulaşım modlarının (Otobüs, Metro, Marmaray, Vapur vb.) genel kullanım oranları.
        * Ulaşım modlarına göre saatlik yoğunluk profilleri (günün hangi saatlerinde hangi mod daha yoğun).
    * **En Yoğun Hatlar ve İlçeler Analizi (Video 52:12):**
        * Toplam geçiş sayısına göre en yoğun toplu taşıma hatlarının belirlenmesi.
        * En yoğun hatların geçtiği (tahmini) ilçelerin analizi (Not: Not defterinde ilçe bilgisi doğrudan olmayabilir, ancak hat isimlerinden çıkarım yapılabilir veya ek veriyle zenginleştirilebilir).
    * **Aktarma ve Bilet Türü Analizleri (Video 1:00:23):**
        * Kullanılan bilet türlerinin (Tam, Öğrenci, Abonman vb.) dağılımı.
        * Aktarma davranışları (`AKTARMA_TURU` alanı üzerinden) ve en çok aktarma yapılan durumların incelenmesi.
    * **Pik Saat Yoğunluk ve Frekans Analizi (Video 1:06:06):**
        * Sabah (örn: 07:00-09:00) ve akşam (örn: 17:00-19:00) pik saatlerinin belirlenmesi.
        * Pik saatlerdeki toplam yoğunlukların ve modlara göre dağılımının incelenmesi.
        * **Pik Saat Yoğunluk Oranları Grafiği (Video 1:11:31):** Farklı hatların veya modların pik saatlerdeki genel yoğunluğa katkısının görselleştirilmesi.
        * **Yoğunluk - Frekans İlişkisi Saçılım Grafiği (Video 1:14:54):** Pik saatlerde hatların sefer sıklığı (frekans) ile taşıdığı yolcu yoğunluğu arasındaki ilişkinin incelenmesi.
        * **Pik Saatlerde Sefer Başına Düşen Tahmini Yolcu Sayısı Grafiği (Video 1:20:31):** Sefer sıklığı ve toplam yolcu sayısı kullanılarak, pik saatlerde bir seferin ortalama ne kadar yolcu taşıdığına dair bir göstergenin hesaplanması ve görselleştirilmesi.

## Nasıl Kullanılır?

1.  Bu Colab not defterini Google Colab'de açın.
2.  Not defterinin en başında Google Drive'ınıza erişim için gerekli kimlik doğrulama adımlarını tamamlayın.
3.  Veri dosyaları İBB Açık Veri Portalı'ndan API aracılığıyla dinamik olarak çekilmektedir. İlgili hücreler çalıştırıldığında veriler indirilecektir. Not defteri, `/content/drive/MyDrive/Colab Notebooks/Veri Bilimi Projesi/` gibi bir yola kaydetme veya oradan okuma eğiliminde olabilir; kendi Drive yapınıza göre bu yolları düzenlemeniz gerekebilir.
4.  Hücreleri sırayla çalıştırarak analiz adımlarını, çıktılarını ve görselleştirmelerini inceleyebilirsiniz.

## Video Zaman Damgaları (Kolay Takip İçin)

* **00:00** - Hücre 1: Kurulum, Ana Veri Yükleme ve Temel GTFS Hazırlığı
* **08:19** - Hücre 2.0: Hazırlık ve Gerekli Kontroller
* **11:25** - Hücre 2.1: Öncelikli Hedefli Eşleştirme (Kapsamlı GTFS)
* **16:24** - Hücre 2.2: Vapur Hatları İçin Gelişmiş Eşleştirme
* **22:04** - Hücre 2.3: Kalanlar İçin İETT GTFS İle Eşleştirme
* **27:29** - Hücre 2.4: Sonuçların Birleştirilmesi ve `df_analysis_mart2021` Oluşturma
* **32:20** - Hücre 2.5: Ek GTFS (trips, frequencies) İndirme ve Hazırlama
* **37:04** - Hücre 2.6: Frekans Bilgisinin Ana Analiz DataFrame'ine Eklenmesi
* **42:34** - Hücre 3.0: EDA Başlangıç - Zamansal Analizler ve Mod Bazlı İlk Bakış
* **52:12** - Hücre 3.1: En Yoğun Hatlar ve İlçeler Analizi
* **1:00:23** - Hücre 3.2: Aktarma ve Bilet Türü Analizleri
* **1:06:06** - Hücre 3.3: Pik Saat Yoğunluk ve Frekans Analizi
* **1:11:31** - Hücre 3.3.1: Pik Saat Yoğunluk Oranları Grafiği
* **1:14:54** - Hücre 3.3.2: Yoğunluk - Frekans İlişkisi Saçılım Grafiği
* **1:20:31** - Hücre 3.3.3: Pik Saatlerde Sefer Başına Düşen Yolcu Sayısı Grafiği
* **1:24:47** - Kapanış ve Değerlendirme (Videoda)

## Katkıda Bulunma ve İletişim

Proje hakkındaki sorularınızı, geri bildirimlerinizi veya potansiyel iyileştirme önerilerinizi iletebilirsiniz.

---


## 📞 İletişim

🐛 **Bug Report**: GitHub Issues kullanın  
💡 **Feature Request**: Discussions bölümünden önerinizi paylaşın  
📧 E-posta: [mehmetaksoy49@gmail.com]

- Pull Request ile katkıda bulunun
- Projeyi yıldızlamayı unutmayın! ⭐

---

**Not**: Bu proje eğitim amaçlı geliştirilmiştir ve akademik çalışmalarda referans olarak kullanılabilir.

