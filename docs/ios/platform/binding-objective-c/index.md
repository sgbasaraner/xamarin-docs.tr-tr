---
title: "İOS kitaplıkları bağlama"
description: "İOS yerel kitaplıkları (ve CocoaPods) nasıl Xamarin uygulamaları erişilebilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3afe1a03299e600502d49b1db039af4c6642e131
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-ios-libraries"></a>İOS kitaplıkları bağlama

_İOS yerel kitaplıkları (ve CocoaPods) nasıl Xamarin uygulamaları erişilebilir._

Objective-C kitaplıkları ve CocoaPods Xamarin.iOS ve Xamarin.Mac için bağlama hakkında bilgi için bu bağlantıları izleyin:

- [**Genel Bakış** ](~/cross-platform/macios/binding/overview.md) -
  bağlama nasıl çalıştığı açıklanmaktadır.
- [**Objective-C kitaplıkları bağlama** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Objective-C kitaplıkları Xamarin projelerinde kullanılmak bağlamak yönergeler.
- [**Tür tanımı Başvuru Kılavuzu** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  tüm bağlama oluşturma işlemi sürücü bağlama yazarları için kullanılabilir özniteliklerini açıklar.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Amaç Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Amaç Sharpie bağlaması ilk geçişi bootstrap yardımcı olmak için bir komut satırı aracıdır.
Genel API uygulamasına eşlemek için yerel kitaplık üstbilgi dosyaları ayrıştırarak çalışır [tanımı bağlama](~/cross-platform/macios/binding/objective-c-libraries.md) (Aksi halde el ile gerçekleştirilen bir işlem). Amaç Sharpie tek başına bir bağlama oluşturmaz, ancak başlamanıza yardımcı olabilir!

Amaç Sharpie 3.0 Cocoapods doğrudan bağlamak özelliği sunulan!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[İzlenecek yol - iOS Objective-C Kitaplığı bağlama](walkthrough.md)

Bu sayfa, açık kaynak kullanan bir iOS bağlama projesi oluşturma bir adım adım kılavuz sağlar [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) Objective-C projesi bir örnek olarak. **InfColorPicker** kitaplığı, kullanıcının kendi HSB gösterimi üzerinde renk seçim yaparak daha kullanıcı dostu dayalı olarak renk seçmesine izin yeniden kullanılabilir görünüm denetleyicisi sağlar.
Amaç Sharpie bağlama işleminde yardımcı olmak için kullanılır.



## <a name="related-links"></a>İlgili bağlantılar

- [Objective-C bağlama](~/cross-platform/macios/binding/index.md)
- [Mac Binding](~/mac/platform/binding.md)
