---
title: "Uygulama içi satın alma"
description: "iOS uygulamaları, dijital ürün ve hizmetlerini deposu Seti API'lerini kullanarak satmak. Ürünler oluşturulur ve iTunes Bağlan Portal'da yönetilir. Apple işlem yönetir ve bunların satmak önce tüm ürünleri onaylar ve her bir işleme (şu anda % 30) için bir ücret ister. Apple uygulamanızda dijital satış için uygulama içi satın kullanır, ancak fiziksel mal ve sayısal olmayan hizmetleri satışlarının kullanamazsınız gerektirir. Dijital Ürün ve Hizmetleri için alternatif ödeme seçenekleri sunan reddedilmesi büyük olasılıkla uygulamalardır. Bu belge, uygulamanızı mağaza Seti kullanacak şekilde yapılandırılmasını açıklamaktadır ve en yaygın uygulama içi satın alma senaryoları Xamarin.iOS örnekler sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: af8eb556215679bab2da8f54e8231f7d7d3ed418
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="in-app-purchasing"></a>Uygulama içi satın alma

_iOS uygulamaları, dijital ürün ve hizmetlerini deposu Seti API'lerini kullanarak satmak. Ürünler oluşturulur ve iTunes Bağlan Portal'da yönetilir. Apple işlem yönetir ve bunların satmak önce tüm ürünleri onaylar ve her bir işleme (şu anda % 30) için bir ücret ister. Apple uygulamanızda dijital satış için uygulama içi satın kullanır, ancak fiziksel mal ve sayısal olmayan hizmetleri satışlarının kullanamazsınız gerektirir. Dijital Ürün ve Hizmetleri için alternatif ödeme seçenekleri sunan reddedilmesi büyük olasılıkla uygulamalardır. Bu belge, uygulamanızı mağaza Seti kullanacak şekilde yapılandırılmasını açıklamaktadır ve en yaygın uygulama içi satın alma senaryoları Xamarin.iOS örnekler sağlar._


iOS uygulamaları dijital ürünleri veya hizmetleri StoreKit – Apple sunucuları ile iletişim kuran iOS tarafından sağlanan API'leri aracılığıyla kendi Apple kimliği kullanıcıyla Finansal işlemler gerçekleştirmek için bir dizi kullanarak satabilir miyim StoreKit API'ları ürün bilgilerini ve alma işlemleri gerçekleştirme ile öncelikle ilgilenen – hiç kullanıcı arabirimi bileşeni yoktur. Uygulama içi satın alma kullanan uygulamalar, kendi kullanıcı arabirimi oluşturma ve kullanıcı için gerekli ürünleri veya hizmetleri sağlamak için özel kod ile satın alınan öğeleri izleme gerekir.

Uygulama içi satın alma işlevsellik sağlayan bir dizi adımı gerektirir:

-  **Uygulamanızı yapılandırmaya** – uygulama sağlama profili Kurulum doğru olması gerekir.
-  **Ürün oluşturma** – ürün tanımları ve fiyatlarını iTunes Bağlan Portalı'nda oluşturulmalıdır.
-  **StoreKit uygulama** – Satılan ürünler türlerine göre StoreKit API uygulanmadı.
-  **Kullanıcı arabirimi ve ürünleri kendileri derleme** – her satın alma izlemek ve yedekleme/bunları uygunsa, geri yükleme için mekanizmaları dahil olmak üzere ürünler uygulanmalıdır.
-  **Satış izleme ve fon alma** – satış eğilimlerini izlemek ve, geliri izlemek için Bağlan iTunes tarafından sağlanan bilgileri kullanın.


Bu belge, Xamarin.iOS kullanarak uygulama içi satın alımlar sağlamak için tüm bu adımları tamamlamak açıklanmaktadır.


## <a name="requirements"></a>Gereksinimler

Uygulama içi satın alma desteklemek için Xcode 7 ve üstünde 5.0 veya daha yeni bir Xamarin.iOS kullanmanız gerekir.

## <a name="contents"></a>İçindekiler

 * [Uygulama içi satın alma temelleri ve yapılandırma](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [Mağaza Paketi'ne Genel Bakış ve yazdırılacak ürün bilgileri](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [Kaynaklarda ürünleri satın alma](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [Tüketilemezi ürünleri satın alma](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [İşlemleri ve doğrulama](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [Abonelikler ve Raporlama](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)


## <a name="summary"></a>Özet

Bu makalede olan uygulama içi satın alma kavramı sunulan, onu yararlanmak için uygulamanızın yapılandırma konusunda özetlenen ve Xamarin.iOS kullanan örnekler sunulan. Bunu ele:

-  **iOS sağlama portalı** – uygulama etkinleştirme yönergeleri işlevselliği satın alın.
-  **iTunes Bağlan** – uygulamanızda satmak Ürünleri yapılandırılıyor.
-  **Kit depolamak** – uygulama içi satın alma özellikleri oluşturmak için kullanılan sınıflar açıklaması.
-  **Satın almak için uygulamanızı kodlama** – uygulama içi satın alma bir Xamarin.iOS uygulaması oluşturma örnekleri.
-  **Raporlama** – Bağlan iTunes kullanılabilir istatistikleri genel bakış.


## <a name="related-links"></a>İlgili bağlantılar

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [Uygulama içi satın alma Programlama Kılavuzu](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Bağlan Geliştirici Kılavuzu](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Mağaza Seti Framework başvurusu](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Uygulama içi satın alma ürün tanımlayıcıları soru- cevap](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Teknik Not uygulama içi satın alma](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [İlk App Store gönderiminiz](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [Uygulama mağazası Kaynak Merkezi](https://developer.apple.com/appstore/index.html)
- [Uygulama mağazası gönderimi ipuçları](https://developer.apple.com/appstore/resources/submission/tips.html)
- [App Store gözden geçirme yönergeleri](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Uygulamalarınızı yönetme](https://developer.apple.com/appstore/resources/managing/index.html)
