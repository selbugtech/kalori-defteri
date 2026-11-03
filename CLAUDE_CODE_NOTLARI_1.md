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

## Veri
- `localStorage` anahtarı `kalori-defteri-v1`. Yapı:
  `{v:2, sample, profile:{sex,age,height,weight,neck,waist,hip,act,goal}, days:{"YYYY-MM-DD":{ex, w, wo:[...], meals:{kahvalti,ogle,aksam,ara}}}}`
- Öğün kaydı: `{name, por, k, p, c, f, fib, sample?, est?}` (değerler kayıt anında hesaplanıp saklanır).
- Antrenman kaydı: `{id, ex, t, sets?, dur?, dist?, n?, note, sample?}` (kalori her çizimde o günün kilosuyla yeniden hesaplanır).
- İlk açılışta örnek profil, örnek öğünler ve 4 haftalık örnek antrenman yüklenir (`sample:true`), "Örnekleri temizle" ile silinir.

## Olası sonraki adımlar
- Hazır antrenman şablonları (Push / Pull / Leg, tek tıkla ekleme)
- Kilo eğrisi grafiği
- Verileri dışa/içe aktarma (JSON) veya bir backend'e taşıma
- Mobil uygulamaya (React Native / PWA) dönüştürme
