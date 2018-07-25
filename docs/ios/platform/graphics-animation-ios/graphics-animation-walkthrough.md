---
title: Temel grafikler ve animasyon çekirdek Xamarin.ios'ta kullanma
description: Bu makalede adım adım temel grafikler ve animasyon Core kullanan bir uygulamanın nasıl oluşturulacağını gösterir. Bu yol boyunca her fırsatta seyahat etmeye görüntü animasyon uygulamak nasıl yanı sıra nasıl yanıt olarak kullanıcı dokunmatik ekranda çizileceğini gösterir.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: cecfd7f3a9678f298af3ed547aa7b50a18238729
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242010"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Temel grafikler ve animasyon çekirdek Xamarin.ios'ta kullanma

Bu kılavuz için giriş dokunmaya yanıt olarak temel grafikler kullanarak bir yol çizmek için kullanacağız. Daha sonra ekleyeceğiz bir `CALayer` içeren bir görüntü sizi yol boyunca animasyon uygular.

Aşağıdaki ekran görüntüsünde tamamlanan uygulama gösterilir:

![](graphics-animation-walkthrough-images/00-final-app.png "Tamamlanmış uygulama")

Yükleme başlamadan önce *GraphicsDemo* bu kılavuzu eşlik eden örnek. Bu indirilebilir [burada](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) ve içinde bulunan **GraphicsWalkthrough** dizin adlı projeyi Başlat **GraphicsDemo_starter** üzerine çift tıklayarak ve Açık `DemoView` sınıfı.

## <a name="drawing-a-path"></a>Bir yol çizme


1. İçinde `DemoView` ekleme bir `CGPath` değişken sınıfına ve oluşturucuda örneği. Ayrıca iki bildirmek `CGPoint` değişkenleri `initialPoint` ve `latestPoint`, size, biz oluşturmak yolu touch noktası yakalamak için kullanacağız:
    
    ```csharp
    public class DemoView : UIView
    {
        CGPath path;
        CGPoint initialPoint;
        CGPoint latestPoint;
    
        public DemoView ()
        {
            BackgroundColor = UIColor.White;
    
            path = new CGPath ();
        }
    }
    ```

2. Aşağıdaki yönergeleri kullanarak:

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. Ardından, geçersiz kılma `TouchesBegan` ve `TouchesMoved,` ve ilk dokunmatik noktası ve her bir sonraki touch noktasına sırasıyla yakalamak için aşağıdaki uygulamaları ekleyin:

    ```csharp
    public override void TouchesBegan (NSSet touches, UIEvent evt){
    
        base.TouchesBegan (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }
    
    public override void TouchesMoved (NSSet touches, UIEvent evt){
    
        base.TouchesMoved (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }
    ```

    `SetNeedsDisplay` dokunuşları taşımak için sırayla her zaman çağrılacağı `Draw` sonraki çalıştırma döngü geçişte çağrılabilir.

4. Satırları yoluna ekleyeceğiz `Draw` yöntemi ve kullanımı ile çizmek için kırmızı, kesikli bir çizgi. [Uygulama `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) aşağıda kod ile:

    ```csharp
    public override void Draw (CGRect rect){
    
        base.Draw (rect);
    
        if (!initialPoint.IsEmpty) {
    
            //get graphics context
            using(CGContext g = UIGraphics.GetCurrentContext ()){
                    
                //set up drawing attributes
                g.SetLineWidth (2);
                UIColor.Red.SetStroke ();
    
                //add lines to the touch points
                if (path.IsEmpty) {
                    path.AddLines (new CGPoint[]{initialPoint, latestPoint});
                } else {
                    path.AddLineToPoint (latestPoint);
                }
            
                //use a dashed line
                g.SetLineDash (0, new nfloat[] { 5, 2 * (nfloat)Math.PI });
                                
                //add geometry to graphics context and draw it
                g.AddPath (path);       
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
    ```

Biz uygulamayı şimdi çalıştırırsanız, biz ekranda çizim yapmak için aşağıdaki ekran görüntüsünde gösterildiği gibi dokunma:

![](graphics-animation-walkthrough-images/01-path.png "Ekranda çizim")

## <a name="animating-along-a-path"></a>Yol boyunca animasyon ekleme

Bir yol çizmek kullanıcılara izin verecek kod uyguladık, katman çizilen yol üzerinde animasyonunu oluşturma için kod ekleyelim.

1. İlk olarak, ekleme bir [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) değişken sınıfına ve oluşturucuda oluşturun:

    ```csharp
    public class DemoView : UIView
        {
            …
    
            CALayer layer;
    
            public DemoView (){
                …
    
                //create layer
                layer = new CALayer ();
                layer.Bounds = new CGRect (0, 0, 50, 50);
                layer.Position = new CGPoint (50, 50);
                layer.Contents = UIImage.FromFile ("monkey.png").CGImage;
                layer.ContentsGravity = CALayer.GravityResizeAspect;
                layer.BorderWidth = 1.5f;
                layer.CornerRadius = 5;
                layer.BorderColor = UIColor.Blue.CGColor;
                layer.BackgroundColor = UIColor.Purple.CGColor;
            }
    ```

2. Kullanıcı kendi parmak ekranından'kurmak kaldırıncaya, ardından, katman bir görünümün katmanı alt katman ekleyeceğiz. Ardından yolu kullanarak bir ana kare animasyon katmanın hareketlendirme oluşturacağız `Position`.

    Bunu geçersiz kılmak için ihtiyacımız gerçekleştirmek için `TouchesEnded` ve aşağıdaki kodu ekleyin:

    ```csharp
    public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            base.TouchesEnded (touches, evt);

            //add layer with image and animate along path

            if (layer.SuperLayer == null)
                Layer.AddSublayer (layer);

            // create a keyframe animation for the position using the path
            layer.Position = latestPoint;
            CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
            animPosition.Path = path;
            animPosition.Duration = 3;
            layer.AddAnimation (animPosition, "position");
        }
    ```

3. Artık ve çizim sonra uygulamayı çalıştırmak, görüntü ile bir katman eklenir ve dolaşan çizilen yol:

![](graphics-animation-walkthrough-images/00-final-app.png "Görüntü ile bir katman eklendikten ve dolaşan çizilen yol")

## <a name="summary"></a>Özet

Bu makalede, biz grafikler ve animasyon kavramları birbirine bağlı bir örnek adım adım gösterildi. İlk olarak biz temel grafikler bir yol çizmek için nasıl kullanılacağını gösterdi bir `UIView` kullanıcı dokunmaya yanıt. Ardından çekirdek animasyon yol seyahat bir görüntü oluşturmak için nasıl kullanılacağını gösterdi.


## <a name="related-links"></a>İlgili bağlantılar

- [Temel Animasyon](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Temel Grafikler](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Temel animasyon tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
