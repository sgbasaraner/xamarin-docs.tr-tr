---
title: TextKit
description: "Metin Seti API güçlü metin Xamarin.iOS düzeni ve işleme özelliklerini sunar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 6b065c4b695edc3c88c5aed8c407c53fa6bbc578
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="text-kit"></a>Metin Seti

Metin, güçlü metin düzeni ve işleme özellikleri sunan yeni bir API setidir. Düşük düzey çekirdek metin çerçevesi üzerine inşa ancak çekirdek metin kullanmak çok daha kolaydır.

Metin Seti özelliklerini standart denetimleri kullanılabilir hale getirmek için birkaç iOS metin denetimleri de dahil olmak üzere metin Seti kullanacak şekilde yeniden uygulanan:

-  UITextView
-  UITextField
-  UILabel


## <a name="architecture"></a>Mimari

Metin Seti metin depolama düzeni ve aşağıdaki sınıflar gibi görünen ayıran katmanlı bir mimari sağlar:

-  `NSTextContainer` – Yerleşim metni için kullanılan geometri ve koordinat sistemi sağlar.
-  `NSLayoutManager` – Metni karakterlerin metin kapatarak yerleştirir. 
-  `NSTextStorage` – Metin verilerini tutan gibi toplu metin özellik güncelleştirmelerini işler. Herhangi bir toplu güncelleştirme düzenini yeniden hesaplanması ve metin yeniden gibi değişiklikler, işlenmesini Düzen Yöneticisi verilir.


Bu üç sınıfları metin işleyen bir görünüme uygulanır. Yerleşik metin görünümler gibi işleme `UITextView`, `UITextField`, ve `UILabel` bunları kümesi zaten var, ancak oluşturabilir ve bunları herhangi biri geçerli `UIView` de örneği.

Aşağıdaki şekilde bu mimari gösterilmektedir:

 ![](textkit-images/textkitarch.png "Bu şekilde metin Seti mimarisi gösterilmektedir")

## <a name="text-storage-and-attributes"></a>Metin depolama ve öznitelikleri

`NSTextStorage` Sınıf, bir görünüm tarafından görüntülenen metni içerir. Herhangi bir değişiklik - karakter değişiklikleri gibi metin veya öznitelikleriyle - görüntü düzen Yöneticisi için iletişim kurar. `NSTextStorage` öğesinden devralınan `MSMutableAttributed` arasında yığınlardaki belirtilmesini metin özniteliklere yapılan değişiklikleri izin dizesi `BeginEditing` ve `EndEditing` çağrıları.

Örneğin, aşağıdaki kod parçacığını bir değişiklik ön belirtir ve arka plan renklerini, sırasıyla ve belirli aralıkları hedefler:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

Sonra `EndEditing` olduğu olarak adlandırılan, tüm gerekli düzeni ve işleme hesaplamalar Görünümde görüntülenecek metni için sırayla gerçekleştirir Düzen Yöneticisi değişiklikleri gönderilir.

## <a name="layout-with-exclusion-path"></a>Dışarıda tutma yolu düzeniyle

Metin Seti ayrıca düzenini destekler ve birden çok sütun metin ve belirtilen yollar etrafında akan metin olarak adlandırılan gibi karmaşık senaryolar için verir *dışlama yolları*. Dışlama yolları metnin belirtilen yollar akmasını neden metin düzenini geometri değiştirir metin kapsayıcı uygulanır.

Dışarıda tutma yolu ekleme ayarlanması gerekir `ExclusionPaths` Düzen Yöneticisi özelliği. Bu özelliği ayarlamak metin düzeni geçersiz kılmak ve dışarıda tutma yolu çevresine metin akış düzeni Yöneticisi neden olur.

### <a name="exclusion-based-on-a-cgpath"></a>Üzerinde bir CGPath dayalı dışlama

Aşağıdakileri göz önünde bulundurun `UITextView` bir alt uygulama:

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

Bu kod çekirdek grafikleri kullanarak metin görünümde çizim için destek ekler. Bu yana `UITextView` sınıfı, metin işleme ve düzeni için metin Seti kullanmak için yerleşik şimdi, metin dışlama yollarını ayarlama gibi Seti tüm özelliklerini kullanabilirsiniz.

> [!IMPORTANT]
>   Not: Bu örnek alt sınıfların `UITextView` destek çizim dokunma eklemek için. Sınıflara `UITextView` metin Seti özelliklerini almak gerekli değildir.



Metin görünümü, çizilmiş kullanıcı çizer sonra `CGPath` uygulanan bir `UIBezierPath` ayarlayarak örneği `UIBezierPath.CGPath` özelliği:

```csharp
bezierPath.CGPath = exclusionPath;
```

Aşağıdaki kod satırını güncelleştirme yol etrafında güncelleştirme metin düzenini yapar:

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

Aşağıdaki ekran görüntüsünde, metin düzeni nasıl çizilmiş yol etrafında akış değişikliklerini gösterilmektedir:

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "Bu ekran çizilmiş yol etrafında akış için metin düzeni nasıl değiştiğini gösterir")

Dikkat düzeni manager'ın `AllowsNonContiguousLayout` özelliği false olarak ayarlanmış, bu durumda. Bu, tüm durumları burada metni değiştirir yeniden hesaplanması gerektiği Düzen neden olur. Bu ayarı true olarak performans özellikle büyük belgeleri olması durumunda bir tam yerleşimi yenileme kaçınarak yararlı. Ancak, ayarı `AllowsNonContiguousLayout` için true engelleyebilir dışarıda tutma yolu düzeni bazı durumlarda - Örneğin, güncelleştirme sonunda bir satır başı olmadan ayarlanan yol önce metin çalışma zamanında girdiyseniz.


## <a name="related-links"></a>İlgili bağlantılar

- [İOS 7 (örnek) giriş](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 kullanıcı arabirimine genel bakış](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
