---
title: .NET katıştırma sınırlamaları
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: d8995946966020955a1d9378dea631387ed5f4bd
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="net-embedding-limitations"></a>.NET katıştırma sınırlamaları

Bu belge, .NET katıştırma (Embeddinator-4000) sınırlamaları açıklar ve mümkün olduğunda, geçici çözümler kendileri için sağlar.

## <a name="general"></a>Genel

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Bir projede birden fazla katıştırılmış kitaplığını kullanma

Aynı uygulama içinden birlikte var olan iki mono çalışma zamanları olması olası değil. Bu, aynı uygulama iki farklı embeddinator 4000 oluşturulan kitaplıkları kullanamayacağınız anlamına gelir.

**Geçici çözüm:** oluşturucunun birkaç derlemelerden (farklı projelere) içeren tek bir kitaplığı oluşturmak için kullanabilirsiniz.

### <a name="subclassing"></a>Alt sınıf yapma

Embeddinator bir dizi hedef dil ve platformu için kullanıma hazır API göstererek mono çalışma zamanı uygulamaları içine tümleştirmeyi kolaylaştırır.

Ancak bu bir iki yönlü tümleştirme değildir, örneğin bir yönetilen türü bir alt edemez ve yönetilen kod bu ortak varlığını denetle farkında olduğundan, yerel kod geri çağırmaya yönetilen kod beklediğiniz.

Gereksinimlerinize bağlı olarak, örneğin bu sınırlamaya geçici çözüm bölümlerine mümkün olabilir

* yönetilen kod yerel kod içine olabilir p/Invoke. Bu, yerel koddan özelleştirmeye izin verecek şekilde, yönetilen kodu özelleştirme gerektirir;

* (Bu örnekte) için bir alt ObjC bazı yönetilen NSObject alt sınıfların olanak tanır yönetilen bir kitaplık kullanıma ve Xamarin.iOS gibi ürün kullanın.


## <a name="objc-generated-code"></a>Oluşturulan ObjC kodu

### <a name="nullability"></a>Null atanabilirlik

.NET içinde hiçbir meta veri olan, bir null başvuru kabul edilebilir mi olduğunu, yoksa bir API için bize. Çoğu API'leri atar `ArgumentNullException` başa edilemez ise bir `null` bağımsız değişkeni. Özel durumlar ObjC işlenmesini kaçınılması daha iyi bir şey olduğundan bu sorunlu oluşturur.

Biz doğru null atanabilirlik ek açıklamaları üstbilgi dosyaları oluşturmak ve yönetilen özel durumlar en aza indirmek isterseniz bu yana biz null olmayan bağımsız değişkenler için varsayılan (`NS_ASSUME_NONNULL_BEGIN`) ve bazı özel duyarlık mümkün olduğunda, null atanabilirlik ek açıklamalar ekleyin.
