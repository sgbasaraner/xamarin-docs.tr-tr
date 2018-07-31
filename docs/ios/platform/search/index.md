---
title: Xamarin.iOS arama API'leri
description: Bu makale, kullanıcıların bilgi ve Xamarin.iOS uygulamalarınız içinde özellikleri arayın izin vermek için yeni uygulama arama iOS 9 tarafından sağlanan API'leri kullanarak kapsar.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4e73e1bc34df8628790a3734e5b3b32a687fdf14
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351658"
---
# <a name="search-apis-in-xamarinios"></a>Xamarin.iOS arama API'leri

_Bu makale, bilgi ve Xamarin.iOS uygulamalarınızdaki özellikleri aramak kullanıcıların uygulama arama iOS 9 tarafından sağlanan API'leri kullanarak kapsar._

Arama, içinde iOS 9 bilgilere ve bir Xamarin.iOS uygulaması içinde özelliklere erişim için yeni yollar sağlamak için genişletilmiştir. Yeni uygulama arama API'leri kullanarak, uygulama içeriği yenilik ve Safari'nin Arama sonuçlarıyla, iletim ve Siri anımsatıcılar ve öneriler aranabilir yapılır. Bu etkinlikleri ve uygulamanızda ayrıntılı bilgileri kolayca erişmesine olanak tanır.

Ayrıca, yeni arama API'leri arama önceki arama uygulaması deneyimi olmadan uygulamanıza tümleştirmek kolaylaştırır. Bu nedenle, genellikle bir iOS 9 uygulamanın içeriği Evrensel aranabilir kullanarak uygulama arama yapmak için birkaç saat sürer Apple talepleri.

[![](images/intro01.png "İOS 9 uygulama içeriği Evrensel uygulama arama ile aranabilir örneği")](images/intro01.png#lightbox)

Uygulama araması üç ayrı API'leri oluşur:

1. [**NSUserActivity** ](nsuseractivity.md) -Bu, Apple iOS 8'de yayınlanan İletimli API bir uzantısıdır. Genel ve özel olarak uygulama etkileşimi geçmişi aranabilir yapmak için kullanılır) kullanıcı tarafından.

2. [**Çekirdek Spotlight** ](corespotlight.md) -bir uygulamanın dizin içeriği arama sonuçlarında gösterilmesini sağlar. Burada öğeleri eklendi ve kaldırıldı ve bu uygulama içinde özel içerik dizini en iyi yolu API veritabanı gibi çalışır.

3. [**WebMarkup** ](web-markup.md) - içeriklerinin bir web arabirimi üzerinden erişim sağlayan uygulamalar (değil yalnızca uygulama içinde). Web içeriği, Apple tarafından gezinme ve uygulamanıza kullanıcının iOS 9 cihazında ayrıntılı bağlantı sağlama sağlayan özel bağlantılar ile işaretlenebilir.

## <a name="selecting-an-app-search-approach"></a>Bir uygulama arama yaklaşım seçme

Hangi uygulamak için bu yöntemlerin karar, uygulamanız tarafından sağlanan etkileşim türlerini ve sunduğu içerik türünü bağlıdır.

Aşağıdaki yönergeleri kullanın:

- [**NSUserActivity** ](nsuseractivity.md) – hem ortak ve özel içeriği hem de uygulamanızın içinde gezinti noktalarının aranabilirliğini aranabilirliğini sağlamak için bu çerçeveyi kullanın.

- [**Çekirdek Spotlight** ](corespotlight.md) – aranabilirliğini cihazda depolanan özel veri sağlamak için bu çerçeveyi kullanın.

- [**Web biçimlendirmesi** ](web-markup.md) – aranabilirliğini içeriklerini uygulama içinde ancak uygulamanın Web sitesinden değil yalnızca mevcut uygulamaları sağlamak için bu çerçeveyi kullanın.

Her uygulama yaklaşımları farklıdır ve olabilir arama, ayrı ayrı, ancak Apple bunları birlikte çalışmak üzere tasarlanan kullanılır. Belirli bir öğeyi dizini oluşturmak için birden fazla yaklaşımı kullanarak, aynı kullandığınızdan emin olun **öğesi kimliği** her yaklaşımı, bu nedenle kişi bağlantıları iş birlikte.

Birden fazla yaklaşımı kullanarak içeriğinizi son kullanıcı tarafından bulundu ancak ayrıca arama içinde öğenizin sıralamasından geliştirmeye yardımcı yalnızca sağlar.

Derecelendirme işlemde çalışırken çoğunlukla saydam bir geliştirici olarak belirli bir öğe ile kullanıcı etkileşimi yoğun olarak bu dereceyi (örneğin bir bağlantı basmaya kullanıcı) üzerinde ağırlıklandıran.
Zengin, bilgilendirici öğeleri sağlayarak kullanıcı içeriğinizi ile etkileşim kurmak için bu nedenle, sıralama yükseltme enticed emin emin olabilirsiniz.

## <a name="what-content-to-index"></a>Dizin içeriği

Apple için uygulamanıza arama dizinleri sağlamak için aşağıdaki önerileri hangi içerik ve eylemleri sağlar:

 - Herhangi bir içerik görüntülenebilir, oluşturulan veya kullanıcı tarafından uygulamanızın içinde seçkin.
 - Gezinti noktaları ve uygulama içinde özellikleri.
 - Yeni iletileri, içeriği veya yakın zamanda cihaza indirilip uygulamanız tarafından görüntülenen öğelerin diğer türleri gibi şeyler.

## <a name="app-search-enhancements"></a>Uygulama araması geliştirmeleri

İOS 10 temel odak noktası, bazı geliştirmeler gibi uygulama ara içerir:

- **Kitle kaynaklı ayrıntılı bağlantı popülerliği (ile fark gizlilik)** -ayrıntılı bağlantılı uygulama içeriği arama sonuçlarında yükseltmek için bir yol sağlar.
- **Uygulama içi arama** -yeni `CSSearchQuery` uygulamanın Spotlight arama özelliğine benzer posta, mesajlar ve notlar uygulamaları nasıl sağlamak için sınıf.
- **Arama devamlılık** - Spotlight veya Safari bir arama başlatın, ardından bir uygulama açma açmasına olanak sağlar ve bu aramanın devam.
- **Doğrulama sonuçlarını görselleştirme** -Apple'nın [uygulama arama API Doğrulama Aracı](https://search.developer.apple.com/appsearch-validation-tool) artık testleri preforming bir Web sitesinin işaretleme ve derin bağlama görsel bir temsilini görüntüler.
- **İleti görüntü paylaşım uygulamasının** -Spotlight arama görünmesini (ileti uygulaması uzantısı) üzerinden mesajları alıp içinde paylaşmak için sağlanan popüler uygulama görüntüleri sağlar.

Daha fazla bilgi edinmek için bkz. bizim [uygulama araması geliştirmeleri](~/ios/platform/search/app-search-enhancements.md) Kılavuzu.

### <a name="proactive-suggestions"></a>Proaktif öneriler

iOS 10 proaktif olarak yararlı bilgiler otomatik olarak kullanıcıya uygun zamanlarda sunmak üzere sistem vererek sürüş engagement bir uygulama için yeni yollarını sunar. Yalnızca olarak iOS 9 spot, iletim ve Siri öneriler, bir uygulama kullanıcıya aşağıdaki konumlardan sistemi tarafından sunulan işlevselliği kullanıma sunabileceğiniz iOS 10 ile kullanarak uygulama için ayrıntılı arama ekleme olanağı sağlanan:

- Uygulama değiştirici
- Kilit ekranı
- CarPlay
- Haritalar
- Siri etkileşimleri
- QuickType önerileri 

Bir uygulamanın gibi teknolojileri koleksiyonunu kullanarak sisteme bu işlevselliği kullanıma sunma [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), temel bakış, MapKit, Media Player ve Uıkit web biçimlendirmesi.

Daha fazla bilgi edinmek için bkz. bizim [proaktif öneriler](~/ios/platform/search/proactive-suggestions.md) Kılavuzu.

## <a name="summary"></a>Özet

Bu makalede yeni kapsamına arama API'si özellikleri, iOS 9, xamarin iOS uygulamaları için sağlar. Bunu ele [NSUserActivity](nsuseractivity.md), [temel bakış](corespotlight.md) ve [Web biçimlendirmesi](web-markup.md) yöntemleri için içerik dizini mi oluşturuyorsunuz. Belirtilen arama yaklaşım ne zaman kullanılması gerektiğini ve hangi içerik türlerini olmalıdır kısa bir açıklama ile tamamlandı dizini.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Uygulama araması Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
