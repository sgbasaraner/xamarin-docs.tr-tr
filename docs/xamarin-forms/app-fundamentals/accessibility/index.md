---
title: Xamarin.Forms erişilebilirlik
description: Erişilebilir uygulama oluşturma uygulama gereksinimlerini ve deneyimleri çeşitli kullanıcı arabirimiyle yaklaşımını kişiler tarafından kullanılabilir olmasını sağlar.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 813100acad03684fa9db5343534aee7ca13400c1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241861"
---
# <a name="xamarinforms-accessibility"></a>Xamarin.Forms erişilebilirlik

_Erişilebilir uygulama oluşturma uygulama gereksinimlerini ve deneyimleri çeşitli kullanıcı arabirimiyle yaklaşımını kişiler tarafından kullanılabilir olmasını sağlar._

Xamarin.Forms uygulaması erişilebilir hale getirme düzenini ve çok sayıda kullanıcı arabirimi öğeleri tasarımını düşünmek anlamına gelir. Dikkate alınması gereken sorunlar hakkında yönergeler için bkz: [erişilebilirlik denetim listesi](~/cross-platform/app-fundamentals/accessibility.md). Büyük yazı tipleri ve uygun renk ve karşıtlığı ayarları gibi birçok erişilebilirlik sorunları zaten Xamarin.Forms API'leri tarafından çözülebilir.

[Android erişilebilirlik](~/android/app-fundamentals/accessibility.md) ve [iOS erişilebilirlik](~/ios/app-fundamentals/accessibility.md) kılavuzları Xamarin tarafından sunulan yerel API'lerde ayrıntılarını içeren ve [UWP erişilebilirlik Kılavuzu MSDN'de](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) açıklar yerel bir yaklaşım, platform üzerinde. Bu API'leri erişilebilir uygulamalar her platformda tam olarak uygulamak için kullanılır.

Xamarin.Forms şu anda yok *yerleşik* tüm erişilebilirlik API'leri her temel platformlar için destek. Ancak, bu ayar erişilebilirlik değerlerini ekran okuyucusu ve gezinti yardım araçları desteklemek için kullanıcı arabirimi öğeleri erişilebilir uygulamalar oluşturmanın en önemli parçalarından biri olan destekler. Daha fazla bilgi için bkz: [ayarı erişilebilirlik kullanıcı arabirimi öğeleri değerlerine](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md).

Diğer erişilebilirlik API'leri (gibi [iOS PostNotification](~/ios/app-fundamentals/accessibility.md)) daha iyi için uygun bir [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) veya [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) uygulaması. Bunlar bu kılavuzda ele alınmamıştır.

## <a name="testing-accessibility"></a>Erişilebilirlik test etme

Xamarin.Forms uygulamalar genellikle hedef birden çok platform, erişilebilirlik özellikleri platforma göre sınama anlamına gelir. Her platformda erişilebilirlik test öğrenmek için aşağıdaki bağlantıları izleyin:

- [**iOS test etme**](~/ios/app-fundamentals/accessibility.md)
- [**Android test etme**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)


## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası erişilebilirlik](~/cross-platform/app-fundamentals/accessibility.md)
- [Kullanıcı Arabirimi Öğelerinde Erişilebilirlik Değerlerini Ayarlama](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)
