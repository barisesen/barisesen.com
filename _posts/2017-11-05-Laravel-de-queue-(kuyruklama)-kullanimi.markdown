---
layout: post
title:  "Laravel Kuyruklama ile Mail gönderimi | Laravel Queueing Mail"
date: 2017-11-05 00:00:00 +0300
categories: Laravel
permalink: /:title/
author: [Barış Esen]
tag: [laravel, kuyruklama, queue, mail, arka plan, mail gonderme, php, redis, supervisor]
---

Yaptığımız projelerde genellikle mail gönderme işlemlerimiz oluyor. Ama mail işlemi bir miktar gecikmeyi de beraberinde getiriyor. Örneğin yaptığımız bir uygulamada yeni kayıt olan her kullanıcımıza hoşgeldin maili göndermek istiyoruz. Burada mail işlemini arka plana atmadan yapmayı düşünürsek browserda bir loading gif görmemiz çok olağan olacaktır. Ayrıca o sayfa kapatılırsa işlem de yarım kalacağı için hatalı bir seçim olmuş olacaktır. Bu sebeple mail işlemleri için genellikle bir kuyruklama, arka planda çalıştırma işlemi kullanılır. Kuyruklama işlemi için de farklı sistemler kullanabiliyoruz. Ben bilgisayarıma docker ile kurduğum bir redis kullanacağım. Burada isterseniz, Amazon SQS, Beanstalkd, redis vb. kullanabiliyorsunuz. Ama kullanacağınız driver için ayarları yapmanız gerekecektir. Ben redis kullandığım için projeye öncelikle redis driverını dahil edeceğim. Bunun için,

```sh
composer require predis/predis 
```
komutu kullanıyoruz. Daha sonra .env dosyamız içerisine şu ayarları ekliyoruz. Tabi kendimize göre düzenlemeyi unutmamalıyız.
```php

QUEUE_DRIVER=redis // Kuyruklama için redis driverını seçiyoruz.

REDIS_HOST=127.0.0.1 // localhost ta bir redis sunucunuz var ise bu şekilde kalabilir. Remote ise ip adresinizi yazacaksınız.
REDIS_PASSWORD=null // varsa buraya parolanız gelecek
REDIS_PORT=6379 // Default redis portu 6379'dur. Ama kurulum sırasında farklı bir port kullandıysanız güncellemeyi unutmayın.
```

Şimdi standart mail işlemlerini yapacağız.
```sh
php artisan make:mail Welcome
```
Bize app/Mail klasörü içerisinde Welcome.php adında bir class oluşturacaktır. Birde el ile bir mail viewı oluşturuyoruz. Views/emails klasörü içine welcome.blade.php adında bir view oluşturdum. Burası bizim email tasarımını ekleyeceğimiz alan olacak.

Oluşturduğumuz mail sınıfı içerisini aşağıdaki gibi hazırladım. Kullanıcı kayıt olduğunda bu mail sınıfını tetikleyeceğiz ve değer olarakta içine isim ve email adresi alacak.
```php
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Contracts\Queue\ShouldQueue;

class Welcome extends Mailable
{
    use Queueable, SerializesModels;

    protected $name;
    protected $email;
    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct($name, $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this
            ->subject('Aramıza Hoşgeldin!')
            ->to($this->email)
            ->with(['name'=> $this->name])
            ->view('emails.welcome');
    }
}
```
Oluşturduğumuz mail viewını aşağıdaki gibi çok basit bir şekilde hazırladım.
```html
Merhaba <b>{{$name}}</b>,
Seni aramızda görmekten çok mutluyuz. Bizi tercih ettiğin için teşekkürler.
```

Şimdi sırada kullanıcı kayıt olduğunda bu maili göndermeye geldi. Bunun için maili hangi işlemden sonra göndermek istiyorsak o kısma aşağıdaki gibi bir ekleme yapmamız gerekiyor.

```php
Mail::queue(new Welcome($name, $email));
```

Redisi çalışır duruma getiriyoruz. Ve işlemi gerçekleştiriyoruz. Bekliyoruz ama mail gelmediğini farkediyoruz :). Laravel de kuyruklama işleminin çalışması için kuyruklama işlemlerini dinlememiz gerekiyor. Bunun için aşağıdaki işlemi uygulayabiliriz.
```sh
php artisan queue:listen 
```
Şimdi işlemi bir daha çalıştıyoruz.
![Laravel mail gönderimi](https://res.cloudinary.com/deuit9vp2/image/upload/v1509887086/barisesencom/mail.png)

İşlemimizi başarı ile gerçekleştirdik. Şimdi bir problememiz daha var. Kuyruklama işlemini dinlemek için bir komutun çalışması gerekiyor. Local de biz bunu çalıştırabiliyoruz. Lakin sunucuda terminali kapattığımız da bu işlem sonlanacaktık. Bunun için supervisor kullanacağız.

Sunucumuz da supervisor kurulu değilse
```sh
sudo apt-get install supervisor
```
komutunu çalışıtırıyoruz. Daha sonra /etc/supervisor/conf.d klasörü içerisine giriyor ve yeni bir ayar dosyası oluşturuyoruz. Hızlıca,
```sh
sudo vim laravel-worker.conf
```
Komutunu çalışıtıryor ve arkasından içerisini aşağıdaki örnekteki gibi düzenliyoruz.

```bash
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php uygulama-dizininiz/artisan queue:work sqs --sleep=3 --tries=3
autostart=true
autorestart=true
user=forge
numprocs=8
redirect_stderr=true
stdout_logfile=uygulama-dizininiz/storage/logs/worker.log
```
Ardından supervisorı yeni oluşturduğumuz dosyalarımızı tanıtıyoruz.
```sh
sudo supervisorctl reread

sudo supervisorctl update

sudo supervisorctl start laravel-worker:*
```

Artık php artisan queue:listen komutunu çalıştırmaya ihtiyacımız yok arka planda sürekli bizim için supervisor bu işlemi yapacaktır.

Bir sonraki yazıda bu işlemler ile alakalı olan Job kullanımını işleyeceğim. Soru ve görüşleriniz için yorumlarınızı bekliyorum. Teşekkürler.
