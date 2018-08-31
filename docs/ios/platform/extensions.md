---
title: iOS Xamarin.iOS uzantıları
description: Bu belge, pencere öğeleri gibi standart bağlamı bildirim Merkezi içinde iOS tarafından sunulan uzantıları açıklar. Bu, bir uzantı oluşturma ve üst uygulama ile iletişim nasıl ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 831625c88bb3c0f8403d3b330054050488956cb6
ms.sourcegitcommit: f9fb316344fc4dce2fdce0dde3c5f4c2e4a42d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43257902"
---
# <a name="ios-extensions-in-xamarinios"></a>Xamarin.iOS iOS uzantıları

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**İOS, uzantıları kriterlere göre [Xamarin University](https://university.xamarin.com/)**

Uzantılar, iOS 8 ' tanıtılan özelleştirilmiş `UIViewControllers` , sunulur standart bağlamları içinde iOS tarafından gibi olarak içinde **bildirim Merkezi**gerçekleştirmek için kullanıcı tarafından istenen özel klavye türleri özelleştirilmiş gibi Giriş veya diğer bağlamlarda uzantısı özel efekt filtreleri burada sağlayabilir bir fotoğraf düzenleme gibi.

Tüm uzantıları (64-bit birleşik API'ları kullanılarak yazılan her iki öğe ile) bir kapsayıcı uygulaması ile birlikte yüklenir ve bir konak uygulamasında belirli bir uzantı noktasından etkinleştirilir. Ve mevcut sistem işlevleri için eklerin olarak kullanılacak olduğundan, yüksek performanslı, basit ve güçlü olması gerekir. 

## <a name="extension-points"></a>Uzantı noktaları

|Tür|Açıklama|Uzantı noktası|Konak uygulama|
|--- |--- |--- |--- |
|Eylem|Özelleştirilmiş bir düzenleyici veya belirli bir medya türünün için Görüntüleyicisi|`com.apple.ui-services`|Tüm|
|Belge sağlayıcısı|Uygulamasının uzak belge deposu kullanmasına izin verir|`com.apple.fileprovider-ui`|Kullanarak uygulamaları bir [UIDocumentPickerViewController](https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/)|
|Klavye|Alternatif klavyeler|`com.apple.keyboard-service`|Tüm|
|Fotoğraf düzenleme|Fotoğraf düzenleme ve düzenleme|`com.apple.photo-editing`|Photos.App Düzenleyicisi|
|Paylaş|Veri Hizmetleri, vb. Mesajlaşma sosyal ağlarla paylaşır.|`com.apple.share-services`|Tüm|
|Bugün|Bugün ekran veya bildirim merkezi görünen "Pencere öğeleri"|`com.apple.widget-extensions`|Bugün ve bildirim Merkezi|

[Ek uzantı noktaları](~/ios/platform/introduction-to-ios10/index.md#app-extensions) iOS 10 eklendi.

## <a name="limitations"></a>Sınırlamalar

Uzantılara sahip bir dizi sınırlandırması bazıları olan tüm türleri için evrensel (örneği, uzantı hiçbir tür kamera ve mikrofon erişmek için), diğer tür uzantılarla belirli sınırlamalar bunların kullanımını (örneğin, özel klavye olabilir, ancak parolalar gibi güvenli veri girişi alanları için kullanılamaz). 

Evrensel sınırlamalar vardır:

- [Sistem durumu Seti](~/ios/platform/healthkit.md) ve [olay paketi UI](~/ios/platform/eventkit.md) çerçeveleri kullanılabilir değil
- Uzantıları kullanamaz [genişletilmiş arka plan modları](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- (Bunlar var olan medya dosyalarını erişebilir ancak) uzantıları cihazın kameralar veya mikrofon erişemiyor
- (Bunlar hava bırak aracılığıyla veri iletebilir rağmen) uzantıları hava bırak veri alınamıyor
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/) ve [UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/) kullanılabilir uzantıları kullanmalıdır değil; [UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- Çeşitli üyelerinin [Uıapplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/) kullanılamaz: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/), `UIApplication.OpenURL`, `UIApplication.BeginIgnoringInteractionEvents` ve `UIApplication.EndIgnoringInteractionEvents`
- iOS 16 MB bellek kullanım sınırı günümüzün uzantılarına zorlar.
- Varsayılan klavye uzantıları ağa erişiminiz yok. Bu (kısıtlama zorlanmaz simülatörde) cihazda hata ayıklama etkiler Xamarin.iOS çalışmak için hata ayıklama için ağ erişim gerektirdiğinden. Ağ erişimi isteği olası ayarlayarak `Requests Open Access` değeri için projenin Info.plist dosyasında `Yes`. Lütfen Apple'nın bakın [özel klavye Kılavuzu](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) klavye uzantısı sınırlamaları hakkında daha fazla bilgi.

Apple'nın tek sınırlamalar için bkz [uygulama uzantısı Programlama Kılavuzu](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/).

## <a name="distributing-installing-and-running-extensions"></a>Uzantıları'nı çalıştırarak dağıtma ve yükleme

Uzantılar, hangi sırayla gönderildi ve App Store dağıtılmış bir kapsayıcı uygulama içinde üzerinden dağıtılır. Bu noktada uygulama ile birlikte dağıtılan uzantısı yüklü ancak kullanıcının her uzantı açıkça etkinleştirmeniz gerekir. Farklı uzantı türleri farklı şekilde etkinleştirilir; birkaç gitmek gerektirmek **ayarları** uygulama ve bunları burada etkinleştirin. Olsa da başkalarının kullanımı bir fotoğraf gönderirken bir paylaşım uzantısı etkinleştirme gibi bir noktada etkinleştirilir. 

İçinde uzantı kullanılır (burada genişletme noktası kullanıcı karşılaştığında) uygulama olarak adlandırılır **konak uygulama**, uzantıyı yürütüldüğünde barındıran uygulama olduğundan. Uzantıyı yükler uygulama **kapsayıcı uygulaması**yüklendiğinde uzantıyı içeren uygulamanın olduğundan.  

Genellikle, kapsayıcı uygulamasını uzantısı açıklanır ve kullanıcı, etkinleştirme işleminde size yol gösterir.

## <a name="extension-lifecycle"></a>Uzantı yaşam döngüsü

Bir uzantı tek bir basit [Uıviewcontroller](https://developer.xamarin.com/api/type/UIKit.UIViewController/) veya kullanıcı arabiriminin birden fazla ekrana sunmak daha karmaşık uzantıları. Kullanıcı karşılaştığında bir _uzantı noktaları_ (ne zaman gibi bir görüntü paylaşımına), bunlar bu uzantı noktası için kayıtlı uzantıları aralarından seçim yapabileceğiniz olanağına sahip olacaksınız. 

Uygulamanızı birini seçerseniz, uzantılar, kullanıcının kendi `UIViewController` örneği oluşturulur ve normal görünüm denetleyicisi yaşam döngüsü başlayın. Ancak, olan askıya alındı, ancak kullanıcı normalde tamamlandığında değil genellikle sonlandırıldı, normal bir uygulama uzantıları yüklenen yürütülen ve art arda sonlandırıldı.

Uzantıları ana bilgisayar uygulamalarını ile iletişim kurabilir bir [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) nesne. Bazı uzantılar, zaman uyumsuz geri çağırmalar sonuçlarla alma işlemlerini sahip. Arka plan iş parçacıklarında bu geri aramaların yürütüleceği ve uzantısı bu dikkate almanız gerekir. kullanarak örneği için [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/) kullanıcı arabirimini güncelleştirmek istiyorsanız. Bkz: [ana bilgisayar uygulamasıyla iletişim kuran](#Communicating-with-the-Host-App) bölümünde daha fazla ayrıntı için.

Varsayılan olarak, uzantıları ve kapsayıcı uygulamalarını, birlikte yüklenen rağmen iletişim. Bazı durumlarda, kapsayıcı uygulamasını amacı uzantısı yüklendikten sonra sunulan aslında bir boş "dağıtımı" kapsayıcıdır. Ancak, koşulların gerektirdiği, kapsayıcı uygulamasını ve uzantısını ortak kaynaklardan paylaşabiliriz. Ayrıca, bir **bugün uzantısı** bir URL açmak için kapsayıcı uygulaması isteyebilir. Bu davranış gösterilen [evrim Geçiren geri sayım pencere öğesi](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo).

## <a name="creating-an-extension"></a>Bir uzantı oluşturma

Uzantılar (ve kapsayıcı uygulamalarını) 64 bit ikili dosyaları olmalıdır ve Xamarin.iOS kullanılarak oluşturulan [birleşik API](http://developer.xamarin.com/guides/cross-platform/macios/unified). Bir uzantı geliştirirken, çözümlerinizi en az iki proje içerir: kapsayıcı uygulamasını ve bir proje her uzantı kapsayıcı sağlar. 

### <a name="container-app-project-requirements"></a>Kapsayıcı uygulama projesi gereksinimleri

Uzantıyı yüklemek için kullanılan kapsayıcı uygulamasını aşağıdaki gereksinimlere sahiptir:

- Bu uzantı projesine bir başvuru sürdürmeniz gerekir.   
- Bütün bir uygulama (sağlayabilmelidir başlatın ve başarılı bir şekilde çalıştırılması) olması gerektiğini, hiçbir şey yapmaz birden fazla olsa bile bir uzantı yüklemek için bir yol sağlar. 
- Paket grubu tanımlayıcısı uzantısı'nın temelini bir paket grubu tanımlayıcısı olmalıdır (daha fazla ayrıntı için aşağıdaki bölüme bakın) projesi.

### <a name="extension-project-requirements"></a>Uzantı projesi gereksinimleri

Ayrıca, uzantı projesi aşağıdaki gereksinimlere sahiptir:

- Kendi kapsayıcı uygulamanın paket tanımlayıcısı ile başlayan bir paket grubu tanımlayıcısı olmalıdır. Örneğin, kapsayıcı uygulamanın bir paket grubu tanımlayıcısı varsa `com.myCompany.ContainerApp`, uzantının tanımlayıcısı olabilir `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- Anahtar tanımlamanız gerekir `NSExtensionPointIdentifier`, uygun bir değerle (gibi `com.apple.widget-extension` için bir **Bugün** bildirim merkezi pencere öğesi), kendi `Info.plist` dosya.
- Ayrıca tanımlamanız gerekir *ya da* `NSExtensionMainStoryboard` anahtarı veya `NSExtensionPrincipalClass` anahtarını kendi `Info.plist` dosyasını uygun bir değere sahip:
    - Kullanım `NSExtensionMainStoryboard` anahtar uzantısı için ana kullanıcı Arabirimi sunan görsel taslağın adını belirtmek için (eksi `.storyboard`). Örneğin, `Main` için `Main.storyboard` dosya.
    - Kullanım `NSExtensionPrincipalClass` uzantı başlatıldığında başlatılacak sınıf belirtmek için anahtar. Değer eşleşmelidir **kaydetme** değerini, `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

Belirli türde uzantısı ek gereksinimleri olabilir. Örneğin, bir **Bugün** veya **bildirim Merkezi** uzantının asıl sınıf uygulanmalı [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/).

> [!IMPORTANT]
> Mac için Visual Studio tarafından sağlanan uzantıları şablonları kullanarak projenize başlayın, bu gereksinimlerin çoğu (tüm varsa) sağlanan ve sizin için otomatik olarak şablon tarafından karşılanması.

## <a name="walkthrough"></a>İzlenecek yol 

Aşağıdaki kılavuzda bir örnek oluşturur **Bugün** gün ve yıl içinde kalan gün sayısı hesaplar pencere öğesi:

[![](extensions-images/carpediemscreenshot-sm.png "Gün ve yıl içinde kalan gün sayısını hesaplayan bir örnek bugün pencere öğesi")](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>Çözüm oluşturma

Gerekli çözüm oluşturmak için aşağıdakileri yapın:

1. İlk olarak, yeni bir iOS oluşturma **tek görünüm uygulaması** projesine **sonraki** düğmesi: 

    [![](extensions-images/today01.png "İlk olarak, yeni iOS, tek görünüm uygulama projesi oluşturma ve İleri düğmesine tıklayın")](extensions-images/today01.png#lightbox)
2. Projenin `TodayContainer` tıklatıp **sonraki** düğmesi: 

    [![](extensions-images/today02.png "' % S'proje TodayContainer çağırın ve İleri düğmesine tıklayın")](extensions-images/today02.png#lightbox)
3. Doğrulama **proje adı** ve **SolutionName** tıklatıp **Oluştur** çözümü oluşturmak için: 

    [![](extensions-images/today03.png "SolutionName ve proje adını doğrulayın ve çözüm oluşturmak için Oluştur düğmesine tıklayın")](extensions-images/today03.png#lightbox)
4. Ardından **Çözüm Gezgini**, çözüme sağ tıklayın ve yeni bir **iOS uzantısı** gelen proje **bugün uzantısı** şablonu: 

    [![](extensions-images/today04.png "Ardından, Çözüm Gezgini'nde çözüme sağ tıklayın ve bugün uzantısı şablondan yeni bir iOS uzantı projesi ekleme")](extensions-images/today04.png#lightbox)
5. Projenin `DaysRemaining` tıklatıp **sonraki** düğmesi: 

    [![](extensions-images/today05.png "' % S'proje DaysRemaining çağırın ve İleri düğmesine tıklayın")](extensions-images/today05.png#lightbox)
6. Projeyi gözden geçirin ve tıklayın **Oluştur** düğme oluşturmak için: 

    [![](extensions-images/today06.png "Projeyi gözden geçirin ve oluşturmak için Oluştur düğmesine tıklayın")](extensions-images/today06.png#lightbox)

Ortaya çıkan çözüm artık burada gösterildiği gibi iki proje sahip olmalıdır:

[![](extensions-images/today07.png "Burada gösterildiği gibi sonuçta ortaya çıkan çözüm şu anda iki proje olmalıdır")](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>Uzantı kullanıcı arabirimi oluşturma

Ardından, arabirimin tasarlamak gerekir, **Bugün** pencere öğesi. Bu, yapılabilir bir görsel taslak kullanarak ya da kod oluşturma kullanıcı Arabirimi ile. Her iki yöntem aşağıda ayrıntılı olarak ele alınacaktır.

#### <a name="using-storyboards"></a>Görsel Taslaklar kullanarak

Görsel taslak ile kullanıcı arabirimini derlemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, uzantı projenin çift `Main.storyboard` dosyayı düzenlemek için açın: 

    [![](extensions-images/today08.png "Düzenlemek üzere açmak için uzantı projeleri Main.storyboard dosyasına çift tıklayın")](extensions-images/today08.png#lightbox)
2. Kullanıcı Arabirimi için şablon tarafından otomatik olarak eklenmiş olan etiketi seçin ve verin **adı** `TodayMessage` içinde **pencere öğesi** sekmesinde **özellikleri Gezgini**: 

    [![](extensions-images/today09.png "Kullanıcı Arabirimi için şablon tarafından otomatik olarak eklenmiş olan etiketi seçin ve Özellikler Gezgini pencere öğesi sekmede adı TodayMessage verin")](extensions-images/today09.png#lightbox)
3. Görsel taslak için değişiklikleri kaydedin.

#### <a name="using-code"></a>Kod kullanarak

Kullanıcı arabirimini kodundaki derlemek için aşağıdakileri yapın: 

1. İçinde **Çözüm Gezgini**seçin **DaysRemaining** proje, yeni bir sınıf ekleyin ve onu çağırmak `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect DaysRemaining proje yeni bir sınıf ekleyin ve CodeBasedViewController çağırın")](extensions-images/code01.png#lightbox)
2. Yeniden **Çözüm Gezgini**, uzantının çift `Info.plist` dosyayı düzenlemek için açın: 

    [![](extensions-images/code02.png "Düzenlemek üzere açmak için uzantıları Info.plist dosyasına çift tıklayın")](extensions-images/code02.png#lightbox)
3. Seçin **kaynağı görünümü** (Alttan ekranın) ve açık `NSExtension` düğüm: 

    [![](extensions-images/code03.png "NSExtension düğümünü açın ve ekranın alt kaynak görünümü seçin")](extensions-images/code03.png#lightbox)
4. Kaldırma `NSExtensionMainStoryboard` ekleyin ve anahtar bir `NSExtensionPrincipalClass` değerle `CodeBasedViewController`: 

    [![](extensions-images/code04.png "NSExtensionMainStoryboard anahtarını kaldırın ve bir NSExtensionPrincipalClass CodeBasedViewController değeriyle ekleyin")](extensions-images/code04.png#lightbox)
5. Değişikliklerinizi kaydedin.

Ardından, Düzenle `CodeBasedViewController.cs` dosyasını açıp aşağıdaki gibi görünmesi:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewController")]
    public class CodeBasedViewController : UIViewController, INCWidgetProviding
    {
        public CodeBasedViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Add label to view
            var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
                TextAlignment = UITextAlignment.Center
            };

            View.AddSubview (TodayMessage);
            
            // Insert code to power extension here...

        }
    }
}
```

Unutmayın `[Register("CodeBasedViewController")]` için belirtilen değerle eşleşen `NSExtensionPrincipalClass` yukarıda.

### <a name="coding-the-extension"></a>Uzantı kodlama

Oluşturulan kullanıcı arabirimi ile açık `TodayViewController.cs` veya `CodeBasedViewController.cs` (yukarıdaki kullanıcı arabirimi oluşturmak için kullanılan yöntemine göre), dosya değişikliği **ViewDidLoad** yöntemi ve aşağıdaki gibi görünmesi:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Calculate the values
    var dayOfYear = DateTime.Now.DayOfYear;
    var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
    var daysRemaining = 365 + leapYearExtra - dayOfYear;

    // Display the message
    if (daysRemaining == 1) {
        TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
    } else {
        TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
    }
}
```

Kod kullanarak kullanıcı arabirimi yöntemi tabanlıysa değiştirin `// Insert code to power extension here...` yukarıdaki Yeni kod ile açıklama. Sonra taban uygulamasını çağırma (ve kod tabanlı sürümü için bir etiket ekleme), bu kod, yıl ve kaç gün kaldığını da almak için basit bir hesaplama gerçekleştirir. Etikette iletisini görüntüler (`TodayMessage`), kullanıcı Arabirimi tasarımı, oluşturduğunuz.

Bu işlem uygulama yazma normal işleme nasıl benzer olduğuna dikkat edin. Bir uzantının `UIViewController` uzantıları arka plan modları yoktur ve kullanıcı tamamlandığında askıya alınmaz dışındaki bir uygulamada bir görünüm denetleyicisi ile aynı yaşam döngüsünü sahip bunları kullanarak. Bunun yerine, uzantıları sürekli olarak başlatılır ve gerektiğinde edilemez.

### <a name="creating-the-container-app-user-interface"></a>Kapsayıcı uygulama kullanıcı arabirimi oluşturma

Bu kılavuz, kapsayıcı uygulamasını yalnızca bir yöntem olarak gönderin ve uzantıyı yüklemek için kullanılır ve kendi hiçbir işlevsellik sağlar. TodayContainer'ın Düzenle `Main.storyboard` dosya ve uzantının işlevi ve nasıl yükleneceği tanımlama metin ekleyin:

[![](extensions-images/today10.png "TodayContainers Main.storyboard dosyasını düzenleyin ve uzantıları işlevi ve nasıl yükleneceği tanımlama metin ekleyin")](extensions-images/today10.png#lightbox)

Görsel taslak için değişiklikleri kaydedin.

### <a name="testing-the-extension"></a>Uzantıyı test etme

İOS simülatörü uzantınızı test etmek için çalıştırma **TodayContainer** uygulama. Kapsayıcının ana görünüm görüntülenir:

[![](extensions-images/run01.png "Kapsayıcılar ana görünüm görüntülenir")](extensions-images/run01.png#lightbox)

Ardından, isabet **giriş** düğmesi simulator'da, açmak için ekranın üstünden aşağı kaydırma **bildirim Merkezi**seçin **Bugün** sekmesine ve tıklayın**Düzenle** düğmesi:

[![](extensions-images/run02.png "Simulator'da, bildirim merkezini açın, bugün sekmesini seçin ve Düzenle düğmesini tıklatın, ekranın üst kısmında aşağı çekin giriş düğmesine basın")](extensions-images/run02.png#lightbox)

Ekleme **DaysRemaining** uzantısı **Bugün** görüntülemek ve **Bitti** düğmesi:

[![](extensions-images/run03.png "Bugün görünümüne DaysRemaining uzantısını ekleyin ve bitti düğmesine tıklayın")](extensions-images/run03.png#lightbox)

Yeni pencere öğesi eklenecek **Bugün** görüntüleyin ve sonuçları görüntülenir:

[![](extensions-images/run04.png "Yeni pencere öğesi Bugün görünümüne eklenir ve sonuçları görüntülenir")](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>Ana bilgisayar uygulamasıyla iletişim kurma

Bugün uzantısı yukarıda oluşturduğunuz örnek kendi ana bilgisayar uygulamasıyla iletişim kurmaz ( **Bugün** ekran). Olsaydı, kullanacağınız [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) özelliği `TodayViewController` veya `CodeBasedViewController` sınıfları. 

Konak uygulamalarından veri alacak uzantıları için bir dizi biçiminde veridir [NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/) içinde depolanan nesneleri [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/) özelliği [ExtensionContext ](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) , uzantının `UIViewController`.

Fotoğraf düzenleme uzantıları gibi başka bir uzantı tamamlama veya iptal ediliyor kullanım kullanıcı arasında ayrım. Bu ana bilgisayar uygulaması aracılığıyla dön sinyal [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/) ve [CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/) yöntemlerinin [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) özelliği.

Daha fazla bilgi için lütfen Apple'nın bakın [uygulama uzantısı Programlama Kılavuzu](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1).

## <a name="communicating-with-the-parent-app"></a>Üst uygulama ile iletişim kurma

Bir uygulama grubu farklı uygulamalar (veya bir uygulama ve uzantılarını) paylaşılan dosya depolama konumuna erişim sağlar. Uygulama grupları gibi veriler için kullanılabilir:

- [Apple Watch ayarları](~/ios/watchos/app-fundamentals/settings.md).
- [NSUserDefaults paylaşılan](~/ios/app-fundamentals/user-defaults.md).
- [Paylaşılan dosyalar](~/ios/watchos/app-fundamentals/parent-app.md#files).

Daha fazla bilgi için lütfen bkz [uygulama grupları](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) bölümünü bizim **özellikleriyle çalışma** belgeleri.

## <a name="mobilecoreservices"></a>MobileCoreServices

Uzantıları ile çalışırken, oluşturma ve uygulama, diğer uygulamalar ve/veya hizmetler arasında alınıp verilen verileri işlemek için Tekdüzen bir tür tanımlayıcı (UTI) kullanın.

`MobileCoreServices.UTType` Statik sınıf tanımlar Apple ile ilgili aşağıdaki yardımcı özellikleri `kUTType...` tanımları:

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

Aşağıdaki örneğe bakın:

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

Daha fazla bilgi için lütfen bkz [uygulama grupları](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) bölümünü bizim **özellikleriyle çalışma** belgeleri.

## <a name="precautions-and-considerations"></a>Güvenlik önlemleri ve dikkat edilmesi gerekenler

Uygulamaları daha kullanabilecekleri önemli ölçüde daha az bellek uzantılarına sahiptir. Hızlı bir şekilde ve en düşük yetkisiz erişim için kullanıcı ve uygulama içinde barındırılan gerçekleştirmek beklenir. Ancak, bir uzantı uzantının Geliştirici tanımlamak kullanıcının olanak tanıyan markalı bir kullanıcı Arabirimi ile kullanan uygulama veya kapsayıcı uygulamasına ait oldukları ayırıcı, kullanışlı bir işlev de sağlamalıdır.

Sıkı bu gereksinimi göz önünde bulundurulduğunda, baştan sona test ve performans ve bellek tüketimi için en iyi duruma getirilmiş uzantıları yalnızca dağıtmanız gerekir. 

## <a name="summary"></a>Özet

Uzantılar nedir, türü uzantı noktaları ve uzantı üzerinde iOS tarafından dayatılan bilinen sınırlamalar, bu belgede ele. Bu, oluşturma, dağıtma, yükleme ve uzantılarını ve uzantı yaşam döngüsü çalıştıran açıklanmıştır. Basit bir oluşturmayla ilgili bir kılavuz sağlanan **Bugün** film şeritleri veya kod kullanarak bir pencere öğesinin kullanıcı Arabirimi oluşturmak için iki yol gösteren bir pencere öğesi. Bu, iOS Simulator uzantı test nasıl oluşturulacağını gösterir. Son olarak, bu kısa bir süreliğine konak uygulama ve bazı önlemler ve uzantı geliştirirken alınması gereken önemli noktalar ile iletişim kurulurken açıklanmıştır. 

## <a name="related-links"></a>İlgili bağlantılar

- [ContainerApp (örnek)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Uzantıları Xamarin.ios'ta (video) oluşturma](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
