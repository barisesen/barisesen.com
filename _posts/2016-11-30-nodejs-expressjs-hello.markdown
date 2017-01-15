---
layout: post
title:  "Node.js Express.js Hello World"
date: 2016-11-30 17:50:58 +0300
categories: nodejs
permalink: /:title
author: [Barış Esen]
tag: [nodejs, "hello world", expressjs]

---

Bir önceki yazıda, NodeJs ile konsola "hello world" yazdırmıştık. Şimdi ExpressJs kütüphanesini kullanarak http de NodeJs'e merhaba dedirteceğiz. Kısaca ExpressJs, NodeJs için yazılmış, hızlı ve minimalist bir web frameworküdür. Framework, geliştiricilere projelerinde kullanacakları sınıfları, eklentileri vs. toplu bir şekilde sunulması denebilir.

Hızlıca başlamak gerekirse, öncelikle bir NodeJs projesi oluşturuyoruz. Bunun için bir klasör oluşturup konsol yardımıyla klasörün içine giriyoruz ve npm i burada başlatıyoruz.


```sh
 $ npm init
```


Bize burada bir kaç soru soracak, onları doldurup devam ediyoruz. Projemizin klasörüne package.json adlı bir dosya oluştuğunu göreceksiniz. Bu dosya bizim NodeJs projemizin hangi paketleri kullandığını, projemizin ismini, projeyi oluşturan kişiyi ve github reposunun linkini tutacağı yer. Biz projemize her yeni paket yüklediğimizde burası otomatik bir şekilde yenilenecek. Bu durumun sebebi projeyi başka bir bilgisayarda veya sunucuda çalıştırmamız gerektiğinde rahatça o paketleri otamatize bir halde yeniden yüklememizi sağlayacaktır.

Şuanda elimizde bir NodeJs projesinin başlangıcı var. Buraya bizim ExpressJs kütüphanesinin
 dahil etmemiz gerecek bunun için konsola,


```sh
 $ sudo npm install express --save
```


yazıyoruz. Yukarıda ki komutumuz da dikkat çeken bir yer "--save" burada express kütüphanesini package.json dosyamıza dependencies içerisine kaydedicektir.

Express'i kurduğumuza göre artık kullanabiliriz. Bunun için aşağıdaki kodu oluşturacağımız index.js adlı dosyaya yazmamız yeterli olacaktır.



```js
  var express = require('express')
  var app = express()

  app.get('/', function (req, res) {
    res.send('Hello World!')
  })

  app.listen(3000, function () {
    console.log('Example app listening on port 3000!')
  })
```


Yukarıda ki kod ile ne yaptığımızı açıklamak gerekirse, express kütüphanesini kullanacağımız için onu index.js' e çağırdık.
Daha sonra express kütüphanesini app adlı değişkene atadık.


```js
  app.get('/', function (req, res) {
    res.send('Hello World!')
  })
```


komutu ile ana dizine bir GET isteği yapıldığında sayfaya "Hello World" yazacaktır.


```js
  app.listen(3000, function () {
    console.log('Example app listening on port 3000!')
  })
```


Burada ise http serveri 3000 portunda çalışmaya başlamasını söylüyoruz ve başladığında konsola "Example app listening on port 3000!" yazacaktır.

Uygulamayı çalıştırmak için konsola,


```sh
$ node index.js
```


yazmamız yeterli olacaktır. Şimdi tarayıcımızdan localhost:3000' e gittiğimizde ekranda Hello World yazdığını göreceğiz.

![localhost:3000 hello world](https://res.cloudinary.com/deuit9vp2/image/upload/v1480858664/barisesencom/localhost3000.png)
