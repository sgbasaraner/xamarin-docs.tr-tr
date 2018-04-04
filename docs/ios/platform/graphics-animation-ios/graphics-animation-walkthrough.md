---
title: İzlenecek yol - CoreGraphics ve CoreAnimation kullanma
description: Bu makalede nasıl çekirdek grafikleri ve çekirdek animasyon kullanan bir uygulama oluşturmak adım adım gösterilmektedir. Görüntüyü bir yol boyunca seyahat animasyon nasıl yanı sıra nasıl yanıt olarak kullanıcı dokunma ekranında çizileceğini gösterir.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f857accfcdec4cb60e781936d1d0836dbf8d6ffb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="drawing-and-animating-along-a-path"></a>Çizim ve bir yol boyunca animasyon ekleme

Bu kılavuz için size çekirdek grafikleri yanıtta giriş touch kullanmayı yol çizme alınacaktır. Ardından, ekleyeceğiz bir `CALayer` yol boyunca animasyon bir görüntü içeren.

Aşağıdaki ekran görüntüsünde tamamlanan uygulama gösterilir:

![](graphics-animation-walkthrough-images/00-final-app.png "Tamamlanan uygulama")

Yükleme başlamadan önce *GraphicsDemo* bu kılavuzu eşlik eden örnek. Karşıdan yüklenebilir [burada](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) ve içinde bulunan **GraphicsWalkthrough** directory başlatma adlı proje **GraphicsDemo_starter** , üzerine çift tıklatarak ve Açık `DemoView` sınıfı.

## <a name="drawing-a-path"></a>Bir yol çizme


1. İçinde `DemoView` eklemek bir `CGPath` değişken sınıfına ve oluşturucuda örneği. Ayrıca iki bildirme `CGPoint` değişkenleri `initialPoint` ve `latestPoint`, biz içinden biz oluşturmak yolu dokunma noktası yakalamak için kullanacağınız:
    
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

3. Ardından, geçersiz kılma `TouchesBegan` ve `TouchesMoved,` ve ilk dokunma noktası ve her sonraki dokunma noktası sırasıyla yakalamak için aşağıdaki uygulamaları ekleyin:

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

    `SetNeedsDisplay` rötuşları hareket etmesi için her zaman çağrılacağı `Draw` sonraki çalıştırma döngü geçişte çağrılabilir.

4. Satırları yoluna ekleyeceğiz `Draw` yöntemi ve kullanımı ile çizmek için kırmızı kesikli bir çizgi. [Uygulama `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) aşağıda kod ile:

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

Biz uygulamayı şimdi çalıştırırsanız, biz ekranda çizmek için aşağıdaki ekran görüntüsünde gösterildiği gibi touch:

![](graphics-animation-walkthrough-images/01-path.png "Ekranda çizme")

## <a name="animating-along-a-path"></a>Bir yol boyunca animasyon ekleme

Yol çizmek kullanıcıların izin vermek için kod uygulamış olan, bir katman çizilmiş yol boyunca animasyon için kod ekleyelim.

1. İlk olarak, ekleyin bir [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) değişken sınıfına ve oluşturucuda oluşturun:

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

2. Kullanıcı kendi parmak ekranından yukarı kaldırır, ardından, katman bir görünümün katman alt katman ekleyeceğiz. Ardından, yolu kullanarak ana kare animasyonunun katmanın animasyon oluşturacağız `Position`.

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

3. Uygulamayı şimdi ve çizim sonra çalıştırmak, bir katman bir görüntü ile eklenir ve geçen çizilmiş yol boyunca:

![](graphics-animation-walkthrough-images/00-final-app.png "Bir görüntü ile bir katman eklenir ve geçen çizilmiş yol boyunca")

## <a name="summary"></a>Özet

Bu makalede, biz grafikler ve animasyon kavramları birbirine bağlı bir örnek adım adım gösterildi. İlk olarak, size çekirdek grafikleri bir yol çizmek için nasıl kullanılacağı gösterilmiştir bir `UIView` yanıt kullanıcı dokunma olarak. Daha sonra çekirdek animasyon bu yol boyunca seyahat bir görüntü oluşturmak için nasıl kullanılacağı gösterilmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [Temel Animasyon](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Temel Grafikler](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Animasyon tarif çekirdek](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
