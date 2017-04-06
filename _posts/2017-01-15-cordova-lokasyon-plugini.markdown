---
layout: post
title:  "Cordova Location Plugin"
date: 2017-01-15 14:00:00 +0300
categories: cordova
permalink: /:title/
author: [Barış Esen]
tag: [cordova, mobil, hibrit, android, location]

---

Merhaba arkadaslar, bu yazıda cordova ile lokasyon kullanımını anlatmaya çalışacağım. Cordova ile telefonun ve tarayıcının lokasyonunu almak oldukça basit. Bunun için hazır olarak bulunan cordova location plugini ni kullanacağız. [Cordova-location-plugini](https://github.com/apache/cordova-plugin-geolocation) adresinden plugini detaylıca inceleyebilirsiniz.

Yeni bir cordova projesi açıyoruz, browser ve android platformlarını projemize dahil ediyoruz.


```sh
$ cordova create lokasyon
$ cd lokasyon
$ cordova platform add android
$ cordova platform add browser                                               

```

Projemizi oluşturduk ve kullanacağımız platformları da projeye dahil ettik. Şimdi kullanacağımız lokasyon pluginini projemize dahil edelim bunun için aşağıdaki kodu komut satırına yapıştırıyoruz.

```sh
$ cordova plugin add cordova-plugin-geolocation
```

Artık telefonumuzdan ve tarayıcımızdan konumu alabileceğiz. Kullanımı da oldukça basit. Nasıl kullanıldığının detaylı örneklerine yukarıda verdiğim linkten ulaşailirsiniz. Şimdi Atom editörü ile projemizi açıyorum. (Atom benim tercihimdir başka text editörleride kullanılabilir.)

```sh
$ atom .
```

Atom editörde bütün proje dosyasını bu şekilde açabiliyoruz. Projemizin içinde www/js konumunda ki index.js sayfasını açıyoruz.  

![Cordova index.js](https://res.cloudinary.com/deuit9vp2/image/upload/v1484476897/barisesencom/lokasyon-index-js.png)

Örnek kodumuzu " onDeviceReady: function() " fonksiyonun içine yazacağız. Aynı şekilde script etiketleri arasında html sayfamızada yazsak çalışacaktır.

```js
// onSuccess Callback
// This method accepts a Position object, which contains the
// current GPS coordinates
//
var onSuccess = function(position) {
    alert('Latitude: '          + position.coords.latitude          + '\n' +
          'Longitude: '         + position.coords.longitude         + '\n' +
          'Altitude: '          + position.coords.altitude          + '\n' +
          'Accuracy: '          + position.coords.accuracy          + '\n' +
          'Altitude Accuracy: ' + position.coords.altitudeAccuracy  + '\n' +
          'Heading: '           + position.coords.heading           + '\n' +
          'Speed: '             + position.coords.speed             + '\n' +
          'Timestamp: '         + position.timestamp                + '\n');
};

// onError Callback receives a PositionError object
//
function onError(error) {
    alert('code: '    + error.code    + '\n' +
          'message: ' + error.message + '\n');
}

navigator.geolocation.getCurrentPosition(onSuccess, onError);
```


Yukarıda ki örnek kodu kullandığımızda bize bir alert içinde konum bilgilerini paylaşacaktır.

Projemizi test etmek için komut satırından tarayıcıda çalışmasını söylüyoruz.

```sh
$ cordova run browser
```

Tarayıcımız aıldığında yukarıda bizden lokason izni istediğini göreceğiz. Allow diyerek izin veriyoruz.

![Allow location permission](https://res.cloudinary.com/deuit9vp2/image/upload/v1484476897/barisesencom/browser-allow-location.png)

İzin verdikten sonra ise alert içinde konum bilgilerimizi bize gösteriyor.

![location info on cordova](https://res.cloudinary.com/deuit9vp2/image/upload/v1484476896/barisesencom/browser-lokasyon-sonuc.png)

Aynı şekilde telefonumuza build ettiğimiz apk yı atarsak bizden önce konum izni isteyecek ve arkasından tarayıcıda gördüğümüzün aynısını verecektir.

Artık cihazın lokasyon bilgisi elinizde. Bunu ister map üzerinde gösterirsiniz ister veritabanına kaydedersiniz. :)
bir sonraki yazıda lokasyon plugini kullanılarak lokasyon bilgisinden açık adres nasıl elde edebileceğimizi anlatacağım.
