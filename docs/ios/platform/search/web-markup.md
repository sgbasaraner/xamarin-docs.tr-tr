---
title: Web biçimlendirme Xamarin.iOS içinde arama
description: Bu belge, bir Xamarin.iOS uygulamasına bağlantı web tabanlı arama sonuçları oluşturmayı açıklar. Dizin oluşturma, uygulamanızın Web sitesi bulunabilir, akıllı uygulama başlıkları, Evrensel bağlantılar ve daha fazlasını kullanarak olmasını web içeriği etkinleştirmek nasıl açıklanır.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 438a65de3eb78f849493e3478bce5522a325d0cd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788000"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Web biçimlendirme Xamarin.iOS içinde arama

Bir web sitesi aracılığıyla kendi içeriğine erişim sağlayan uygulamalar için (değil yalnızca uygulama içinde), web içeriği işaretlenir, Apple tarafından gezinme ve, uygulama kullanıcının iOS 9 cihazında derin bağlama sağlayan özel bağlantılarla.

İOS uygulamanızı zaten mobil derin bağlama destekliyorsa ve Web sitenizi içerik, uygulamanızın, Apple içinde ayrıntılı bağlantılarını sunulan _Applebot_ web Gezgini bu içerik dizin ve otomatik olarak kendi bulut dizini ekleyin:

[![](web-markup-images/webmarkup01.png "Bulut dizini genel bakış")](web-markup-images/webmarkup01.png#lightbox)

Apple bu sonuçları Spotlight arama ve Safari arama sonuçlarında belirir.
Üzerinde kullanıcı dokunur varsa bunlardan birini sonuçları (ve uygulamanızı yüklü olan), uygulamanızda içeriğe uygulanır:

[![](web-markup-images/webmarkup02.png "Derin bir Web sitesi arama sonuçlarında bağlama")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>Web içerik dizinini etkinleştirme

Uygulamanın içeriği aranabilir Web biçimlendirme kullanarak yapmak için gereken dört adım vardır:

1. Apple bulmak ve uygulamanızın Web sitesi olarak tanımlayarak dizin olun **Destek** veya **pazarlama** iTunes Connect Web sitesi.
2. Uygulamanızın Web sitesi mobil derin bağlama uygulamak için gerekli biçimlendirme içerdiğinden emin olun. Aşağıdaki bölümlerde daha fazla ayrıntı için bkz.
3. İOS uygulamanızı işleme ayrıntılı bağlantısı etkinleştirin.
4. Son kullanıcı için zengin ve çekici sonuç sağlamak için uygulamanızın Web sitesi tarafından ortaya yapılandırılmış veriler için işaretleme ekleyin. Bu adım kesinlikle gerekli olmamasına karşın, Apple tarafından önerilir.

Aşağıdaki bölümlerde ayrıntılı adımları üzerinden geçer.

## <a name="make-your-apps-website-discoverable"></a>Uygulamanızın Web sitesi bulunabilirlik olun

Olarak kullanmak için en kolay yolu, uygulamanızın Web sitesini bulma Apple sahip olduğu **Destek** veya **pazarlama** Apple uygulamanıza iTunes Bağlan aracılığıyla gönderdiğinizde Web sitesi.

## <a name="using-smart-app-banners"></a>Akıllı uygulama başlıkları kullanma

Uygulamanıza açık bir bağlantı sunmak için Web sitenizde akıllı bir uygulama içi başlık sağlayın. Uygulama zaten yüklü değilse, Safari otomatik olarak, uygulamayı yüklemek için kullanıcıya sorar. Aksi takdirde kullanımı dokunabilirsiniz **Görünüm** Web uygulamanızı başlatmak için bağlantı. Örneğin, akıllı bir uygulama içi başlık oluşturmak için aşağıdaki kodu kullanabilirsiniz:

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

Daha fazla bilgi için lütfen Apple'nın bkz [akıllı uygulama başlıkları yükseltme uygulamalarla](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) belgeleri.

## <a name="using-universal-links"></a>Evrensel bağlantıları kullanma

Yeni iOS 9, aşağıdakileri sağlayarak daha iyi bir alternatif akıllı uygulama başlıkları veya var olan özel URL şemalarını Evrensel bağlantılar sağlar:

- **Benzersiz** – aynı URL'ye birden fazla Web sitesi tarafından istenemiyor.
- **Güvenli** – Web sitesi sizin tarafınızdan ve geçerli bir şekilde ait sağlar Web sitesi bağlı için imzalı bir sertifika gereklidir, uygulamanızın.
- **Esnek** – son kullanıcı URL Web sitesi veya uygulaması başlatır olup olmadığını kontrol edebilirsiniz.
- **Evrensel** – aynı URL, Web sitenizin ve uygulamanızın içeriğini tanımlamak için kullanılabilir.

## <a name="using-twitter-cards"></a>Twitter kart kullanma

Bir Twitter kart kullanarak, uygulamanızın içerik ayrıntılı bağlantılarını sağlayabilir. Örneğin:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

Daha fazla bilgi için lütfen Twitter'ın bakın [Twitter kartı Protokolü](http://dev.twitter.com/cards/mobile) belgeleri.

## <a name="using-facebook-app-links"></a>Facebook uygulama bağlantıları kullanma

Bir Facebook uygulama bağlantıyı kullanarak, uygulamanızın içerik ayrıntılı bağlantılarını sağlayabilir. Örneğin:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

Daha fazla bilgi için lütfen Facebook'ın bakın [uygulama bağlantılar](http://applinks.org) belgeleri.

## <a name="opening-deep-links"></a>Ayrıntılı bağlantılar açma

Açma ve derin bağlantılar Xamarin.iOS uygulamanızı görüntülemek için destek eklemeniz gerekir. Düzen **AppDelegate.cs** dosya ve geçersiz kılma `OpenURL` yöntemini özel URL biçimi. Örneğin:

```csharp
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{

    // Handling a URL in the form http://company.com/appname/?123
    try {
        var components = new NSUrlComponents(url,true);
        var path = components.Path;
        var query = components.Query;

        // Is this a known format?
        if (path == "/appname") {
            // Display the view controller for the content
            // specified in query (123)
            return ContentViewController.LoadContent(query);
        }
    } catch {
        // Ignore issue for now
    }

    return false;
}
```

Yukarıdaki kodda URL'sini içeren arıyoruz `/appname` ve değerini geçirme `query` (`123` Bu örnekte) kullanıcıya istenen içeriği görüntülemek için bizim uygulamaya özel görünüm denetleyicisi.

## <a name="providing-rich-results-with-structured-data"></a>Yapılandırılmış verileri sağlayan zengin sonuçları

Yapılandırılmış verileri biçimlendirme dahil olmak üzere tarafından yalnızca bir başlık ve açıklama gidin son kullanıcının zengin arama sonuçları sağlayabilir. Görüntüleri, uygulama özel verileri (örneğin, derecelendirmeleri) ve yapılandırılmış veri biçimlendirmesi kullanarak sonuçları eylemleri içerir.

Zengin sonuçları daha fazla çekici olan ve artırmaya yardımcı olabilir, derecelendirme bulutta tabanlı arama dizinini etkileşim için daha fazla kullanıcı cazip tarafından.

Yapılandırılmış verileri biçimlendirme sağlamak için bir seçenek açık grafik kullanmaktır. Örneğin:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

Daha fazla bilgi için lütfen bkz [açık grafik](http://ogp.me) Web sitesi.

Yapılandırılmış verileri biçimlendirme için başka bir ortak biçim schema.org'ın mikro veri biçimidir. Örneğin:

```xml
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
    <span itemprop="ratingValue">4** stars -
    <span itemprop="reviewCount">255** reviews


```

Aynı bilgileri schema.org'ın JSON-LD biçiminde gösterilebilir:

```xml
<script type="application/ld+json">
    "@content":"http://schema.org",
    "@type":"AggregateRating",
    "ratingValue":"4",
    "reviewCount":"255"
</script>
```

Zengin arama sonuçlarını son kullanıcıya sağlayan Web sitenizi meta verilerinin bir örnek gösterilmektedir:

[![](web-markup-images/deeplink01.png "Zengin arama sonuçları aracılığıyla yapılandırılmış veri biçimlendirme")](web-markup-images/deeplink01.png#lightbox)

Apple şu anda schema.org aşağıdaki şema türlerden destekler:

 - AggregateRating
 - ImageObject
 - InteractionCount
 - Sunar
 - Kuruluş
 - PriceRange
 - Tarif
 - SearchAction

Bu düzeni türleri hakkında daha fazla bilgi için lütfen bkz [schema.org](http://schema.org).

## <a name="providing-actions-with-structured-data"></a>Yapılandırılmış verileri eylemlerle sağlama

Yapılandırılmış verileri belirli türde son kullanıcı tarafından kullanılabilir olması bir arama sonucu izin verir. Şu anda aşağıdaki eylemleri desteklenir:

 - Aranan bir telefon numarası.
 - Belirli bir adresi eşlemesi yönüne alınıyor.
 - Ses veya video dosyası yürütülüyor.

Örneğin, bir telefon numarasını aramak için bir eylem tanımlama aşağıdakine benzeyebilir:

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

Bu arama sonucu son kullanıcıya sunulduğunda, sonuçta bir küçük telefon simgesi görüntülenir. Kullanıcı simgesi dokunur, belirtilen sayı çağrılır.

Aşağıdaki HTML arama sonuç bir ses dosyasını oynatmak için bir eylem ekleyin:

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

Son olarak, aşağıdaki HTML Arama sonuçlarından yönergeleri almak için bir eylem ekleyin:

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

Daha fazla bilgi için lütfen Apple'nın bkz [App arama Geliştirici sitesi](http://developer.apple.com/ios/search/).



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Uygulama arama Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
