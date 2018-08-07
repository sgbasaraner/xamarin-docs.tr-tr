---
title: İOS Designer ile özel denetimleri kullanma
description: Bu belge, özel bir denetim oluşturmak ve iOS için Xamarin tasarımcı ile kullanmak açıklar. Bu denetimin iOS Tasarımcısı araç çubuğunda kullanılabilir hale getirmek, böylece doğru şekilde işlediğinden denetimi uygulamak ve tasarım zamanı ve daha fazla bilgi nasıl gösterir.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 0097cdf006944a51d938ea91d3ea0b0c2aee08cf
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573587"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>İOS Designer ile özel denetimleri kullanma

## <a name="requirements"></a>Gereksinimler

İOS için Xamarin Tasarımcısı, Windows üzerinde Visual Studio Mac, Visual Studio 2015 ve 2017 için de kullanılabilir.

Bu kılavuzda ele içeriği bilindiğini varsayar [Başlarken kılavuzları](~/ios/get-started/index.md).

## <a name="walkthrough"></a>İzlenecek yol

> [!IMPORTANT]
> İçinde Xamarin.Studio 5.5 başlayarak, özel denetimler oluşturulduğu şekilde önceki sürümleri için biraz farklı olacaktır. Özel bir denetim ya da oluşturmak için `IComponent` arabirimi (ile ilişkili uygulama yöntemlerinin) gerekli olduğunu veya bir sınıf olabilir ek açıklama ile `[DesignTimeVisible(true)]`. İkinci yöntem aşağıdaki izlenecek yol örnekte kullanılır.


1. Yeni bir çözüm oluşturma **iOS > Uygulama > tek görünüm uygulaması > C#** şablon adlandırın `ScratchTicket`ve yeni proje sihirbaza devam edin:

    [![](ios-designable-controls-walkthrough-images/01new.png "Yeni çözüm oluşturma")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Adlı yeni bir boş sınıf dosyası oluşturma `ScratchTicketView`:

    [![](ios-designable-controls-walkthrough-images/02new.png "Yeni bir ScratchTicketView sınıf oluşturma")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. İçin aşağıdaki kodu ekleyin `ScratchTicketView` sınıfı:

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;
    
            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }
    
            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }
    
            public ScratchTicketView()
            {
                Initialize();
            }
    
            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }
    
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }
    
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }
    
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }
    
            public override void Draw(CGRect rect)
            {
                base.Draw(rect);
    
                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);
    
                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();
    
                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }
    
                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);        
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```


1. Ekleme `FillTexture.png`, `FillTexture2.png` ve `Monkey.png` dosyaları (kullanılabilir [github'dan](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) için **kaynakları** klasör.
    
1. Çift `Main.storyboard` tasarımcıda açmak için dosya:

    [![](ios-designable-controls-walkthrough-images/03new.png "İOS Tasarımcısı")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. Sürükle ve bırak bir **resim görünümü** gelen **araç kutusu** üzerine görsel taslak görünüm.

    [![](ios-designable-controls-walkthrough-images/04new.png "Düzenine eklenen bir resim görünümü")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. Seçin **resim görünümü** değiştirip kendi **görüntü** özelliğini `Monkey.png`.

    [![](ios-designable-controls-walkthrough-images/05new.png "Monkey.png için görüntü görünüm görüntü özelliğini ayarlama")](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. Boyut sınıflarını kullanıyoruz gibi bu görüntü görünüm sınırlamak gerekir. İki kez, kısıtlama moduna görüntüye tıklayın. Şimdi kısıtlamak, merkezine merkezi sabitleme tanıtıcı tıklayarak ve dikey ve yatay Hizala:

    [![](ios-designable-controls-walkthrough-images/06new.png "Görüntü ortalama")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Yüksekliğini ve genişliğini sınırlamak için ('şeklinde bone' tutamaçlar) boyutu sabitleme tutamaçları tıklayın ve sırasıyla genişlik ve yükseklik seçin:

    [![](ios-designable-controls-walkthrough-images/07new.png "Kısıtlamaları ekleme")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. Araç çubuğunda güncelleştir düğmesine tıklayarak kısıtlamalarına göre çerçeveyi güncelleştirin:

    [![](ios-designable-controls-walkthrough-images/08new.png "Kısıtlamalar araç çubuğu")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. Ardından, projeyi derleyin. böylece **karalama bilet görünümü** altında görünür **özel bileşenlerin** araç kutusunda:

    [![](ios-designable-controls-walkthrough-images/09new.png "Özel bileşenlerin araç kutusu")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. Sürükle ve bırak bir **karalama bilet görünümü** böylece monkey resminin görünür. Karalama bilet görünümü monkey tamamen, aşağıda gösterildiği gibi kapsar şekilde tutamaçlarını sürükleyin ayarla:

    [![](ios-designable-controls-walkthrough-images/10new.png "Bir karalama bilet görünüm üzerinden resim görünümü")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Karalama bilet görünümün görüntü, her iki görünümde seçmek için sınırlayıcı bir dikdörtgen çizerek kısıtlar. Sınırlamak için kısıtlamalarına göre genişlik, yükseklik, Center ve orta ve güncelleştirme çerçeveleri aşağıda gösterildiği gibi seçenekleri seçin:

    [![](ios-designable-controls-walkthrough-images/11new.png "Ortalama ve kısıtlamaları ekleme")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. Uygulamayı çalıştırın ve "kapalı monkey gösterilmesi için görüntüyü karalama".

    [![](ios-designable-controls-walkthrough-images/10-app.png "Bir örnek uygulama çalıştırma")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Tasarım zamanı özellikleri ekleme

Tasarımcı, özellik türü sayısal, numaralandırma, string, bool, CGSize, UIColor ve UIImage özel denetimler için tasarım zamanı desteği de vardır. Göstermek için şimdi bir özellik ekleyin `ScratchTicketView` "kapalı çizik." görüntü ayarlamak için

Aşağıdaki kodu ekleyin `ScratchTicketView` sınıf özelliği için:

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image 
{
    get { return image; }
    set { 
            image = value;
            SetNeedsDisplay ();
        }
}
```

Biz de için bir null kontrolü eklemek isteyebilirsiniz `Draw` yöntemi şu şekilde:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);
            
        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);       
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Dahil olmak üzere bir `ExportAttribute` ve `BrowsableAttribute` kümesine bağımsız değişkeniyle `true` sonuçları Tasarımcısı'nda kişinin görüntülenmesini özelliğinde **özelliği** paneli. Projeyle yer alan şunun gibi başka bir görüntüye özelliği değiştirmeyi `FillTexture2.png`, sonuçları güncelleştirme denetimi tasarım zamanında, aşağıda gösterildiği gibi:

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Tasarım zamanı özelliklerini düzenleme")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Özet

Bu makalede, biz yanı sıra özel bir denetim oluşturmak, iOS Tasarımcısı'nı kullanarak bir iOS uygulaması kullanma konusunda öğrendiniz. Oluşturma ve Denetim Tasarımcısı'nda kişinin bir uygulama tarafından kullanılabilir hale getirmek için yapı gördüğümüz **araç kutusu**. Ayrıca, tasarımcısında özel denetim özelliklerini nasıl sunacağınızı öğrenin yanı sıra, hem tasarım zamanı ve çalışma zamanının düzgün bir şekilde işler, denetimi uygulamak nasıl inceledik.



## <a name="related-links"></a>İlgili bağlantılar

- [ScratchTicket (örnek)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [gerekli görüntüleri (örnek)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
