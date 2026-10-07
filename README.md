# eptran-library

Eptran'ın bitmiş çevirilerinin kütüphanesi. İçerik pipeline (ononoki-dp) tarafından yönetilir, elle düzenlenmez.

## Yapı

```
<seri-slug>/
  index.json
  <cilt-slug>/
    001.txt
    002.txt
    ...
```

- Bölüm dosyaları sıfırla doldurulur (001, 002, ...) ki sıralama bozulmasın.
- Sadece çeviri tutulur, orijinal metinler bu repoda saklanmaz.
- EPUB dosyaları repoda değil, Release'lerde durur.
- Süreç devam ederken içerik `staging/<slug>` branch'indedir, eptran'daki PR merge olunca `main`'e alınır.

## index.json (taslak)

```json
{
  "seri": "ornek-seri",
  "ad": "Örnek Seri",
  "ciltler": [
    {
      "slug": "cilt-01",
      "bolum_sayisi": 12,
      "epub": "https://github.com/dpentx/eptran-library/releases/download/<tag>/<dosya>.epub"
    }
  ]
}
```
