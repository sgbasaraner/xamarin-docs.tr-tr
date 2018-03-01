---
title: Xamarin.Mac performans
description: "Bu belge Xamarin.Mac uygulamalar için bazı başarım düşünceleri sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 54B07DED-FDF2-49B2-A5FB-3A9357E65922
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: a10e423251a0e172722d254896fd4410dcf496b4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinmac-performance"></a>Xamarin.Mac performans

## <a name="overview"></a>Genel Bakış

Xamarin.Mac uygulamaları Xamarin.iOS için benzer ve birçok aynı performans önerileri geçerlidir:

- [Xamarin.iOS performans](~/ios/deploy-test/performance.md)
- [Platformlar arası performansı](~/cross-platform/deploy-test/memory-perf-best-practices.md)

Ancak yararlı olabilecek macOS belirli öneriler sayısı vardır.

## <a name="prefer-modern-target-framework"></a>Modern hedef framework tercih

Vardır birden çok [hedef çerçeveyi](~/mac/platform/target-framework.md) Xamarin.Mac uygulamayla farklı performans özellikleri ve özellikler için kullanılabilir.

Mümkün olduğunda, Modern tercih ve desteği eklemek için bağımlı kitaplıkları ile çalışabilirsiniz. Yalnızca Modern hedef Framework hangi derleme boyutları büyük ölçüde azaltabilir bağlama sağlar. Uygulama Nesne AĞACI derleme tam derlemelerin büyük son paketleri oluşturabilir gibi bu uygulama nesne AĞACI, etkinleştirirken özellikle önemli hale gelir.

## <a name="enable-the-linker"></a>Bağlayıcı etkinleştir

Başlangıç zamanı, hem yük ve "Yalnızca süre" (JIT), son ikili dosyalarının boyutunu biraz doğrusal olarak ölçeklendirir. Ölü koduyla kaldırarak bu geliştirmek için en kolay yolu olan [bağlayıcı](~/mac/deploy-test/linker.md).

Bu öneri, öncelikle Modern hedef Framework'ü kullanıcılar için geçerlidir, ancak kullanımı [Platform bağlama](~/mac/deploy-test/linker.md) sınırlı performansı artırma de sağlayabilir.

## <a name="enable-aot-when-appropriate"></a>Uygulama Nesne AĞACI uygun olduğunda etkinleştir

Başka bir başlangıç performans makine koda derlemelerin JIT derleme yönüdür. Şimdi zaman derleme başlangıç zamanı önemli ölçüde azaltabilirsiniz Tıkların, ancak ele bileşim sayısı ile birlikte [Uygulama Nesne AĞACI belgelerine](~/mac/internals/aot.md).

## <a name="ensure-performant-delegates"></a>Kullanıcı temsilcileri emin olun

Birçok Xamarin.Mac uygulama Cocoa görünümler gibi ortalanır `NSCollectionView`, `NSOutlineView`, veya `NSTableView`. Genellikle tarafından Bu görünümlere sağlanmıştır `Delegate` ve `DataSource` sınıfları ne görüntülenecek sorulara yanıt verilmesi Cocoa için sağlayın.

Bu giriş noktaları çoğunu genellikle, bazen birden çok kez saniyede çağrılan kaydırma sırasında.

Bu işlevlerin dönüş kolayca hesaplanan veya zaten, kullanıcı arabirimi Engellemesi engellemek için önbelleğe alınan bilgileri kullanın değerleri emin olun.

## <a name="use-cocoa-provided-apis-for-reusing-views"></a>Görünümleri yeniden kullanmak için sağlanan Cocoa API'lerini kullanma

Çok sayıda alt görünümleri veya hücreler içeren birçok Cocoa görünümler (gibi `NSCollectionView`, `NSOutlineView`, ve `NSTableView`) oluşturmak ve görünümleri yeniden kullanmak için API'ler sağlar. Bu paylaşılan öğelerinin havuzları oluşturma ve görünümleri aracılığıyla hızlı bir şekilde kaydırma sırasında performans sorunlarını önlemek.

## <a name="use-async-and-do-not-block-the-ui"></a>Zaman uyumsuz kullanın ve kullanıcı arabirimini engellemez

Masaüstü uygulamaları genellikle çok miktarda veri işleme ve eşzamanlı bir işlem üzerinde bekleyen kullanıcı Arabirimi iş parçacığı engellemek çok kolaydır.

Mümkün olduğunda kullanın [zaman uyumsuz](~/cross-platform/platform/async.md) ve kullanıcı arabirimini engellenmesini önlemek için iş parçacığı.

Uzun çalıştırmak için operations kullanmayı düşünün [NSProgressIndicator](https://developer.xamarin.com/samples/mac/ProgressBarExample/) veya diğer kısmında belirtilen Apple'nın seçenekleri [HIG](https://developer.apple.com/macos/human-interface-guidelines/indicators/progress-indicators/) kullanıcıları bilgilendirmek için.


## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası performansı](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Xamarin.iOS performans](~/ios/deploy-test/performance.md)
