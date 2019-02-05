---
layout: post
title:  "Tesseract ile basit OCR işlemi"
date:   2018-09-27 10:03:44 +0300
author: Mehmet Aydın
comments: true
categories: Genel
tags: [md, tesseract, ocr, data, science, analytics, image, process]
---
Merhabalar,

Bu yazımda, kısaca [Tesseract](https://github.com/tesseract-ocr/) kütüphanesini kullarak nasıl basit bir [OCR](https://en.wikipedia.org/wiki/Optical_character_recognition) işlemi yapabileceğimizden bahsetmek istiyorum. Hem internette araştırma yapanlara hem de kendime not olarak buraya bırakayım :)


Kütüphane temel olarak, verilen bir imaj dosyasını tarayarak üzerindeki karakterleri bulmanızı sağlıyor. Dünyada bulunan tüm dilleri düşündüğünüzde aslında bu işlemin ne kadar zor olduğunu görebilirsiniz. Örneğin İngilizce karakterleri yüksek doğrulukla bulması Türkçe için yeterli olmayacaktır (İ, Ş, Ğ, Ü, Ç, Ö karakterleri sebebiyle). Bu kütüphane yaklaşık olarak 130 tane dil paketi içeriyor. Bu sayede arapça, vietnamca, uygurca, vs.. gibi dillere ait karakterleri de sorunsuz tarayabiliyorsunuz.

Desteklenen dillerin listesine [buradan](https://github.com/tesseract-ocr/tesseract/wiki/Data-Files) ulaşabilirsiniz.

## Kurulum

Önceden build edilmiş bir versiyonunu repolardan basitçe kurabilirsiniz.

Bu işlem ile kütühanenin release olmuş son sürümü yani 3.04 ün kurulumu gerçekleştirilecektir.

`sudo apt install tesseract-ocr`

veya

`yum install tesseract-ocr.x86_64`

Burada benim istediğim tesseractın henüz release olmamış 4.0 versiyonunu kurmak. Bu versiyon ile gelen [LSTM](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) özelliğini denemek isterim açıkçası.

Bunun dışındaki yeni eklenen özellikleri [sürüm notlarından](https://github.com/tesseract-ocr/tesseract/wiki/ReleaseNotes) okuyabilirsiniz.


#### Docker

Docker kullanımına aşinaysanız, bu versiyon için hazırlanmış imajları kullanabilirsiniz.

İmajı indirip kendiniz compile etmek isterseniz

`docker pull tesseractshadow/tesseract4cmp`

Hazır compile edilmiş imajı kullanmak için

`docker pull tesseractshadow/tesseract4re`

Bu işlemlerin nasıl veya neden yapıldığından bahsetmek istemiyorum bu yazıda. Docker nedir, ne yapar gibi bilgileri internet üzerinde aratıp bulabilirsiniz.

Kısaca Runtime Environment imajı üzerinde yapılması gereken işlemleri ekran görüntüleri ile aşağıda belirttim.

Docker imajını kurmak için;

![DockerPull](/assets/uploads/2019/02/docker1.png "DockerPull")

Dockerı çalıştıralım

`docker run -dt --name t4re tesseractshadow/tesseract4re`

![DockerRun](/assets/uploads/2019/02/docker2.png "DockerRun")

#### Centos 7

Kurulum için komutlar aşağıdaki gibidir.

`yum-config-manager --add-repo https://download.opensuse.org/repositories/home:/Alexander_Pozdnyakov/CentOS_7/`

`sudo rpm --import https://build.opensuse.org/projects/home:Alexander_Pozdnyakov/public_key`

`yum update`

`yum install tesseract`

İlave dil paketlerini ise aşağıdaki şekilde kurabilirsiniz.

`yum install tesseract-langpack-tur`




#### Centos 6

Centos 6 ile kurulum Centos 7 e göre biraz daha meşakatli. Bunun temel sebebi ise tesseractin imaj işleme için ihtiyaç duyduğu Leptonica kütüphanesinin built-in olarak gelmemesi.

Kurulum aşamaları Centos 7 ile birebir aynı. Sadece öncesinde Leptonica, autoconf-archive ve C++11 önceden kurulması, build edilmesi ve yüklenmesi gerekiyor.

Bu aşamaları tek tek yazmayacağım. Aşağıdaki bağlantıda tüm komut ve aşamalar net olarak belirtilmiş durumda.


https://www.tekovic.com/installing-tesseract-ocr-40-on-centos-6


## Sonuç

Örnek testimizi docker üzerinde yapabiliriz. Bu işlemde internette bulduğum ilk imajı deneyerek yapacağım.

Aşağıdaki işlemi herhangi bir unix/macos veya windows üzerine kuracağınız docker imajı ile veya manuel kurulum yaptığınız tesseract ile yapabilirsiniz.

Dockera örnek bir imaj kopyalayıp testimizi yapalım:

`docker cp testocr.png t4re:/home/work/testocr.png`

`docker exec -it t4re /bin/bash`

![DockerTest](/assets/uploads/2019/02/docker3.png "DockerTest")


Sonuçlar bence fena sayılmaz :)

![DockerResult](/assets/uploads/2019/02/resultocr.jpg "DockerResul")


Şimdi işi biraz zorlaştırıp, Türkçe imajlar ile devam edelim. Kullandığımız docker imajı içerisinde bu dil paketi otomatik geliyor. Manuel kurulum yaptıysanız aşağıdaki komut ile 4.0 için train edilmiş dil paketini indirebilirsiniz.

`wget https://github.com/tesseract-ocr/tessdata/raw/master/tur.traineddata`

bu dosyayı ise `/usr/local/share/tessdata/` pathine taşımalısınız.


**Örnek İmaj:**

![TrTest](/assets/uploads/2019/02/tr_test.png "TrTest")

**Sonuç:**

![TrResult](/assets/uploads/2019/02/tr_result.png "TrResult")

Türkçe için de gayet güzel sonuçlar alabildiğimizi görüyoruz.

Aslında şimdi aklıma geldi, hem kendi hem de insanlığın kullanımı için [Tesseract](https://github.com/tesseract-ocr/) kullanarak basit bir uygulama hazırlayabilirim.

Dil Seçimi -> İmaj Seç / URL -> Run OCR

şeklinde basit bir kurgu ile bazı durumlarda benim de işime yarayabilir. Yaptığımda bu postu editlerim :)

Sevgiler
Mehmet;
