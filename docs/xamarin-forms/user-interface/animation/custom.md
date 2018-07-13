---
title: Xamarin.Forms içinde özel animasyon
description: Bu makalede, oluşturmak ve animasyonları iptal, birden çok animasyon eşitlemek için Xamarin.FOrms animasyonu sınıfı kullanın ve var olan bir animasyon yöntemlerle animasyonlu olmayan özelliklerine animasyon özel animasyonlar oluşturma gösterilmektedir.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 519368031384e72a2d2e0a7c99053be44ea4cffc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995227"
---
# <a name="custom-animations-in-xamarinforms"></a>Xamarin.Forms içinde özel animasyon

_Animasyon sınıfı, bir veya daha fazla animasyon nesneleri oluşturma ViewExtensions sınıfı alanında uzantı yöntemlerini içeren tüm Xamarin.Forms animasyon yapı taşıdır. Bu makalede, oluşturmak ve animasyonları iptal, birden çok animasyon eşitlemek için animasyon sınıfı kullanın ve var olan bir animasyon yöntemlerle animasyonlu olmayan özelliklerine animasyon özel animasyonlar oluşturma gösterilmektedir._


Bir dizi parametre oluştururken belirtilmelidir bir `Animation` animasyon uygulanan özelliğinin başlangıç ve bitiş değerlerini de dahil olmak üzere, nesne ve özelliğin değerini değiştiren bir geri çağırma. Bir `Animation` nesne de çalıştıran ve eşitlenmiş alt animasyonları koleksiyonunu sağlamak. Daha fazla bilgi için [alt animasyonları](#child).

İle oluşturulan bir animasyon çalıştıran [ `Animation` ](xref:Xamarin.Forms.Animation) olabilir veya alt animasyonları içermeyebilir sınıfını çağrılarak gerçekleştirilir [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) yöntemi. Bu yöntem, animasyonun ve diğer öğeler arasında animasyonu yineleme sorulmayacağını denetler bir geri çağırma süresi belirtir.

## <a name="creating-an-animation"></a>Bir animasyon oluşturma

Oluştururken bir [ `Animation` ](xref:Xamarin.Forms.Animation) genellikle, aşağıdaki kod örneğinde gösterildiği gibi en az üç parametre gerekli, nesne:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Bu kod, bir animasyon tanımlar [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) özelliği bir [ `Image` ](xref:Xamarin.Forms.Image) örneğinden bir değer olan 1 2 değerini. Xamarin.Forms ile elde edilir, animasyonlu değeri ilk bağımsız değişkeni belirtilen geri dönüş değerini değiştirmek için kullanıldığı geçirilir `Scale` özelliği.

Animasyonun bir çağrıyla başlatılır [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Unutmayın [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) yöntemi bir `Task` nesne. Bunun yerine, bildirimler, geri çağırma yöntemleri ile sağlanır.

Şu bağımsız değişkenler belirtilen `Commit` yöntemi:

- İlk bağımsız değişken (*sahibi*) animasyon sahibi tanımlar. Bu, animasyonun uygulandığı bir görsel öğe veya sayfa gibi başka bir görsel öğe olabilir.
- İkinci bağımsız değişkeni (*adı*) bir ada sahip bir animasyon tanımlar. Ad, animasyonun benzersiz olarak tanımlanabilmesi için sahip birleştirilir. Bu benzersiz tanımlayıcıya sonra animasyon çalışıp çalışmadığını belirlemek için kullanılabilir ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String))), veya iptal etmek için ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))).
- Üçüncü bağımsız değişken (*oranı*) tanımlı geri çağırma yöntemine yapılan her çağrı arasındaki milisaniye sayısını gösteren [ `Animation` ](xref:Xamarin.Forms.Animation) Oluşturucusu
- Dördüncü bağımsız değişken (*uzunluğu*) milisaniye cinsinden animasyonun süresini gösterir.
- Beşinci bağımsız değişken (*hızlandırma*) animasyon kullanılacak hızlandırma işlevini tanımlar. Alternatif olarak, hızlandırma işlevini bağımsız değişken olarak belirtilebilir [ `Animation` ](xref:Xamarin.Forms.Animation) Oluşturucusu. Kolaylaştırıcı işlevler hakkında daha fazla bilgi için bkz. [kolaylaştırıcı işlevler](~/xamarin-forms/user-interface/animation/easing.md).
- Altıncı bağımsız değişken (*tamamlandı*) olan animasyon tamamlandığında yürütülecek bir geri çağırma. Bu geri çağırma son değer ve ikinci bağımsız değişkeni olan gösteren ilk bağımsız değişken ile iki bağımsız değişkeni alır bir `bool` ayarlanmış `true` animasyon iptal edildiyse. Alternatif olarak, *tamamlandı* geri çağırma bağımsız değişken olarak belirtilebilir [ `Animation` ](xref:Xamarin.Forms.Animation) Oluşturucusu. Bununla birlikte, tek bir animasyon değilse *tamamlandı* geri çağırmaları her ikisinde de belirtilirse `Animation` oluşturucusu ve `Commit` yöntemi, yalnızca belirtilen geri çağırma `Commit` yöntemi yürütülür.
- Yedinci bağımsız değişken (*yineleyin*) animasyonun yinelenmesi sağlayan aramasıdır. Animasyon sonunda çağrılır ve döndürerek `true` animasyonun yinelenmesi gösterir.

Genel etki artıran animasyon oluşturmaktır [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) özelliği bir [ `Image` ](xref:Xamarin.Forms.Image) 2 ' den 2 kullanarak saniyeler içinde (2000 milisaniye cinsinden), 1'den [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) hızlandırma işlevi. Animasyon tamamlandıktan, her zaman kendi `Scale` özelliği 1 olarak sıfırlanır ve animasyonu yineler.

> [!NOTE]
> Birbirinden çalıştıran eşzamanlı animasyonları oluşturarak değişkenden oluşan bir `Animation` nesne için her bir animasyon ve ardından arama `Commit` her animasyonu yöntemi.

<a name="child" />

### <a name="child-animations"></a>Alt animasyonları

[ `Animation` ](xref:Xamarin.Forms.Animation) Sınıfı da oluşturulmasını ilgilendirir alt animasyonları destekler bir `Animation` hangi diğer nesne `Animation` nesneleri eklenir. Bu, bir dizi çalıştırın ve eşitlenmesi animasyonları sağlar. Aşağıdaki kod örneği, oluşturma ve çalıştırma alt animasyonları gösterir:

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Alternatif olarak, kod örneği daha kısaca, aşağıdaki kod örneğinde kanıtlanabilir olarak yazılabilir:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Her iki kod örnekleri, bir üst öğe içinde [ `Animation` ](xref:Xamarin.Forms.Animation) nesne oluşturulur, ek `Animation` nesneleri a eklenir. İlk iki bağımsız değişkenleri [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) yöntemi ne zaman başlar ve son alt animasyon belirtin. Bağımsız değişken değerleri 0 ile 1 arasında olması ve göreli süre içinde belirtilen alt animasyon etkin olacaktır üst animasyon temsil eder. Bu nedenle, bu örnekte `scaleUpAnimation` ilk yarısında, animasyon için etkin olacaktır `scaleDownAnimation` ikinci yarısında animasyon için etkin olacaktır ve `rotateAnimation` sürenin tamamı boyunca etkin olacaktır.

Genel etki animasyon 4 saniyenin üzerindeki (4000 milisaniye cinsinden) gerçekleşir. `scaleUpAnimation` Canlandırır [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) özelliğini 1'den 2 ' den 2 saniye. `scaleDownAnimation` Ardından canlandırır `Scale` özelliğini 1, 2 saniyenin üzerindeki 2. Her iki ölçeklendirme animasyonları gerçekleşirken, `rotateAnimation` canlandırır [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 0-360, 4 saniyenin üzerindeki özelliği. Ölçeklendirme animasyonlara kolaylaştırıcı işlevler kullanmanız gerektiğini unutmayın. [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) Hızlandırma işlevi neden [ `Image` ](xref:Xamarin.Forms.Image) başlangıçta daha büyük almadan önce küçültmek ve [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) hızlandırma işlevi neden `Image` tam animasyon sonuna doğru gerçek boyutundan daha küçük olacak.

Bir dizi arasındaki farklar vardır bir [ `Animation` ](xref:Xamarin.Forms.Animation) alt animasyonlarını kullanıp bir nesne ve olmayan bir:

- Alt animasyonları kullanırken *tamamlandı* alt animasyon üzerinde geri arama alt tamamlandığında gösterir ve *tamamlandı* geçirilen geri çağırma `Commit` yöntemi gösterir tüm animasyon tamamlandı.
- Alt animasyonları kullanırken döndüren `true` gelen *yineleyin* üzerinde geri arama `Commit` yöntemi yinelemek animasyon değil neden olur, ancak animasyon yeni değerler olmadan çalışmaya devam eder.
- Kolaylaştırıcı bir işlev içinde dahil ederken `Commit` yöntemi hızlandırma işlevini döndürür ve bir değer 1'den büyük, animasyon sonlandırılacak. Hızlandırma işlevini 0'dan küçük bir değer döndürürse, değeri 0 olarak sıkıştırılır. 0'den küçük ya da 1'den büyük bir değer döndüren bir hızlandırma işlevini kullanmak için gereken belirtilen bir alt animasyon yerine `Commit` yöntemi.

[ `Animation` ](xref:Xamarin.Forms.Animation) Sınıfı da içeren [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) bir üst öğeye alt animasyon eklemek için kullanılan yöntemleri `Animation` nesne. Ancak, kendi *başlamak* ve *son* bağımsız değişken değerleri 0 ile 1 sınırlı değildir, ancak bu bölümü yalnızca 0 ile 1 aralığına karşılık gelen alt animasyonun etkin olacaktır. Örneğin, bir `WithConcurrent` yöntem çağrısının hedefleyen bir alt animasyon tanımlar bir [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) özelliği 1'den 6 ancak ile *başlamak* ve *son* değerleri -2 ve 3 ' ü *başlamak* -2 değerini karşılık gelen bir `Scale` 1 değerini ve *son* 3 değerini karşılık gelen bir `Scale` 6 değeri. Bir animasyon değerleri 0 ile 1 aralığının dışında hiçbir bölümü aldığından `Scale` özelliği yalnızca animasyonlu 3 ila 6.

## <a name="canceling-an-animation"></a>Bir animasyon iptal ediliyor

Bir uygulama bir çağrı ile animasyon iptal edebilirsiniz [ `AbortAnimation` ](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)) genişletme yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Not animasyonları animasyon sahibi ve animasyon adının bir birleşimi tarafından benzersiz şekilde tanımlanır. Bu nedenle, sahip ve ad, animasyonun çalıştıran animasyon iptal etmek için belirtilmelidir belirtilen. Bu nedenle, kod örneği hemen adlı animasyonu iptal edilecek `SimpleAnimation` sayfa tarafından ait.

## <a name="creating-a-custom-animation"></a>Özel Animasyon oluşturma

Buraya kadar gösterilen örneklerden eşit yöntemleri ile elde edilebilir animasyonları sergilemiştir [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) sınıfı. Ancak avantajı [ `Animation` ](xref:Xamarin.Forms.Animation) sınıfı, erişim animasyonlu değeri değiştiğinde yürütülen geri çağırma yöntemine sahip. Bu, istenen tüm animasyon uygulamak geri çağırma sağlar. Örneğin, aşağıdaki kod örneği canlandırır [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) özelliğini ayarlayarak [ `Color` ](xref:Xamarin.Forms.Color) tarafından oluşturulan değerleri [ `Color.FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))yöntemiyle hue değerleri 0 ile 1 arasında:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Elde edilen animasyon, sayfa arka plan renklerini rainbow aracılığıyla ilerledikten görünümünü sağlar.

Bezier eğrisi animasyon dahil olmak üzere, karmaşık animasyonları oluşturmaya daha fazla örnek için bkz. [Bölüm 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) , [Xamarin.Forms ile Mobile Apps oluşturma](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Özel Animasyon genişletme yöntemi oluşturma

Uzantı yöntemleri [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) sınıfı belirtilen değer geçerli değerinden bir özelliğe animasyon ekleme. Bu, oluşturmak, örneğin, zorlaştırır bir `ColorTo` bir değerden bir renk diğerine animasyon uygulamak için kullanılabilir animasyon yöntemi:

- Yalnızca [ `Color` ](xref:Xamarin.Forms.Color) özelliği tarafından tanımlanan [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) sınıfı [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor), hangi değil her zaman istenen `Color` özelliği animasyon uygulamak için.
- Genellikle geçerli değerini bir [ `Color` ](xref:Xamarin.Forms.Color) özelliği [ `Color.Default` ](xref:Xamarin.Forms.Color.Default), gerçek bir renk değil ve hangi kullanılamaz ilişkilendirme hesaplamalarda.

Bu sorunun çözümü sahip olmaktır `ColorTo` yöntemi belirli bir hedef [ `Color` ](xref:Xamarin.Forms.Color) özelliği. Bunun yerine, bir geri çağırma yöntemi ile ilişkilendirilmiş geçirir yazılabilir `Color` çağırana geri değeri. Ayrıca, yöntemi olacaktır ve son `Color` bağımsız değişkenler.

`ColorTo` Yöntemi kullanan bir genişletme yöntemi uygulanabilir [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) yönteminde [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) işlevselliği sağlamak için sınıf. Bunun nedeni, `Animate` yöntemi türü olmayan hedef özellikleri kullanılabilir `double`, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

[ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) Yöntemi gerektiren bir *dönüştürme* bir geri çağırma yöntemi bağımsız değişken. Bu geri arama her zaman giriştir bir `double` 0 ile 1 arasında. Bu nedenle, `ColorTo` yöntemi tanımlar, kendi dönüştürme `Func` kabul eden bir `double` 0- 1 döndürür arasında bir [ `Color` ](xref:Xamarin.Forms.Color) bu değerine karşılık gelen bir değer. `Color` Değeri ilişkilendirme ile hesaplanır [ `R` ](xref:Xamarin.Forms.Color.R), [ `G` ](xref:Xamarin.Forms.Color.G), [ `B` ](xref:Xamarin.Forms.Color.B), ve [ `A` ](xref:Xamarin.Forms.Color.A) sağlanan iki değerlerini `Color` bağımsız değişkenler. `Color` Değeri uygulama için belirli bir özellik için geri çağırma yöntemine geçirilen sonra.

Bu yaklaşım sağlar `ColorTo` herhangi animasyon uygulamak için gereken yöntemini [ `Color` ](xref:Xamarin.Forms.Color) aşağıdaki kod örneğinde gösterildiği gibi özelliği:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

Bu kod örneğinde `ColorTo` yöntemi canlandırır [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) ve [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) özelliklerini bir [ `Label` ](xref:Xamarin.Forms.Label), `BackgroundColor`özelliği bir sayfasının ve [ `Color` ](xref:Xamarin.Forms.BoxView.Color) özelliği bir [ `BoxView` ](xref:Xamarin.Forms.BoxView).

## <a name="summary"></a>Özet

Bu makalede nasıl kullanılacağı gösterilmiştir [ `Animation` ](xref:Xamarin.Forms.Animation) sınıfı oluşturmak ve animasyonları iptal, birden çok animasyonlarına eşitleyebilir ve var olan bir animasyon tarafından animasyonlu olmayan özelliklerine animasyon özel animasyon oluşturmak için yöntemleri. `Animation` Sınıfı, tüm Xamarin.Forms animasyon yapı taşı.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel animasyonlara (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Animasyon](xref:Xamarin.Forms.Animation)
- [AnimationExtensions](xref:Xamarin.Forms.AnimationExtensions)
