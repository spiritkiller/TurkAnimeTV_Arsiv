# 🧠 TurkAnimeTV Arşiv / Animeci Melkor — Memory Bank

Bu doküman, projenin mimarisini, kullanıcı direktiflerini, link temizliği kurallarını ve yeni anime ekleme standartlarını kalıcı olarak hafızada tutmak için oluşturulmuştur. Her yeni oturumda ve işlemde buradaki kurallara sadık kalınır.

---

## 📌 1. Proje Kimliği ve Mimarisi

* **Canlı Site:** [https://spiritkiller.github.io/TurkAnimeTV_Arsiv/](https://spiritkiller.github.io/TurkAnimeTV_Arsiv/)
* **Depo:** `https://github.com/spiritkiller/TurkAnimeTV_Arsiv`
* **Marka / Logo:** **Animeci Melkor** (Sauron miğferi, Tek Yüzük, `logo.png`). Sayfadaki genel isimler Animeci Melkor'a uyarlanmıştır.
* **Mimari:** Tamamen statik ve sunucusuz (GitHub Pages).
* **Veri Yapısı:**
  * `b/<slug>.js`: JSONP formatında anime bölümleri (`window.__TKA__["slug"] = [{no, ad, slug, links, fansubs}]`).
  * `search.html`: Tek parça ana sayfa, arama motoru, oynatıcı mantığı ve gömülü `window.INDEX` listesi (6.111+ anime).
  * `kaldirilan.js`: Kırık/ölü linklerin çalışma zamanında filtrelenmesini sağlayan dizi (`window.KALDIRILAN = [...]`).
  * `eklenen.js`: Mevcut animelere dışarıdan yeni link yaması yapan dizi.

---

## ⚡ 2. Kullanıcı Direktifleri & Rutin İş Akışı

1. **İsteğe Bağlı (On-Demand) Temizlik:**
   * Link kontrolü ve temizliği sürekli arka planda otomatik çalışmaz; **kullanıcı talep ettikçe** yapılır.
2. **Standart Temizlik & İkame Kuralı:**
   * Kullanıcı temizlik istediğinde: **Arşiv genelinde en az 50 çalışmayan link kaldırılır**, **5 tanesinin yerine çalışan link ikame edilir**.
3. **Yeni Anime Ekleme Kotası:**
   * Kullanıcı yeni anime eklenmesini istediğinde, linkleri doğrulanmış (HTTP 200) seriler seçilir (varsayılan paket: 5 yeni anime).
4. **Gerçek Link İlkesi:**
   * Sahte, tahmin edilmiş veya başka animelerin videosunu açan mükerrer linkler ASLA eklenmez. Yalnızca test edilip çalıştığı onaylanan linkler girilir.

---

## 🧹 3. Link Temizliği Prensipleri (`kaldirilan.js`)

Kırık linkler tespit edildiğinde hem ilgili `b/<slug>.js` dosyasından temizlenebilir hem de `kaldirilan.js` dosyasına eklenerek UI'dan tamamen gizlenir.

### Platform Durum Matrisi:
* **Sibnet (`video.sibnet.ru`):** Türkiye IP'lerine idari kurallar gereği **HTTP 403 Forbidden** engeli uygulamaktadır. Özel olarak çalışan embed bulunmadıkça öncelikli listeden düşürülür veya temizlenir.
* **Cyberfile.me:** Alan adı tamamen kapanmıştır. Tüm linkleri ölüdür (`kaldirilan.js` listesine alınır).
* **Doodstream (`dood.watch`):** Yönlendirme zinciri kırılmıştır (`dood.watch -> vide0.net -> playmogo.com` 404 döner).
* **Ghbrisk.com:** Alan adı ve CDN kapanmıştır.
* **MP4Upload / Uqload / Voe.sx:** Sunucudan dosya silindiğinde ("File was deleted" / 404) doğrudan `kaldirilan.js`'e eklenir.
* **Gömülemeyen Host'lar (`GOMULEMEZ_HOST`):**
  * `yadi.sk`, `disk.yandex.*`, `turkanime.tv/net`, `tranimaci.com`, `tranimeci.com`.
  * Bu siteler `X-Frame-Options: SAMEORIGIN` başlığı gönderdiği için iframe içinde açılamaz. `search.html` bunları iframe'e koyup "bağlanmayı reddetti" hatası vermek yerine şık bir **"Kaynak linkini aç"** butonuyla yeni sekmede açar.

---

## 🎬 4. Yeni / Çalışan Link Ekleme Prensipleri

Bir bölüme yeni çalışan link eklerken şu standartlara uyulur:

1. **Oynatıcı Öncelik Sıralaması (`PLAYER_ONCE`):**
   * `AITRVIP: 5` (JWPlayer / Optraco vb. doğrudan video oynatan özel embed'ler)
   * `SIBNET: 10` (Çalıştığı nadir durumlarda)
   * `MAIL: 20` (Mail.ru embed — CSP/iframe engelsiz)
   * `UQLOAD: 30`, `MP4UPLOAD: 40`, `MEGA: 60`, `VK: 70`
   * `OK.RU / ODNOKLASSNIKI: 90 / 200` (OK.ru embed — iframe dostu)
   * `GDRIVE: 240` (Google Drive `/preview` embed)
2. **Doğrulama Adımları:**
   * Link eklenmeden önce `curl` veya `urllib` ile HTTP status kodu (200 OK) ve iframe başlıkları (`X-Frame-Options`) doğrulanır.
3. **`window.INDEX` Güncellemesi:**
   * Eğer bir animeye yeni bir oynatıcı türü (örneğin `AITRVIP` veya `ODNOKLASSNIKI`) eklenirse, `search.html` içindeki `window.INDEX` satırında `top_players` dizisine bu etiket de eklenir.

---

## 📝 5. Özel Düzeltmeler & İşlem Geçmişi Günlüğü

| Tarih | Konu / Seri | Yapılan İşlem |
|---|---|---|
| **2026-10-01** | **Branding** | Logo "Animeci Melkor" olarak güncellendi, arayüzdeki 21 TürkAnime metni temizlendi. |
| **2026-10-01** | **2026 Sezonu 5 Yeni Anime** | `clevatess`, `clevatess-season-2`, `solo-leveling-season-2`, `dandadan-season-2`, `silent-witch` eklendi (Arşiv 6.107 -> 6.111 (yinelenen elendi) oldu). |
| **2026-10-01** | **Silent Witch** | `silent-witch-chinmoku-no-majo-no-kakushigoto` 13 bölüm çalışan TRAnimeci linkleriyle güncellendi. |
| **2026-10-03** | **Ys (4. Bölüm)** | 4. bölüme çalışan OK.ru embed'i (`https://ok.ru/videoembed/917285964507`) eklendi. |
| **2026-10-03** | **Ys (Sibnet Temizliği)** | `b/ys.js` içindeki 9 çalışmayan Sibnet linki kaldırıldı, `kaldirilan.js`'e işlendi. |
| **2026-10-03** | **3-gatsu no Lion 2. Sezon** | 16. bölüme çalışan AITRVIP JWPlayer embed'i (`optraco.top/...`) eklendi, ölü Sibnet kaldırıldı. |
| **2026-10-03** | **50 Link Temizliği + 5 İkame** | Arşivdeki 50 ölü link (MP4Upload vb.) `kaldirilan.js`'e eklendi. 5 bölüme çalışan Mail.ru ve TRAnimeci linkleri eklendi. |

---


---

## 📊 6. Güncel İstatistikler (README Senkronizasyonu - 03 Ekim 2026)

* **Toplam Anime Sayısı:** 6.111
* **Toplam Bölüm Sayısı:** 71.737
* **Toplam Video Linki:** ~317.102
* **Gizlenen Ölü Link Sayısı (`kaldirilan.js`):** 2.497
  * `cyberfile.me`: 1.149
  * `dood.watch`: 782
  * `ghbrisk.com`: 322
  * `docs.google.com`: 68
  * `byse.sx`: 57
  * `mp4upload.com`: 48
  * `turkanime.tv`: 19
  * `voe.sx`: 18
  * `mega.nz`: 18
  * `video.sibnet.ru`: 10
  * Diğer / Çeşitli: 25
* **Son Büyük Temizlik Tarihi:** 03 Ekim 2026
* **Yeni Eklenen / İkame Edilen Çalışan Linkler:**
  * Ys 4. Bölüm → OK.ru embed
  * 3-gatsu no Lion 2. Sezon 16. Bölüm → AITRVIP (optraco.top) JWPlayer embed
  * 0-saiji-start-dash-monogatari & Sezon 2 → 5 adet çalışan Mail.ru ve TRAnimeci linki

*Bu dosya, gelecekteki bakım seanslarında yapay zekanın doğrudan referans alacağı ana rehberdir.*
