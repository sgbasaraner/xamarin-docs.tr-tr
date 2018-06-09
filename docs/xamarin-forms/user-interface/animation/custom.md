---
title: Xamarin.Forms özel animasyonları
description: Bu makalede Xamarin.FOrms animasyon sınıfı oluşturun ve animasyonları iptal etme, birden çok animasyon eşitlemek için ve varolan animasyon yöntemlerle animasyonlu değildir özelliklerine animasyon özel animasyon oluşturma nasıl kullanılacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 74430f6c158e74569f1b2cbfa0b6a85e8d40fbcf
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242982"
---
# <a name="custom-animations-in-xamarinforms"></a>Xamarin.Forms özel animasyonları

_Animasyon sınıfı, bir veya daha fazla animasyon nesneleri oluşturma ViewExtensions sınıfında genişletme yöntemleri ile tüm Xamarin.Forms animasyonların yapı taşıdır. Bu makalede, animasyon sınıfı oluşturun ve animasyonları iptal etme, birden çok animasyon eşitlemek için kullanın ve var olan animasyon yöntemlerle animasyonlu değildir özelliklerine animasyon özel animasyon oluşturma gösterilmektedir._


Parametre sayısı oluştururken belirtilmelidir bir `Animation` hareketli özelliğinin başlangıç ve bitiş değerleri dahil olmak üzere nesnenin ve özelliğin değerini değiştiren bir geri çağırma. Bir `Animation` nesnesi de çalıştırın ve eşitlenen bir alt animasyon koleksiyonu korumak. Daha fazla bilgi için bkz: [alt animasyonları](#child).

İle oluşturulmuş bir animasyon çalıştıran [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) olabilir veya alt animasyonları içermeyebilir sınıfını çağırarak elde edilir [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) yöntemi. Bu yöntem animasyonun ve diğer öğeleri arasında gösterilip animasyonun yinelenecek denetler bir geri çağırma süresini belirtir.

## <a name="creating-an-animation"></a>Bir animasyon oluşturma

Oluştururken bir [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) tipik olarak, aşağıdaki kod örneğinde gösterildiği gibi üç parametre en az gerekli nesnesi:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Bu kod bir animasyon tanımlar [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) özelliği bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) bir değerden 1'örneğinin 2 değeri. Xamarin.Forms tarafından türetilmiş, animasyonlu değer ilk bağımsız değişkeni olarak belirtilen geri dönüş değerini değiştirmek için kullanıldığı geçirilir `Scale` özelliği.

Animasyonun bir çağrı kullanmaya [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Unutmayın [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) döndürmemesi bir `Task` nesnesi. Bunun yerine, bildirimler geri arama yöntemleri sağlanır.

Şu bağımsız değişkenleri belirtilen `Commit` yöntemi:

- İlk bağımsız değişken (*sahibi*) animasyon sahibini tanımlar. Bu, animasyonun uygulandığı bir görsel öğe ya da sayfa gibi başka bir görsel öğe olabilir.
- İkinci bağımsız değişken (*adı*) bir adla animasyon tanımlar. Ad, animasyonun benzersiz şekilde tanımlamak için sahibiyle birleştirilir. Bu benzersiz tanımlayıcıya sonra animasyonun çalışır durumda olup olmadığını belirlemek için kullanılabilir ([`AnimationIsRunning`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AnimationIsRunning/p/Xamarin.Forms.IAnimatable/System.String/)), ya da iptal etmek ([`AbortAnimation`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/)).
- Üçüncü bağımsız değişken (*oranı*) tanımlanan geri çağırma yöntemine yapılan her çağrı arasındaki milisaniye sayısını gösterir. [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Oluşturucusu
- Dördüncü bağımsız değişken (*uzunluğu*) milisaniye cinsinden animasyonun süresini gösterir.
- Beşinci bağımsız değişken (*kolaylaştırma*) animasyon kullanılacak hareket hızı işlevi tanımlar. Alternatif olarak, hareket hızı işlevi bağımsız değişken olarak belirtilebilir [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Oluşturucusu. İşlevler kolaylaştırma hakkında daha fazla bilgi için bkz: [kolaylaştırma işlevleri](~/xamarin-forms/user-interface/animation/easing.md).
- Altıncı bağımsız değişken (*tamamlandı*) animasyon tamamlandıktan sonra yürütülecek bir geri çağırma olduğu. Bu geri çağırma son değeri ve ikinci bağımsız değişkeni olan gösteren ilk bağımsız değişkeni ile iki bağımsız değişken almayan bir `bool` ayarlanmış `true` animasyon iptal edilirse. Alternatif olarak, *tamamlandı* geri çağırma bağımsız değişken olarak belirtilebilir [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Oluşturucusu. Bununla birlikte, tek bir animasyonu varsa *tamamlandı* geri aramalar hem de belirtilir `Animation` oluşturucusu ve `Commit` yöntemi, yalnızca belirtilen geri çağırma `Commit` yöntemi yürütülür.
- Yedinci bağımsız değişken (*yineleyin*) animasyonun yinelenmesi verir aramasıdır. Animasyon sonunda çağrılır ve döndürme `true` animasyonun yinelenmesi gerektiğini gösterir.

Genel etki artırır animasyonun oluşturmaktır [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) özelliği bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 1'den 2 ' den 2 kullanarak saniye (2000 milisaniye) [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) işlevi kolaylaştırma. Animasyon tamamlandıktan, her zaman kendi `Scale` özelliği 1 olarak sıfırlar ve animasyonu yineler.

> [!NOTE]
> Diğer bağımsız olarak çalıştırılan eşzamanlı bir animasyon oluşturarak olacak oluşturulan bir `Animation` nesne her animasyonun ve ardından çağırma `Commit` her animasyon yöntemi.

<a name="child" />

### <a name="child-animations"></a>Alt animasyonları

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Sınıfı ayrıca oluşturulmasını ilgilendirir alt animasyonları destekler bir `Animation` hangi diğer nesneye `Animation` nesneleri eklenir. Bu, bir dizi çalıştırıp eşitlenen animasyonları sağlar. Aşağıdaki kod örneğinde, oluşturma ve alt animasyonları çalıştırma gösterilmektedir:

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

Alternatif olarak, kod örneği daha özlü biçimde, aşağıdaki kod örneğinde kanıtlanabilir yazılabilir:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Her iki kod örnekleri, bir üst içinde [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) nesnesi oluşturulur, ek olduğu `Animation` nesneleri sonra eklenir. İlk iki bağımsız değişken [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) yöntemi başlamak ve alt animasyon bitiş zamanı belirtin. Bağımsız değişken değerleri 0 ile 1 arasında olmalıdır ve belirtilen alt animasyon etkin olacağını üst animasyon göreli döneminde temsil eder. Bu nedenle, bu örnekte `scaleUpAnimation` animasyon ilk yarısı için etkin olur `scaleDownAnimation` animasyon ikinci yarısında için etkin olur ve `rotateAnimation` tüm süresince etkin olacaktır.

Genel etki animasyonun 4 saniye (4000 milisaniye) gerçekleşir. `scaleUpAnimation` Canlandırır [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 1 özelliğine 2 ' den 2 saniye. `scaleDownAnimation` Sonra canlandırır `Scale` 2 özelliğine 1 ' den 2 saniye. Her iki ölçek animasyonları yaşanan olsa da, `rotateAnimation` canlandırır [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 0 özelliğinden 360, üzerinde 4 saniye. Ölçeklendirme animasyonları hareket hızı işlevleri kullandığını unutmayın. [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) İşlevi kolaylaştırma neden [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) başlangıçta daha büyük almadan önce daraltmak için ve [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) işlevi kolaylaştırma neden `Image` tam animasyon sonuna gerçek boyutundan daha küçük olacak.

Bir dizi arasındaki farklar vardır bir [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) alt animasyon kullanır nesne ve değil biri:

- Alt animasyonları kullanırken *tamamlandı* alt animasyon üzerinde geri arama gösterir zaman alt tamamlandı ve *tamamlandı* geri çağırma geçirilen `Commit` yöntemi gösterir Animasyonun tamamını tamamlandı.
- Alt animasyonları kullanırken, döndürme `true` gelen *yineleyin* üzerinde geri arama `Commit` yöntemi animasyonun yineler değil neden olur, ancak animasyon yeni değerleri çalışmaya devam eder.
- Bir hareket hızı işlevinde dahil ederken `Commit` yöntemi ve hareket hızı işlevi döndürür değeri 1'den büyük, animasyonun sonlandırılacak. Hareket hızı işlevi 0'dan küçük bir değer döndürürse, değeri 0 olarak clamped. 0'den küçük ya da 1'den büyük bir değer döndüren bir hareket hızı işlevi kullanmak için onu belirtilmesi birinde yerine alt animasyonların `Commit` yöntemi.

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Sınıfı ayrıca içerir [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/) alt animasyonları bir üst öğeye eklemek için kullanılan yöntemleri `Animation` nesnesi. Ancak, kendi *başlamak* ve *son* bağımsız değişken değerleri 0 ile 1 sınırlı değildir, ancak yalnızca 0 ve 1 aralığına karşılık gelen alt animasyon kısmı etkin olacaktır. Örneğin, varsa bir `WithConcurrent` yöntem çağrısı hedefleyen bir alt animasyon tanımlayan bir [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) özelliğinden 1-6 ancak ile *başlamak* ve *son* değerleri -2 ve 3 ' ü *başlamak* -2 değerini karşılık gelen bir `Scale` 1 değerini ve *son* değeri 3 karşılık gelen bir `Scale` 6 değeri. Değerler 0 ile 1 aralığının dışında hiçbir bölümü animasyonda, yürütmek için `Scale` özellik yalnızca animasyonlu 3 ile 6 '.

## <a name="canceling-an-animation"></a>Bir animasyon iptal etme

Bir uygulama bir animasyon çağrısıyla iptal edebilirsiniz [ `AbortAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/) genişletme yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Animasyon animasyon sahibi ve animasyon adının birleşimiyle benzersiz şekilde tanımlanır unutmayın. Bu nedenle, sahip ve ad, animasyonun çalıştıran animasyonun iptal etmek için belirtilmemiş belirtilmelidir. Bu nedenle, kod örneği hemen adlı animasyon iptal edilecek `SimpleAnimation` sayfa tarafından ait.

## <a name="creating-a-custom-animation"></a>Özel Animasyon oluşturma

Burada kadarki örneklerde eşit yöntemleri ile elde edilebilir animasyonları göstermiştir [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) sınıfı. Ancak, avantajı [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) sınıftır animasyonlu değeri değiştiğinde yürütülen geri çağırma yöntemi erişimi olan. Bu, istenen tüm animasyon uygulamak geri çağırma sağlar. Örneğin, aşağıdaki kod örneğinde canlandırır [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) ayarlayarak sayfanın özelliğini [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) tarafından oluşturulan değerleri [ `Color.FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/)0 ile 1 arasında değişen ton değerlerle yöntemi:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Sonuçta elde edilen animasyon sayfası arka plan renklerini şifre aracılığıyla ilerledikten görünümünü sağlar.

Bezier eğrisi animasyon dahil olmak üzere, karmaşık bir animasyon oluşturma daha fazla örnek için bkz: [Bölüm 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) , [Xamarin.Forms ile Mobile Apps oluşturma](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Özel Animasyon genişletme yöntemi oluşturma

Genişletme yöntemleri [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) sınıfı, geçerli değeri bir özellikten belirli bir değere hareketli hale getirmeyi. Bu, oluşturmak, örneğin, zorlaştırır bir `ColorTo` bir değer renkten diğerine animasyon için çünkü kullanılabilir animasyon yöntemi:

- Yalnızca [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) özelliği tarafından tanımlanan [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) sınıf [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), hangi değil her zaman istenen `Color` özelliği animasyon uygulamak için.
- Genellikle geçerli değeri bir [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) özelliği [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), gerçek bir renk değil ve hangi kullanılamaz ilişkilendirme hesaplamalarda.

Bu sorun için çözüm sahip olmaktır `ColorTo` yöntemi, belirli bir hedef [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) özelliği. Bunun yerine, Ara değerli geçirmeden bir geri çağırma yöntemi yazılabilir `Color` çağırana geri değeri. Ayrıca, yöntemi başlangıç alın ve bitiş `Color` bağımsız değişkenler.

`ColorTo` Yöntemi kullanan bir genişletme yöntemi uygulanabilir [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) yönteminde [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) işlevselliği sağlamak için sınıf. Bunun nedeni, `Animate` yöntemi türü olmayan hedef özellikleri kullanılabilir `double`, aşağıdaki kod örneğinde gösterildiği gibi:

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

[ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) Yöntemi gerektiren bir *dönüştürme* bağımsız değişkeni bir geri çağırma yöntemi. Bu geri çağırma giriş her zaman olduğu bir `double` 0 ile 1 arasında. Bu nedenle, `ColorTo` yöntemi tanımlar kendi dönüştürme `Func` kabul eden bir `double` 0 ile 1 ve bu geri döner arasında değişen bir [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) bu değerine karşılık gelen değer. `Color` Değeri enterpolasyonla tarafından hesaplanır [ `R` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/), [ `G` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/), [ `B` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/), ve [ `A` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/) sağlanan iki değerlerini `Color` bağımsız değişkenler. `Color` Değer belirli bir özellik için uygulama için geri çağırma yöntemine geçirilen sonra.

Bu yaklaşım sağlar `ColorTo` herhangi bir animasyon yöntemi [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) özelliği, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

Bu kod örneğinde, `ColorTo` yöntemi canlandırır [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) ve [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) özelliklerini bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), `BackgroundColor`bir sayfanın özelliğini ve [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) özelliği bir [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/).

## <a name="summary"></a>Özet

Bu makalede nasıl kullanılacağı gösterilmiştir [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) oluşturmak ve animasyonları iptal, birden çok animasyon eşitlemek ve varolan animasyonun animasyonlu değildir özelliklerine animasyon özel animasyon oluşturmak için sınıfı yöntemleri. `Animation` Tüm Xamarin.Forms animasyonları Yapı bloğu bir sınıftır.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Animasyon (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Animasyon](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)
- [AnimationExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)
