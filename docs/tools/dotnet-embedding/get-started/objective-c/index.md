---
title: "Objective-C ile çalışmaya başlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 832ad2e6d462b39df7fa4a45739e97f7a7d6e5b5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-objective-c"></a>Objective-C ile çalışmaya başlama

Desteklenen tüm platformlar için temel kavramları kapsayan Objective-C için alma başlangıç sayfasına budur.


## <a name="requirements"></a>Gereksinimler

.NET katıştırma Objective-C ile kullanmak için çalıştıran bir Mac gerekir:

* macOS 10,12 (Sierra) veya daha yenisi
* Xcode 8.3.2 veya daha yenisi
* [Mono 5.0](http://www.mono-project.com/download/)

Yükleyebileceğiniz [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/) düzenleyin ve C# kodunuzu derleyin.


Notlar:

* MacOS, Xcode ve Mono önceki sürümleri _olabilir_ çalışabilir, ancak sınanmamıştır ve desteklenmeyen;
* Kod oluşturma Windows yapılabilir, ancak yalnızca Xcode yüklendiği Mac bilgisayarda derlemek mümkündür;


## <a name="installation"></a>Yükleme

Sonraki adımınız indirin ve .NET katıştırma Mac üzerindeki yüklemektir

* [Paket](https://dl.xamarin.com/embeddinator/Xamarin.Embeddinator-4000-0.2.0.79.pkg)
* [Sürüm Notları](https://github.com/mono/Embeddinator-4000/tree/master/docs/releases)

Alternatif olarak bundan oluşturmak mümkün müdür bizim [git deposu](https://github.com/mono/Embeddinator-4000/tree/objc), bkz: [katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) belge yönergeler için.

Yükleyici göre standart pkg yükleyicisi bulunur:

![Yükleyici giriş](images/install1.png)
![yükleyicisi yükleme türünü](images/install2.png)
![yükleyici özeti](images/install3.png)

Yeni bir terminal oturumu başlattıktan sonra yükleyicisi aracılığıyla yüklenen sonra kullanabileceğiniz `objcgen` komutu.
Aksi takdirde, her zaman aracı, mutlak yolu çalıştırabilirsiniz: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`.

## <a name="platforms"></a>Platformlar

Objective-C macOS, iOS, tvOS ve watchOS uygulamaları yazmak için en yaygın olarak kullanılan bir dildir; ve embeddinator tüm bu platformları destekler. Her platform ile çalışma, bazı temel farklar olduğu anlamına gelir ve bunlar açıklanacak [burada](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[MacOS uygulaması oluşturma](~/tools/dotnet-embedding/get-started/objective-c/macos.md) kimlik, provisining profilleri, benzeticileri ve cihazlar ayarlama gibi kadar ek adımlar içermeyen bu yana en kolay yoludur. İOS için macOS belgeyle başlatılması önerilir.

### <a name="ios--tvos"></a>iOS / tvOS

Lütfen, zaten embeddinator kullanarak bir tane oluşturmak denemeden önce iOS uygulamaları geliştirmek için ayarlandığından emin olun. [Yönergeleri izleyerek](~/tools/dotnet-embedding/get-started/objective-c/ios.md) zaten başarıyla yerleşik ve bilgisayarınızdan bir iOS uygulaması dağıtmış olduğunu varsayın.

TvOS desteği, iOS, yalnızca tvOS projeleri iOS projeleri yerine (Visual Studio ve Xcode) IDE kullanarak işleyişi için benzerdir.

> [!NOTE]
> Not: WatchOS desteği gelecekteki bir sürümde kullanılabilir olur ve iOS/tvOS çok benzer olacaktır.


## <a name="further-reading"></a>Daha Fazla Bilgi

* [Objective-C için belirli özellikler .NET katıştırma](~/tools/dotnet-embedding/objective-c/index.md)
* [Objective C için en iyi uygulamalar](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET katıştırma sınırlamaları](~/tools/dotnet-embedding/limitations.md)
* [Açık kaynak projesine katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Hata kodları ve açıklamaları](~/tools/dotnet-embedding/errors.md)
* [Hedef platformlar](~/tools/dotnet-embedding/objective-c/platforms.md)


## <a name="related-links"></a>İlgili bağlantılar

- [Hava durumu örnek (IOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
