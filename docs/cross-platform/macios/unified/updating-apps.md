---
title: Birleşik API için var olan uygulamaları güncelleştirme
description: Bu belge bağlantılar çeşitli kılavuzlara Unified API Xamarin uygulamaları güncelleştirmek açıklar. Xamarin.iOS uygulamaları, Xamarin.Mac uygulamalar açıklanmaktadır. Xamarin.Forms uygulamalar, platformlar arası uygulamalar ve bağlama projeleri içindeki yerel türler.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d09be7b85980e5c5a8eb209dc1b4ff3136c34b3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781637"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Birleşik API için var olan uygulamaları güncelleştirme

> [!IMPORTANT]
> Xamarin Klasik hangi Unified API öncesinde, API, kullanım dışı bırakıldı. 
> - Klasik API (monotouch.dll) desteklemek için Xamarin.iOS son sürümü Xamarin.iOS 9.10 oluştu.
> - Xamarin.Mac hala Klasik API destekler, ancak artık güncelleştirilir. Kullanım dışı bırakılmıştır olduğundan, geliştiriciler uygulamalarını Unified API taşımanız gerekir.

## <a name="how-to-update-your-apps"></a>Uygulamaları güncelleştirme

Xamarin University sahip ücretsiz kullanılabilir video **iOS Unified API yükseltme**. Ziyaret [Xamarin University Şimşek dersler](http://university.xamarin.com/lightninglectures) izlemek için!

[ ![](updating-apps-images/xamu-video-sml.png "Xamarin University")](http://university.xamarin.com/lightninglectures)

Uygulamalarınızı güncelleştirmek için üç adım vardır:

1. Derleyici Uyarısı mevcut kodunuzu özellikle kullanım dışı API'lerine ilgili düzeltin.

2. Proje dosyaları ve ad alanlarını güncelleştirmek için Mac için Visual Studio içinde yerleşik geçiş aracını kullanın.

3. Yeni ilgili derleyici hataları kalan düzeltme [64 türleri](~/cross-platform/macios/nativetypes.md) ve [diğer API'leri](~/cross-platform/macios/unified/overview.md#deprecated-typos) , değişti. Kullanıma [bu ipuçlarını](~/cross-platform/macios/unified/updating-tips.md) gerekebilecek el ile güncelleştirme hakkında daha fazla bilgi için.

64-bit desteği ve birleşik API uygulamalarınızı güncelleştirmenize yardımcı olması her ürün için kullanılabilen belirli kılavuzları vardır:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.iOS uygulamaları](~/cross-platform/macios/unified/updating-ios-apps.md)

Varolan Xamarin.iOS uygulamaları için Visual Studio Mac için yerleşik otomatik geçiş aracını kullanarak birleşik API güncelleştirmek için Bazı ek düzeltmeler sonra açıklandığı şekilde, gerekebilecek [bu yönergeleri](~/cross-platform/macios/unified/updating-ios-apps.md) ve [ipuçları](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac uygulamalar](~/cross-platform/macios/unified/updating-mac-apps.md)

Var olan Xamarin.Mac uygulamaları birleşik Mac için Visual Studio için yerleşik otomatik geçiş aracını kullanarak API güncelleştirmek için Bazı ek düzeltmeler sonra açıklandığı şekilde, gerekebilecek [bu yönergeleri](~/cross-platform/macios/unified/updating-mac-apps.md) ve [ipuçları](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms uygulamalar](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Mevcut bir Xamarin.Forms çözümünü Unified API kullanmak için bir iOS projesi güncelleştirmek için bu yönergeleri izleyin. Birleşik API desteği yalnızca Xamarin.Forms 1.3 kullanılabilir ve daha sonra böylece [yönergeleri](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) de Xamarin.Forms uygulamanızı 1.3 sürüme güncelleştirmek açıklanmaktadır. Bunlar [ipuçları](~/cross-platform/macios/unified/updating-tips.md) özel oluşturucu veya bağımlılık Hizmetleri tüm yerel iOS kodunda güncelleştirme yardımcı olabilir.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Platformlar Arası Uygulamalarda Yerel Türlerle Çalışma](~/cross-platform/macios/nativetypes.md)

Bu makalede, Android veya Windows Phone işletim sistemleri gibi iOS olmayan cihazlara sahip code paylaşıldığı bir platformlar arası uygulamasında yeni iOS (nint, nuint, nfloat) birleşik API yerel türler kullanma yer almaktadır. Yerel türler ne zaman kullanılmalı içine bilgiler sağlar ve yeni türü platformlar arası koduyla burada kullanılmalıdır durumlarda birkaç olası çözümleri sağlar.

## <a name="update-bindings-to-the-unified-api"></a>Birleşik API bağlamalarını güncelleştirin

Objective-C kitaplıklarına bağlamaları oluşturmuş müşteriler (burada bazı türleri şimdi 64-bit olacaktır) temel alınan API değişiklikleri yansıtmak üzere bağlama proje güncelleştirmeniz gerekecektir.
Bu yönergeleri izleyin [varolan bağlama Unified API desteklemek için projesini güncelleştirme](~/cross-platform/macios/unified/update-binding.md).

## <a name="related-links"></a>İlgili bağlantılar

- [İOS uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Güncelleştirme bağlamaları](~/cross-platform/macios/unified/update-binding.md)
- [İpuçları güncelleştiriliyor](~/cross-platform/macios/unified/updating-tips.md)
- [Klasik vs Unified API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
