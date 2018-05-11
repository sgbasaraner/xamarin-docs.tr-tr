---
title: Platform uygulamaları oluşturma
description: Bu bölüm, özeti artı altı bölümleri mobil uygulamalar tasarlama ve test etme ve çeşitli uygulama mağazaları dağıtma için Xamarin nasıl çalıştığını anlamak gelen Xamarin geliştirme platformu – kullanarak uygulamalarının nasıl oluşturulacağını anlatır.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: fba13ab921949cd2361e78535d5ffc96952a1336
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="sharing-code-options"></a>Paylaşım kodu seçenekleri

Platformlar arası mobil uygulamalar arasında kod paylaşmak için iki seçenek vardır: paylaşılan varlık projeleri ve taşınabilir sınıf kitaplıkları. Bu seçenekler [burada tartışılan](~/cross-platform/app-fundamentals/code-sharing.md); hakkında daha fazla bilgi [taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md) ve [paylaşılan projeleri](~/cross-platform/app-fundamentals/shared-projects.md) da kullanılabilir.

<a name="Sections" />

## <a name="building-cross-platform-mobile-apps"></a>Platformlar arası mobil uygulamaları oluşturma

 [Genel bakış](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [Bölüm 1 – Xamarin mobil Platform anlama](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Bölüm 2 – mimarisi](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [Bölüm 3 – Xamarin Çapraz Platform çözümü ayarladıktan](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [Bölüm 4 – ile birden çok platform ele alma](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [5 – stratejileri paylaşımı pratik kod parçası](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [Bölüm 6 - Test Etme ve App Store Onayları](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

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
