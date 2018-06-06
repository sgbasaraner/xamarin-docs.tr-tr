---
title: Ek iOS 9 çerçeveleri değişiklikleri
description: Bu belgede iOS 9 sunulan ek framework değişiklikler açıklanmaktadır. AVFoundation, AVKit ve CloudKit açıklanır.
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 15c9364cf3bdcb8c797882cc9ac76219959de439
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787714"
---
# <a name="additional-ios-9-frameworks-changes"></a>Ek iOS 9 çerçeveleri değişiklikleri

_Bu makalede, ek, küçük değişiklikler veya iOS 9 varolan çerçeveyi geliştirmeler kapsar._

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 logosu")](additional-framework-changes-images/ios9.png#lightbox)

İOS için önemli değişiklikler yanı sıra Apple iOS 9 değişikliklerini ve birkaç varolan çerçeveleri geliştirmeleri yaptı.

## <a name="avfoundation-framework-additions"></a>AVFoundation Framework eklemeler

AVFoundation Framework [AVSpeechSynthesisVoice](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechSynthesisVoice/) sınıfı şimdi bir sesli dil yanı sıra tanımlayıcısıyla belirtmenize olanak verir.

Örneğin, aşağıdaki kod, tüm kullanılabilir seslerini listesini alır:

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

Ardından sesi listeden birini olarak ayarlayarak kullanabileceğiniz `Voice` özelliği olan bir kopyasının [AVSpeachUtterance](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechUtterance/) sınıfı.

[AVQueuePlayer](https://developer.xamarin.com/api/type/AVFoundation.AVQueuePlayer/) sınıfı kuyrukta şimdi Internet akış ve dosya tabanlı media bir karışımını destekler. Önceki sürümler yalnızca sırası medya aynı türde olabilir.

Daha fazla bilgi için lütfen Apple'nın bkz [AVSpeechSynthesisVoice başvuru](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice).

## <a name="avkit-framework-additions"></a>AVKit Framework eklemeler

Yeni resim içinde resim (PIP) özelliği ile çalışmak için AVKit framework yeni içerir `AVPictureInPictureController` ve [AVPlayerViewController](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) sınıfları:

- **AVPictureInPictureController** -iOS 9 uygulama iPad bir kayan, yeniden boyutlandırılabilir PIP penceresinde bir video oynatma başlatmayı kullanıcı yanıt vermesi için bu sınıf sağlar.
- **AVPlayerViewController** -yöneten bir `AVPlayer` iPad bir kayan, yeniden boyutlandırılabilir PIP penceresinde bir video sunmak için kullanılan denetleyici.

Daha fazla bilgi için lütfen bkz bizim [iPad için çoklu](~/ios/platform/introduction-to-ios9/index.md#multitasking) belgeleri ve Apple'nın [AVPictureInPictureController başvuru](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) ve [AVPlayerViewController başvuru](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>CloudKit Web hizmetlerine giriş

CloudKit framework bu erişim iCloud uygulamaların geliştirilmesini kolaylaştırır. Bu, uygulama verilerini ve varlık hakları yanı sıra uygulama bilgilerini güvenli bir şekilde erişebildiklerinden alınmasını içerir. Bu paketi kişisel bilgi paylaşımı olmadan kullanıcıların iCloud kimlikleri ile uygulamalara erişim sağlayarak kullanıcılara anonim bir katman sağlar.

Yeni _CloudKit Web Hizmetleri_ framework tabanlı CloudKit verilere ve Xamarin.iOS uygulamanızı içeriğine erişim sağlamak için Web sitenizin birleştirilmiş bir JavaScript kitaplığı (CloudKit JS) sağlar.

> [!IMPORTANT]
> Erişebilir, sunmak veya içeriği CloudKit JS kullanarak CloudKit veritabanından güncelleştirme önce daha önce bu veritabanının şema tanımlanmış gerekir.




Daha fazla bilgi için lütfen aşağıdaki belgelere bakın:

- [CloudKit giriş](~/ios/data-cloud/intro-to-cloudkit.md) -CloudKit bir Xamarin.iOS uygulaması kullanarak bizim giriş.
- [CloudKit Hızlı Başlangıç](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -CloudKit Apple'nın Giriş.
- [CloudKit JS başvuru](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -Apple CloudKit JS belgeleri.
- [CloudKit Web Hizmetleri başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -CloudKit HTTP arabirimine açıklar Apple'nın başvuru.
- [CloudKit Kataloğu: Giriş CloudKit (Cocoa ve JavaScript)](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -CloudKit ve CloudKit JS kullanarak Apple'nın örnek uygulaması.

> [!IMPORTANT]
> Apple [araçlar sağlar](https://developer.apple.com/support/allowing-users-to-manage-data/) Avrupa Birliği'nın genel veri koruma düzenleme (GDPR) düzgün bir şekilde işlemek geliştiricilere yardımcı olmak için.

## <a name="foundation-framework-additions"></a>Foundation Framework eklemeler

Apple iOS 9 Foundation altyapısına yapılan aşağıdaki değişiklikler dahil:

### <a name="changes-to-nsbundle"></a>NSBundle yapılan değişiklikler

Aşağıdaki değişiklikler yapılmıştır [NSBundle](https://developer.xamarin.com/api/type/Foundation.NSBundle/) sınıfı iOS 9 için:

* `GetPreservationPriorityForTag (NSString tag)` -Belirtilen etiketle kaynaklar için geçerli koruma önceliği alır. Geçerli değerler aralığında `0.0` için `1.0`, en düşük öncelik kaynaklarla ilk temizlenir.
* `SetPreservationPriorityForTag (double priority, NSSet tags)` -Belirtilen etiketlere sahip kaynaklar için geçerli koruma önceliği ayarlar. Geçerli değerler aralığında `0.0` için `1.0`, en düşük öncelik kaynaklarla ilk temizlenir.

Daha fazla bilgi için lütfen Apple'nın bkz [NSBundle başvuru](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle).

### <a name="changes-to-nsprocessinfo"></a>NSProcessInfo yapılan değişiklikler

Tek bir iOS cihazında çalışan her işlem sahip _işlem bilgileri Aracısı_ (PIA). Kullanım [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) sınıf geçerli PIA ve denetim güç ve belirli bir işlemi için ısı yönetimi hakkında bilgi sağlar.

Örneğin, bir işlemin otomatik sonlandırma denetlemek için aşağıdaki kodu kullanabilirsiniz:

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

Daha fazla bilgi için lütfen Apple'nın bkz [NSProcessInfo başvuru](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo).

### <a name="reacting-to-low-power-mode"></a>Düşük güç modu tepki

Kullanım `LowPowerModeEnabled` özelliği [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) düşük güç modu uygulamayı çalıştıran iOS cihazında etkinleştirilmiş olup olmadığını belirlemek için sınıf. Örneğin:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit Framework değişiklikleri

Apple yapılan aşağıdaki değişiklikler dahil [HealthKit](https://developer.xamarin.com/api/namespace/HealthKit/) iOS 9 framework:

- Toplu silme ve HealthKit veritabanındaki girdileri silme izlenmesini desteği. Apple'nın bkz [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) ve [HKHealthStore sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) daha fazla bilgi için.
- Yeni izleme kategoriler ve Özellikler eklenmiştir `HKQuantityTypeIdentifier` sınıfı (gibi `UVExposure`) ve `HKCategoryTypeIdentifier` sınıfı (gibi `OvulationTestResult`). Apple'nın bkz [HealthKit sabitleri başvuru](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) daha fazla bilgi için.

Lütfen bakın bizim [HealthKit giriş](~/ios/platform/healthkit.md) Xamarin.iOS HealthKit ile çalışma hakkında daha fazla bilgi için.

## <a name="local-authentication-framework-changes"></a>Yerel kimlik doğrulaması Framework değişiklikleri

Apple yapılan aşağıdaki değişiklikler dahil [yerel kimlik doğrulaması](https://developer.xamarin.com/api/namespace/LocalAuthentication/) iOS 9 framework:

- Kullanarak `EvaluateAccessControl` ve `EvaluatePolicy` yöntemlerinin [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) sınıfı, şunları yapabilirsiniz Touch ID eşleşen önceki başarılı kilidini gelen yeniden deneme artık.
- Şu anda kayıtlı parmakları listesini almak için yeteneği.
- Bir parmak eklendiğinde veya kimlik doğrulamasını kaldırıldığında izleme desteği.
- Kullanma yeteneğini _kimlik doğrulaması bağlamı_ Anahtarlık çağrıları ve Anahtarlık erişim denetimini değerlendirmek için destek listeler.
- Kullanıcı istemi kodundan iptal etme yeteneği.

Lütfen bakın bizim [Touch ID giriş](~/ios/platform/touchid.md) Xamarin.iOS dokunma kimliği ile çalışma hakkında daha fazla bilgi için.

### <a name="lacontext-changes"></a>LAContext değişiklikleri

Aşağıdaki değişiklikler yapılmıştır [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) sınıfı iOS 9 için:

- **TouchIdAuthenticationMaximumAllowableReuseDuration** - Returns the maximum amount of time that a touch ID authentication can be reused.
- **EvaluatedPolicyDomainState** - alır veya hesaplanan bir ilke durumunu ayarlar.
- **MaxBiometryFailures** -iOS 9 amorti.
- **TouchIdAuthenticationAllowableReuseDuration** alır veya touch ID kimlik doğrulama yeniden kullanılabilir süre miktarını ayarlar.
- **EvaluateAccessControl** - zaman uyumsuz olarak bir kimlik doğrulama ilkesini değerlendirir.
- **Geçersiz** -verilen touch ID kimlik doğrulama geçersiz kılar.
- **IsCredentialSet** -döndürür `true` kimlik bilgileri şu anda ayarlarsanız.
- **SetCredentialType** verilen kimlik bilgisi türünü ayarlar.

Lütfen bkz. Apple'nın [LAContext başvuru](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) daha fazla ayrıntı için.

## <a name="mapkit-framework-changes"></a>MapKit Framework değişiklikleri

Apple yapılan aşağıdaki değişiklikler dahil [MapKit](https://developer.xamarin.com/api/namespace/MapKit/) iOS 9 framework:

- MapKit doğrudan geçiş yönergeleri harita uygulamasını başlatarak ve geçiş tahmini zaman varış (ETA) kullanarak sorgulamak için destek şimdi sağlar [MKLaunchOptions](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) ve [MKDirections](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) sınıfları.
- MapKit tarafından döndürülen arama sonuçlarını ve [CLGeocoder](https://developer.xamarin.com/api/type/CoreLocation.CLGeocoder/) sınıfı sonucunun saat dilimi de sağlayabilir.
- Şimdi harita ek açıklamalar, iOS uygulamasını kullanarak sunulan tam olarak özelleştirebilirsiniz `DetailCalloutAccessoryView` özelliği [MKAnnotationView](https://developer.xamarin.com/api/type/MapKit.MKAnnotationView/) sınıfı.

Lütfen bakın bizim [iOS eşlemeleri](~/ios/user-interface/controls/ios-maps/index.md) ve [ek açıklamalar ve MapKit içindeki yer paylaşımları keşfetme izlenecek -](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) eşlemeleri ve Xamarin.iOS ve Apple'nın ekaçıklamalarileçalışmahakkındadahafazlabilgiiçin[CLGeocoder başvuru](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) daha fazla bilgi için.

## <a name="passkit-framework-additions"></a>PassKit Framework eklemeler

Apple yapılan aşağıdaki değişiklikler dahil [PassKit](https://developer.xamarin.com/api/namespace/PassKit/) iOS 9 framework:

- Apple Pay artık deposu borç ve kredi kartı birlikte bulma kartları destekler. Bkz: **ödeme ağları** Apple'nın bölümünü [PKPaymentRequest sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) daha fazla bilgi için.
- Gelen doğrudan bir Xamarin.iOS uygulaması içinde artık ödeme ağlar ve kart verenler Apple Pay ekleyebilirsiniz. Apple'nın bkz [PKAddPaymentPassViewController sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) daha fazla ayrıntı için.

Lütfen bakın bizim [PassKit giriş](~/ios/platform/passkit.md) Xamarin.iOS PassKit ile çalışma hakkında daha fazla bilgi için.

## <a name="safari-services-framework-additions"></a>Safari Framework eklemeleri Hizmetleri

Apple yapılan aşağıdaki değişiklikler dahil [Safari Hizmetleri](https://developer.xamarin.com/api/namespace/SafariServices/) iOS 9 framework:

- Şimdi yeni kullanabilirsiniz [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) bir Xamarin.iOS uygulaması içinde web içeriğini görüntülemek için sınıf. Web sitesi verileri ve tanımlama bilgileri Safari uygulamayla paylaşma olanağı sağlar ve birkaç (örneğin, okuyucu ve otomatik doldurmaya) Safari'nin özellikleri içerir. [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) özellikleri bir **Bitti** web içeriği görüntüledikten sonra kullanıcılar uygulamanıza döndürür düğmesi.

Çünkü [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) sınıfı özel olarak hazırlanmış web içeriği tek bir sayfayı görüntülemek için herhangi bir değiştirmek için kullanmayı düşünmelisiniz [WKWebKit](https://developer.xamarin.com/api/type/WebKit.WKWebView/) veya [UIWebView](https://developer.xamarin.com/api/type/UIKit.UIWebView/)mevcut Xamarin.iOS uygulamalarınızı içinde denetimleri.

### <a name="displaying-a-website"></a>Bir Web sitesi görüntüleme

Aşağıdaki kod, arama örneğidir bir [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) alanından başka bir görünüm denetleyicisi içinde:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>Uıkit Framework değişiklikleri

Apple, çeşitli öğeleri için birçok iyileştirme eklendi [Uıkit](https://developer.xamarin.com/api/namespace/UIKit/) iOS 9 için çerçevesi. Aşağıdaki bölümlerde bu değişiklikleri ayrıntılarını kaydeder.

### <a name="3d-touch-events"></a>3B dokunma olayları

İOS 9 iPhone 6s ve iPhone 6s yeni 3B dokunma Ayrıca, iOS uygulamalarınızı baskısı hassas hareketleri ekler. Sonuç olarak, uygulamanızı iOS 9 (veya daha büyük) çalıştıran ve iOS cihazı destekleyici 3B dokunma özellikli ise, baskısı değişiklikleri neden olacak `TouchesMoved` çıkarılmasına olay. 

Bu davranış değişikliği nedeniyle, iOS uygulamaları için hazırlıklı olmalıdır `TouchesMoved` daha sık çağrılacak olay olsa bile X / Y koordinatları değişmemiştir. 

Daha fazla bilgi için lütfen bkz bizim [3B dokunma giriş](~/ios/platform/3d-touch.md) Kılavuzu.

### <a name="document-open-in-place-functionality"></a>Belge açık yerinde işlevi

Kullanarak ya da `FinishedLaunching (application, launchOptions)` veya `WillFinishLaunching (Application, launchOptions)` yöntemlerinin [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) sınıfı, artık bir belgeyi açın ve (aksine bir kopyasında çalışarak) yerinde değiştirin.

Yeni açık yerinde işlevleri desteklemek için ekleme `LSSupportsOpeningDocumentsInPlace` Xamarin.iOS uygulamanızın anahtar **Info.plist** değerini dosyasıyla `YES`.

Lütfen bkz. Apple'nın [UIApplicationDelegate başvuru](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) daha fazla ayrıntı için.

### <a name="enhanced-touch-events"></a>Gelişmiş dokunma olayları

Apple, iOS 9 Touch olaylarına bazı geliştirmeler sağlamıştır. Bunlar Touch tahmin kullanmak için ve görüntü yenilemeleri arasında ara rötuşları erişmek için özelliği içerir.

Lütfen bkz. Apple'nın [iOS için olay işleme Kılavuzu](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) daha fazla ayrıntı için.

### <a name="fetching-tailored-content"></a>Özel içerik getirme

Yeni `NSDataAsset` sınıfı, bellek ve şu anda çalıştığı iOS cihazı grafik özelliklerini uyarlanmış içeriği almak bir Xamarin.iOS uygulaması olanak tanır.

### <a name="new-layout-anchors"></a>Yeni düzen tutturucular

Yeni `NSLayoutAnchor` ve `NSLayoutDimension` düzeni bağlantı sınıflarının çalışma ile yeni bağlantı özelliklerini [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) sınıfı (gibi `LeadingAnchor` ve `WidthAnchor`) düzeni iOS 9 kolaylaştırmak için.

Lütfen bakın bizim [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) bir Xamarin.iOS uygulaması ve Apple'nın otomatik düzenleme ve boyutu sınıflar ile çalışma hakkında daha fazla bilgi için [NSLayoutAnchor başvuru](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [ NSLayoutDimension başvuru](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) ve [UIView başvuru](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) daha fazla bilgi için.

### <a name="new-readable-content-margins"></a>Yeni okunabilir içerik kenar boşlukları

Yeni `UILayoutGuide` sınıfı, okunabilir içerik kenar boşluklarını sağlamak ve bir görünüm içinde içerik için çizim bölgeleri tanımlamak için kullanılabilir. Apple'nın bkz [UILayoutGuide başvuru](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) daha fazla bilgi için.

### <a name="text-input-in-notifications-modifications"></a>Metin girişi bildirimleri değişiklikler

[UIUserNotificationAction](https://developer.xamarin.com/api/type/UIKit.UIUserNotificationAction/) sınıfına sahip yeni bir `Behavior` bildirimleri metin girişten desteklemek için kullanılan özellik.

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate değişiklikleri

Apple tarafından değil resmi olarak kullanım dışı olsa da, bunlar tüm çağrıları değiştirme önermek `FinishedLaunching (UIApplication application)` yöntemi [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) ya da sınıfıyla `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` veya `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` yöntemleri.

Lütfen bkz. Apple'nın [UIApplicationDelegate başvuru](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) daha fazla ayrıntı için.

### <a name="uikit-dynamics-changes"></a>Uıkit Dynamics değişiklikleri

Apple iOS 9 Uıkit Dynamics yapılan aşağıdaki değişiklikler dahil:

- Dynamics şimdi dikdörtgen olmayan çakışma sınırları için destek sağlar.
- Yeni, özelleştirilebilir `UIFieldBehavior` sınıfı çeşitli alan türlerini desteklemek için kullanılır.
- Ek türleri eklenmiştir `UIAttachmentBehavior` sınıfı.

Lütfen bkz. Apple'nın [UIAttachment başvuru](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) daha fazla ayrıntı için.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView ve UIDatePicker değişiklikleri

İOS 9, önce [UIPickerView](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) ve [UIDatePicker](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/) denetimleri yeniden boyutlandırılabilir olmayan ve bunların kapsayıcı (genellikle genişliğini uygulama olan iOS cihazı genişliğini kaplayacak şekilde boyutlandırılabilecek üzerinde çalışan).

İOS 9, bu otomatik yeniden boyutlandırma artık oluşur ve denetimleri ekran boyutuna ve yönüne bağımsız olarak tüm iOS cihazlara 320 noktası uzaklığında çizilir.

Bu durumu düzeltmek için üst öğe kapsayıcısı (Görünüm) kenarlarına denetiminin genişliğini sabitleme ve gerekli yüksekliğini belirtmek için otomatik düzeni ve boyutu sınıfları'ı kullanın. Lütfen bakın bizim [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) bir Xamarin.iOS uygulaması otomatik düzeni ve boyutu sınıflar ile çalışma hakkında daha fazla bilgi için.

### <a name="new-uitextinputassistantitem-class"></a>Yeni UITextInputAssistantItem sınıfı

Yeni `UITextInputAssistantItem` düzeni çubuğu düğmesi gruplarına sınıfı bir _Kısayol Çubuğu_. Kısayol Çubuğu'nu yazarak kısayolları sağlamak için yazılım klavyesini kullanılabilir olan yeni bir alandır.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9’a Giriş](~/ios/platform/introduction-to-ios9/index.md)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [Yeni iOS 9.0 nedir](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
