---
title: "TvOS 10 giriş"
description: "Bu makalede Xamarin.tvOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve tvOS 10 kullanılabilir özellikler sunar."
ms.topic: article
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8bc379e507287cd609dca8440b1210b7f1514114
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-tvos-10"></a>TvOS 10 giriş

_Bu makalede Xamarin.tvOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve tvOS 10 kullanılabilir özellikler sunar._

Yeni tvOS ile 10 SDK Apple yeni API'ler ve uygulamalar ve özellikler, yeni kategori oluşturmak geliştiricinin Etkinleştirme Hizmetleri eklemiştir. 

Apple'nın tvOS 10 hakkında daha fazla bilgi için lütfen bkz [tvOS + uygulamaları](https://developer.apple.com/tvos/) belgeleri.

## <a name="whats-new-in-tvos-10"></a>Yeni tvOS 10 nedir

Apple, tvOS 10 var olan özellikleri de dahil olmak üzere, birçok geliştirmelerin yanı sıra, birkaç yeni API'ler ve Hizmetleri ekledi:

## <a name="new-user-interface-styles"></a>Yeni kullanıcı arabirimi stilleri

tvOS tüm yapı içinde Uıkit denetler otomatik olarak koyu hem ışık kullanıcı arabirimi tema destekler uyarlamak için artık 10 kullanıcının tercihlerini temel.

Oluştururken ve yeni özel kullanıcı Arabirimi denetimler uygulamak, geliştiricinin kullanması gereken [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection) kullanıcının seçilen tema uyarlamak için sınıf.

Daha fazla bilgi için lütfen bkz bizim [yeni kullanıcı arabirimi stilleri](~/ios/tvos/platform/user-interface-styles.md) belgeleri.

## <a name="security-and-privacy-enhancements"></a>Güvenlik ve gizlilik geliştirmeleri

Apple, güvenlik ve gizlilik tvOS 10 uygulamalarını güvenliğini artırmak ve son kullanıcının gizliliği güvence altına Geliştirici yardımcı olacak bazı geliştirmeler yaptı.

Sonuç olarak, 3 (veya üstü) watchOS üzerinde çalışan uygulamalar içinde gizlilik özel anahtarlarını bir veya daha fazla girerek belirli özellikler veya kullanıcı bilgilerini erişmek için kendi hedefi statik olarak bildirilmelidir kendi `Info.plist` neden uygulamanın istediği erişmek için kullanıcı açıklayan dosyaları.

Bizim iOS 10 tvOS 10 bu değişiklikler ile iOS 10 paylaşımları olduğundan, lütfen bkz [güvenlik ve gizlilik geliştirmeler](~/ios/app-fundamentals/security-privacy.md) daha fazla bilgi için Kılavuzu.

## <a name="video-subscriber-account"></a>Video abone hesabı

Yeni tvOS 10 Video abone hesap framework uygulamalarının, kimlik doğrulaması desteği akış veya son kullanıcı için bir tek oturum açma deneyimi kullanarak, kablo veya uydu TV sağlayıcıları, kimlik doğrulaması için isteğe bağlı video sağlar.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>Geniş rengi

tvOS 10 genişletilmiş aralığı piksel biçimleri ve çekirdek grafikleri, çekirdek görüntü, Metal ve AVFoundation gibi çerçeveleri dahil olmak üzere Sistem genelindeki wide gam renk alanları desteği genişletir. Geniş renk görüntüler ile cihazları için destek Bu davranış tüm grafik yığını boyunca sağlayarak daha fazla hızının.

Ayrıca, `UIKit` değiştirildi yeni çalışmak için Genişletilmiş **sRGB** renk uzayı, geniş renk gamutları birbirinden önemli performans kaybı olmadan renkleri karışık kolaylaştırır.

Apple geniş renklerle çalışırken aşağıdaki en iyi yöntemleri sunar:

 - `UIColor` sRGB Renk aralığı ve artık kullanır değerlere clamp artık `0.0` için `1.0` aralık. Uygulamanın önceki clamp davranışı dayalıysa, 10 tvOS için değiştirilmesi gerekir.
 - Uygulama özel çizmeye gerçekleştirirse `UIImages`, yeni [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) genişletilmiş aralığı veya standart aralık biçimleri kullanımını belirlemek için sınıf.
 - Görüntü işleme sağlamak için çekirdek grafikleri veya Metal gibi düşük düzey API'si kullanılarak uygulama 16 bit kayan nokta değerlerine destekleyen bir genişletilmiş aralık renk alanını ve piksel biçimi kullanmalıdır. Gerektiğinde, uygulamayı el ile renk bileşeni değerleri clamp gerekecektir.
 - Çekirdek grafikleri, çekirdek görüntü ve Metal performans gölgelendiriciler tüm iki renk alanları arasında dönüştürme için yeni yöntemler sağlar.

Daha fazla bilgi için lütfen bkz bizim [geniş renk giriş](~/ios/platform/wide-color.md) Kılavuzu.

## <a name="newly-available-existing-frameworks"></a>Yeni kullanılabilir varolan çerçeveler

İOS (ve değil tvOS), kullanılabilir birkaç çerçeveleri yapıldı tvOS 10 kullanılabilir gibi:

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - Fotoğraf
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>Ek Framework değişiklikleri

Ana framework değişiklikler ve eklemeler yukarıda listelenen ek olarak, Apple tvOS 10 birçok ek küçük framework değişiklikler yaptı.

Daha fazla bilgi için lütfen bkz bizim [ek Framework değişiklikler](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) Kılavuzu.

## <a name="deprecated-apis"></a>Kullanım dışı API'leri

Herhangi bir API veya çerçeveler 10 tvOS tarafından kullanım dışı bırakılan. Apple'nın bkz [tvOS 10 API farklılıkları](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) tam listesi için belgelere API değişiklikler.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
