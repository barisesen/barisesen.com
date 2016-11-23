---
layout: post
title:  "NodeJs kurulumu [Ubuntu]"
date:   2016-11-21 07:22
categories: nodejs
permalink: /:title
---

Ubuntu işletim sistemi üzerine nodejs kurmak oldukça basit. Kurulum için Ubuntu paket yöneticisini kullanacağız.
Ben kurulum için Digital Ocean'da ki bir sunucuyu kullandım ama normal kullandığımız bilgisayarada kurulumu birebir aynı şekildedir.

Ubuntu'da kurulum için, aşağıdaki komutları kullanmamız yetecektir.

```sh
$ curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
```
![nodejs-download-with-curl](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/nodejs-download-with-curl.png)

```sh
$ sudo apt-get install -y nodejs
```
![nodejs-intall-via-package-manager](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/nodejs-intall-via-package-manager.png)

```sh
$ sudo apt-get install -y build-essential
```
Kurulum işlemi bu kadar.

![node-v](https://res.cloudinary.com/deuit9vp2/image/upload/barisesencom/node-v.png)

Artık rahatlıkla NodeJs projeleri yazabilir, yazılmış projeleri indirip çalıştırabilirsiniz.

Diğer işletim sistemlerine nasıl kurulum yapacağınızı öğrenmek için aşağıdaki linki kullanabilirsiniz.

[link](https://nodejs.org/en/download/package-manager/)
