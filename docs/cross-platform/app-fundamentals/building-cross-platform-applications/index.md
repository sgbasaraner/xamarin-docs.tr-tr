---
title: "Platform uygulamaları oluşturma"
description: "Bu bölüm, özeti artı altı bölümleri mobil uygulamalar tasarlama ve test etme ve çeşitli uygulama mağazaları dağıtma için Xamarin nasıl çalıştığını anlamak gelen Xamarin geliştirme platformu – kullanarak uygulamalarının nasıl oluşturulacağını anlatır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: 7934738a546a266036573b81e15ef9b2fa28d7b4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="sharing-code-options"></a>Paylaşım kodu seçenekleri

Platformlar arası mobil uygulamalar arasında kod paylaşmak için iki seçenek vardır: paylaşılan varlık projeleri ve taşınabilir sınıf kitaplıkları. Bu seçenekler [burada tartışılan](~/cross-platform/app-fundamentals/code-sharing.md); hakkında daha fazla bilgi [taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md) ve [paylaşılan projeleri](~/cross-platform/app-fundamentals/shared-projects.md) da kullanılabilir.

<a name="Sections" />

## <a name="building-cross-platform-mobile-apps"></a>Platformlar arası mobil uygulamaları oluşturma

 [Genel bakış](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-0-overview.md)

 [Bölüm 1 – Xamarin mobil Platform anlama](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-1-understanding-the-xamarin-mobile-platform.md)

 [Bölüm 2 – mimarisi](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-2-architecture.md)

 [Bölüm 3 – Xamarin Çapraz Platform çözümü ayarladıktan](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-3-setting-up-a-xamarin-cross-platform-solution.md)

 [Bölüm 4 – ile birden çok platform ele alma](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-4-platform-divergence-abstraction-divergent-implementation.md)

 [5 – stratejileri paylaşımı pratik kod parçası](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-5-practical-code-sharing-strategies.md)

 [Bölüm 6 - Test Etme ve App Store Onayları](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-6-testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />


## <a name="case-studies"></a>Örnek olay incelemeleri

Bu belgede özetlenen ilkeleri örnek uygulama uygulamada içine konur *Tasky*, yanı [uygulamaları önceden oluşturulmuş](https://xamarin.com/prebuilt) gibi [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm).

 <a name="Tasky" />


### <a name="tasky"></a>Tasky

Tasky, iOS, Android ve Windows Phone için bir basit bir Yapılacaklar listesi uygulamasıdır.
Xamarin ile platformlar arası uygulaması oluşturma temellerini gösterir ve yerel bir SQLite veritabanı kullanır.

 [![tasky listesi](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky listesi](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Okuma [Tasky örnek olay incelemesi](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).


## <a name="summary"></a>Özet

Bu bölümde, Xamarin'ın uygulama geliştirme araçları sunar ve hedef birden çok mobil Platform uygulamalarının nasıl oluşturulacağını açıklar.

Katmanlı mimari bu yapıları kodu yeniden kullanım için birden çok platform genelinde kapsar ve bu mimarisi içinde kullanılabilir farklı yazılım desenleri açıklar.

Örnekler (gibi dosya ve ağ işlemlerini) ortak uygulama işlevleri ve nasıl platformlar arası şekilde oluşturulabilir verilir.

Son olarak, bunu kısaca sınaması açıklanır ve bu ilkeler eyleme yerleştirir bir vaka başvuru sağlar.



## <a name="related-links"></a>İlgili bağlantılar

- [Paylaşım kodu seçenekleri](~/cross-platform/app-fundamentals/code-sharing.md)
- [Örnek Olay İncelemesi: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky örnek uygulaması (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Xamarin mobil uygulama geliştirme: Platformlar arası C# ve Xamarin.Forms Temelleri](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/))
- [C# Greg tarafından ile mobil geliştirme (O'Reilly) Shackles](http://shop.oreilly.com/product/0636920024002.do)
- [C# ' tan Olson, John Hunter, Ben Horgen, Kenny Fantastik (Wrox) tarafından profesyonel platformlar arası Mobil Geliştirme](http://www.wiley.com/WileyCDA/WileyTitle/productCd-1118157702.html)
