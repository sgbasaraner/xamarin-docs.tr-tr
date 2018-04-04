---
title: Kolaylaştırıcı İşlevler
description: Xamarin.Forms nasıl animasyonları hızlandırma veya çalıştırdıkları gibi yavaşlaması denetleyen bir aktarım işlevi belirtmenize olanak tanır bir hareket hızı sınıfı içerir. Bu makalede önceden tanımlanmış hareket hızı işlevlerini kullanma ve nasıl özel hareket hızı işlevler oluşturulacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: e9171b885bdf5958b6969719301a1d7dad51d95b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="easing-functions"></a>Kolaylaştırıcı İşlevler

_Xamarin.Forms nasıl animasyonları hızlandırma veya çalıştırdıkları gibi yavaşlaması denetleyen bir aktarım işlevi belirtmenize olanak tanır bir hareket hızı sınıfı içerir. Bu makalede önceden tanımlanmış hareket hızı işlevlerini kullanma ve nasıl özel hareket hızı işlevler oluşturulacağı gösterilmektedir._


[ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Sınıfı animasyonları tarafından kullanılabilecek hareket hızı işlevleri tanımlar:

- [ `BounceIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) Geri dönmeler başında animasyon işlevi kolaylaştırma.
- [ `BounceOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/) Geri dönmeler sonunda animasyon işlevi kolaylaştırma.
- [ `CubicIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/) İşlevi yavaş kolaylaştırma animasyonun hızlandırır.
- [ `CubicInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/) İşlevi kolaylaştırma başında animasyon hızlandırır ve sonunda animasyon decelerates.
- [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/) Decelerates animasyon işlevi hızla kolaylaştırma.
- [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) İşlevi kolaylaştırma sabit hız kullanır ve varsayılan hareket hızı işlevi.
- [ `SinIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/) İşlevi sorunsuz kolaylaştırma animasyonun hızlandırır.
- [ `SinInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/) İşlevi sorunsuz kolaylaştırma başında animasyon hızlandırır ve sorunsuz sonunda animasyon decelerates.
- [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/) Decelerates animasyon işlevi sorunsuz kolaylaştırma.
- [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) İşlevi kolaylaştırma çok hızlı bir şekilde doğru son hızlandırmak için animasyon neden olur.
- [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) İşlevi kolaylaştırma hızlı bir şekilde doğru son hızını düşürün animasyonun neden olur.

`In` Ve `Out` sonekleri hareket hızı işlevi tarafından sağlanan geçerlilik bitiş veya her ikisini animasyon başındaki belirgin olup olmadığını gösterir.

Ayrıca, özel hareket hızı işlevler oluşturulabilir. Daha fazla bilgi için bkz: [özel kolaylaştırma işlevleri](#customeasing).

## <a name="consuming-an-easing-function"></a>Hareket hızı işlevi kullanma

Animasyon uzantı yöntemleri [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) sınıfı aşağıdaki kod örneğinde gösterildiği gibi son yöntem parametre olarak belirtilen bir hareket hızı işlevi izin ver:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Animasyonun bir hareket hızı işlevi belirterek, animasyon hız doğrusal olmayan haline gelir ve hareket hızı işlevi tarafından sağlanan efekti oluşturur. Bir animasyon oluştururken bir hareket hızı işlevi atlama neden animasyonu varsayılan kullanmak üzere [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) doğrusal hız üreten işlevi kolaylaştırma.

Animasyon uzantı yöntemleri kullanma hakkında daha fazla bilgi için [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) sınıfı için bkz: [basit animasyonları](~/xamarin-forms/user-interface/animation/simple.md). İşlevler kolaylaştırma ayrıca tüketilen tarafından [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) sınıfı. Daha fazla bilgi için bkz: [özel animasyon](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Özel işlevler kolaylaştırma

Özel bir hareket hızı işlev oluşturma için üç ana yaklaşım vardır:

1. Alan bir yöntem oluşturma bir `double` bağımsız değişkeni ve döndürür bir `double` sonucu.
1. Oluşturma bir `Func<double, double>`.
1. Bağımsız değişken olarak hareket hızı işlevi belirtin [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Oluşturucusu.

Her üç durumda özel hareket hızı işlevin 1 bağımsız değişkeni için 0 ve 1 bağımsız değişken için 0 döndürmelidir. Ancak, bağımsız değişken değeri 0 ile 1 arasında herhangi bir değer döndürülebilir. Her iki yaklaşımın şimdi sırayla incelenecektir.

### <a name="custom-easing-method"></a>Özel yöntem kolaylaştırma

Özel bir hareket hızı işlev götüren bir yöntem olarak tanımlanabilir bir `double` bağımsız değişkeni ve döndürür bir `double` , aşağıdaki kod örneğinde gösterildiği şekilde neden:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` Yöntemi 0, 0.2, 0.4, 0,6, 0,8 ve 1 değerlerine gelen değeri tamsayıya dönüştürür. Bu nedenle, [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) örneği ayrık atlar içinde yerine sorunsuz çevrilir.

### <a name="custom-easing-func"></a>Özel FUNC kolaylaştırma

Özel bir hareket hızı işlevi olarak da tanımlanabilir bir `Func<double, double>`, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func` Başlamanızı bir hareket hızı işlevi hızlı yavaşlar indirmelere geri alır ve ardından tersine çevirir temsil kurs yeniden hızlı bir şekilde doğru son hızlandırmak için. Bu nedenle, genel hareketini while [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) örneğidir aşağı doğru hareket, geçici olarak indirmelere üzerinden animasyon yarısı tersine çevirir.

### <a name="custom-easing-constructor"></a>Özel oluşturucu kolaylaştırma

Özel bir hareket hızı işlev bağımsız değişkeni olarak da tanımlanabilir [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) aşağıdaki kod örneğinde gösterildiği şekilde Oluşturucusu:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Özel hareket hızı işlevi bir lambda işlev bağımsız değişkeni olarak belirtilen [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) kullanır ve oluşturucu `Math.Cos` yöntemi tarafından bırakılan bir yavaş açılan efekti oluşturmak için `Math.Exp` yöntemi. Bu nedenle, [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) örneği için son işi olmayan yerine bırakmaya görünmesi çevrilir.

## <a name="summary"></a>Özet

Bu makalede önceden tanımlanmış hareket hızı işlevlerini kullanma ve nasıl özel hareket hızı işlevler oluşturulacağı gösterilmektedir. Xamarin.Forms içeren bir [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) nasıl animasyonları hızlandırma denetleyen aktarımı işlev belirtin veya çalıştırdıkları gibi yavaşlaması sağlayan sınıf.



## <a name="related-links"></a>İlgili bağlantılar

- [Zaman Uyumsuz Desteğe Genel Bakış](~/cross-platform/platform/async.md)
- [İşlevler (örnek) kolaylaştırma](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Kolaylaştırma](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
