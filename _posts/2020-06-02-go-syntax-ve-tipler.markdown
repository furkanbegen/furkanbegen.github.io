---
layout: post
title: "Go Synax ve Tipler"
date: 2020-06-02
categories: genel
tags: [go, golang]
---

Bu yazıda Go programlama dilinde temel söz dizimi (syntax) ve tiplerden (types) bahsedeceğim 

## Değişken tanımlama
***
<br>Go dilinde değişken tanımlamak için var anahtar sözcüğü kullanılır.

{% highlight go %}
var i int
{% endhighlight %}

Ayrıca değişkeni tanımlarken tanımladığımız değişkene değer atayabiliriz

{% highlight go %}
var i int
{% endhighlight %}

Go dilinde değişken tanımlarken kısa değişken tanımlama yöntemini de kullanabiliriz

{% highlight go %}
s := "Furkan"
i := 2
{% endhighlight %}

Kısa değişken tanımlama yöntemi ile değişken tanumladığımızda değişkenin tipi, atadığımız değer ne ise o olur. 
Yukarıdaki örnekte **s** değişkeninin tipi **string** **i** değişkeninin tipi **int** dir

# Çoklu Atama
***
<br>Go dilinde aynı satırda birden bazşa değişken ataması yapabilirsiniz.

{% highlight go %}
h,j,k,l := ture, "Furkan", 2.05, 25
{% endhighlight %}

Çoklu atama yaparken aynı satırda atama yaptığımız değişlenlerin tiplerinin aynı olması gerekmez.  
  
Yukarıdaki örnekte **h** değişkenine bir **bool** değer, **j** değişkenine bir **string** değer, **k** değişkenine bir **float** değer ve **l** değişkenine bir **int** değer atanmıştır.

## Zero Values
***
<br>Go dili ile beraber gelen tiplerin ön tanımlı değerleri vardır. Bu tiplede değişken oluştururken herhangi bir değer ataması yapmazsanız oluşturduğunuz değişkenlerin değeri ilgili tipin **zero value** değeri olur.  

Go dilindeki tiplerin ön tanımlı değerleri aşağıdaki gibidir.
{% highlight go %}
int = 0
string = ""
float = 0
bool = false
{% endhighlight %}

Go dilinde tiplerin ön tanımlı değerleri olması dolayısıyla bazı dillerde olduğu gibi **undefined** değerler ile karşılaşmazsınız

## İsimlendirme Kuralları
***
<br>Go dilinde depişkenleri isimlendirmek oldukça esnek olsa da bazı kurallar vardır.

- Değişken isimleri tek kelime olmalıdır(boşluk yok)
- Değişken isimlerinde harf, rakam ve alt tire olabilir
- Değişken isimleri rakam ile başlayamaz

*Örnekler*

| Doğru   |      Yanlış    |  Sebep                            |
|:-------:|:-------------:|:---------------------------------:|
| userName |  user-name    | tire kullanılamaz                 |
| user1    |  1user        | değişlenler rakam ile başlayamaz  |
| user     |  $user        | değişkenlerde sembol kullanulamaz |

Bir diğer önemli nokta büüyk küçük harf duyarlılığıdır. Yani **userName**, **UserName** ve **USERNAME** bitbirinden farklı değişlenlerdir

## İsimlendirme Stili
***
<br>Go dilinde değişkennlere kısa isim vermek yaygındır. **user** ve **userName** arasında bir seçim yapılacaksa, seçiminiz **user** olsun

*Örnekler*

| Kabul Gören Kullanım     |      Kabul Görmeyen Kıllanım   |   Sebep                                          | 
|:------------------------:|:-----------------------------:|:-------------------------------------------------:|
| userName                 |  user_name                    | alt çizgi geleneksel kullanıma uymaz              |
| i                        |  index                        | daha kısa olduğundan index yerine i tercih edilir |
| serveHTTP                |  serveHttp                    | kısaltmalar büyük harf olmalı                     |
| userID                   |  userID                       | kısaltmalar büyük harf olmalı                     |

## Nümerik Tipler
***
<br>Go dilinde iki çeşit sayısal tip vardır

- Birincisi mimariden bağımsız olarak, tipin doğru boyuta(byte) sahip olacağı anlamına gelir
- İkincisi **implementasyona özgü** tiplerdir ve bu tiplerin boyutu(byte) uygulamanın derlendiği
mimariye göre değişiklik gösterir.

*İmplementasyona özgü tipler*
{% highlight go %}
int, uint, uintptr
{% endhighlight %}

## Overflow vs Wraparound
***
<br>Compile zamanında eğer compiler bir değişkene atanan değerin, o değişken tipinin max değerinden büyük olduğunu saptarsa 
**owerflow error** fırlatır
*İmplementasyona özgü tipler*
{% highlight go %}
var maxUint32 := 4294967295 // Max Uint32 size
fmt.Println(maxUint32)
{% endhighlight %}

başarılı bir şekilde derlenir ve ekrana *4294967295* yazar 
{% highlight go %}
fmt.Println(maxUint32 + 1)
{% endhighlight %}
runtime da  1 eklerek **wraparound** olur ve ekrana 0(sıfır) yazar

{% highlight go %}
var maxUint32 := 4294967295 // Max Uint32 size
fmt.Println(maxUint32 + 1)
{% endhighlight %}

compile olmaz.

## Stringler
***
<br>Eğer string oluştururken tek tırnak (') kullabursak bir raw string oluşturmuş oluruz.
Eğer çift tırnak (") kıllanırsak bir interpreted string oluşturmuş oluruz

{% highlight go %}
s1 := 'Raw string'
s2 := "Interpreted string"
{% endhighlight %}

## UTF-8
***
<br>Go dili hiç bir kütüphane, paket, veya kuruluma gerek duymakszın default olarak utf-8 destekler.

## Constant
***
<br>Constant lar değişkenler gibidir ancak bir kere değer atandığı zaman tekrar değiştirilemezler
{% highlight go %}
const gopher = "GOPHER"
{% endhighlight %}

## IOTA
***
<br>Go dilinde iota, constant tanımlarken artan sayılarda değer üretimini basitleştirmek için kullanılır
{% highlight go %}
const (
	Apple int = iota
	Orange
	Banana
)
{% endhighlight %}

Değerleri ekrana yazdırırsan çıktısı aşağıdaki gibi olur.   
Apple = 1
Orange = 2
Banana = 3