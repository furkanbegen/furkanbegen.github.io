---
layout: post
title:  "OAuth2 Basit Anlatım"
date:   2017-08-29
categories: genel
tags: [OAuth2]
---

OAuth2 yetkilendirme işlemleri için kullanılan standartlaşmış bir protokoldür. OAuth2 versiyonu 2006 yılında ortaya çıkmış olan orijinal OAuth protokolünün yerine geçmiştir.

OAuth2 kullanarak kullanıcıdan, hesabının tamamına ya da belli bir kısmına erişmek için yetki alıp bunu kendi uygulamanızda kullanabilirsiniz. Bunun ön yaygın örnekleri Facebook, Twitter, Google hesabınız ile başka uygulamalara ya da web sitelerine giriş yapabilmenizdir.

![OAuth2 Protokol Akışı](/assets/img/oauth2-protoko-akisi.png)

- Uygulama kullanıcıdan hesabının tamamına ya da bir kısmına erişebilmek için izin ister.
- Eğer kullanıcı bu isteği onaylarda uygulama bir yetki izni (authorization grant) elde eder.
- Uygulama yetkilendirme (authorization) sunucusuna kendi bilgilerini ve almış olduğu yetki iznini (authorization grant) göndererek sunucudan access token isteğinde bulunur
- Eğer uygulama bilgileri doğruysa ve yetki izni (authorization grant) geçerli ise, yetkilendirme (authorization) sunucusu uygulamaya bir access token sağlar.
- Uygulama almış olduğu access token ile birlikte içerik (resource) sunucusuna erişmek istediği içerikle ilgili istek gönderir.
- Eğer uygulamanın göndermiş olduğu access token geçerli ise, içerik (resource) sunucusu, ilgili içeriği uygulamaya geri gönderir.
Yetkilendirme sürecini anlayabilmemiz için bazı terimleri bilmemiz gerekiyor. Bunları aşağıda basit bir şekilde sıraladım.

## Client (Uygulama)

Client kullanıcının hesabına erişmeye çalışan uygulamadır. Hesaba erişebilmek için kullanıcıdan izin alması gerekir.

## Resource Server/API (İçerik Sunucusu)

Resource Server (İçerik Sunucusu) kullanıcının bilgilerine erişmek için kullanılan API sunucusudur.

## Authorization Server (Yetkilendirme Sunucusu)

Kullanıcının isteği onayladığı ya da reddettiği arayüzü sağlayan sunucudur. Küçük çaplı uygulamalarda bu API sunucusu ile aynı sunucu olabilir fakat büyük ölçekli uygulamalarda genellikle ayrı bir bileşen olarak oluşturulur.

## Resource Owner (Kullanıcı)

Resource owner hesabının belli kısımlarına erişmenize izin veren kullanıcıdır.

## Uygulama Kaydı

Yetkilendirme işlemi yapabilmemiz için önce uygulamamızı kayıt ettirmemiz gerekir. Yetkilendirme (authorization) sunucusu sadece önceden kayıt edilmiş uygulamalardan gelen isteklere cevap verecektir. Uygulamamızı kayıt ederken uygulama ismi, açıklaması, logosu gibi bilgileri gireriz, bunlar opsiyoneldir. Fakat her uygulamada bulunması gereken bilgiler vardır. Bunlar; Redirect (Yönlendirme) URI, Client ID ve Client Secret.

## Redirect (Yönlendirme) URIı

Yetkilendirme (authorization) sunucusu, kullanıcıyı, kullanıcının izin verme işleminden sonra yalnızca uygulamanın sistemde kayıtlı yönlendirme adresine yönlendirir.

## Client ID

Uygulama sisteme kayıt edildikten sonra, sunucunun her uygulama için özel olarak vermiş olduğu ID değeridir. Client ID yetkilendirme (authorization) sunucusunun uygulamayı tanımlayabilmesi için kullanılır. Client ID ayrıca kullanıcıdan izin almak için kullanılacak URL oluşturmak için de kullanılır.

## Client Secret

Uygulama sisteme kayıt edildikten sonra, sunucunun her uygulama için özel olarak vermiş olduğu secret değeridir. Bu secret değeri gizli tutulmalıdır.

## Yetkilendirme (Authorization)
Yukarıda da belirttiğimiz üzere OAuth 2 işleminin ilk adımı kullanıcıdan yetki almaktır. OAuth 2 ile kullanabileceğiniz birkaç izin tipi (grant type) bulunur. Bu izin tipleri aşağıda listelenmiştir

- **Authorization Code**: Server tabanlı çalışan uygulamalar tarafından kullanılır.
- **Password**: Güvenilir uygulamalar (trusted clients) tarafından kullanılır. Güvenilen uygulamalar servisin kendi uygulamalarıdır. Örneğin facebook mobil uygulaması için password izin tipini kullanır çünkü izin alacağı kullanıcı zaten kendi kullanıcısıdır ve kullanıcının bu uygulamaya şifresini girmekte bir mahsur yoktur.
- **Client Credentials**: Uygulama API erişimi için kullanılır.
- **Implicit**: Eskiden client secret olmaksızın, mobil uygulamalar ve server tabanlı olmayan, tarayıcıda çalışan web uygulamaları (örneğin JavaScript ile geliştirilmiş bir web uygulaması) için kullanılırdı, fakat şimdi client secret siz Authorization Code izin tipi ile kullanılması öneriliyor. Yani Implicit izin tipinin kullanılması önerilmiyor.

## Grant Type: Authorization Code Örnek Akış

**Adım 1**: Authorization Code Linki
Önce kullanıcıya yetki verebilmesi için bir link oluşturalım.

{% highlight json %}
<https://api.orneksite.com/oauth/authorize?responsetype=code&client_id=CLIENT_ID&redirect_uri=REDIRECT_URL&scope=read>
{% endhighlight %}

Yukarıdaki link ne anlama geliyor onu açıklayalım

- https://api.orneksite.com/oauth/authorize: APIımızın yetkilendirme linki
- responsetype=code: uygulamamızın bir authorization code istediğini belirtir.
- client_id=**CLIENT_ID**: isteği yapan uygulamanın Client IDsi
redirect_uri=**REDIRECT_URL**: kullanıcıdan izni alıp, authorization code elde edildikten sonra servisin kullanıcıyı yönlendireceği adres
- scope=read: uygulamanın hangi yetkileri istediğini belirtir.

**Adım 2**: Kullanıcının Uygulamaya İzin Vermesi
Kullanıcı yukarıdaki linki tıkladıktan sonra, servis tarafından uygulamaya izin vermesi karşısına bir arayüz çıkartılır

![OAuth 2 İzin Ekranı](/assets/img/oauth2-izin-ekrani.png)

## Adım 3: Uygulama Authorization Code Elde Eder

Eğer kullanıcı Kabul et butonuna tıklarsa, servis kullanıcı uygulamanın sistemde kayıtlı yönlendirme adresine yönlendirir.

{% highlight json %}
<https://ornekuygulama.com/yonlendir?code=**AUTHORIZATION_CODE**>
{% endhighlight %}

## Adım 4: Uygulama Access Token İsteğinde bulunur

Uygulama access token almak için servise uygulama detaylarıyla beraber bir POST isteği gönderir

{% highlight json %}
<https://api.orneksite.com/oauth/token?client_id=**CLIENT_ID**&client_secret=**CLIENT_SECRET**&grant_type=authorization_code&code=**AUTHORIZATION_CODE**&redirect_uri=**REDIRECT_URL**>
{% endhighlight %}

## Adım 5: Uygulama Acces Token Elde Eder

Eğer uygulamanın gönderdiği bilgiler geçerli ise, servis uygulamaya bir access token gönderir.

{% highlight json %}
{

“access_token”:”RsT5OjbzRn430zqMLgV3Ia”,

“expires_in”:3600

}
{% endhighlight %}

Eğer uygulamanın gönderdiği bilgiler geçerli değilse, servis uygulamaya hata mesajı gönderir.

{% highlight json %}
{

“error”:”invalid_request”

}
{% endhighlight %}

## Grant Type: Password Örnek Akış

Kullanıcı bilgilerini uygulamaya girdikten sonra, uygulama bu bilgileri kullanarak servise POST isteği gönderir. Eğer bilgiler geçerli ise servis uygulamaya access token gönderir.

{% highlight json %}

<https://api.orneksite.com/token?grant_type=password&username=**USERNAME**&password=**PASSWORD**&client_id=**CLIENT_ID**>

{% endhighlight %}

## Grant Type: Client Credentials Örnek Akış

Uygulama kendi Client ID ve Client Secret bilgilerini POST metodu ile servise gönderir. Eğer bu bilgiler geçerli ise servis uygulamaya bir access token gönderir

{% highlight json %}
<https://api.orneksite.com/token?grant_type=client_credentials&client_id=**CLIENT_ID**&client_secret=**CLIENT_SECRET**>
{% endhighlight %}

## Yetkilendirilmiş İstek Yapmak

Uygulama access token elde ettikten sora, bu tokeni kullanarak kullanıcının adına istekte bulunabilir. Curl kullanarak aşağıdaki gibi bir istek yapabilirsiniz

{% highlight json %}
curl -G “Authorization: Bearer **ACCES_TOKEN**” \ “<https://api.orneksite.com/1/profile”>
{% endhighlight %}
