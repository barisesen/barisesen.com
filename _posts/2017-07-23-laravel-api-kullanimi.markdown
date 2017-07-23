---
layout: post
title:  "Laravel Api nasıl oluşturulur?"
date: 2017-07-23 14:00:00 +0300
categories: Laravel
permalink: /:title/
author: [Barış Esen]
tag: [laravel, api, token, api_token, authorization, api auth, api login, api register]

---
# Laravel api oluşturma
Api kısaca uygulamanızı dışarı açan bir kapı olarak yorumluyorum ben. örneğin bir mobil uygulama ile iletişim kurmasını api ile sağlıyoruz. Herhangi bir arayüze ihtiyaç duymadan veri alışverişi diye özetleyebilirim. Laravel ile api kullanımının farklı yolları var. Ben token based api olarak adlandırılan halini anlatacağım. Diğer api çeşitleri için makalenin altındaki linkleri ziyaret edebilirsiniz. Unutmamak gerekir ki Api kullanımın da en önemli şeylerden biriside ssl sertifakasıdır. Ssl sertifikası olmayan bir sistem üzerinden api hizmeti sağlarsanız, trafik spoof edilebilir ve bu bir güvenlik zafiyetidir. Bu konu ile ilgili daha detaylı bilgi için size netsparker ekibinin hazırladığı videoyu önerebilirim. Video linkine makalenin altından ulaşabilirsiniz.

İlk olarak yeni bir proje oluşturuyorum. Siz varolan projeniz üzerinden de devam edebilirsiniz.
```sh
laravel new api
```

Yeni laravel uygulamamızı oluşturduk. Kullandığımız bir editör veya ide ile projeyi açıyor, gerekli env ayarları ve database oluşturma işlemlerini gerçekleştiriyoruz ve user migrationlarını düzenlemeye başlıyoruz. User create migrationına 
```php
$table->string('api_token', 60)->unique()->nullable();
```
![Api token alanının eklenmesi](https://res.cloudinary.com/deuit9vp2/image/upload/v1500807110/barisesencom/laravel-api/users_migration_api_token_alan%C4%B1_eklendi.png)
Satırını ekliyorum. Yani users tablosunda api_token isimli bir kolon oluşturduk. Burada kullanıcının api tokenını tutarak auth işlemlerini sağlayacağız.

Daha sonra database migrate işlemini gerçekleştiriyoruz.

```sh
php artisan migrate
```

İkinci adım olarak User modelimizi açıyoruz. Fillable ve hidden alanlarına api_token ekliyoruz.

![Fillable ve hidden alanlarına api_token eklenmesi](https://res.cloudinary.com/deuit9vp2/image/upload/v1500805682/barisesencom/laravel-api/fillable_ve_hidden_alanlar%C4%B1na_api_token_eklenmesi.png)

Auth modülünü aktif ediyoruz.

```sh
php artisan make:auth
```

Bu işlem sonrasında db yi migrate ediyoruz.

```sh
php artisan migrate
```

![php artisan migrate](https://res.cloudinary.com/deuit9vp2/image/upload/v1500806084/barisesencom/laravel-api/migrate.png)

Genel anlamda api için oluşturmamız gerekenler bu kadardı. Şimdi nasıl kullanacağımıza bakalım.

Api işlemleri için Api routing ini kullanacağız. Kullanıcı girişi zorunlu olan routingler için ise:
```php
middleware('auth:api')
```
middleware'ını kullancağız. Bu middleware bizim için gelen parametreler arasında api_token paremetresi var mı? Var ise bir kullanıcıya ait mi diye kontrol edecek. Aynı şekilde parametre olarak göndermek zorunda da değiliz api tokenı header içinde 
```php
Authorization:Bearer api_token
```
şeklinde de kullanabiliriz.

## Api ile login işleminin yapılışı
Bunun için Controller dosyamızın içinde Api adında bir klasör oluşturacağım. Daha sonra bu klasör içinde bir userController oluşturacağım. Bu controller içinde örnek olması için bir login fonksiyonu ekliyorum. Api'a gelecek login isteklerini burada alıp email ve password eşleşiyor ise kullanıcıya bir api token döndüreceğiz.

```php
public function login(Request $request)
    {
        #Todo: Use Validation
        $user = User::where('email', $request->email)->first();
        if (Hash::check($request->password, $user->password)) {
            $user->api_token = str_random(60);
            $user->save();
            return response()->json([
                'status' => 200,
                'api_token' => $user->api_token,
                'username' => $user->name,
                'email' => $user->email,
                'id' => $user->id
            ]);
        }
        return response()->json([
            'status' => 401,
            'message' => 'Unauthenticated.'
        ]);
    }
```

Bu işlem sırasında gerek duyduğunuz validationları ekleyerek burayı geliştirebilirsiniz.

Browser üzerinden register'ı kullanarak bir kullanıcı oluşturuyorum. Login testini yapabilmek için.

Api testleri için Postman isimli aracı kullancağım. [Postman chrome store linki](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)'ni kullanarak sizde uygulamayı edinebilirsiniz. Oldukça kullanışlıdır.

Unutmadan birde routeing oluşturmamız gerekiyor. Bunun için Api.php dosyamızı açıyor ve bir tane routing ekliyoruz. 
```php
Route::post('/login', 'Api\UserController@login');
```
Ben yukardaki gibi bir routing oluşturdum. api.dev/api/login urline post isteği ile email ve password gönderilecek.

Postmanı açıyor ve aşağıda resimde görüldüğü gibi bir istek gönderiyorum.

![Login isteği sonunda dönen response](https://res.cloudinary.com/deuit9vp2/image/upload/v1500807720/barisesencom/laravel-api/login_response.png)

Görüldüğü gibi başarılı bir şekilde login işlemi gerçekleştirildi. Ve sonuç olarak kullanıcı bilgileri ve api_token döndü. Kullanıcı girişinin zorunlu olduğu her alanda artık bu api tokenıda göndereceğiz. Ayrıca middleware olarak auth:api kullancağız.

Örneğin giriş yapan bir kullanıcın kendi bilgilerini listelemesini istiyoruz. Örnek kullanım aşağıdaki gibi olacaktır.

```php
Route::group(['middleware' => 'auth:api', 'prefix' => 'user'], function () {
    Route::get('/', 'Api\UserController@user');
});
```

user fonksiyonumuz ise aşağıdaki gibi:
```php
public function user()
    {
        $user = Auth::guard('api')->user();
        if (!$user) {
            return response()->json([
                'status' => 401,
                'message' => 'Unauthenticated.'
            ]);
        }
        return $user;
    }
```

Postman de isteği yaparken headera ayrıca
```js
Accept: application/json
```
Eklersek istediğimiz gibi yanıt alabileceğiz.

![user bilgilerini geri döndüren api](https://res.cloudinary.com/deuit9vp2/image/upload/v1500808671/barisesencom/laravel-api/user_info.png)

Örneğin geçersiz bir api token ile istek yapacak olursak yanıt aşağıdaki gibi olucaktır.

![Geçersiz token ile istek yollanması](https://res.cloudinary.com/deuit9vp2/image/upload/v1500808756/barisesencom/laravel-api/ge%C3%A7ersiz_token_ile_istek_yollanams%C4%B1.png)

Kısaca ve en basit hali ile Laravel'de api kullanımını anlatmaya çalıştım. Sorularınız olursa bilgim dahilinde yanıtlamaktan mutluluk duyarım.

SSL Nedir? Neden Önemlidir?
[![SSL Nedir? Neden Önemlidir?](https://img.youtube.com/vi/ozBijMxC3Hs/0.jpg)](https://www.youtube.com/watch?v=ozBijMxC3Hs)

### Diğer kullanılabilecek api türleri
Json web token [Github json web token](https://github.com/tymondesigns/jwt-auth)

Laravel Passport [Laravel passport](https://laravel.com/docs/5.4/passport)
