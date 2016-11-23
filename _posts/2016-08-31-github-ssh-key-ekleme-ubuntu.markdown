---
layout: post
title:  "Github Ssh Key Eklenmesi [Ubuntu]"
date:   2016-08-31 15:50:58 +0300
categories: github
permalink: /:title
---

Merhaba, Ubuntu’da github kullanırken her yeni push olayında kullanıcı adı ve parola sorması belli biyerden sonra can sıkmaya başlıyor. Çözümü ise çok basit. Yapmamız gereken, oluşturacağımız ssh keyi github hesabımıza eklemek. Daha sonra her push yaptığımızda bizim için kullanıcı bilgileri arkada giriliyor olacak ve kullanıcı adı ve parolayı her defasında yazmak zorunda kalmayacağız.

Öncelikle yapmamız gereken terminali kullanarak bir ssh key üretmemiz olacak. Bunun için aşağıdaki kod parçacığını kullanıyoruz.

```sh
ssh-keygen -t rsa -b 4096 -C "githubmailadresiniz"
```
Karşımıza aşağıdaki gibi bir soru gelecek, değiştirmeden entera basmamız yeterli olacaktır.

![github ssh key](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/ssh_keygen_-t_rsa_-b_4096.png)

Daha sonra bizden **passphrase** (_parola_) isteyecek oraya unutmayacağımız bir parola yazıyoruz.

![github ssh key passphrase](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/ssh-key-passphrase.png)

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

![ssh-add](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/eval-ssh-agent.png)

Şimdi sıra ssh-key’i github profilimize eklemeye geldi. Bunun için github ayarlar sayfasından SSH and GPG keys sekmesine tıklıyoruz.

![github-settings](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/github-settings-page.png)

Açılan sayfada New SSH key butonuna basıyoruz ve formda başlık kısmına istediğimiz başlığı yazıyoruz. Key kısmına da oluşturduğumuz keyi yazacağız. Kullanacağımız keye, terminalde anadizinde iken aşağıdaki komut ile ulaşabiiliriz.

```sh
$ cat .ssh/id_rsa.pub
```
![github ssh id_rsa.pub](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/ssh-id-rsa-pub-cat.png)

Çıktıyı kopyalıyor , key kısmına yazıyoruz ve kaydediyoruz.

![github ssh key kayıt](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/github-ssh-key-kayit.png)

Şimdi sıra test etmeye geldi bunun için terminalden githuba ssh bağlantısı deneyeceğiz.

```sh
$ ssh -T git@github.com
```

Yukarıdaki gibi bir ssh bağlantısı kuruyoruz. Aşağıdaki gibi bir çıktı alırsak işlemimiz başarıyla sonuçlanmış olacaktır.

![github ssh successfully auth](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/github-ssh-successfully-auth.png.png)

Github’da ayar sayfasını yenilediğimizde ise

![github ssh successfully](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/github-ssh-successfully.png)

gibi bir erkran göreceğiz. Artık github kullanırken sürekli kullanıcı adı ve şifremizi girmemize gerek kalmadı.

![github ssh kullanımı](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/github-ssh-key-test.png)
