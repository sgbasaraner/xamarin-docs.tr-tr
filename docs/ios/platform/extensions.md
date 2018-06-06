---
title: iOS Xamarin.iOS uzantıları
description: Bu belgede iOS gibi standart bağlamı içinde bildirim Merkezi tarafından sunulan pencere öğeleri olan uzantıları açıklanmaktadır. Uzantı oluşturmak ve üst uygulamadan ile iletişim nasıl açıklanır.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c26f951ddaff34cf23662f701395e636e1258b7d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786736"
---
# <a name="ios-extensions-in-xamarinios"></a>iOS Xamarin.iOS uzantıları

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**Uzantıları, iOS göre oluşturma [Xamarin Üniversitesi](https://university.xamarin.com/)**

Uzantılar, iOS 8 ' tanıtılan özelleştirilmiş `UIViewControllers` , sunulur standart bağlamları içinde iOS tarafından gibi olarak içinde **bildirim Merkezi**gerçekleştirmek için kullanıcı tarafından istenen özel klavye türleri özelleştirilmiş gibi Giriş veya diğer bağlamlarda uzantısı özel efekt filtreleri burada sağlayabilir bir fotoğraf düzenleme gibi.

Tüm uzantıları (64-bit birleşik API'leri kullanılarak yazılmış hem öğeleriyle) bir kapsayıcı uygulama ile birlikte yüklenir ve bir ana bilgisayar uygulamasında belirli bir uzantı noktasından etkinleştirilir. Ve eklerin varolan sistem işlevlere olarak kullanılacak olduğundan, yüksek performans, yalın ve sağlam olması gerekir. 

Bu makalede aşağıdaki konuları içerir:

- [Uzantı noktaları](#Extension-Points) -uzantı noktaları kullanılabilir türünü ve her noktası için oluşturulan uzantısı türünü listeler.
- [Sınırlamalar](#Limitations) -uzantılı bir dizi sınırlama, bazıları olan tüm türleri için evrensel diğer türleri uzantısı'nın kullanımı hakkında belirli sınırlamalar olabilir, ancak.
- [Dağıtma, yükleme ve çalışan Extensions](#Distributing-Installing-and-Running-Extensions) -uzantıları üzerinden dağıtılan hangi sırayla gönderilen ve uygulama mağazası dağıtılmış bir kapsayıcı uygulama içinde. Uygulamayla dağıtılmış uzantılarını bu noktada yüklenir, ancak kullanıcının her bir uzantı açıkça etkinleştirmelisiniz. Uzantı farklı türlerini farklı şekillerde etkinleştirilir.
- [Uzantı yaşam döngüsü](#Extension-Lifecycle) - uzantının bir `UIViewController` örneğinin oluşturulması ve normal View Controller yaşam döngüsü başlayın. Ancak, bir normal uygulama, uzantıları yüklenen, yürütülen ve art arda sonlandırıldı.
- [Bir uzantısı oluşturma](#Creating-an-Extension) -uzantı geliştirirken çözümlerinizi en az iki proje içerir: kapsayıcı uygulama bir proje ve her bir uzantı kapsayıcı sağlar.
- [İzlenecek yol](#Walkthrough) - basit bir oluşturma kapsar **Bugün** pencere öğesi film şeridi kullanarak kendi kullanıcı arabirimi sağlayan uzantısı veya uzantısı kodda, yükleme ve iOS Simulator'da test etme.
- [Ana bilgisayar uygulamayla iletişim](#Communicating-with-the-Host-App) -kısaca uzantı konak uygulamadan kurduğu açıklanır.
- [Üst uygulamayla iletişim](#Communicating-with-the-Parent-App) -kısaca uzantı üst uygulamadan kurduğu açıklanır.
- [Güvenlik önlemleri ve konuları](#Precautions-and-Considerations) -bazı bilmeniz önlemleri ve tasarlarken ve uzantı uygulama dikkate alınması gereken konuları listesi.
 

<a name="Extension-Points" />

## <a name="extension-points"></a>Uzantı noktaları

|Tür|Açıklama|Uzantı noktası|Ana bilgisayar uygulaması|
|--- |--- |--- |--- |
|Eylem|Özel düzenleyici veya belirli bir medya türünün Görüntüleyicisi|`com.apple.ui-services`|tüm|
|Belge sağlayıcısı|Bir uzak belge deposu kullanmak uygulamanın verir|`com.apple.fileprovider-ui`|Kullanarak uygulamaları bir [UIDocumentPickerViewController](https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/)|
|Klavye|Alternatif klavyeler|`com.apple.keyboard-service`|tüm|
|Fotoğraf düzenleme|Fotoğraf düzenleme ve düzenleme|`com.apple.photo-editing`|Photos.App Düzenleyicisi|
|Paylaş|Veri Hizmetleri, vb. Mesajlaşma sosyal ağlarla paylaşır.|`com.apple.share-services`|tüm|
|Bugün|Bugün ekranı ya da bildirim merkezi görünen "Pencere öğeleri"|`com.apple.widget-extensions`|Bugün ve bildirim Merkezi|

[Ek uzantı noktaları](~/ios/platform/introduction-to-ios10/index.md#app-extensions) iOS 10 eklendi.

<a name="Limitations" />

## <a name="limitations"></a>Sınırlamalar

Uzantılara sahip bir dizi sınırlama bazıları olan tüm türleri için evrensel (örneği, uzantının tür kameralar veya mikrofonlar erişebilir için), diğer tür uzantılarla kendi kullanımı (örneğin, özel klavyeler belirli sınırlamaları olabilir, ancak parolalar gibi güvenli veri girişi alanları için kullanılamaz). 

Evrensel sınırlamalar vardır:

- [Sistem durumu Seti](~/ios/platform/healthkit.md) ve [olay Seti UI](~/ios/platform/eventkit.md) çerçeveleri kullanılamaz
- Uzantıları kullanamaz [arka plan modları genişletilmiş](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- (Var olan medya dosyalarını eriştiklerinde ancak) uzantıları cihazın kameralar veya mikrofonlar erişemiyor
- (Bunların veri hava bırakma aracılığıyla iletebilir rağmen) uzantıları hava bırakma verileri alamıyor
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/) ve [UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/) kullanılabilir uzantıları kullanmalıdır değil; [UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- Birkaç üyeleri [Uıapplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/) kullanılamaz: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/), `UIApplication.OpenURL`, `UIApplication.BeginIgnoringInteractionEvents` ve `UIApplication.EndIgnoringInteractionEvents`
- iOS 16 MB bellek kullanım sınırına bugünün uzantıları hakkında zorlar.
- Varsayılan olarak, ağ erişimi klavye uzantınız yok. Bu aygıtta (kısıtlama benzeticisinde değil de zorlanır) hata ayıklama etkiler Xamarin.iOS çalışmak için hata ayıklama için ağ erişim gerektirdiğinden. Ağ erişim isteğinde bulunmak için olası ayarlayarak `Requests Open Access` projenin Info.plist değerinde `Yes`. Lütfen bkz. Apple'nın [özel klavye Kılavuzu](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) klavye uzantısı sınırlamalar hakkında daha fazla bilgi.

Apple'nın tek tek kısıtlamaları için lütfen bkz [uygulama uzantısı Programlama Kılavuzu](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/).

<a name="Distributing-Installing-and-Running-Extensions" />

## <a name="distributing-installing-and-running-extensions"></a>Dağıtma, yükleme ve Uzantıları'nı çalıştırarak

Uzantılar, hangi sırayla gönderilen ve uygulama mağazası dağıtılmış bir kapsayıcı uygulama içinde üzerinden dağıtılır. Uygulamayla dağıtılmış uzantılarını bu noktada yüklenir, ancak kullanıcının her bir uzantı açıkça etkinleştirmelisiniz. Uzantı farklı türlerini farklı şekillerde etkinleştirilir; birkaç gitmek kullanıcının gerektiren **ayarları** uygulama ve buradan etkinleştirin. Başkalarının kullanımı bir fotoğraf gönderirken bir paylaşımı uzantı etkinleştiriliyor gibi noktada etkin durumda değilken. 

İçinde uzantısı kullanılan (burada kullanıcı uzantı noktası karşılaştığında) uygulama olarak adlandırılır **ana uygulama**, uzantı yürütüldüğünde barındıran uygulama olduğundan. Uzantı yüklemelerinden uygulaması **kapsayıcı uygulama**, uzantı yüklendiğinde içeren bir uygulama olduğundan.  

Genellikle, kapsayıcı uygulama uzantısı açıklar ve kullanıcı bunu etkinleştirme işlemiyle anlatılmaktadır.

<a name="Extension-Lifecycle" />

## <a name="extension-lifecycle"></a>Uzantı yaşam döngüsü

Bir uzantısı tek bir basit olabilir [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) veya birden çok ekran UI sunmak daha karmaşık uzantıları. Kullanıcı karşılaştığında bir _uzantı noktaları_ (ne zaman gibi bir görüntü paylaşımı), o uzantı noktası için kaydedilen uzantılar aralarından seçim yapabileceğiniz olanağına sahip olacaksınız. 

Uygulamanızı birini seçerseniz uzantılar, kullanıcının kendi `UIViewController` örneğinin oluşturulması ve normal View Controller yaşam döngüsü başlayın. Ancak, farklı olarak, askıya alındı ancak kullanıcı bunları ile etkileşim tamamlandığında değil genellikle sonlandırıldı, normal bir uygulaması, uzantıları yüklenen, yürütülen ve art arda sonlandırıldı.

Uzantıları yoluyla kendi ana bilgisayar uygulamaları ile iletişim kurabilir bir [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) nesnesi. Bazı uzantılar sonuçları ile zaman uyumsuz geri aramalar alma işlemleri vardır. Bu geri aramalar arka plan iş parçacığı üzerinde yürütülür ve uzantı bunu göz önünde bulundurmalıdır; kullanarak örneği için [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/) kullanıcı arabirimini güncelleştirme yapmak istiyorsanız. Bkz: [konak uygulamayla iletişim](#Communicating-with-the-Host-App) daha fazla ayrıntı için bölüm aşağıda.

Varsayılan olarak, uzantıları ve kapsayıcı uygulamalarını, birlikte yüklenen rağmen iletişim kurabilir değil. Bazı durumlarda, kapsayıcı uygulama uzantısı yüklendikten sonra amacı sunulan aslında bir boş "Sevkiyat" kapsayıcıdır. Ancak, koşullar söylüyorsa, kapsayıcı uygulama ve uzantı ortak alanından kaynakları paylaşabilir. Ayrıca, bir **bugün uzantısı** bir URL açmak için kapsayıcı uygulama isteyebilir. Bu davranış gösterilen [gelişmesi geri sayım pencere öğesi](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo).

<a name="Creating-an-Extension" />

## <a name="creating-an-extension"></a>Bir uzantısı oluşturma

Uzantıları (ve kapsayıcı uygulamalarını) 64 bit ikili dosyaları olmalı ve Xamarin.iOS kullanılarak oluşturulan [Unified API](http://developer.xamarin.com/guides/cross-platform/macios/unified). Uzantı geliştirirken çözümlerinizi en az iki proje içerir: kapsayıcı uygulama bir proje ve her bir uzantı kapsayıcı sağlar. 

<a name="Container-App-Project-Requirements" />

### <a name="container-app-project-requirements"></a>Kapsayıcı uygulama projesi gereksinimleri

Uzantıyı yüklemek için kullanılan kapsayıcı uygulama aşağıdaki gereksinimlere sahiptir:

- Uzantı projesi başvuru bakımını yapmanız gerekir.   
- (Sağlayabilmelidir başlatın ve başarılı bir şekilde çalıştırılması) tam bir uygulama olmalıdır, hiçbir şey yapmaz birden fazla olsa bile bir uzantı yüklemeniz için bir yol sağlar. 
- Paket tanımlayıcısı uzantısı için temel bir paket tanımlayıcı olması gerekir (daha fazla ayrıntı için aşağıdaki bölüme bakın) proje.

<a name="Extension-Project-Requirements" />

### <a name="extension-project-requirements"></a>Uzantı projesi gereksinimleri

Ayrıca, uzantının proje aşağıdaki gereksinimlere sahiptir:

- Kendi kapsayıcı uygulamanın paket tanımlayıcısı ile başlayan bir paket tanımlayıcısı olmalıdır. Örneğin, kapsayıcı uygulamanın paket tanıtıcısı varsa `com.myCompany.ContainerApp`, uzantının tanımlayıcısı olmayabilir `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- Anahtar tanımlamalısınız `NSExtensionPointIdentifier`, uygun bir değerle (gibi `com.apple.widget-extension` için bir **Bugün** bildirim merkezi pencere öğesi), kendi `Info.plist` dosya.
- Ayrıca tanımlamalıdır *ya da* `NSExtensionMainStoryboard` anahtar veya `NSPrincipalClass` anahtarını kendi `Info.plist` dosyasını uygun bir değere sahip:
    - Kullanım `NSExtensionMainStoryboard` anahtar uzantısı için ana UI gösterir film şeridi adını belirtin (eksi `.storyboard`). Örneğin, `Main` için `Main.storyboard` dosya.
    - Kullanım `NSPrincipalClass` anahtar uzantısı başlatıldığında başlatılacak sınıf belirtin. Değerle eşleşmelidir **kaydetmek** değeri, `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

Belirli türlerdeki uzantıları ek gereksinimleri olabilir. Örneğin, bir **Bugün** veya **bildirim Merkezi** uzantının asıl sınıfı uygulanmalı [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/).

> [!IMPORTANT]
> Mac için Visual Studio tarafından sağlanan uzantıları şablonları kullanarak projenize başlatırsanız (tüm varsa) çoğu bu gereksinimleri sağlanan ve sizin için otomatik olarak şablon tarafından karşılanır.

<a name="Walkthrough" />

## <a name="walkthrough"></a>İzlenecek yol 

Aşağıdaki örnekte, bir örnek oluşturur **Bugün** gün ve yıl içinde kalan gün sayısını hesaplar pencere öğesi:

[![](extensions-images/carpediemscreenshot-sm.png "Gün ve yıl içinde kalan gün sayısını hesaplar bir örnek bugün pencere öğesi")](extensions-images/carpediemscreenshot.png#lightbox)

<a name="Creating-the-Solution" />

### <a name="creating-the-solution"></a>Çözüm oluşturma

Gerekli çözüm oluşturmak için aşağıdakileri yapın:

1. İlk olarak, yeni bir iOS oluşturma **Single View uygulaması** proje ve tıklatın **sonraki** düğmesi: 

    [![](extensions-images/today01.png "İlk olarak, yeni iOS, tek görünüm uygulaması projesi oluşturun ve İleri düğmesine tıklayın")](extensions-images/today01.png#lightbox)
2. Proje çağrısı `TodayContainer` tıklatıp **sonraki** düğmesi: 

    [![](extensions-images/today02.png "Proje TodayContainer çağırın ve İleri düğmesine tıklayın")](extensions-images/today02.png#lightbox)
3. Doğrulayın **proje adı** ve **SolutionName** tıklatıp **Oluştur** düğmesi çözümü oluşturun: 

    [![](extensions-images/today03.png "Proje adı ve SolutionName doğrulayın ve çözüm oluşturmak için Oluştur düğmesine tıklayın")](extensions-images/today03.png#lightbox)
4. İleri ' **Çözüm Gezgini**, çözüm üzerinde sağ tıklayın ve yeni bir ekleme **iOS uzantısı** gelen proje **bugün uzantısı** şablonu: 

    [![](extensions-images/today04.png "Ardından, Çözüm Gezgini'nde, çözüm üzerinde sağ tıklatın ve bugün uzantısı şablondan yeni bir iOS uzantı projesi ekleme")](extensions-images/today04.png#lightbox)
5. Proje çağrısı `DaysRemaining` tıklatıp **sonraki** düğmesi: 

    [![](extensions-images/today05.png "Proje DaysRemaining çağırın ve İleri düğmesine tıklayın")](extensions-images/today05.png#lightbox)
6. Projeyi gözden geçirin ve tıklatın **oluşturma** düğmesi oluşturmak için: 

    [![](extensions-images/today06.png "Projeyi gözden geçirmek ve oluşturmak için Oluştur düğmesine tıklayın")](extensions-images/today06.png#lightbox)

Sonuçta elde edilen çözüm şimdi aşağıda gösterildiği gibi iki proje sahip olmalıdır:

[![](extensions-images/today07.png "Aşağıda gösterildiği gibi sonuçta elde edilen çözüm şimdi iki proje olmalıdır")](extensions-images/today07.png#lightbox)

<a name="Creating-the-Extension-User-Interface" />

### <a name="creating-the-extension-user-interface"></a>Uzantısı kullanıcı arabirimi oluşturma

Ardından, arabirim için tasarlamak gerekir, **Bugün** pencere öğesi. Bu da yapılabilir bir film şeridi kullanarak veya kod oluşturma UI göre. Her iki yöntem aşağıda ayrıntılı olarak ele alınacaktır.

<a name="Using-Storyboards" />

#### <a name="using-storyboards"></a>Film şeritleri kullanma

Film şeridi ile kullanıcı arabirimini oluşturmak için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, uzantı projenin çift `Main.storyboard` dosyayı düzenlemek için açın: 

    [![](extensions-images/today08.png "Düzenlemek üzere açmak için uzantı projeleri Main.storyboard dosyasını çift tıklatın")](extensions-images/today08.png#lightbox)
2. UI için şablon tarafından otomatik olarak eklenen etiketi seçin ve bu verin **adı** `TodayMessage` içinde **pencere öğesi** sekmesinde **özellikleri Explorer**: 

    [![](extensions-images/today09.png "UI için şablon tarafından otomatik olarak eklenen etiketi seçin ve Özellikler Explorer pencere öğesi sekmesinde adı TodayMessage verin")](extensions-images/today09.png#lightbox)
3. Film şeridi için değişiklikleri kaydedin.

<a name="Using-Code" />

#### <a name="using-code"></a>Kod kullanarak

Kullanıcı Arabiriminde kodu oluşturmak için aşağıdakileri yapın: 

1. İçinde **Çözüm Gezgini**seçin **DaysRemaining** proje, yeni bir sınıf ekleyin ve onu çağrısı `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect DaysRemaining proje yeni bir sınıf ekleyin ve CodeBasedViewController çağırın")](extensions-images/code01.png#lightbox)
2. Yeniden, **Çözüm Gezgini**, uzantının çift `Info.plist` dosyayı düzenlemek için açın: 

    [![](extensions-images/code02.png "Uzantıları Info.plist dosyasını düzenlemek üzere açmak için çift tıklayın")](extensions-images/code02.png#lightbox)
3. Seçin **kaynağı görünümü** (ekran Alttan) ve açık `NSExtension` düğümü: 

    [![](extensions-images/code03.png "Ekranın alt kısmından kaynağı görünümünü seçin ve NSExtension düğümünü açın")](extensions-images/code03.png#lightbox)
4. Kaldırma `NSExtensionMainStoryboard` anahtarı ve ekleme bir `NSPrincipalClass` değerle `CodeBasedViewController`: 

    [![](extensions-images/code04.png "NSExtensionMainStoryboard anahtarı kaldırın ve NSPrincipalClass CodeBasedViewController değeriyle ekleyin")](extensions-images/code04.png#lightbox)
5. Değişikliklerinizi kaydedin.

Ardından, düzenleme `CodeBasedViewController.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewContoller")]
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

Unutmayın `[Register("CodeBasedViewContoller")]` için belirtilen değerle eşleşen `NSPrincipalClass` üstünde.

<a name="Coding-the-Extension" />

### <a name="coding-the-extension"></a>Uzantı kodlama

Oluşturulan kullanıcı arabirimi ile ya da açmak `TodayViewController.cs` veya `CodeBasedViewController.cs` (yukarıdaki kullanıcı arabirimi oluşturmak için kullanılan yöntemi tabanlı), dosya değişikliği **ViewDidLoad** yöntemi ve aşağıdaki gibi görünmesi:

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

Kod kullanan kullanıcı arabirimi yöntemine temel değiştirirseniz `// Insert code to power extension here...` üstten yeni kod açıklaması. Temel uygulama çağırmak (ve sonra temel kod sürümü için bir etiket ekleme), bu kod yıl ve kaç gün kalan gün almak için basit bir hesaplama gerçekleştirir. İleti etiketinde görüntüler (`TodayMessage`) UI tasarımında oluşturduğunuz.

Bu işlem bir uygulama yazma normal işlem nasıl benzer olduğunu unutmayın. Bir uzantının `UIViewController` uzantıları arka plan modları yoksa ve kullanıcı tamamlandığında askıya değil dışında bir uygulamada bir görünüm denetleyicisi aynı yaşam döngüsü sahip bunları kullanarak. Bunun yerine, uzantıları sürekli olarak başlatıldı ve gerektiği gibi XML'deki ayrılmış.

<a name="Creating-the-Container-App-User-Interface" />

### <a name="creating-the-container-app-user-interface"></a>Kapsayıcı uygulama kullanıcı arabirimi oluşturma

Bu kılavuz kapsayıcı uygulama yalnızca sevk ve uzantıyı yüklemek için bir yöntem olarak kullanılır ve kendi hiçbir işlevsellik sağlar. TodayContainer's Düzenle `Main.storyboard` dosya ve uzantının işlevi ve nasıl yükleneceği tanımlama biraz metin ekleyin:

[![](extensions-images/today10.png "TodayContainers Main.storyboard dosyasını düzenleyin ve uzantıları işlevi ve nasıl yükleneceği tanımlama bazı metin ekleme")](extensions-images/today10.png#lightbox)

Film şeridi için değişiklikleri kaydedin.

<a name="Testing-the-Extension" />

### <a name="testing-the-extension"></a>Testi uzantısı

İOS Simulator'da uzantınızı test etmek için çalıştırın **TodayContainer** uygulama. Kapsayıcının ana görünüm görüntülenir:

[![](extensions-images/run01.png "Ana görünüm görüntülenir kapsayıcıları")](extensions-images/run01.png#lightbox)

Ardından, isabet **giriş** açmak için ekranın üstünde aşağı sağdan Simulator düğmesini **bildirim Merkezi**seçin **Bugün** sekmesine ve tıklayın**Düzenle** düğmesi:

[![](extensions-images/run02.png "Ekranın üst kısmındaki bildirim merkezi açın, bugün sekmesini seçin ve Düzenle düğmesini tıklatın aşağı sağdan Simulator giriş düğmesini tıklatın")](extensions-images/run02.png#lightbox)

Ekleme **DaysRemaining** uzantısı **Bugün** görüntülemek ve **Bitti** düğmesi:

[![](extensions-images/run03.png "Bugün görünümüne DaysRemaining uzantısı ekleyin ve Bitti düğmesini tıklatın")](extensions-images/run03.png#lightbox)

Yeni pencere öğesi eklenecek **Bugün** Görünüm ve sonuçları görüntülenir:

[![](extensions-images/run04.png "Yeni pencere öğesi Bugün görünümüne eklenir ve sonuçları görüntülenir")](extensions-images/run04.png#lightbox)

<a name="Communicating-with-the-Host-App" />

## <a name="communicating-with-the-host-app"></a>Ana bilgisayar uygulamayla iletişim kurma

Bugün yukarıda oluşturduğunuz uzantısı örnek kendi ana bilgisayar uygulamasıyla iletişim değil ( **Bugün** ekran). Kaldırdıysanız, kullanacağınız [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) özelliği `TodayViewController` veya `CodeBasedViewController` sınıfları. 

Kendi ana bilgisayar uygulamalardan veri almasına uzantıları için verileri bir dizi biçimindedir [NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/) içinde depolanan nesneleri [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/) özelliği [ExtensionContext ](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) , uzantının `UIViewController`.

Uzantılar, fotoğraf düzenleme gibi başka bir uzantı tamamlayarak veya kullanım iptal kullanıcı arasında ayrım. Bu ana bilgisayar uygulaması aracılığıyla dön işaret [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/) ve [CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/) yöntemlerinin [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) özelliği.

Daha fazla bilgi için lütfen Apple'nın bkz [uygulama uzantısı Programlama Kılavuzu](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1).

<a name="Communicating-with-the-Parent-App" />

## <a name="communicating-with-the-parent-app"></a>Üst uygulamayla iletişim kurma

Bir uygulama grubu farklı uygulamaları (veya bir uygulama ve uzantılarını) paylaşılan dosya depolama konumuna erişim sağlar. Uygulama grupları gibi verileri için kullanılabilir:

- [Apple Watch ayarları](~/ios/watchos/app-fundamentals/settings.md).
- [NSUserDefaults paylaşılan](~/ios/app-fundamentals/user-defaults.md).
- [Paylaşılan dosyalara](~/ios/watchos/app-fundamentals/parent-app.md#files).

Daha fazla bilgi için lütfen bkz [uygulama grupları](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) bölümünü bizim **özellikleriyle çalışma** belgeleri.

<a name="MobileCoreServices" />

## <a name="mobilecoreservices"></a>MobileCoreServices

Uzantıları ile çalışırken, oluşturma ve uygulama, diğer uygulamalar ve/veya hizmetleri arasında alınıp verileri işlemek için Tekdüzen türü tanımlayıcısı (UTI) kullanın.

`MobileCoreServices.UTType` Statik sınıf tanımlar, Apple için ilgili aşağıdaki yardımcı Özellikler `kUTType...` tanımları:

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


<a name="Precautions-and-Considerations" />

## <a name="precautions-and-considerations"></a>Güvenlik önlemleri ve dikkat edilmesi gerekenler

Uzantıları uygulamaları göründüklerinden kullanabilecekleri önemli ölçüde daha az bellek yok. Hızlı bir şekilde ve kullanıcı ve içinde barındırılan uygulama için en az yetkisiz erişim ile gerçekleştirmek beklenir. Ancak, bir uzantı uzantının Geliştirici tanımlamak kullanıcı izin markalı bir kullanıcı Arabirimi ile kullanıcı uygulaması veya ait kapsayıcı uygulama ayırt edici, yararlı işleve sağlamalıdır.

Bu sıkı gereksinim verildiğinde, baştan sona test ve performans ve bellek tüketimi için en iyi duruma getirilmiş uzantıları yalnızca dağıtmanız gerekir. 

<a name="Summary" />

## <a name="summary"></a>Özet

Bu belgenin ne olduğu, uzantı noktaları ve bir uzantısı'iOS tarafından dayatılan bilinen sınırlamalar tür uzantılar, kapsamdaki. Oluşturma, dağıtma, yükleme ve uzantılar ve uzantı yaşam döngüsü çalıştıran açıklanır. Basit bir oluşturma bir kılavuz sağlanan **Bugün** film şeritleri ya da kod kullanarak pencere öğesi'nin UI oluşturmanın iki yolu gösteren pencere öğesi. Uzantı iOS Simulator'da test etme gösterdi. Son olarak, bu kısaca konak uygulama ve birkaç önlemleri ve uzantı geliştirirken alınması gereken noktalar ile iletişim açıklanmıştır. 


## <a name="related-links"></a>İlgili bağlantılar

- [ContainerApp (örnek)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Xamarin.iOS içinde Uzantıları (video) oluşturma](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
