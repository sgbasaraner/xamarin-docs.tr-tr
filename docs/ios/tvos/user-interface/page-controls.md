---
title: "Sayfa denetimiyle çalışma"
description: "Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde sayfa denetimiyle çalışma kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b1b53fefdd72c36bdffd3c5ade0b8d86da225b14
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-page-control"></a>Sayfa denetimiyle çalışma

_Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde sayfa denetimiyle çalışma kapsar._

Bazen sayfaları veya görüntüleri bir dizi Xamarin.tvOS uygulamanızda görüntülenecek gerekebilir. Sayfası denetimi, bir kullanıcı üzerinde en fazla sayfa sayısını dışında hangi sayfasını açıkça göstermek için tasarlanmıştır. Bir sayfa denetimi bir dizi arka plan şeklinde bir koyu, oval karşı nokta görüntüler. Geçerli sayfa dolu nokta görüntülenir, diğer tüm sayfalar boş noktalar göster. Varsa, arka plan alanında sığması için çok fazla sayfa denetimini dış çoğu nokta küçük.

[![](page-controls-images/page01.png "Örnek sayfası denetimi")](page-controls-images/page01.png#lightbox)

Yalnızca kullanıcı geri bildirim sağlamak için tasarlanmış etkileşimli olmayan bir öğe sayfa denetiminde. Geçerli sayfa numarası (örneğin, hareketleri veya düğmeleri) değiştirmek için diğer denetimleri eklemeniz gerekir.

Apple sayfası denetimi kullanırken aşağıdaki önerileri vardır:

- **Yalnızca tam koleksiyonları kullanımda** -sayfası denetimleri iş en iyi şekilde tek bir koleksiyonda mevcut birden çok sayfada görüntülemek için bir tam ekran ortamda.
- **Sayfa sayısı sınırı** -sayfası denetimleri çalışma sayfaları on (10) veya daha az ve en fazla yirmi (20) sayfaları için en iyisidir. Yirmiden fazla sayfalar için kullanmayı bir [koleksiyon görünümü](~/ios/tvos/user-interface/collection-views.md) ve kılavuzda sayfalarını görüntüleme.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>Sayfa denetimleri ve film şeritleri

Xamarin.tvOS uygulamada sayfa denetimleri ile çalışmak için en kolay yolu, onları iOS Tasarımcısı kullanarak uygulamanın UI eklemektir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

    
1. İçinde **çözüm paneli**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **sayfası denetimi** gelen **araç** ve görünümünde bırak: 

    [![](page-controls-images/page02.png "Bir sayfa denetimi")](page-controls-images/page02.png#lightbox)
1. İçinde **pencere öğesi sekmesini** , **özellikleri paneli**, gibi çeşitli özellikleri sayfasında denetiminin ayarlayın, **geçerli sayfa** ve **# sayfa**: 

    [![](page-controls-images/page03.png "Pencere öğesi sekmesi")](page-controls-images/page03.png#lightbox)
1. Ardından, denetimleri veya hareketleri geri taşımak ve sayfaları toplulukta iletmek için görünümüne ekleyin.
1. Son olarak, Ata **adları** denetimlere için C# kodunda yanıt vermesi. Örneğin: 

    [![](page-controls-images/page04.png "Ad denetimi")](page-controls-images/page04.png#lightbox)
1. Değişikliklerinizi kaydedin.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **sayfası denetimi** gelen **araç** ve görünümünde bırak: 

    [![](page-controls-images/page02-vs.png "Bir sayfa denetimi")](page-controls-images/page02-vs.png#lightbox)
1. İçinde **pencere öğesi sekmesini** , **özellikleri Explorer**, gibi çeşitli özellikleri sayfasında denetiminin ayarlayın kendi **geçerli sayfa** ve **sayfalarısayısı**: 

    [![](page-controls-images/page03-vs.png "Pencere öğesi sekmesi")](page-controls-images/page03-vs.png#lightbox)
1. Ardından, denetimleri veya hareketleri geri taşımak ve sayfaları toplulukta iletmek için görünümüne ekleyin.
1. Son olarak, Ata **adları** denetimlere için C# kodunda yanıt vermesi. Örneğin: 

    [![](page-controls-images/page04-vs.png "Ad denetimi")](page-controls-images/page04-vs.png#lightbox)
1. Değişikliklerinizi kaydedin.
    

-----

> [!IMPORTANT]
> Olayları gibi atamak mümkün olmakla birlikte `TouchUpInside` Apple TV ekranında veya dokunma olaylarını destekleyen bir touch sahip olmadığından bir kullanıcı Arabirimi öğesine (örneğin, bir UIButton) iOS Tasarımcısı'nda, hiçbir zaman çağrılır. Her zaman kullanmalısınız `Primary Action` olay işleyicileri tvOS için kullanıcı arabirimi öğeleri oluştururken olay.




Görünüm denetleyicinizi Düzenle (örneğin `ViewController.cs`) dosya ve değiştirilmekte sayfalarını işlemek için kodu ekleyin. Örneğin:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public nint PageNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            PageView.Pages = 6;
            ShowCat ();
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void NextCat (UIBarButtonItem sender) {

            // Display next Cat
            if (++PageNumber > 5) {
                PageNumber = 5;
            }
            ShowCat();

        }

        partial void PreviousCat (UIBarButtonItem sender) {
            // Display previous cat
            if (--PageNumber < 0) {
                PageNumber = 0;
            }
            ShowCat();
        }
        #endregion

        #region Private Methods
        private void ShowCat() {

            // Adjust UI
            PreviousButton.Enabled = (PageNumber > 0);
            NextButton.Enabled = (PageNumber < 5);
            PageView.CurrentPage = PageNumber;

            // Display new cat
            CatView.Image = UIImage.FromFile(string.Format("Cat{0:00}.jpg",PageNumber+1));
        }
        #endregion
    }
}
```

Sayfa denetimini iki özelliklerini daha yakın bir göz atalım. İlk olarak, en fazla sayfa sayısını belirtmek için aşağıdakileri kullanın:

```csharp
PageView.Pages = 6;
```

Geçerli sayfa numarasını değiştirmek için aşağıdaki kodu kullanın:

```csharp
PageView.CurrentPage = PageNumber;
```

`CurrentPage` Sıfır (0), ilk sayfasının sıfır olur ve son bir en fazla sayfa sayısını eksiği olacak şekilde dayalı bir özelliktir.

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve Xamarin.tvOS uygulama içinde sayfa denetimiyle çalışma kapsamına.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
