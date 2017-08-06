---
layout: post
title:  "Laravel Sosyal Medya Giriş işlemleri"
date: 2017-08-06 18:00:00 +0300
categories: Laravel
permalink: /:title/
author: [Barış Esen]
tag: [laravel, facebook, twitter, google, login, php, laravel facebook login, facebook giris, giris, social, laravel social]
---
# Laravel Social
Günümüzde sosyal medya hesaplarımızı diğer uygulama giriş için aktif olarak kullanıyoruz. Kullanıcılar için oldukça büyük rahatlık sağlıyor. Bende kişisel olarak bir uygulamayı kullanacağım zaman email adresimi yazmak yerine var ise direk sosyal medya hesaplarım ile giriş yapmayı tercih ediyorum. Bu yazıda laravel ile geliştirilen bir uygulamaya sosyal medya hesaplarıyla giriş işlemini nasıl uygulayacağımızı anlatmaya çalışacağım. Bu işlem için offical paketler arasında yer alan Laravel/Social ı kullanabiliriz. [Laravel Social github](https://github.com/laravel/socialite) adresinden detaylara bakabilirsiniz.

## Laravel ile Facebook Login -Giriş- işlemi
İlk olarak social paketini projemize composer yardımıyla dahil ediyoruz.
```sh
composer require laravel/socialite
```
Daha sonra projemizi bir editör yardımıyla açıyoruz. Projemizin config klasörü içerisinde ki app.php dosyasını açıyoruz. Ve aşağıdaki satırı providers arrayi içine ekliyoruz.
```php
Laravel\Socialite\SocialiteServiceProvider::class,
```
Kısaca providers görünümü şu şekilde olucak
```php
'providers' => [
    // Diğer providers...
    Laravel\Socialite\SocialiteServiceProvider::class,
],
```
Providers eklemeden önce composer komutunun bitmiş olmasına dikkat etmemizde fayda var.

Yine app.php içerisinde ki aliases içerisine aşağıdaki satırı ekliyoruz.
```php
'Socialite' => Laravel\Socialite\Facades\Socialite::class,
```
Bu işlemden sonra facebook developer sayfasına girerek bir uyglama oluşturuyoruz.
Bunun için [Facebook dev](https://developers.facebook.com) adresine giriş yapıyor ve sağ yukardaki myapps butonun altından Add a new app seçeneğine tıklıyor ve uygulamamıza bir isim veriyoruz. Oluşturduğumuzda karşımızda aşağıdak gibi bir sayfa gelecektir. 

![Facebook developer login app](https://res.cloudinary.com/deuit9vp2/image/upload/v1502046606/barisesencom/laravel-social/facebook_app.png)
Daha sonra settings sekmesinden Add platform butonuna basıyoruz ve uygulamamızın çalıştığı domaini yazıyoruz. Ben localde uygulamam http://blog.dev olarak ulaştığım için bu urli yazıyorum. Eğer php artisan serve ile çalıştırıyorsanız http://localhost:8000 yazabilirsiniz.


Facebook keylerimizi aldığımıza göre kod kısmına geri dönebiliriz. Yine config klasörü içerisinde ki services.php dosyasını açıyoruz. Ve diğer servislerin altına facebook servisini ekliyoruz.
```php
'facebook' => [
    'client_id' => 'buraya facebook tan aldığınız app id yazılacak',
    'client_secret' => 'Buraya facebooktan aldığınız app secret yazılacak',
    'redirect' => 'http://blog.dev/facebook-callback',
],
```

Şimdi fcebook ve diğer giriş işlemleri için kullancağımız bir controller oluşturuyoruz. 
```sh
php artisan make:controller SocialAuthController
```

Oluşturduğum controller şu şekilde.
```php
<?php

namespace App\Http\Controllers;

use Laravel\Socialite\Facades\Socialite;

class SocialAuthController extends Controller
{
    public function facebookRedirect()
    {
        return Socialite::driver('facebook')->redirect();
    }

    public function facebookCallback(Request $request)
    {
        dd(Socialite::driver('facebook')->user());
    }
}
```
Şimdi routinglerimizi oluşturalım. Routes dosyamızın içine şu iki satırı ekliyoruz.
```php
Route::get('/facebook-redirect', 'SocialAuthController@facebookRedirect');
Route::get('/facebook-callback', 'SocialAuthController@facebookCallback');
```
Şimdi view kısmında facebook ile giriş yap butonu ekleyerek bu routinge kullanıcılarımızı yönlendirelim. Bunun için resources/views/auth/login.blade.php dosyamızı açıyoruz. Ve şu satırı ekliyoruz. Burayı kendinizce düzenleyede bilirsiniz tabiki.

```html
<a href="facebook-redirect" class="btn btn-primary">Facebook ile giriş yap</a>
```
Şimdi ilk testimizi yapabiliriz. Facebook ile giriş yap kısmına tıkladığımızda bizi facebooka yönlendirecek kabul ettiğimizde ise bir user objesine geri dönecek. Bu user objesi ile ne yapmak istediğiniz açıkçası size bırakıyorum. İsterseniz user tablonuza bir kolon daha açarak facebook_id yi tutabilir. İsterseniz başka bir tabloda tutabilirsiniz. Burası tamamen sizin kuracağınız yapıya kalıyor.

![Facebook login izin ekrani](https://res.cloudinary.com/deuit9vp2/image/upload/v1502049420/barisesencom/laravel-social/facebook-login-ekrani.png)

![Dönen yanıt](https://res.cloudinary.com/deuit9vp2/image/upload/v1502049430/barisesencom/laravel-social/donen-yanit.png)

Diğer sosyal medya hesaplarıyla login işlemide aynı mantıkla çalışmaktadır. Twitter login, github login ve google login işlemlerinide kısa bir süre sonra ekliyor olacağım.

Soru ve tavsiyeler için yorum bırakmanız yetecektir. Teşekkür ederim.
