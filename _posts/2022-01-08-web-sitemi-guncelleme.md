---
layout: post
title: Github Pages üzerindeki Chirpy template'li Jekyll sitem nasıl güncellenir?
categories:
  - Bilişim
tags: github jekyll
published: true
toc: true
---
# Git klasörünü indirdikten sonra, yerel bilgisayarda çalıştırmak
Gerekli bileşenleri indirip kurabilmek içinm, Ubuntu deposundan ön gereklilikleri kuralım:
```sh
sudo apt-get install ruby-full build-essential
```

Ubuntu deposundan jekyll yükleyelim:
```sh
sudo apt install ruby-jekyll-watch jekyll
```

Ruby Bundle bileşenlerini kullanmak için, bunları sistem geneline değil, kendi kullanıcı klasörüme kurmasını istiyorum:
```sh
bundle config set --local path 'vendor/bundle'
```

Projede jekyll kullanılacağını belirtelim. Gereklilikleri anlasın:
```sh
bundle add jekyll
```

Gereklilikleri kuralım:
```sh
bundle install
```

Web sitesini yerelde çalıştır:
```sh
bundle exec jekyll serve
```


# Siteyi güncelleme
Birkaç farklı yöntem var.

# Yerel bilgisayarda güncelleyip GIT ile publish etme
Kaynak: <https://chirpy.cotes.page/posts/getting-started/>

**Yerelde Çalıştırma**
Önce kontrol et. Sorun yoksa, publish et.
```sh
bundle exec jekyll serve
```
**Github Pages'e yükleme**
Önce düzenlemeleri yap, sonra commit, sonra push:
```bash
git add .
git commit -m "açıklama..."
git push
```

# Doğrudan web üzerinden ekleme
Github içindeki ilgili repo'daki `_posts` klasörü altında özel formatta tarih ile başlayan dosyayı doğrudan editleyebilirsin, yeni dosya ekleyebilirsin.

# Editör ile güncelleme.
Markdown ile uğraşmak istemiyorsan, online-offline bir sürü editör varmış, bunlar kullanılabilir. Örneğin bu postu editlediğim <http://prose.io> var. Ana sayfasında github'dan erişim istiyor. Yetki verilince doğrudan repolara göz atıp psot düzenleme yapılabiliyor. Arayüzü de yine markdown biçiminde ama birkaç tane de toolbox içinde düğme var. Preview özelliği de var. Aşağıda bu yazıyı yazarkenki kısmın ekran görüntüsü var:

![prose.io yazım ekranı](/images/2022-01-08-prose.io.png)

# Github Pages'te desteklenen Jekyll temaları
Şurada yazmışlar: <https://pages.github.com/themes/>