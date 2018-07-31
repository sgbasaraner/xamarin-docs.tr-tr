---
title: Uygulama içinde Xamarin.ios'ta satın alma
description: Bu belge, dijital ürün ve hizmetlerini StoreKit API'lerini kullanarak satış açıklar. Bu yapılandırma, tüketilebilir ürünler, tüketilebilir olmayan ürünler, işlemler, abonelikleri ve daha birçok konuda kılavuzlara bağlar.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 102ff2f11cc2f3d536e3ce9dd595a881f370f764
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351427"
---
# <a name="in-app-purchasing-in-xamarinios"></a>Uygulama içinde Xamarin.ios'ta satın alma

iOS uygulamaları, dijital ürün veya hizmetler StoreKit – Apple sunucuları ile iletişim kuran iOS tarafından sağlanan API'ler aracılığıyla Apple kimlikleri kullanıcıyla Finansal işlemler gerçekleştirmek için bir dizi kullanarak satabilirsiniz. StoreKit API'leri öncelikle işlemleri gerçekleştirme ve ürün bilgilerini alma ile ilgili: kullanıcı arabirimi bileşen yok. Uygulama içi satın alma kullanan uygulamalar, kendi kullanıcı arabirimi oluşturma ve kullanıcıya gerekli ürünleri veya hizmetleri sağlamak için özel kod ile satın alınan öğeleri izler.

Uygulama içi satın alma işlevsellik sağlayan birkaç adımı gerektirir:

-  **Uygulamanızı yapılandırma** – uygulama sağlama profili Kurulum doğru olması gerekir.
-  **Ürün oluşturma** – iTunes Connect Portalı'nda ürün açıklamalarını ve fiyatlarını oluşturulmalıdır.
-  **StoreKit uygulama** – türde Satılan ürünler göre StoreKit API uygulanır.
-  **Kullanıcı arabirimi ve ürünler oluşturmak** – mekanizmaları her satın izlemek ve yedekleme/bunları uygunsa, geri yükleme dahil olmak üzere ürünler uygulanmalıdır.
-  **Satış izleme ve fon alma** – satış eğilimlerini izleyin ve sizin geliri izlemek için iTunes Connect tarafından sağlanan bilgileri kullanın.

Bu belgede, Xamarin.iOS kullanarak uygulama içi Satınalmalar sağlamak için tüm adımları tamamlamak açıklanmaktadır.

## <a name="requirements"></a>Gereksinimler

Uygulama içi satın alma desteği için Xcode 7 ve üzeri Xamarin.iOS 5.0 ve üzeri sürümlerde kullanmanız gerekir.

## <a name="contents"></a>İçindekiler

 * [Temel Uygulama İçi Satın Alma Bilgileri ve Yapılandırması](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [StoreKit genel bakış ve ürün bilgileri alınırken](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [Tüketilebilir Ürünler Satın Alma](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [Tüketilebilir Olmayan Ürünler Satın Alma](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [İşlemler ve Doğrulama](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [Abonelikler ve Raporlama](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>Özet

Bu makalede uygulama içi satın alma kavramın tanıtımını yararlanmak için uygulamanızı yapılandırma konusunda özetlenen ve Xamarin.iOS kullanımı örnekleri gösterilir. Bu kapsam dahilinde:

-  **iOS sağlama portalı** – uygulama etkinleştirmek için yönergeleri işlevsellik satın.
-  **iTunes Connect** – uygulamanızın satmak Ürünleri yapılandırılıyor.
-  **Kit Store** – uygulama içi satın alma özellikleri oluşturmak için kullanılan sınıflar açıklaması.
-  **Satın almak için uygulamanızı kodlama** – uygulama içi satın alma bir Xamarin.iOS uygulaması oluşturma örnekleri.
-  **Raporlama** – iTunes Connect kullanılabilir İstatistikleri'ne genel bakış.


## <a name="related-links"></a>İlgili bağlantılar

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [Uygulama satın alma Programlama Kılavuzu'nda](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect Geliştirici Kılavuzu](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Store Seti Framework başvurusu](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Uygulama içi satın alma ürün tanımlayıcıları soru- cevap](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Uygulama içi satın alma Teknik Not](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [İlk App Store gönderiminiz](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [App Store Kaynak Merkezi](https://developer.apple.com/appstore/index.html)
- [App Store gönderimi ipuçları](https://developer.apple.com/appstore/resources/submission/tips.html)
- [App Store gözden geçirme yönergeleri](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Uygulamalarınızı yönetme](https://developer.apple.com/appstore/resources/managing/index.html)
