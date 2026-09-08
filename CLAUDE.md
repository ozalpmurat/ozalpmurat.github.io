# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# Proje

Murat Özalp'in kişisel blogunun kaynağı: [https://ozalpmurat.github.io/](https://ozalpmurat.github.io/). Jekyll + **Chirpy** teması (`jekyll-theme-chirpy ~> 7.6`), GitHub Pages ile yayında.

- **İçerik dili Türkçe.** Commit mesajları da Türkçe yazılır (örn. `Tavsiyeler eklendi`).
- Blog amacı: Google'da arayınca bulunması istenen şeyler; "aspirin gibi" kısa/özet teknik notlar. Uzun tanıtım yazısı değil.
- `main` = kaynak. Yayın **CI'da otomatik derlenir** ve `gh-pages` branch'ine basılır (`tools/deploy.sh`). `gh-pages` üzerinde elle işlem yapılmaz; kaynak değişiklikleri hep `main`'e gider.
- `.nojekyll` yalnızca `gh-pages` yayını içindir (GitHub'ın derlememesi için); `main`'deki varlığı normaldir.

# Yerelde çalıştırma / komutlar

Bağımlılıklar README'deki gibi kurulur (`ruby-bundler ruby-dev`, `bundle config set --local path 'vendor/bundle'`, `bundle install`). `vendor/`, `_site/`, `.jekyll-cache/` gitignore'lıdır.

```bash
bundle exec jekyll serve        # yerel geliştirme: http://localhost:4000 (auto-regeneration açık)
bash tools/deploy.sh --dry-run  # production build + html-proofer testi (yayınlamaz)
```

- `deploy.sh` yalnızca GitHub Actions ortamında gerçekten yayınlar (`--dry-run` dışında). CI: `.github/workflows/pages-deploy.yml` → `main`'e push.
- Production build için: `JEKYLL_ENV=production bundle exec jekyll b`
- HTML doğrulama (html-proofer v5): `bundle exec htmlproofer --disable-external --no-enforce-https _site`

# Mimari / yapı

Chirpy starter yapısı: tema dosyaları (layout/include/sass dahil) gem'den gelir; repoda tema klasörlerine (`_layouts/`, `_includes/`) **kopya tutulmaz** — onları override etme. Sitenin kendi dosyaları: `_config.yml`, `_data/`, `_plugins/`, `_tabs/`, `index.html`. Önemli parçalar:

- **`_posts/YYYY-MM-DD-slug.md`** — blog içerikleri. Frontmatter: `layout: post`, `title`, `categories`, `tags`, `toc: true`. `_config.yml` defaults'ları `permalink: /posts/:title/` uygular (permalink'i değiştirme). Açılış bildirimi/not gibi öne çıkan satırlar için post gövdesinde `> Not: ...` blockquote kullanılıyor.
- **Görseller** kök **`/images/`** klasöründe tutulur; post içinde `../images/<dosya>` göreli yoluyla eklenir (post `/posts/:title/` altında render olduğu için bu kök `images/`'e çözülür). `_config.yml`'deki `avatar: /images/avatar.jpg`.
- **`_tabs/`** — sabit sayfalar: `about.md`, `archives.md`, `categories.md`, `tags.md` (frontmatter'da `icon`, `order`). `/categories/` ve `/tags/` liste sayfalarını buradaki `_tabs/categories.md` / `_tabs/tags.md` üretir; tek tek kategori/etiket arşivlerini jekyll-archives üretir (`/categories/:name/`, `/tags/:name/`).
- **`_data/`** — `locales/tr.yml` (arayüz metinleri; `lang: tr`, tema'nın resmî `tr-TR.yml`'ini ezmek için bu özel dosya kullanılır), `contact.yml` (iletişim seçenekleri), `share.yml`. `_config.yml` içi yorumlar da Türkçe.
  - `tr.yml` düzenlerken şemayı v7'ye uygun tut: tarih biçimi anahtarı `df.post.strftime` / `df.post.dayjs` / `df.archives.*` (eski `date_format:` değil).
- **`_plugins/posts-lastmod-hook.rb`** — git geçmişinden `last_modified_at` üretir; CI bu yüzden `fetch-depth: 0` kullanır.
- **Sitemap**: Kök `sitemap.xml` manuel dosyası (yalnızca gönderiler + `/about/`) nihai çıktıdır. jekyll-sitemap plugin'i `sitemap.xml` zaten varsa kendi sitemap'ini **üretmez** (yalnızca eksikse `robots.txt` ekler) — bu yüzden config'e `jekyll-sitemap.exclude` bloğu ekleme (plugin onu okumaz, manuel dosya zaten temiz tutar).

# İçerik yazarken dikkat

- Posta özgü detayları doğrudan gözlemden yaz; fiyat/ölçü gibi somut değerlerin dönemini belirt (örn. "Ocak 2026 sonu itibarıyla").
- Yeni bir post için görsel varsa önce kök `images/` altına koy, `![[açıklama]](../images/...)` biçiminde ekle.
