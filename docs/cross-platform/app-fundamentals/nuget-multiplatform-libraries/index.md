---
title: NuGet çok platformlu kitaplık projeleri (diğer adıyla Nugetizer 3000)
description: Bu belge, platformlar arasında kod paylaşmak için NuGet paketleri otomatik olarak oluşturmak için Nugetizer 3000 Aracı'nı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 1d48bc28aa4477361ca8057fda91ee3258f36a73
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270434"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>NuGet çok platformlu kitaplık projeleri (Nugetizer 3000)

_Otomatik olarak 'Nugetizer 3000' kullanarak platformlar arasında kod paylaşmak için NuGet paketleri oluşturun!_

Kod kullanarak tüm platformlarda paylaşmak için NuGet paketleri otomatik olarak oluşturmanıza olanak _Nugetizer 3000_. Yeni bir oluşturarak veya mevcut kitaplık projeleri NuGet paketleri oluşturmak bu yapar mümkündür **çok platformlu kitaplık projesi**.

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

Nugetizer 3000 Mac için Visual Studio ile birlikte gelir &ndash; Ara **kitaplığı > Mulitplatform Kitaplığı** proje türü **Dosya > Yeni** penceresi:

[![](images/mulitplatform-library-sml.png "Yeni çok platformlu kitaplık penceresi oluştur")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio'da Nugetizer 3000 kullanmak için lütfen [indirme ve çalıştırma VSIX yükleyicisi](http://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>NuGet paketleri oluşturma

Yapılandırıldıktan sonra kod, dahili olarak diğer uygulamalar ile paylaşmak için kullanılan veya yüklenmiş eksiksiz bir NuGet paketi projesinin tüm yapı çıkışları [NuGet.org](https://www.nuget.org).

Bu özelliği kullanmak için üç senaryo vardır:

- [Mevcut Kitaplık Projeleri](existing-library.md)

  Bir NuGet paketi PCL (veya .NET Standard) var olan projeleri oluşturun.

- [Yeni çok platformlu kitaplık projesi oluşturma](single-codebase.md)

  Bir PCL veya .NET Standard kullanarak, NuGet aracılığıyla ortak kod paylaşmak için yeni bir kitaplık oluşturun.

- [Yeni platforma özgü kitaplık projeleri oluşturma](platform-specific.md)

  Yeni Kitaplık ve iOS ve Android için platforma özgü kod içerir ve iOS veya Android belirli işlevleri desteklemek için platforma özel Proje ve ortak kod içeren bir paylaşılan proje kullanan NuGet oluşturun.

Başvurmak [meta veri Kılavuzu](metadata.md) herhangi bir NuGet paketi eklenmelidir gerekli ve isteğe bağlı meta veriler hakkında ayrıntılı bilgi için.

## <a name="further-nuget-information"></a>NuGet hakkında daha fazla bilgi

Daha fazla bilgi edinin [el ile Nuget'i için Xamarin oluşturma](~/cross-platform/app-fundamentals/nuget-manual.md) ve nasıl [bir uygulamada bir NuGet paketi dahil](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Microsoft'un [NuGet belgeleri](https://docs.microsoft.com/nuget/) üzerinde daha ayrıntılı bilgi içeren **.nupkg** biçimi ve Visual Studio'da NuGet paketlerini kullanarak.

NuGet paket projeleri için tasarım tartışması (yani) NuGetizer 3000) üzerinde kullanılabilir [NuGet GitHub deposu](https://github.com/NuGet/Home/wiki/NuGetizer-3000).

## <a name="related-links"></a>İlgili bağlantılar

- [NuGetizer 3000 kullanım örnekleri](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [NuGet paketleri için Xamarin el ile oluşturma](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet belgeleri](https://docs.microsoft.com/nuget/)
- [Sürüm Notları](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
