---
layout: post
title: Kategorik veri türlerini yapay sinir ağına gönderme
categories: [Yapay Zeka]
tags: ysa veri

toc: true
katex: true  # if you're using math
---

Temel olarak 3 tür kategorik veri var:
* **Binary**. evet/hayır, dişi/erkek
* **Nominal**. Aralarında sıralı matematiksel ilişki bulunmayan veriler. Saç rengi, araba markası, şehir, vb.
* **Ordinal**. Aralarında sıra anlamında ilişki var. Kötü/Orta/İyi/Süper, not (A,B,C,D,F), Motor hacmi - CC (<1000, 1001-1500, 1501-2000, >2001)

Ordinal verilerde, doğrudan her kategori için bir tamsayı eşleştirip sisteme (YSA mesela) gönderilebilir. Zaten sıralı veriler olduğu için çok ta güzel olur. Buna label encoding, ordinal encoding, integer encoding gibi isimler vermişler.

Nominal ve Binary verilerde ise tamsayı eşleştirip bunu sisteme göndermek problem olabilir. Örneğin TCP ve UDP için 7 ve 8 numarası ile kodlama yaptığımızı varsayalım. Sistem 7,5 gibi arada bir değer verirse bunun bizim için hiç bir anlamı olmayacak. Oysa ordinal verilerde ara değerler de çıkarım için anlam ifade etmektedir.  Nominal veriyi sıralı kodlama yaparak sisteme gönderirsek, istemeden "sıralama"ya da anlam katmasına sebep olabilir. Bu sorunun önüne geçmek için [One Hot Encoding](https://womaneng.com/one-hot-encoding-nedir-nasil-yapilir/ "OHE") kullanılmasını önermişler.

Açıklamalarım ve alıntılar için kaynak: <https://machinelearningmastery.com/one-hot-encoding-for-categorical-data/>
>
> For categorical variables where no ordinal relationship exists, the integer encoding may not be enough, at best, or misleading to the model at worst.
>
> Forcing an ordinal relationship via an ordinal encoding and allowing the model to assume a natural ordering between categories may result in poor performance or unexpected results (predictions halfway between categories).
>
> In this case, a one-hot encoding can be applied to the ordinal representation. This is where the integer encoded variable is removed and one new binary variable is added for each unique integer value in the variable.

 
Ordinal ve nominal veriler (ve diğerleri) konusunda daha fazla okuma yapmak isteyen olursa, konunun ismi: "[Levels of measurement](https://www.google.com/search?client=firefox-b-d&q=Levels+of+measurement)". Aşağıda da güzel bir sınıflandırma var. Görseli aldığım adres: <https://www.advanceinnovationgroup.com/blog/data-types>

![Veri Türleri]({{ site.baseurl }}/images/2022-01-07-kategorik-veriler.png)

