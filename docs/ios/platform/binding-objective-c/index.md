---
title: İOS kitaplıklarını bağlama
description: Bu belgede, yerel kitaplıkları ve CocoaPods içinde bir Xamarin.iOS uygulaması kullanmak olası hale getirerek C# bağlamaları Objective-C kodu oluşturmayı açıklar.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 75c8f9a2eea080c3da031366b314d94929080811
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855227"
---
# <a name="binding-ios-libraries"></a>İOS kitaplıklarını bağlama

Objective-C kitaplıklarını ve CocoaPods Xamarin.iOS ve Xamarin.Mac bağlama hakkında bilgi edinmek için bu bağlantıları izleyin:

- [**Genel Bakış** ](~/cross-platform/macios/binding/overview.md) -
  bağlamanın nasıl çalıştığı açıklanır.
- [**Objective-C kitaplıklarını bağlama** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Xamarin projeleri kullanmak için Objective-C kitaplıklarını bağlama konusunda yönergeler.
- [**Tür tanımı Başvuru Kılavuzu** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  tüm bağlama oluşturma işlemi sürücü bağlama yazarları için kullanılabilir özniteliklerini açıklar.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Amaç Sharpie bağlama ilk geçişinde bootstrap yardımcı olmak için bir komut satırı aracıdır.
Genel API uygulamasına eşlemek için yerel bir kitaplığı üstbilgi dosyaları ayrıştırarak çalıştığını [tanımı bağlama](~/cross-platform/macios/binding/objective-c-libraries.md) (Aksi halde el ile gerçekleştirilen bir işlem). Amaç Sharpie tek başına bir bağlama oluşturmaz, ancak başlamanıza yardımcı olabilir!

Amaç Sharpie 3.0 Cocoapods doğrudan bağlama olanağı sundu!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[İzlenecek yol - bir iOS Objective-C kitaplığını bağlama](walkthrough.md)

Bu sayfa, açık kaynak kullanan bir iOS bağlama projesi oluşturma adım adım bir kılavuz sağlar [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) örnek olarak Objective-C projesi. **InfColorPicker** kitaplığı, renk seçimi daha kullanışlı hale getirme HSB gösterimine alarak bir rengi seçmesini sağlayan bir yeniden kullanılabilir görünüm denetleyicisi sağlar.
Amaç Sharpie bağlama işleminde yardımcı olmak için kullanılır.

## <a name="xamarin-university-lightning-lecture"></a>Xamarin University ışık Ders

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS bağlamaları C/C++ ' ta tarafından [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>İlgili bağlantılar

- [Objective-C’yi Bağlama](~/cross-platform/macios/binding/index.md)
- [Mac bağlama](~/mac/platform/binding.md)
- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)