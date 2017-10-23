---
layout: post
title:  "Laravel Browser Testi | Dusk"
date: 2017-10-24 00:00:00 +0300
categories: Laravel
permalink: /:title/
author: [Barış Esen]
tag: [laravel, e2e, browser, test, otomasyon, php, laravel test]
---

## Test nedir? Neden Test Yazmalıyız?
Test, kısaca yazdığımız kodda ki hataları kullanıcıdan önce bulmamızı kolaylıştıracak ve sistemimizi daha kolay kontrol altında tutmamıza yarayan şeydir. Sistem büyüdüğünde yaptığımız küçük değişiklikler farklı yerleri etkileyebiliyor ve o an bunları bulmamız güç oluyor. Sırtımızı testlere dayadığımızda ise herhangi bir hata olursa kodu daha deploy etmeden önce hatalarımızı yakalayabiliyoruz.
Test yazalım yazdıralım :)

## Laravel'de Browser Test Nasıl Yazılır? (Dusk Kullanımı)
Laravel kullanarak yazdığımız bir projede e2e yani browser testi yapma ihtiyacımız var ise Dusk yardımımıza koşuyor. Dusk'ı kullanmak için composer aracı ile projemize dahil ediyoruz.
```sh
composer require --dev laravel/dusk
```

Daha sonra indirdiğimiz plugini install ediyoruz.
```sh
php artisan dusk:install
```

Testleri çalıştırmak için ise
```sh
php artisan dusk
```
Dememiz yeterli olucaktır. Dikkat etmemiz gereken bir nokta .env doysamız içerisinde ki APP_URL kısmı doğru olması gerekiyor. Ben valet kullanıyorum ve proje ismimde app olduğu için
```php
APP_URL=http://app.dev
```
şeklinde tanımladım. Dusk Chrome driverını kullanıyor. Bu sebeple ayrıca Selenium vb. indirimemize gerek kalmıyor. Ayrıca default ayarında **headless** olarak çalışıyor. Bu ne demek oluyor diye düşünüyorsanız. Arka planda çalışıyor olarak düşünebilirsiniz. Test sırasında yapılan işlemleri görmek istiyorsanız, ana dizindeki test klasörü içinde ki DuskTestCase içerisinden headless parametresini kaldırabiliriz.

Örnek bir test oluşturalım. Senaryomuz oldukça basit olucak. Kullanıcı anasayfadan sisteme dahil olucak. Daha sonra login butonuna basacak, gerekli alanları doldurarak giriş işlemini tamamlayacak. Daha sonra Dashboard'da ki Posts butonuna tıklayarak postlar sayfasına ulaşacak. Son olarak yeni post oluşturmak için gerekli alanları doldurup postu oluşturacak ve işlemi tamamlayacak.

Yeni bir Test oluşturarak işleme başlayalım.
```sh
php artisan dusk:make PostTest
```

```php
    /** @test */
    public function createPost()
    {
        $this->browse(function (Browser $browser) {
            $browser->visit('/')
                    ->assertSee('Laravel')
                    ->clickLink('Login')
                    ->assertSee('E-Mail Address')
                    ->value('#email', 'b@b.b')
                    ->value('#password', '123456')
                    ->press('Login')
                    ->assertPathIs('/home')
                    ->clickLink('Posts')
                    ->assertSee('Title:')
                    ->value('#title', 'e2e Test post')
                    ->value('#body', 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ad, adipisci dicta ducimus eos exercitationem ipsam labore maiores numquam officiis reiciendis repellendus similique sunt tempora velit veniam. At autem placeat reprehenderit!')
                    ->press('Submit')
                    ->assertSee('e2e Test post');
        });
    }
```

![laravel DUSK browser e2e](https://res.cloudinary.com/deuit9vp2/image/upload/v1508795391/barisesencom/browser-test.png)


Dusk kullanırken standart css selektörlerini kullanabiliyoruz. oluşturduğumuz test fonksiyonunun üzerine /** @test */ eklememiz gerekiyor. assertSee ile gittiğimiz sayfayı doğruluyoruz. Yani ->assertSee('E-Mail Address') şeklinde bir tanımlama yaptığımızda o sayfada E-Mail Address olup olmadığına bakıyor var ise true yok ise false dönüyor. Herhangi biri false olursa testimiz başarısız olucaktır.
Örnekteki gibi senaryolar üreterek sistemimizi gerçek bir kullanıcı hareketlerini taklit ederek basitçe test edebiliriz.

Eksik gördüğünüz yerleri iletirseniz, makaleyi güncelleebilirim. Teşekkürler.
