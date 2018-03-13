---
title: "Çok dokunma parmak izleme"
description: "Bu belge, birden çok parmakları dokunma olayları izlemek gösterilmiştir"
ms.topic: article
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: a9e3842611aab86d23a2b0c2a832efce18c22465
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="multi-touch-finger-tracking"></a>Çok dokunma parmak izleme

_Bu belge, birden çok parmakları dokunma olayları izlemek gösterilmiştir_

Aynı anda ekranda taşırken tek tek parmakları izlemek için çok touch uygulaması gerektiği zaman zamanlar vardır. Bir genel uygulama finger-paint programdır. İle tek bir parmak çizmek için aynı zamanda birden fazla parmakları ile aynı anda çizmek için kullanabilmek için kullanıcının istiyor. Birden çok dokunma olayları programınızı işler gibi bu parmakları arasında ayrım yapmak gerekir.

Bir parmak ilk ekran dokunduğunda iOS oluşturur bir [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/) bu parmak nesnesi. Bu nesne parmak ekranda taşır ve ardından bu noktada nesne atıldı ekranından kaldırır aynı kalır. Parmakları izlemek için bir program bu depolama kaçınmalısınız `UITouch` doğrudan nesne. Bunun yerine, kullanabilirsiniz [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) türündeki özelliği `IntPtr` bunlar benzersiz şekilde tanımlamak için `UITouch` nesneleri.

Neredeyse her zaman, tek tek parmakları izleyen bir program dokunma izleme için bir sözlük tutar. Bir iOS için Sözlük anahtarı programdır `Handle` belirli bir parmak tanımlayan bir değer. Sözlük değeri uygulamaya bağlıdır. İçinde [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) program, her parmak vuruşun (Başlangıç yayımlamayı touch) ile bu parmak çizgi işlemek için gerekli tüm bilgileri içeren bir nesne ile ilişkili. Küçük bir programı tanımlayan `FingerPaintPolyline` sınıfı bu amaç için:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new CGPath();
    }

    public CGColor Color { set; get; }

    public float StrokeWidth { set; get; }

    public CGPath Path { private set; get; }
}
```

Bir renk, vuruşun genişliğini ve bir iOS grafik her çoklu çizgi sahip [ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/) biriktirmesi ve çizilen gibi birden çok noktası satırının işlemek için nesne.


Tüm kodun kalan kısmı aşağıda bulunan bir `UIView` adlı türevi `FingerPaintCanvasView`. Sınıf bir sözlüğü nesne türü bulundurur `FingerPaintPolyline` bunlar etkin olarak bir veya daha fazla parmakları tarafından çizilen süre boyunca:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Bu sözlük hızlı bir şekilde almak görünümü sağlar `FingerPaintPolyline` her parmak ile ilişkili bilgileri temel alarak `Handle` özelliği `UITouch` nesnesi.

`FingerPaintCanvasView` Sınıfı ayrıca korur bir `List` nesne tamamlandı kullansa için:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Bu nesneleri `List` bunlar çizilmiş sırayla.

`FingerPaintCanvasView` tarafından tanımlanan beş yöntemlerini geçersiz kılar `View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

Çeşitli `Touches` geçersiz kılmaları birikmesini kullansa noktalar.

[`Draw`] Geçersiz kılma çizer tamamlanmış kullansa ve devam eden kullansa:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (CGContext context = UIGraphics.GetCurrentContext())
    {
        // Stroke settings
        context.SetLineCap(CGLineCap.Round);
        context.SetLineJoin(CGLineJoin.Round);

        // Draw the completed polylines
        foreach (FingerPaintPolyline polyline in completedPolylines)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }

        // Draw the in-progress polylines
        foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Her biri `Touches` geçersiz kılmaları olası bir veya daha fazla tarafından belirtilen birden çok parmakları eylemlerini raporlar `UITouch` içinde depolanan nesneleri `touches` yöntemi için bağımsız değişken. `TouchesBegan` Bu nesneler geçersiz kılmaları döngü. Her `UITouch` nesne, yöntem oluşturur ve yeni bir başlatır `FingerPaintPolyline` alınan parmak ilk konumunu depolama dahil olmak üzere nesnenin `LocationInView` yöntemi. Bu `FingerPaintPolyline` nesne eklenmiş olup `InProgressPolylines` sözlüğünü kullanarak `Handle` özelliği `UITouch` nesnesi bir sözlük anahtarı olarak:

```csharp
public override void TouchesBegan(NSSet touches, UIEvent evt)
{
    base.TouchesBegan(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Create a FingerPaintPolyline, set the initial point, and store it
        FingerPaintPolyline polyline = new FingerPaintPolyline
        {
            Color = StrokeColor,
            StrokeWidth = StrokeWidth,
        };

        polyline.Path.MoveToPoint(touch.LocationInView(this));
        inProgressPolylines.Add(touch.Handle, polyline);
    }
    SetNeedsDisplay();
}
```

Yöntemini çağırarak sonucuna `SetNeedsDisplay` yapılan bir çağrı oluşturmak için `Draw` geçersiz kılmak ve ekranı güncelleştirmek için.

Parmak veya parmakları ekran üzerinde hareket ederken `View` birden fazla çağrı alır, `TouchesMoved` geçersiz kılar. Bu geçersiz kılma yoluyla benzer şekilde döngü `UITouch` içinde depolanan nesneleri `touches` bağımsız değişkeni ve grafik yolu parmak geçerli konumunu ekler:

```csharp
public override void TouchesMoved(NSSet touches, UIEvent evt)
{
    base.TouchesMoved(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Add point to path
        inProgressPolylines[touch.Handle].Path.AddLineToPoint(touch.LocationInView(this));
    }
    SetNeedsDisplay();
}
```

`touches` Koleksiyonu içeren yalnızca `UITouch` son çağrısından sonra taşınmış parmakları nesneleri `TouchesBegan` veya `TouchesMoved`. Şimdiye kadar gerekiyorsa `UITouch` karşılık gelen nesneleri *tüm* parmakları ekran iletişim kurmayan şu anda bu bilgi edinilebilir `AllTouches` özelliği `UIEvent` yöntemi için bağımsız değişken.

`TouchesEnded` Geçersiz kılma sahip iki işler. Son noktası grafik yolunu ve aktarım eklemelisiniz `FingerPaintPolyline` nesnesinin `inProgressPolylines` sözlüğe `completedPolylines` listesi:

```csharp
public override void TouchesEnded(NSSet touches, UIEvent evt)
{
    base.TouchesEnded(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Get polyline from dictionary and remove it from dictionary
        FingerPaintPolyline polyline = inProgressPolylines[touch.Handle];
        inProgressPolylines.Remove(touch.Handle);

        // Add final point to path and save with completed polylines
        polyline.Path.AddLineToPoint(touch.LocationInView(this));
        completedPolylines.Add(polyline);
    }
    SetNeedsDisplay();
}
```

`TouchesCancelled` Geçersiz kılma, yalnızca terk tarafından işlenir `FingerPaintPolyline` sözlük nesnesi:

```csharp
public override void TouchesCancelled(NSSet touches, UIEvent evt)
{
    base.TouchesCancelled(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        inProgressPolylines.Remove(touch.Handle);
    }
    SetNeedsDisplay();
}
```

Bu işleme tamamen verir [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) tek tek parmakları izlemek ve sonuçlar ekranda çizmek için program:

[![](touch-tracking-images/image01.png "Tek tek parmakları izleme ve sonuçlar ekranda çizme")](touch-tracking-images/image01.png#lightbox)

Şimdi nasıl ekranda tek tek parmakları izlemek ve bunlar arasında ayrım gördünüz.



## <a name="related-links"></a>İlgili bağlantılar

- [Eşdeğer Xamarin Android Kılavuzu](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (örnek)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
