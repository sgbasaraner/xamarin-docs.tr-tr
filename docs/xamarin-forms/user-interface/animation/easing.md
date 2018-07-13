---
title: Xamarin.Forms içinde kolaylaştırıcı İşlevler
description: Xamarin.Forms nasıl animasyonları hızını artırın veya gibi çalıştırdıkları yavaşlamasına denetleyen bir aktarım işlevi belirtmek için sağlayan bir hareket hızı sınıfı içerir. Bu makalede, önceden tanımlanmış kolaylaştırıcı işlevler kullanma ve özel kolaylaştırıcı işlevler oluşturma işlemini gösterir.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 1c75771173d94a18c7c1cc5100c64d45bdc32078
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998127"
---
# <a name="easing-functions-in-xamarinforms"></a>Xamarin.Forms içinde kolaylaştırıcı İşlevler

_Xamarin.Forms nasıl animasyonları hızını artırın veya gibi çalıştırdıkları yavaşlamasına denetleyen bir aktarım işlevi belirtmek için sağlayan bir hareket hızı sınıfı içerir. Bu makalede, önceden tanımlanmış kolaylaştırıcı işlevler kullanma ve özel kolaylaştırıcı işlevler oluşturma işlemini gösterir._


[ `Easing` ](xref:Xamarin.Forms.Easing) Sınıfı animasyonları tarafından tüketilebilecek kolaylaştırıcı işlevler bir dizi tanımlar:

- [ `BounceIn` ](xref:Xamarin.Forms.Easing.BounceIn) Geri dönmeler başında animasyonu hızlandırma işlevi.
- [ `BounceOut` ](xref:Xamarin.Forms.Easing.BounceOut) Geri dönmeler sonunda animasyonu hızlandırma işlevi.
- [ `CubicIn` ](xref:Xamarin.Forms.Easing.CubicIn) Hızlandırma işlevi yavaş animasyonu hızlandırır.
- [ `CubicInOut` ](xref:Xamarin.Forms.Easing.CubicInOut) Hızlandırma işlevi başında animasyonu hızlandırır ve sonunda animasyon decelerates.
- [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut) Decelerates animasyon hızla hızlandırma işlevi.
- [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) Hızlandırma işlevi, bir sabit hız kullanır ve olan varsayılan hızlandırma işlevi.
- [ `SinIn` ](xref:Xamarin.Forms.Easing.SinIn) Hızlandırma işlevi sorunsuz animasyonu hızlandırır.
- [ `SinInOut` ](xref:Xamarin.Forms.Easing.SinInOut) Hızlandırma işlevi sorunsuz başında animasyonu hızlandırır ve sorunsuzca animasyon sonunda decelerates.
- [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut) Decelerates animasyon sorunsuz hızlandırma işlevi.
- [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) Hızlandırma işlevi çok hızlı bir şekilde sonuna doğru hızlandırmak animasyon neden olur.
- [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) Neden olan hızla sonuna doğru yavaşlatma animasyonu hızlandırma işlevi.

`In` Ve `Out` sonekleri son veya her ikisini animasyon başındaki belirgin kolaylaştırıcı işlevi tarafından sağlanan etkin olup olmadığını gösterir.

Ayrıca, özel kolaylaştırıcı işlevler oluşturulabilir. Daha fazla bilgi için [özel kolaylaştırıcı işlevler](#customeasing).

## <a name="consuming-an-easing-function"></a>Kolaylaştırıcı bir işlev kullanma

Animasyon uzantı yöntemleri [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) sınıfı aşağıdaki kod örneğinde gösterildiği gibi son yöntem parametresi olarak belirtilmesi üzere kolaylaştırıcı bir işlev izin ver:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Bir animasyonun kolaylaştırıcı bir işlev belirterek, animasyon hızı doğrusal olmayan hale gelir ve Hızlandırma işlevini tarafından sağlanan efekti oluşturur. Kolaylaştırıcı bir işlev bir animasyon oluştururken atlama neden olan varsayılan kullanmak animasyon [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) doğrusal hız oluşturan işlevi.

Animasyon uzantı yöntemleri kullanma hakkında daha fazla bilgi için [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) sınıfı [basit animasyonlar](~/xamarin-forms/user-interface/animation/simple.md). Kolaylaştırıcı işlevler ayrıca tarafından kullanılabilir [ `Animation` ](xref:Xamarin.Forms.Animation) sınıfı. Daha fazla bilgi için [özel animasyonlar](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Kolaylaştırıcı işlevler özel

Özel kolaylaştırıcı bir işlev oluşturma için üç ana yaklaşım vardır:

1. Alan bir yöntem oluşturma bir `double` döndürür ve bağımsız değişken bir `double` sonucu.
1. Oluşturma bir `Func<double, double>`.
1. Bağımsız değişkeni olarak hızlandırma işlevini belirtin [ `Easing` ](xref:Xamarin.Forms.Easing) Oluşturucusu.

Her üç durumda özel hızlandırma işlevini bağımsız değişken 1 için 0 ve 1 bağımsız değişken için 0 döndürmelidir. Bununla birlikte, bağımsız değişken değeri 0 ile 1 arasında herhangi bir değer döndürülebilir. Her bir yaklaşıma artık sırasıyla açıklanmıştır.

### <a name="custom-easing-method"></a>Özel yöntem hızlandırma

Özel bir hızlandırma işlevini alan bir yöntem tanımlanabilir bir `double` döndürür ve bağımsız değişken bir `double` aşağıdaki kod örneğinde gösterildiği gibi sonuç:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` Yöntemi gelen değer 0, 0.2, 0.4, 0,6, 0.8 ve 1 değerlerine keser. Bu nedenle, [ `Image` ](xref:Xamarin.Forms.Image) örneği ayrık atlar, yerine sorunsuz çevrilir.

### <a name="custom-easing-func"></a>Özel FUNC hızlandırma

Özel kolaylaştırıcı bir işlev olarak da tanımlanabilir bir `Func<double, double>`, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func` Başlangıcıdır kolaylaştırıcı bir işlev hızlı yavaşlar kurs tersine çevirir ve ardından tersine temsil kurs yeniden hızla sonuna doğru hızlandırmak için. Bu nedenle, genel hareketini while [ `Image` ](xref:Xamarin.Forms.Image) örneğidir aşağı yönde, kurs animasyon olan sürenin yarısına ulaşıldığında geçici olarak tersine çevirir.

### <a name="custom-easing-constructor"></a>Özel oluşturucu hızlandırma

Özel kolaylaştırıcı bir işlev bağımsız değişkeni olarak da tanımlanabilir [ `Easing` ](xref:Xamarin.Forms.Easing) oluşturucusu, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Özel kolaylaştırıcı işlevi lambda işlevi bağımsız değişkeni olarak belirtilen [ `Easing` ](xref:Xamarin.Forms.Easing) oluşturucusu ve kullanımları `Math.Cos` tarafından bırakılan bir yavaş bırakma etkisi yöntemini `Math.Exp` yöntemi. Bu nedenle, [ `Image` ](xref:Xamarin.Forms.Image) örneği son işi olmayan kullanışlılığını bırakmak görünmesi çevrilir.

## <a name="summary"></a>Özet

Bu makalede, önceden tanımlanmış kolaylaştırıcı işlevler kullanma ve özel kolaylaştırıcı işlevleri nasıl oluşturabileceğinizi gösterdik. Xamarin.Forms içeren bir [ `Easing` ](xref:Xamarin.Forms.Easing) nasıl animasyonları hızlandırma denetleyen bir aktarım işlevi belirtin veya gibi çalıştırdıkları yavaşlamasına sağlar sınıfını.



## <a name="related-links"></a>İlgili bağlantılar

- [Zaman Uyumsuz Desteğe Genel Bakış](~/cross-platform/platform/async.md)
- [Kolaylaştırıcı işlevler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Hızlandırma](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
