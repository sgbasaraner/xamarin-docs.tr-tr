---
title: Xamarin.iOS TextKit
description: Bu belge, içinde Xamarin.iOS TextKit kullanmayı açıklar. TextKit güçlü metin düzeni ve işleme özelliklerini sağlar.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ac80d1d07f5649d377dd6fdefcb4911ba9ec2dcb
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788341"
---
# <a name="textkit-in-xamarinios"></a>Xamarin.iOS TextKit

TextKit güçlü metin düzeni ve işleme özellikleri sunan yeni bir API'dir. Alt düzey çekirdek metin çerçevesi üzerine inşa ancak çekirdek metin kullanmak çok daha kolaydır.

TextKit özelliklerini standart denetimleri kullanılabilir hale getirmek için birkaç iOS metin denetimleri TextKit, kullanacak şekilde yeniden uygulanan silinmiş dahil olmak üzere:

-  UITextView
-  UITextField
-  UILabel

## <a name="architecture"></a>Mimari

TextKit metin depolama düzeni ve aşağıdaki sınıflar gibi görünen ayıran katmanlı bir mimari sağlar:

-  `NSTextContainer` – Yerleşim metni için kullanılan geometri ve koordinat sistemi sağlar.
-  `NSLayoutManager` – Metni karakterlerin metin kapatarak yerleştirir. 
-  `NSTextStorage` – Metin verilerini tutan gibi toplu metin özellik güncelleştirmelerini işler. Herhangi bir toplu güncelleştirme düzenini yeniden hesaplanması ve metin yeniden gibi değişiklikler, işlenmesini Düzen Yöneticisi verilir.


Bu üç sınıfları metin işleyen bir görünüme uygulanır. Yerleşik metin görünümler gibi işleme `UITextView`, `UITextField`, ve `UILabel` bunları kümesi zaten var, ancak oluşturabilir ve bunları herhangi biri geçerli `UIView` de örneği.

Aşağıdaki şekilde bu mimari gösterilmektedir:

 ![](textkit-images/textkitarch.png "Bu şekilde TextKit mimarisi gösterilmektedir")

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

TextKit de düzeni destekler ve birden çok sütun metin ve belirtilen yollar etrafında akan metin olarak adlandırılan gibi karmaşık senaryolar için verir *dışlama yolları*. Dışlama yolları metnin belirtilen yollar akmasını neden metin düzenini geometri değiştirir metin kapsayıcı uygulanır.

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

Bu kod çekirdek grafikleri kullanarak metin görünümde çizim için destek ekler. Bu yana `UITextView` sınıfı, metin işleme ve düzeni için TextKit kullanmak için yerleşik şimdi, dışlama yollarını ayarlama gibi TextKit tüm özelliklerini kullanabilirsiniz.

> [!IMPORTANT]
> Bu örnek alt sınıfların `UITextView` destek çizim dokunma eklemek için. Sınıflara `UITextView` TextKit özelliklerini almak gerekli değildir.



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
- [iOS 7 Kullanıcı Arabirimine Genel Bakış](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Arka Planda İşleme](~/ios/app-fundamentals/backgrounding/index.md)
