---
layout: post
title: "Go Array ve Slice"
description: "Go dilinin array ve slice veri tipleri"
date: 2020-06-02
categories: genel
tags: [go, golang]
---

Bu yazıda Go programlama dilindeki array ve slice tiplerinden bahsedeceğim

## Array

---

<br>Array sabit uzunluklu, belirli bir tipte, sıfır veya daha fazla elemanlı, birbiri ardına gelen dizidir. Sabit uzunluklu olması sebebiyle Go dilinde nadiren kullanılır.

Array ın kapasitesi oluşturulurken belirtilir ve sonradan değiştirilemez.

{% highlight go %}
fruits := [4]string{}
{% endhighlight %}

Array in elemanları array oluşturulurken de girilebilir.

{% highlight go %}
fruits := 2[]string{"Elma", "Armut"}
{% endhighlight %}

Array in her bir elemanının zaro value su array in tipinin zero value sudur.

{% highlight go %}
fruits :=2[]string{} // " ", " "
numbers := 3[]int{} // 0, 0, 0
{% endhighlight %}

Birçok dilde olduğu gibi Go dilinde de array in ilk elemanı 0(sıfır) dan başlar

{% highlight go %}
fruits := 2[]string{"Elma", "Armut"}
fmt.Println(fruits[0]) // Elma
{% endhighlight %}

*len* fonksiyonu array içinde kaç tane eleman olduğu bilgisini döner
{% highlight go %}
f := [2]string{"Elma", "Armut"}
fmt.Println(len(f)) //2
{% endhighlight %}

Eğer array in elemanlarının tipi karşılaştırılabilir ise array de karşılaştırılabilirdir ve == operatörü ile eşit, != operatörü ile eşit değil karşılaştırması yapabiliriz.
{% highlight go %}
s1 := [2]string{"Elma", "Armut"}
s2 := [2]string{"Elma", "Armut"}

if s1 == s2{
    fmt.Println("2 array eşit")
}

//2 array eşit
{% endhighlight %}

Bir fonksiyon çağırıldığı zaman, her bir argümanın değerinin kopyası karşılık gelen parametre değerine atanır. Yani fonksiyon kopyayı alır orijinali değil. Fonksiyonlara argüman olarak büyük array ler geçmek efektif değildir ve fonksiyonun yapacağı değişiklikler kopyayı etkiler, orijinali değil.

Fonksiyona array ı işaret eden pointer argüman olarak geçilebilir. Bu sayede orijinal array üzerinde değişiklik yapılabilir. Fakat yine de array ler sabit uzunluklu olmaları sebebiyle efektif değildirler.

Go dilinde array ler fonksiyon parametresi ve dönüş değeri olarak nadiren kullanılırlar. Bunun yerine **slice** tercih edilir

## Slice

---

<br>Slice değişken uzunluklu, belirli bir tipte, sıfır veya daha fazla elemanlı, birbiri ardına gelen dizidir. Slice array e benzer fakat slice tanımlarken uzunluk belirtmeyiz


{% highlight go %}
s := []string
i := []int
{% endhighlight %}

*len* fonksiyonu slice ın kaç elemana sahip olduğu söyler, *cap* fonksiyonu ise slice ın kapasitesini, bir başka deyişle slice ın kaç tane elemana sahip olabileceğini söyler.
Slice aynı zamanda make anahtar sözcüğü ile de oluşturulabilir


{% highlight go %}
s := make([]string, 1, 3)
{% endhighlight %}

make slice oluşturulurken *length* ve *capacity* değerlerini verebilmemizi sağlar

Yukarıdaki örnekte her ne kadar ekstra kapasite allocate etmiş olsak da, değer atayana kadar ekstra kapasiteye erişim hakkımız yoktur.

Bir slice ın subetini aldığımızda eğer subseti mutate edersek, orijinal slice ı da mutate etmiş oluruz. Bunun önüne geçmek içi copy kullanabiliriz


{% highlight go %}
original := []string{"elma", "armut"}
ref := original
dup := make([]string, len(original))
copy(dup, original)

ref[0] = "portakal"
original[1] = "muz"
fmt.Println("original: ", original)
fmt.Println("ref:      ", ref)
fmt.Println("dup:      ", dup)

//original:  [portakal muz]
//ref:       [portakal muz]
//dup:       [elma armut]
{% endhighlight %}
