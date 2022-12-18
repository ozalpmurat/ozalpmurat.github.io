---
layout: post
title: Linux'ta Envision EBYS ile e-imza atma. Kesin çözüm.
categories:
  - Bilişim
tags: envision e-imza linux
published: true
toc: true
---

# Neden bu yazı?
Linux'ta e-imza sorunları ile uğraşmaktan bıktım. Uzun bir süre uğraşmamayı düşünüyorum. Artık kendisi çalışırsa kullanacağım, çalışmazsa da bu çözümü uygulayacağım.

Daha önceki "Linux'ta e-imza" konulu yazılarımı [Bilecik Üniversitesi blog sitemde](http://web.bilecik.edu.tr/murat-ozalp?s=linux+e-imza) okuyabilirsiniz. Yazıların girişinde göreceğiniz gibi, yaptıktan bir süre sonra çözümler iptal olmuş.

Bu sefer çoook uzun süre iptal olmayacak bir çözüm paylaşacağım: **Sanal makinedeki Windows üzerinden imza atmak** :( :( :(

# Güncel durum
Pardus'ta imza atabildiğini söyleyen Hasan Abi'den ondaki ilgili paketlerin sürüm bilgisini aldım, aynısını kendi bilgisayarımda (Ubuntu) da denedim, olmadı. Her bileşenin bir önceki sürümlerini teker teker denedim, olmadı. Son olarak Pardus'u denemek istedim. Live USB ile açıp Pardus'ta ilgili bileşenleri yükleyip denedim yine olmadı.

Bilgisayarımdaki güncel durumda ilgili bileşenlerin sürümlerini aşağıda ekliyorum.

```bash
murat@BSEU0793:~$ dpkg-query -l | egrep -i "akis|akia|pcsc|libacs|envision|jre|python2" | cut -c5-71
akia                                       6.4.1                   
akis                                       2.0                     
default-jre                                2:1.11-72build2         
default-jre-headless                       2:1.11-72build2         
envision.client.service                    6.4                     
libacsccid1:i386                           1.1.8-1~ubuntu18.04.1   
libpcsclite1:amd64                         1.9.9-1                 
libpython2.7-minimal:amd64                 2.7.18-13ubuntu2        
libpython2.7-stdlib:amd64                  2.7.18-13ubuntu2        
openjdk-11-jre:amd64                       11.0.17+8-1ubuntu2      
openjdk-11-jre-headless:amd64              11.0.17+8-1ubuntu2      
openjdk-8-jre:amd64                        8u352-ga-1~22.10        
openjdk-8-jre-headless:amd64               8u352-ga-1~22.10        
pcscd                                      1.9.9-1                 
python2.7                                  2.7.18-13ubuntu2        
python2.7-minimal                          2.7.18-13ubuntu2        
```

Denediklerim:
- Python'ın, JRE'nin, JDK'nın bulabildiğim bütün sürümleri
- Farklı farklı tarayıcılar (Chrome, Brave, Vivaldi, Firefox)
- envision istemcisinin arşivlediğim eski sürümleri
- Tarayıcıyı konsolda başlatarak debug modda bir şey yakalamaya çalışmak
- Log dosyalarını incelemek

Mevcut durum:
- E-devlet'e e-imza ile girebiliyorum
- Akis/Akia uygulaması çalışıyor, kartın pinini girerek kart içine girebiliyorum.
- SertifikaDeposu.svt ile ilgili işlemleri yaptım.
- Envision EBYS'de tarayıcıdan "imzala" düğmesine basınca bekliyor bekliyor bekliyor...

# Uzun süreli, kalıcı, garantili, sağlam çözüm

Yapılacak olan işlem adımları:
- VirtualBox'a Bir tane Windows kur. Ben 7'de sorunsuz çalıştırdım.
- VirtualBox'ta USB kart okuyucuyu sanal Windows'a bağlamak için ilgili ayarları yap.
- Sanal Windows'u aç. Kart okuyucuyu USB porta takınca, Windows içinde "yeni sürücü yükleniyor" zımbırtısı çıkacak.
- Bundan sonrası, "Windows'ta Envision için klasik e-imza kurulumu"

## Ubuntu üzerinde, VirtualBox'ta USB kart okuyucuyu sanal Windows'a bağlamak
İki işlem yapılacak:

### 1. Ubuntu'da USB yetkisi
Önce şu komutu girerek, senin kullanıcını ilgili gruba dahil ederek VirtualBox USB erişim yetkisini ver:
```bash
sudo usermod -aG vboxusers $USER
```

Ubuntu'yu yeniden başlat.

### 2. VirtualBox'ta hangi USB cihazın sanal makineye takılacağının seçilmesi
VirtualBox'a geçerek, hangi USB cihazların sanal makineye takılabileceği ile ilgili filtreleri gir. Aşağıda ekran görüntüsünde gösterdim:

![](/images/2022-12-09-linux-envision-eimza-kesincozum.png)
_VirtualBox VM ayarları. USB sekmesi_

Görselde görüldüğü gibi, USB sekmesinde bir filtre girilmesi gerekiyor. Bu filtreye uyan USB aygıtları, VM içerisinde kullanabileceğiz.

- Görselde (1) ile işaretlenen simgeye basılırsa tüm USB aygıtları içeren bir jenerik filtre ekleniyor. Ekranda benim bu amaçla oluşturduğum filtremin ismi "Hepsi".
- Görseldeki (2) ile işaretlenen simgeye tıklayınca, fiziksel bilgisayarımızdaki USB cihazları listeliyor. Birisini seçince sadece ona özel bir filtre oluşturuyor.
- Görselde (3) ile işaretlenen kısımda, (2) numaralı simgeye bastıktan sonra listeden benim ACS kart okuyucumu seçerek otomatik oluşturulan filtrenin detaylarını görüyoruz.

Bitti. Güle güle kullanın.