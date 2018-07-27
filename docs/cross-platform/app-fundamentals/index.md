---
title: Birden fazla platformda kod paylaşımı
description: Bu belge bağlantıları çeşitli kılavuzlara taşınabilir sınıf kitaplıkları, paylaşılan projeler, .NET Standard ve NuGet kod paylaşma teknikleri açıklar.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 3a2c3f98e3ba83db0794a68ff1d62a9845a111c0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270195"
---
# <a name="sharing-code-on-multiple-platforms"></a>Birden fazla platformda kod paylaşımı

Bu makale Windows, Android, iOS ve daha fazlası dahil olmak üzere platformları arasında kod paylaşmak için kullanılabilir farklı seçenekler açıklanır.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Kod paylaşımını genel bakış](code-sharing.md)

Farklı kod paylaşma .NET standart kitaplıkları ve paylaşılan projeleri dahil olmak üzere, Xamarin projeleri için kullanılabilir seçenekleri hakkında bilgi edinin. Taşınabilir sınıf kitaplıkları da desteklenir, ancak kabul .NET Standard ile değiştiriliyor kullanım dışı.

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard platformları arasında kod paylaşımı için tercih edilen seçenektir. Kodu belirli bir sürüme karşı yerleşik (2.0, var olan .NET Framework kodu en iyi API uyum sağlar) ve ardından bu düzey destekleyen diğer projeleri tarafından tüketilen ya da daha yüksek olabilir. .NET standard projelerine Mac için Visual Studio 2017 ve Visual Studio ile desteklenir

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Paylaşılan Projeler](~/cross-platform/app-fundamentals/shared-projects.md)

Paylaşılan projeler farklı uygulama projeleri bir dizi tarafından başvurulan ortak kod yazmanıza olanak sağlar. Kod her başvurulan projenin bir parçası olarak derlenir ve platforma özgü işlevselliği paylaşılan kod tabanının dahil edilip derecelendirilir yardımcı olmak için derleyici yönergeleri içerebilir. Bu makalede, paylaşılan projeler nasıl çalıştığı ve nasıl oluşturup bunları Xamarin projeleri ile kullanma açıklanmaktadır.

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Taşınabilir Sınıf Kitaplıkları](~/cross-platform/app-fundamentals/pcl.md)

Taşınabilir sınıf kitaplığı projeleri derleme ve dağıtma birden çok platformda çalıştırmak için paylaşılan kodu içeren derlemeler sağlar. Taşınabilir sınıf kitaplığı (veya "PCL") oluşturmak için öncelikle hangi platformları hedefleyin ve ardından bu platformları için tanımlanır profili kullanılabilir .NET Framework alt kümesi üzerinde kod yazmak için seçin. PCLs Visual Studio'nun en son sürümlerde kullanım dışı olarak kabul edilir; Geliştiriciler için bunun yerine .NET Standard 2.0 kullanmanız önerilir.

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet projeler: kod paylaşmak için çok platformlu kitaplıkları](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet paketleri otomatik olarak PCL veya .NET standard projeleri oluşturulabilir; ve "baıt ve anahtar" NuGet paketlerine ayrı NuGet proje türü kullanılarak paylaşılan projelere paketlenmesi. Bu bölümde, kod paylaşımı her senaryo için NuGet paketlerini oluşturma açıklanmaktadır.

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[NuGet paketleri için Xamarin el ile oluşturma](~/cross-platform/app-fundamentals/nuget-manual.md)

Xamarin platformu ile iş NuGet paketleri oluşturmaya yönelik ipuçları.
