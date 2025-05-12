PROJE RAPORU

Proje Adı: YouTube Video Özetleyici

Hazırlayan: Burhan Karadağlı 202523010

Tarih: 17.04.2024

Ders: Mühendislikte Bilgisayar Uygulamaları 2

Giriş ve Amaç
YouTube gibi platformlarda yer alan video içeriklerin süresi giderek artmaktadır. Bu durum verimliği azaltmakla kalmayıp tam bir zaman kaybına dönüşmektedir. Özellikle eğitim, haber veya belgesel gibi bilgi odaklı uzun videoların tamamını izlemek yerine ana fikirlerini hızlıca öğrenme ihtiyacı doğmaktadır. Bu proje, bu ihtiyaca cevap vermek ve zaman kaybını engelleme amacıyla oluşturulmuştur.

Projenin Temel Amacı: Kullanıcının girdiği bir YouTube video linkinden(Türkçe veya İngilizce), videonun sesini otomatik olarak indirmek, bu sesi metne dönüştürmek (transkript oluşturmak) ve elde edilen metni yapay zeka (Google Gemini API) kullanarak anlamlı bir şekilde Türkçe olarak özetlemektir. Bu sayede kullanıcılar, uzun videoların içeriği hakkında kısa sürede bilgi sahibi olabilirler.

Hedef Kitle
Bu araç öncelikli olarak aşağıdaki grupları hedeflemektedir: Öğrenciler: Ders tekrarları, ders notu çıkarma veya araştırma için video içerikleri hızlıca taramak isteyenler. Araştırmacılar: Alanlarıyla ilgili video sunumları veya konferans kayıtlarını hızlıca değerlendirmek isteyenler. İçerik Üreticileri: Kendi veya başkalarının videolarının özetlerini oluşturmak isteyenler. Zamanı Kısıtlı Profesyoneller: Sektörleriyle ilgili güncel bilgileri içeren uzun videoları takip etmek isteyenler.

Genel Kullanıcılar: Merak ettikleri konulardaki uzun videoların ana hatlarını öğrenmek isteyenler.

Proje Kapsamı
Kapsama Dahil Olanlar: Kullanıcıdan YouTube video URL'si ve Google Gemini API anahtarı istemek. yt-dlp kütüphanesi kullanarak verilen URL'den videonun sadece ses dosyasını indirmek. İndirilen ses dosyasını OpenAI'nin Whisper modeli (bu projede base modeli) kullanarak metne dönüştürmek. Oluşturulan metni, google-generativeai kütüphanesi aracılığıyla Google Gemini API'yına göndererek Türkçe özetini oluşturmak. Özeti kullanıcı arayüzünde (GUI) göstermek. Oluşturulan özeti yerel bir metin dosyasına (video_ozeti.txt) kaydetmek. Basit bir grafik kullanıcı arayüzü (GUI) sunmak (customtkinter ile). Uzun süren işlemleri (indirme, transkript, özetleme) ayrı bir thread üzerinde çalıştırarak GUI'nin donmasını engellemek. Açık/Koyu özelliği sunmak(göz rahatlığı için). Program başlangıcında FFmpeg'in kurulu olup olmadığını kontrol etmek ve kullanıcıyı uyarmak.

Kapsama Dahil Olmayanlar: (İhtiyaç olmadığından bunlar kapsam dışındandır, ihtiyaca sonrada da eklenebilir.) YouTube canlı yayınlarının desteklenmesi. Çoklu video işleme veya kuyruk yönetimi. Kullanıcı hesapları veya geçmiş işlem kaydı. Gelişmiş özetleme parametreleri (örn. özet uzunluğu ayarı).

Kullanılan Teknolojiler ve Araçlar Programlama Dili: Python
Python Sürümü: Proje, Python 3.9 ve üzeri sürümlerle uyumlu olacak şekilde geliştirilmiştir (Python 3.10.6).

Ana Kütüphaneler:

customtkinter: Modern görünümlü ve tema destekli grafik kullanıcı arayüzü (GUI) oluşturmak için. yt-dlp: YouTube ve birçok diğer platformdan video/ses indirmek için güçlü ve esnek bir kütüphane. openai-whisper: İndirilen ses dosyalarını yüksek doğrulukla metne dönüştürmek için kullanılan açık kaynaklı bir model. google-generativeai: Google Gemini API ile etkileşim kurarak metin özetleme işlemini gerçekleştirmek için (burada gemini-2.0-flash modelini kullanacağız). threading: Uzun süren görevleri arka planda çalıştırarak GUI'nin yanıt verirliğini korumak için. os, sys, logging, subprocess: Dosya sistemi işlemleri, sistem etkileşimi, olay günlüğü tutma ve harici program (FFmpeg) kontrolü gibi standart görevler için.

Harici Bağımlılık:

FFmpeg: yt-dlp'nin bazı ses formatlarını indirmesi/dönüştürmesi ve Whisper'ın bazı ses dosyalarını işlemesi için gereklidir. Kullanıcının sisteminde kurulu ve PATH'e eklenmiş olması beklenir.

Modüler Yapı
Proje, kodun okunabilirliğini, yönetilebilirliğini ve tekrar kullanılabilirliğini artırmak amacıyla modüler bir yapıda tasarlanmıştır:

main_gui.py: Ana uygulama dosyasıdır. Kullanıcı arayüzünü (GUI) oluşturur, kullanıcı girdilerini alır, diğer modülleri çağırarak iş akışını yönetir ve sonuçları kullanıcıya gösterir. Thread yönetiminden de sorumludur.

downloader.py: Sadece YouTube'dan ses indirme işlemini gerçekleştiren fonksiyonu (download_audio_yt_dlp) içerir. yt-dlp kütüphanesi ile ilgili mantık burada bulunur.

transcriber.py: Ses dosyasını metne çevirme işlemini gerçekleştiren fonksiyonu (transcribe_audio) içerir. Whisper modelinin yüklenmesi ve kullanılmasıyla ilgili mantık buradadır. İsteğe bağlı olarak işlenen ses dosyasını silebilir.

summarizer.py: Metni özetleme (summarize_text) ve özeti dosyaya kaydetme (save_summary) fonksiyonlarını içerir. Google Gemini API ile iletişim ve dosya yazma işlemleri bu modülde yer alır.

Bu yapı, her bir bileşenin görevini netleştirir ve gelecekteki değişikliklerin veya hata ayıklamanın daha kolay yapılmasını sağlar.

Metodoloji ve Geliştirme Süreci
Proje geliştirme sürecinde çevik (agile) yaklaşıma benzer adımlar izlenmiştir:

Çekirdek Fonksiyonellik: İlk olarak, komut satırı üzerinden çalışacak şekilde temel indirme, transkript ve özetleme fonksiyonları ayrı ayrı geliştirilip test edildi.

GUI Entegrasyonu: customtkinter kullanılarak temel kullanıcı arayüzü tasarlandı ve çekirdek fonksiyonlar GUI olaylarına bağlandı.

Threading Implementasyonu: Uzun süren işlemlerin GUI'yi kilitlememesi için threading modülü kullanılarak bu işlemler arka plana taşındı ve GUI'nin güvenli güncellenmesi için root.after metodu kullanıldı.

Hata Yönetimi ve Geri Bildirim: try-except blokları ile olası hatalar (dosya bulunamadı, API hatası, ağ hatası vb.) yakalanmaya çalışıldı. Kullanıcıya messagebox ve durum çubuğu (status_label) aracılığıyla bilgilendirme ve hata mesajları gösterildi.

Ek Özellikler: Tema değiştirme ve FFmpeg kontrolü gibi ek kullanıcı deneyimi özellikleri eklendi.

Modülerleştirme: Kod mantıksal parçalara ayrılarak downloader.py, transcriber.py, summarizer.py modülleri oluşturuldu.

Test ve İyileştirme: Farklı video linkleri, API anahtarları ve senaryolarla testler yapılarak hatalar giderildi.

Çalışma Prensibi (İş Akışı)
Kullanıcı programı çalıştırır ve GUI açılır. Kullanıcı ilgili alanlara geçerli bir YouTube video URL'si ve Google Gemini API anahtarını girer. Kullanıcı "Video Özeti Çıkar" butonuna tıklar. GUI, girdilerin temel geçerliliğini kontrol eder (boş olup olmadığını, URL formatını). Girdiler geçerliyse, buton devre dışı bırakılır ve durum "İşleniyor..." olarak güncellenir. Ana run_processing fonksiyonu ayrı bir thread'de başlatılır.

Thread İçinde: downloader.download_audio_yt_dlp çağrılarak ses indirilir. Durum güncellenir. transcriber.transcribe_audio çağrılarak ses metne dönüştürülür. Durum güncellenir. Ses dosyası silinir. summarizer.summarize_text çağrılarak metin Gemini API ile özetlenir. Durum güncellenir. Özet, update_summary_text aracılığıyla GUI'deki metin kutusuna yazdırılır. summarizer.save_summary çağrılarak özet video_ozeti.txt dosyasına kaydedilir. İşlem durumu (başarılı/başarısız kaydetme) kullanıcıya messagebox ile bildirilir.

Thread Sonu (Finally Bloğu): İşlem butonu root.after kullanılarak tekrar aktif edilir. Varsa kalan geçici dosyalar temizlenir. Thread sonlanır. Kullanıcı yeni bir işlem yapabilir veya programı kapatabilir.

eNpNVMtu2zAQvPcrFj45QIKieV0KtJAiv2IrNhqnQBrkwFpbhxBFGRRlwCH6G736mGt8yck32f_VJfWwdRM1Mzu7O9RcscULTINPQI_35LP9WjD9DGdn38A3rd27iGGYq1zkCbTDNCq2QqA6hW4q44yvUikYvY3S-RzVSevvV6fjO_qN8SSD3sMAJihnqDDjksNY5Pu1zhUsEbwVU4J

Beklenen Çıktılar
Ana Çıktı: Kullanıcının girdiği YouTube videosunun içeriğini yansıtan, Türkçe olarak oluşturulmuş, okunabilir bir metin özeti. Arayüzde Gösterim: Oluşturulan özetin programın GUI'sindeki metin kutusunda görüntülenmesi. Dosya Kaydı: Oluşturulan özetin programın çalıştığı dizinde video_ozeti.txt adıyla bir metin dosyasına kaydedilmesi. Durum Bildirimleri: İşlem adımları (indirme, transkript, özetleme) ve olası hatalar hakkında kullanıcıyı bilgilendiren durum mesajları.

Test ve Doğrulama
Projenin doğruluğu ve güvenilirliği için aşağıdaki testler yapılmıştır planlanmaktadır:

Birim Testler (Planlanan): Her modül (downloader, transcriber, summarizer) için temel fonksiyonların beklenen girdilerle doğru çıktıları üretip üretmediğini ve hata durumlarını doğru yönetip yönetmediğini kontrol edilmiştir.

Entegrasyon Testleri: Farklı uzunluklarda, farklı ses kalitelerinde ve farklı konulardaki YouTube videoları ile tüm iş akışının baştan sona test edilmesi. Geçersiz URL, hatalı API anahtarı, internet bağlantısı kesintisi gibi senaryoların denenmesi. FFmpeg kurulu olmadığında programın uyarı verip vermediğinin kontrolü yapılmıştır.

Kullanıcı Arayüzü Testleri: GUI elemanlarının (butonlar, giriş alanları, metin kutusu) doğru çalışıp çalışmadığı, butonun işlem sırasında doğru şekilde devre dışı kalıp tekrar aktif olup olmadığı, tema değiştirmenin sorunsuz çalıştığı, durum mesajlarının doğru görüntülendiği kontrol edilmiştir.

Sonuçlar ve Değerlendirme
Proje, belirlenen temel amacı başarıyla yerine getirmektedir. Kullanıcılar, sağlanan arayüz üzerinden kolayca Türkçe veya İngilizce bir YouTube videosunun Türkçe özetini çıkartabilmektedir.

Başarılar:

Modüler yapı sayesinde kod yönetimi kolaylaşmıştır. threading kullanımı ile uzun işlemlerde GUI donması engellenmiştir. customtkinter ile modern ve kullanıcı dostu bir arayüz sağlanmıştır. yt-dlp, Whisper ve Gemini API gibi güçlü araçların entegrasyonu başarılı olmuştur. Temel hata yakalama ve kullanıcı bilgilendirme mekanizmaları mevcuttur.

Karşılaşılan Zorluklar ve Sınırlılıklar:

API Bağımlılığı: Gemini API anahtarı gerekliliği ve API'nin kullanım limitleri/maliyeti bir sınırlamadır. Transkript Kalitesi: Whisper'ın base modeli hızlı olsa da, çok gürültülü veya karmaşık seslerde transkript kalitesi düşebilir. Daha büyük modeller daha iyi sonuç verir ancak daha yavaştır ve daha fazla kaynak gerektirir. Özet Kalitesi: Özetin kalitesi, hem transkriptin doğruluğuna hem de Gemini API'nin yeteneklerine bağlıdır. Çok teknik veya niş konularda özet kalitesi değişebilir. FFmpeg Gerekliliği: Kullanıcının sistemine ek bir yazılım (FFmpeg) kurma gerekliliği bir engel olabilir. Hata Yönetimi: Hata yönetimi daha kapsamlı hale getirilebilir (örn. ağ hatalarının daha detaylı ele alınması). İnternet Bağlantısı: Tüm işlemler internet bağlantısı gerektirir.

Gelecek Çalışmalar ve İyileştirmeler
Kullanıcının farklı Whisper modellerini (tiny, small, medium, large) seçebilmesi için arayüze bir seçenek eklemek. Özetleme için farklı modelleri (örn. farklı Gemini versiyonları veya başka API'ler) desteklemek veya parametre ayarı (örn. özet uzunluğu) sunmak. İlerleme çubuğu (progress bar) ekleyerek indirme ve transkript süreçlerinin ilerlemesini daha görsel hale getirmek. Desteklenen dilleri artırmak (hem transkript hem özetleme için). FFmpeg'in otomatik kurulumu veya gömülü bir çözüm araştırmak. Daha detaylı loglama ve hata ayıklama seçenekleri sunmak. Uygulamayı paketleyerek (örn. PyInstaller ile) kolayca dağıtılabilir bir .exe dosyası oluşturmak.

Bu rapor, projenin mevcut durumunu, hedeflerini ve potansiyelini özetlemektedir. Geliştirme süreci öğretici olmuş ve farklı teknolojilerin bir araya getirilmesi konusunda önemli deneyimler kazandırmıştır. 
