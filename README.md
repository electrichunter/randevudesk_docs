# 🚀 RandevuDesk — Tüm Özellikler, Sayfa Haritası ve Kutulu Canlı Görsel Rehberi

Bu depo/klasör (`docs/`), **RandevuDesk B2B SaaS** platformunun tüm modüllerini, hangi sayfanın ve hangi özelliğin ne işe yaradığını, Docker (`http://localhost:3000`) üzerinden alınmış **55 gerçek ekran görüntüsünü (üzerinde numaralı kutular ve Türkçe açıklamalarla)** ve en altta **Yapılması Planlanan Değişiklikler** listesini içerir.

---

## 📂 Klasör İçeriği

| Dosya / Klasör | Açıklama |
| :--- | :--- |
| 📄 **[`ozellikler.md`](./ozellikler.md)** | **Ana Doküman:** Tüm sistem özellikleri (Bölüm 1–24), Kapsamlı Sayfa & Rota Haritası (Bölüm 25), **55 Kutulu & Açıklamalı Canlı Ekran Rehberi (Bölüm 26)**, Backend Kod Mimarisi (Bölüm 27) ve **Yapılması Planlanan Değişiklikler (Bölüm 28)** |
| 🖼️ **[`screenshots/`](./screenshots/)** | Canlı uygulamadan alınan ve her UI bölgesi `[1]`, `[2]`, `[3]`, `[4]` numaralı kutularla + alt rehber paneliyle açıklanan **55 adet yüksek çözünürlüklü PNG görsel** |

---

## 🧭 Menü Navigasyon Durumu (Aktif vs. 🚧 Geliştirilme Aşamasında)

- **✅ Aktif (Menüde Linki Var):** Ana üst menüde (`Navbar`), İşletme Paneli sol menüsünde (`MiniOfficeSidebar`) veya Süper Admin sol menüsünde (`AdminSidebar`) doğrudan tıklanabilir bağlantısı bulunan aktif ekranlar.
- **🚧 Geliştirilme Aşamasında (Menüde Linki Yok):** Kod tabanında (`frontend/src/app/*` ve `backend/app/routers/*`) mevcut olan ancak aktif navigasyon menülerine henüz bağlanmamış (yalnızca doğrudan URL ile erişilebilen) özellikler ve 10 Radikal İnovasyon ekranı. Bu özellikler hem [`ozellikler.md`](./ozellikler.md) içinde hem de ekran görüntülerinin üst barında turuncu **`[GELİŞTİRİLME AŞAMASINDA — Menüde Linki Yok]`** etiketiyle işaretlenmiştir.

### 📸 55 Ekran Görüntüsü Hızlı İndeksi

| Ekran No | Görsel Dosyası | Sayfa Rotası (URL) | Menü Durumu |
| :---: | :--- | :--- | :--- |
| **01** | [`01_ana_sayfa.png`](./screenshots/01_ana_sayfa.png) | `/` | ✅ Aktif (Menüde Linki Var) |
| **02** | [`02_randevu_formu.png`](./screenshots/02_randevu_formu.png) | `/form/[slug]` | ✅ Aktif (Menüde Linki Var) |
| **03** | [`03_isletme_vitrin.png`](./screenshots/03_isletme_vitrin.png) | `/vitrin/[slug]` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **04** | [`04_qr_menu_b2c.png`](./screenshots/04_qr_menu_b2c.png) | `/menu/[slug]` | ✅ Aktif (Menüde Linki Var) |
| **05** | [`05_tv_kiosk_ekrani.png`](./screenshots/05_tv_kiosk_ekrani.png) | `/kiosk/[slug]` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **06** | [`06_randevu_takip_radari.png`](./screenshots/06_randevu_takip_radari.png) | `/takip` | ✅ Aktif (Menüde Linki Var) |
| **07** | [`07_fiyatlar_paketler.png`](./screenshots/07_fiyatlar_paketler.png) | `/fiyatlar` | ✅ Aktif (Menüde Linki Var) |
| **08** | [`08_kayip_ciro_hesaplayici.png`](./screenshots/08_kayip_ciro_hesaplayici.png) | `/araclar/kacirilan-randevu-maliyeti` | ✅ Aktif (Menüde Linki Var) |
| **09** | [`09_sektorel_demo_merkezi.png`](./screenshots/09_sektorel_demo_merkezi.png) | `/demo` | ✅ Aktif (Menüde Linki Var) |
| **10** | [`10_isletme_kayit.png`](./screenshots/10_isletme_kayit.png) | `/register` | ✅ Aktif (Menüde Linki Var) |
| **11** | [`11_dashboard_genel_bakis.png`](./screenshots/11_dashboard_genel_bakis.png) | `/dashboard?salon=1` | ✅ Aktif (Menüde Linki Var) |
| **12** | [`12_dashboard_takvim.png`](./screenshots/12_dashboard_takvim.png) | `/dashboard?tab=calendar` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **13** | [`13_dashboard_randevular.png`](./screenshots/13_dashboard_randevular.png) | `/dashboard/appointments` | ✅ Aktif (Menüde Linki Var) |
| **14** | [`14_dashboard_kroki_spaces.png`](./screenshots/14_dashboard_kroki_spaces.png) | `/dashboard?tab=spaces` | ✅ Aktif (Menüde Linki Var) |
| **15** | [`15_dashboard_bekleme_listesi.png`](./screenshots/15_dashboard_bekleme_listesi.png) | `/dashboard?tab=waitlist` | ✅ Aktif (Menüde Linki Var) |
| **16** | [`16_dashboard_boya_formulasyon.png`](./screenshots/16_dashboard_boya_formulasyon.png) | `/dashboard?tab=formulations` | ✅ Aktif (Menüde Linki Var) |
| **17** | [`17_dashboard_dis_laboratuvar.png`](./screenshots/17_dashboard_dis_laboratuvar.png) | `/dashboard/dental/laboratuvar` | ✅ Aktif (Menüde Linki Var) |
| **18** | [`18_dashboard_dis_implant.png`](./screenshots/18_dashboard_dis_implant.png) | `/dashboard/dental/implants` | ✅ Aktif (Menüde Linki Var) |
| **19** | [`19_dashboard_dis_hakedis.png`](./screenshots/19_dashboard_dis_hakedis.png) | `/dashboard/dental/hakedis` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **20** | [`20_dashboard_hizmetler.png`](./screenshots/20_dashboard_hizmetler.png) | `/dashboard/services` | ✅ Aktif (Menüde Linki Var) |
| **21** | [`21_dashboard_stok_pos.png`](./screenshots/21_dashboard_stok_pos.png) | `/dashboard?tab=inventory` | ✅ Aktif (Menüde Linki Var) |
| **22** | [`22_dashboard_personel_bordro.png`](./screenshots/22_dashboard_personel_bordro.png) | `/dashboard?tab=payroll` | ✅ Aktif (Menüde Linki Var) |
| **23** | [`23_dashboard_memnuniyet_ligi.png`](./screenshots/23_dashboard_memnuniyet_ligi.png) | `/dashboard?tab=competition` | ✅ Aktif (Menüde Linki Var) |
| **24** | [`24_dashboard_analitik_rfm.png`](./screenshots/24_dashboard_analitik_rfm.png) | `/dashboard?tab=analytics` | ✅ Aktif (Menüde Linki Var) |
| **25** | [`25_dashboard_abonelik_paytr.png`](./screenshots/25_dashboard_abonelik_paytr.png) | `/dashboard/subscription` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **26** | [`26_dashboard_bildirimler.png`](./screenshots/26_dashboard_bildirimler.png) | `/dashboard/notifications` | ✅ Aktif (Menüde Linki Var) |
| **27** | [`27_dashboard_qr_menu_studyosu.png`](./screenshots/27_dashboard_qr_menu_studyosu.png) | `/dashboard/qr-menu` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **28** | [`28_dashboard_widget_entegrasyon.png`](./screenshots/28_dashboard_widget_entegrasyon.png) | `/dashboard/integration` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **29** | [`29_dashboard_isletme_ayarlari.png`](./screenshots/29_dashboard_isletme_ayarlari.png) | `/dashboard/settings` | ✅ Aktif (Menüde Linki Var) |
| **30** | [`30_dashboard_akilli_ayna_nfc.png`](./screenshots/30_dashboard_akilli_ayna_nfc.png) | `/dashboard/smart-mirrors` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **31** | [`31_dashboard_story_studyosu.png`](./screenshots/31_dashboard_story_studyosu.png) | `/dashboard/story-studio` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **32** | [`32_dashboard_vip_sadakat.png`](./screenshots/32_dashboard_vip_sadakat.png) | `/dashboard/loyalty` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **33** | [`33_admin_isletmeler.png`](./screenshots/33_admin_isletmeler.png) | `/admin` | ✅ Aktif (Menüde Linki Var) |
| **34** | [`34_admin_sektorler.png`](./screenshots/34_admin_sektorler.png) | `/admin/sektorler` | ✅ Aktif (Menüde Linki Var) |
| **35** | [`35_admin_abonelikler.png`](./screenshots/35_admin_abonelikler.png) | `/admin/abonelikler` | ✅ Aktif (Menüde Linki Var) |
| **36** | [`36_admin_paket_fiyatlari.png`](./screenshots/36_admin_paket_fiyatlari.png) | `/admin/paket-fiyatlari` | ✅ Aktif (Menüde Linki Var) |
| **37** | [`37_admin_finans_kdv.png`](./screenshots/37_admin_finans_kdv.png) | `/admin/finans` | ✅ Aktif (Menüde Linki Var) |
| **38** | [`38_admin_aktivasyon_kodlari.png`](./screenshots/38_admin_aktivasyon_kodlari.png) | `/admin/kodlar` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **39** | [`39_admin_pazarlamacilar.png`](./screenshots/39_admin_pazarlamacilar.png) | `/admin/pazarlamacilar` | ✅ Aktif (Menüde Linki Var) |
| **40** | [`40_admin_pazarlamaci_adaylari.png`](./screenshots/40_admin_pazarlamaci_adaylari.png) | `/admin/pazarlamaci-adaylari` | ✅ Aktif (Menüde Linki Var) |
| **41** | [`41_admin_web_paketleri.png`](./screenshots/41_admin_web_paketleri.png) | `/admin/web-paketleri` | ✅ Aktif (Menüde Linki Var) |
| **42** | [`42_admin_guvenlik_ve_loglar.png`](./screenshots/42_admin_guvenlik_ve_loglar.png) | `/admin/guvenlik` | ✅ Aktif (Menüde Linki Var) |
| **43** | [`43_admin_site_ayarlari.png`](./screenshots/43_admin_site_ayarlari.png) | `/admin/site-ayarlari` | ✅ Aktif (Menüde Linki Var) |
| **44** | [`44_pazarlamaci_portali.png`](./screenshots/44_pazarlamaci_portali.png) | `/pazarlamaci` | ✅ Aktif (Menüde Linki Var) |
| **45** | [`45_vizyon_merkezi.png`](./screenshots/45_vizyon_merkezi.png) | `/vizyon` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **46** | [`46_inovasyon_taksimetre.png`](./screenshots/46_inovasyon_taksimetre.png) | `/taksimetre` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **47** | [`47_inovasyon_kiyamet_modu.png`](./screenshots/47_inovasyon_kiyamet_modu.png) | `/kiyamet-modu` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **48** | [`48_inovasyon_veto_tusu.png`](./screenshots/48_inovasyon_veto_tusu.png) | `/veto` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **49** | [`49_inovasyon_ifsa_ekrani.png`](./screenshots/49_inovasyon_ifsa_ekrani.png) | `/ifsa-ekrani` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **50** | [`50_inovasyon_randevu_borsasi.png`](./screenshots/50_inovasyon_randevu_borsasi.png) | `/randevu-borsasi` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **51** | [`51_inovasyon_ozgurluk_radari.png`](./screenshots/51_inovasyon_ozgurluk_radari.png) | `/ozgurluk-radari` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **52** | [`52_inovasyon_golge_planlama.png`](./screenshots/52_inovasyon_golge_planlama.png) | `/golge-planlama` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **53** | [`53_inovasyon_pitstop.png`](./screenshots/53_inovasyon_pitstop.png) | `/pit-stop` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **54** | [`54_inovasyon_hayalet_checkin.png`](./screenshots/54_inovasyon_hayalet_checkin.png) | `/hayalet-checkin` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |
| **55** | [`55_inovasyon_gecikme_panosu.png`](./screenshots/55_inovasyon_gecikme_panosu.png) | `/gecikme-panosu` | 🚧 Geliştirilme Aşamasında (Menüde Linki Yok) |

---

> Detaylı açıklamalar, kutu rehberleri ve **Yapılması Planlanan Değişiklikler** için **[`ozellikler.md`](./ozellikler.md)** dosyasını inceleyiniz.
