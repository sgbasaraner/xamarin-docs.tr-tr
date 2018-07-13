---
title: Xamarin.Forms içinde basit animasyonlar
description: ViewExtensions sınıf basit animasyon oluşturmak için kullanılan genişletme yöntemleri sağlar. Bu makalede oluşturma ve animasyonları ViewExtensions sınıfı kullanarak iptal etme işlemlerini gösterir.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: 124fc311d5e2c8c89353ba813df60f0bf1d0b34a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997077"
---
# <a name="simple-animations-in-xamarinforms"></a>Xamarin.Forms içinde basit animasyonlar

_ViewExtensions sınıf basit animasyon oluşturmak için kullanılan genişletme yöntemleri sağlar. Bu makalede oluşturma ve animasyonları ViewExtensions sınıfı kullanarak iptal etme işlemlerini gösterir._


[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Sınıfı aşağıdaki basit animasyon oluşturmak için kullanılan genişletme yöntemleri sağlar:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) canlandırır [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) ve [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) özelliklerini bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`ScaleTo`](xref:Xamarin.Forms.VisualElement.Scale) canlandırır [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) özelliği bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) bir animasyonlu artımlı artırmak veya azaltmak için geçerli [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) özelliği bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) canlandırır [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) özelliği bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) bir animasyonlu artımlı artırmak veya azaltmak için geçerli [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) özelliği bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) canlandırır [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) özelliği bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) canlandırır [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) özelliği bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) canlandırır [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) özelliği bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).

Varsayılan olarak, her animasyonu 250 milisaniye götürür. Ancak, her animasyon için süre animasyon oluşturulurken belirtilebilir.

[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Sınıfı da içeren bir [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) herhangi bir animasyonun iptal etmek için kullanılan yöntem.

> [!NOTE]
> [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Sağlar sınıfını bir [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) genişletme yöntemi. Ancak bu yöntem tarafından düzenleri boyutunu içeren ve değişiklikleri konumlandırma Düzen durumlar arasındaki geçişleri animasyon uygulamak için kullanılması amaçlanmıştır. Bu nedenle, yalnızca tarafından kullanılması gereken [ `Layout` ](xref:Xamarin.Forms.Layout) alt sınıflar.

Animasyon uzantı yöntemleri [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) sınıfı, tüm zaman uyumsuz ve dönüş bir `Task<bool>` nesne. Dönüş değeri `false` animasyon tamamlanırsa ve `true` animasyon iptal edildiği takdirde. Bu nedenle, animasyon yöntemleri genellikle kullanılmalıdır `await` kolayca animasyon tamamlandığında belirlemek mümkün kılan işleci. Ayrıca, ardından önceki yöntemi tamamlandıktan sonra yürütme sonraki animasyon yöntemleriyle sıralı animasyonları oluşturmak mümkün olur. Daha fazla bilgi için [bileşik animasyonları](#compound).

Bir animasyon izin vermek için bir gereksinimi varsa arka planda tam sonra `await` işleci atlanabilir. Bu senaryoda, animasyon genişletme yöntemleri arka planda gerçekleşen animasyonu ile animasyon başlatıldıktan sonra hızlı bir şekilde döndürür. Bu işlem, bileşik animasyonları oluştururken avantajlarından alınabilir. Daha fazla bilgi için [bileşik animasyonları](#composite).

Hakkında daha fazla bilgi için `await` işleci bkz [zaman uyumsuz desteğe genel bakış](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Tek animasyonları

Her bir genişletme yöntemi [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) bir süre aşamalı bir özelliği bir değerden başka bir değerle değiştiren bir tek animasyon işlemi uygular. Bu bölümde, her bir animasyon işlem keşfediyor.

### <a name="rotation"></a>Döndürme

Aşağıdaki kod örneği kullanmayı gösterir [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animasyon uygulamak için gereken yöntemini [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) özelliği bir [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Bu kod canlandırır [ `Image` ](xref:Xamarin.Forms.Image) 2 saniye (2000 milisaniye cinsinden) en fazla 360 derece döndürme tarafından örneği. [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Yöntemi alır Geçerli [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) özellik değeri animasyonun başlangıcında ve ardından ilk bağımsız değişken (360) için bu değeri döndürür. Animasyonun bir kez tamamlandı, görüntünün [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) özelliği 0 olarak sıfırlanır. Bu, sağlar `Rotation` özelliği ek dönüşümüne önleyen, animasyon, sonuçlandıktan sonra 360 kalması değil.

Aşağıdaki ekran görüntüleri döndürme, devam eden her platformda göster:

![](simple-images/rotateto.png "Döndürme animasyon")

### <a name="relative-rotation"></a>Göreli döndürme

Aşağıdaki kod örneği kullanmayı gösterir [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) artımlı olarak artırmak veya azaltmak için yöntemi [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) özelliği bir [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelRotateTo (360, 2000);
```

Bu kod canlandırır [ `Image` ](xref:Xamarin.Forms.Image) 2 saniye (2000 milisaniye cinsinden) başlangıç konumuna kaynağından 360 derece döndürme tarafından örneği. [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Yöntemi alır Geçerli [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) özellik değeri animasyonun başlangıcında ve ardından değeri artı (360) ilk bağımsız değişkeni için bu değeri döndürür. Bu, her animasyon başlangıç konumundan 360 derece döndürme işlemini her zaman olmasını sağlar. Bu nedenle, bir animasyon devam ederken yeni bir animasyon çağrılırsa, geçerli konumdan başlar ve artışı içeren 360 derece olmayan bir konumdan sonlandırabiliriz.

Aşağıdaki ekran görüntüleri, devam eden her platformda göreli döndürme göster:

![](simple-images/relrotateto.png "Göreli döndürme animasyon")

### <a name="scaling"></a>Ölçeklendirme

Aşağıdaki kod örneği kullanmayı gösterir [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) animasyon uygulamak için gereken yöntemini [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) özelliği bir [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.ScaleTo (2, 2000);
```

Bu kod canlandırır [ `Image` ](xref:Xamarin.Forms.Image) 2 saniye (2000 milisaniye cinsinden) iki kez boyutuna ölçekleme yaparak örneği. [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) Yöntemi alır Geçerli [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) animasyon ve ilk bağımsız değişken (2) için bu değerden sonra ölçek başlangıcı için özellik değeri (varsayılan değer olan 1). Bu resmin boyutu iki kez boyutunu genişletme etkisi vardır.

Aşağıdaki ekran görüntüleri, devam eden her platformda ölçeklendirme göster:

![](simple-images/scaleto.png "Animasyon ölçeklendirme")

### <a name="relative-scaling"></a>Göreli ölçeklendirme

Aşağıdaki kod örneği kullanmayı gösterir [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animasyon uygulamak için gereken yöntemini [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) özelliği bir [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelScaleTo (2, 2000);
```

Bu kod canlandırır [ `Image` ](xref:Xamarin.Forms.Image) 2 saniye (2000 milisaniye cinsinden) iki kez boyutuna ölçekleme yaparak örneği. [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Yöntemi alır Geçerli [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) animasyon ve ek olarak, (2) ilk bağımsız değişkeninin değeri bu değerden sonra ölçek başlangıcı için özellik değeri. Bu, her animasyonu 2 konumdan başlayan bir ölçeklendirme her zaman olmasını sağlar.

### <a name="scaling-and-rotation-with-anchors"></a>Ölçeklendirme ve döndürme yer işaretleri ile

[ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) Ve [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) özelliklerini ayarlama ölçekleme veya döndürme için merkezi [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) ve [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) özellikleri. Bu nedenle, bunların değerleri de etkiler [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) ve [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) yöntemleri.

Verilen bir [ `Image` ](xref:Xamarin.Forms.Image) , yerleştirilen bir düzen merkezde, aşağıdaki kod örneği ayarlayarak düzenini merkezi etrafındaki görüntü döndürme gösterir, [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) Özellik:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Döndürmek için [ `Image` ](xref:Xamarin.Forms.Image) Düzen merkezi etrafındaki örneğini [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) ve [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) olan değerleri için özellikleri ayarlama göreli genişliğini ve yüksekliğini `Image`. Bu örnekte, merkezi `Image` Düzen merkezinde olarak tanımlanır ve bu nedenle varsayılan `AnchorX` 0,5 değeri değiştirmek gerektirmez. Ancak, `AnchorY` özelliği üstünden bir değer olacak şekilde yeniden `Image` merkez noktasını düzeni için. Bu, sağlar `Image` aşağıdaki ekran görüntülerinde gösterildiği gibi tam bir düzeni merkez noktası çevresinde 360 derece döndürme yapar:

![](simple-images/rotate-anchors.png "Yer işaretleri ile döndürme animasyonu")

### <a name="translation"></a>Çeviri

Aşağıdaki kod örneği kullanmayı gösterir [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) animasyon uygulamak için gereken yöntemini [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) ve [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) bir özellikleri[ `Image`](xref:Xamarin.Forms.Image):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Bu kod canlandırır [ `Image` ](xref:Xamarin.Forms.Image) 1 saniye (1000 milisaniye cinsinden), yatay ve dikey olarak çevirme tarafından örneği. [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Yöntemi aynı anda sol görüntü 100 piksel ve 100 piksel yukarı çevirir. Her iki negatif sayılar birinci ve ikinci bağımsız değişkenler olmasıdır. Pozitif sayıları sağlama resmi sağa ve aşağı çevirir.

Aşağıdaki ekran görüntüleri, devam eden her platformda çeviri göster:

![](simple-images/translateto.png "Çeviri animasyon")

> [!NOTE]
> Bir öğe ekranın dışına başlangıçta düzenlendiği ve sonra ekrana çevrilmiş çeviri sonra öğenin giriş düzeni ekran devre dışı kalır ve kullanıcı ile etkileşime giremezler. Bu nedenle, bir görünüm içindeki son konumu düzenleneceğini ve sonra gerçekleştirilen çevirileri gerekli önerilir.

### <a name="fading"></a>Soluklaşan

Aşağıdaki kod örneği kullanmayı gösterir [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animasyon uygulamak için gereken yöntemini [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) özelliği bir [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Bu kod canlandırır [ `Image` ](xref:Xamarin.Forms.Image) 4 saniyenin üzerindeki içinde (4000 milisaniye cinsinden) Soluklaşan tarafından örneği. [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Yöntemi alır Geçerli [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) animasyon ve ardından yavaşça içinde bu değerden ilk bağımsız değişken (1) için başlangıç özellik değeri.

Aşağıdaki ekran görüntüleri, devam eden her platformda fade göster:

![](simple-images/fadeto.png "Soluklaşan animasyon")

<a name="compound" />

## <a name="compound-animations"></a>Bileşik animasyonları

Bileşik bir animasyon animasyonları sıralı bir bileşimidir ve ile oluşturulan `await` işleci, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

Bu örnekte, [ `Image` ](xref:Xamarin.Forms.Image) 6 saniye (6000 milisaniye cinsinden) çevrilir. Çevirisi `Image` ile beş animasyonları kullanan `await` işleci belirten her animasyonu sıralı olarak yürütür. Bu nedenle, önceki yöntem tamamlandıktan sonra sonraki animasyon bir yöntem yürütülemez.

<a name="composite" />

## <a name="composite-animations"></a>Bileşik animasyonları

Bileşik bir animasyon animasyon iki veya daha fazla animasyonları aynı anda çalıştırdığı oluşur. Bileşik animasyonları beklenen ve bekleniyor animasyonları birleştirilmesinden aşağıdaki kod örneğinde gösterildiği gibi oluşturulabilir:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

Bu örnekte, [ `Image` ](xref:Xamarin.Forms.Image) ölçeği ve 4 saniye (4000 milisaniye cinsinden) aynı anda döndürülebilir. Ölçeklendirme `Image` döndürme aynı anda gerçekleşen iki ardışık animasyon kullanır. [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Yöntemini yürütür olmadan bir `await` işleci ve ilk hemen döndürür [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) animasyon sonra başlayan. `await` İşleci ilk `ScaleTo` yöntem çağrısının, saniye geciktirir `ScaleTo` yöntem çağrısının ilk kadar `ScaleTo` yöntem çağrısı tamamlandı. Bu noktada `RotateTo` animasyon yarısı şekilde tamamlanmış olduğu ve `Image` Döndürülmüş 180 derece olacaktır. Son 2 saniye (2000 milisaniye) sırasında ikinci `ScaleTo` animasyon ve `RotateTo` animasyon her ikisi de tamamlayın.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Birden çok zaman uyumsuz yöntem eşzamanlı olarak çalışan

`static` `Task.WhenAny` Ve `Task.WhenAll` yöntemleri birden çok zaman uyumsuz yöntem eşzamanlı olarak çalıştırmak için kullanılır ve bu nedenle bileşik animasyon oluşturmak için kullanılabilir. Her iki yöntem dönüş bir `Task` nesne ve yöntemlerin koleksiyonunu kabul edin, her iade bir `Task` nesne. `Task.WhenAny` Yöntemi, yürütme, koleksiyondaki herhangi bir yöntemi tamamlandığında aşağıdaki kod örneğinde gösterildiği gibi tamamlar:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

Bu örnekte, `Task.WhenAny` yöntemi çağrısı, iki görevi içerir. İlk görev görüntüyü 4 saniyenin üzerindeki (4000 milisaniye cinsinden) döndürür ve ikinci görev 2 saniye (2000 milisaniye cinsinden) görüntüyü ölçeklendirir. İkinci görev tamamlandığında `Task.WhenAny` yöntem çağrısını tamamlar. Ancak, olsa bile [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) hala yöntemi çalışıyor, ikinci [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) yöntemi başlayabilirsiniz.

`Task.WhenAll` Yöntemi, kendi koleksiyondaki tüm yöntemleri tamamladıktan sonra aşağıdaki kod örneğinde gösterildiği gibi tamamlar:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

Bu örnekte, `Task.WhenAll` yöntemi çağrısı, 10 dakika içinde yürütür her biri üç görev içerir. Her `Task` 360 derece döndürme – 307 döndürme için farklı sayıda yapar [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), 251 döndürme için [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))ve 199 döndürme için [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)). Bu değerler asal sayıları, bu nedenle döndürmenin eşitlenmedi ve bu nedenle yinelenen desenlerini neden olmaz sağlama.

Aşağıdaki ekran görüntüleri her platformda işlemde birden çok döndürmenin göster:

![](simple-images/multiple-rotations.png "Bileşik animasyon")

## <a name="canceling-animations"></a>Animasyonları iptal ediliyor

Bir uygulama bir veya daha fazla animasyonları çağrısıyla iptal edebilirsiniz `static` [ `ViewExtensions.CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
ViewExtensions.CancelAnimations (image);
```

Bu şu anda çalışmakta olan tüm animasyonları hemen iptal edecek [ `Image` ](xref:Xamarin.Forms.Image) örneği.

## <a name="summary"></a>Özet

Bu makalede oluşturmak ve animasyonları kullanarak iptal gösterilen [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) sınıfı. Bu sınıf, döndürme, ölçeklendirme, çevirme ve Soldurma basit animasyon oluşturmak için kullanılan genişletme yöntemleri sağlar [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) örnekleri.


## <a name="related-links"></a>İlgili bağlantılar

- [Zaman Uyumsuz Desteğe Genel Bakış](~/cross-platform/platform/async.md)
- [Temel animasyon (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
