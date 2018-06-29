---
title: Objective-C ile çalışmaya başlama
description: Bu belgede açıklanan nasıl .NET katıştırma Objective-c ile kullanmaya başlama .NET katıştırma NuGet ve desteklenen platformlar yükleme gereksinimleri açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 02d79103825d150b6e6f5bec7ed3ee1788078312
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066633"
---
# <a name="getting-started-with-objective-c"></a>Objective-C ile çalışmaya başlama

Desteklenen tüm platformlar için temel kavramları kapsayan Objective-C için alma başlangıç sayfasına budur.

## <a name="requirements"></a>Gereksinimler

.NET katıştırma Objective-C ile kullanmak için çalışan bir Mac gerekir:

* macOS 10,12 (Sierra) veya daha yenisi
* Xcode 8.3.2 veya daha yenisi
* [Mono 5.0](http://www.mono-project.com/download/)

Yükleyebileceğiniz [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/) düzenleyin ve C# kodunuzu derleyin.

> [!NOTE]
> * MacOS, Xcode ve Mono önceki sürümleri _olabilir_ çalışabilir, ancak sınanmamıştır ve desteklenmeyen
> * Kod oluşturma Windows yapılabilir, ancak yalnızca Xcode yüklendiği Mac bilgisayarda derlemek mümkündür

## <a name="installing-net-embedding-from-nuget"></a>.NET Nuget'ten katıştırma yükleme

Aşağıdaki adımları [yönergeleri](~/tools/dotnet-embedding/get-started/install/install.md) yüklemek ve .NET katıştırma projeniz için yapılandırmak için.

Bir örnek komut çağrısının listelenen [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) ve [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) kılavuzları Başlarken.

## <a name="platforms"></a>Platformlar

Objective-C macOS, iOS, tvOS ve watchOS uygulamaları yazmak için en yaygın olarak kullanılan bir dildir; .NET katıştırma tüm bu platformları destekler. Her platform ile çalışma bazı gelir [farklar ve bu açıklanmıştır burada](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[MacOS uygulaması oluşturma](~/tools/dotnet-embedding/get-started/objective-c/macos.md) kimlik, provisining profilleri, benzeticileri ve cihazlar ayarlama gibi kadar ek adımlar içermeyen bu yana en kolay yoludur. İOS için macOS belgeyle başlatılması önerilir.

### <a name="ios--tvos"></a>iOS / tvOS

Lütfen, zaten ayarlandığı oluşturmak denemeden önce iOS uygulamaları geliştirmek için emin olun .NET katıştırma kullanma. [Yönergeleri izleyerek](~/tools/dotnet-embedding/get-started/objective-c/ios.md) zaten başarıyla yerleşik ve bilgisayarınızdan bir iOS uygulaması dağıtmış olduğunu varsayın.

TvOS desteği, iOS, yalnızca tvOS projeleri iOS projeleri yerine (Visual Studio ve Xcode) IDE kullanarak işleyişi için benzerdir.

> [!NOTE]
> WatchOS gelecekteki bir sürümde kullanılabilir olur ve iOS/tvOS çok benzer desteği.

## <a name="further-reading"></a>Daha fazla bilgi

* [Objective-C için belirli özellikler .NET katıştırma](~/tools/dotnet-embedding/objective-c/index.md)
* [Objective C için en iyi uygulamalar](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET katıştırma sınırlamaları](~/tools/dotnet-embedding/limitations.md)
* [Açık kaynak projesine katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Hata kodları ve açıklamaları](~/tools/dotnet-embedding/errors.md)
* [Hedef platformlar](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>İlgili bağlantılar

- [Hava durumu örnek (IOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
