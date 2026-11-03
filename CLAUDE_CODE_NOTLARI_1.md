# Kalori Defteri — devir notları

Tek dosyalık web uygulaması: `kalori.html` (HTML + CSS + vanilla JS, kütüphane yok). Tarayıcıda doğrudan açılır.

## Özellikler
- **Profil:** cinsiyet, yaş, boy, kilo, boyun/bel/kalça → yağ oranı (ABD Donanması formülü; ölçü yoksa VKİ'den tahmin). Dinlenme metabolizması: yağ oranı ölçülmüşse Katch-McArdle, yoksa Mifflin-St Jeor. × hareket katsayısı = günlük yakım. Hedef (−750…+300 kcal) → kalori + makro hedefleri (protein 1,8 g/kg veya 2,2 g/kg yağsız kütle, yağ ≥ %25, kalan karbonhidrat, lif 14 g/1000 kcal). Alt sınır: erkek 1500, kadın 1200 kcal.
- **Günlük:** kalori halkası, hedef / alınan / yakılan / açık, haftalık kilo kaybı tahmini, makro barları (eksik / yerinde / fazla), öneri metinleri, 4 öğün.
- **Öğün ekleme (3 mod):**
  1. Listeden — `RAW` sabiti: `kategori|ad|kcal|protein|karb|yağ|lif|porsiyon:gram;...` (100 g başına).
  2. Tariften hesapla — tencere malzemeleri + kaç tabak çıktı + ne kadar yendi (`EAT_POR`).
  3. Tahmini — yemek türü (`GUESS`) × porsiyon (`GUESS_POR`: avuç, tabak, tepeleme tabak…).
- **Spor:** `EXR` sabiti: `kategori|ad|tür|MET`. Türler: `s` kg×tekrar, `bw` vücut ağırlığı, `t` süreli tutuş, `c` kardiyo (dk, km), `d` drill (set × m). Kalori = MET × kilo × saat (koşuda MET hızdan hesaplanır). Son seanstan otomatik doldurma, seans farkı, ilerleme grafiği (SVG), tahmini 1TM (Epley), haftalık özet + son 6 hafta tablosu. Spor kalorisi günlük ekrana otomatik eklenir.

- **Şablonlar:** `TPLR` sabiti: `program|ad|hareket*plan;...` (plan: `3x10`, `3x45s`, `4x30m`, `20dk`, `30dk/5km`). 63 şablon, 14 program. Spor › Hareket ekle › Şablonlar: "Plan olarak başlat" günün `plan` alanına yazar (hareketlere tek tek tıklanır, ağırlık son seanstan dolar); "Hepsini hemen ekle" önceki ağırlığı bilinenleri doğrudan ekler.
- **Antrenman düzenleme (✎), "Şablon olarak kaydet"** (`S.tpl`, program adı "Kendi şablonlarım"), **dinlenme sayacı** (`S.rest` sn, sesli + titreşim).
- **Öğün ekleme:** son / sık yenenler (yeni kayıtlarda `pl`, `amt` saklanır), "Yeni yemek" sekmesi → `S.foods` (100 g değerleri, "Kendi yemeklerim").
- **Su:** `days[*].water` bardak (250 ml), hedef kilo × 35 ml + antrenman dk × 10 ml.
- **Rapor sekmesi:** haftalık özet (‹ › hafta değiştirir), günlük kalori çubukları, gün gün tablo. Boş günleri `S.days`'e yazmadan okur (`dayInfo`).
- **Vücut değişimi:** Profil sekmesi; kilo (`days[*].w`), yağ oranı ve bel (`S.meas["YYYY-MM-DD"]={neck,waist,hip}`, ölçü alanları değişince bugüne yazılır).
- **Veriler:** Profil sekmesinde JSON dışa/içe aktarma (içe aktarma mevcut verinin yerini alır).
- **PWA:** `manifest.json`, `sw.js` (ağ öncelikli, çevrimdışıysa önbellek), `icons/`. Service worker yalnızca http(s) üzerinde kaydolur. Yayın: https://selbugtech.github.io/kalori-defteri/

## Veri
- `localStorage` anahtarı `kalori-defteri-v1`. Yapı:
  `{v:2, sample, profile:{sex,age,height,weight,neck,waist,hip,act,goal}, days:{"YYYY-MM-DD":{ex, w, wo:[...], meals:{kahvalti,ogle,aksam,ara}}}}`
- Öğün kaydı: `{name, por, k, p, c, f, fib, sample?, est?}` (değerler kayıt anında hesaplanıp saklanır).
- Antrenman kaydı: `{id, ex, t, sets?, dur?, dist?, n?, note, sample?}` (kalori her çizimde o günün kilosuyla yeniden hesaplanır).
- İlk açılışta örnek profil, örnek öğünler ve 4 haftalık örnek antrenman yüklenir (`sample:true`), "Örnekleri temizle" ile silinir.

- Gün kaydında `plan:{name,prog,items:[{ex,spec,done}]}` ve örnek kilolar için `ws:true` olabilir.

## Olası sonraki adımlar
- Barkod okuma (Open Food Facts)
- Cihazlar arası senkron için backend
