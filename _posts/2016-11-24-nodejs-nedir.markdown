---
layout: post
title:  "Node.js Nedir ?"
date: 2016-11-24 23:50:58 +0300
categories: nodejs
permalink: /:title
---
NodeJs, yaygın olarak web tarayıcılarında kullanılmakta olan dinamik bir programlama dili olan JavaScript'in, sunucu tarafında yani backend dili olarak kullanılabilmesine olanak sağlayan bir programlama dilidir. Bu durum sunucu ve istemci tarafında yazılan kodlar arasındaki farklılıkları ortadan kaldırarak, tek programlama dili ile tüm ihtiyaçların karşılanmasını amaçlamaktadır.

Ayrıca NodeJs tek yürütmeli(single thread) mimari yapısının içinde asenkron programlama yapısını kullanmaktadır. Bu durum yüksek performans ve ölçeklendirilebilme olanakları sunmaktadır. Asenkron yapısı sayesinde gerçek zamanlı(real time) uygulamalarda oldukça iyi performans sağlamaktadır. Yani NodeJs ile büyük ve performansa ihtiyacınız olan projeleri geliştirebilirsiniz.

Birçok büyük proje de şuanda NodeJs kullanılmaktadır. Bu örnekleri [link1](https://www.quora.com/What-companies-are-using-Node-js-in-production)-[link2](https://www.quora.com/What-are-the-biggest-websites-built-with-Node-js-on-the-server-side) Quora makalelerinden de inceleyebilirsiniz.

![Quora-using-nodejs](https://res.cloudinary.com/deuit9vp2/image/upload/v1479934445/barisesencom/using-nodejs-in-production.png)

Görüldüğü gibi anlık yüksek kullanıcı sayılarına ulaşan popüler uygulamalar, özellikle gerçek zamanlı uygulamalarda sunucularınızı rahat ettirecektir. Örneğin chat uygulaması vb.

### NodeJs'in Avantajları Nelerdir ?

* NpmJs' e sahip olması. NpmJs, NodeJs'in paket yöneticisidir. İçerisinde şuan itibariyle 375 bin civarı paket bulunuyor. Bu paketleri kullanarak hızlı bir şekilde proje çıkarabilirsiniz. Ayrıca NpmJs'e sizde yaptığınız paketleri yükleyebilir, diğer kişilere yardımcı olabilirsiniz.
![npmjs](https://res.cloudinary.com/deuit9vp2/image/upload/v1479942606/barisesencom/npmjs.png)

* Modüler yapıya sahip olması. Yukarıda ki maddeyle birbirini bütünler bir avantajdır. Modüler olduğu için paketleri rahatlıkla projenize dahil edebilirsiniz. Buda size geliştirme sırasında hız ve kolaylık olarak geri dönecektir.

* Hızlı, basit ve yüksek miktardaki istemi verimli bir şekilde yönetebilen bir yapıya sahiptir.

* Asenkron yapısı sayesinde, yoğun veri trafiğine sahip sistemlerde, özellikle veritabanı işlemlerinin yol açtığı beklemeleri ortadan kaldırmaktadır.

* MongoDB ile ilişkisinin üst seviyede olması NodeJs’nin birbaşka avantajı. NoSQL veritabanlarının en ünlüsü MongoDB de JavaScript tabanlı olduğu için üretkenliğiniz yine artıyor.

Ama unutmamak gerek ki şu durumla karşılabilirsiniz. 😃😃
http://callbackhell.com/
![callback-hell](https://res.cloudinary.com/deuit9vp2/image/upload/v1479943610/barisesencom/callback-hell.gif)
