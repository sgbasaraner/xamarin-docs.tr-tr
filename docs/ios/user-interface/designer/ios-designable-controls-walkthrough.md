---
title: İzlenecek yol - özel denetimleri, iOS için Xamarin Tasarımcısı ile kullanma
description: Bu makale, özel bir denetim oluşturmak ve iOS için Xamarin Tasarımcısı'nda kullanmak üzere nasıl gösteren bir adım adım kılavuz sağlar. Sürükle / bir görünümü yoksaymış böylece bir denetim tasarımcının araç kullanılabilmesini kullanmayı gösterir. Ayrıca, tasarım zamanı ve çalışma zamanı sırasında düzgün işler için bir denetim uygulamak nasıl yanı sıra tasarım zamanında ayarlanabilir özelliklerin nasıl oluşturulacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 4a7fb6cba82b52f2a3506df7a36b4813a88ff583
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---using-custom-controls-with-the-xamarin-designer-for-ios"></a>İzlenecek yol - özel denetimleri, iOS için Xamarin Tasarımcısı ile kullanma

_Bu makale, özel bir denetim oluşturmak ve iOS için Xamarin Tasarımcısı'nda kullanmak üzere nasıl gösteren bir adım adım kılavuz sağlar. Sürükle / bir görünümü yoksaymış böylece bir denetim tasarımcının araç kullanılabilmesini kullanmayı gösterir. Ayrıca, tasarım zamanı ve çalışma zamanı sırasında düzgün işler için bir denetim uygulamak nasıl yanı sıra tasarım zamanında ayarlanabilir özelliklerin nasıl oluşturulacağı gösterilmektedir._

## <a name="requirements"></a>Gereksinimler

İOS için Xamarin Tasarımcısı Mac ve Visual Studio 2015 ve 2017 için Visual Studio Windows üzerinde kullanılabilir.

Bu kılavuzda ele içeriği bilindiğini varsayar [Başlarken kılavuzları](~/ios/get-started/index.md).

## <a name="walkthrough"></a>İzlenecek yol

> [!IMPORTANT]
> Xamarin.Studio 5.5 başlayarak, özel denetimler oluşturulduğu şekilde önceki sürümlerine biraz farklıdır. Özel bir denetim ya da oluşturmak için `IComponent` arabirimi (ilişkili uygulama yöntemleriyle) gereklidir veya sınıf olabilir açıklama `[DesignTimeVisible(true)]`. İkinci yöntemi aşağıdaki izlenecek yol örnekte kullanılıyor.


1. Yeni bir çözüm oluşturmak **iOS > Uygulama > tek görünüm uygulaması > C#** şablon adlandırın `ScratchTicket`ve yeni proje sihirbaza devam edin:

    [![](ios-designable-controls-walkthrough-images/01new.png "Yeni bir çözümü oluşturun")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Adlı yeni bir boş sınıfı dosya oluşturma `ScratchTicketView`:

    [![](ios-designable-controls-walkthrough-images/02new.png "Yeni ScratchTicketView sınıf oluşturma")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. İçin aşağıdaki kodu ekleyip `ScratchTicketView` sınıfı:

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
    
1. Çift `Main.storyboard` dosyayı tasarımcısında açın:

    [![](ios-designable-controls-walkthrough-images/03new.png "İOS Tasarımcısı")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. Sürükle ve bırak bir **resim görünümü** gelen **araç** film şeridi görünümünde üzerine.

    [![](ios-designable-controls-walkthrough-images/04new.png "Düzenine eklenen bir resim görünümü")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. Seçin **resim görünümü** değiştirip kendi **görüntü** özelliğine `Monkey.png`.

    [![](ios-designable-controls-walkthrough-images/05new.png "Setting Image View Image property to Monkey.png)](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. Boyutu sınıfları kullanıyoruz gibi Biz bu görüntü görünüm sınırlamak gerekir. Görüntünün iki kez kısıtlaması moduna almak için tıklayın. Şimdi sınırlamak onu merkezine merkezi sabitleme tanıtıcı tıklayarak ve dikey ve yatay olarak hizala:

    [![](ios-designable-controls-walkthrough-images/06new.png "Görüntü ortalama")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Yüksekliğini ve genişliğini sınırlamak için boyutu sabitleme işleyicilerinin ('şeklinde kemik' tanıtıcı) tıklayın ve genişlik ve yükseklik sırasıyla seçin:

    [![](ios-designable-controls-walkthrough-images/07new.png "Kısıtlamaları ekleme")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. Araç çubuğu Güncelleştir düğmesini tıklatarak kısıtlamalarına göre çerçeve güncelleştirin:

    [![](ios-designable-controls-walkthrough-images/08new.png "Kısıtlamaları araç çubuğu")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. Ardından, projeyi derlemek böylece **karalama bilet Görünüm** altında görünür **özel bileşenleri** araç kutusunda:

    [![](ios-designable-controls-walkthrough-images/09new.png "Özel bileşenler araç kutusu")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. Sürükleme ve bırakma bir **karalama bilet Görünüm** böylece monkey görüntünün üzerinde görünür. Karalama bilet görünüm monkey tamamen, aşağıda gösterildiği gibi kapsayan şekilde Sürükle tanıtıcıları ayarla:

    [![](ios-designable-controls-walkthrough-images/10new.png "Görüntü görünüm üzerinden karalama bilet Görünüm")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Her iki görünümde seçmek için bir sınırlayıcı dikdörtgenini çizerek resim görünümü karalama bilet görünümüne kısıtlar. Sınırlamak için kısıtlamalarını temel alarak genişlik, yükseklik, merkezi ve orta ve güncelleştirme çerçeveleri aşağıda gösterildiği gibi seçenekleri seçin:

    [![](ios-designable-controls-walkthrough-images/11new.png "Ortalama ve kısıtlamaları ekleme")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. Uygulamayı çalıştırın ve "devre dışı monkey gösterilmesi için görüntüyü karalama".

    [![](ios-designable-controls-walkthrough-images/10-app.png "Çalıştıran bir örnek uygulama")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Tasarım zamanı özellikleri ekleme

Tasarımcı özellik türü sayısal, numaralandırma, string, bool, CGSize, UIColor ve UIImage özel denetimler için tasarım zamanı destek de içerir. Göstermek üzere bir özelliğe ekleyelim `ScratchTicketView` "kapalı disk bozulmuş." resmi ayarlamak için

Aşağıdaki kodu ekleyin `ScratchTicketView` sınıfı bir özellik için:

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

Biz de bir null denetimi eklemek isteyebilirsiniz `Draw` yöntemi, şu şekilde:

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

Dahil olmak üzere bir `ExportAttribute` ve `BrowsableAttribute` kümesine bağımsız değişkeniyle `true` Tasarımcısı'nda 's görüntülenmesini özelliği sonuçlanıyor **özelliği** paneli. Proje ile gibi dahil başka bir görüntü özelliği değiştirmeyi `FillTexture2.png`, sonuçları güncelleştirilirken Denetim tasarım zamanında aşağıda gösterildiği gibi:

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Tasarım zamanı özelliklerini düzenleme")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Özet

Bu makalede biz yanı sıra özel bir denetim oluşturmak iOS Tasarımcısı'nı kullanarak iOS uygulamada kullanmak nasıl gitti. Ve tasarımcıda ait bir uygulama kullanılabilir hale getirmek için denetimi oluşturmak nasıl gördüğümüz **araç**. Ayrıca, özel denetim özellikleri Tasarımcısı'nda kullanıma sunmak nasıl yanı sıra sağlayacak şekilde tasarım zamanı ve çalışma zamanı sırasında doğru işler denetimi nasıl inceledik.



## <a name="related-links"></a>İlgili bağlantılar

- [ScratchTicket (örnek)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [gerekli görüntüleri (örnek)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
