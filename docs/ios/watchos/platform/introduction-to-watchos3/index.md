---
title: WatchOS 3 giriş
description: Bu makalede Xamarin geliştiriciler için tüm yeni ve değiştirilen API'ler ve watchOS 3 kullanılabilir özellikler sunar.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2017
ms.openlocfilehash: 506e3795538ceffc77301a608c504fc6ec2045a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-watchos-3"></a>WatchOS 3 giriş

_Bu makalede Xamarin geliştiriciler için tüm yeni ve değiştirilen API'ler ve watchOS 3 kullanılabilir özellikler sunar._

Bu belge aşağıdaki konular ele alınacaktır:

- [Yeni watchOS 3 nedir](#Whats-New-in-watchOS-3)
    - [Apple ödeme geliştirmeleri](#Apple-Pay-Enhancements) üzerinde Apple Watch uygulama ödemeler için destek ekler.
    - [Arka plan görevleri](#Background-Tasks) uygulama kullanıcı gerektiğinde hazır olması için arka planda bilgilerini güncelleştirmek olanağı verir.
    - [Zorluklar geliştirmeleri](#Complications-Enhancements) 3 watchOS uygulamalar için yeni özellikler sağlamak için yapılmıştır.
    - [Yeni kullanılabilir çerçeveler](#Newly-Available-Frameworks) watchOS uygulamaları için kullanıma.
    - [Öngörülü önerileri](#Proactive-Suggestions) proaktif olarak kullanıcıya bilgi göster yazmasına izin verir.
    * Birkaç [güvenlik ve gizlilik geliştirmeler](#Security-and-Privacy-Enhancements) watchOS 3 yapılmıştır.
    - [Anlık görüntüler ve yerleştirme](#Snapshots-and-Dock) kullanıcı uygulama watchOS uygulamalara hızlı erişim sağlar.
    - [Kullanıcı bildirimleri](#User-Notifications) kullanıcıya hem yerel hem de uzak bildirimler sağlar.
    * Birkaç [izleme bağlantı Framework geliştirmeleri](#Watch-Connectivity-Framework-Enhancements) watchOS 3 yapılmıştır.
    * Birkaç [WatchKit Framework geliştirmeleri](#WatchKit-Framework-Enhancements) watchOS 3 yapılmıştır.
    - [Etkinlik uygulama geliştirmeleri](#Workout-App-Enhancements) verir yeni yeteneklerini etkinlik için ilgili Apple Watch uygulamalar.
- [Ek Framework değişiklikler](#Additional-Framework-Changes) 3 watchOS yapılmıştır.
- [Kullanım dışı API'leri](#Deprecated-APIs) watchOS 3 içinde.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>Yeni watchOS 3 nedir

Apple, var olan özellikleri de dahil olmak üzere, birçok geliştirmelerin yanı sıra 3 watchOS, birkaç yeni API'ler ve Hizmetleri ekledi:

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Apple Pay geliştirmeleri

WatchOS 3'de, PassKit framework güvenli, uygulama Ödemeler (fiziksel mal ve hizmet için) Apple Watch üzerinde çalışan uygulamalar için desteği sağlamak için genişletilmiştir.

Yeni [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) ve [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) sunmak ve burada kullanıcı yetkilendirmek ödeme istekleri arabirim yanıt için sınıflar.

Daha fazla bilgi için lütfen bkz bizim [Apple ödeme geliştirmeleri](~/ios/watchos/platform/apple-pay.md) Kılavuzu.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>Arka plan görevleri

watchOS 3 uygulama açmadan önce kullanıcının gerektiren içeriğe sahip sağlama bilgilerini güncelleştirmek için kullanabileceğiniz birkaç arka plan görevleri tanıtır.

Aşağıdaki yeni arka plan görevleri kullanılabilir:

- **App Refresh arka plan** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) görev uygulamanın durumunu arka planda güncelleştirmesine izin verir. Genellikle bu kullanarak internet yeni içerik indirme gibi başka bir görev içerecektir bir [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Arka plan anlık görüntü yenileme** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) görev uygulamanın sistem yuva doldurmak için kullanılan bir anlık görüntüsünü alır önce kendi içeriği ve kullanıcı Arabirimi güncelleştirmesine izin verir.
- **Arka plan izleme bağlantı** - [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) görev eşleştirilmiş iPhone arka plan veri aldığında uygulama için başlatıldı.
- **Arka plan URL oturum** - [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) görev, uygulama için başlatılır, arka plan aktarım yetkilendirme gerektirir veya (başarıyla veya hata) tamamlar.

Daha fazla bilgi için lütfen bkz bizim [arka plan görevleri](~/ios/watchos/platform/background-tasks.md) Kılavuzu.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>Zorluklar geliştirmeleri

Zorluklar bir bakışta yararlı bilgiler sağlayan küçük visual öğelerdir. Seçilen gözlem yüz bağlı olarak, kullanıcı bir veya daha fazla olası sahip bir izleme yazıtipi özelleştirme yeteneği vardır.

watchOS 3 uygulama kullanıcı, bilgileri bir bakışta izleme yüz erişebilmesi için izleme uygulaması için bir veya daha fazla olası oluşturma olanağı verir.

Ayrıca, zorluklar aşağıdaki avantajları sağlar:

- Kullanıcı bir izleme yazıtipi doğrudan olası üzerinde e dokunabilirsiniz uygulama hızlı bir şekilde başlatabilir.
- Bir uygulamanın zorluklar izleme yüz nedenler sahip uygulama arka planda uygulamayı başlatmak için deneme burada hazır başlatma durumda tutmak için sistem tutun bellek ve çok zaman güncelleştirmek için sağlar.
- Zorluklar günde en az 50 itme güncelleştirmeleri sağlanır.
- Uygulama zorluklar içerdiğinde, Apple Watch yüz galeride kullanıma (Apple'nın bkz [ekleme zorluklar Galeriye](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) daha fazla bilgi için).

WatchOS 3'de, ClockKit framework artık çok büyük zorluklar için birkaç yeni şablonlar gibi içerir [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) ve [CLKComplicationTemplateExtraLargeRingImage ](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). Ayrıca, yerelleştirilebilir metin oluşturmak için yeni yöntemlerini kullanın [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) sınıfı.

Daha fazla bilgi için lütfen bkz bizim [watchOS 3 için hızlı etkileşim teknikleri](~/ios/watchos/platform/quick-interaction-techniques.md) Kılavuzu.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>Yeni kullanılabilir çerçeveler

watchOS 3 önceden kullanılamayan gibi birkaç var olan Apple çerçeveleri içerir:

- **SceneKit** -kullanım 3B içerecek şekilde SceneKit çoğu özellik, animasyon, gölgelendirme aydınlatma fizik gibi diğer platformlarda kullanılabilir ve parçacık sistemleri de dahil olmak üzere izleme uygulamanın UI modeller. 3B uzamsal ses, özel Metal veya OpenGL gölgelendiriciler, çekirdek resmi filtreleri ve fiziksel olarak bağlı malzemeleri desteklenmez.
- **SpriteKit** -SpriteKit işlemek ve Eylemler, fizik, aydınlatma ve parçacık sistemleri gibi diğer platformlarda kullanılabilir özelliklerin çoğunu dahil olmak üzere uygulama izleme uygulamanın kullanıcı arabiriminde hareketli grafik animasyon için kullanın. 3B uzamsal ses, video oynatmayı ve çekirdek resmi filtreleri desteklenmez.
- **AVFoundation** - yönetebilir ve ses kaydını oynatın.
- **CloudKit** - izleme uygulama ve iCloud kapsayıcıları arasında veri taşıyabilir.
- **Çekirdek ses** - ses akışları, karmaşık arabellek ve saat değerlerini temsil eden veri türlerini yönetmek için.
- **GameKit** - sosyal oyun oluşturmak için.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>Öngörülü önerileri

watchOS 3 sağlayan önceden içinde kullanıcıya bilgi sunmak uygulama bağlamları verilir. Bu özelliği desteklemek için [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) artık içerir `MapItem` diğer uygulamalar tarafından daha sonra kullanmak için konum bilgilerini sağlayın uygulamanın sağlar özelliği.

Daha fazla bilgi için lütfen bkz bizim [öngörülü önerileri giriş](~/ios/watchos/platform/proactive-suggestions.md) Kılavuzu.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>Güvenlik ve gizlilik geliştirmeleri

Apple, güvenlik ve gizlilik watchOS 3 uygulamalarını güvenliğini artırmak ve son kullanıcının gizliliği güvence altına Geliştirici yardımcı olacak bazı geliştirmeler yaptı.

Sonuç olarak, 3 (veya üstü) watchOS üzerinde çalışan uygulamalar içinde gizlilik özel anahtarlarını bir veya daha fazla girerek belirli özellikler veya kullanıcı bilgilerini erişmek için kendi hedefi statik olarak bildirilmelidir kendi `Info.plist` neden uygulamanın istediği erişmek için kullanıcı açıklayan dosyaları.

Bizim iOS 10 watchOS 3 Bu değişiklikler ile iOS 10 paylaşımları olduğundan, lütfen bkz [güvenlik ve gizlilik geliştirmeler](~/ios/app-fundamentals/security-privacy.md) daha fazla bilgi için Kılavuzu.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>Anlık görüntüler ve yerleştirme

WatchOS 3'de, Apple, kullanıcıların sık kullanılan uygulamalarını sabitleyin ve hızlı bir şekilde erişmesine yuva ekledi. Kullanıcının Apple Watch yan düğmesine bastığında sabitlenmiş uygulama anlık görüntü Galerisi görüntülenir. Kullanıcı istediğiniz uygulamayı bulun ve ardından anlık görüntü çalışan uygulamanın arabirimi ile değiştirerek başlatmak için uygulama dokunun için sola veya sağa doğru çekin.

Sistem, düzenli aralıklarla uygulamanın UI anlık görüntüleri alır ve bu anlık görüntülerin belgeleri doldurmak için kullanır. watchOS uygulama içeriği ve kullanıcı Arabirimi bu anlık görüntü oluşturulduğunda önce güncelleştirme olanağı sağlar.

Daha fazla bilgi için lütfen bkz bizim [arka plan görevleri](~/ios/watchos/platform/background-tasks.md) Kılavuzu ve Apple'nın [WKSnapshotRefreshBackgroundTask başvuru](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) .

<a name="User-Notifications" />

## <a name="user-notifications"></a>Kullanıcı bildirimleri

WatchOS 3 sunulan kullanıcı bildirimi framework Apple Watch hem yerel hem de uzak bildirimlerini teslimini destekler. Bu çerçeve, belirli koşullara gün veya konuma göre zamanı gibi bildirimleri zamanlamak ve almak ve bildirimleri işlemek için kullanın.

Daha fazla bilgi için lütfen bkz bizim [watchOS 3 için hızlı etkileşim teknikleri](~/ios/watchos/platform/quick-interaction-techniques.md) Kılavuzu.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>Bağlantı çerçevesi geliştirmeleri izleyin

Yeni `HasContentPending` özelliği [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) sınıfı gösterir oturum işlenmesi gereken arka planda veri aldı. Ve `RemainingComplicationUserInfoTransfers` özelliği döndürür saatleri iOS uygulaması kalan kendi watchOS olası güncelleştirebilirsiniz.

Daha fazla bilgi için lütfen bkz bizim [arka plan görevleri](~/ios/watchos/platform/background-tasks.md) Kılavuzu.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>WatchKit Framework geliştirmeleri

watchOS 3 WatchKit framework aşağıdakiler de dahil bazı geliştirmeler içerir:

- Uygulamayı kullanarak yeni dijital Dama yapma durumunu alabilirsiniz [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) sınıfı ve kullanıcı Dama yapma kullanarak döndürdüğünde güncelleştirmeleri almak [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) sınıfı.
- [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) sınıfı şimdi içerir `ApplicationState` yöntemi ve [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) uygulamayı uygulama çalışma zamanı durumunu izlemek için kullanabileceğiniz sabiti. `WKExtension` Ayrıca, arka plan görevleri zamanlamak için kullanılan iki yeni yöntemleri sağlar.
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) artık yeni içerir `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` ve `HandleBackgroundTasks` uygulamanın durumu değişiklikleri izlemek ve arka plan görevi güncelleştirmeleri işlemek için yöntemleri.
- Yeni bir [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) sınıfı, izleme uygulamalara hareketi tanıma aşağıdaki türlerini sağlamak için eklendi: [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer ](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) ve [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- Yeni [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) sınıfı herhangi HomeKit IP Kamera bağlı bir arabirim sağlar.
- Yeni [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) sınıfı, bir filmi "kullanıcı onu dokunur yükleyen tarafından çalışan film değiştirilir posteri" görüntülenecek uygulama olanak tanır.
- Yeni [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) sınıfı, bir Apple Pay düğmesi dokunduğunuz olduğunda bir ödeme isteği başlatan kullanıcı Arabiriminde göstermek uygulama olanak tanır.
- Yeni [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) sınıfı üzerinde Apple Watch SceneKit Sahne görüntülemek için bir arabirim sunar.
- Yeni [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) sınıfı üzerinde Apple Watch SpriteKit Sahne görüntülemek için bir arabirim sunar.

Daha fazla bilgi için lütfen bkz bizim [watchOS 3 için hızlı etkileşim teknikleri](~/ios/watchos/platform/quick-interaction-techniques.md) Kılavuzu.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>Etkinlik uygulama geliştirmeleri

Yeni watchOS 3 için etkinlik uygulamalar üzerinde Apple Watch arka planda çalıştırma yeteneğini ilişkili. Bu özelliği etkinleştirmek (ve HealthKit verilere erişmek için), uygulama içermelidir `WKBackgroundModes` anahtarını `Info.plist` değeri dosyasıyla `workout-processing`.

Ayrıca, geliştirici şimdi eşleştirilmiş iPhone iOS app sürümünden watchOS etkinlik uygulamasını başlatmak için özelliğine sahiptir.

Daha fazla bilgi için lütfen bkz bizim [etkinlik uygulama geliştirmeleri](~/ios/watchos/platform/workout-apps.md) Kılavuzu.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Ek Framework değişiklikleri

Ana framework değişiklikler ve eklemeler yukarıda listelenen ek olarak, Apple watchOS 3 birçok ek küçük framework değişiklikler yaptı.

Daha fazla bilgi için lütfen bkz bizim [ek Framework değişiklikler](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) Kılavuzu.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Kullanım dışı API'leri

Aşağıdaki API'leri watchOS 3 kullanım dışı bırakıldı:

- `UILocalNotification` Uıkit sınıfının kullanım dışı bırakıldı ve kullanıcı bildirimi framework ile değiştirilmelidir.

Apple'nın bkz [watchOS 2.2 watchOS 3.0 API farklılıklarına](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) tam listesi için belgelere deprecations ve değişiklikleri.


## <a name="related-links"></a>İlgili bağlantılar

- [watchOS Örnekleri](https://developer.xamarin.com/samples/watchos/all/)
- [WatchOS 3 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
