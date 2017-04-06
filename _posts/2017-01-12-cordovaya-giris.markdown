---
layout: post
title:  "Cordovaya Giriş"
date: 2017-01-12 10:00:00 +0300
categories: cordova
permalink: /:title/
author: [Barış Esen]
tag: [cordova, mobil, hibrit, android, "hello world"]

---
Mobil uygulama geliştirme günümüzde oldukça popüler. Tüm platformlara uygulama geliştirmek önceleri oldukça meşaketli ve uzun süren bir süreçti. Her platforma özgü programlama dilini öğrenmeli ve daha sonra uygulamayı hayata geçirmek gerekiyordu. Cordova bu durumda yardımımıza koşuyor.

# Cordova Nedir ?
Cordova, Html, Javascript ve css yardımıyla web tabanlı mobil uygulamalar geliştirmemizi sağlayan bir uygulama geliştirme çatısıdır(framework). Detayli bilgi için [erdemoflaz.com](http://www.erdemoflaz.com/cordova-ile-mobil-uygulama-gelistirme/).


Cordova'yı işletim sistemimize kurduktan sonra işletim sistemimizin komut satırını açıyoruz. Komutlarımızı buraya yazacağız. İlk komutumuz projeyi oluşturmayı sağlayacak. Bunun için aşağıdaki kodu komut satırına yapıştırıyoruz.


```sh
$ cordova create merhaba-dunya
```

![cordova-merhaba-dunya](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/cordova-merhaba-dunya.png)


Bulunduğumuz dizinde merhaba-dunya isimli bir klasör oluşacaktır. Bu klasör bizim projemizin dosyasıdır. Onun için cd komutunu kullanarak klasörün içine giriyoruz.

```sh
$ cd merhaba-dunya
```


Android uygulaması oluşturmak için bilgisayarımızda android-sdk nın kurulu olması gerekiyor. Bu kurulumu yaptıktan sonra projemize android platformunu ekleyebiliriz.

```sh
$ cordova platform-add android
```


![cordova-platform-add-android](https://res.cloudinary.com/deuit9vp2/image/upload/v1484182048/barisesencom/platform-add-android.png)

Aynı zamanda cordova yaptığımız değişiklikleri browserda da görüntülememiz mümkün bunun için browserıda platform olarak eklememiz gerekiyor. Daha sonra browser üzerinde projemizi çalıştırabiliriz.

![cordova-platform-add-browser](https://res.cloudinary.com/deuit9vp2/image/upload/v1484182052/barisesencom/cordova-browser.png)

```sh
$ cordova platform-add browser
$ cordova run browser
```

Browserda bizi karşılayan ekran cordovanın proje oluşturulurken oluşturduğu standart ekranı.
Bir de proje klasöründe ki dosyaları inceleyelim. Bizim için en önemli dosya " www " klasörü. Bütün kodumuzu buraya yazacağız.


İndex.html projemizin index sayfasını temsil ediyor buraya html kodlarını yazıyoruz.
![cordova-folder](https://res.cloudinary.com/deuit9vp2/image/upload/v1484182052/barisesencom/index-folder.png)

Js/index.js projede ki javascript kodlarımızı yazabileceğimiz alan. Javascript kodlarını index.html'e de yazsak çalışacaktır. Ama daha düzenli bir proje için index.js içine yazılması tercih edilmelidir.

![cordova-folder](https://res.cloudinary.com/deuit9vp2/image/upload/v1484182051/barisesencom/index-js.png)

Birde proje ayarlarımızın kaydedildiği config.xml dosyamız var.

![cordova-config](https://res.cloudinary.com/deuit9vp2/image/upload/v1484182050/barisesencom/config.png)

Dosyalarımızı görüntülediğimize göre projemizi androide derleyebiliriz. Bunun içi komut satırına aşağıdaki kodu yazıyoruz.

```sh
$ cordova build android
```

![cordova-build-android](https://res.cloudinary.com/deuit9vp2/image/upload/v1484182045/barisesencom/build-android1.png)
![cordova-build-android](https://res.cloudinary.com/deuit9vp2/image/upload/v1484182045/barisesencom/build-android2.png)

Oluşan apk bulunduğumuz dizin içinde /platforms/android/build/outputs/apk/ dizininde android-debug.apk adında bizi bekliyor :)

![cordova-apk](https://res.cloudinary.com/deuit9vp2/image/upload/v1484182044/barisesencom/apk.png)

Bu apkyı telefonumuza yükleyerek uygulamamızı telefondan test edebiliriz.

<p><img src="https://res.cloudinary.com/deuit9vp2/image/upload/v1484184635/barisesencom/cordova-apk.png" alt="apk-on-phone" width="300px"/></p>

Daha sonra ki yazılarımda pluginlerden bahsedip, sık kullanılan pluginlerin kullanımından bahsedeceğim.
