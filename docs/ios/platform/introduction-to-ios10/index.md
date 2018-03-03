---
title: "İOS 10 giriş"
description: "Bu makalede Xamarin.iOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve iOS 10 kullanılabilir özellikler sunar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: d5618ad4477cadfe8977faa9616f5ab6201f14ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-10"></a>İOS 10 giriş

_Bu makalede Xamarin.iOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve iOS 10 kullanılabilir özellikler sunar._

## <a name="introducing-ios-10"></a>İOS 10 Tanıtımı

Yeni iOS 10 SDK'sı, Apple eklendi yeni API'ler ve uygulamalar ve özellikler, yeni kategori oluşturmak geliştiricinin Etkinleştirme Hizmetleri. Bir iOS uygulaması artık önceden kullanılamayan son kullanıcı için zengin, çekici işlevselliği sağlamak için iletileri, Siri, telefon ve haritalar uygulamaları genişletebilirsiniz.

Apple iOS 10 hakkında daha fazla bilgi için lütfen bkz [iOS + uygulamaları](https://developer.apple.com/ios/) belgeleri.

## <a name="whats-new-in-ios-10"></a>Yeni iOS 10 nedir

Apple, iOS 10 var olan özellikleri de dahil olmak üzere, birçok geliştirmelerin yanı sıra, birkaç yeni API'ler ve Hizmetleri ekledi:


## <a name="adapting-to-the-true-tone-display"></a>Doğru ton görüntülenecek uyarlama

Apple'nın doğru ton görüntüleme teknolojisi ortam hafif sensörü dinamik olarak geçerli aydınlatma koşullara uyan görüntülenecek yoğunluğunu ve rengini ayarlamak için bir iOS aygıtı kullanır. iOS 10 sağlayan yeni [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) uygulamanın eklenebilir anahtar `Info.plist` dosya ve doğru ton standart renk geçişinin nasıl uygulandığı denetler. 

Aşağıdaki değerler kullanılabilir:

- `UIWhitePointAdaptivityStyleStandard` **Varsayılan** -standart beyaz nokta adaptivity kullanın.
- `UIWhitePointAdaptivityStyleReading` -Okuma odaklı uygulamalar için kullanılır.
- `UIWhitePointAdaptivityStyleGame` -Oyun odaklı uygulamalar için kullanılır.
- `UIWhitePointAdaptivityStyleVideo` -Video odaklı uygulamalar için kullanılır.
- `UIWhitePointAdaptivityStylePhoto` -Renk uygunluğunu ortam beyaz nokta ayarlamalar daha fazla önemli olduğu fotoğraf odaklı uygulamalar için kullanılır.

<a name="app-extensions" />

## <a name="app-extensions"></a>Uygulama uzantıları

Apple iOS 10 birkaç yeni uygulama uzantısı noktaları sağlamaktadır:

- Arama dizini
- Hedefleri ve amacı kullanıcı Arabirimi
- İletiler
- Bildirim içerik
- Bildirim Hizmetleri
- Etiket paketi

Ayrıca, 3. taraf klavye uygulama uzantıları aşağıdaki geliştirmeleri vardır:

- Yeni `DocumentInputMode` özelliği `UITextDocumentProxy` sınıfı bir belge giriş dili belirlemek ve o dili ile hizalamak klavye uzantısını izin.
- Yeni `HandleInputModeList` yöntemi yanıt dünya dokunduğunuz anahtar olarak sistemin klavye Seçici menüsünü görüntüleme klavye uzantısı sağlar.

Daha fazla bilgi için lütfen bkz bizim [uzantıları giriş](~/ios/platform/extensions.md), [ileti uygulama tümleştirmesi](~/ios/platform/message-app-integration/index.md), [öngörülü önerileri giriş](~/ios/platform/search/proactive-suggestions.md), [ SiriKit giriş](~/ios/platform/sirikit/index.md), [kullanıcı bildirimleri giriş](~/ios/platform/user-notifications/index.md) ve Apple'nın [uygulama uzantısı Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

## <a name="app-search-enhancements"></a>Uygulama arama geliştirmeleri

İOS 10 çekirdek Spotlight bazı geliştirmeler gibi uygulama ara sağlar:

- **Nun ayrıntılı bağlantı popülerliği (ile fark gizlilik)** -ayrıntılı bağlantılı uygulama içeriği arama sonuçlarında yükseltmek için bir yol sağlar.
- **Uygulama arama** -yeni `CSSearchQuery` uygulama Spotlight arama özelliğine posta, mesajlar ve notlar uygulamaları nasıl çalışmasına benzer sağlamak için sınıf.
- **Arama devamlılık** - Spotlight veya Safari bir arama başlatır ve ardından bir uygulamayı açmak kullanıcının izin verir ve arama devam edin.
- **Doğrulama sonuçlarını görselleştirme** -Apple'nın [uygulama arama API Doğrulama Aracı](https://search.developer.apple.com/appsearch-validation-tool) şimdi testleri preforming görsel bir Web sitesinin biçimlendirme ve derin bağlama görüntüler.
- **İleti görüntü paylaşım uygulamasını** -Spotlight aramalarda görünmesi (üzerinden bir ileti uygulama uzantısı) iletilerindeki paylaşmak için sağlanan popüler uygulama görüntüleri sağlar.

Daha fazla bilgi için lütfen bkz bizim [uygulama arama geliştirmeleri](~/ios/platform/search/app-search-enhancements.md) Kılavuzu.

## <a name="apple-pay-enhancements"></a>Apple Pay geliştirmeleri

Apple bazı geliştirmeler Apple Pay için güvenli Web sitelerinde ve Siri ve Haritalar ile etkileşimi aracılığıyla ödemelerini kullanıcıya izin 10 iOS içinde kullanıma sunmuştur.

İOS 10 birkaç yeni API hem iOS hem de dinamik ödeme ağlar ve yeni bir korumalı alan test ortamı desteklemek için watchOS birlikte çalışan eklendi.

Ayrıca, PassKit framework Apple Pay dışında desteklemek için genişletilmiştir `UIKit` ve uygulamalarını içinde kartlarını sunmak kart verenler izin vermek için.

Daha fazla bilgi için lütfen bkz bizim [Apple ödeme geliştirmeleri](~/ios/platform/apple-pay.md) Kılavuzu.

## <a name="alternate-app-icons"></a>Diğer uygulama simgeleri

Apple bazı geliştirmeler simgesini yönetmek bir uygulama sağlayan iOS 10.3 ekledi:

 - `ApplicationIconBadgeNumber` -Alır veya uygulama simgesini rozet Springboard ayarlar.
 - `SupportsAlternateIcons` -İf `true` simgeleri alternatif bir dizi uygulama vardır.
 - `AlternateIconName` -Şu anda seçili alternatif simgenin adını döndürür veya `null` birincil simgesi kullanıyorsanız.
 - `SetAlternameIconName` -Uygulamanın simgesi verilen alternatif simgesi geçiş yapmak için bu yöntemi kullanın.

Daha fazla bilgi için lütfen bkz bizim [diğer uygulama simgeleri](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) Kılavuzu.


## <a name="introduction-to-callkit"></a>CallKit giriş

Yeni iOS 10 CallKit API VoIP uygulamaların UI iPhone ile tümleştirme, bilinen bir arayüz sağlar ve son kullanıcı deneyimi bir yol sağlar. Bu API ile kullanıcılar görebilir ve iOS cihaz kilit ekranı VoIP çağrılarından etkileşim ve telefon uygulamanın kullanarak kişileri Yönet **Sık Kullanılanlar** ve **Recents** görünümleri.

Ayrıca, CallKit API uygulama adıyla (arayan kimliği) bir telefon numarası ilişkilendirmek veya bir sayı olmalıdır, sistem (çağrı engelleme) engellenen söyleyin uzantıları oluşturma olanağı sağlar.

Daha fazla bilgi için lütfen bkz bizim [Callkit giriş](~/ios/platform/callkit.md) Kılavuzu.

## <a name="message-app-integration"></a>İleti uygulama tümleştirmesi

iOS 10 ile tümleştirilir Xamarin.iOS çözümde bir ileti uygulama uzantısı dahil edilmesi verir **iletileri** kullanıcı yeni işlevsellik uygulama ve sunar. Uzantı, metin, etiketler, ortam dosyaları ve etkileşimli iletilerin gönderebilirsiniz. İleti Uygulama Uzantısı iki tür mevcuttur:

- **Etiket paketleri** -kullanıcı bir iletiye ekleyebilirsiniz etiketler koleksiyonunu içerir. Etiket paketleri hiçbir kod yazmadan oluşturulabilir.
- **iMessage uygulama** -etiketler seçerek, metin girerek, ortam dosyalarıyla (isteğe bağlı tür dönüşümleri) dahil olmak üzere ve oluşturma, düzenleme ve etkileşim iletileri göndermek için Messages uygulamasının içinde özel bir kullanıcı arabirimi sunabilir.

Daha fazla bilgi için lütfen bkz bizim [ileti uygulama tümleştirmesi](~/ios/platform/message-app-integration/index.md) Kılavuzu.

## <a name="news-publisher-enhancements"></a>Haber yayımcı geliştirmeleri

İOS 10 Apple herkesin ana dergi ve Günlük tutanlar ve kaydolmak için bağımsız yayımcı ve ürün için yeni kuruluşlara ve Apple haber uygulama içerik dağıtımı. Daha fazla bilgi için Apple'nın bkz [haber kaynakları](https://newsresources.apple.com/) belgeleri.

## <a name="providing-haptic-feedback"></a>Haptic geribildirim sağlama

İPhone ve iPhone 7 üzerinde fiziksel olarak kullanıcı bulunmaya ek yöntemler sağlayan yeni haptics yanıtlar 7 artı ve Apple eklendi. Kullanıcının dikkatini ve eylemlerini pekiştirmek için yeni tactile geri bildirim seçeneklerini kullanın.

Birkaç yerleşik kullanıcı Arabirimi öğeleri zaten seçiciler, anahtarlar ve kaydırıcılar gibi haptic geri bildirim sağlayın. iOS 10 şimdi haptics somut alt sınıfı kullanılarak programlı olarak tetiklemek yeteneği ekler `UIFeedbackGenerator` sınıfı.

Daha fazla bilgi için lütfen bkz bizim [Haptic geri bildirim sağlama](~/ios/user-interface/ios-ui/haptic-feedback.md) Kılavuzu.

## <a name="proactive-suggestions"></a>Öngörülü önerileri

iOS 10 proaktif olarak yararlı bilgiler otomatik olarak kullanıcı için uygun zamanlarda sunmak sistem vererek yönlendirmeli katılım bir uygulama için yeni yolları gösterir. Yalnızca, iOS 9 derin arama Spotlight, iletim ve Siri öneriler, bir uygulama kullanıcıya aşağıdaki konumlardan sistemi tarafından sunulan işlevselliği kullanıma iOS 10 ile kullanarak uygulama ekleme yeteneği sağlanan:

- Uygulama değiştirici
- Kilit ekranı
- CarPlay
- Eşlemeleri
- Siri etkileşimleri
- QuickType önerileri 

Bir uygulama teknolojileri koleksiyonu gibi kullanarak sisteme bu işlevselliği kullanıma sunan [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), web biçimlendirme, çekirdek Spotlight, MapKit, Media Player ve Uıkit.

Daha fazla bilgi için lütfen bkz bizim [öngörülü önerileri giriş](~/ios/platform/search/proactive-suggestions.md) Kılavuzu.

## <a name="request-app-review"></a>İstek uygulama gözden geçirme

10.3, iOS için yeni `RequestReview()` yöntemi oran veya incelemek için kullanıcıya sor bir iOS uygulamasını sağlar. Bu yöntem, burada kullanıcı deneyimi mantıklıdır herhangi bir noktada çağrılabilir rağmen gözden geçirme işlemi tarafından yönetilen ve uygulama mağazası İlkesi tarafından işlenen. Sonuç olarak, bu yöntem olabilir veya bir uyarı görüntülenmeyebilir ve hiçbir zaman yanıt olarak bir düğmesine gibi bir kullanıcı eylemi çağrılmalıdır.

Daha fazla bilgi için lütfen bkz bizim [isteği uygulama İnceleme](~/ios/platform/request-app-review.md) Kılavuzu.

## <a name="security-and-privacy-enhancements"></a>Güvenlik ve gizlilik geliştirmeleri

Apple, güvenlik ve gizlilik 10 iOS uygulamalarını güvenliğini artırmak ve son kullanıcının gizliliği güvence altına Geliştirici yardımcı olacak bazı geliştirmeler yaptı.

Sonuç olarak, uygulamaları iOS 10 (veya sonraki sürümler) çalıştıran bir veya daha fazla gizlilik belirli anahtarlarında girerek belirli özellikler veya kullanıcı bilgilerini erişmek için kendi hedefi statik olarak bildirmelidir kendi `Info.plist` neden uygulamanın istediği erişmek için kullanıcı açıklayan dosyaları.

Daha fazla bilgi için lütfen bkz bizim [güvenlik ve gizlilik geliştirmeler](~/ios/app-fundamentals/security-privacy.md) Kılavuzu.

## <a name="sirikit"></a>SiriKit

Yeni iOS 10, iOS cihazında Siri kullanarak kullanıcı tarafından erişilebilir hizmetleri sağlamak bir Xamarin.iOS uygulaması SiriKit sağlar. Bu işlevi kullanarak yeni bir veya daha fazla uygulama uzantısı'nda sağlanan **hedefleri** ve **hedefleri UI** çerçeveleri.

Aşağıdaki hizmet alanları SiriKit destekler:

- Ses veya video çağırma.
- Bir kılma kayıt.
- Egzersizleriniz yönetme.
- İleti.
- Fotoğraf aranıyor.
- Gönderme veya ödemeler alma.

Kullanıcı, uygulama uzantının hizmetlerinden birini içeren Siri isteği yaptığında, SiriKit uzantısı gönderir bir **hedefi** kullanıcının isteği destekleyici verilerin tanımlayan nesne. Uygulama Uzantısı sonra uygun oluşturur **yanıt** için nesne verilen **hedefi**, uzantısı isteği nasıl işleyebilir ayrıntılı.

Siri genellikle tüm kullanıcı etkileşimi işlerken uygulama uzantısı kullanabileceğiniz **hedefi UI** bir zengin, özel kullanıcı arabirimi uygulamayı markalama özelliklerine sahip sunmak için framework ve ek bilgiler.

Daha fazla bilgi için lütfen bkz bizim [SiriKit giriş](~/ios/platform/sirikit/index.md) Kılavuzu.

## <a name="speech-recognition"></a>Konuşma tanıma

iOS 10 metne uygulamanın sürekli konuşma tanıma desteği ve konuşma (gelen Canlı veya kaydedilmiş ses akışları) transcribe izin veren yeni bir konuşma API içerir.

Konuşma tanıma iletim ve Apple'nın sunucularındaki uygulama verileri geçici olarak depolanmasını gerektirdiğinden _gerekir_ kullanıcının dahil ederek tanıma gerçekleştirme izni istemek `NSSpeechRecognitionUsageDescription` anahtarını kendi `Info.plist` Dosya ve arama `SFSpeechRecognizer.RequestAutorization` yöntemi.

Daha fazla bilgi için lütfen bkz bizim [konuşma tanıma giriş](~/ios/platform/speech.md) Kılavuzu.

## <a name="user-notifications"></a>Kullanıcı bildirimleri

Yeni ios'a 10, kullanıcı bildirimi teslimi ve yerel ve uzak bildirimler işleme framework olanak sağlar. Bu çerçeve kullanarak, uygulama veya uygulama uzantısı yerel bildirimleri teslim konum gibi koşulları veya günün saatini belirterek zamanlayabilirsiniz.

Ayrıca, uygulama veya uzantı alabilir (ve büyük olasılıkla değiştirmek) hem yerel hem de uzak bildirimler kullanıcının iOS cihazına teslim edilir.

Yeni Kullanıcı bildirim UI framework uygulaması veya uygulama kullanıcıya sunulan hem yerel hem de uzak bildirimler görünümünü özelleştirmek için uzantı sağlar.

Daha fazla bilgi için lütfen bkz bizim [kullanıcı bildirimleri Framework](~/ios/platform/user-notifications/index.md) Kılavuzu.

## <a name="video-subscriber-account"></a>Video abone hesabı

Yeni iOS 10 Video abone hesap framework uygulamalarının, kimlik doğrulaması desteği akış veya son kullanıcı için bir tek oturum açma deneyimi kullanarak, kablo veya uydu TV sağlayıcıları, kimlik doğrulaması için isteğe bağlı video sağlar.

## <a name="wide-color"></a>Geniş rengi

iOS 10 genişletilmiş aralığı piksel biçimleri ve çekirdek grafikleri, çekirdek görüntü, Metal ve AVFoundation gibi çerçeveleri dahil olmak üzere Sistem genelindeki wide gam renk alanları desteği genişletir. Geniş renk görüntüler ile cihazları için destek Bu davranış tüm grafik yığını boyunca sağlayarak daha fazla hızının.

Ayrıca, [Uıkit](https://developer.xamarin.com/api/namespace/UIKit/) değiştirildi yeni çalışmak için Genişletilmiş **sRGB** renk uzayı, geniş renk gamutları birbirinden önemli performans kaybı olmadan renkleri karışık kolaylaştırır.

Apple geniş renklerle çalışırken aşağıdaki en iyi yöntemleri sunar:

- [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/) değerlere sRGB Renk aralığı ve artık kullanır clamp artık `0.0` için `1.0` aralık. Uygulamanın önceki clamp davranışı dayalıysa, 10 iOS için değiştirilmesi gerekir.
- Çizim ortamı sRGB Renk alanını özel gerçekleştirirken yapılandırılacak `UIView` iPad Pro çizim.
- Uygulama özel çizmeye gerçekleştirirse `UIImages`, yeni [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) genişletilmiş aralığı veya standart aralık biçimleri kullanımını belirlemek için sınıf.
- Görüntü işleme sağlamak için çekirdek grafikleri veya Metal gibi düşük düzey API'si kullanılarak Geliştirici 16 bit kayan nokta değerlerine destekleyen bir genişletilmiş aralık renk alanını ve piksel biçimi kullanmalıdır. Gerektiğinde, geliştirici el ile renk bileşeni değerleri clamp gerekecektir.
- Çekirdek grafikleri, çekirdek görüntü ve Metal performans gölgelendiriciler tüm iki renk alanları arasında dönüştürme için yeni yöntemler sağlar.

Daha fazla bilgi için lütfen bkz bizim [geniş renk giriş](~/ios/platform/wide-color.md) Kılavuzu.

## <a name="widget-enhancements"></a>Pencere öğesi geliştirmeleri

Apple pencere öğeleri 10 kilit ekranı üzerinde yeni iOS mevcut herhangi bir arka plan üzerine harika göründüğünden emin olmak için pencere sistemin bazı geliştirmeler anlatılmıştır. [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) özelliği kullanım dışı bırakıldı ve yeni değiştirildi [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) veya [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) özellikleri. Ayrıca, pencere öğeleri artık içeren bir [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) ne kadar içerik kullanılabilir açıklamak Geliştirici sağlar ve içeriği genişletme ve daraltma kullanıcıya izin veren özellik.

Daha fazla bilgi için lütfen bkz bizim [arama ve giriş ekranı pencere öğesi geliştirmeleri](~/ios/platform/search/widgets.md) Kılavuzu.

## <a name="additional-framework-changes"></a>Ek Framework değişiklikleri

Ana framework değişiklikler ve eklemeler yukarıda listelenen ek olarak, Apple iOS 10 birçok ek küçük framework değişiklikler yaptı.

Daha fazla bilgi için lütfen bkz bizim [ek Framework değişiklikler](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) Kılavuzu.

## <a name="deprecated-apis"></a>Kullanım dışı API'leri

Aşağıdaki API'leri iOS 10 kullanım dışı bırakıldı:

- `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` Ve `CKFetchRecordChangesOperation` sınıfları kullanımdan kaldırıldı CloudKit 10 iOS için. Kullanım [CKDiscoverAllUserIdentitiesOperation](https://developer.xamarin.com/api/type/CloudKit.CKDiscoverUserIdentitiesOperation/), [CKUserIdentity](https://developer.xamarin.com/api/type/CloudKit.CKUserIdentity/) ve [CKFetchRecordZoneChangesOperation](https://developer.xamarin.com/api/type/CloudKit.CKFetchRecordZoneChangesOperation/) (kayıt paylaşımı desteği) sınıflarını yerine.
- Birkaç [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) API'leri (örneğin, bölge ve sorgu tabanlı abonelikler) kullanımdan kaldırıldı. Kullanım [CKRecordZoneSubscription](https://developer.xamarin.com/api/type/CloudKit.CKRecordZoneSubscription/) ve [CKQuerySubscription](https://developer.xamarin.com/api/type/CloudKit.CKQuerySubscription/) API'leri yerine.
- [NSPersistentStoreCoordnator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) bulunabilen içerikle ilgili simgeleri kullanımdan kaldırıldı.
- `ADBannerView`, `ADInterstitialAd` ve ilişkili simgeleri ın [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) sınıfı kullanımdan kaldırıldı.
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform) kayan nokta değerlerine ilişkili simgeleri kullanımdan kaldırıldı.
- `UILocalNotification`, `UIMutableUserNotificationAction`, `UIMutableUserNotificationCategory`, `UIUserNotificationAction`, `UIUserNotificationCategory` Ve `UIUserNotificationSettings` Uıkit sınıfları kullanımdan kaldırıldı. Kullanım [kullanıcı bildirimleri](#User-Notifications) framework yerine.
- `HandleActionForLocalNotification`, `HandleActionForRemoteNotification`, `DidReceiveLocalNotification` Ve `DidReceiveRemoteNotification` WatchKit yöntemleri kullanımdan kaldırıldı. Kullanım `HandleActionForNotification` ve `DidReceiveNotification` yöntemleri yerine.
- `DidReceiveLocalNotification` Ve `DidReceiveRemoteNotification` yöntemlerinin [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) kullanım dışı bırakıldı. Bir örneğini oluşturmak [UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) uygun yöntemlerini uygular ve atayın `Delegate` özelliği [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) nesnesi.
- **Game Center uygulamasının** kullanım ve iOS kaldırıldı. Uygulama GameKit, kullanıyorsa, _gerekir_ liderlik, vb. gibi GameKit özellikleri görüntülemek için kendi arabirimi sunar.

Apple'nın bkz [iOS 9.3 iOS 10.0 API farklılıkları](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) deprecations tam listesi için belgelere.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
- [İOS 10 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
