# 🛡️ RandevuDesk — Sistem Mimarisi & Geliştirici Kılavuzu

RandevuDesk, B2B SaaS çok kiracılı (multi-tenant) randevu, klinik ve işletme yönetim platformudur. Yüksek veri güvenliği, KVKK Madde 6/12 uyumu, sıfır veri sızıntısı riski ve donanım hızlandırmalı modern kriptografik mimariyle geliştirilmiştir.

---

## 🔒 Merkezi Veri Şifreleme Standardı (Data-at-Rest Encryption)

RandevuDesk sisteminde **TEK VE STANDART BİR ŞİFRELEME ALGORİTMASI** kullanılmaktadır:

> ### 🔑 Standart Algoritma: **AES-256-GCM (Galois/Counter Mode, AEAD)**
> - **Anahtar Uzunluğu:** 256-bit (32 byte simetrik anahtar)
> - **Nonce / IV (Başlatma Vektörü):** 96-bit (12 byte) kriptografik rastgele (`os.urandom`)
> - **Bütünlük & Kimlik Doğrulama Etiketi (Auth Tag):** 128-bit (16 byte)
> - **Kütüphane:** Python `cryptography.hazmat.primitives.ciphers.aead.AESGCM` (C-seviyesi ve donanım hızlandırmalı: Intel AES-NI / ARM Crypto Extension)
> - **Veritabanı Saklama Formatı:** `enc::v1::<base64(12_byte_nonce + ciphertext_with_tag)>`

### 💡 Neden Yalnızca AES-256-GCM?
1. **AEAD Güvencesi (Authenticated Encryption):** Klasik CBC veya ECB modlarının aksine, verinin hem gizliliğini (confidentiality) hem de bütünlüğünü (integrity) garanti eder. Şifreli metin üzerinde yapılabilecek en ufak bir bit manipülasyonu veya tahrifat (tampering), auth tag doğrulaması ile anında yakalanır ve reddedilir.
2. **Semantik Güvenlik (Rastgele Nonce):** Aynı metin (örneğin "Penisilin alerjisi var") her kaydedildiğinde tamamen farklı bir şifreli metin üretilir. Saldırganların frekans veya örüntü analizi yapması matematiksel olarak imkansızdır.
3. **Milisaniye Altı Performans:** Donanım seviyesinde (AES-NI) çalıştığı için sunucu CPU'suna ek yük getirmez, web taleplerinde hissedilmeyecek hızda çalışır.
4. **Şeffaf ORM Entegrasyonu (SQLAlchemy TypeDecorator):** Backend'deki `EncryptedText` ve `EncryptedString` tipleri sayesinde kod içinde karmaşık şifreleme/çözme çağrılarına gerek kalmaz. Veri tabana yazılırken otomatik şifrelenir, tabandan okunurken otomatik çözülür.

### 🛡️ Olası Bir Sızıntıda Korunan Veriler (Threat Model)
Olası bir SQL Injection, sunucu ihlali veya MySQL dump sızıntısı senaryosunda saldırganların eline geçebilecek tüm kritik veriler fiziksel tabloda anlamsız şifreli metin (`enc::v1::...`) olarak saklanır:

| Tablo | Kolon | Korunan Veri Türü & KVKK Niteliği |
|---|---|---|
| `customer_formulations` | `formula_content` | Kuaför/Klinik özel formülasyon oranları, gramajları ve reçeteleri (Ticari Sır) |
| `customer_formulations` | `allergy_warning` | Saç/Cilt kimyasal alerjileri ve hassasiyet uyarıları (**KVKK Madde 6 Özel Nitelikli Sağlık Verisi**) |
| `customer_formulations` | `hair_skin_analysis` | Müşteri fiziki cilt ve saç analiz verileri (Hassas Değerlendirme) |
| `dental_patient_profiles` | `other_allergies` | Diş hekimliği ilaç/madde alerjileri (**KVKK Madde 6 Sağlık Verisi**) |
| `dental_patient_profiles` | `systemic_diseases` | Kronik ve sistemik hastalıklar: Diyabet, Astım vb. (**KVKK Madde 6 Sağlık Verisi**) |
| `dental_patient_profiles` | `regular_medications` | Düzenli kullanılan ilaçlar: Kan sulandırıcı, kalp vb. (**KVKK Madde 6 Sağlık Verisi**) |
| `dental_patient_profiles` | `notes` | Hekim klinik notları ve anamnez bulguları (**KVKK Madde 6 Sağlık Verisi**) |
| `dental_implant_records` | `notes` | Cerrahi implant ameliyat notları, kemik grefti/membran detayları |
| `dental_lab_orders` | `notes` | Laboratuvar özel protez talimatları ve hekim notları |
| `appointments` | `notes` | Müşteri randevu özel notları ve kişisel istekleri |
| `appointment_form_values` | `value` | Sektörel dinamik form sorularına müşterinin verdiği yanıtlar |
| `contact_messages` | `message` | İletişim formu mesaj metinleri |
| `contact_messages` | `admin_notes` | İletişim mesajı yönetici gizli notları |
| `appointment_reviews` | `review_text` | Müşteri randevu değerlendirme ve deneyim yorumları |

### ⚙️ Şifreleme Anahtarı Yapılandırması (`.env`)
```bash
# Üretim ortamında en az 32 karakterlik güçlü rastgele anahtar tanımlanmalıdır:
DATA_ENCRYPTION_KEY="sizin_32_karakterden_uzun_guclu_rastgele_anahtariniz_2026"
```
*Not: Eğer ortam değişkeni girilmezse sistem JWT_SECRET_KEY ve sistem salt'ı üzerinden güvenli 256-bit anahtar türeterek veri kaybı ve kesinti yaşanmasını önler.*

---

## 🚀 10 Devrimsel Randevu & Kuyruk Yönetimi Algoritması (Git Bulut / Origin Branch Haritası)

RandevuDesk, sıradan bir takvim yazılımı olmanın ötesinde bekleme salonundaki zehirli stresi, personelin hantallığını ve gelinmeyen randevuları (no-show) tamamen sıfırlayan 10 agresif algoritmaya sahiptir. Her bir özellik `main` dalına dokunulmaksızın bağımsız olarak geliştirilmiş, test edilmiş ve kendi `origin` branch'ine gönderilmiştir:

| # | Özellik Adı | Slogan & Konsept | Git Origin Branch | Canlı Arayüz | Backend API & Model |
|---|---|---|---|---|---|
| **1** | **Tersine Taksimetre** | *Bekleyen değil, bekleten öder.* Gecikilen her 5 dakikada müşteriye anlık QR kahve kuponu veya indirim tanımlanır. | `origin/feature/reverse-taximeter` | `/taksimetre` | `/api/v1/taximeter/*`<br>`TimeCompensationPolicy` |
| **2** | **Kıyamet Modu** | *İnternet ölür, sistem ölmez.* İnternet kopsa dahi yerel IndexedDB önbelleğinde çalışır, ağ gelince mesh sync ile eşitlenir. | `origin/feature/apocalypse-mode-offline` | `/kiyamet-modu` | `/api/v1/offline-sync/*`<br>`OfflineSyncQueue` |
| **3** | **"Et-Kemik" Veto Tuşu** | *Yapay zeka önerir, insan hükmeder.* Kırmızı veto butonuyla 10 saniyelik bypass token'ı üretilir, acil durumlarda kural delinir. | `origin/feature/veto-override-button` | `/veto` | `/api/v1/veto/*`<br>`VetoOverrideLog` |
| **4** | **Çift Taraflı İfşa Ekranı** | *Bürokrasinin arkasına saklanamazsınız.* Memurun ekranı dışarıdaki ikinci ekrana anlık yansıtılarak 'sistem dondu' bahanesi engellenir. | `origin/feature/anti-matrix-dual-display` | `/ifsa-ekrani` | `/api/v1/transparency/*`<br>`DeskSessionStatus` |
| **5** | **Randevu Borsası (P2P)** | *Gelmeyeceğin randevu çöp değil, nakittir.* Gelemeyecek müşteri slotunu serbest piyasada satışa çıkarır, no-show sıfırlanır. | `origin/feature/p2p-slot-trading` | `/randevu-borsasi` | `/api/v1/marketplace/*`<br>`SlotTradingOffer` |
| **6** | **Anti-Rehin Radarı** | *Bekleme salonu açık hava cezaevi değildir.* 400m geofence özgürlük sinyali; sıraya 5 dk kala otomatik dönüş çağrısı yapar. | `origin/feature/anti-hostage-radar` | `/ozgurluk-radari` | `/api/v1/radar/*`<br>`AntiHostageRadarSession` |
| **7** | **Manipülatif Yapay Zekâ** | *Personelin hantallığını içeriden hackleyen algoritma.* Doktor 7 dk sarkıyorsa ona gizli saat 14:08 gösterilir, sıfır bekleme elde edilir. | `origin/feature/shadow-scheduling-ai` | `/golge-planlama` | `/api/v1/shadow-schedule/*`<br>`StaffVelocityProfile` |
| **8** | **Giyotinli Pit-Stop Gişesi** | *Bürokrasinin acımasız Formula 1 şeridi.* 45 saniyelik mikro-şerit; 00:00 anında giyotin iner ve terminal kilitlenir. | `origin/feature/pitstop-guillotine-counter` | `/pit-stop` | `/api/v1/pitstop/*`<br>`PitStopCounterSession` |
| **9** | **Hayalet Check-In** | *En iyi karşılama deski, var olmayan destir.* Kapıdan giren danışanı UWB anteni tanır, doğrudan odaya ve koltuğa yönlendirir. | `origin/feature/ghost-checkin-uwb` | `/hayalet-checkin` | `/api/v1/ghost-checkin/*`<br>`GhostCheckinBeaconLog` |
| **10** | **Utanç Kronometresi** | *Başarısızlığı ifşa ederek hıza mecbur bırakmak.* Bekleme salonu TV ekranında Oda 1 Yeşil (Zamanında) vs Oda 3 Kırmızı (+18 dk gecikme) ifşa edilir. | `origin/feature/public-delay-board` | `/gecikme-panosu` | `/api/v1/delay-board/*`<br>`PublicDelayBoardItem` |

---

## 🐳 Docker Kullanım Kılavuzu

Bu belgede RandevuDesk projesinin Docker ile derlenmesi, önbellek temizliği ve log takibi için gerekli tüm komutlar yer almaktadır.

---

## 1. 🏗️ Docker'ı Build Etmek ve Çalıştırmak

### Derleyip Arka Planda Başlatma (Build & Up)
Projeyi derlemek ve tüm servisleri arka planda ayağa kaldırmak için:
```bash
docker compose up -d --build
```

### Sadece İmajları Build Etmek (Başlatmadan)
```bash
docker compose build
```

### Önbelleksiz Sıfırdan Temiz Build (No-Cache)
```bash
docker compose build --no-cache
```

### Servisleri Durdurma / Yeniden Başlatma
```bash
# Tüm servisleri durdur
docker compose down

# Tüm servisleri yeniden başlat
docker compose restart
```

## 🚀 VPS Prod Dağıtımı ve Güvenli Veritabanı Güncelleme (Tek Satır)

GitHub'dan güncel kodları çekmek, prod konteynerlerini derleyip arka planda ayağa kaldırmak ve **mevcut kullanıcı verilerini, salonları ve şifreleri kesinlikle bozmadan/silmeden** SQL yeniliklerini uygulamak için VPS terminalinize yapıştırabileceğiniz tek satırlık komut:

```bash
git pull && docker compose -f docker-compose.prod.yml up -d --build && docker exec -i randevudesk_mysql mysql -u root -pesteti_root_2026 estetidb < mysql/vps_safe_migration_2026.sql
```

---

## 🚀 VPS Hızlı Güncelleme (MySQL'e Dokunmadan Sıfırdan Build & Cache Temizleme)

VPS terminalinize yapıştırıp çalıştırabileceğiniz, **MySQL'e kesinlikle dokunmayan**, Redis önbelleğini sıfırlayan ve **Frontend, Backend ve Nginx** servislerini sıfırdan (`--no-cache`) derleyip başlatan tek satırlık komut:

```bash
docker exec randevudesk_redis redis-cli FLUSHALL && \
docker compose build --no-cache frontend backend nginx && \
docker compose up -d --no-deps frontend backend nginx && \
docker ps
```

### 🛡️ Bu Komut Neler Yapar? (Güvenlik & Detaylar)
- **`docker exec randevudesk_redis redis-cli FLUSHALL`**: Redis içerisindeki tüm oturum/sorgu önbelleğini anında temizler.
- **`docker compose build --no-cache frontend backend nginx`**: MySQL kesinlikle derlenmez ve hariç tutulur. Sadece frontend, backend ve nginx sıfırdan, tertemiz derlenir.
- **`docker compose up -d --no-deps frontend backend nginx`**: `--no-deps` parametresi sayesinde MySQL konteyneri durdurulmaz veya yeniden başlatılmaz, kesintisiz çalışmaya devam eder. Sadece derlenen yeni konteynerler güncellenir.
- **`docker ps`**: İşlem bittiğinde tüm servislerin aktif durumunu ekranda listeler.


---

## 2. ⚡ Redis Ön Belleğini (Cache) Temizlemek

Redis konteyneri içerisindeki tüm önbellek ve anahtarları sıfırlamak için:

```bash
docker exec -it randevudesk_redis redis-cli FLUSHALL
```

> **Not:** Arka planda sunucuyu kilitlemeden asenkron silmek için:
> ```bash
> docker exec -it randevudesk_redis redis-cli FLUSHALL ASYNC
> ```

---

## 3. 🧹 Docker Ön Belleğini (Cache) Temizlemek

### Docker Build Cache'ini Temizleme
Eski derleme kalıntılarını ve kullanılmayan build katmanlarını silmek için:
```bash
docker builder prune -a -f
```

### Tüm Kullanılmayan Docker Kaynaklarını Temizleme (Tam Temizlik)
Durdurulmuş konteynerler, kullanılmayan imajlar ve ağları temizlemek için:
```bash
docker system prune -a --volumes -f
```

---

## 4. 📜 Konteynerlerin Son 50 Geçmiş Logunu Göstermek

### Tüm Servislerin Son 50 Satır Logunu Canlı İzleme
```bash
docker compose logs --tail=50 -f
```

### Her Bir Konteynerin Ayrı Ayrı Son 50 Satır Logu

#### 🖥️ Frontend (Next.js)
```bash
docker logs --tail=50 -f randevudesk_frontend
```

#### ⚙️ Backend (FastAPI)
```bash
docker logs --tail=50 -f randevudesk_backend
```

#### 🌐 Nginx (Reverse Proxy & Web Server)
```bash
docker logs --tail=50 -f randevudesk_nginx
```

#### 🗄️ MySQL (Veritabanı)
```bash
docker logs --tail=50 -f randevudesk_mysql
```

#### ⚡ Redis (Cache)
```bash
docker logs --tail=50 -f randevudesk_redis
```

#### 🔒 Certbot (SSL)
```bash
docker logs --tail=50 -f randevudesk_certbot
```

#### 📊 Portainer (Yönetim Paneli)
```bash
docker logs --tail=50 -f randevudesk_portainer
```

#### 📦 MinIO (Object Storage)
```bash
docker logs --tail=50 -f randevudesk_minio
```

#### 🔍 Elasticsearch (Arama & Analitik)
```bash
docker logs --tail=50 -f randevudesk_elasticsearch
```


---

## ⚖️ Lisans ve Telif Hakkı (License & Copyright)

Bu projenin tüm kaynak kodları, mimarisi ve fikri mülkiyet hakları münhasıran **Ömer Faruk Uysal**'a aittir. Yazılı izin olmaksızın kısmen veya tamamen kopyalanması, yayımlanması, dağıtılması veya kullanılması kesinlikle yasaktır. İhlal durumunda 5846 sayılı FSEK ve ilgili mevzuat uyarınca hukuki ve cezai dava açma hakkı saklıdır. Detaylı bilgi için [LICENSE](LICENSE) belgesine bakınız.

**Copyright (C) 2026 Ömer Faruk Uysal. Tüm Hakları Saklıdır.**
