
<div align="center">

# TurkAnimeTV Arşiv

**turkanime.tv 19 Eylül 2026'da bir gece ansızın kapandı ve arkasında devasa bir arşiv bıraktı. Bu proje, o arşivi kurtarmak ve erişilebilir kılmak için başlatıldı. Tamamen statik ve sunucusuz (GitHub Pages) çalışan sitemizde 6.112 anime ve 317.068 video linki bulunuyor. Animelerin isimleri, bölüm linkleri, fansub grupları ve çevirmen bilgileri de açık bir şekilde paylaşılmaktadır.**

[![Website](https://img.shields.io/website?url=https%3A%2F%2Fspiritkiller.github.io%2FTurkAnimeTV_Arsiv%2F&label=canlı%20site)](https://spiritkiller.github.io/TurkAnimeTV_Arsiv/)
[![Anime](https://img.shields.io/badge/anime-6.112-green)](https://spiritkiller.github.io/TurkAnimeTV_Arsiv/)
[![Video Linki](https://img.shields.io/badge/video%20linki-317.068-blue)](https://spiritkiller.github.io/TurkAnimeTV_Arsiv/)
[![Son Güncelleme](https://img.shields.io/badge/son%20güncelleme-Ekim%202026-orange)](https://github.com/spiritkiller/TurkAnimeTV_Arsiv/commits/main)

<!-- TODO: Ekran görüntüsü eklenecek -->
  
🔗 [**Canlı site**](https://spiritkiller.github.io/TurkAnimeTV_Arsiv/)

🔗 [**Veritabanı dosyalarını indirmek için tıkla**](https://github.com/spiritkiller/TurkAnimeTV_Arsiv/releases)

</div>

---

## Bu proje nedir?

2010 yılında kurulan **turkanime.tv**, 19 Eylül 2026'da hiçbir uyarı vermeden kapandı. Yıllarca biriktirilen fansub çevirilerini, bölüm linklerini ve çevirmen emeklerini kaybetmemek için bu arşiv projesi hayata geçirildi.

Sitede bulunan tüm player (video) linkleri, fansub bilgileri, çevirmen ve redaktör bilgileri bu depoda herkese açık şekilde sunulmaktadır.

---

## Hikâye

Turkanime kapanmadan haftalar önce sitenin çalışma mantığını incelemiştim; başka platformların linklerini alıp embed olarak kullandığını fark etmiştim. [KebabLord](https://github.com/kebablord)'un daha önce yazdığı [turkanime-indirici](https://github.com/KebabLord/turkanime-indirici) projesinden ilham alarak, site hakkında bildiklerimi yapay zeka ile birleştirip tüm arşivi çıkarmayı başardım.

Elde ettiğimiz veritabanı başlangıçta 1,6 milyona yakın link içeriyordu; büyük çoğunluğu kapanmış sitelere, telif nedeniyle kaldırılan videolara ya da artık bambaşka içeriklere hizmet eden domainlere gidiyordu. Yaklaşık 3,5 gün, günlük 4 saatlik uykuyla veriyi temizledik. Bu süreçte yardımcı olan herkese ayrı ayrı teşekkür ederim.

**Bugün itibarıyla (Ekim 2026)** arşiv aktif bakım altında: bozuk linkler tespit edilip gizleniyor, yeni linkler ekleniyor, site düzenli olarak güncelleniyor.

---

## Özellikler

- 🔍 **Anlık arama** (`/` kısayolu), sıralanabilir & filtrelenebilir 6.112 animelik tam liste, kategori sayfaları, rastgele anime butonu
- ▶️ **Bölüme tıkla → doğrudan player**; KAYNAK çipleriyle kaynaklar arası hızlı geçiş (çalışan kaynak otomatik seçilir)
- 🏷️ Bölüm bazlı fansub + çevirmen bilgisi (kaynak çiplerinde ve player kartında görünür)
- 🛡️ **Override mimarisi**: `kaldirilan.js` ve `eklenen.js` ile 6.107 dosyaya dokunmadan anlık link gizleme/ekleme
- 🖼️ **AniList zenginleştirme**: kapak, banner, özet, puan, yıl ve tür bilgileri
- 📱 Mobil uyumlu arayüz, koyu tema
- ⚡ Sunucu yok, tamamen statik — GitHub Pages üzerinden çalışır

---

## Site nasıl çalışıyor?

```
Tarayıcı
 └─ search.html          → tek dosyalık SPA (gömülü arama dizini: 6.112 anime)
     ├─ anilist.js       → AniList metadata (kapak, banner, özet, puan…)
     ├─ b/<slug>.js      → her animenin bölümleri + video linkleri (6.107 dosya)
     ├─ kaldirilan.js    → çalışma anında gizlenen linkler   (override)
     └─ eklenen.js       → sonradan eklenen linkler           (override)
```

- Arama dizini (`window.INDEX`) doğrudan `search.html` içine gömülüdür; arama ve liste hiçbir istek atmadan çalışır.
- Bir anime seçildiğinde yalnızca o animenin `b/<slug>.js` dosyası yüklenir.
- AniList verisi önce `anilist.js`'ten, yoksa doğrudan [AniList GraphQL API](https://docs.anilist.co/)'sinden alınır ve `localStorage`'da önbelleklenir.
- **Override mimarisi** projenin kalbi: ölü linki gizlemek veya yeni link eklemek için 6.107 dosyayı yeniden üretmeye gerek yoktur; iki küçük JS dosyası çalışma anında okunur ve push'landıktan 1-2 dakika sonra değişiklik herkes için yayındadır.

---

## Bir link öldüğünü fark ettim — ne yapmalıyım?

1. `kaldirilan.js` dosyasına ölü linki tek satır olarak ekle (GitHub web arayüzünden de yapılabilir).
2. Pull request aç — onaylandıktan sonra link herkes için gizlenir.
3. Ya da [issue aç](https://github.com/spiritkiller/TurkAnimeTV_Arsiv/issues) — biz elle düzeltiriz.

---

## Bakım durumu (Ekim 2026)

| Kontrol | Sonuç |
|---|---|
| Toplam anime | 6.107 |
| Toplam video linki | ~317.068 |
| Gizlenen ölü link | 2.439 (80 + 323 ghbrisk + 69 docs.google + 15 voe.sx + 1.150 cyberfile.me + 19 mega.nz + 783 dood.watch) |
| Son büyük temizlik | 01 Ekim 2026 |
| Aktif video kaynakları | Sibnet (133K), Mail.ru (77K), OK.ru (36K), VK (22K)… |

---

## Katkıda bulunanlar

- [**Kebablord**](https://github.com/KebabLord) — [turkanime-indirici](https://github.com/KebabLord/turkanime-indirici) projesi olmasa bu arşiv var olamazdı. o7
- [**Ayruki**](https://x.com/ayruki) — 1,6M linklik veritabanının temizlenmesinde büyük emek. [Silinen linkler projesi](https://github.com/ayruki/turkanime-silinenler) hayat kurtardı.
- [**roxyrekt**](https://github.com/roxyrekt) — Veritabanının karmaşık yapısını bizden önce çözdü ve paylaşmaya izin verdi. Kendi projesi [Migurdex](https://github.com/roxyrekt/Migurdex)'te kullanmaya devam ediyor.
- [**Kerim Demirkaynak**](https://x.com/KDxOFFICIAL) — GitHub Pages sayfasına katkı, [Ekşi Sözlük duyurusu](https://eksisozluk.com/entry/186545960).
- **Anonim** — 2025'te turkanime.net'ten tüm bölümlerin fansub bilgilerini çekip bizimle paylaştı.
- [**OpenAnime**](https://github.com/OpenAnime) — Projeyi duyurarak daha fazla kişinin haberdar olmasını sağladı.
- [**AnimeHaber**](https://www.reddit.com/user/AnimeHaber/) — r/lostmediatr üzerinden duyurdu.
- [**Burakuslendera**](https://github.com/Burakuslendera) — Varlığı yeter.

---

## Yasal uyarı

Bu proje yalnızca **kişisel kullanım ve kültürel arşivleme** amacıyla hazırlanmıştır. Depoda hiçbir video barındırılmaz; yalnızca üçüncü taraf video platformlarındaki, o sırada kamuya açık olan sayfalara giden bağlantılar listelenir. Tüm içerik hakları ilgili hak sahiplerine aittir. Hak sahibi bir talepte bulunmak isterseniz depo üzerinden iletişime geçin; ilgili bağlantılar derhal kaldırılacaktır.

## Lisans

Kod [MIT](LICENSE) lisansı altındadır. Arşiv verisine (anime, bölüm ve link listeleri) ilişkin haklar ilgili hak sahiplerine aittir; bkz. [Yasal uyarı](#yasal-uyarı).
