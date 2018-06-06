---
title: Xamarin.iOS arama API'leri
description: Bu makalede, bilgi ve Xamarin.iOS uygulamalarınızı içinde özellikleri arayın yapmalarına izin vermek için yeni uygulama arama iOS 9 tarafından sağlanan API'leri kullanarak kapsar.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bc62ad34af0b9b98f0475599a08946122badd21e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788182"
---
# <a name="search-apis-in-xamarinios"></a>Xamarin.iOS arama API'leri

_Bu makalede, bilgi ve Xamarin.iOS uygulamalarınızı içinde özellikleri arayın yapmalarına izin vermek için uygulama arama iOS 9 tarafından sağlanan API'lerini kullanarak kapsar._

Arama iOS 9 bilgilere ve bir Xamarin.iOS uygulaması içindeki özelliklere erişmek için yeni yollar sağlamak için genişletilmiştir. Yeni uygulama arama API'lerini kullanarak, uygulama içeriğini Spotlight ve Safari arama sonuçları iletim ve Siri anımsatıcıları ve öneriler aranabilir yapılır. Bu etkinlikler ve uygulamanızda ayrıntılı bilgileri hızlı bir şekilde erişmesine olanak tanır.

Ayrıca, yeni arama API'leri, önceki arama uygulaması deneyimi olmadan, uygulamanızda arama tümleştirmek kolaylaştırır. Bu nedenle, Apple genellikle bir iOS 9 uygulamanın içeriği Evrensel aranabilir uygulama arama'yı kullanarak yapmak için birkaç saat sürer talepleri.

[![](images/intro01.png "İOS 9 uygulama içeriğini uygulama arama'yı kullanarak evrensel aranabilir örneği")](images/intro01.png#lightbox)

Uygulama arama üç ayrı API'lerinin oluşur:

1. [**NSUserActivity** ](nsuseractivity.md) -Bu, Apple iOS 8 yayımlanan iletimi API bir uzantısıdır. Genel ve özel olarak uygulama etkileşim geçmişi aranabilir yapmak için kullanılır) kullanıcı tarafından.

2. [**Çekirdek Spotlight** ](corespotlight.md) -arama sonuçlarında sunulacak içeriği dizini oluşturmak bir uygulama sağlar. Bir veritabanı burada öğeleri eklenir ve kaldırılır ve bu bir uygulama içinde dizin özel içerik için en iyi yoludur API gibi çalışır.

3. [**WebMarkup** ](web-markup.md) - içeriklerini bir web arabirimi üzerinden erişim sağlayan uygulamalar için (değil yalnızca uygulama içinde). Web içeriği, Apple tarafından gezinme ve uygulamanıza kullanıcının iOS 9 cihazında derin bağlama sağlayan özel bağlantılar ile işaretlenebilir.

## <a name="selecting-an-app-search-approach"></a>Bir uygulama arama yaklaşım seçme

Hangi uygulamak için bu yöntemlerin karar uygulamanız tarafından sağlanan etkileşim türlerini ve onu sunar içerik türünü bağlıdır.

Aşağıdaki yönergeleri kullanın:

- [**NSUserActivity** ](nsuseractivity.md) – hem ortak ve özel içerik ve ayrıca, uygulamanızın içinde gezinti noktalarının aranabilirliğini aranabilirliğini sağlamak için bu framework kullanın.

- [**Çekirdek Spotlight** ](corespotlight.md) – aranabilirliğini cihazda depolanan özel veri sağlamak için bu framework kullanın.

- [**Web biçimlendirme** ](web-markup.md) – bu çerçeve, içeriği yalnızca uygulama içinde ancak uygulamanın Web sitesinden sunmak uygulamaları aranabilirliğini sağlamak için kullanın.

Her uygulama yaklaşımlar farklıdır ve olabilir arama, ayrı ayrı ancak Apple bunları birlikte çalışacak biçimde tasarlanan kullanılır. Belirli bir öğeyi dizini oluşturmak için birden fazla yaklaşımı kullanarak, aynı kullandığınızdan emin olun **öğesi kimliği** her iki yaklaşımın üzerinde bu nedenle bu kişi bağlantıları iş birlikte.

Birden fazla yaklaşımı kullanarak içeriğinizi son kullanıcı tarafından bulundu, ancak aynı zamanda gelen öğenizin derecelendirme arama içinde artırmaya yardımcı olur, yalnızca sağlar.

Sıralama işlemi sırasında Geliştirici çoğunlukla saydam belirli bir öğe ile kullanıcı etkileşimi yoğun bu derece (örneğin kullanıcı bağlantısı basmaya) hafiftir.
Zengin ve bilgilendirici öğeleri sağlayarak, bir kullanıcı, içeriği ile etkileşim kurmak için bu nedenle, derecelendirme oluşturma enticed emin emin olabilirsiniz.

## <a name="what-content-to-index"></a>Dizin içeriği

Apple uygulamanızda için arama dizinleri sağlamak için aşağıdaki önerileri hangi içerik ve eylemleri sağlar:

 - Herhangi bir içerik görüntülediğinde, oluşturulan veya kullanıcı tarafından uygulamanızın içinde seçkin.
 - Gezinti noktaları ve Özellikler uygulama içinde.
 - Şeyleri yeni iletiler, içerik veya yakın zamanda cihaza indirilip uygulamanız tarafından görüntülenen öğelerinin diğer türleri gibi.

## <a name="app-search-enhancements"></a>Uygulama arama geliştirmeleri

İOS 10 çekirdek Spotlight bazı geliştirmeler gibi uygulama ara sağlar:

- **Nun ayrıntılı bağlantı popülerliği (ile fark gizlilik)** -ayrıntılı bağlantılı uygulama içeriği arama sonuçlarında yükseltmek için bir yol sağlar.
- **Uygulama arama** -yeni `CSSearchQuery` uygulama Spotlight arama özelliğine posta, mesajlar ve notlar uygulamaları nasıl çalışmasına benzer sağlamak için sınıf.
- **Arama devamlılık** - Spotlight veya Safari bir arama başlatır ve ardından bir uygulamayı açmak kullanıcının izin verir ve arama devam edin.
- **Doğrulama sonuçlarını görselleştirme** -Apple'nın [uygulama arama API Doğrulama Aracı](https://search.developer.apple.com/appsearch-validation-tool) şimdi testleri preforming görsel bir Web sitesinin biçimlendirme ve derin bağlama görüntüler.
- **İleti görüntü paylaşım uygulamasını** -Spotlight aramalarda görünmesi (üzerinden bir ileti uygulama uzantısı) iletilerindeki paylaşmak için sağlanan popüler uygulama görüntüleri sağlar.

Daha fazla bilgi için lütfen bkz bizim [uygulama arama geliştirmeleri](~/ios/platform/search/app-search-enhancements.md) Kılavuzu.

### <a name="proactive-suggestions"></a>Öngörülü önerileri

iOS 10 proaktif olarak yararlı bilgiler otomatik olarak kullanıcı için uygun zamanlarda sunmak sistem vererek yönlendirmeli katılım bir uygulama için yeni yolları gösterir. Yalnızca, iOS 9 derin arama Spotlight, iletim ve Siri öneriler, bir uygulama kullanıcıya aşağıdaki konumlardan sistemi tarafından sunulan işlevselliği kullanıma iOS 10 ile kullanarak uygulama ekleme yeteneği sağlanan:

- Uygulama değiştirici
- Kilit ekranı
- CarPlay
- Eşlemeleri
- Siri etkileşimleri
- QuickType önerileri 

Bir uygulama teknolojileri koleksiyonu gibi kullanarak sisteme bu işlevselliği kullanıma sunan [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), web biçimlendirme, çekirdek Spotlight, MapKit, Media Player ve Uıkit.

Daha fazla bilgi için lütfen bkz bizim [öngörülü önerileri](~/ios/platform/search/proactive-suggestions.md) Kılavuzu.

## <a name="summary"></a>Özet

Bu makalede yeni kapsamına arama API özellikleri, iOS 9 Xamarin.iOS uygulamaları için sağlar. Bu kapsamdaki [NSUserActivity](nsuseractivity.md), [çekirdek Spotlight](corespotlight.md) ve [Web biçimlendirme](web-markup.md) içerik dizin oluşturma için yöntemleri. Belirtilen arama yaklaşım ne zaman kullanılmalı ve hangi içerik türlerini olmalıdır kısa tartışması ile tamamlandı dizini.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Uygulama arama Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
