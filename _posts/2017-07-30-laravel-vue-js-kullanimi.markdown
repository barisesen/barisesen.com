---
layout: post
title:  "Laravel Vue Js Kullanımı"
date: 2017-07-30 18:00:00 +0300
categories: Laravel
permalink: /:title/
author: [Barış Esen]
tag: [laravel, vue, js, vue js, frontend, php]
---

## Vue Js nedir?
Vue Js, web arayüz geliştirme işlerini kolaylaştırmak için geliştirilmiş bir javascript kütüphanesidir. Hızlıca geliştirme yapılabilen bir yapısı olduğunu düşünüyorum. Vue Js öğrenmek için Türkçe kaynakta gün geçtikçe artmaya devam ediyor. Benim bildiğim ve takip ettiğim ana kaynak Fatih Acet'in youtube kanalı. Kanalın linkine yazının altından ulaşabilirsiniz.

## Laravel'de Vue Js Kullanmaya Başlamak
Ben şuan da Laravel'in şuan için son veriyonu olan 5.4 kullanıyorum. 5.4'de Vue js kullanıma hazır bir şekilde geliyor. Sadece npm paketlerini kurmamız gerekiyor. Bunun için uygulama dizininde,
```sh
npm install
```
komutunu çalıştırıyoruz. Bu komut ile projemiz ana dizininde ki package.json dosyasının içinde tanımlanmış gereksinimler node_modules klasörü içine oluşturulacak.

## Laravel Vue Js Dizinleri
Projemizi oluşturduğumuzda oluşan resources klasörü içinde assets klasörü bulunuyor. Vue component dosyamızda bu assets klasörü içinde ki js klasörünü içinde components isimli bir klasör var. Biz kullanacağımız templateleri bu klasör içine oluşturabiliriz. Ayrıca components klasörü içinde Example.vue isimli örnek bir kullanımda bulunuyor.

```js
<template>
    <div class="container">
        <div class="row">
            <div class="col-md-8 col-md-offset-2">
                <div class="panel panel-default">
                    <div class="panel-heading">Example Component</div>

                    <div class="panel-body">
                        I'm an example component! heyy
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
```

Kullanacağımız diğer dosya ise Js klasörü içindeki app.js. Yazdığımız component leri bu dosya içerisinde kullanıma hazırlayacağız. Kısacası tanımlayacağız.

![Component tanımlama - laravel](https://res.cloudinary.com/deuit9vp2/image/upload/v1501425201/barisesencom/laraval-vue/comonents.png)

Unutmamamız gereken bir nokta resource dosyası içerisinde yaptığımız değişikliklerin kullanılabilmesi için
```sh
npm run watch
```
komutunu çalıştırmalıyız.

## Laravel Vue js Component oluşturma örneği
Components klasörü içine yeni bir dosya oluşturuyoruz. Ben Blog yazılarını listelediğim bir örnek yapacağım. Bunun için ilk olarak Posts.vue isimli bir dosya oluşturdum.

```js
<template>
    <div>
        <postitem v-for="post in posts" v-bind:post="post"></postitem>
    </div>
</template>

<script>
    export default {
        props: ['posts'],
        mounted: function () {
            console.log(this.posts);
        }
    }
</script>
```
Oluşturduğum dosyanın içeriği yukarıdaki gibi. Template çağrılırken bir array alacak ve yazdğımız component, bu arrayi for ile döndürerek başka bir componente o datayı verecek. Burda dikkat etmemiz gereken, diğer componenti cağırdığımızda bir döngü işlemi kuruyoruz. Bu gibi durumlarda oluşacak alt itemleri kapsayan bir element olması gerekiyor. Ben burada div tagleri arasında componenti çağırarak yaptım.
Yu
karıda yaptığımız gibi PostItem.vue isimli bir component daha oluşturuyorum.
```js
<template>
    <div class="blog-post">
        <h2 class="blog-post-title"><a :href="'/posts' + post.id">{{ post.title }}</a></h2>
        <p class="blog-post-meta">{{ post.created_at }} by <a href="#">{{ post.user.name}}</a></p>
        {{ post.content }}
    </div>
</template>

<script>
    export default {
       props: ['post']
    }
</script>
```
Bununda içeriği yukarıda ki gibi. Diğer componentten gelen post objesini alıyoruz ve oluşturduğumuz html ile birleştiriyoruz. Şimdi sıra bu oluşturduğumuz componentleri tanımlamaya geldi. Bunun için yukarıda bahsettiğim gibi, app.js dosyamızın içine şu eklemeyi yapıyoruz.
```js
Vue.component('posts', require('./components/Posts.vue'));
Vue.component('postitem', require('./components/PostItem.vue'));
```

Yazdığımız kodun aktif olması için
```sh
npm run watch
```
komutumuz çalışmıyor ise çalıştırmamız gerekiyor. Componentlerimiz oluşturduk, artık kullanılabilir durumdalar. Dikkat etmemiz gereken diğer iki nokta var.
İlk olarak sayfamızda app id li bir kapsayıcı element olması gerekiyor.
```html
<div id="app" class="container">
...
</div>
```
Şu şekilde olması gerekiyor, burda app kullanılmasının sebebi, Vue js'i tanımlarken 
```js
const app = new Vue({
    el: '#app'
});
```
element olarak #app seçilmiş olmasıdır.
İkinci dikkat etmemiz gereken durum ise javascript dosyamızı çağırmış olmamız gerekiyor. 

```html
<script src="js/app.js"></script>
```
Kodunu javascript dosyalarımızı çağırdığımız alana ekleyebiriliriz.

Componentleri oluşturduk, gerekli ayarlamalarıda yaptık son olarak kullanmak istediğimiz alanda componentleri çağırmamız gerekiyor. Bunun için postları listelemek istediğim alana 
```html
    <posts v-bind:posts="{{  $posts }}"></posts>
```
Eklememiz yeterli olucaktır.

### Vue js Kaynağı
[Fatih Acet Youtube kanalı](https://youtu.be/byuboztKw9E?list=PLa3NvhdFWNipwk1KXeUpVQnAiAfuBw4El)
[Vue Js Guide](https://vuejs.org/v2/guide/)

Eklememi istediğiniz kaynaklar var ise yorum olarak eklersin burayı düzenleyebiliriz.
