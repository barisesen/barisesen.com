---
layout: post
title:  "Cordova Android Push Notification"
date:   2016-07-01 14:41:58 +0300
categories: Cordova
permalink: /:title
---
Mobil uygulama geliştirme günümüzde oldukça popüler. Tüm platformlara uygulama geliştirmek önceleri oldukça meşaketli ve uzun süren bir süreçti. Her platforma özgü programlama dilini öğrenmeli ve daha sonra uygulamayı hayata geçirmek gerekiyordu. Cordova bu durumda yardımımıza koşuyor.

### Cordova Nedir ?

**Cordova**, Html, Javascript ve css yardımıyla web tabanlı mobil uygulamalar geliştirmemizi sağlayan bir uygulama geliştirme çatısıdır(framework).

Cordovanın ne olduğunu öğrendiğimize göre asıl konumuza geçebiliriz..

Mobil uygulamamızı kullanıma sunduktan sonra kullanıcıya uygulamamızı kullandırmamız gerekir. Bunuda en kolay kullanıcıya belli bildirimler göndererek bizi hatırlamısını sağlayabiliriz. Ayrıyetten uygulamamızda bildirim sistemi varsa kullanıcı bildirimi aldığı anda o bildirimden haberdar olmak isteyecektir.

Hali hazırda cordova pluginleri sayesinde uygulama açıkken bildirim gönderebiliyoruz. Ama uygulama kapalı durumdayken bildirim göndermemiz gerekirse push notification servislerine ihtiyacımız olacaktır.

Yazının devamında Google Cloud Messaging servisini kullanarak php yardımıyla push notification göndermeyi anlatacağım.

Öncelikle [Google developer console](https://console.developers.google.com/) adresine giriyoruz. Bizi aşağıdaki gibi bir ekran karşılayacaktır.

[![](https://i1.wp.com/barisesen.com/wp-content/uploads/2016/07/google-dev-console.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/google-dev-console-2/)

Daha sonra create a project yazan yere basarak yeni bir proje oluşturuyoruz.

[![](https://i1.wp.com/barisesen.com/wp-content/uploads/2016/07/create-new-projeckt.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/create-new-projeckt/)

Açılan ekrana proje adımızı yazıyor ve create butonuna basıyoruz. Projemiz oluşturulduktan sonra Google Cloud Messaging yazan linke tıklıyor ve GCM’i aktifleştiriyoruz.

[![](https://i1.wp.com/barisesen.com/wp-content/uploads/2016/07/gcm-enable-300x169.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/gcm-enable/)

Gcm aktifleştirildikten sonra sol taraftaki panelden Credentials’ a tıklıyoruz.

[![](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/07/credentials.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/credentials/)

Açılan sayfada Create Credentials’a tıklıyor ve API Key’i seçiyoruz. Açılan menüden Server key seçeneğine tıklıyoruz. İstediğimiz ismi yazarak Create butonuna basıyoruz.

[![](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/07/server-key-sec-1.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/server-key-sec-2/)

Karşımıza çıkan Api keyi kopyalıyoruz.(Php ile yazacağımız server kodumuzda lazım edecektir.)  
Sol yukarıda Google Apis’in yanındaki menüden IAM & Admin sayfasına geçiyoruz.

[![ıam admin e git](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/07/ıam-admin-e-git.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/iam-admin-e-git/)

Menudeki Settings’e tıklıyoruz ve karşımıza çıkan Project number’ı kopyalıyoruz. **Project numberı** **Sender Id** olarak kullanacağız.

[![](https://i0.wp.com/barisesen.com/wp-content/uploads/2016/07/copy-project-number.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/copy-project-number/)

Google Developer console’da ki işlemlerimizi tamamlamış olduk. Şimdi sıra cordova projesi oluşturup gerekli plugini dahil etmeye geldi.

Projeyi açacağımız dizine gelerek

```sh
$ cordova create projeAdi
```

[![](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/07/cordova-projesi-oluştur.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/cordova-projesi-olustur/)

[Cordova push notification pluginini](https://github.com/phonegap/phonegap-plugin-push) projemize dahil ediyoruz.

```sh
$ cordova plugin add https://github.com/phonegap/phonegap-plugin-push --variable SENDER_ID="XXXXXXX"
```

Sender_Id olarak yani xxxxx yerine kopyaladığımız project numberı yazıyoruz.

[![](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/07/plugini-ekledik-300x169.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/plugini-ekledik/)

Projemize android platformunu ekliyoruz.

```sh
$ cordova platform add android
```
Şimdi projeyi kullandığımız editör yardımıyla açıyoruz ve index.js’de bir kaç ekleme yapacagız. İndex.js deki onDeviceReady fonksiyonun içine aşağıdaki kodları kendiimize göre düzenleyerek ekliyoruz.

```javascript
var push = PushNotification.init({ "android": {"senderID": "297336031131"},
            "ios": {"alert": "true", "badge": "true", "sound": "true"}, "windows": {} } );

        push.on('registration', function(data) {

            /*
             * Registration id bize cihaza ait uniq bir değer döndürecektir hangi cihaza
             * bildirim atacağımızı bu id sayesinde ayırt edecegız.               
             */

            alert(data.registrationId);

        });

        push.on('notification', function(data) {
            console.log(data.message);
            alert(data.title+" Message: " +data.message);
            // data.title,
            // data.count,
            // data.sound,
            // data.image,
            // data.additionalData
        });

        push.on('error', function(e) {
            console.log(e.message);
        });
```

<span style="text-decoration: underline;">Sender id yerine kendi proje numaranızı yazmaya unutmayınız.</span>

[![](https://i0.wp.com/barisesen.com/wp-content/uploads/2016/07/cordova-build.png?resize=300%2C169&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/cordova-build/)

Uygulamayı build edip çalıştıdığımızda bize alert içinde bir id verecektir bu id o cihaz için tanımlanan uniq bir değerdir ve bildirimleri o id aracılığı ile göndereceğiz.

[![](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/07/Screenshot_2016-07-01-10-26-45.png?resize=169%2C300&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/screenshot_2016-07-01-10-26-45/)

Şimdi sırada bu id yi kullanarak bildirimi göndermeye geldi.Bunun için php kullanarak bir servis yazacağız.Bunun için aşşağıdaki kod bloğunu kullanabilirsiniz.

```php
<?php
// API access key from Google API's Console
define( 'API_ACCESS_KEY', 'AIzaSyAw_zFP342R6E30y5xG7jMwkK2XNi9x5LI');

//alert ile aldığımız id buraya gelecek array olarak alıyoruz.birden fazlada gönderilebilir.
$registrationIds = ['eIC_TuOVLbo:APA91bHIqx7h_G2cmF82VEMZ0FLVAQPuB0F5MmPSHhPJjEbjW4SDOtDJGDBmZF3v0kzetn3tiQeVZVT1wQ2HX2hRZlH5oiteQ5ddSk3I3DEM1khOcX2I-UTbk0H33emNqJc7-F6_fqDD'];

// prep the bundle
$msg = array
(
	'body' 	=> 'Push notification test',
	'title'		=> 'Hello world',
	"content_available"=> true
);
$fields = array
(
	'registration_ids' 	=> $registrationIds,
	'data'	=> $msg,
	'priority'    => 'high'
);

$headers = array
(
	'Authorization: key=' . API_ACCESS_KEY,
	'Content-Type: application/json'
);

$ch = curl_init();
curl_setopt( $ch,CURLOPT_URL, 'https://android.googleapis.com/gcm/send' );
curl_setopt( $ch,CURLOPT_POST, true );
curl_setopt( $ch,CURLOPT_HTTPHEADER, $headers );
curl_setopt( $ch,CURLOPT_RETURNTRANSFER, true );
curl_setopt( $ch,CURLOPT_SSL_VERIFYPEER, false );
curl_setopt( $ch,CURLOPT_POSTFIELDS, json_encode( $fields ) );
$result = curl_exec($ch );
curl_close( $ch );
echo json_encode($result);
```

Yukarıdaki kodu çalıştıdığınızda telefonunuzda tatlı bir ses ve biraz titreşim duymalısınız 🙂

[![](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/07/Screenshot_2016-07-01-08-49-09.png?resize=169%2C300&ssl=1)](https://barisesen.com/index.php/cordova-android-push-notification/screenshot_2016-07-01-08-49-09/)

Sonuna kadar okuduğunuz için teşekkür ederim. Beni takip etmek istersen [GİTHUB](https://github.com/barisesen)
