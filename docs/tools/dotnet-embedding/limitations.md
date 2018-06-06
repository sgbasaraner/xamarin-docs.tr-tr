---
title: .NET katıştırma sınırlamaları
description: Bu belgede .NET katıştırma, diğer programlama dillerinde .NET kodu kullanmasına olanak tanıyan aracı sınırlamaları açıklanmaktadır.
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: fdd3ac4cd57ac7f79f9071d62e758625b30f05dd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794130"
---
# <a name="net-embedding-limitations"></a>.NET katıştırma sınırlamaları

Bu belge, .NET katıştırma sınırlamaları açıklar ve mümkün olduğunda, geçici çözümler kendileri için sağlar.

## <a name="general"></a>Genel

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Bir projede birden fazla katıştırılmış kitaplığını kullanma

Aynı uygulama içinden birlikte var olan iki Mono çalışma zamanları olması olası değil. Bu, aynı uygulama iki farklı .NET katıştırma üretilmiş kitaplık kullanamayacağınız anlamına gelir.

**Geçici çözüm:** oluşturucunun birkaç derlemelerden (farklı projelere) içeren tek bir kitaplığı oluşturmak için kullanabilirsiniz.

### <a name="subclassing"></a>Alt sınıf yapma

.NET katıştırma, bir dizi hedef dil ve platformu için kullanıma hazır API göstererek Mono çalışma zamanı uygulamaları içinde tümleştirmesini kolaylaştırır.

Ancak bu iki yönlü bir tümleştirmesi değildir, örneğin bir yönetilen türü bir alt edemez ve yönetilen kod bu ortak varlığını denetle farkında olduğundan, yerel kod geri çağırmaya yönetilen kod beklediğiniz.

Gereksinimlerinize bağlı olarak, örneğin bu sınırlamaya geçici çözüm bölümlerine mümkün olabilir

* yönetilen kod yerel kod içine olabilir p/Invoke. Bu, yerel koddan özelleştirmeye izin verecek şekilde, yönetilen kodu özelleştirme gerektirir;

* Objective-C (Bu örnekte) bir alt kümesi için bazı NSObject alt sınıfların yönetilen belirleyebilmesini yönetilen bir kitaplık kullanıma ve Xamarin.iOS gibi ürün kullanın.

## <a name="objective-c-generated-code"></a>Objective-C oluşturulan kod

### <a name="nullability"></a>Null atanabilirlik

Bir null başvuru kabul edilebilir olup olmadığını bize .NET veya bir API için meta veri yoktur. Çoğu API'leri atar `ArgumentNullException` başa edilemez ise bir `null` bağımsız değişkeni. Özel durumlar Objective-C işlenmesini kaçınılması daha iyi bir şey olduğundan bu sorunlu oluşturur.

Biz doğru null atanabilirlik ek açıklamaları üstbilgi dosyaları oluşturmak ve yönetilen özel durumlar en aza indirmek isterseniz bu yana biz null olmayan bağımsız değişkenler için varsayılan (`NS_ASSUME_NONNULL_BEGIN`) ve bazı özel duyarlık mümkün olduğunda, null atanabilirlik ek açıklamalar ekleyin.

### <a name="bitcode-ios"></a>Bitcode (iOS)

Şu anda .NET katıştırma bitcode'u bazı Xcode proje şablonları etkin iOS desteklemiyor. Bu, başarılı bir şekilde oluşturulan bağlantı çerçeveleri devre dışı bırakılması gerekir.

![Bitcode'u seçeneği](images/ios-bitcode-option.png)
