---
title: "İleti Uygulama Uzantısı temelleri"
description: "Bu makalede gösterir nasıl dahil bir ileti uygulama uzantısı bir Xamarin.iOS çözümde iletileri uygulamayla tümleştirilen ve kullanıcı için yeni işlevler sunar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 083920aba3c8dc83b157b591e194c43935dcc566
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="message-app-extension-basics"></a>İleti Uygulama Uzantısı temelleri

_Bu makalede gösterir nasıl dahil bir ileti uygulama uzantısı bir Xamarin.iOS çözümde iletileri uygulamayla tümleştirilen ve kullanıcı için yeni işlevler sunar._

Yeni bir ileti uygulama uzantısı bütünleşir iOS 10, **iletileri** kullanıcı yeni işlevsellik uygulama ve sunar. Uzantı, metin, etiketler, ortam dosyaları ve etkileşimli iletilerin gönderebilirsiniz.

## <a name="about-message-app-extensions"></a>İleti Uygulama uzantıları hakkında

Yukarıda belirtildiği gibi bir ileti uygulama uzantısı ile tümleştirilir **iletileri** kullanıcı yeni işlevsellik uygulama ve sunar. Uzantı, metin, etiketler, ortam dosyaları ve etkileşimli iletilerin gönderebilirsiniz. İleti Uygulama Uzantısı iki tür mevcuttur:

- **Etiket paketleri** -kullanıcı bir iletiye ekleyebilirsiniz etiketler koleksiyonunu içerir. Etiket paketleri hiçbir kod yazmadan oluşturulabilir.
- **iMessage uygulama** -etiketler seçerek, metin girerek, ortam dosyalarıyla (isteğe bağlı tür dönüşümleri) dahil olmak üzere ve oluşturma, düzenleme ve etkileşim iletileri göndermek için Messages uygulamasının içinde özel bir kullanıcı arabirimi sunabilir.

İleti uygulamaları uzantıları üç ana içerik türlerini sağlar:

- **Etkileşimli iletilerin** -bir uygulamanın oluşturduğu özel ileti içerik türünü, kullanıcı uygulama ileti üzerinde ne zaman dokunur ön planda başlatılacak.
- **Etiketler** -olan kullanıcılar arasında gönderilen iletileri eklenebilir ve uygulama tarafından üretilen görüntüler.
- **Desteklenen diğer içeriği** - uygulama içeriği fotoğrafları, videoları, metin gibi sağlayabilir veya diğer içeriklere bağlantıların türleri her zaman iletileri uygulama tarafından desteklenir.

Yeni iOS 10, ileti uygulama artık kendi özel ve yerleşik uygulama mağazası içerir. İleti uygulamaları uzantıları dahil tüm uygulamalar görüntülenir ve bu depolama alanındaki yükseltildi. Yeni iletiler uygulama bölümü kullanıcılara hızlı erişim sağlamak için iletileri uygulama mağazasından indirilmiş uygulamaları görüntüler.

Ayrıca iOS 10'da, kullanıcının bir uygulamayı kolay bulmasına olanak veren satır içi uygulama Attribution Apple yeni ekledi. Bir kullanıcı içeriği başka bir 2 kullanıcının yoksa bir uygulamadan gönderirse, örneğin, (bir etiket gibi örneğin), yüklü adı gönderen uygulama ileti geçmişi içeriği altında listelenir. Uygulamanın kullanıcı dokunur varsa adı, ileti uygulama Mağazası'nin biz açılması ve Mağazası'nda seçilen uygulama.

İleti uygulamaları uzantıları Geliştirici oluşturma konusunda bilgi sahibi ve tüm standart çerçeveler ve standart iOS uygulaması özelliklerini erişimi vardır var olan iOS uygulamaları benzerdir. Örneğin:

- Uygulama içi satın alma erişimi.
- Apple Pay erişime sahip.
- Aygıt donanım kamera gibi erişimi.

İletisi uygulamaları uzantıları yalnızca 10 İos'ta desteklenir, ancak bu uzantılar Gönder içeriği watchOS ve macOS cihazlarda görüntülenebilir. Yeni _Recents sayfa_ watchOS 3 eklenen, ileti uygulamaları uzantılardan dahil olmak üzere Phone gönderilen son etiketler görüntüler ve izleme bu etiketler Gönder izin verin.

## <a name="about-the-messages-framework"></a>İletileri çerçevesi hakkında

Yeni iOS 10, iletileri framework ileti uygulamaları uzantısı ve ileti uygulama arasında arabirim kullanıcının iOS cihazında sağlar. Kullanıcı bir uygulamadan Messages uygulamasının içinde başlattığında, bu çerçeve uygulamanın bulunmasına izin verir ve veri ve kendi kullanıcı arabirimini düzenlemek için gereken bağlam sağlar.

Uygulama başlatıldıktan sonra kullanıcı üzerinden bir ileti paylaşmak için yeni içerik oluşturmak üzere ile etkileşim kurar. Uygulama iletileri çerçevesi iletileri uygulaması işleme için yeni oluşturulan içerik aktarmak için sonra kullanır.

İletileri framework ve uygulamaları uzantılarını iletisini önceden var olan iOS uygulama uzantıları teknolojileri üzerine oluşturulmuştur. Apple'nın uygulama uzantıları hakkında daha fazla bilgi için lütfen bkz [uygulama uzantısı Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

İleti Uygulama kapsayıcı olarak hareket bu yana bir ana bilgisayar uygulaması kendi ileti uygulamaları uzantılarını sağlamak diğer uzantı Apple sistem genelinde sağlanan noktaları farklı olarak, herhangi bir geliştirici gerekmez. Ancak, geliştiricinin yeni veya var olan iOS uygulaması içinde ileti uygulamaları uzantısı dahil ve paket birlikte sevk seçeneği vardır.

İleti uygulamaları uzantıları bir iOS uygulamanın pakette yer alıyorsa aygıtın giriş ekranında hem de mesajlar uygulamasının içinde ileti uygulama bölümü uygulamanın simgesi görüntülenir. Bir uygulama paketine dahil edilmezse, ileti uygulamaları uzantısı yalnızca ileti uygulama bölmesinde görüntülenir.

İleti uygulamaları uzantıları bir ana bilgisayar uygulaması pakete dahil edilmeyen olsa bile, bu sistem iletisi uygulama bölümü veya ayarları gibi diğer bölümlerinde görüntülenecek olan simgedir gibi geliştirici uygulama simge ileti uygulamaları uzantının paketteki sağlamanız gerekir , uzantısı.

## <a name="about-stickers"></a>Etiketler hakkında

Satır içi başka bir ileti içeriği gönderilmek üzere iMessage kullanıcıların etiketler vererek iletişim kurmak yeni bir yolu olarak etiketler Apple tasarlanmış veya önceki iletiyi balonları konuşma içinde için eklenebilir.

Etiketler nelerdir?

- İleti uygulamaları uzantısı sağlayan görüntüler oldukları.
- Animasyonlu veya statik görüntü olabilirler.
- Bir uygulama içinde görüntü içeriği paylaşmak için yeni bir yolunu sağlarlar.

Etiketler oluşturmanın iki yolu vardır:

1. Bir etiketi paketi iletisi uygulamaları uzantıları Xcode içinde herhangi bir kod eklemeden oluşturulabilir. Gerekli olan tek şey etiketler ve uygulama simgeleri için varlıklar.
2. Standart bir ileti uygulamaları uzantısı oluşturarak, iletileri çerçevesi aracılığıyla kodundan etiketler sağlar.

### <a name="creating-sticker-packs"></a>Etiket paketleri oluşturma

Etiket paketleri Xcode içinde özel bir şablondan oluşturulan ve etiketler kullanılabilmesi için görüntüyü varlıklar statik bir dizi belirtmeniz yeterlidir. Yukarıda belirtildiği gibi herhangi bir kod gerektirmeyen, geliştirici görüntü dosyaları yalnızca etiketler varlık Kataloğu içinde etiketi paketi klasöre sürüklediği.

Görüntüyü bir etiket paketine dahil edilecek aşağıdaki gereksinimleri karşılamalıdır:

- Görüntü PNG, APNG, GIF veya JPEG biçiminde olması gerekir. Apple etiketi varlıklar sağlanırken yalnızca PNG ve APNG biçimlerini kullanarak önerir.
- Animasyonlu etiketler yalnızca APNG ve GIF biçimleri için destek.
- Bunlar ileti balonları konuşmadaki üzerinden kullanıcı tarafından yerleştirilebilir beri etiketi görüntüleri saydam arka plan sağlamalıdır.
- Tek tek resim dosyalarını 500 KB'tan daha az olmalıdır.
- Görüntüleri 206 x 206 işaret noktaları 100 x 100'den küçük veya büyük olamaz.

> [!IMPORTANT]
> Etiket görüntüleri her zaman sağlanmalıdır adresindeki `@3x` 300 x 300 için 618 x 618 piksel aralığında çözümleme. Sistem otomatik olarak oluşturur `@2x` ve `@1x` gerektiği gibi çalışma zamanında sürümleri.

Apple, bunlar tüm olası durumlarda en iyi göründüğünden emin olmak için etiket görüntü varlıklarının çeşitli farklı renkli arka plan (örneğin, beyaz, siyah, kırmızı, sarı ve çok renkli) ve üstünde fotoğraf karşı test önerir.

Etiket paketleri üç kullanılabilir boyutları birinde etiketler sağlayabilirsiniz:

- **Küçük** - 100 x 100 noktaları.
- **Orta** - 136 x 136 noktaları. Varsayılan boyut budur.
- **Büyük** - 206 x 206 noktaları.

Xcode'nın öznitelikleri denetçisi için etiket paketinin tamamını boyutunu ayarlayın ve yalnızca en iyi sonuçlar için Messages uygulamasının içinde etiketi tarayıcıda istenen boyutuna uyacak görüntü varlıklar sağlamak için kullanın.

Daha fazla bilgi için lütfen bkz bizim [dondurma Oluşturucu](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) uygulama ve Apple'nın [iletileri başvuru](https://developer.apple.com/reference/messages).

## <a name="creating-a-custom-sticker-experience"></a>Bir özel etiket deneyimi oluşturma

Uygulama daha fazla denetim veya bir etiket paketi tarafından sağlanan daha esneklik gerektiriyorsa, bu ileti uygulama uzantısını ekleyin ve iletileri çerçevesi aracılığıyla etiketler için bir özel etiket deneyimi sağlar.

Bir özel etiket deneyim oluşturmada yararları nelerdir?

1. Etiketler uygulama kullanıcılara gösterilme biçimini Özelleştir yazmasına izin verir. Örneğin, mevcut etiketler için farklı renkli arka plan üzerinde veya standart kılavuz düzeni dışında bir biçimde.
2. Statik görüntü varlıklar olarak eklenmesini yerine kodundan dinamik olarak oluşturulması gereken etiketler sağlar.
3. Etiket görüntü varlıklarının uygulama mağazası yeni bir sürüm yayın yapmak zorunda kalmadan Geliştirici web sunucusundan dinamik olarak yüklenmesine izin verir.
4. Cihazın kamera erişim için etiketler üzerinde-çalışma sırasında oluşturmasına olanak verir.
5. Uygulama içi satın almalara için sağladığından, kullanıcı uygulama içinde daha fazla etiketlerini satın alabilirsiniz.

Bir özel etiket deneyimi oluşturmak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'yu başlatın
2. Bir ileti uygulama uzantısı eklemek için çözümü açın. 
3. Seçin **iOS** > **uzantıları** > **iMessage uzantısı** tıklatıp **sonraki** düğmesi: 

    [![](intro-to-message-app-extensions-images/message01.png "İMessage uzantısı seçin")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Girin bir **uzantı adı** tıklatıp **sonraki** düğmesi: 

    [![](intro-to-message-app-extensions-images/message02.png "Bir uzantı adı girin")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. Tıklatın **oluşturma** uzantısı düğmesi: 

    [![](intro-to-message-app-extensions-images/message03.png "Oluştur düğmesine tıklayın")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio'yu başlatın.
2. Bir ileti uygulama uzantısı eklemek için çözümü açın. 
3. Seçin **iOS** > **uzantıları** > **iMessage uzantısı** tıklatıp **sonraki** düğmesi: 

    [![](intro-to-message-app-extensions-images/message01w.png "İMessage uzantısı seçin")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Girin bir **uzantı adı** tıklatıp **Tamam** düğmesi

-----

Varsayılan olarak, `MessagesViewController.cs` dosya çözüme eklenir. Bu uzantı ana giriş noktasına ve öğesinden devralınan `MSMessageAppViewController` sınıfı.

İletileri framework kullanılabilir etiketler kullanıcıya sunmak için sınıflar sağlar:

- `MSStickerBrowserViewController` -Etiketler içinde sunulan görünüm denetler. Ayrıca uyacağını `IMSStickerBrowserViewDataSource` etiket sayısı ve verilen tarayıcı dizin için etiket döndürmek için arabirim.
- `MSStickerBrowserView` -Bu, kullanılabilir etiketler görüntülenir görünümüdür.
- `MSStickerSize` -Tarayıcı görünümünde sunulan etiketler kılavuz için tek hücre boyutlar karar verir.

### <a name="creating-a-custom-sticker-browser"></a>Bir özel etiket tarayıcı oluşturma

Geliştirici daha fazla etiket deneyimi kullanıcı için bir özel etiket tarayıcı sağlayarak özelleştirebilirsiniz (`MSMessageAppBrowserViewController`) ileti uygulama uzantısı'nda. Özel Etiket tarayıcı ileti akışında dahil etmek için bir etiket seçerken etiketler kullanıcıya nasıl sunulacağının değiştirir.

Aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. İçinde **çözüm paneli**, uzantının proje adına sağ tıklayın ve seçin **Ekle** > **yeni dosya...**   >  **iOS | Apple Watch** > **Arabirim Denetleyicisi**.
2. Girin `StickerBrowserViewController` için **adı** tıklatıp **yeni** düğmesi: 

    [![](intro-to-message-app-extensions-images/browser01.png "Enter StickerBrowserViewController for the Name")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Açık `StickerBrowserViewController.cs` dosyayı düzenlemek için.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. İçinde **Çözüm Gezgini**, uzantının proje adına sağ tıklayın ve seçin **Ekle** > **yeni dosya...**   >  **iOS | Apple Watch** > **Arabirim Denetleyicisi**.
2. Girin `StickerBrowserViewController` için **adı** tıklatıp **yeni** düğmesi: 

    [![](intro-to-message-app-extensions-images/browser01w.png "Enter StickerBrowserViewController for the Name")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Açık `StickerBrowserViewController.cs` dosyayı düzenlemek için.

-----

Olun `StickerBrowserViewController.cs` şu şekilde görünür:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MonkeyStickers
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MSStickerSize stickerSize) : base (stickerSize)
        {
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            ...
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();
        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            return Stickers[(int)index];
        }
        #endregion
    }
}
```

Yukarıdaki kodu ayrıntılı olarak bakalım. Depolama için uzantı sağlar etiketler oluşturur:

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

Ve iki yöntemlerini geçersiz kılar `MSStickerBrowserViewController` sınıfı bu veri deposu tarayıcıdan veri sağlamak için:

```csharp
public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
{
    return Stickers.Count;
}

public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
{
    return Stickers[(int)index];
}
```

`CreateSticker` Yöntemi bir görüntü varlığı yolunu uzantının paketindeki alır ve yeni bir örneğini oluşturmak için kullandığı bir `MSSticker` koleksiyona ekler bu varlık gelen:

```csharp
private void CreateSticker (string assetName, string localizedDescription)
{

    // Get path to asset
    var path = NSBundle.MainBundle.PathForResource (assetName, "png");
    if (path == null) {
        Console.WriteLine ("Couldn't create sticker {0}.", assetName);
        return;
    }

    // Build new sticker
    var stickerURL = new NSUrl (path);
    NSError error = null;
    var sticker = new MSSticker (stickerURL, localizedDescription, out error);
    if (error == null) {
        // Add to collection
        Stickers.Add (sticker);
    } else {
        // Report error
        Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
    }
}
```

`LoadSticker` Yöntemi çağrılır `ViewDidLoad` (uygulamanın pakete eklenen) adlandırılmış resim varlık bir etiket oluşturmak ve etiketler koleksiyonuna ekleyin.

Özel Etiket tarayıcı uygulamak için düzenleme `MessagesViewController.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using UIKit;
using Messages;

namespace MonkeyStickers
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public StickerBrowserViewController BrowserViewController { get; set;}
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();


            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);
        }
        #endregion
    }
}
```

Bu kod göz ayrıntılı olarak alma, özel tarayıcı için depolama oluşturur:

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

Ve `ViewDidLoad` yöntemi, başlatır ve yeni bir tarayıcı yapılandırır:

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

Ardından tarayıcı görüntülemek için görünümüne ekler:

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>Daha fazla etiket özelleştirme

Daha fazla etiket özelleştirme ileti uygulama uzantısı'nda yalnızca iki sınıf dahil olmak üzere mümkündür:

- `MSStickerView`
- `MSSticker`

Yukarıdaki yöntemleri kullanarak, uzantı standart etiketi tarayıcı yöntemine bağlı değildir etiketi seçimi destekleyebilir. Ayrıca, iki farklı görünüm modlar arasında etiketi görüntüleme değiştirilebilir:

- **Compact** -burada etiket görünüm alt % 25'ini ileti görünümünü alır varsayılan mod budur.
- **Genişletilmiş** -etiketi View tüm ileti görünümü doldurur.

Bu etiket görünüm Bu modlar arasında program aracılığıyla veya el ile kullanıcı tarafından değiştirilebilir.

Aşağıdaki örnekte, iki farklı görünüm modlar arasında geçiş yapma işleme göz atın. İki farklı görünümü denetleyicileri her durum için gerekli olacaktır. `StickerBrowserViewController` Tanıtıcıları **Compact** görünümü ve görünüm aşağıdaki gibi:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MessageExtension
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set; }
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MessagesViewController messagesAppViewController, MSStickerSize stickerSize) : base (stickerSize)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("add", "Add New Sticker");
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();

        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            // Wanting to add a new sticker?
            if (index == 0) {
                // Yes, ask controller to present add sticker interface
                MessagesAppViewController.AddNewSticker ();
                return null;
            } else {
                // No, return existing sticker
                return Stickers [(int)index];
            }
        }
        #endregion
    }
}
```

`AddStickerViewController` İşleyecek **Genişletilmiş** etiketi görünümü ve görünüm aşağıdaki gibi:

```csharp
using System;
using Foundation;
using UIKit;
using Messages;

namespace MessageExtension
{
    public class AddStickerViewController : UIViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set;}
        public MSSticker NewSticker { get; set;}
        #endregion

        #region Constructors
        public AddStickerViewController (MessagesViewController messagesAppViewController)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Override Method
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Build interface to create new sticker
            var cancelButton = new UIButton (UIButtonType.RoundedRect);
            cancelButton.TouchDown += (sender, e) => {
                // Cancel add new sticker
                MessagesAppViewController.CancelAddNewSticker ();
            };
            View.AddSubview (cancelButton);

            var doneButton = new UIButton (UIButtonType.RoundedRect);
            doneButton.TouchDown += (sender, e) => {
                // Add new sticker to collection
                MessagesAppViewController.AddStickerToCollection (NewSticker);
            };
            View.AddSubview (doneButton);
            
            ...
        }
        #endregion
    }
}
```

`MessageViewController` İstenen durumu sürücü için bu görünümü denetleyicileri uygular:

```csharp
using System;
using UIKit;
using Messages;

namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public bool IsAddingSticker { get; set;}
        public StickerBrowserViewController BrowserViewController { get; set; }
        public AddStickerViewController AddStickerController { get; set;}
        #endregion

        #region Constructors
        protected MessagesViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Public Methods
        public void PresentStickerBrowser ()
        {
            // Is the Add sticker view being displayed?
            if (IsAddingSticker) {
                // Yes, remove it from view
                AddStickerController.RemoveFromParentViewController ();
                AddStickerController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);

            // Save mode
            IsAddingSticker = false;
        }

        public void PresentAddSticker ()
        {
            // Is the sticker browser being displayed?
            if (!IsAddingSticker) {
                // Yes, remove it from view
                BrowserViewController.RemoveFromParentViewController ();
                BrowserViewController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (AddStickerController);
            AddStickerController.DidMoveToParentViewController (this);
            View.AddSubview (AddStickerController.View);

            // Save mode
            IsAddingSticker = true;
        }

        public void AddNewSticker ()
        {
            // Switch to expanded view mode
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void CancelAddNewSticker ()
        {
            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }

        public void AddStickerToCollection (MSSticker sticker)
        {
            // Add sticker to collection
            BrowserViewController.Stickers.Add (sticker);

            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (this, MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Create new Add controller and configure it as well
            AddStickerController = new AddStickerViewController (this);
            AddStickerController.View.Frame = View.Frame;

            // Initially present the sticker browser
            PresentStickerBrowser ();
        }

        public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            base.DidTransition (presentationStyle);

            // Take action based on style
            switch (presentationStyle) {
            case MSMessagesAppPresentationStyle.Compact:
                PresentStickerBrowser ();
                break;
            case MSMessagesAppPresentationStyle.Expanded:
                PresentAddSticker ();
                break;
            }
        }
        #endregion
    }
}
```

Kullanıcının kendi kullanılabilir koleksiyona yeni bir etiket eklemek için istediğinde yeni bir `AddStickerViewController` görünür denetleyici ve etiket görünüm yapılan girer **Genişletilmiş** görünümü:

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

Kullanıcı eklemek için bir etiket seçtiğinde kullanılabilir kendi koleksiyonuna eklenir ve **Compact** görünüm istendi:

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
}
```

`DidTransition` İki modlar arasında geçiş yapma işlemek için yöntem geçersiz:

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
``` 

## <a name="summary"></a>Özet

Bu makalede ele alınan bir ileti uygulama uzantısı ile tümleşen bir Xamarin.iOS çözümü dahil **iletileri** uygulama ve kullanıcıya mevcut yeni işlevsellik. Bu metin, etiketler, ortam dosyaları ve etkileşimli iletileri göndermek için uzantısını kullanarak ele.



## <a name="related-links"></a>İlgili bağlantılar

- [Dondurma Oluşturucusu (örnek)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [İletileri başvurusu](https://developer.apple.com/reference/messages)
- [Uygulama Uzantısı Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
