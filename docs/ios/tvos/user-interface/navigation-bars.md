---
title: Gezinti denetleyicileri ile çalışma
description: Bu makalede tasarlama ve gezinti çubukları Xamarin.tvOS uygulama içinde çalışma kapsar.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8a9a1c852137a2bcc0d46615e69eef0a245a9768
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-navigation-controllers"></a>Gezinti denetleyicileri ile çalışma

_Bu makalede tasarlama ve gezinti çubukları Xamarin.tvOS uygulama içinde çalışma kapsar._

Gezinti çubukları bir başlık ve isteğe bağlı gezinti çubuğu düğmelerinin görüntülenecek görünüm dön eklenebilir. Kullanıcı bir tablo görünümü, koleksiyon veya seçili öğenin ayrıntılarını gösteren bir alt Görünüm menüsüne gibi bir ana sayfa gezinen olduğunda genellikle bunlar kullanılır.

[![](navigation-bars-images/navbar01.png "Örnek gezinti çubuğu")](navigation-bars-images/navbar01.png#lightbox)

Ayrıca (Center'da görüntülenir) başlığı için bir veya daha fazla gezinti çubuğu düğmelerinin gezinti çubukları içerebilir (`UIBarButtonItem`) ve sol tarafında çubuğunun üzerinde.

> [!IMPORTANT]
> Gezinti çubukları varsayılan olarak tamamen saydamdır. Gezinti çubuğu içeriğini altındaki içerik üzerinde okunabilir kalmasını sağlamak için dikkatli olunmalıdır. Örneğin, ne zaman bir tablo görünümü ya da koleksiyonu içeriği altında kayar.




<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Gezinti çubukları ve film şeritleri

Xamarin.tvOS uygulamasında Gezinti çubukları ile çalışmak için en kolay yolu, onları iOS Tasarımcısı kullanarak uygulamanın UI eklemektir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


1. İçinde **çözüm paneli**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **gezinti çubuğu** gelen **araç** ve ekranın üst kısımdaki görünümü bırakın: 

    [![](navigation-bars-images/navbar02.png "Gezinti çubuğu")](navigation-bars-images/navbar02.png#lightbox)
1. Çift **gezinti çubuğu** seçmeniz **Gezinti öğesi**. İçinde **pencere öğesi** sekmesinde **özellikleri paneli**, ayarlayabileceğiniz **başlık**: 

    [![](navigation-bars-images/navbar03.png "Başlık ayarlama")](navigation-bars-images/navbar03.png#lightbox)
1. Ardından, bir veya daha fazla ekleyebilirsiniz **çubuğu düğmesini öğeleri** çubuğu ucunu için: 

    [![](navigation-bars-images/navbar04.png "A çubuğu düğme öğesi")](navigation-bars-images/navbar04.png#lightbox)
1. Son olarak, kablo yukarı **çubuğu düğmesini öğeleri** eylemler için **olayları** sekmesinde **özellikleri Explorer**: 

    [![](navigation-bars-images/navbar05.png "Bir düğme öğesi eylemi çubuğu")](navigation-bars-images/navbar05.png#lightbox)
1. Değişikliklerinizi kaydedin.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **gezinti çubuğu** gelen **araç** ve ekranın üst kısımdaki görünümü bırakın: 

    [![](navigation-bars-images/navbar02-vs.png "Gezinti çubuğu")](navigation-bars-images/navbar02-vs.png#lightbox)
1. Çift **gezinti çubuğu** seçmeniz **Gezinti öğesi**. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**, ayarlayabileceğiniz **başlık**: 

    [![](navigation-bars-images/navbar03-vs.png "Başlık ayarlama")](navigation-bars-images/navbar03-vs.png#lightbox)
1. Ardından, bir veya daha fazla ekleyebilirsiniz **çubuğu düğmesini öğeleri** çubuğu ucunu için: 

    [![](navigation-bars-images/navbar04-vs.png "Bir düğme öğeleri çubuğu")](navigation-bars-images/navbar04-vs.png#lightbox)
1. Son olarak, kablo yukarı **çubuğu düğmesini öğeleri** eylemler için **olayları** sekmesinde **özellikleri Explorer**: 

    [![](navigation-bars-images/navbar05-vs.png "Bir düğme öğesi eylemleri çubuğu")](navigation-bars-images/navbar05-vs.png#lightbox)
1. Değişikliklerinizi kaydedin.


-----

> [!IMPORTANT]
> Olayları gibi atamak mümkün olmakla birlikte `TouchUpInside` Apple TV ekranında veya dokunma olaylarını destekleyen bir touch sahip olmadığından bir kullanıcı Arabirimi öğesine (örneğin, bir UIButton) iOS Tasarımcısı'nda, hiçbir zaman çağrılır. Her zaman kullanmalısınız `Primary Action` olay işleyicileri tvOS için kullanıcı arabirimi öğeleri oluştururken olay.




Aşağıdaki kodu olay işleyicilerini örneği üzerinde üç farklı BarButtonItems verir: `ShowFirstHotel`, `ShowSecondHotel`, ve `ShowThirdHotel`. Her bir öğe tıklandığında, arka plan görüntüsü `HotelImage` değiştirilir. Bu görünüm denetleyiciye düzenlenmesi (örneğin `ViewController.cs`) dosyası:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

Düğmenin sürece `Enabled` özelliği `true` ve başka bir denetim veya görünümü tarafından kapsanmıyor, Siri uzaktan kullanarak odak öğesi yapılabilir.

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve gezinti çubukları Xamarin.tvOS uygulama içinde çalışma ele.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
