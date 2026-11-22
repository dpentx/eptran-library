# eptran-library

eptran'ın bitmiş çevirilerinin kütüphanesi. Çeviri pipeline'ı (eptran reposundaki
GitHub Actions) staging dallarına yazar; kitap onaylanınca `main`'e alınır.
Bu repo public'tir ve **sadece çeviriyi** tutar: orijinal (İngilizce) metin
burada saklanmaz.

## Yapı

```
<seri-slug>/
  index.json
  <cilt-slug>/
    001.txt
    002.txt
    ...
    status.json
```

- Bölüm dosyaları sıfırla doldurulur (`001`, `002`, ...) ki sıralama bozulmasın.
  Dosya numarası sırayla bölüm sırasıdır; web sitesindeki bölüm sırası bununla
  birebir eşleşir.
- `status.json`, pipeline'ın o ciltteki durumunun bir **aynasıdır** (çeviri,
  review, QA, convert aşamaları). Asıl kopya eptran'daki kitap dalında durur;
  burası sadece okunur bilgi içindir, elle düzenlenmez.
- EPUB dosyaları repoda değil, Release'lerde durur (etiket = cilt slug'ı).

## Dallar

- `staging/<cilt-slug>`: kitap çevrilirken pipeline buraya her bölümü ve durum
  aynasını artımlı yazar. Kitabın onay PR'ı eptran'da merge olunca içeriği
  `main`'e alınır ve bu dal silinir.
- `main`: onaylanmış, yayımlanmış kitaplar.
- `images` (planlı): PDF modunda çıkarılan görseller. Henüz kullanılmıyor.

## Bölüm dosyası biçimi

Bölüm dosyalarının iki biçimi vardır; hangisini gördüğünüz dala bağlıdır.

**`main` (yayın biçimi)** — okuyucu sitesinin ve düzeltme PR'larının gördüğü:

```
<başlık, ör. "Bölüm 1: ...">

<paragraf>

<paragraf>
```

İlk satır başlıktır, boş satırdan sonra paragraflar gelir (paragraflar boş
satırla ayrılır). Başlık satırında `#` ya da İngilizce başlık yoktur.

**`staging/*` (çalışma biçimi)** — pipeline'ın yazdığı, çeviri sürerken:

```
# <orijinal bölüm başlığı>

<çevrilmiş başlık satırı>

<paragraf>
```

İlk satır `# ` ile başlayan orijinal başlıktır. Kitap `main`'e alınırken
(promote) pipeline bu biçimi yayın biçimine çevirir; dönüşüm tekrar
uygulandığında değişmez. Kural eptran'daki `scripts/lib/chapter_format.py`
dosyasındadır (`convert.py`'deki `load_txt_chapters` ile aynı sonucu verir,
testle korunur). Bu dosyaları okuyan araçlar başlık/gövde ayırmasını kendi
başına yorumlamaz.

## index.json

```json
{
  "seri": "ornek-seri",
  "ad": "Örnek Seri",
  "ciltler": [
    {
      "slug": "cilt-01",
      "baslik": "Cilt 1",
      "yazar": null,
      "bolum_sayisi": 12,
      "epub": "https://github.com/dpentx/eptran-library/releases/download/<etiket>/<dosya>.epub"
    }
  ]
}
```

`ciltler` slug'a göre sıralıdır. Girdiyi pipeline yayın adımında üretir.

## Düzenleme

- Süren çeviriye dokunulmaz: `staging/*` dallarına sadece pipeline yazar.
- Yayımlanmış kitaplarda düzeltme, `main`'e açılan PR'larla yapılır (okuyucu
  önerileri bu yoldan gelecek). Yayın biçimini (ilk satır başlık, sonra
  gerisi) bozmamaya dikkat edin; dosya adı ve numarasını değiştirmeyin.
