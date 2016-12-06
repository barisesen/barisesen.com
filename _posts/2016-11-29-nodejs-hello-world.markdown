---
layout: post
title:  "Node.js Hello World"
date: 2016-11-30 14:50:58 +0300
categories: nodejs
permalink: /:title
---

NodeJs ile "Hello World" yazmak aslında oldukça basit. Node.js konsolda çalışabildiği için ilk merhabamızı konsola "Hello World" yazdırarak yapacağız. Bunun için hello.js adında bir dosya oluşturuyoruz. Linux kullananlar bunu hızlıca konsola bulundukları dizinde,


```sh
 $ touch hello.js
```


komutu ile açabilirler. Oluşturduğumuz dosyamızı text editörü yardımıyla açıyoruz. Ben text editörü olarak Atom kullanıyorum. Konsolda,


```sh
 $ atom hello.js
```


komutu ile hızlıca açabilirsiniz.

------------


##### ipucu :


```sh
 $ atom .
```


Yazarak bütün klasörü atom editöründe açabilirsiniz.

-----------

Açtığımız hello.js in içerisine,


```js
 console.log('Hello World')
```


yazmak yeterli olacaktır. Gördüğünüz gibi bu kadar basit. Ayrıca C dil ailesinde ki gibi sona noktalı virgül(;) koymayada gerek yok.

Kodun çıktısını konsolda görmek için konsola,


```sh
 $ node hello.js
```


yazıyoruz. Bu şekilde Node.js'e ilk merhabamızı demiş olduk.

![node-hello-js](https://res.cloudinary.com/deuit9vp2/image/upload/v1480506816/barisesencom/node-hello-js.png)

Aynı işlemi fonksiyon kullanarak yapmak istersek, yazmamız gerekenler şu şekilde olacaktı,

```js
var name = "baris"

function hello(name) {
  console.log("Hello ",name);
}

hello(name)
```
