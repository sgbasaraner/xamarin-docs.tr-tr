---
title: Basit animasyonları
description: ViewExtensions sınıfı basit animasyonları oluşturmak için kullanılan genişletme yöntemleri sağlar. Bu makalede, oluşturma ve ViewExtensions sınıfını kullanarak animasyonları iptal etme gösterilmektedir.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: 0d2cc30f9bc1ae5602394b8ca2d8e75517a01b54
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="simple-animations"></a>Basit animasyonları

_ViewExtensions sınıfı basit animasyonları oluşturmak için kullanılan genişletme yöntemleri sağlar. Bu makalede, oluşturma ve ViewExtensions sınıfını kullanarak animasyonları iptal etme gösterilmektedir._


[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Sınıfı basit animasyonları oluşturmak için kullanılan aşağıdaki genişletme yöntemleri sağlar:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) canlandırır [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) ve [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) özelliklerini bir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`ScaleTo`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) canlandırır [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) özelliği bir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animasyonlu artımlı artırmak veya azaltmak için geçerlidir [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) özelliği bir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) canlandırır [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) özelliği bir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelRotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animasyonlu artımlı artırmak veya azaltmak için geçerlidir [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) özelliği bir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateXTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) canlandırır [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) özelliği bir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateYTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) canlandırır [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) özelliği bir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`FadeTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) canlandırır [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) özelliği bir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).

Varsayılan olarak, her animasyon 250 milisaniye olur. Bununla birlikte, her animasyon süresi animasyon oluştururken belirtilebilir.

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Sınıfı da içeren bir [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) herhangi bir animasyon iptal etmek için kullanılan yöntem.

> [!NOTE]
> [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) SAX bir [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/) genişletme yöntemi. Ancak, bu yöntem tarafından düzenleri değişiklikleri, konum ve boyut içeren düzeni durumları arasında geçişler animasyon için kullanılması amaçlanmıştır. Bu nedenle, onu yalnızca tarafından kullanılması gereken [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) alt sınıflar.

Animasyon uzantı yöntemleri [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) sınıfı tüm zaman uyumsuz ve dönüş bir `Task<bool>` nesnesi. Dönüş değeri `false` animasyon tamamlarsa ve `true` animasyon iptal edilirse. Bu nedenle, animasyon yöntemleri genellikle kullanılmalıdır `await` kolayca animasyonun zaman tamamlanıp belirlemek mümkün kılar işleci. Ayrıca, bu da önceki yöntemi tamamlandıktan sonra yürütme sonraki animasyon yöntemleriyle sıralı animasyonları oluşturmak mümkün olur. Daha fazla bilgi için bkz: [bileşik animasyonları](#compound).

Bir animasyon izin vermek için bir gereksinimi varsa arka planda tam sonra `await` işleci etmeyebilirsiniz. Bu senaryoda, animasyon genişletme yöntemleri ile arka planda gerçekleşen animasyon animasyon başlatıldıktan sonra hızlı bir şekilde döndürür. Bu işlem, bileşik bir animasyon oluştururken avantajlarından alınabilir. Daha fazla bilgi için bkz: [bileşik animasyonları](#composite).

Hakkında daha fazla bilgi için `await` işleci, bkz: [zaman uyumsuz desteğine genel bakış](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Tek bir animasyon

Her bir genişletme yöntemi [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) bir özellik bir değerden başka bir değere bir süre boyunca aşamalı olarak değişiklikleri tek animasyon işlemi uygular. Bu bölümde her animasyon işlemi araştırır.

### <a name="rotation"></a>Döndürme

Aşağıdaki kod örneğinde kullanımı gösterilir [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) hale getirmeyi yöntemi [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) özelliği bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Bu kod canlandırır [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 2 saniye (2000 milisaniye) kadar 360 derece döndürme tarafından örneği. [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Yöntemi alır Geçerli [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) özellik değeri için animasyon başlangıç ve sonra ilk bağımsız değişken (360) için bu değeri döndürür. Animasyonun olduğunda tamamlandı, görüntünün [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) özelliği 0 olarak sıfırlanır. Bu sağlar `Rotation` özelliği, ek döndürmeleri önleyen animasyon, sonucuna sonra 360 kalır değil.

Aşağıdaki ekran görüntüleri döndürme her platformda işlemde göster:

![](simple-images/rotateto.png "Döndürme animasyon")

### <a name="relative-rotation"></a>Göreli döndürme

Aşağıdaki kod örneğinde kullanımı gösterilir [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) aşamalı olarak artırmak veya azaltmak için yöntemi [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) özelliği bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelRotateTo (360, 2000);
```

Bu kod canlandırır [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 2 saniye (2000 milisaniye) başlangıç konumuna 360 derece döndürme tarafından örneği. [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Yöntemi alır Geçerli [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) özelliği değeri animasyonun başlangıç için ve ardından bu değerden değeri artı ilk bağımsız değişken (360) döndürür. Bu, her animasyon her zaman 360 derece döndürme başlangıç konumundan olmasını sağlar. Bu nedenle, bir animasyon zaten devam ederken yeni bir animasyon çağrılırsa, geçerli konumundan başlar ve artışı 360 derece olmayan bir konumda bitebilir.

Aşağıdaki ekran görüntüleri, her bir platform üzerinde devam eden göreli döndürme göster:

![](simple-images/relrotateto.png "Göreli döndürme animasyon")

### <a name="scaling"></a>ölçeklendirme

Aşağıdaki kod örneğinde kullanımı gösterilir [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) hale getirmeyi yöntemi [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) özelliği bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.ScaleTo (2, 2000);
```

Bu kod canlandırır [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 2 saniye (2000 milisaniye) iki kez boyutuna ölçeklendirmeyi tarafından örneği. [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Yöntemi alır Geçerli [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) animasyon ve ilk bağımsız değişken (2) için o değerinden sonra ölçek başlangıcı için özellik değeri (varsayılan değer olan 1). Bu, iki kez boyutuna görüntü boyutunu genişletme etkisi vardır.

Aşağıdaki ekran görüntüleri, her bir platform üzerinde devam eden ölçeklendirme göster:

![](simple-images/scaleto.png "Animasyon ölçeklendirme")

### <a name="relative-scaling"></a>Göreli ölçeklendirme

Aşağıdaki kod örneğinde kullanımı gösterilir [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) hale getirmeyi yöntemi [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) özelliği bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelScaleTo (2, 2000);
```

Bu kod canlandırır [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 2 saniye (2000 milisaniye) iki kez boyutuna ölçeklendirmeyi tarafından örneği. [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Yöntemi alır Geçerli [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) animasyon ve değerinin yanı sıra, (2) ilk bağımsız değişkeni için bu değerden sonra ölçek başlangıcı için özellik değeri. Bu her animasyon her zaman bir başlangıç konumundan 2 ölçekleme olmasını sağlar.

### <a name="scaling-and-rotation-with-anchors"></a>Ölçekleme ve bağlayıcılarını ile döndürme

[ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) Ve [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) özelliklerini ayarlama ölçekleme veya dönüş Merkezi [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) ve [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) özellikleri. Bu nedenle, bunların değerleri de etkiler [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) ve [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) yöntemleri.

Verilen bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) , yerleştirilen bir düzen merkezinde, aşağıdaki kod örneğinde ayarlayarak görüntünün düzeni Merkez etrafında döndürme gösterilmektedir, [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) Özellik:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Döndürmek için [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) düzeni Merkez etrafında örneğini [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) ve [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) olan değerleri özellikleri ayarlayın Genişlik ve yüksekliğini göre `Image`. Bu örnekte, merkezi `Image` düzeni merkezinde olarak tanımlanır ve bu nedenle varsayılan `AnchorX` 0,5 değerini değiştirme gerektirmez. Ancak, `AnchorY` üstündeki arasında bir değer olacak şekilde özelliği yeniden `Image` düzeni orta noktası için. Bu sağlar `Image` düzeni orta noktası etrafında 360 derece tam dönüşünü aşağıdaki ekran görüntülerinde gösterildiği gibi yapar:

![](simple-images/rotate-anchors.png "Bağlayıcıları ile döndürme animasyonu")

### <a name="translation"></a>Çeviri

Aşağıdaki kod örneğinde kullanımı gösterilir [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) hale getirmeyi yöntemi [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) ve [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) bir özellikleri[ `Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Bu kod canlandırır [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 1 saniye (1000 milisaniye cinsinden) da yatay ve dikey olarak çevirme tarafından örneği. [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Yöntemi aynı anda görüntü 100 piksel sola ve yukarı 100 piksel çevirir. Her iki negatif sayılar birinci ve ikinci bağımsız değişkenler olmasıdır. Pozitif sayıları sağlama görüntünün sağına ve aşağı çevirir.

Aşağıdaki ekran görüntüleri, her bir platform üzerinde devam eden çeviri göster:

![](simple-images/translateto.png "Çeviri animasyon")

> [!NOTE]
> Öğenin başlangıçta ekranı düzenlendiği ve ekrana çevrilen çeviri sonra öğenin giriş düzenini ekran devre dışı kalır ve kullanıcı ile etkileşime giremezler. Bu nedenle, bir görünüm içindeki son konumunu düzenleneceğini ve sonra gerçekleştirilen çevirileri gerekli önerilir.

### <a name="fading"></a>Soluklaşan

Aşağıdaki kod örneğinde kullanımı gösterilir [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) hale getirmeyi yöntemi [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) özelliği bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Bu kod canlandırır [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) üzerinde 4 saniye içinde (4000 milisaniye) Soluklaşan tarafından örneği. [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Yöntemi alır Geçerli [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) animasyon ve ardından silinmeleri içinde bu değerden ilk bağımsız değişken (1) için başlangıç özellik değeri.

Aşağıdaki ekran görüntüleri, her bir platform üzerinde devam eden yavaşça göster:

![](simple-images/fadeto.png "Animasyon Soluklaşan")

<a name="compound" />

## <a name="compound-animations"></a>Bileşik animasyonları

Bileşik bir animasyon animasyonları sıralı bir bileşimidir ve ile oluşturulan `await` aşağıdaki kod örneğinde gösterildiği şekilde işleci:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

Bu örnekte, [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 6 saniye (6000 milisaniye) çevrilir. Çevrilmesi `Image` ile beş animasyon kullanır `await` her animasyon sıralı olarak yürütür belirten işleci. Bu nedenle, önceki yöntemi tamamlandıktan sonra sonraki animasyon bir yöntem yürütülemez.

<a name="composite" />

## <a name="composite-animations"></a>Bileşik animasyonları

Bileşik animasyon animasyonları iki veya daha fazla animasyonları aynı anda çalıştırdığı birleşimidir. Bileşik animasyonları beklemenin ve beklemenin olmayan animasyonları birleştirilmesinden aşağıdaki kod örneğinde gösterildiği şekilde oluşturulabilir:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

Bu örnekte, [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) ölçeklendirilebilir ve 4 saniye (4000 milisaniye) aynı anda döndürülebilir. Ölçeklendirme `Image` döndürme aynı anda gerçekleşen iki sıralı animasyon kullanır. [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Yöntemini yürütür olmadan bir `await` işleci ve hemen ilk döndürür [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) sonra başlayan animasyon. `await` İlk işlecinin `ScaleTo` yöntem çağrısı gecikmeler ikinci `ScaleTo` yöntem çağrısı ilk kadar `ScaleTo` yöntem çağrısı tamamlandı. Bu noktada `RotateTo` animasyon yarısı şekilde tamamlanmış olduğunu ve `Image` Döndürülmüş 180 derece olacaktır. Son 2 saniye (2000 milisaniye) sırasında ikinci `ScaleTo` animasyon ve `RotateTo` animasyon her ikisi de tamamlayın.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Aynı anda birden çok zaman uyumsuz yöntemleri çalıştırma

`static` `Task.WhenAny` Ve `Task.WhenAll` yöntemleri birden çok zaman uyumsuz yöntemleri aynı anda çalıştırmak için kullanılan ve bu nedenle bileşik animasyon oluşturmak için kullanılabilir. Her iki yöntem dönüş bir `Task` nesne ve yöntem koleksiyonu kabul edin, her dönüş bir `Task` nesnesi. `Task.WhenAny` Yöntemi, herhangi bir yöntemi, koleksiyondaki yürütme tamamlandığında aşağıdaki kod örneğinde gösterildiği şekilde tamamlar:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

Bu örnekte, `Task.WhenAny` yöntem çağrısı iki görev içerir. İlk görev görüntüyü 4 saniye (4000 milisaniye olarak) döndürür ve ikinci görev 2 saniye (2000 milisaniye) görüntüyü ölçeklendirir. İkinci görevi tamamlandığında `Task.WhenAny` yöntem çağrısı tamamlar. Ancak, olsa bile [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) yöntemi çalışmaya, ikinci [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) yöntemi başlayabilir.

`Task.WhenAll` Yöntemi, kendi koleksiyonundaki tüm yöntemleri tamamladıktan sonra aşağıdaki kod örneğinde gösterildiği şekilde tamamlar:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

Bu örnekte, `Task.WhenAll` yöntem çağrısı üzerinde 10 dakika yürütür her biri üç görev içerir. Her `Task` 360 derecelik döndürme – 307 döndürmeleri için farklı sayıda yapar [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 251 döndürmeleri için [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)ve için 199 döndürmeleri [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/). Bu değerler asal sayılar, bu nedenle döndürmenin eşitlenmedi ve bu nedenle yinelenen düzenleri neden olmaz sağlama.

Aşağıdaki ekran görüntüleri her platformda işlemde birden çok döndürmeleri göster:

![](simple-images/multiple-rotations.png "Bileşik animasyon")

## <a name="canceling-animations"></a>Animasyon iptal etme

Bir uygulama bir veya daha fazla animasyonları çağrısıyla iptal edebilirsiniz `static` [ `ViewExtensions.CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
ViewExtensions.CancelAnimations (image);
```

Bu şu anda çalışmakta olan tüm animasyonları hemen iptal eder [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) örneği.

## <a name="summary"></a>Özet

Bu makalede oluşturma ve kullanma animasyonları iptal etme gösterilen [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) sınıfı. Bu sınıf, döndürme, Ölçek, çevir ve silinerek basit animasyonları oluşturmak için kullanılan genişletme yöntemleri sağlar [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) örnekleri.


## <a name="related-links"></a>İlgili bağlantılar

- [Zaman Uyumsuz Desteğe Genel Bakış](~/cross-platform/platform/async.md)
- [Temel animasyon (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
