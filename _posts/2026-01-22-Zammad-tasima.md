---
layout: post
title: Zammad Taşıma Günlüğü
tags: Genel
categories: [Genel]

toc: true
---

# Zammad Taşıma Günlüğü: Docker'dan Paket Kurulumuna Geçişte Karşılaşılan Engeller ve Çözüm Yolları

Zammad sistemini bir ortamdan diğerine (örneğin Docker’dan Virtualbox paket kurulumuna) taşımak, bazen beklenmedik teknik zorlukları beraberinde getirebilir. Bu rehberde, bir migrasyon sonrası dış dünyayla bağlantısı kopan ve yönetici erişimi kilitlenen bir Zammad sistemini nasıl hayata döndürebileceğinizi adım adım inceleyeceğiz.

## Giriş Kapısı Kapalıysa: Rails Konsolu ile Müdahale

Docker tarafında etkinleştirilen harici kimlik doğrulama (LDAP, SAML vb.) ayarları yeni sunucuda çalışmadığında ve yerel giriş formu gizlendiğinde sistemin dışında kalabilirsiniz. Zammad, bu tür durumlar için yöneticilere **Rails konsolu** üzerinden doğrudan veri tabanına müdahale imkanı sunar.

**Sorun:** Giriş sayfasında kullanıcı adı/şifre alanı yok, sadece çalışmayan harici butonlar var. **Çözüm:** Paket kurulumu kullanılan sistemlerde terminale `zammad run rails c` yazarak konsola bağlanın. Şu komutla yerel şifre giriş alanlarını tekrar görünür kılın:

```ruby
Setting.set('user_show_password_login', true)
```

Ardından, yönetici hesabınıza yerel bir şifre tanımlayarak erişimi garanti altına alın:

```ruby
u = User.find_by(email: 'admin@example.com')
u.update!(password: 'YeniGucluSifre123!')
```

## "CSRF Token Verification Failed!" Hatasını Aşmak

Yerel giriş formunu geri getirdikten sonra "Sign In" butonuna bastığınızda bu hatayı alıyorsanız, sorun muhtemelen yeni sunucunuzun protokol (HTTP/HTTPS) ve FQDN ayarlarının eski sistemden farklı kalmasıdır.

**Sorun:** Güvenlik katmanı, bağlantı türü uyuşmazlığı nedeniyle oturum açmanıza izin vermiyor. **Çözüm:** Konsol üzerinden protokolü ve sunucu adresini güncelleyin:

```ruby
Setting.set('http_type', 'http') # Eğer HTTPS kullanmıyorsanız
Setting.set('fqdn', 'yeni_sunucu_ip_adresi')
```

**Önemli Not:** Eğer bir proxy (Nginx) arkasındaysanız, Nginx konfigürasyonunda `proxy_set_header X-Forwarded-Proto` başlığının kullandığınız protokolle (http/https) eşleştiğinden emin olun. Değişiklik sonrası tarayıcı çerezlerini temizlemek veya gizli sekme kullanmak çakışmaları önleyecektir.

## Kaybolan Yetkiler: Yerelleştirilmiş Rol İsimleri

Sisteme giriş yaptınız ancak hiçbir yönetim paneli görünmüyorsa, rolleriniz veritabanında doğru tanımlanmamış veya LDAP senkronizasyonu nedeniyle ezilmiş olabilir. Zammad’de roller sistem diline göre yerelleştirilmiş olabilir.

**Sorun:** `u.roles = Role.where(name: ['Agent', 'Admin'])` komutu, veritabanındaki isimler farklı olduğu için (örn. Türkçe kurulum) yetki atayamıyor. **Çözüm:** Önce sisteminizdeki rollerin gerçek isimlerini ve ID'lerini sorgulayın:

```ruby
Role.pluck(:id, :name)
```

Eğer çıktı `[[5, "Sistem Yöneticisi"], [1, "Yönetici"]]` şeklindeyse, atama işlemini bu isimlerle veya doğrudan ID numaralarıyla yapmalısınız:

```ruby
u.roles = Role.where(id: [23, 24])
u.save!
```

## Son Adım: Önbelleği ve Senkronizasyonu Kontrol Edin

Yaptığınız tüm bu değişikliklerin web arayüzünde hemen aktif olması için uygulama önbelleğini boşaltmanız kritiktir:

```ruby
Rails.cache.clear
```

Ayrıca, çalışmayan bir LDAP bağlantısının rollerinizi tekrar "Kullanıcı" seviyesine çekmesini engellemek için entegrasyonu geçici olarak devre dışı bırakabilirsiniz:

```ruby
Setting.set('ldap_integration', false)
```

Özet

Başarılı bir taşıma sonrası **Yönetim Paneli > Sistem > Temel Ayarlar** kısmından FQDN ve HTTP tipi ayarlarını doğrulamayı unutmayın. Bu adımlar, kilitli kalmış bir Zammad kurulumunu kısa sürede tekrar işlevsel hale getirmenizi sağlayacaktır.
