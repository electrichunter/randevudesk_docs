# 🚀 RandevuDesk — Kapsamlı Sistem Özellikleri ve Mimari Kılavuzu

Bu belge, **RandevuDesk** B2B SaaS Dinamik Randevu, Rezervasyon, Stok, Finans, Sağlık & Güzellik İhtisas Modülleri ve Çok Kiracılı (Multi-Tenant) İşletme Yönetim Platformu'nun tüm mimari yeteneklerini, iş akışlarını, güvenlik yapılandırmalarını ve teknik özelliklerini en ince ayrıntısına kadar açıklamaktadır.

---

## 📑 İÇİNDEKİLER
1. [Platform Mimarisi ve Teknoloji Yığını](#1-platform-mimarisi-ve-teknoloji-yığını)
2. [📅 Akıllı Randevu ve Rezervasyon Motoru](#2-akıllı-randevu-ve-rezervasyon-motoru)
3. [🔗 Kullanıcı Dostu Temiz Link & Slug Mimarisi](#3-kullanıcı-dostu-temiz-link--slug-mimarisi)
4. [🏢 Kurumsal Bilgiler ve Doğrulanmış Şirket Künyesi](#4-kurumsal-bilgiler-ve-doğrulanmış-şirket-künyesi)
5. [💰 Sabit ve Şeffaf Abonelik Paketleri (Fiyatlandırma)](#5-sabit-ve-şeffaf-abonelik-paketleri-fiyatlandırma)
6. [⚙️ Süper Admin Dinamik Site ve Fiyat Yönetimi](#6-süper-admin-dinamik-site-ve-fiyat-yönetimi)
7. [🦷 Diş Kliniği ve Medikal İhtisas Modülleri](#7-diş-kliniği-ve-medikal-ihtisas-modülleri)
8. [🧪 Müşteri Teknik Geçmişi ve Formülasyon / Reçete Takibi](#8-müşteri-teknik-geçmişi-ve-formülasyon--reçete-takibi)
9. [📦 Stok, Sarf Malzeme ve Perakende Satış Takip Modülü](#9-stok-sarf-malzeme-ve-perakende-satış-takip-modülü)
10. [💼 Personel Primi, Hak Ediş ve Maaş Bordrolama Sistemi](#10-personel-primi-hak-ediş-ve-maaş-bordrolama-sistemi)
11. [👑 Daima Aktif Premium Süre Uzatma & PayTR Altyapısı](#11-daima-aktif-premium-süre-uzatma--paytr-altyapısı)
12. [🗺️ Masa, Koltuk, Oda & Kroki Yönetimi (Spaces)](#12-masa-koltuk-oda--kroki-yönetimi-spaces)
13. [⏳ Akıllı Bekleme Listesi (Waitlist) ve Otomasyon](#13-akıllı-bekleme-listesi-waitlist-ve-otomasyon)
14. [📱 QR Menü & Canlı Stand Tasarım Stüdyosu](#14-qr-menü--canlı-stand-tasarım-stüdyosu)
15. [📈 Finansal Analitik, RFM Müşteri Segmentasyonu & Ciro Hedefleri](#15-finansal-analitik-rfm-müşteri-segmentasyonu--ciro-hedefleri)
16. [🔔 Bildirim Merkezi, PWA & Web Push Bildirim Altyapısı](#16-bildirim-merkezi-pwa--web-push-bildirim-altyapısı)
17. [💬 WhatsApp Cloud API Otomasyonu (2 Saat & 24 Saat Hatırlatıcı)](#17-whatsapp-cloud-api-otomasyonu-2-saat--24-saat-hatırlatıcı)
18. [⭐ Müşteri Değerlendirme, Puanlama ve Memnuniyet Ligi](#18-müşteri-değerlendirme-puanlama-ve-memnuniyet-ligi)
19. [🤝 Pazarlamacı (Affiliate / Referral) ve Komisyon Portalı](#19-pazarlamacı-affiliate--referral-ve-komisyon-portalı)
20. [💾 Otomatik Veritabanı Yedekleme ve SQL Dump Dışa Aktarma](#20-otomatik-veritabanı-yedekleme-ve-sql-dump-dışa-aktarma)
21. [📝 Dinamik Sektör Alanları ve SEO Blog Yönetimi](#21-dinamik-sektör-alanları-ve-seo-blog-yönetimi)
22. [🛡️ Siber Güvenlik, OWASP Dayanıklılığı ve KVKK Uyumluluğu](#22-siber-güvenlik-owasp-dayanıklılığı-ve-kvkk-uyumluluğu)
23. [🎨 Tasarım İlkeleri: Sıfır İkon, Sıfır Emoji ve Rem Ölçü Standardı](#23-tasarım-ilkeleri-sıfır-ikon-sıfır-emoji-ve-rem-ölçü-standardı)
24. [🧪 Test Güvencesi, Sağlık Denetimleri ve Canlı İzleme](#24-test-güvencesi-sağlık-denetimleri-ve-canlı-izleme)

---

## 1. Platform Mimarisi ve Teknoloji Yığını

RandevuDesk, yüksek eşzamanlılık (high concurrency), düşük gecikme ve sıfır veri kaybı hedefleyen mikroservis mimarisiyle Docker üzerinde koşar:

- **Frontend:** Next.js 16 (App Router), React 19, TypeScript 5, Tailwind CSS v4.
  - *Tasarım Felsefesi:* İkon kalabalığından ve emojilerden tamamen arındırılmış, kurumsal tipografi ve net rozetler (badges) odaklı, açık mavi / beyaz / kurumsal slate renk paleti.
  - *Responsive Standart:* Keyfi `%` ve `px` yerine katı rem bazlı Tailwind ölçekleri (`w-full`, `max-w-6xl`, `p-6`, `gap-4`).
- **Backend:** Python 3.12, FastAPI (Asenkron ASGI mimarisi), SQLAlchemy 2.0 (Tamamen parametrik asenkron ORM), Pydantic v2 (Tip güvenliği ve girdi sanitizasyonu).
- **Veritabanı & Önbellek:** 
  - MySQL 8.0 (InnoDB, `utf8mb4_turkish_ci`, ACID garantisi, indeksli yabancı anahtarlar).
  - Redis 8.10 (Slot önbelleği, brute-force IP sayacı ve iptal edilen JWT kara listesi).
- **Medya ve Belge Depolama:** MinIO Object Storage (S3 uyumlu, UUID ile yeniden adlandırılmış ve sadece güvenli görsel formatları kabul eden izole depolama).
- **Ters Vekil (Reverse Proxy) & Ağ Güvenliği:** Nginx Reverse Proxy (HTTP/2, SSL/TLS 1.3, Rate Limiting, WAF başlıkları, dahili `/nginx-health` uç noktası).
- **Arka Plan Görevleri:** `asyncio` tabanlı zamanlanmış servisler (Otomatik SQL yedekleme, 24 saat & 2 saat önce randevu hatırlatıcıları, pazarlamacı veri temizliği).

---

## 2. 📅 Akıllı Randevu ve Rezervasyon Motoru

RandevuDesk'in çekirdek randevu motoru, randevu çakışmalarını ve salon içi zaman kayıplarını sıfıra indirir:

1. **Dinamik Müsaitlik Matrisi (Redis Destekli):**
   - İşletmenin haftalık çalışma günleri (Örn: Pazartesi-Cumartesi), genel mesai saatleri (09:00 - 19:00), mola saatleri (13:00 - 14:00) taranır.
   - İlgili günde izinli olan personeller (`staff_leaves`) otomatik olarak slot hesaplamasından düşülür.
   - Seçilen hizmetin süresi (Örn: Saç Kesimi 30 dk, Diş İmplantı 90 dk) hesaplanarak takvimde yalnızca *kesintisiz boş olan* saat aralıkları müşteriye sunulur.
2. **Sürükle-Bırak Randevu Akışı:**
   - Panelde randevular "Bekliyor", "Onaylandı", "Tamamlandı", "İptal Edildi" sütunları arasında fareyle veya dokunmatik ekranla sürüklenerek anında güncellenir.
   - Randevu saati değiştirilirken 30 dakikadan fazla kaydırmalarda operasyonel bütünlük kontrolü uygulanır.
3. **Müşteri Online Randevu Formu (`/form/[slug_or_id]`):**
   - Mobil uyumlu adım adım (step-by-step) randevu formu: Hizmet seçimi -> Personel seçimi -> Dinamik sektör soruları -> Tarih & Saat seçimi -> İletişim bilgileri -> Onay.
   - Her randevu için benzersiz 8 haneli alfanümerik **Takip Kodu** üretilir. Müşteri bu kodla randevusunu dilediği an canlı takip edebilir.

---

## 3. 🔗 Kullanıcı Dostu Temiz Link & Slug Mimarisi

Sistemin URL yapısı, teknik veritabanı ID'leri yerine okunabilir kurumsal adresler üretecek şekilde tasarlanmıştır:

- **Eski Format:** `https://randevudesk.com/dashboard?salon_id=1`
- **Yeni Temiz Format:** `https://randevudesk.com/dashboard?salon=salon-beyoglu-kuafor-1` veya doğrudan yol olarak `https://randevudesk.com/dashboard/salon-beyoglu-kuafor-1`
- **Çözümleme Algoritması (`parseSalonIdentifier`):**
  - Trailing ID yakalayıcı regex (`salon-[slug]-[id]`, `salon([slug])[id]`, yalın slug veya numerik ID) sayesinde tüm rotalar ve alt sayfalar (`appointments`, `services`, `settings`, `notifications`, `qr-menu`) kusursuz çalışır.
- **Canlı Adres Çubuğu Senkronizasyonu:**
  - Bir kullanıcı eski `?salon_id=1` parametresiyle giriş yapsa dahi, `DashboardView.tsx` sayfa yenilenmesine gerek kalmadan adresi `window.history.replaceState` ile anında temiz linke dönüştürür.
- **IDOR Güvenlik Duvarı:**
  - `DashboardAuthGuard.tsx`, URL'deki salon parametresini kullanıcının yetkili olduğu `user.salon_id` ile kıyaslar; yetkisiz müdahaleleri engelleyerek kendi salonuna yönlendirir.

---

## 4. 🏢 Kurumsal Bilgiler ve Doğrulanmış Şirket Künyesi

RandevuDesk ana sitesinde ve `/hakkimizda` sayfasında resmi kurumsal bilgiler eksiksiz olarak sunulmaktadır:

- **Vergi Numarası:** `8670862311` (Beyoğlu Vergi Dairesi)
- **Konum / Merkez:** `Beyoğlu / İstanbul`
- **Müşteri Hizmetleri & Destek Telefonları:**
  - `+90 532 271 46 37`
  - `+90 530 958 63 85`
- **E-Posta:** `destek@randevudesk.com` & `bilgi@randevudesk.com`
- **Arama Motoru Uyumluluğu:** Schema.org `Organization` JSON-LD biçiminde tescilli şirket unvanı, iletişim kanalları ve resmi lokasyon meta verileri arama motoru botlarına iletilir.

---

## 5. 💰 Sabit ve Şeffaf Abonelik Paketleri (Fiyatlandırma)

Platformda gizli maliyet veya belirsiz fiyat teklifleri bulunmaz. `/fiyatlar` sayfasında sunulan resmi 4 paket:

| Paket Adı | Süre | Fiyat | Aylık Eşdeğer | Temel Avantajlar |
| :--- | :---: | :---: | :---: | :--- |
| **14 Gün Ücretsiz Deneme** | 14 Gün | **0 TL** | 0 TL | Kredi kartı gerekmez, tüm Pro özellikler anında açık |
| **1 Aylık Pro** | 30 Gün | **499 TL** | 499 TL/ay | Sınırsız randevu, personel yönetimi, SMS/WhatsApp hatırlatıcılar |
| **3 Aylık Avantajlı Pro** | 90 Gün | **1.299 TL** | 433 TL/ay | **%15 Tasarruflu**, sezonluk işletmeler için ideal |
| **1 Yıllık Tam Kapsamlı Pro**| 365 Gün | **3.999 TL** | 333 TL/ay | **%35 Tasarruflu**, kesintisiz en avantajlı kurumsal çözüm |

- **Ödeme Güvencesi:** PayTR 256-Bit SSL Sanal POS altyapısı ile peşin veya taksitli güvenli ödeme.
- **Paket Karşılaştırma Matrisi:** Sınırsız personel, QR menü stüdyosu, detaylı ciro analitiği, müşteri formülasyon geçmişi ve bekleme listesi gibi tüm özellikler şeffaf tabloda kıyaslanabilir.

---

## 6. ⚙️ Süper Admin Dinamik Site ve Fiyat Yönetimi

Süper yöneticiler, hiçbir kod değişikliği veya sunucu yeniden başlatması yapmadan kurumsal verileri canlı güncelleyebilir:

- **Yönetim Sayfası:** `/admin/site-ayarlari`
- **Yönetilebilir Veriler:**
  - Vergi dairesi ve vergi numarası.
  - Birincil ve ikincil kurumsal telefonlar, destek e-postası.
  - Şirket açık adresi, Google Maps harita koordinatları.
  - Çalışma günleri ve destek mesai saatleri.
  - Sosyal medya bağlantıları (Instagram, LinkedIn, X, Facebook).
  - 1 Aylık, 3 Aylık ve 1 Yıllık paket fiyatları (TL cinsinden).
  - Hero ve kurumsal slogan metinleri.
- **Çift Katmanlı Veri Güvenliği:**
  - Veriler öncelikle MySQL `site_settings` tablosuna atomik olarak kaydedilir.
  - Eşzamanlı olarak `backend/data/site_settings.json` dosyasına yazılır; veritabanı bağlantı kesintilerinde dahi public sayfalar kesintisiz çalışmaya devam eder.

---

## 7. 🦷 Diş Kliniği ve Medikal İhtisas Modülleri

RandevuDesk yalnızca klasik salonları değil; medikal estetik merkezleri ve diş kliniklerini de tam donanımlı destekler:

1. **Diş Hekimi Anamnez & Tıbbi Geçmiş Formu:**
   - Müşteri profilinde tıbbi risk bayrakları: Penisilin alerjisi, lokal anestezi alerjisi, kalp pili / hipertansiyon, kan sulandırıcı (aspirin/kumadin) kullanımı.
   - Randevu ekranında hekime kırmızı renkte hayati risk uyarıları gösterilir.
2. **Protez & Diş Laboratuvarı Takip Modülü (`tab=dental_lab`):**
   - Zirkonyum, porselen kaplama, lamina veya hareketli protezlerin laboratuvara gönderiliş tarihi, prova tarihi ve teslimat durumu adım adım izlenir.
3. **İmplant & Medikal Lot Takibi (`tab=dental_implants`):**
   - Hastaya uygulanan implantın markası, çapı, boyu ve sterilizasyon/üretim **Lot Numarası** hasta kartına işlenir.
   - Sağlık Bakanlığı ve medikal cihaz takip standartlarına tam uyumludur.
4. **Cerrahi İşlem Prim Oranları:**
   - Hekimlerin cerrahi operasyonları için standart hizmet priminden farklı olarak `%40` gibi bağımsız bir cerrahi komisyon oranı tanımlanabilir.

---

## 8. 🧪 Müşteri Teknik Geçmişi ve Formülasyon / Reçete Takibi

Kuaförlük ve estetisyenlikte müşteriye daha önce yapılan teknik işlemlerin (boya kodu, gramaj, volüm, bekleme süresi) kaybolmasını önler:

- **Telefon Tabanlı Eşleşme:** Müşteri 6 ay sonra bile gelse, telefon numarası üzerinden eski tüm teknik kartları listelenir.
- **Reçete Detayları:**
  - Örnek: `"Dip: Igora 7.1 (40g) + 7.0 (20g) + %6 20vol (60g). Bekleme: 35 dk. Uçlar: Cila 9.1 + 0.22 bezelye tanesi."`
- **Alerji ve Hassasiyet Notları:** Saç derisi hassasiyeti, amonyak reaksiyonu veya kullanılan özel organik açıcılar kaydedilir.
- **Görsel Öncesi/Sonrası Depolama:** Yapılan işlemin sonucu MinIO depolamasına fotoğraf olarak eklenebilir.

---

## 9. 📦 Stok, Sarf Malzeme ve Perakende Satış Takip Modülü

Salonlarda sadece hizmet verilmez; şampuan, krem, serum ve boya gibi ürünlerin satışı ve tüketimi de yapılır:

- **Ürün Kartı:** Ürün adı, Barkod/EAN, SKU, Kategori, Birim (Adet, ml, tüp), Kritik Stok Eşiği (Min Alert).
- **Maliyet ve Kâr Analizi:** Alış ve satış fiyatları ayrı tutularak net kâr marjı hesaplanır.
- **Kritik Stok Uyarıları:** Stok sayısı belirlenen limitin altına düştüğünde sistem otomatik bildirim üretir.
- **Odaya / Koltuğa Ürün Sevki:** Kasadan satış yapılırken ürün doğrudan krokideki bir odaya (Örn: `VIP Masaj Odası 1`) sevk edilebilir.
- **Zayi / Fire Kaydı:** Dökülen veya kırılan ürünler stok hareketlerine zayi olarak işlenir.

---

## 10. 💼 Personel Primi, Hak Ediş ve Maaş Bordrolama Sistemi

Personellerin adil ve şeffaf hak ediş hesaplamalarını otomatikleştirir:

- **Üçlü Komisyon Yapısı:**
  - **Hizmet Primi (%):** Uzmanın tamamladığı randevu cirosundan aldığı yüzde.
  - **Ürün Satış Primi (%):** Kasadan sattığı perakende ürünlerden aldığı yüzde.
  - **Cerrahi / Özel İşlem Primi (%):** Ameliyat ve klinik müdahalelerden aldığı yüzde.
  - **Sabit Taban Maaş (TL):** Aylık garanti maaş tutarı.
- **Aylık Bordro Raporu (`tab=payroll`):**
  $$\text{Toplam Hak Ediş} = \text{Taban Maaş} + (\text{Hizmet Cirosu} \times \%_{\text{Hizmet}}) + (\text{Ürün Cirosu} \times \%_{\text{Ürün}})$$
- Tek tıkla personelin tamamladığı randevu sayısı, ciro katkısı ve hak ettiği net tutar listelenir.

---

## 11. 👑 Daima Aktif Premium Süre Uzatma & PayTR Altyapısı

İşletmelerin abonelik süresi dolmasını beklemeden diledikleri zaman paketlerini uzatabilmeleri sağlanmıştır:

- **Erişim Noktaları:** Sidebar, sayfa başlığı (header) ve Ayarlar panelindeki dinamik kartlar.
- **Kümülatif Süre Ekleme:** Mevcut aboneliğin bitmesine 20 gün varken 90 günlük paket alınırsa yeni bitiş süresi $20 + 90 = 110$ gün olur.
- **Maksimum Sınır (13 Ay):** Kötüye kullanımı önlemek amacıyla abonelik süresi bugünden itibaren en fazla 396 gün (1 yıl 1 ay) ileriye uzatılabilir.
- **PayTR Callback Kuruş Doğrulaması:** İstemciden gelen tutara asla güvenilmez; PayTR'dan dönen tutar ile paketin kuruş değeri kuruşu kuruşuna eşleşmek zorundadır.

---

## 12. 🗺️ Masa, Koltuk, Oda & Kroki Yönetimi (Spaces)

İşletmenin fiziksel mekanını dijital ikizine dönüştüren 2D kroki stüdyosu:

- **Sürükle-Bırak 2D Izgara:** Koltuklar, masalar, sedyeler ve diş üniteleri parmakla veya fareyle yerleştirilir.
- **Canlı Doluluk Renkleri:** Hangi koltuğun şu anda dolu, hangisinin boş olduğu gerçek zamanlı renklerle gösterilir.
- **Mobil Taşma Koruması:** `PointerEvents` sınır kontrolleri ile küçük mobil ekranlarda krokilerin dışarı taşması engellenmiştir.

---

## 13. ⏳ Akıllı Bekleme Listesi (Waitlist) ve Otomasyon

Popüler saatlerde yer bulamayan müşterilerin salonu terk etmesini engeller:

1. Randevu bulamayan müşteri talep ettiği tarih ve saat aralığını belirterek bekleme listesine kaydolur.
2. Bir randevu iptal edildiğinde sistem sıradaki ilk müşteriye (FIFO) 15 dakika geçerli özel bir rezervasyon teklifi üretir.
3. Müşteriye anında WhatsApp/Push bildirimi iletilir; müşteri 15 dakika içinde onaylarsa randevu takvime otomatik kesinleştirilir. Onaylamazsa sıra sonraki müşteriye geçer.

---

## 14. 📱 QR Menü & Canlı Stand Tasarım Stüdyosu

Grafik tasarımcıya ihtiyaç duymadan salona özel profesyonel masa standı tasarlama imkanı:

- **Canlı Önizleme:** Seçilen kurumsal renk ve başlıklar sağ taraftaki telefon maketinde ve stand şablonunda anlık değişir.
- **Formatlar:** A4 Masa Kartlığı, A5 Dikey Masa Çerçevesi, Kare Masa Kartı.
- **Vektörel Çıktı:** Yüksek çözünürlüklü SVG ve PNG QR kodları doğrudan indirilebilir.
- **Dinamik Yönlendirme:** Okutulduğunda doğrudan `/menu/[slug_or_id]` sayfasına giderek güncel hizmetleri ve fiyatları listeler.

---

## 15. 📈 Finansal Analitik, RFM Müşteri Segmentasyonu & Ciro Hedefleri

İşletmenin büyüme performansını yönetici seviyesinde raporlar:

- **Aylık Ciro ve Randevu Hedefleri:** Ay başında belirlenen hedeflerin gerçekleşme yüzdesi canlı ilerleme çubuğu ile gösterilir.
- **RFM Müşteri Segmentasyonu:** Müşteriler ziyaret sıklığı ve harcama tutarına göre otomatik sınıflandırılır:
  - *VIP Müşteriler:* En sık gelen ve en yüksek harcamayı yapanlar.
  - *Sadık Müşteriler:* Düzenli periyotlarla randevu alanlar.
  - *Kayıp Riski Taşıyanlar:* Son 60 gündür randevu almayan eski müşteriler.
  - *Yeni Müşteriler:* İlk kez randevu oluşturanlar.
- **İstatistiki Gelecek Tahmini (Forecast):** Geçmiş 30 günün trendine göre gelecek ayın muhtemel cirosunu projeksiyon olarak sunar.

---

## 16. 🔔 Bildirim Merkezi, PWA & Web Push Bildirim Altyapısı

Tarayıcı kapalı olsa dahi randevu bildirimlerinin cep telefonuna ve bilgisayara anında düşmesi:

- **VAPID Web Push:** Standart W3C Push API ile tarayıcı seviyesinde güvenli kriptografik bildirimler.
- **Kurumsal Bildirim Paneli (`/dashboard/notifications`):**
  - Yeni randevu (`RDV`), durum değişikliği (`DRM`), saat güncellemesi (`ZMN`), müşteri yorumu (`YRM`) ve sistem duyurusu (`BIL`) rozetleri.
  - Okunmamışları tek tıkla onaylama ve geçmişi toplu temizleme.
- **PWA Desteği:** iOS ve Android cihazlarda Safari ve Chrome üzerinden "Ana Ekrana Ekle" ile mobil uygulama olarak kurulabilir.

---

## 17. 💬 WhatsApp Cloud API Otomasyonu (2 Saat & 24 Saat Hatırlatıcı)

Gelmeyen müşteri (no-show) oranını en aza indiren akıllı mesajlaşma:

- **24 Saat Önce:** Randevu teyit ve lokasyon hatırlatması.
- **2 Saat Önce:** Randevunun başlamak üzere olduğunu bildiren ve gelemeyecek müşteriler için tek tıkla iptal linki (`/randevu-iptal/[code]`) sunan acil durum mesajı.
- **Post-Op / İşlem Sonrası Bakım:** Randevu tamamlandıktan 2 saat sonra saç veya diş bakım önerileri ile müşteri yorum linki iletilir.
- **Güvenlik Kuralı:** WhatsApp mesajlaşma altyapısı sadece aktif Pro aboneliği olan işletmeler için devrededir; API anahtarları `.env` üzerinden şifreli yönetilir.

---

## 18. ⭐ Müşteri Değerlendirme, Puanlama ve Memnuniyet Ligi

Hizmet tamamlandıktan sonra şeffaf müşteri geri bildirimi:

- **Yorum Sayfası (`/yorum/[code]`):** Müşteri 1 ila 5 yıldız vererek deneyimini puanlar, dilerse isimsiz (anonim) yorum bırakabilir.
- **Personel Rekabet Podyumu:** Personeller aldıkları yıldız puanlarına ve tamamladıkları randevu adedine göre haftalık ligde sıralanır (1., 2., 3. sıra podyumu).

---

## 19. 🤝 Pazarlamacı (Affiliate / Referral), Rütbe & Satış / Devamlılık Takip Portalı

RandevuDesk'in organik büyümesini sağlayan, pazarlamacıların kaç paket sattığını, getirdiği ciroyu ve **müşterilerin Premium almaya devam edip etmediğini (Abonelik Devamlılığı / Retention)** anlık takip eden kurumsal ortaklık altyapısı:

- **Premium Alırken Pazarlamacı Kodu & Otomatik Devamlılık Bağlantısı:**
  - İşletmeler Pro paket satın alırken veya süre uzatırken modal üzerinde "Pazarlamacı / Referans Kodu" girebilir.
  - Kod girildiğinde sistem anında doğrular (`/api/v1/referral/verify/{kod}`) ve geçerli temsilcinin adını onay rozetiyle gösterir.
  - Bir işletme daha önce bir temsilci koduyla kayıt olduysa veya paket aldıysa, gelecekteki yenilemelerinde tekrar kod yazmasa bile kayıtlı temsilcisi otomatik olarak korunur ve devam satışları temsilcinin hanesine yazılır.
- **Satış Tespiti ve Otomatik Komisyon Tahakkuku:**
  - PayTR ödeme token'ında pazarlamacı kimliği güvenli olarak kodlanır.
  - Ödeme başarıyla tamamlandığı anda pazarlamacının **Satılan Paket Adedi (`total_sales_count`)**, **Getirdiği Toplam Ciro (`total_sales_volume`)** ve **Hak Ettiği Komisyon (`total_earned`)** otomatik artırılır.
  - Her satış için bağımsız `marketer_sales` tablosuna işlem kaydı (Salon adı, satılan paket, ödenen tutar, komisyon oranı ve tutarı) işlenir.
- **Abonelik Devamlılığı (Retention) & Yaşam Boyu Takip:**
  - `marketed_salons` tablosunda işletmelerin toplam harcaması (`total_spent`), paket satın alma adedi (`renewal_count`), son yenileme tarihi (`last_renewed_at`) kalıcı olarak saklanır. Kayıtlar abonelik süresi bitse bile silinmez; böylece portföy sadakat analizi yapılabilir.
  - Gece denetim servisi (`marketer_cleanup_service`), portföydeki salonların aktiflik durumunu periyodik olarak doğrular ve devamlılık oranını denetler.
- **Süper Admin Pazarlamacı Yönetimi (`/admin/pazarlamacilar`):**
  - **Yeni Pazarlamacı Hesabı Ekleme:** İsim, e-posta, telefon, başlangıç rütbesi ve komisyon oranı ile saniyeler içinde yeni temsilci tanımlanır; sisteme özel benzersiz referral kodu otomatik atanır.
  - **Rütbe & Seviye Yönetimi:** Pazarlamacıların rütbeleri admin panelinden anında değiştirilebilir (Örn: *Stajyer, Bronz Temsilci, Gümüş Temsilci, Altın Temsilci, Platin Lider, Direktör & İş Ortağı* veya özel rütbe unvanı).
  - **Komisyon Oranı Düzenleme:** Her pazarlamacının komisyon yüzdesi bağımsız olarak güncellenebilir.
  - **Müşteri Portföyü & Devamlılık Modalı (`[Salonlar]`):**
    - Her pazarlamacının getirdiği tüm işletmeler listelenir.
    - Canlı rozetler: `[Aktif - Devam Ediyor]` (Kalan gün sayısı ve bitiş tarihi) vs `[Süresi Bitti - Yenilemedi]`.
    - İşletmenin bugüne kadar kaç kez yenileme yaptığı, toplam cirosu ve son işlem tarihi şeffaf olarak izlenir.
    - Hızlı filtreleme: Tümü, Aktif Devam Edenler, Süresi Dolanlar.
  - **Detaylı Satış Makbuzları (`[Satışlar]`):** Hangi pazarlamacının hangi işletmelere hangi paketleri sattığı bağımsız fatura dökümü ile incelenebilir.
  - **Kod Yenileme & Durum:** İhtiyaç halinde tek tıkla yeni referans kodu üretilebilir veya hesap aktif/pasif yapılabilir.

---

## 20. 💾 Otomatik Veritabanı Yedekleme ve SQL Dump Dışa Aktarma

Sıfır veri kaybı garantisi:

- **Canlı SQL Dump:** MySQL `SHOW CREATE TABLE` ve `SELECT *` komutları ile tüm şema ve veriler eksiksiz `.sql` formatında paketlenir.
- **Süper Admin Tek Tıkla İndirme:** Admin panelinden dilediği an veritabanı yedeği ZIP dosyası olarak bilgisayara indirilebilir.
- **Otomatik Scheduler:** Her gece saat 03:00'te sistem otomatik yedek alır ve son 3 yedeği rotasyonla saklar.

---

## 21. 📝 Dinamik Sektör Alanları ve SEO Blog Yönetimi

Platformun her sektöre kusursuz adapte olmasını sağlayan altyapı:

- **Sektörel Dinamik Sorular:** Diş kliniği için "Protez Türü", kuaför için "Saç Uzunluğu", oto yıkama için "Araç Modeli" gibi özel alanlar formda otomatik açılır.
- **SEO Blog Sistemi:** `/blog` rotasında zengin içerikler, tahmini okuma süreleri, izlenme sayaçları ve kategori etiketleri ile organik Google trafiği hedeflenir.

---

## 22. 🛡️ Siber Güvenlik, OWASP Dayanıklılığı ve KVKK Uyumluluğu

RandevuDesk, bağımsız denetim standartlarında **94/100 (A+)** siber güvenlik dayanıklılığına sahiptir:

- **Merkezi Veri Şifreleme (Data-at-Rest - AES-256-GCM):** Olası bir veritabanı dump sızıntısında veya sunucu ihlalinde hassas verilerin korunması için sistemde tek ve standart bir algoritma (**AES-256-GCM AEAD**) çalışır. Müşteri formülasyon reçeteleri (`formula_content`), alerji uyarıları (`allergy_warning`), cilt analizleri (`hair_skin_analysis`), diş hekimliği medikal anamnez verileri (`other_allergies`, `systemic_diseases`, `regular_medications`, `notes`), randevu notları (`notes`) ve dinamik form yanıtları (`value`) veritabanında fiziksel olarak `enc::v1::...` formatında şifreli depolanır. Ham metin sızıntısı riski sıfırlanmıştır.
- **BOLA / IDOR:** Zorunlu `current_user.salon_id` eşitlemesi ile yetkisiz işletme verilerine erişim imkansızdır.
- **SQL Injection:** SQLAlchemy 2.0 ORM tamamen parametrik sorgular (Prepared Statements) çalıştırır.
- **Brute-Force Koruması:** 15 dakikada 5 başarısız denemede hesap kilitleme ve IP başına rate limiting.
- **XSS & Güvenlik Başlıkları:** `nosniff`, `SAMEORIGIN`, `strict-origin-when-cross-origin`.
- **KVKK / GDPR Maskeleme:** Loglarda telefonlar `0532 *** ** 89` ve e-postalar `a***e@alan.com` şeklinde sansürlenir.

---

## 23. 🎨 Tasarım İlkeleri: Sıfır İkon, Sıfır Emoji ve Rem Ölçü Standardı

Platformun görsel kimliği sıkı kurallarla korunur:

- **Sıfır İkon & Sıfır Emoji:** Arayüzde hiçbir SVG ikon, Lucide kütüphanesi veya unicode emoji kullanılmaz. Tüm durumlar profesyonel tipografik kısaltmalarla (`RDV`, `DRM`, `ZMN`, `YRM`, `BIL`) ve sade metinlerle ifade edilir.
- **Rem Ölçü Standardı:** Keyfi `%` ve `px` kullanılmaz; Tailwind rem ölçekleri ile %100 kusursuz mobil ve masaüstü uyumu sağlanır.
- **Renk Uyumu:** Açık mavi (`#1D4ED8`, `#EFF6FF`), beyaz ve slate tonları kurumsal bir SaaS deneyimi sunar.

---

## 24. 🧪 Test Güvencesi, Sağlık Denetimleri ve Canlı İzleme

- **92+ Pytest Birim ve Entegrasyon Testi:** Kimlik doğrulama, randevu yaşam döngüsü, PayTR webhook, WhatsApp servisi ve güvenlik kuralları otomatik testlerle doğrulanmaktadır.
- **Uptime Kuma & Nginx Health:** `/health` ve `/nginx-health` uç noktaları 7/24 canlı kontrol edilir.

---

*Belge Tarihi: 2026-10-02 | RandevuDesk B2B SaaS Kurumsal Teknik Dokümantasyonu*


---

## 25. 🗺️ Kapsamlı Sayfa Haritası: Hangi Sayfa Ne İşe Yarıyor ve Hangi Sayfada Hangi Özellikler Var?

Bu bölümde, `frontend/src/app/*` altında yer alan **70'in üzerindeki tüm sayfa rotası**, her sayfanın arkasında çalışan React/Next.js bileşenleri, bağlı olduğu FastAPI uç noktaları ve sayfa içindeki tüm özellikler tek tek analiz edilmiştir.

```text
+-----------------------------------------------------------------------------------+
|                        RANDEVUDESK SAYFA & ROTA MIMARISI                          |
+-----------------------------------------+-----------------------------------------+
                                          |
       +-----------------+----------------+----------------+-----------------+
       |                 |                |                |                 |
       v                 v                v                v                 v
+--------------+  +--------------+  +--------------+  +--------------+  +--------------+
| PUBLIC & B2C |  |  /dashboard  |  |    /admin    |  | /pazarlamaci |  | 10 INOVASYON |
| - Ana Sayfa  |  | - Mini Ofis  |  | - Salonlar   |  | - Ozet       |  | - Taksimetre |
| - Randevu Al |  | - Randevular |  | - Sektorler  |  | - Aday (CRM) |  | - Kiyamet    |
| - QR Menu    |  | - Kroki      |  | - Abonelik   |  | - Takipler   |  | - Veto Tusu  |
| - TV Kiosk   |  | - Dis & Boya |  | - Finans/Kod |  | - Web Satis  |  | - P2P Borsa  |
| - Sira Takip |  | - Stok/Bordro|  | - CRM/WebPkg |  | - Komisyon   |  | - Radar/UWB  |
+--------------+  +--------------+  +--------------+  +--------------+  +--------------+
```

---

### 25.1. Herkese Açık (Public) Vitrin, Pazarlama ve B2C Müşteri Sayfaları

| Sayfa Rotası (URL) | Kaynak Dosya & Bileşenler | Sayfa Ne İşe Yarar ve İçinde Hangi Özellikler Var? |
| :--- | :--- | :--- |
| **`/` (Ana Sayfa)** | `frontend/src/app/page.tsx`<br>`HeroTabbedSlider.tsx`<br>`RandevuDeskMockupCard.tsx`<br>`LiveMetricsTicker.tsx`<br>`PlatformFeaturesShowcase.tsx`<br>`YouTubeVideoFacade.tsx`<br>`HomeInteractiveFaq.tsx`<br>`FloatingDemoWidget.tsx` | **Platformun Kurumsal Karşılama ve Dönüşüm Merkezi:**<br>• **Hero Sekmeli Vitrin:** Sektörlere göre değişen başlık ve değer önerileri.<br>• **Açık Tema Ürün Arayüz Maketi:** Gerçek panel deneyimini sayfa içinde canlı gösteren interaktif kart.<br>• **Canlı Metrik Bandı:** Aktif salon, alınan randevu ve memnuniyet istatistikleri.<br>• **2 Dakikalık Tanıtım Videosu:** TBT performansını düşürmeyen tembel yüklemeli (`YouTubeVideoFacade`) video oynatıcı.<br>• **Canlı Demo Form Kartları:** Estetik, Diş Kliniği ve Saç Tasarım demolarına doğrudan geçiş.<br>• **JSON-LD SEO Şeması:** `Organization` ve `FAQPage` yapısal verileri. |
| **`/form/[salon_id]`** | `frontend/src/app/form/[salon_id]/page.tsx`<br>`AppointmentForm.tsx`<br>`SmartBookingWizard.tsx`<br>`ServiceMultiSelect.tsx`<br>`AppointmentDateTimePicker.tsx`<br>`DynamicFormFields.tsx`<br>`PriceSummary.tsx`<br>`NoShowProtectionCard.tsx` | **B2C Müşteri Online Randevu Alma Sayfası:**<br>• **Temiz Slug & ID Desteği:** `/form/salon-adi-1` veya `/form/1` ile çalışır.<br>• **Çoklu Hizmet Seçimi:** Süre ve toplam tutarı anlık toplayan hizmet seçici.<br>• **Personel Seçimi:** Seçilen hizmeti verebilen uzmanları listeler veya "Fark Etmez" havuzu sunar.<br>• **Canlı Müsaitlik Takvimi:** İzinli personelleri, kapalı günleri ve dolu saatleri düşerek sadece açık slotları gösterir (`GET /api/v1/salons/{id}/occupied-slots`).<br>• **Sektörel Dinamik Sorular:** Diş, kuaför, diyetisyen, spa vb. sektörlere özel ek form alanları.<br>• **Bekleme Listesi Kaydı:** Dolu saatlerde müşteriyi tek tıkla yedek sıraya (`Waitlist`) alma.<br>• **Cloudflare Turnstile & 8 Haneli Takip Kodu:** Bot korumalı kayıt ve işlem sonunda `EST-XXXX` takip kodu üretimi. |
| **`/embed/[salon_id]`** | `frontend/src/app/embed/[salon_id]/page.tsx`<br>`AppointmentForm.tsx` | **Web Sitesi iFrame Randevu Widget Sayfası:**<br>• İşletmelerin kendi WordPress, Wix veya özel web sitelerine `<iframe>` olarak gömdükleri, üst menü (`Navbar`) ve alt bilgiden (`Footer`) arındırılmış saf randevu alma ekranıdır. |
| **`/vitrin/[salon_id]`** | `frontend/src/app/vitrin/[salon_id]/page.tsx` | **İşletme Dijital Vitrin & Mini Web Sitesi:**<br>• İşletmenin logosu, açık adresi, Google Maps konumu, çalışma saatleri, aktif hizmet/fiyat listesi, müşteri değerlendirme yıldızları ve doğrudan "Randevu Al" aksiyonunu barındıran mini kurumsal web sayfasıdır. |
| **`/menu/[salon_id]`** | `frontend/src/app/menu/[salon_id]/page.tsx`<br>`QrMenuView.tsx` | **Dijital QR Hizmet & Fiyat Menüsü:**<br>• Müşteriler salondaki masa veya ayna standındaki QR kodu okuttuğunda açılır.<br>• Hizmetleri kategorilerine göre gruplar, süre ve güncel fiyatları gösterir, seçilen hizmetle doğrudan randevu formuna yönlendirir. |
| **`/kiosk/[salon_id]`** | `frontend/src/app/kiosk/[salon_id]/page.tsx`<br>`LiveKioskDisplay.tsx` | **Sokak Vitrini & Bekleme Salonu Canlı TV Ekranı:**<br>• Dükkan camına veya bekleme salonuna yerleştirilen TV/monitörler için tasarlanmıştır.<br>• Günün boş randevu saatlerini, içerideki yoğunluk durumunu ve sokaktan geçenlerin telefonla okutup 30 saniyede randevu alabileceği dev QR kodu canlı yayınlar. |
| **`/tracking` & `/takip`** | `frontend/src/app/tracking/page.tsx`<br>`frontend/src/app/takip/page.tsx`<br>`TrackingLookupClient.tsx`<br>`LiveQueueRadar.tsx` | **Canlı Randevu Sorgulama & Sıra Takip Radarı:**<br>• Müşterinin üye olmadan **8 haneli takip kodu** veya **telefon numarası** ile randevusunu sorguladığı sayfadır (`GET /api/v1/appointments/search`).<br>• **Özellikler:** Randevu durumu (Bekliyor / Onaylandı / Tamamlandı / İptal), kalan süre sayacı, Google Takvim'e ve Apple Takvim'e (`.ics`) ekleme butonları, KVKK uyumlu telefon/kod maskeleme. |
| **`/randevu-iptal/[code]`** | `frontend/src/app/randevu-iptal/[code]/page.tsx` | **Müşteri Tek Tıkla Randevu İptal Ekranı:**<br>• WhatsApp 2 saat / 24 saat hatırlatma mesajındaki güvenli bağlantıdan açılır.<br>• Gelemeyecek müşterinin şifre girmeden randevusunu iptal etmesini sağlar; koltuk boşaldığı anda bekleme listesindeki (Waitlist) sıradaki müşteriye otomatik bildirim tetiklenir. |
| **`/waitlist/accept/[token]`** | `frontend/src/app/waitlist/accept/[token]/page.tsx`<br>`WaitlistAcceptClient.tsx` | **Bekleme Listesi 15 Dakikalık Fırsat Onay Sayfası:**<br>• İptal edilen bir randevu için sıradaki müşteriye WhatsApp/SMS ile gönderilen tek kullanımlık token bağlantısıdır.<br>• Müşteri 15 dakika içinde "Randevuyu Onayla" butonuna basarsa randevu kesinleşir ve takvime işlenir. |
| **`/review/[code]` & `/form/[salon_id]/review/[code]`** | `frontend/src/app/review/[code]/page.tsx`<br>`frontend/src/app/form/[salon_id]/review/[code]/page.tsx` | **Müşteri Memnuniyet & Yıldız Değerlendirme Sayfası:**<br>• Tamamlanan randevu sonrası müşterinin 1-5 yıldız puan verdiği ve deneyim yorumu yazdığı sayfadır.<br>• İsimli veya anonim yorum seçeneği sunar; verilen puanlar doğrudan personelin **Memnuniyet Ligi** sıralamasına yansır. |
| **`/fiyatlar`, `/fiyatlandirma`, `/pricing`** | `frontend/src/app/fiyatlar/page.tsx`<br>`PricingPlansClient.tsx` | **Paketler ve Şeffaf Fiyatlandırma Sayfası:**<br>• **Temel Paket (5.000 Randevu)** ve **Profesyonel Paket (Sınırsız Randevu)** için 1 Aylık, 3 Aylık ve 1 Yıllık seçenekleri sunar.<br>• Fiyatları canlı olarak Süper Admin ayarlarından (`SiteSettings`) çeker.<br>• Referans/davet koduyla **14 + 7 = 21 Gün Ücretsiz Pro Deneme** bilgisini ve detaylı özellik karşılaştırma matrisini içerir. |
| **`/ozellikler`** | `frontend/src/app/ozellikler/page.tsx` | **Sistem Özellikleri Vitrin Sayfası:**<br>• RandevuDesk'in tüm çekirdek modüllerini, güvenlik mimarisini ve sektörel yeteneklerini kurumsal kartlarla tanıtan bilgi sayfası. |
| **`/sektorler`** | `frontend/src/app/sektorler/page.tsx`<br>`SectorsInteractiveGrid.tsx` | **Sektörler Keşif Sayfası:**<br>• Kuaför, Diş Kliniği, Güzellik Merkezi, Diyetisyen, Psikolog, Oto Yıkama dahil 17+ sektöre özel dinamik form alanlarını ve iş akışlarını interaktif ızgarada gösterir. |
| **`/cozumler` & `/cozumler/[slug]`** | `frontend/src/app/cozumler/page.tsx`<br>`frontend/src/app/cozumler/[slug]/page.tsx`<br>`UnifiedSolutionsShowcase.tsx` | **Sorun Odaklı Çözümler & Dinamik SEO Sayfaları:**<br>• İşletmelerin yaşadığı spesifik sorunlara (No-show önleme, gece randevu toplama, personel prim hesabı, koltuk çakışması) yönelik özel çözüm sayfaları ve sektörel SEO açılış sayfaları (`landingPages.ts`, `solutionsData.ts`). |
| **`/demo` & `/canli-demo`** | `frontend/src/app/demo/page.tsx`<br>`frontend/src/app/canli-demo/page.tsx`<br>`SectorDemoHub.tsx` | **Canlı Sektörel Demo Merkezi:**<br>• Ziyaretçilerin üye olmadan farklı sektörlerin müşteri randevu formlarını ve işletme yönetim ekranlarını canlı simülasyonla test ettiği merkez. |
| **`/vaka-analizleri` & `/vaka-analizleri/[slug]`** | `frontend/src/app/vaka-analizleri/page.tsx`<br>`frontend/src/app/vaka-analizleri/[slug]/page.tsx` | **Müşteri Başarı Hikayeleri (Case Studies):**<br>• RandevuDesk'e geçen işletmelerin önceki/sonraki ciro artışlarını, telefon yükü azalmasını ve no-show düşüş oranlarını somut verilerle sunan vaka inceleme sayfaları (`caseStudies.ts`). |
| **`/araclar` & `/araclar/kacirilan-randevu-maliyeti`** | `frontend/src/app/araclar/page.tsx`<br>`frontend/src/app/araclar/kacirilan-randevu-maliyeti/page.tsx`<br>`CostCalculatorClient.tsx` | **Ücretsiz İşletme Araçları & Kayıp Ciro Hesaplayıcı:**<br>• İşletme sahibinin günlük işlem adedi, ortalama işlem ücreti ve haftalık gelmeyen müşteri sayısını kaydırıcılarla girerek aylık/yıllık kaybettiği parayı ve RandevuDesk ile geri kazanacağı net ciroyu hesapladığı interaktif araç. |
| **`/blog` & `/blog/[slug]`** | `frontend/src/app/blog/page.tsx`<br>`frontend/src/app/blog/[slug]/page.tsx` | **SEO Blog & İçerik Merkezi:**<br>• Kategori filtreleme, tahmini okuma süresi, görüntülenme sayacı (`view_count`) ve `DOMPurify` ile temizlenmiş zengin HTML makale okuma sayfası. |
| **`/hakkimizda` & `/iletisim`** | `frontend/src/app/hakkimizda/page.tsx`<br>`frontend/src/app/iletisim/page.tsx`<br>`ContactFormClient.tsx` | **Kurumsal Künye ve Güvenli İletişim Sayfası:**<br>• Resmi şirket bilgileri (Vergi dairesi/no, Beyoğlu/İstanbul merkez, destek telefonları).<br>• İletişim formu mesajlarını rate-limit korumalı ve veritabanında AES-256-GCM şifreli olarak kaydeder (`POST /api/v1/contact`). |
| **`/login`** | `frontend/src/app/login/page.tsx` | **Merkezi Kullanıcı Giriş Ekranı:**<br>• Brute-force korumalı, Cloudflare Turnstile destekli giriş.<br>• Kullanıcının rolüne göre otomatik yönlendirme yapar: `admin` ➔ `/admin`, `marketer` ➔ `/pazarlamaci`, `salon_owner` / `salon_staff` ➔ `/dashboard`. |
| **`/register` & `/kayit`** | `frontend/src/app/register/page.tsx`<br>`frontend/src/app/kayit/page.tsx` | **Yeni İşletme Kayıt Sihirbazı:**<br>• İşletme adı, yetkili adı, telefon, e-posta, şifre ve sektör seçimiyle saniyeler içinde yeni salon ve yönetici hesabı açar.<br>• **Özellikler:** Listede olmayan sektörler için "Yeni Sektör Talebi" açabilme, pazarlamacı/referans kodu (`?ref=`) veya aktivasyon kodu doğrulamasıyla **14 + 7 = 21 gün ücretsiz Profesyonel Paket** tanımlama. |
| **`/odeme/basarili` & `/odeme/basarisiz`** | `frontend/src/app/odeme/basarili/page.tsx`<br>`frontend/src/app/odeme/basarisiz/page.tsx` | **PayTR Ödeme Sonuç Ekranları:**<br>• PayTR 3D Secure işlemi tamamlandığında başarılı ödeme makbuzunu gösteren veya başarısız işlemde tekrar deneme imkanı sunan sayfalar. |

---

### 25.2. İşletme Yönetim Paneli (`/dashboard` — Mini Ofis) Sayfaları ve Sekmeleri

İşletme paneli `DashboardLayout` (`UnifiedRouteGuard` + `MiniOfficeShell` + `DashboardProvider` + `DashboardAuthGuard`) tarafından sarmalanır. URL'deki `?salon=salon-adi-1` parametresiyle çoklu şube/admin geçişini destekler ve IDOR korumasını uygular.

| Sayfa / Sekme Rotası | Kaynak Dosya & Bileşenler | Erişim Yetkisi | Sayfa Ne İşe Yarar ve İçinde Hangi Özellikler Var? |
| :--- | :--- | :--- | :--- |
| **`/dashboard` & `/dashboard/[salon_slug]`** (`?tab=financial`) | `frontend/src/app/dashboard/page.tsx`<br>`DashboardView.tsx`<br>`MiniOfficeDashboard.tsx`<br>`ExecutiveSummarySection.tsx`<br>`TodayAppointmentsDragList.tsx`<br>`TradingMetricsChart.tsx` | Tüm Roller *(Personelde ciro gizli)* | **Genel Bakış & Mini Ofis Kokpiti:**<br>• **4 KPI Kartı:** Bugünün randevuları, onay bekleyenler, onaylı/tamamlanan ciro (sadece yönetici) ve aktif randevu havuzu.<br>• **Sürükle-Bırak Günün Randevuları:** Randevu kartlarını sürükleyerek durum veya saat değiştirme.<br>• **Borsa Tipi Ciro & Randevu Grafiği:** Günlük/haftalık ciro ve randevu trendini görselleştirir.<br>• **Hızlı İşlem Modalı (`MiniOfficeQuickActionModal`):** Panelden çıkmadan hızlı randevu oluşturma, bildirim çekmecesi (`NotificationDrawer`) ve hızlı QR modalı (`QrCodeModal`). |
| **`/dashboard?tab=calendar`** | `ClinicAvailabilityCalendar.tsx`<br>`StaffScheduleCalendarView.tsx` | Tüm Roller | **Klinik & Salon Müsaitlik Takvimi:**<br>• Personel sütunları ve saat satırları bazında tüm günün/haftanın doluluk matrisini gösterir, boş slota tıklayarak hızlı randevu ataması yapar. |
| **`/dashboard/appointments`** | `frontend/src/app/dashboard/appointments/page.tsx`<br>`AppointmentsManagementView.tsx`<br>`AppointmentTable.tsx`<br>`BulkWhatsAppReminderModal.tsx`<br>`WhatsAppActionTrigger.tsx` | Tüm Roller | **Gelişmiş Randevu Yönetim Merkezi:**<br>• **Durum Yönetimi:** Bekleyen randevuyu Onayla, Tamamla veya İptal Et.<br>• **Çoklu Filtre & Arama:** Durum, zaman aralığı (Bugün, Bu Hafta, Gelecek, Geçmiş), müşteri adı/telefonu/takip kodu araması ve tarih/fiyat sıralaması.<br>• **Toplu WhatsApp Hatırlatma:** Yarınki veya bugünkü randevulara tek merkezden WhatsApp mesajı tetikleme.<br>• **Sektörel Hasta/Müşteri Kartı:** Diş kliniklerinde "Hasta Kartı" (`DentalPatientModal`), kuaförlerde "Reçete Kartı" (`CustomerFormulationsModal`) açma.<br>• **Canlı Polling:** 30 saniyede bir tabloyu sessizce güncelleyerek yeni düşen randevuları yakalar. |
| **`/dashboard?tab=spaces`** | `SpaceManagementView.tsx` | Tüm Roller *(Pro Paket)* | **2D Mekan, Koltuk & Ünit Kroki Yönetimi:**<br>• Salonun fiziksel koltuklarını, odalarını veya diş ünitelerini 2D ızgara üzerinde konumlandırma.<br>• Hangi koltuğun dolu (Kırmızı - aktif müşteri adı) veya müsait (Yeşil) olduğunu canlı izleme ve koltuğa randevu bağlama. |
| **`/dashboard?tab=waitlist`** | `WaitlistManagementView.tsx` | Tüm Roller *(Pro Paket)* | **Akıllı Bekleme Listesi (Waitlist) Yönetimi:**<br>• Dolu saatler için sıraya giren müşterileri listeleme.<br>• Müşteriye manuel 15 dakikalık fırsat teklifi gönderme veya kaydı tek tıkla kesin randevuya dönüştürme. |
| **`/dashboard/dental/laboratuvar`** (`?tab=dental_lab`) | `frontend/src/app/dashboard/dental/laboratuvar/page.tsx`<br>`DentalLabManagementView.tsx` | Sadece Diş Kliniği (`isSalonDental`) | **Diş Kliniği: Protez & Dış Laboratuvar Takibi:**<br>• Zirkonyum, porselen, E-max lamina, protez ve gece plağı iş emirlerini oluşturma.<br>• Ölçü, laboratuvara gönderim, prova ve teslim tarihlerini aşama aşama (`Ölçü Alındı` ➔ `Laboratuvarda` ➔ `Prova` ➔ `Teslim`) ve laboratuvar maliyetiyle takip etme. |
| **`/dashboard/dental/implants`** (`?tab=dental_implants`) | `frontend/src/app/dashboard/dental/implants/page.tsx`<br>`DentalImplantsView.tsx` | Sadece Diş Kliniği (`isSalonDental`) | **Diş Kliniği: Cerrahi İmplant & ÜTS Lot Takibi:**<br>• Hastaya uygulanan implantın markası, çapı (mm), boyu (mm), diş kök numarası ve Sağlık Bakanlığı **Üretici Lot Numarası** kaydı.<br>• Kemik grefti ve membran gibi cerrahi notları AES-256-GCM şifreli saklama, hasta telefonu veya lot numarasıyla sorgulama. |
| **`/dashboard/dental/hakedis`** (`?tab=dental_payroll`) | `frontend/src/app/dashboard/dental/hakedis/page.tsx`<br>`DentalPayrollView.tsx` | Salon Sahibi & Admin *(Diş Kliniği)* | **Diş Kliniği: Hekim Hak Ediş & Cerrahi Prim Bordrosu:**<br>• Klinikteki diş hekimlerinin standart tedavi primi, cerrahi/implant işlem primi ve sabit maaşlarını birleştirerek aylık net hekim hak ediş raporunu üretir. |
| **`/dashboard?tab=formulations`** | `HairFormulationsView.tsx`<br>`CustomerFormulationsModal.tsx` | Sadece Kuaför (`isSalonHair`) | **Kuaför: Boya & Teknik Formülasyon Reçeteleri:**<br>• Müşteri telefonuna bağlı teknik arşiv: Dip boya kodu, gramaj, oksidan volümü, bekleme süresi, cila reçetesi ve saç derisi alerji uyarıları.<br>• MinIO üzerinden işlem öncesi/sonrası fotoğraf yükleme. |
| **`/dashboard/services`** | `frontend/src/app/dashboard/services/page.tsx`<br>`ServiceForm.tsx` | Salon Sahibi & Admin | **Hizmet & Tedavi Fiyat Listesi Yönetimi:**<br>• Yeni hizmet ekleme, fiyat (TL), süre (dakika) ve kategori güncelleme.<br>• **Personel Yetkinlik Eşleştirme:** Hangi hizmeti hangi personellerin verebileceğini seçme. |
| **`/dashboard?tab=inventory`** | `InventoryManagementView.tsx` | Tüm Roller | **Stok, Sarf Malzeme & Perakende POS Satış:**<br>• Barkod/EAN ve SKU destekli ürün kartları, alış/satış fiyatı ve kâr marjı.<br>• **Kasadan Satış:** Nakit/Kart/Havale ile ürün satışı yapma, satışı yapan personele otomatik ürün primi yazma, koltuğa/odaya ürün sevk etme.<br>• **Stok Hareketleri:** Yeniden stok girişi (`restock`), sayım düzeltme (`adjustment`) ve kırık/bozuk ürün için zayi (`waste`) kaydı; kritik stok (`min_stock_alert`) uyarıları. |
| **`/dashboard?tab=payroll`** | `StaffPayrollView.tsx` | Salon Sahibi & Admin | **Personel Prim, Hak Ediş & Maaş Bordrolama:**<br>• `Taban Maaş + (Hizmet Cirosu x %Hizmet) + (Ürün Cirosu x %Ürün) + (Cerrahi Ciro x %Cerrahi)` formülüyle aylık bordro hesaplama ve randevu bazlı komisyon dökümü. |
| **`/dashboard?tab=competition`** | `StaffCompetitionChart.tsx` | Tüm Roller *(Pro Paket)* | **Personel Memnuniyet Ligi & Rekabet Podyumu:**<br>• Personelleri tamamladıkları randevu sayısı ve müşterilerden aldıkları ortalama yıldız puanına göre 1., 2., 3. podyumunda sıralar. |
| **`/dashboard?tab=analytics`** | `AnalyticsDashboardView.tsx`<br>`FinancialAnalyticsView.tsx` | Tüm Roller *(Pro Paket)* | **Finansal Analitik, RFM Segmentasyonu & Ciro Hedefleri:**<br>• **Aylık Ciro & Randevu Hedef Çubuğu:** Belirlenen hedefe ulaşma yüzdesi.<br>• **RFM Müşteri Segmentasyonu:** Müşterileri otomatik olarak *VIP*, *Sadık*, *Kayıp Riski (60+ gün gelmeyen)* ve *Yeni* gruplarına ayırır.<br>• **Yoğun Saatler Isı Haritası & Gelecek Ay Ciro Tahmini (Forecast).** |
| **`/dashboard/subscription`** | `frontend/src/app/dashboard/subscription/page.tsx`<br>`SubscriptionManagementClient.tsx`<br>`RenewSubscriptionModal.tsx` | Salon Sahibi & Admin | **Paket & Abonelik Yönetimi:**<br>• Mevcut paket (`Ücretsiz`, `Temel`, `Profesyonel`), kalan gün sayacı ve yıllık randevu kotası (`Kullanılan / Kota`).<br>• **Kümülatif Süre Uzatma:** PayTR 256-Bit SSL iFrame ile mevcut sürenin üzerine 30, 90 veya 365 gün ekleme.<br>• Pazarlamacı referans kodu ve hediye aktivasyon kodu kullanma, dönem sonu iptal/paket düşürme yönetimi. |
| **`/dashboard/notifications`** | `frontend/src/app/dashboard/notifications/page.tsx`<br>`NotificationDrawer.tsx` | Tüm Roller | **Bildirim Merkezi:**<br>• Yeni randevu (`RDV`), durum değişimi (`DRM`), saat güncellemesi (`ZMN`), müşteri yorumu (`YRM`) ve sistem (`BIL`) bildirimlerini filtreleme, tümünü okundu işaretleme ve temizleme. |
| **`/dashboard/qr-menu`** | `frontend/src/app/dashboard/qr-menu/page.tsx`<br>`QrMenuStudioView.tsx` | Tüm Roller | **QR Menü & Canlı Masa Standı Tasarım Stüdyosu:**<br>• A4 Masa Kartlığı, A5 Dikey Çerçeve ve Kare Masa Kartı şablonları.<br>• Kurumsal renk ve başlık özelleştirme, canlı telefon önizlemesi, yüksek çözünürlüklü SVG/PNG indirme ve doğrudan yazdırma (`window.print`). |
| **`/dashboard/integration`** | `frontend/src/app/dashboard/integration/page.tsx`<br>`IntegrationSnippet.tsx` | Tüm Roller | **Web Sitesi & Instagram Bio Entegrasyon Merkezi:**<br>• İşletmenin kendi web sitesine yapıştırabileceği tek satırlık iFrame kodu, yüzen (floating) randevu butonu kodu ve Instagram biyografi temiz linkini tek tıkla kopyalama. |
| **`/dashboard/settings`** | `frontend/src/app/dashboard/settings/page.tsx`<br>`SalonSettingsClient.tsx`<br>`StaffAvailabilityEditor.tsx` | Salon Sahibi & Admin | **Mesai, Kapalı Günler, Personel & İşletme Ayarları (5 Sekme):**<br>1. *Çalışma Saatleri:* Haftalık açık günler, açılış/kapanış ve öğle molası saatleri, slot süresi (15-60 dk).<br>2. *Kapalı Günler:* Resmi tatil ve tadilat günlerini takvimde kapatma.<br>3. *Personel Yönetimi:* Yeni personel/yönetici ekleme, şifre güncelleme, soft-delete/restore ve personele özel izin/mesai takvimi.<br>4. *Genel Bilgiler:* Salon adı, telefon, adres, temiz URL slug ve **MinIO kurumsal logo yükleme**.<br>5. *Bildirim Ayarları:* WhatsApp 24s / 2s hatırlatıcı tercihleri. |
| **`/dashboard/kiosk`** | `frontend/src/app/dashboard/kiosk/page.tsx`<br>`KioskControlView.tsx` | Tüm Roller | **Sokak Vitrini TV Kiosk Kontrol Paneli:**<br>• `/kiosk/[salon_id]` ekranında yayınlanacak mesajları, görünümü ve canlı vitrin bağlantısını yönetme. |
| **`/dashboard/loyalty`** | `frontend/src/app/dashboard/loyalty/page.tsx`<br>`LoyaltySimulatorStudio.tsx`<br>`VipCelebrationModal.tsx` | Tüm Roller | **VIP Müşteri Karşılama & Sadakat Simülatörü:**<br>• Salona gelen sadık/VIP müşteriler için karşılama tableti ekranı ve özel ikram/kutlama kurgusu. |
| **`/dashboard/smart-mirrors`** | `frontend/src/app/dashboard/smart-mirrors/page.tsx`<br>`SmartMirrorStudio.tsx` | Tüm Roller | **Dokun-Geç Akıllı Ayna (NFC & QR) Stüdyosu:**<br>• Kuaför aynalarına ve koltuklara yapıştırılan NFC/QR etiketlerinin koltuk numarasına göre üretimi ve baskı şablonu. |
| **`/dashboard/story-studio`** | `frontend/src/app/dashboard/story-studio/page.tsx`<br>`InstagramStoryStudio.tsx` | Tüm Roller | **Instagram Story Tasarım Stüdyosu:**<br>• Yapılan işlemin fotoğrafını 9:16 dikey formatta, salon logosu ve randevu QR koduyla birleştirerek sosyal medyada paylaşıma hazır görsel olarak indirme. |

---

### 25.3. Süper Admin Platform Yönetim Paneli (`/admin/*`) Sayfaları

Süper Admin paneli `frontend/src/app/admin/layout.tsx` ve `AdminSidebar.tsx` ile korunur; yalnızca `admin` / `superadmin` rolüne sahip platform yöneticileri erişebilir.

| Sayfa Rotası (URL) | Kaynak Dosya | Sayfa Ne İşe Yarar ve İçinde Hangi Özellikler Var? |
| :--- | :--- | :--- |
| **`/admin`** | `frontend/src/app/admin/page.tsx` | **Platform Komuta Merkezi & İşletmeler Listesi:**<br>• Kayıtlı tüm işletmeleri sektörlerine göre filtreleyerek listeler.<br>• Her işletmenin kullanıcı sayısı, randevu sayısı, hizmet sayısı ve abonelik bitiş tarihini gösterir.<br>• **Özellikler:** Yeni işletme açma, tek tıkla işletmenin `/dashboard` paneline yönetici olarak girme, WhatsApp Cloud API servisini tek tıkla açıp kapatma ve **Canlı SQL Veritabanı Yedeği (ZIP)** indirme. |
| **`/admin/salons/[id]`** | `frontend/src/app/admin/salons/[id]/page.tsx` | **İşletme Detay & Kullanıcı Yönetimi:**<br>• İşletmenin tüm iletişim, sektör ve slug bilgilerini düzenleme.<br>• İşletmeye yeni yönetici/personel hesabı ekleme, mevcut personelin şifresini sıfırlama, silinen kullanıcıyı geri getirme.<br>• İşletmeyi dondurma/aktif etme, manuel gün ekleme ve salonun son 50 denetim logunu (`ActivityLog`) inceleme. |
| **`/admin/sektorler`** | `frontend/src/app/admin/sektorler/page.tsx` | **Kayıt Sektörleri & Dinamik Form Alanları Yönetimi:**<br>• Platforma yeni sektör ekleme, sıralamasını değiştirme (`reorder`).<br>• Her sektör için randevu formunda müşteriye sorulacak dinamik soruları (`text`, `number`, `select`, `date`, `textarea`) tanımlama.<br>• Kayıt olan işletmelerin gönderdiği "Yeni Sektör Taleplerini" (`SectorRequest`) onaylama veya reddetme. |
| **`/admin/abonelikler`** | `frontend/src/app/admin/abonelikler/page.tsx` | **Abonelik & Süre Uzatma Yönetimi:**<br>• Tüm salonların abonelik durumlarını (`Aktif`, `Süresi Dolmak Üzere`, `Süresi Dolmuş`, `Ücretsiz`) izleme.<br>• Tek tıkla +7 gün, +30 gün, +90 gün, +365 gün ekleme veya paket seviyesini (`free`, `basic`, `pro`) ve yıllık randevu kotasını güncelleme. |
| **`/admin/paket-fiyatlari`** | `frontend/src/app/admin/paket-fiyatlari/page.tsx` | **Merkezi Paket Fiyatları Yönetimi:**<br>• Temel Paket (1 Ay / 3 Ay / 1 Yıl) ve Profesyonel Paket (1 Ay / 3 Ay / 1 Yıl) fiyatlarını TL cinsinden canlı güncelleme.<br>• Kaydedildiği anda hem `/fiyatlar` sayfası hem de PayTR ödeme motoru yeni fiyatları kullanır. |
| **`/admin/finans`** | `frontend/src/app/admin/finans/page.tsx` | **Platform Finans, Ciro & KDV Raporu:**<br>• PayTR üzerinden tahsil edilen tüm abonelik ödemelerini, brüt ciroyu, KDV dökümünü, pazarlamacı komisyon paylarını ve net platform gelirini raporlar. |
| **`/admin/kodlar`** | `frontend/src/app/admin/kodlar/page.tsx` | **Aktivasyon & İndirim Kodları Yönetimi:**<br>• Tekil özel kampanya kodu (Örn: `YAZ2026`) veya tek seferde 50+ adet toplu (batch) aktivasyon kodu üretme.<br>• Kod başına tanımlanacak gün sayısı, indirim yüzdesi, maksimum kullanım limiti, son geçerlilik tarihi ve hangi salonların kullandığını (`usages`) izleme. |
| **`/admin/pazarlamacilar`** | `frontend/src/app/admin/pazarlamacilar/page.tsx` | **Pazarlamacı (Affiliate) & Devamlılık (Retention) Yönetimi:**<br>• Yeni saha satış temsilcisi hesabı açma, benzersiz referans kodu üretme.<br>• **Kademeli Komisyon Ayarları:** İlk satış komisyonu (varsayılan %40), yenileme komisyonu (varsayılan %10), komisyon süresi (12 ay) ve rütbe (`Junior`, `Bronz`, `Gümüş`, `Altın`, `Platin Lider`) tanımlama.<br>• **`[Salonlar]` & `[Satışlar]` Modalları:** Temsilcinin getirdiği işletmelerin aboneliğe devam edip etmediğini (`Aktif` vs `Süresi Doldu`), kaç kez yenilediğini ve satış makbuzlarını inceleme. |
| **`/admin/pazarlamaci-adaylari`** | `frontend/src/app/admin/pazarlamaci-adaylari/page.tsx`<br>`LeadFunnelVisualizer.tsx` | **Saha Satış CRM — Tüm Müşteri Adayları ve Arama Denetimi:**<br>• Tüm pazarlamacıların sahada aradığı işletmeleri (`marketer_leads`), kiminle görüşüldüğünü, arama sonuçlarını ve görüşme notlarını tek ekranda denetleme.<br>• Pazarlamacı bazlı arama sayısı, dönüşüm oranı, geciken takip istatistikleri, dönüşüm hunisi (`LeadFunnelVisualizer`) ve **Excel/CSV toplu aday içe/dışa aktarma**. |
| **`/admin/web-paketleri`** | `frontend/src/app/admin/web-paketleri/page.tsx` | **Web Sitesi Paketleri Kataloğu & Satış Onay Kuyruğu:**<br>• Pazarlamacıların işletmelere satabileceği "Kurumsal Web Sitesi Paketleri" kataloğunu (`WebPackage`) oluşturma ve fiyatlandırma.<br>• Pazarlamacıların bildirdiği web paketi satışlarını (`WebPackageSale`) inceleme; **Onayla** (`approved` ➔ komisyonu pazarlamacıya atomik olarak yazar), **Yapım Aşamasında** (`in_progress`), **Teslim Edildi** (`delivered`) veya **Reddet / İptal Et** (komisyonu geri alır) işlemlerini yürütme. |
| **`/admin/blog`** | `frontend/src/app/admin/blog/page.tsx`<br>`RichBlogEditor.tsx` | **SEO Blog & Kategori Yönetimi:**<br>• Yeni blog makalesi oluşturma, düzenleme, yayına alma/taslağa çekme ve blog kategorilerini yönetme. |
| **`/admin/guvenlik`** | `frontend/src/app/admin/guvenlik/page.tsx` | **Siber Güvenlik & Kilitli Hesap/IP Yönetimi:**<br>• Redis üzerinde brute-force koruması nedeniyle kilitlenen e-posta ve IP adreslerini listeleme, tek tıkla kilidi açma (`unlock`). |
| **`/admin/logs`** | `frontend/src/app/admin/logs/page.tsx` | **Sistem Denetim (Audit) Logları:**<br>• Platform genelinde gerçekleşen tüm kritik işlemlerin (`salon_created`, `subscription_updated`, `web_sale_approved` vb.) zaman damgalı denetim izlerini listeleme. |
| **`/admin/site-ayarlari`** | `frontend/src/app/admin/site-ayarlari/page.tsx` | **Dinamik Site & Kurumsal Künye Ayarları:**<br>• Vergi dairesi, vergi numarası, destek telefonları, e-posta, açık adres, çalışma saatleri, sosyal medya linkleri ve ana sayfa hero metinlerini hem MySQL (`site_settings`) hem de `site_settings.json` yedekli olarak güncelleme. |
| **`/admin/mesajlar`** | `frontend/src/app/admin/mesajlar/page.tsx` | **İletişim Formu Mesajları Yönetimi:**<br>• `/iletisim` sayfasından gönderilen şifreli mesajları okuma, durumunu (`okunmadı`, `okundu`, `yanıtlandı`, `arşivlendi`) güncelleme ve yönetici gizli notu (`admin_notes`) ekleme. |

---

### 25.4. Pazarlamacı (Affiliate & Saha Satış CRM) Portalı (`/pazarlamaci`)

- **Dosya Konumu:** `frontend/src/app/pazarlamaci/page.tsx`, `MarketerTabs.tsx`, `LeadsTab.tsx`, `FollowUpsTab.tsx`, `WebSalesTab.tsx`, `LeadFormModal.tsx`, `LeadDrawer.tsx`, `ActivityForm.tsx`, `LeadImportModal.tsx`.
- **Erişim Yetkisi:** Yalnızca `marketer` rolüne sahip saha satış temsilcileri ve `admin`.
- **Sayfa İçindeki 4 Ana Sekme ve Özellikleri:**
  1. **Özet (`overview`):**
     - Temsilcinin benzersiz referans kodu, tek tıkla kopyalanabilir Kayıt Davet Linki (`/register?ref=...`) ve Fiyatlar Linki (`/fiyatlar?ref=...`).
     - Toplam kazanç (`total_earned`), satılan paket adedi, getirilen salonların aktiflik ve yenileme (retention) tablosu, komisyon makbuzları ve profil/şifre düzenleme modalı.
  2. **Müşteri Adaylarım (`leads`):**
     - Görüşülen potansiyel işletmeleri (İşletme adı, yetkili kişi ve unvanı, telefon, şehir/ilçe, sektör, Instagram/web adresi, öncelik, durum) kaydetme.
     - **Telefon Mükerrerlik Koruması:** Aynı numara tekrar girildiğinde `409 Conflict` ile uyarıp mevcut aday kartını açar.
     - **Aday Çekmecesi (`LeadDrawer`) & Arama Günlüğü (`ActivityForm`):** Yapılan her telefon aramasını, WhatsApp mesajını veya yüz yüze görüşmeyi sonucuyla (`Ulaşıldı`, `Cevap Yok`, `Meşgul`, `Tekrar Aranacak`, `Teklif Kabul Edildi`) ve süresiyle zaman çizelgesine işler; arama sayacını (`call_count`) otomatik artırır.
     - **Excel / CSV Toplu İçe ve Dışa Aktarma (`LeadImportModal`):** Örnek şablon indirme, toplu aday yükleme ve mevcut listeyi Excel/CSV olarak dışa aktarma.
  3. **Bugünün Takipleri (`follow_ups`):**
     - `next_follow_up_at` tarihi bugün olan (`today`) veya tarihi geçmiş (`overdue` - kırmızı uyarı rozetli) müşteri adaylarını öncelik sırasıyla listeler; tek tıkla telefon arama (`tel:`) veya WhatsApp (`wa.me`) başlatır.
  4. **Web Paketi Satışlarım (`web_sales`):**
     - RandevuDesk aboneliğinden bağımsız olarak işletmelere kurumsal web sitesi paketi satışı bildirme (`POST /api/v1/marketer/web-sales`).
     - **Kademeli Web Komisyonu:** Standart **%10** komisyon; temsilcinin portföyünde 50+ aktif RandevuDesk üyesi olduğunda otomatik olarak **%15 bonus komisyon** kademesi açılır.

---

### 25.5. 10 Devrimsel İnovasyon & Vizyon Modülü Sayfaları

| Sayfa Rotası (URL) | Frontend Dosyası | Backend Router & Model | Sayfa Ne İşe Yarar ve İçinde Hangi Özellikler Var? |
| :--- | :--- | :--- | :--- |
| **`/vizyon`** | `frontend/src/app/vizyon/page.tsx`<br>`VisionModulesClient.tsx` | Tüm İnovasyon Router'ları | **10 İnovasyon Modülü Merkezi Vitrini:** 10 algoritmanın tamamını tek sayfada interaktif kartlarla tanıtan ve ilgili canlı modüle yönlendiren merkez. |
| **`/taksimetre`** | `frontend/src/app/taksimetre/page.tsx` | `routers/taximeter.py`<br>`models/taximeter.py` | **Tersine Taksimetre (SLA Garanti Motoru):** Müşteri check-in yaptığında geri sayımı başlatır; işletme belirlenen gecikme eşiğini aşarsa gecikilen her 5 dakikada müşteriye otomatik kahve kuponu veya indirim kodu üretir. |
| **`/kiyamet-modu`** | `frontend/src/app/kiyamet-modu/page.tsx` | `routers/offline_sync.py`<br>`models/offline_sync.py`<br>`lib/offlineSync.ts` | **Kıyamet Modu (Offline-First PWA & Mesh Sync):** İnternet kopsa bile randevu ve kasa işlemlerini tarayıcının yerel `IndexedDB` kuyruğunda saklar; bağlantı geldiğinde çakışmasız olarak sunucuya eşitler. |
| **`/veto`** | `frontend/src/app/veto/page.tsx` | `routers/veto.py`<br>`models/veto.py` | **"Et-Kemik" Kırmızı Veto Tuşu:** Dolu takvimde acil VIP müşteri veya tıbbi aciliyet olduğunda yöneticinin 10 saniye geçerli kriptografik bypass token'ı üreterek kural delmesini sağlar. |
| **`/ifsa-ekrani`** | `frontend/src/app/ifsa-ekrani/page.tsx` | `routers/transparency.py`<br>`models/transparency.py` | **Çift Taraflı İfşa Ekranı:** Resepsiyon/memur ekranındaki işlem durumunu ve hızını bekleme salonundaki ikinci ekrana canlı yansıtarak şeffaflık sağlar. |
| **`/randevu-borsasi`** | `frontend/src/app/randevu-borsasi/page.tsx` | `routers/marketplace.py`<br>`models/marketplace.py` | **Serbest Piyasa P2P Randevu Borsası:** Randevusuna yetişemeyecek müşterinin saatini iptal etmek yerine borsada açık artırma/devir listesine koymasını ve başka bir müşterinin saniyeler içinde devralmasını sağlar. |
| **`/ozgurluk-radari`** | `frontend/src/app/ozgurluk-radari/page.tsx` | `routers/geofence_radar.py`<br>`models/geofence_radar.py` | **Anti-Rehin 400m Özgürlük Radarı:** Müşterinin bekleme salonunda oturmak yerine 400m çapındaki kafelerde vakit geçirmesini sağlar; sırasına 5 dakika kala otomatik geri dönüş sinyali gönderir. |
| **`/golge-planlama`** | `frontend/src/app/golge-planlama/page.tsx` | `routers/shadow_schedule.py`<br>`models/shadow_schedule.py` | **Manipülatif Yapay Zekâ / Gölge Planlama:** Sürekli geciken doktor veya uzmanın ortalama sarkma süresini hesaplar ve uzman ekranında randevu saatini gizlice birkaç dakika erken göstererek sıfır bekleme sağlar. |
| **`/pit-stop`** | `frontend/src/app/pit-stop/page.tsx` | `routers/pitstop.py`<br>`models/pitstop.py` | **Giyotinli Pit-Stop Gişesi:** Hızlı işlemler ve koltuk sterilizasyonu için 45 saniyelik Formula 1 tipi geri sayım sayacı ve performans istatistikleri sunar. |
| **`/hayalet-checkin`** | `frontend/src/app/hayalet-checkin/page.tsx` | `routers/ghost_checkin.py`<br>`models/ghost_checkin.py` | **UWB Hayalet Check-In:** Kapıdan giren müşteriyi UWB/Bluetooth Beacon sinyaliyle resepsiyona uğramadan tanır ve doğrudan koltuğuna/odasına yönlendirir. |
| **`/gecikme-panosu`** | `frontend/src/app/gecikme-panosu/page.tsx` | `routers/delay_board.py`<br>`models/delay_board.py` | **Utanç Kronometresi / Canlı Gecikme Panosu:** Bekleme salonu TV ekranında tüm odaların ve uzmanların zamanında (Yeşil) veya gecikmeli (Kırmızı `+X dk`) durumunu canlı yayınlar. |

---

### 25.6. Hukuki / KVKK Sayfaları ve Dahili API Proxy Rotaları

- **Resmi Sözleşme ve KVKK Sayfaları:**
  - `/kvkk` (`frontend/src/app/kvkk/page.tsx`): KVKK Aydınlatma Metni.
  - `/acik-riza` & `/acik-riza-metni`: Açık Rıza Beyanı.
  - `/gizlilik-politikasi`: Gizlilik ve Veri Güvenliği Politikası.
  - `/cerez-politikasi`: Çerez (Cookie) Politikası.
  - `/abonelik-sozlesmesi` & `/sozlesme` & `/sartname`: B2B SaaS Kullanıcı ve Abonelik Sözleşmesi.
  - `/mesafeli-satis` & `/mesafeli-satis-sozlesmesi`: Mesafeli Satış Sözleşmesi (PayTR uyumlu).
  - `/iptal-iade`, `/teslimat`, `/teslimat-iade`, `/teslimat-ve-iade`: İptal, İade ve Hizmet Teslimat Koşulları.
- **Next.js API Proxy Rotaları:**
  - `/api/proxy/[...path]` (`frontend/src/app/api/proxy/[...path]/route.ts`) ve `/api/v1/[...path]` (`frontend/src/app/api/v1/[...path]/route.ts`): Tarayıcıdan gelen API çağrılarını CORS sorunsuz şekilde arka plandaki FastAPI servisine (`http://backend:8000/api/v1` veya `http://127.0.0.1:8000/api/v1`) iletir; 5MB JSON / 10MB dosya boyutu sınırını denetler.

---

## 26. ⚙️ Backend Kod Mimarisi: Router, Servis ve Veritabanı Modelleri Analizi

`backend/app/` dizini katmanlı mimari (Layered Architecture) ile yapılandırılmıştır:

1. **API Router Katmanı (`backend/app/routers/`):**
   - `auth.py`: Giriş (`/auth/login`), çıkış (`/auth/logout`), işletme kaydı (`/auth/register-business`), mevcut kullanıcı (`/auth/me`).
   - `salons.py`: Salon CRUD, müsait saatler (`occupied-slots`), telefonla müşteri tanıma (`customer-lookup`), personel yönetimi, izinler, kapalı günler, abonelik iptal/düşürme ve personel memnuniyet ligi.
   - `appointments.py`: Randevu oluşturma, durum/saat güncelleme, takip kodu/telefon araması, Google Calendar ve iCal üretimi, müşteri yorumu ekleme.
   - `services.py`: Hizmet detayı, güncelleme ve soft-delete (IDOR korumalı).
   - `spaces.py`: 2D mekan/koltuk/ünit CRUD ve canlı doluluk durumu.
   - `waitlist.py`: Bekleme listesi kaydı, FIFO teklif gönderimi ve 15 dk token onayı.
   - `dental.py`: Diş kliniği hasta anamnez profili, implant & lot kayıtları, protez laboratuvar siparişleri, hekim bordro raporu.
   - `formulations.py`: Kuaför müşteri boya reçetesi ve alerji notları CRUD.
   - `inventory.py`: Ürün/stok CRUD, kasadan perakende satış (`ProductSale`) ve stok hareketleri (`StockMovement`).
   - `commissions.py`: Personel maaş ve prim oranları tanımlama, aylık hak ediş hesaplama.
   - `analytics.py`: Ciro hedefleri, RFM müşteri analizi, yoğun saat ısı haritası ve ciro tahmini.
   - `payments.py`: PayTR iFrame token üretimi, 3D Secure webhook callback doğrulaması ve paket yükseltme.
   - `marketers.py`, `marketer_crm.py`, `marketer_crm_admin.py`: Pazarlamacı referans sistemi, müşteri adayı (Lead) yönetimi, arama geçmişi, Excel/CSV aktarım ve Web Sitesi Paketi satış/onay akışı.
   - `admin.py`: Süper Admin salon, kullanıcı, sektör, abonelik, aktivasyon kodu, blog, güvenlik ve yedekleme uçları.
   - `whatsapp.py`, `notifications.py`, `site_settings.py`, `contact.py`, `blog.py`, `sectors.py` ve 10 inovasyon router'ı.
2. **İş Mantığı ve Zamanlayıcı Servisler (`backend/app/services/`):**
   - `crypto_service.py`: AES-256-GCM AEAD şifreleme/çözme motoru.
   - `backup_scheduler.py` & `backup_service.py`: Her gece 03:00'te çalışan otomatik MySQL dump ve ZIP yedekleme servisi.
   - `reminder_service.py` & `whatsapp_service.py`: Randevuya 24 saat ve 2 saat kala otomatik WhatsApp hatırlatması gönderen arka plan döngüsü.
   - `marketer_cleanup_service.py`: Pazarlamacı portföylerindeki salonların abonelik devamlılığını denetleyen periyodik servis.

---

## 27. 🛠️ Yapılması Planlanan Değişiklikler

Kaynak kodları üzerinde yapılan statik ve mimari analiz sonucunda, mevcut sistemin performansını, güvenliğini, kod bakım kolaylığını ve ticari özellik kapsamını bir üst seviyeye taşımak için **yapılması planlanan değişiklikler ve öneriler** aşağıda listelenmiştir:

### 1. Veritabanı ve API Performans Optimizasyonları (N+1 Sorgu Darboğazlarının Giderilmesi)
- **Admin Salon Listesi N+1 Sorgu Optimizasyonu (`backend/app/routers/admin.py` — `list_admin_salons`):**
  - *Tespit:* `for s in salons:` döngüsü içinde her işletme için 3 ayrı `SELECT COUNT` sorgusu (`User`, `Appointment`, `Service`) çalıştırılmaktadır (200 işletmede toplam 601 SQL sorgusu).
  - *Öneri:* `GROUP BY salon_id` kullanan alt sorgular (subquery) ve `selectinload(Salon.sector)` kullanılarak tüm salon metriklerinin **tek bir SQL sorgusunda** çekilmesi.
- **Admin Pazarlamacı CRM İstatistikleri N+1 Optimizasyonu (`backend/app/routers/marketer_crm_admin.py` — `get_admin_crm_stats`):**
  - *Tespit:* `for m in marketers:` döngüsü içinde her pazarlamacı için 6 ayrı `SELECT COUNT` sorgusu (`leads_cnt`, `won_cnt`, `due_cnt`, `calls_cnt`, `pending_ws`, `member_count`) çalıştırılmaktadır.
  - *Öneri:* İlgili tabloların `GROUP BY marketer_id` ile toplu olarak tek seferde sorgulanıp bellek içinde eşleştirilmesi.
- **Personel Memnuniyet Ligi Sorgu Optimizasyonu (`backend/app/routers/salons.py` — `get_staff_competition`):**
  - *Tespit:* Her personel için döngü içinde ayrı `COUNT(Appointment.id)` ve `AVG(AppointmentReview.rating)` sorguları atılmaktadır.
  - *Öneri:* Tek bir `LEFT JOIN` + `GROUP BY User.id` sorgusu ile tüm personel skorlarının tek seferde hesaplanması.

### 2. Mimari Temizlik ve Kod Bakım Kolaylığı (Modülerleştirme)
- **Monolitik `api.ts` Dosyasının Modüllere Bölünmesi (`frontend/src/lib/api.ts`):**
  - *Tespit:* `frontend/src/lib/api.ts` dosyası **4.242 satıra** ulaşmıştır.
  - *Öneri:* Tıpkı `src/lib/api/marketerCrm.ts` örneğinde olduğu gibi, `api.ts` dosyasının `src/lib/api/auth.ts`, `salons.ts`, `appointments.ts`, `dental.ts`, `inventory.ts`, `admin.ts` alt modüllerine ayrılması ve ana `api.ts` dosyasının geriye dönük uyumluluk için `export *` (barrel file) olarak bırakılması.
- **Büyük Backend Router Dosyalarının Alt Modüllere Ayrılması:**
  - *Tespit:* `admin.py` (1.522 satır), `salons.py` (1.484 satır), `marketer_crm.py` (1.043 satır) ve `marketers.py` (~1.000 satır) tek dosyada çok fazla sorumluluk taşımaktadır.
  - *Öneri:* `routers/admin/` ve `routers/salons/` paket dizinleri oluşturularak uç noktaların mantıksal alt dosyalara bölünmesi.
- **Uygulama Açılışındaki (`lifespan`) Ham `ALTER TABLE` Komutlarının Alembic'e Taşınması (`backend/app/main.py`):**
  - *Tespit:* `main.py` satır 80-169 arasında her sunucu açılışında `salons`, `notifications` ve `marketers` tabloları için ham SQL `ALTER TABLE` kontrolleri çalışmaktadır.
  - *Öneri:* Çoklu worker açılışlarında tablo kilitlemesi (metadata lock) yaşanmaması adına tüm şema değişikliklerinin yalnızca `alembic/versions/` ve `mysql/vps_safe_migration_2026.sql` üzerinden yürütülmesi.
- **Fiyat Sabitlerinin Senkronizasyonu (`backend/app/constants.py`):**
  - *Tespit:* `constants.py` içindeki `PLAN_PRICES` sözlüğünde eski paket isimleri (`starter: 499`, `pro: 999`, `enterprise: 2499`) kalmıştır; aktif sistem ise `payments.py` içinde `Temel` (399/999/2999 TL) ve `Profesyonel` (699/1699/4999 TL) yapısını kullanmaktadır.
  - *Öneri:* `constants.py` içindeki sabitlerin güncel paket mimarisiyle eşitlenmesi.

### 3. Siber Güvenlik, Oturum ve Proxy İyileştirmeleri
- **JWT Token Saklama Mimarisi (`HttpOnly Cookie` Geçişi):**
  - *Tespit:* Frontend'de JWT token `localStorage` (`esteti_token`, `randevudesk_token`) ve JS erişimine açık cookie içinde saklanmaktadır.
  - *Öneri:* Olası XSS vektörlerine karşı oturum belirtecinin `HttpOnly; Secure; SameSite=Lax` bayraklı çerezler üzerinden taşınması.
- **Next.js API Proxy IP Doğrulaması ve Rate-Limit Tekilleştirmesi (`frontend/src/app/api/proxy/[...path]/route.ts`):**
  - *Tespit:* Next.js proxy katmanı hız sınırlamasını Node.js belleğindeki `ipRequestMap` içinde tutmakta ve `x-forwarded-for` başlığını doğrudan `X-Real-IP` üzerine yazmaktadır.
  - *Öneri:* Yalnızca güvenilir proxy (Nginx/Cloudflare) IP'lerinden gelen başlıkların kabul edilmesi ve hız sınırlamasının merkezi olarak FastAPI + Redis (`SlowAPI`) katmanına devredilmesi.

### 4. Ürün Yol Haritasındaki Eksik / Yeni Modüllerin Tamamlanması
- **Pazarlamacı CRM Faz 6 — Aday (Lead) ile Kayıt Olan Salonun Otomatik Eşleştirilmesi (`plan.md` Madde 4.4 & 8):**
  - *Öneri:* `/register` sayfasından yeni bir işletme kaydolduğunda, işletme telefonu veya kullanılan referans kodu `marketer_leads` tablosundaki bir adayla eşleşirse ilgili adayın `converted_salon_id` alanının otomatik doldurulması ve durumunun `won` (Kazanıldı) olarak işaretlenmesi.
- **Modül 13 — Paket & Çoklu Seans Yönetimi (Lazer Epilasyon, Cilt Bakımı, Fizyoterapi, Diyetisyen):**
  - *Öneri:* İşletmelerin müşterilerine 6, 8 veya 10 seanslık paket satışı yapabileceği, her tamamlanan randevuda kalan seans hakkının otomatik düşüldüğü ve müşteriye kalan seans bilgisinin WhatsApp ile iletildiği **Paket & Seans Takip Modülü** (`SessionPackage` & `CustomerSessionUsage`) eklenmesi.
- **Modül 14 — İşletme Gider & Masraf Yönetimi (Gerçek Net Kâr Tablosu):**
  - *Öneri:* Kira, elektrik, su, internet, mutfak ve personel avansı gibi sabit/değişken giderlerin kaydedilebildiği bir **Gider Modülü** (`SalonExpense`) eklenerek, `AnalyticsDashboardView` içinde `Brüt Ciro - (Ürün Maliyeti + Personel Primleri + İşletme Giderleri) = Gerçek Net Kâr` raporunun sunulması.
- **Ana Sayfa İnteraktif Simülatörün (`HeroInteractiveWidget`) Yeniden Devreye Alınması (`frontend/src/app/page.tsx`):**
  - *Tespit:* `HeroInteractiveWidget` bileşeni ilk sayfa yükleme (TBT) optimizasyonu amacıyla `page.tsx` içinde yorum satırına alınmıştır.
  - *Öneri:* Bileşenin "Tıkla ve Canlı Simülatörü Başlat" (Interaction-on-Demand Facade) mimarisiyle sayfa açılış hızını hiç etkilemeden tekrar aktif hale getirilmesi.

### 5. SEO, Rota Tekilleştirme ve Tasarım Anayasası Uyumu
- **Çift (Alias) Sayfa Rotalarında Canonical / 308 Yönlendirme Düzenlemesi:**
  - *Tespit:* `/fiyatlar`, `/fiyatlandirma`, `/pricing` ile `/kayit`, `/register` ve `/takip`, `/tracking` ile `/teslimat`, `/teslimat-iade`, `/teslimat-ve-iade` rotaları aynı içeriği farklı URL'lerde sunmaktadır.
  - *Öneri:* Arama motorlarında kopya içerik (duplicate content) oluşmaması için ikincil rotaların `next.config.ts` üzerinden `308 Permanent Redirect` ile ana Türkçe URL'lere (`/fiyatlar`, `/kayit`, `/takip`) yönlendirilmesi.
- **Tasarım Anayasası (`DEGISMEZ_KURALLAR.md`) Açık Tema Uyum Denetimi:**
  - *Öneri:* `DEGISMEZ_KURALLAR.md` Madde 4 uyarınca bazı inovasyon ve vizyon alt sayfalarında kalan koyu arka plan sınıflarının (`bg-slate-900` vb.) taranarak tamamen ferah açık mavi ve beyaz kurumsal temaya (`bg-white`, `bg-slate-50`, `bg-blue-50`) dönüştürülmesi.
