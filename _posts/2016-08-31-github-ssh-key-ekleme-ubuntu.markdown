---
layout: post
title:  "Github Ssh Key Eklenmesi [Ubuntu]"
date:   2016-08-31 15:50:58 +0300
categories: Github
permalink: /:title
---

Merhaba, Ubuntu’da github kullanırken her yeni push olayında kullanıcı adı ve parola sorması belli biyerden sonra can sıkmaya başlıyor. Çözümü ise çok basit. Yapmamız gereken, oluşturacağımız ssh keyi github hesabımıza eklemek. Daha sonra her push yaptığımızda bizim için kullanıcı bilgileri arkada giriliyor olacak ve kullanıcı adı ve parolayı her defasında yazmak zorunda kalmayacağız.

Öncelikle yapmamız gereken terminali kullanarak bir ssh key üretmemiz olacak. Bunun için aşağıdaki kod parçacığını kullanıyoruz.

```sh
ssh-keygen -t rsa -b 4096 -C "githubmailadresiniz"
```
Karşımıza aşağıdaki gibi bir soru gelecek, değiştirmeden entera basmamız yeterli olacaktır.

![github ssh key](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/08/Screenshot-from-2016-08-31-200903.png?resize=385%2C77&ssl=1)

Daha sonra bizden **passphrase** (_parola_) isteyecek oraya unutmayacağımız bir parola yazıyoruz.

![github ssh key passphrase](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/08/Screenshot-from-2016-08-31-201431.png?resize=384%2C242&ssl=1)

Başarıyla ssh-keyimizi oluşturuyoruz. Oluşan ssh keyi sistemimize tanıtmak için aşağıdaki komutu kullanmamız gerekecek.

```sh
eval "$(ssh-agent -s)"
```
Bu komutu yazdığımızda çıktı olarak,

```sh
Agent pid 59566 (59566 bende o anki process id si sizde farklı çıkacaktır.)
```
Daha sonra aşağıdaki komutu girerek ssh keyi ekliyoruz.

```sh
$ ssh-add ~/.ssh/id_rsa
```

![ssh-add](https://i0.wp.com/barisesen.com/wp-content/uploads/2016/08/Screenshot-from-2016-08-31-201918.png?resize=371%2C115&ssl=1)

Şimdi sıra ssh-key’i github profilimize eklemeye geldi. Bunun için github ayarlar sayfasından SSH and GPG keys sekmesine tıklıyoruz.

![github-settings](https://i1.wp.com/barisesen.com/wp-content/uploads/2016/08/Screenshot-from-2016-08-31-202523.png?resize=648%2C348&ssl=1)

Açılan sayfada New SSH key butonuna basıyoruz ve formda başlık kısmına istediğimiz başlığı yazıyoruz. Key kısmına da oluşturduğumuz keyi yazacağız. Kullanacağımız keye, terminalde anadizinde iken aşağıdaki komut ile ulaşabiiliriz.

```sh
$ cat .ssh/id_rsa.pub
```
![github ssh id_rsa.pub](https://i2.wp.com/barisesen.com/wp-content/uploads/2016/08/Screenshot-from-2016-08-31-203032.png?resize=648%2C231&ssl=1)

Çıktıyı kopyalıyor , key kısmına yazıyoruz ve kaydediyoruz.

![github ssh key kayıt](https://i0.wp.com/barisesen.com/wp-content/uploads/2016/08/Screenshot-from-2016-08-31-203229.png?resize=648%2C194&ssl=1)

Şimdi sıra test etmeye geldi bunun için terminalden githuba ssh bağlantısı deneyeceğiz.

```sh
$ ssh -T git@github.com
```

Yukarıdaki gibi bir ssh bağlantısı kuruyoruz. Aşağıdaki gibi bir çıktı alırsak işlemimiz başarıyla sonuçlanmış olacaktır.

![github ssh successfully auth](https://i1.wp.com/barisesen.com/wp-content/uploads/2016/08/Screenshot-from-2016-08-31-203415.png?resize=648%2C118&ssl=1)

Github’da ayar sayfasını yenilediğimizde ise

![github ssh successfully](https://i1.wp.com/barisesen.com/wp-content/uploads/2016/08/Screenshot-from-2016-08-31-203427.png?resize=648%2C209&ssl=1)

gibi bir erkran göreceğiz. Artık github kullanırken sürekli kullanıcı adı ve şifremizi girmemize gerek kalmadı.

![github ssh kullanımı](https://i1.wp.com/barisesen.com/wp-content/uploads/2016/08/Screenshot-from-2016-08-31-203944.png?resize=648%2C355&ssl=1)
