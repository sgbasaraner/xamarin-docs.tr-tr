---
title: Birden çok platform kodu paylaşımı
description: Bu belge bağlantılar çeşitli kılavuzlara taşınabilir sınıf kitaplıkları, paylaşılan projeleri, .NET standart ve NuGet dahil olmak üzere kodu, paylaşım teknikleri açıklar.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 61377afa61e2c2006c2fdf8ef9b21fe7d567b3de
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780080"
---
# <a name="sharing-code-on-multiple-platforms"></a>Birden çok platform kodu paylaşımı

Bu bölümde bazı daha yaygın şeyler görevleri veya geliştiriciler mobil uygulamaları geliştirirken bilmeniz gereken kavramlar hakkında bir kılavuz sağlar.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Kod paylaşımı genel bakış](code-sharing.md)

Taşınabilir sınıf kitaplıkları (PCLs), paylaşılan projeleri ve .NET standart kitaplıkları dahil Xamarin projeleri için kullanılabilir seçenekleri paylaşımı farklı kodu hakkında bilgi edinin.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Taşınabilir Sınıf Kitaplıkları](~/cross-platform/app-fundamentals/pcl.md)

Taşınabilir Sınıf Kitaplığı projelerinde derleme ve birden fazla platformda çalışacak paylaşılan kodu içeren derlemeler dağıtmak olanak tanır. Taşınabilir sınıf kitaplığı (veya "PCL") oluşturmak için önce hedef sonra bu platformlar için tanımlanan profildeki kullanılabilir .NET Framework'ün bir alt kümesi karşı kod yazmak için hangi platformlarını seçin. Bu belge, oluşturma ve Xamarin ile PCLs kullanma açıklar.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Paylaşılan Projeler](~/cross-platform/app-fundamentals/shared-projects.md)

Paylaşılan projeleri, bir dizi farklı uygulama projeleri tarafından başvurulan ortak kod yazmanıza olanak tanır. Kod başvuruda bulunan her proje bir parçası olarak derlenmiş ve paylaşılan kod Base platforma özgü işlevselliğine sahiptir yardımcı olmak için derleyici yönergeleri dahil edebilirsiniz. Bu makalede, paylaşılan projeleri nasıl çalıştığı ve nasıl oluşturulacağı ve Xamarin projeleriyle kullanmak anlatılmaktadır.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standart kod platformları arasında paylaşmak için yeni bir seçenektir. Taşınabilir sınıf kitaplıkları için benzer bir şekilde çalışır; kod karşı belirli bir sürümü (1.0 1.6 aracılığıyla şu anda) oluşturulur ve ardından bu düzey desteği diğer projeler tarafından tüketilen ya da daha yüksek olabilir. .NET standart projeleri Mac için Xamarin Studio 6.2, Windows için Visual Studio ve Visual Studio içinde desteklenir

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet projeler: kod paylaşımını için birden çok platformlu kitaplıkları](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet paketleri otomatik olarak PCL ya da .NET standart projelerden oluşturulabilir; ve paylaşılan projeleri ayrı NuGet proje türü kullanarak "yemi ve anahtar" NuGet paketlerine paketlenmiş. Bu bölümde her kod paylaşımını senaryo için NuGet paketlerini oluşturma açıklanmaktadır.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[NuGet paketleri için Xamarin el ile oluşturma](~/cross-platform/app-fundamentals/nuget-manual.md)

NuGet paketleri oluşturmak için ipuçları Xamarin platformuyla çalışır.
