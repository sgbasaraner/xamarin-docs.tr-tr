---
title: NuGet Projects (Nugetizer 3000)
description: Otomatik olarak kod 'Nugetizer 3000' kullanarak platformları arasında paylaşmak için NuGet paketlerini oluşturun!
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: 2ef8bfc15aaa2e66683c38584f05b94d20a2a9c3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="nuget-projects-nugetizer-3000"></a>NuGet Projects (Nugetizer 3000)

_Otomatik olarak kod 'Nugetizer 3000' kullanarak platformları arasında paylaşmak için NuGet paketlerini oluşturun!_

Kod kullanarak platformları arasında paylaşmak için NuGet paketleri otomatik olarak oluşturmak mümkündür _Nugetizer 3000_. Varolan kitaplık projeleri veya yenisini oluşturarak NuGet paketleri oluşturmak bu yapar mümkündür **Multiplatform kitaplığı projesi**.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Nugetizer 3000 Mac 6.2 için Visual Studio ile birlikte gelir.

[![](images/mulitplatform-library-sml.png "Yeni Multiplatform kitaplığı pencere oluşturma")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da Nugetizer 3000 kullanmak için lütfen [indirin ve VSIX yükleyiciyi çalıştırmak](http://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>Yapı NuGet paketleri

Yapılandırdıktan sonra her yapı projenin kod dahili olarak diğer uygulamalarla paylaşmak için kullanılan veya yüklenen tam bir NuGet paketi çıkarır [NuGet.org](https://www.nuget.org).

Bu özelliği kullanmak için üç senaryo vardır:

- [Mevcut Kitaplık Projeleri](existing-library.md)

  Bir NuGet Paketi varolan PCL (veya .NET standart) projelerden oluşturun.

- [Yeni bir çok platformlu kitaplık projesi oluşturma](single-codebase.md)

  Bir PCL veya .NET standart kullanarak NuGet aracılığıyla ortak kod paylaşmak için yeni bir kitaplık oluşturun.

- [Yeni platforma özgü kitaplık projeleri oluşturma](platform-specific.md)

  Yeni Kitaplık ve iOS ve Android için platforma özgü kodu içerir ve ortak kodun ve iOS veya Android belirli işlevleri desteklemek için platforma özgü projeleri içerecek şekilde paylaşılan bir proje kullanan NuGet oluşturun.

Başvurmak [meta veri Kılavuzu](metadata.md) herhangi bir NuGet paket eklenmelidir gerekli ve isteğe bağlı meta veriler hakkında ayrıntılı bilgi için.


## <a name="further-nuget-information"></a>Daha fazla NuGet bilgi

Daha fazla bilgi edinin [NuGets için Xamarin el ile oluşturma](~/cross-platform/app-fundamentals/nuget-manual.md) ve nasıl [bir uygulamada bir NuGet paketi dahil](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Microsoft'un [NuGet belgelerine](https://docs.microsoft.com/nuget/) hakkında daha ayrıntılı bilgiler içerir **.nupkg** biçimi ve Visual Studio'da NuGet paketleri kullanma.

NuGet paket projeleri için tasarım tartışma (paketini NuGetizer 3000) edinilebilir [NuGet GitHub deposunu](https://github.com/NuGet/Home/wiki/NuGetizer-3000).


## <a name="related-links"></a>İlgili bağlantılar

- [NuGetizer 3000 kullanım örnekleri](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [NuGet paketleri için Xamarin el ile oluşturma](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet belgeleri](https://docs.microsoft.com/nuget/)
- [Sürüm Notları](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
