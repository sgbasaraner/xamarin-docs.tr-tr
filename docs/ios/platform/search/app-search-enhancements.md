---
title: "Uygulama arama geliştirmeleri"
description: "Bu makalede yenilikleri kapsayan Apple iOS 10 ve Xamarin.iOS içinde uygulamak nasıl uygulama ara yaptı."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 95f7ad5069abfe4dff82659c0fbc79eef2125e15
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="app-search-enhancements"></a>Uygulama arama geliştirmeleri

_Bu makalede yenilikleri kapsayan Apple iOS 10 ve Xamarin.iOS içinde uygulamak nasıl uygulama ara yaptı._

İOS 10'da, Apple App arama nun derin bağlama, uygulama arama, arama devamlılık ve doğrulama sonuçlarını görselleştirme gibi bazı geliştirmeler yapmıştır. Bu makalede, bir Xamarin.iOS uygulaması bu özellikleri uygulama ele alınacaktır.

## <a name="about-app-search-enhancements"></a>Uygulama arama geliştirmeleri hakkında

İOS 10 çekirdek Spotlight bazı geliştirmeler gibi uygulama ara sağlar:

- **Nun ayrıntılı bağlantı popülerliği (ile fark gizlilik)** -ayrıntılı bağlantılı uygulama içeriği arama sonuçlarında yükseltmek için bir yol sağlar.
- **Uygulama arama** -yeni `CSSearchQuery` uygulama Spotlight arama özelliğine posta, mesajlar ve notlar uygulamaları nasıl çalışmasına benzer sağlamak için sınıf.
- **Arama devamlılık** - Spotlight veya Safari bir arama başlatır ve ardından bir uygulamayı açmak kullanıcının izin verir ve arama devam edin.
- **Doğrulama sonuçlarını görselleştirme** -Apple'nın [uygulama arama API Doğrulama Aracı](https://search.developer.apple.com/appsearch-validation-tool) şimdi testleri preforming görsel bir Web sitesinin biçimlendirme ve derin bağlama görüntüler.
- **İleti görüntü paylaşım uygulamasını** -Spotlight aramalarda görünmesi (üzerinden bir ileti uygulama uzantısı) iletilerindeki paylaşmak için sağlanan popüler uygulama görüntüleri sağlar.

Aşağıdaki bölümlerde bu konularda daha ayrıntılı ele alınacaktır.

## <a name="crowdsourced-deep-link-popularity"></a>Nun ayrıntılı bağlantı popülerliği

iOS 10 bir uygulamaya popüler derin bağlantılar kullanıcı tarafından izlenen ve kullanan bir uygulama sıralamasını iyileştirmek için bu bilgileri arama sonuçlarında içerik hala kullanarak kullanıcının kimliğini korurken sıklığı saymak için bir mekanizma sağlar  *Fark gizlilik*.

Uygulama kullanan kullanıcının için `NSUserActivity` ayrıntılı bağlantı URL'leri sağlamak ve nesneleri `EligibleForPublicIndexing` özelliğini `true`, iOS 10 gönderen bir alt kümesini *fark gizlilik karmaları* Apple'nın sunucularına. Bu bilgiler daha sonra arama sonuçlarında popüler uygulama içeriği yükseltmek için kullanılır.

Bir Xamarin.iOS uygulaması derin bağlama uygulama konusunda daha fazla bilgi için lütfen bkz bizim [NSUserActivity arama](~/ios/platform/search/nsuseractivity.md) belgeleri.

## <a name="in-app-searching"></a>Uygulama arama

Yeni uygulama tarafından [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) sınıfı, bir uygulama Spotlight'ın arama ve kullanıcının uygulaması (benzer şekilde nasıl posta, mesajlar ve notlar uygulama ayrılmak zorunda kalmadan kendi içinde içeriği bulmak için eşleşen kural teknoloji sağlayabilir İş).

Genellikle, uygulamaları destekleyen `CSSearchQuery` kendi ayrı arama dizini korumak gerekmez. 

## <a name="search-continuation"></a>Arama devamlılık

İOS 9'da, Apple arama API'leri sunulan (çekirdek Spotlight gibi `NSUserActivity` ve web biçimlendirme) derin istediğiniz Spotlight ve Safari arama arabirimleri kullanarak bu içerik için arama yapmalarına izin vermek için bir uygulama içinde içerik sağlamak için. Bkz: bizim [yeni arama API'leri](~/ios/platform/search/index.md) daha fazla ayrıntı için belgeleri.

İOS 10 Apple Servisleri veya Safari bir arama başlatır ve ardından bir uygulama açtığınızda aramaya devam etmek kullanıcı vererek sonra bu özellik oluşturur. 

Bu özelliği uygulamak için uygulamanın Düzenle `Info.plist` dosya, ekleme `CoreSpotlightContinuation` anahtar türü **Boolean** ve değerini ayarlama `YES`:

[[ide name="xs]]

[ ![](app-search-enhancements-images/search01.png "Info.plist dosyasında CoreSpotlightContinuation düzenleme")](app-search-enhancements-images/search01.png)

[[/ide]]

[[ide name="vs]]

[ ![](app-search-enhancements-images/searchw01.png "Info.plist dosyasında CoreSpotlightContinuation düzenleme")](app-search-enhancements-images/search01.png)

[[/ide]]

Arama sonucu etmeden kullanıcıya yanıt vermek için (`NSUserActivity`), düzenleme `AppDelegate.cs` dosya ve geçersiz kılma `ContinueUserActivity` yöntemi. Örneğin:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchQuery.ContinuationActionType) {
            var search = userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString);
            // Continue user's search here...
        }
        break;
    }

    return true;
}
```

Bu kod sorgu devamlılık eylem türü için görünür (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`), kullanıcının geçerli sorgudan okur `NSUserActivity` sınıfının kullanıcı bilgisi sözlüğü (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). Buradan, uygulama kullanıcının aramaya devam etmek için eylem yapması gerekmez.

Bir Xamarin.iOS uygulaması aramalarda ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [çekirdek Spotlight arama](~/ios/platform/search/corespotlight.md) belgeleri.

## <a name="visualization-of-validation-results"></a>Doğrulama sonuçlarını Görselleştirme

Apple'nın [uygulama arama API Doğrulama Aracı](https://search.developer.apple.com/appsearch-validation-tool) şimdi görsel bir Web sitesinin biçimlendirme ve derin bağlama görüntüler (gibi tanımlanan biçimlendirme dahil olmak üzere [Schema.org](http://schema.org/)) testleri preforming olduğunda.

Doğrulama Aracı'nı kullanarak bir geliştirici öğeleri desteklenen Applebot Web Gezgini başlık, açıklama, URL ve diğer gibi site için dizine bilgilerini görebilirsiniz.

Web biçimlendirme ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Web biçimlendirme ile arama](~/ios/platform/search/web-markup.md) belgeleri.

## <a name="message-app-image-sharing"></a>Uygulama Paylaşımı görüntü iletisi

Bir ileti uygulama uzantısı iletilerinde paylaşmak için görüntüleri sağlıyorsa, uzantı uygulama ayrılmak zorunda kalmadan iletileri popüler görüntüleri için Spotlight arama yapmak izin vermek için yapılandırılabilir.

Bu özelliği etkinleştirmek için aşağıdakileri yapın:

1. Bir ileti uygulama uzantısı oluşturun.
2. Ekleme `com.apple.developer.associated-domains` uygulamanın yetkilendirmeler için ve ileti uygulama uzantısı paylaştığı görüntüleri barındıran web etki alanlarının bir listesini içerir. Her etki alanı için belirtmek `spotlight-image-search` hizmet.
3. Ekleme bir `apple-app-site-association` görüntüleri barındırma Web sitesine dosya. Bu dosya için bir sözlük içerir `spotlight-image-search` paket kimliği tarafından izlenen takım kimliği veya uygulama kimliği öneki uygulamanın Kimliğini içerir ve hizmeti Dosya en fazla 500 yolları ve Spotlight tarafından dizine ve popüler görüntü aramalara dahil desenleri içerebilir. Daha fazla bilgi için lütfen Apple'nın bkz [oluşturma ve ilişkilendirme dosyasını karşıya yükleme](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) belgeleri.
4. Web siteleri gezinmek Applebot izin verir. Lütfen bkz. Apple'nın [hakkında Applebot](https://support.apple.com/en-us/HT204683) belgeleri.

Bkz: bizim [ileti uygulama tümleştirmesi](~/ios/platform/message-app-integration/index.md) daha fazla ayrıntı için belgeleri.

## <a name="summary"></a>Özet

Bu makalede ele alınan geliştirmeleri Apple iOS 10 ve Xamarin.iOS içinde uygulamak nasıl uygulama ara yaptı.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
