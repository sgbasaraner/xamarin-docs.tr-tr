---
title: "İTunes bağlan, tvOS uygulama yapılandırma"
description: "Bu makalede yapılandırma iTunes Bağlan tvOS belirli yapılandırmalar için uygulamanızda iOS için ek bir kılavuz sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5db53bef0f62937f7be0a5e5fb6f64f1bf3ca007
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>İTunes bağlan, tvOS uygulama yapılandırma

_Bu makalede yapılandırma iTunes Bağlan tvOS belirli yapılandırmalar için uygulamanızda iOS için ek bir kılavuz sağlar._


Yapılandırmaları ve iOS izleyerek yapmanız gerekecektir ayarı ek olarak [uygulamanızı iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) Kılavuzu, bu belgede kapsayan bir Xamarin.tvOS serbest bırakmak için gerekli olan belirli yapılandırmalar uygulamayı Apple TV uygulama Mağazası'nda.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>TvOS yayın sürümü ekleme

Apple TV App Store'da yayımlanan için yeni bir uygulama oluşturma veya varolan bir iOS uygulamasının Apple TV desteği ekleme, bir iTunes Connect kaydı oluşturduktan ve yapılandırdıktan gerekir aşağıdaki iOS kullanarak özel sırasında size kılavuzluk eder:

- [Bir iTunes Connect kaydı oluşturma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Uygulama videolar ve ekran görüntüleri yönetme](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Ad, açıklama, yenilikler, yönetme anahtar sözcükleri ve URL'leri](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Genel bilgi koruma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

Ayrıca isteğe bağlı olarak, gerektirebilir:

- [Oyun merkezi bilgi koruma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Uygulama içi satın alma bilgileri koruma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

Tüm tamamlanan yukarıdaki adımları, açın, uygulamanızın iTunes Connect kaydı ve sol kenar çubuğu'nu kullanarak tvOS desteği eklemek için seçin:

[ ![](itunes-connect-images/connect01.png "Sol Kenar Çubuğu'nu kullanarak tvOS desteği ekleme")](itunes-connect-images/connect01.png)

TvOS belirli bilgiler ekranlar sonra verilen iTunes Connect kaydı için kullanılabilir:

[ ![](itunes-connect-images/connect02.png "TvOS belirli bilgileri ekranı")](itunes-connect-images/connect02.png)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>tvOS sürüm bilgileri

Sol Kenar Çubuğu'ndan seçin **gönderme 1.0 hazırlanma** tvOS uygulama bölümünün altında:

[ ![](itunes-connect-images/connect03.png "tvOS sürüm bilgileri")](itunes-connect-images/connect03.png)

Bu ekranda aşağıdaki bilgileri sağlayın:

- Gerekli ekran görüntüleri, açıklama, anahtar sözcükleri ve URL'leri.
- Sürüm numarasını ve telif hakkı yaş derecelendirme gibi genel uygulama bilgileri.
- İsteğe bağlı uygulama içi satın almalara.
- İsteğe bağlı Game Center desteği liderlik ve başarılar ile.
- İlgili kişi, tanıtım hesapları ve notlar gibi uygulama gözden geçirme bilgi gerekiyor.

Gerekli bilgileri girdikten sonra tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için ekranın sağ üst köşesinde düğmesini:

[ ![](itunes-connect-images/connect04.png "tvOS sürüm bilgileri gönderme için hazır")](itunes-connect-images/connect04.png)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>Gözden geçirilmek üzere göndermek hazırlama

Apple TV App Store gözden geçirme için Xamarin.tvOS uygulamanıza göndermek son hazır olduğunuzda, uygulamanın iTunes Connect kaydı dönün ve tıklatın **göndermek için gözden geçirme** ekranın üst sağ alt köşesindeki düğmesini:

[ ![](itunes-connect-images/connect05.png "Gözden geçirme için Gönder")](itunes-connect-images/connect05.png)

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede Apple TV uygulama mağazası tvOS uygulama yayımlamayı iTunes Bağlan gerekli tvOS belirli bir ayar genel bakış verdi.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
