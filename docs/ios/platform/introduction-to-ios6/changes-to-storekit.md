---
title: StoreKit yapılan değişiklikler
description: "iOS 6 deposu Seti API'sine iki değişiklikler içermektedir: iTunes (ve uygulama mağazası/iBookstore) görüntüleme yeteneğini ürünlerinden uygulamanızı ve bir yeni uygulama içinde nerede Apple gerçekleştirecektir indirilebilir dosyalarınızı seçeneği satın alın. Bu belgede, bu özellikleri Xamarin.iOS ile uygulamak açıklanmaktadır."
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8a7a70c3f84518141cf44d630fb4137051d0c866
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="changes-to-storekit"></a>StoreKit yapılan değişiklikler

_iOS 6 deposu Seti API'sine iki değişiklikler içermektedir: iTunes (ve uygulama mağazası/iBookstore) görüntüleme yeteneğini ürünlerinden uygulamanızı ve bir yeni uygulama içinde nerede Apple gerçekleştirecektir indirilebilir dosyalarınızı seçeneği satın alın. Bu belgede, bu özellikleri Xamarin.iOS ile uygulamak açıklanmaktadır._

İOS6 ana değişiklikleri deposu Seti için bu iki yeni özellikleri şunlardır:

-   **Uygulama içeriği görüntüleme & satınalma** – kullanıcılar satın alabilirsiniz ve indirme uygulamalar, müzik, books ve diğer iTunes içerik uygulamanızı ayrılmadan. Ayrıca, satın alma yükseltmek veya gözden geçirme ve derecelendirmeleri yalnızca teşvik kendi uygulamalarınızı bağlayabilirsiniz. 
-   **Uygulama içi satın barındırılan içeriği** – Apple depolamak ve dosyalarınızı barındırmak ayrı bir sunucu gereksinimini ortadan kaldırır, otomatik olarak arka plan indirmeyi desteklediğinden ve uygulama içi satın alma ürünlerinizi ile ilişkili içerik teslim daha az kod yazın. 


Bu belgede, mevcut Xamarin.iOS birlikte okunmasına önerilir [uygulama içi satın alma](~/ios/platform/in-app-purchasing/index.md) belgeleri.

## <a name="requirements"></a>Gereksinimler

Bu belgede ele alınan deposu Seti özellikleri, iOS 6 ve Xamarin.iOS 6.0 birlikte Xcode 4.5 gerektirir.


## <a name="in-app-content-display--purchasing"></a>Uygulama içerik görüntüleme ve satın alma

İOS yeni uygulama içi satın alma özelliği, ürün bilgilerini görüntülemek ve satın alma veya ürün, uygulamanızın içinde indirmek kullanıcıların sağlar.
Daha önce uygulamaları iTunes, App Store veya özgün uygulamayı bırakarak kullanıcının oluşturacağı iBookstore tetiklemek gerekir. Bunlar bittiğinde bu yeni özellik, uygulamanızın kullanıcı otomatik olarak döndürür.

 [![](changes-to-storekit-images/image1.png "Bir uygulama satın alma sonra otomatik olarak döndürme")](changes-to-storekit-images/image1.png#lightbox)

Bir dizi nerede olabilir yararlı senaryolar vardır (ancak bunlarla sınırlı değil):

-   **Uygulamanızı değerlendirmelerini kullanıcılar görünümü teşvik eder** – kullanıcı oranı ve uygulamanızı ayrılmanız gerekmeden gözden geçirin. böylece uygulama mağazası sayfasını açabilirsiniz. 
-   **Çapraz yükseltme uygulamaları** – satınalma/hemen karşıdan yükleme olanağı yayımlama diğer uygulamaları görmek kullanıcının verin. 
-   **Kullanıcıların yardımcı olma bulmak ve içeriği indirme** – kullanıcıların uygulamanızı bulur, yönetir veya (ör. toplayan içerik satın yardımcı olun Müzik ilgili uygulama çalma şarkıya listesini sağlayın ve şarkıların uygulama içinde satın alınmasına izin ver). 


Bir kez `SKStoreProductViewController` görüntülenen iTunes, App Store veya iBookstore gibi kullanıcı ürün bilgileri ile etkileşim kurabilirsiniz. Şunları içerir:

-  (Uygulamaları için), ekran görüntüleri görüntüleme
-  Örnekleme şarkıya veya video (için müzik, TV programları ve filmler)
-  Okuma (ve yazma) inceler,
-  Satın alma ve indirme, hangi tamamen görünüm denetleyicisini ve deposu Seti içinde gerçekleşir. Bunun çalışması ek kod sağlamak uygulamanız gerekmez. 


Bazı seçenekler içinde olduğunu unutmayın `SKStoreProductViewController` zorla uygulamanızı bırakın ve tıklayarak gibi ilgili mağazası uygulaması açmak için kullanıcının devam edecek **ilgili ürünler** veya bir uygulamanın **Destek** bağlantı.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

Bir ürün herhangi bir uygulama içinde göstermek için API çok basittir: oluşturma ve görüntüleme yalnızca gerektiren bir `SKStoreProductViewController`. Oluşturma ve bir ürün göstermek için aşağıdaki adımları izleyin:

1.  Oluşturma bir `StoreProductParameters` görünüm denetleyiciye parametreleri geçirmek için nesne dahil olmak üzere `productId` oluşturucuda. 
1.  Örneği `SKProductViewController` . Bir sınıf düzeyinde alan atayın. 
1.  Görünüm denetleyicinin için bir işleyici atamak `Finished` görünüm denetleyicisini kapatmak olay. Bu olay, kullanıcı basarsa iptal ettiğinizde çağrılır; veya aksi halde bir işlem içinde görünüm denetleyicisini sonlandırır. 
1.  Çağrı `LoadProduct` tümleştirilmesidir yöntemi `StoreProductParameters` ve tamamlanma işleyici. Tamamlama işleyici ürün isteği başarıyla olduğunu denetleyin ve gerekiyorsa, sunmak `SKProductViewController` kalıcı olarak. Ürün alınamıyor durumda uygun hata işleme eklenmesi gerekir. 

### <a name="example"></a>Örnek

*ProductView* proje *StoreKit* bu makaledeki örnek kod uygulayan bir `Buy` herhangi bir ürün kabul yöntemi Apple kimliği ve görüntüler `SKStoreProductViewController`. Aşağıdaki kod belirli herhangi Apple kimliği için ürün bilgilerini görüntüler:

```csharp
void Buy (int productId)
{
    var spp = new StoreProductParameters(productId);
    var productViewController = new SKStoreProductViewController ();
    // must set the Finished handler before displaying the view controller
    productViewController.Finished += (sender, err) => {
        // Apple's docs says to use this method to close the view controller
        this.DismissModalViewControllerAnimated (true);
    };
    productViewController.LoadProduct (spp, (ok, err) => { // ASYNC !!!
        if (ok) {
            PresentModalViewController (productViewController, true);
        } else {
            Console.WriteLine (" failed ");
            if (err != null)
                Console.WriteLine (" with error " + err);
        }
    });
}
```

Uygulama şu şekilde görünür – indirme veya satın alma tamamen içinde oluşur çalıştırırken `SKStoreProductViewController`:

 [![](changes-to-storekit-images/image2.png "Uygulama çalışırken şöyle")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>Eski işletim sistemlerini destekleme

Örnek uygulama, uygulama mağazası, iTunes veya iBookstore önceki iOS sürümlerinde nasıl açılacağı gösterilmektedir kodu içerir. Kullanım `OpenUrl` düzgün hazırlanmış açmak için yöntemini **itunes.com** URL.

Aşağıdaki gibi çalıştırmak için hangi kod belirlemek için sürüm denetimi uygulayabilirsiniz:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (6,0)) {
    // do iOS6+ stuff, using SKStoreProductViewController as shown above
} else {
    // don't do stuff requiring iOS 6.0, use the old syntax 
    // (which will take the user out of your app)
    var nsurl = new NSUrl("http://itunes.apple.com/us/app/angry-birds/id343200656?mt=8");
    UIApplication.SharedApplication.OpenUrl (nsurl);
}
```

### <a name="errors"></a>Hatalar

Aşağıdaki hata kullandığınız Apple kimliği geçerli değilse, bu yana kafa karıştırıcı olabilir bir ağ veya kimlik doğrulama sorunu çeşit gelir meydana gelir.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Objective-C belgeleri okuma

Apple Geliştirici Portalı deposu Seti hakkında okuma geliştiriciler, bir protokol – görürsünüz [SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) – bu yeni özellik ile ilgili olarak ele. Temsilci protokolü olarak sunulan tek yöntemi – productViewControllerDidFinish – sahip `Finished` olayda `SKStoreProductViewController` Xamarin.iOS içinde.


## <a name="determining-apple-ids"></a>Apple kimliklerini belirleme

Gerekli Apple kimliği `SKStoreProductViewController` olan bir *numarası* (paket kimliği ile "com.xamarin.mwc2012" gibi karıştırılmamalıdır için). Görüntülemek, aşağıda listelenen istediğiniz ürünleri için Apple kimliği bulabilirsiniz birkaç farklı yolu vardır:

### <a name="itunesconnect"></a>iTunesConnect

Yayımlama uygulamalar için kolayca **Apple kimliği** iTunes Bağlan içinde:

 [![](changes-to-storekit-images/image3.png "Apple kimliği iTunes Bağlan bulma")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />


### <a name="search-api"></a>Arama API

Apple App Store, iTunes ve iBookstore tüm ürünleri sorgulamak için bir dinamik arama API sağlar. API arama erişim hakkında bilgi bulunabilir [Apple'nın ilişkilendirdiğinizden kaynakları](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html), ancak API herkesin (yalnızca kayıtlı kuruluşları) sunulur. Sonuçta elde edilen JSON bulmak için ayrıştırılabilir `trackId` diğer bir deyişle Apple ile kullanılacak kimliği `SKStoreProductViewController`.

Sonuçları görüntüleme bilgileri ve ürün uygulamanızda işlemek için kullanılan resmi URL'leri dahil diğer meta verileri de içerir.

Bazı örnekler şunlardır:

-   **iBooks uygulama*- [http://itunes.apple.com/search?term=ibooks&amp;varlık yazılım =&amp;ülke = us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us) 
-   **Nokta ve Kangaroo iBook*- [http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;varlık ebook =&amp;ülke = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us) 


### <a name="enterprise-partner-feed"></a>Kurumsal iş ortağı besleme

Apple, ürünlerinin bir eksiksiz veri dökümü onaylı iş ortaklarıyla indirilebilir veritabanı hazır düz dosya biçiminde sağlar. Erişim için uygun değilse [iş ortağı kuruluş akış](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html) herhangi bir ürün için Apple kimliği, bir veri kümesini bulunabilir.

İş ortağı kuruluş akışı, çok sayıda kullanıcı üyeleri olduğuna dikkat edin [ilişkilendirdiğinizden Program](http://www.apple.com/itunes/affiliates) ürün satış kazanılan komisyonlar sağlar. `SKStoreProductViewController` İlişkilendirmeden kimlikleri desteklemez (yazma; aynı anda bu Apple'nın gelecek eklenebilir).

### <a name="direct-product-links"></a>Doğrudan ürün bağlantıları

Bir ürün için Apple kimliği, iTunes Önizleme URL bağlantısını çıkarsanabileceği.
Tüm iTunes ürün bağlantı (için uygulamaları, müzik veya books) ile başlayan URL parçası Bul `id` ve izleyen numarasını kullanın.

Örneğin, iBooks doğrudan bağlantı.

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

ve Apple kimliği **364709193**. Benzer şekilde MWC2012 uygulama için doğrudan bağlantıdır

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

ve Apple kimliği **496963922**.

## <a name="in-app-purchase-hosted-content"></a>Uygulama içi satın alma içerik barındırılan

İndirilebilir içeriği (örneğin, kitap veya diğer medya, oyun düzeyi resim ve yapılandırma veya diğer büyük dosyaları), uygulama içi satın almalara oluşur, web sunucunuz üzerinde barındırılması için bu dosyalar kullanılan ve uygulamaları güvenli bir şekilde sonra bunları indirmek için kod dahil gerekiyordu satın alın. İOS 6 Apple ayrı bir sunucu gereksinimini kaldırma burada dosyalarınızı kendi sunucularındaki barındıracak bir seçeneği sunulmuştur. Özellik yalnızca olmayan tüketilebilir ürünler için (kaynaklarda veya abonelikler) kullanılabilir. Apple'nın barındırma hizmeti kullanmanın yararları şunlardır:

-  Barındırma & bant genişliği maliyetlerini kaydedin.
-  Büyük olasılıkla daha ölçeklenebilir kullanmakta olduğunuz ne olursa olsun sunucu ana daha. 
-  Herhangi bir sunucu tarafı işlem oluşturmak zorunda değilsiniz beri yazmak için daha az kod. 
-  Arka plan indirme sizin için uygulanır.


Not: Gerçek bir aygıtla test etmeniz gerekir böylece test iOS simülatörü desteklenmiyor, uygulama içi satın alma içeriği barındırılan.

### <a name="hosted-content-basics"></a>Barındırılan içerik temelleri

İOS 6 önce bir ürün için iki yol vardı (daha ayrıntılı olarak açıklanan [Xamarin'ın uygulama içi satın alma](~/ios/platform/in-app-purchasing/index.md) belgelerine):

-   **Yerleşik ürünleri** –, 'kilidi' satın alarak, ancak uygulama (ya da kod veya katıştırılmış kaynakları olarak) için yerleşik olan özellikler. Kilidi fotoğraf filtreleri veya oyun power-ups yerleşik ürünleri örneklerindendir. 
-   **Ürünler sunucu teslim** – satınalma sonra uygulama içeriği, çalışan bir sunucudan karşıdan yüklemeniz gerekir. Bu içerik satın alma sırasında indirilir, cihazda depolanan ve ürün sağlayan bir parçası olarak çizilir. Defterleri, dergi sorunları veya arka plan resmi ve yapılandırma dosyasından oluşur oyun düzeyleri örnek olarak verilebilir. 


İOS 6 Apple sunucusu teslim ürünleri çeşitlemesi sunar: içerik dosyalarınızı sunucularındaki barındıracak. Çünkü, ayrı bir sunucu çalışması için gerekli değildir ve depolama Seti önceden kendiniz yazmak zorunda arka plan indirme işlevsellik sağlar sunucu teslim ürünleri oluşturmak daha kolay yapar. Apple'nın barındırma avantajlarından yararlanmak için içerik için yeni uygulama içi satın alma ürünler barındırma etkinleştirmek ve onu yararlanmak için depolama Seti kodunuzu değiştirin. Ürün içerik dosyalarını sonra Xcode kullanılarak oluşturulmuş ve Apple'nın sunucularına gözden geçirme ve sürüm karşıya.

 [![](changes-to-storekit-images/image4.png "Derleme ve teslim işlemi")](changes-to-storekit-images/image4.png#lightbox)

Uygulama içi satın alma sağlamak için uygulama mağazası kullanma *ile içerik barındırılan* aşağıdaki Kurulum ve yapılandırma gerektirir:

-   **iTunes Bağlan** –, *gerekir* bunlar sizin adınıza toplanan fon havalesi şekilde bankacılık ve vergi bilgilerinizi Apple'a sağlıyordu. Ardından, ürünler, satış ve satın alma test etmek için korumalı alan kullanıcı hesaplarını ayarlama yapılandırabilirsiniz.  *Ayrıca barındırılan içerik yapılandırmalısınız**Apple ile barındırmak istediğiniz bu Tüketilemezi ürünler için* *.* 
-   **iOS sağlama portalı** – bir paket tanımlayıcısı oluşturma ve uygulama içi satın alma destekleyen herhangi bir uygulama için olduğu gibi uygulamanız için App Store erişimi etkinleştirme. 
-   **Kit depolamak** – ürünleri görüntüleme, ürün satın alma ve işlemleri geri yükleme için uygulamanız için kod ekleme.  *İOS 6 deposu Seti ayrıca ilerleme güncelleştirmeleri ile arka planda ürün içeriğinizi indirme yönetir.* 
-   **Özel kod** – müşteriler tarafından gerçekleştirilen satın alma işlemleri izlemek ve ürünleri veya hizmetleri satın aldığınız sağlamak için. Yeni iOS 6 deposu Seti sınıflar gibi kullanan `SKDownload` Apple tarafından barındırılan içerik almak için. 


Aşağıdaki bölümlerde, oluşturma ve satın alma yönetmeye paketini karşıya barındırılan içerik uygulamak ve indirme işlemini, bu makale için örnek kodu kullanarak açıklamaktadır.


### <a name="sample-code"></a>Örnek kod

Örnek Proje *HostedNonConsumables* (StoreKitiOS6.zip içinde), bir uygulama, barındırılan kullanan içeriği oluşturmak gösterilmiştir. Uygulama iki "kitap başlıkları" içerik için hangi Apple'nın sunucularda barındırılan satışı sunar. Daha karmaşık içerik gerçek bir uygulamada kullanılabilir olsa da içeriği bir metin dosyası ve bir görüntü oluşur.

Uygulama, öncesinde, sırasında ve sonrasında bir satın alma şöyle görünür:

 [![](changes-to-storekit-images/image5.png "Öncesinde, sırasında ve sonrasında bir satın alma uygulama şöyle görünür")](changes-to-storekit-images/image5.png#lightbox)

Resim ve metin dosyası indirilir ve uygulamanın belgeler dizinine kopyalanır. Bkz: [dosya sistemi belgeleriyle çalışma](~/ios/app-fundamentals/file-system.md) uygulama depolaması için kullanılabilecek farklı dizinleri hakkında daha fazla bilgi için.

## <a name="itunes-connect"></a>iTunes Connect

Apple kullanacağı yeni ürünler oluşturma içeriğinin zaman barındırma seçtiğinizden emin **olmayan tüketilebilir** ürün türü. Diğer ürün türleri, içerik barındırma desteklemez. Ayrıca, sizin için içerik barındıran etkinleştirmemelisiniz *varolan* satmak; yalnızca yeni ürünler için içerik barındırma kapatma ürünler.

 [![](changes-to-storekit-images/image6.png "Olmayan tüketilebilir ürün türü seçin")](changes-to-storekit-images/image6.png#lightbox)

Girin bir **ürün kimliği**. Bu ürün için içerik oluşturduğunuzda, bu daha sonra gerekli olacaktır.

 [![](changes-to-storekit-images/image7.png "Ürün kimliği girin")](changes-to-storekit-images/image7.png#lightbox)

İçerik barındıran durumunu Ayrıntılar bölümünde ayarlanır. (Bazı test içerik karşıya olsa bile) iptal etmek isterseniz, uygulama içi satın çalıştırılmadan önce yalnızca "Apple ile ana içerik" onay kutusunun işaretini kaldırın. Uygulama içi satın alma Canlı geçti sonra ancak içerik barındırma kaldırılamaz.

 [![](changes-to-storekit-images/image8.png "Apple ile içeriği barındırma")](changes-to-storekit-images/image8.png#lightbox)

İçeriği barındırma açtıktan sonra ürün girer **karşıya yükleme için bekleyen** durumu ve bu iletiyi göster:

 [![](changes-to-storekit-images/image9.png "Ürünü için yükleme durumunu bekleme girin ve bu iletiyi gösterme")](changes-to-storekit-images/image9.png#lightbox)

İçerik şimdi Xcode ile oluşturulması gerekir ve Arşiv aracı kullanılarak yüklenir. İçerik paketleri oluşturma için yönergeler bir sonraki bölümde verilen **oluşturma. PKG dosyaları**.

## <a name="creating-pkg-files"></a>Oluşturuluyor. PKG dosyaları

Apple'a yüklemesini içerik dosyalarını aşağıdaki kısıtlamalar karşılaması gerekir:

-  2 GB boyutunda aşamaz.
-  Yürütülebilir kod (veya dışındaki içeriği noktası simgesel bağlantı) içeremez. 
-  (Bir .plist dosyası dahil) düzgün bir şekilde biçimlendirilmesi gerekir ve bir .pkg dosya uzantısına sahip. Xcode kullanarak bu yönergeleri izleyin, bu otomatik olarak yapılır. 


Bu kısıtlamalar karşıladıklarından sürece, birçok farklı dosyalar ve dosya türlerinin ekleyebilirsiniz. İçerik, uygulamanızın teslim edilmeden önce daraltılmış ve kodunuzu erişim izni vermeden önce deposu paketi tarafından sıkıştırması.

Bir içerik paketi dosyalarını karşıya yükledikten sonra yeni içerikle değiştirilebilir. Yeni içerik karşıya ve gözden geçirme/onayı normal işlem aracılığıyla gönderilir. Artırmanız gerekir `ContentVersion` güncelleştirilmiş içerik paketleri alanındaki.

### <a name="xcode-in-app-purchase-content-projects"></a>Xcode uygulama içi satın alma içerik projeleri

Uygulama içi satın alma ürünleri için içerik paketleri şu anda oluşturma Xcode gerektirir. Hayır, gerekli OBJECTIVE-C kodlama yoktur; Xcode bu paketleri için yalnızca dosyalarınızı ve bir plist içeren bir yeni bir proje türüne sahip.

Örnek uygulamamız kitap başlıkları satışı sahip – her bölüm içerik paketi içerir:

-  bir metin dosyası ve
-  Bu bölümde temsil etmek için bir görüntü.


Başlangıç seçerek **Dosya > Yeni proje** menüsünde seçerek **uygulama içi satın alma içerik**:

 [![](changes-to-storekit-images/image10.png "Uygulama içi satın alma İçerik Seç")](changes-to-storekit-images/image10.png#lightbox)

Girin **ürün adı** ve **şirket tanımlayıcısını** şekilde **paket tanımlayıcı** eşleşen **ürün kimliği** iTunes girdiğiniz Bu ürün için bağlayın.

 [![](changes-to-storekit-images/image11.png "Ad ve tanımlayıcı girin")](changes-to-storekit-images/image11.png#lightbox)

Boş olacaktır artık **uygulama içi satın alma içerik** projesi. Sağ tıklayarak ve **dosyaları Ekle...** veya bunların içine sürükleyin **Proje Gezgini**. Emin **ContentVersion** (1.0 Başlat, ancak bunu artırmak daha sonra içeriği güncelleştirmek seçerseniz unutmayın) doğrudur.

Bu ekran Xcode projesi ve ana penceresinde görünür plist girişler dahil içerik dosyalarla gösterir:

 [![](changes-to-storekit-images/image12.png "Bu ekran görüntüsü, proje ve ana penceresinde görünür plist girişler dahil içerik dosyalarla Xcode gösterir.")](changes-to-storekit-images/image12.png#lightbox)

Tüm içerik dosyalarının ekledikten sonra bu projeyi kaydedin ve daha sonra yeniden düzenleme veya karşıya yükleme işlemi başlar.

## <a name="uploading-pkg-files"></a>Karşıya yükleniyor. PKG dosyaları

İçerik paketlerini yüklemek için en kolay yolu **Xcode arşiv aracı**. Seçin **ürün > Arşiv** menüsünde başlamak için:

 ![](changes-to-storekit-images/image13.png "Archiven seçin")

İçerik Paketi, ardından aşağıda gösterildiği gibi arşivde görünür. Arşiv türü ve simgesini göster bu bildirim bir **uygulama içi satın alma içerik arşiv**. Tıklatın **doğrula...** karşıya yükleme gerçekten preforming olmadan bizim içerik paketi hataları denetlemek için.

 [![](changes-to-storekit-images/image14.png "Paketi doğrulama")](changes-to-storekit-images/image14.png#lightbox)

Oturum açma, iTunes Connect kimlik bilgileri ile:

 [![](changes-to-storekit-images/image15.png "İTunes Connect kimlik bilgileri ile oturum açma")](changes-to-storekit-images/image15.png#lightbox)

Bu içerik ile ilişkilendirmek için doğru uygulama ve uygulama içi satın alma seçin:

 [![](changes-to-storekit-images/image16.png "Bu içerik ile ilişkilendirmek için doğru uygulama ve uygulama içi satın alma seçin")](changes-to-storekit-images/image16.png#lightbox)

Şuna benzer bir ileti görürsünüz:

 ![](changes-to-storekit-images/image17.png "Bir örnek sorunları ileti yok")

Şimdi benzer bir işlem yapması, ancak tıklatarak **Dağıt...** gerçekte içerik karşıya yükler.

 [![](changes-to-storekit-images/image18.png "Uygulamayı dağıtma")](changes-to-storekit-images/image18.png#lightbox)

İçerik yüklemek için ilk seçeneği seçin:

 ![](changes-to-storekit-images/image19.png "İçeriğin karşıya yükle")

Yeniden oturum açın:

 [![](changes-to-storekit-images/image15.png "Oturum açma")](changes-to-storekit-images/image15.png#lightbox)

Doğru uygulama ve uygulama içi satın alma kaydı içeriği yüklemek için seçin:

 [![](changes-to-storekit-images/image20.png "Uygulama ve uygulama içi satın alma kayıt seçin")](changes-to-storekit-images/image20.png#lightbox)

Dosyalarınız karşıya bekleyin:

 [![](changes-to-storekit-images/image21.png "İçerik Yükle iletişim kutusu")](changes-to-storekit-images/image21.png#lightbox)

Karşıya yükleme tamamlandığında, içeriği uygulama mağazasında gönderildi bildirmek için bir ileti görüntülenir.

 [![](changes-to-storekit-images/image22.png "Bir örnek başarılı karşıya yükleme iletisi")](changes-to-storekit-images/image22.png#lightbox)

Yaptıktan sonra döndüğünüzde ürün sayfasına iTunes Paket ayrıntılarını göster ve olması Connect üzerinde **gönderme hazır** durumu. Ürünün bu durumda olduğunda, korumalı alan ortamında test etme başlayabilirsiniz. 'Korumalı alanında test etmek için ürün göndermek ' gerekmez.

 [![](changes-to-storekit-images/image23.png "iTunes Paket ayrıntılarını göster ve gönderme durumu içinde hazır olması Bağlan")](changes-to-storekit-images/image23.png#lightbox)

Bunu (ör. biraz zaman alabilir birkaç dakika) arasında Arşiv ve güncelleştirilen Bağlan durum iTunes karşıya yükleme. Ürün gözden geçirme için ayrı ayrı gönderme veya bir uygulamaya birlikte gönderin. Yalnızca Apple içeriği resmi olarak onayladıktan sonra uygulamanızda satın App Store üretimde kullanılabilir olacaktır.

### <a name="pkg-file-format"></a>PKG dosya biçimi

Xcode ve Arşiv Aracı'nı kullanarak oluşturma ve barındırılan bir içerik paketi yükleme paketinin içeriğini hiçbir zaman göreceğiniz anlamına gelir. Dosyalar ve dizinler için örnek uygulama oluşturulan paketler şuna benzeyebilir, ile `plist` kök ve ürün dosyalarında dosyasında bir `Contents` alt:

 [![](changes-to-storekit-images/image24.png "Kök ve içeriği alt ürün dosyalarında plist dosyası")](changes-to-storekit-images/image24.png#lightbox)

Paket dizin yapısını unutmayın (özellikle dosyalarının konumu `Contents` alt) aygıttaki paket dosyalarını ayıklamak için bu bilgileri anlamak gerekir çünkü.

### <a name="updating-package-content"></a>Paket içeriği güncelleştirme

Bunu onaylandıktan sonra içeriği güncelleştirme yordamı:

-  Uygulama içi satın alma içerik projenin xcode'da düzenleyin.
-  Sürüm numarası kabartma.
-  İTunes Bağlan yeniden karşıya yükleyin. Sonraki satın alma en son sürümünü otomatik olarak alır ancak eski sürümünü zaten yüklemiş olan kullanıcılar herhangi bir bildirim almazsınız. 
-  Uygulamanızı kullanıcılara bildirme ve içerik daha yeni bir sürümü almak için görünümü teşvik eder sorumludur. Uygulamanın yeni sürümü indirir işlevi de yapılandırmanız gerekir. Bunun yapılması gerekir deposu Seti geri yükleme özelliğini kullanarak. 
-  Daha yeni bir sürüm, olup olmadığını belirlemek için bir özellik (ör. SKProducts getirmek için uygulamanıza oluşturabilirsiniz ürün fiyatı almak için kullanılan aynı işlemi) ve ContentVersion özelliği karşılaştırın. 

## <a name="purchasing-overview"></a>Genel Bakış satın alma

Bu bölümü okumadan önce mevcut gözden [uygulama içi satın alma belgeleri](~/ios/platform/in-app-purchasing/index.md).

Bir ürün barındırılan içerikle oluşur olayların sırası satın ve indirme bu şemada gösterilmiştir:

 [![](changes-to-storekit-images/image25.png "Bir ürün barındırılan içerikle oluşur olayların sırası satın alınır ve indirme")](changes-to-storekit-images/image25.png#lightbox)

1.  Yeni ürünler iTunes Connect barındırılan etkin içerik ile oluşturulabilir. Gerçek içeriği (olarak yalnızca olarak sürükleme dosyaları bir klasöre) Xcode'da ayrı olarak oluşturulur ve ardından arşivlenen ve (hiçbir kodlama gereklidir) iTunes karşıya. Her ürünün daha sonra satın almak için uygun hale onaya sonra gönderilir. Örnek kodda bu ürün kimlikleri sabit kodlanmış olan ancak içeriği Apple ile barındırma böylece yeni ürünler ve iTunes Bağlan içeriği gönderdiğinizde güncelleştirilmesi uzak bir sunucuda kullanılabilir ürün listesi depolarsanız daha esnektir. 
1.  Kullanıcı bir ürün satın aldığında, bir işlem işleme için ödeme sırasındaki yerleştirilir. 
1.  Mağaza Seti satın alma isteği işleme iTunes sunucularına iletir. 
1.  İşlem (ör. iTunes sunucularda tamamlandı Müşteri ücret) ve ürün bilgileri indirilebilir olup dahil bağlı olan uygulama için bir giriş döndürülür (ve bu durumda, dosya boyutu ve diğer meta veriler). 
1.  Kodunuzu ürün downloadble olup olmadığını denetleyin ve gerekiyorsa ödeme sıra üzerinde de yerleştirilen içerik indirme istekte gerekir. Mağaza Seti iTunes sunuculara bu isteği gönderir. 
1.  Sunucu yükleme ilerleme durumu ve kodunuza tahminleri kalan zaman döndürmek için bir geri çağırma sağlar deposu paketi içerik dosyası döndürür. 
1.  Tamamlandıktan sonra bildirim alın ve önbellek klasöründe bir dosya konumu geçirildi. 
1.  Kodunuzu dosya kopyalayabilirler ve bunları, ürünü satın aldığınız unutmayın gereken herhangi bir durum doğrulamanız gerekir. Yedekleme bayrağı yeni dosyalarda doğru bir şekilde ayarlamak için bu fırsatı değerlendirerek (İpucu: bir sunucudan gelen ve hiçbir kullanıcı tarafından düzenlenen, kullanıcının her zaman Apple'nın sunucularından gelecekte alabileceğini olduğundan, büyük olasılıkla bunları yedekleme, atlayın). 
1.  FinishTransaction çağırın. İşlem ödeme sırasından kaldırır gibi önemli budur. Önbellek dizini dışında içerik kopyaladıktan sonra FinishTransaction kadar çağırmayın önemlidir. Bir kez FinishTransaction çağırdığınızda, önbelleğe alınan dosyaları hızlı bir şekilde temizleneceği olasıdır. 


## <a name="implementing-hosted-content-purchase"></a>Barındırılan içerik satın alma uygulama

Aşağıdaki bilgileri tam ile birlikte okunmalıdır [uygulama içi satın almalara belgelerine](~/ios/platform/in-app-purchasing/index.md). Bu belgedeki bilgiler barındırılan içerik ve önceki uygulaması arasındaki farklar odaklanır.


### <a name="classes"></a>Sınıflar

Aşağıdaki sınıflar eklenen veya iOS 6 barındırılan destek içeriğini değiştirilemez:

-   **SKDownload** – devam eden bir indirme temsil eden yeni sınıf. Birden fazla başına ancak başlangıçta yalnızca bir uygulanan ürün için API sağlar. 
-   **SKProduct** – eklenen yeni özellikler: `Downloadable` , `ContentVersion` , `ContentLengths` dizi. 
-   **SKPaymentTransaction** – eklenen yeni özellik: `Downloads` , koleksiyonunu içerir `SKDownload` bu ürünün İçerik indirme için kullanılabilir barındırılan varsa nesneleri. 
-   **SKPaymentQueue** – eklenen yeni yöntemi: `StartDownloads` . Bu yöntem çağrısı `SKDownload` barındırılan içeriklerini getirilecek nesne. Yükleme arka planda ortaya çıkabilir. 
-   **SKPaymentTransactionObserver** – yeni bir yöntem: `UpdateDownloads` . Mağaza Seti geçerli indirme işlemleri hakkında ilerleme durumu bilgileri ile bu yöntemi çağırır. 


Yeni ayrıntılarını `SKDownload` sınıfı:

-   **İlerleme** – tamamlanma yüzdesi bir göstergesi kullanıcıya görüntülemek için kullanabileceğiniz 0-1 arasında bir değer. İlerleme kullanmayın indirme tam olup olmadığını saptamak, durumunu denetlemek için 1 == == tamamlandı. 
-   **TimeRemaining** – kalan karşıdan yükleme süresi, saniye cinsinden tahmin. -1 tahmin hala hesaplıyor anlamına gelir. 
-   **Durum** – etkin, Bekliyor, tamamlandı, başarısız, duraklatıldı, iptal edildi. 
-   **ContentURL** – dosya konumu burada içerik yerleştirme diskte, buna `Cache` dizin. Yükleme tamamlandıktan sonra yalnızca doldurulur. 
-   **Hata** – durumu başarısız olursa bu özelliğini denetleyin. 


Bu diyagramda (barındırılan içerik Harcamaları belirli kod yeşil renkte gösterilir) örnek kodda sınıfları arasındaki etkileşimler gösterilmektedir:

 [![](changes-to-storekit-images/image26.png "Barındırılan içerik satın alma işlemleri Bu diyagramda yeşil olarak gösterilir")](changes-to-storekit-images/image26.png#lightbox)

Burada bu sınıfların kullanılan örnek kod, bu bölümde geri kalanı gösterilir:

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

Varolan değiştirme `UpdatedTransactions` indirilebilir içerik ve çağrı denetlemek için geçersiz kılma `StartDownloads` gerekirse:

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
    foreach (SKPaymentTransaction transaction in transactions) {
        switch (transaction.TransactionState) {
        case SKPaymentTransactionState.Purchased:
            // UPDATED FOR iOS 6
            if (transaction.Downloads != null && transaction.Downloads.Length > 0) {
                // Purchase complete, and it has downloads... so download them!
                SKPaymentQueue.DefaultQueue.StartDownloads (transaction.Downloads);
                // CompleteTransaction() call has moved after downloads complete
            } else {
                // complete the transaction now
                theManager.CompleteTransaction(transaction);
            }
            break;
        case SKPaymentTransactionState.Failed:
            theManager.FailedTransaction(transaction);
            break;
        case SKPaymentTransactionState.Restored:
            // TODO: you must decide how to handle restored transactions.
            // Triggering all the downloads at once is not advisable.
            theManager.RestoreTransaction(transaction);
            break;
        default:
            break;
        }
    }
}
```

Yeni geçersiz kılınan yöntemi `UpdatedDownloads` aşağıda gösterilmiştir. Mağaza Seti sonra bu yöntemi çağırır `StartDownloads` içinde tetiklenen `UpdatedTransactions`. Bu yöntem çağrılır *birden çok kez* ile karşıdan ilerleme sağlamak için belirsiz aralıklarla ve daha sonra tekrar zaman karşıdan yüklemeyi bitirdi. Duyuru yöntemi kabul eden bir dizi `SKDownload` her yöntem çağrısı sırasındaki birden fazla indirme durumunu sağlayabilmesi nesneleri. Gösterildiği gibi her zaman ve uygun eylemi gerçekleştirilecek indirme durumlar uygulama aşağıdaki denetimi.

```csharp
// ENTIRELY NEW METHOD IN iOS6
public override void PaymentQueueUpdatedDownloads (SKPaymentQueue queue, SKDownload[] downloads)
{
    Console.WriteLine (" -- PaymentQueueUpdatedDownloads");
    foreach (SKDownload download in downloads) {
        switch (download.DownloadState) {
        case SKDownloadState.Active:
            // TODO: implement a notification to the UI (progress bar or something?)
            Console.WriteLine ("Download progress:" + download.Progress);
            Console.WriteLine ("Time remaining:   " + download.TimeRemaining); // -1 means 'still calculating'
            break;
        case SKDownloadState.Finished:
            Console.WriteLine ("Finished!!!!");
            Console.WriteLine ("Content URL:" + download.ContentUrl);

            // UNPACK HERE! Calls FinishTransaction when it's done
            theManager.SaveDownload (download);

            break;
        case SKDownloadState.Failed:
            Console.WriteLine ("Failed"); // TODO: UI?
            break;
        case SKDownloadState.Cancelled:
            Console.WriteLine ("Cancelled"); // TODO: UI?
            break;
        case SKDownloadState.Paused:
        case SKDownloadState.Waiting:
            break;
        default:
            break;
        }
    }
}
```

### <a name="inapppurchasemanager-skproductsrequestdelegate"></a>InAppPurchaseManager (SKProductsRequestDelegate)

Bu sınıf, yeni bir yöntem içerir `SaveDownload` her yükleme başarıyla tamamlandıktan sonra çağrılır.

Barındırılan içerik olduğundan başarıyla indirildi ve içine sıkıştırması açılmış `Cache` dizin. Yapısı. PKG dosyası gerektirir kaydedilmesi için tüm dosyaları bir `Contents` aşağıdaki kod dosyaları içinden ayıklar şekilde alt `Contents` alt dizin.

Kod içerik paketteki tüm dosyaların dolaşır ve bunlara kopyalar `Documents` adlı bir alt dizin `ProductIdentifier`. Son çağrıları `CompleteTransaction`, çağıran `FinishTransaction` ödeme sıradan işlem kaldırmak için.

```csharp
// ENTIRELY NEW METHOD IN iOS 6
public void SaveDownload (SKDownload download)
{
    var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
    var targetfolder = System.IO.Path.Combine (documentsPath, download.Transaction.Payment.ProductIdentifier);
    // targetfolder will be "/Documents/com.xamarin.storekitdoc.montouchimages/" or something like that
    if (!System.IO.Directory.Exists (targetfolder))
        System.IO.Directory.CreateDirectory (targetfolder);
    foreach (var file in System.IO.Directory.EnumerateFiles 
             (System.IO.Path.Combine(download.ContentUrl.Path, "Contents"))) { // Contents directory is the default in .PKG files
        var fileName = file.Substring (file.LastIndexOf ("/") + 1);
        var newFilePath = System.IO.Path.Combine(targetfolder, fileName);
        if (!System.IO.File.Exists(newFilePath)) // HACK: this won't support new versions...
            System.IO.File.Copy (file, newFilePath);
        else
            Console.WriteLine ("already exists " + newFilePath);
    }
    CompleteTransaction (download.Transaction); // so it gets 'finished'
}
```

Zaman `FinishTransaction` indirilen adlı dosyalar artık garanti edilir olduğu `Cache` dizin. Çağırmadan önce tüm dosyalarının kopyalanacağı `FinishTransaction`.


## <a name="other-considerations"></a>Diğer Konular

Yukarıdaki örnek kod, barındırılan içerik satın alma oldukça basit bir uygulama gösterir. Dikkate almanız gereken bazı ek noktaları vardır:

### <a name="detecting-updated-content"></a>Güncelleştirilmiş içerik algılama

Barındırılan içerik paketlerinizi güncelleştirmek mümkün olsa da, bu güncelleştirmeleri zaten indirmiş ve ürün satın alan kullanıcılara anında için herhangi bir mekanizma deposu Seti sağlamaz. Bu işlev uygulamak için kodunuzu yeni denetleyebilirsiniz `SKProduct.ContentVersion` özelliği (varsa `SKProduct` olan `Downloadable`) düzenli olarak ve değeri artırılır algılayabilir. Alternatif olarak bir anında iletme bildirimi sistem oluşturabilirsiniz.

### <a name="installing-updated-content-versions"></a>Güncelleştirilmiş içerik sürümler yükleme

Dosya zaten mevcutsa Yukarıdaki örnek kod dosya kopyalamayı atlar. Yüklenen içerik daha yeni sürümünü desteklemek istiyorsanız, iyi bir fikir değil.

İçerik sürümü adlı bir klasöre kopyalayın ve (ör. geçerli sürüm olduğu izlemek için bir alternatif olabilir içinde `NSUserDefaults` veya tamamlanmış satın alma kayıtlarını depolamak yerlerde).

### <a name="restoring-transactions"></a>Geri yükleme işlemleri

Zaman `SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` olduğu adlı kullanıcı için tüm önceki hareketleri deposu Seti döndürür. Her şeyi indirme için aynı anda kuyruğa gibi çok sayıda öğe aldıysanız veya her satın alma çok büyük içerik paketleri varsa, daha sonra geri yükleme çok ağ trafiği neden olabilir.

Bir ürün ilişkili içerik paket gerçek Merkezi'nden ayrı olarak satın alınan olup olmadığını izleme göz önünde bulundurun.

### <a name="pausing-restarting-and-canceling-downloads"></a>Duraklatma, yeniden başlatma ve iptal etme yükler

Örnek kod, bu özellik gösterilmemiştir rağmen duraklatma ve barındırılan içerik indirmeleri yeniden mümkündür. `SKPaymentQueue.DefaultQueue` Yöntemlerini sahip `PauseDownloads`, `ResumeDownloads` ve `CancelDownloads`.

Kod çağırırsa `FinishTransaction` öncesinde indirme ödeme sırasına `Finished` sonra bu yükleme otomatik olarak iptal edilir.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>İndirilen içerik Atla yedekleme bayrağı ayarlama

Apple'nın İcloud'a yedekleme yönergeleri önermek bir sunucudan kolayca geri kullanıcı olmayan içerik gerektiğini *değil* yedeklenen (gereksiz yere iCloud depolama alanı kullanacağınızı nedeniyle). Başvurmak [dosya sistemi ile çalışma](~/ios/app-fundamentals/file-system.md) yedekleme öznitelik ayarlama hakkında daha fazla ayrıntı için belgeleri.

## <a name="summary"></a>Özet

Bu makalede iOS6 deposu Seti'nin iki yeni özellikler anlatılmıştır: iTunes ve diğer içeriği, uygulamanızın içinde satın alma ve Apple'nın sunucu kendi uygulama içi satın almalara barındırmak için kullanma. Bu giriş varolan ile birlikte okunmalıdır [uygulama içi satın alma belgeleri](~/ios/platform/in-app-purchasing/index.md) deposu Seti işlevselliği uygulamanın tam kapsamı için.

## <a name="related-links"></a>İlgili bağlantılar

- [StoreKit (örnek)](https://developer.xamarin.com/samples/StoreKit/)
- [Uygulama İçi Satın Alma](~/ios/platform/in-app-purchasing/index.md)
- [StoreKit Framework başvurusu](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [SKStoreProductViewController sınıf başvurusu](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [iTunes arama API Başvurusu](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC Video: Mağaza Seti ürünlerle satış](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
