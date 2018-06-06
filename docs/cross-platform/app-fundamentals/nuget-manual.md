---
title: NuGet paketleri için Xamarin el ile oluşturma
description: Bu belge Xamarin platformunu hedeflemeniz NuGet paketleri oluşturmanıza yardımcı olmak için ipuçları içermektedir. NuGet paket Xamarin profilleri, PCL NuGets platform bağımlılıkları açıklar ve çeşitli açık kaynak örneklerine bağlar.
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: cc39ade2ccc1192461bcfa19c98b7f9925b667a0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781429"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>NuGet paketleri için Xamarin el ile oluşturma

_Bu sayfa Xamarin platformunu hedeflemeniz NuGet paketleri oluşturmanıza yardımcı olacak bazı ipuçları içerir._

> [!NOTE]
> Xamarin Studio 6.2 (ve Mac için Visual Studio) içeren yeteneği _otomatik olarak_ PCL, .NET standart veya paylaşılan projeleri NuGet paketleri oluşturun. Başvurmak [Multiplatform kitaplıkları kod paylaşım](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) daha fazla ayrıntı için Kılavuzu.

## <a name="nuget-package-xamarin-profiles"></a>NuGet paket Xamarin profilleri

NuGet Web sitesinin [destekleyen birden çok .NET Framework sürümleri ve profilleri](https://docs.nuget.org/create/enforced-package-conventions) farklı Microsoft çerçeveler ve profillerini desteklemek nasıl ele alınmıştır ancak Xamarin tarafından kullanılan hedef framework adları içermez.

Bugün kullanılıyor ana Xamarin hedef çerçeveler şunlardır:

* **MonoAndroid** -Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [Unified API](~/cross-platform/macios/unified/index.md) (64-bit destekler)
* **Xamarin.Mac** -Xamarin.iOS ve Xamarin.Android API yüzeye eşdeğerdir Xamarin.Mac'ın mobil profili.

Ayrıca eski iOS için bir hedefi olan [Klasik API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -iOS Klasik API

A **.nuspec** bunlar tüm hedeflenen dosyası aşağıdakine benzer gibi:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Yukarıdaki tüm taşınabilir sınıf kitaplıkları yok sayar.

Çoğu **.nuspec** dosyaları hedef Framework'ü sürüm numarasını belirtin, ancak derlemenizi hedef Framework'ü tüm sürümleri ile çalışıyorsa isteğe bağlıdır. Hedef varsa bunu **lib\MonoAndroid** Xamarin.Android herhangi bir sürümü ile çalıştığı anlamına gelir.

Sayıları ondalık olmadan kümesiyle sürüm belirtebilir veya ondalık basamak kullanarak belirtebilirsiniz. Ondalık NuGet yalnızca her numarası alın ve bunu bir sürüme ekleyerek devre bir '.' her basamak arasında.

İçinde yukarıdaki "MonoAndroid10", "Android 1.0" anlamına gelir. Bu projenin yalnızca anlamına gelir [hedef framework](~/android/app-fundamentals/android-api-levels.md) MonoAndroid sürüm 1.0 veya üstü olması gerekir. Sürüm belirtilen `<TargetFrameworkVersion>` proje öğesi.

Açıklamak için:

- **MonoAndroid403** Android 4.0.3 eşleşen ve daha yeni (IE API düzeyi 15)
- **Xamarin.iOS10** 1.0 ve daha yeni bir Xamarin.iOS eşleşir
- **Xamarin.iOS1.0** ayrıca 1.0 ve daha yeni bir Xamarin.iOS eşleşir

## <a name="pcl-nugets-with-platform-dependencies"></a>PCL NuGets Platform bağımlılıkları

PCL profilleri hangi .NET framework erişebilecekleri API'leri sınırlıdır ve platforma özgü kodu kesinlikle erişemiyor. Bu 3. taraf bağlantıları Xamarin ve diğer platformlar için uyumluluk sağlamak üzere PCL ve yerel API'lerini kullanan NuGet paketleri oluşturmak için farklı yaklaşımlara ele alınmıştır:

- [Taşınabilir sınıf kitaplıkları iş yaptığınız nasıl](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Yemi ve anahtar PCL eli](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [NuGet PCL oluşturma Xamarin.iOS ile çalışır](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Bu dış [PCL profil listesine kullanıcıların NuGet hedef adı](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) de yararlı başvurudur.

## <a name="examples"></a>Örnekler

Başvurabilirsiniz bazı açık kaynak örnekler:

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) – System.Net.Http kullanarak uygulamanızı yazma, ancak bu kitaplığı bırak ve önemli ölçüde daha hızlı geçer (Görünüm [kaynak](https://github.com/paulcbetts/ModernHttpClient)).
- [**Splat** ](https://www.nuget.org/packages/Splat/) – olması gereken şeyleri platformlar arası yapmak için bir kitaplık (Görünüm [kaynak](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -.NET vektör grafikleri işlemek için platformlar arası kitaplığı (Görünüm [kaynak](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).

## <a name="related-links"></a>İlgili bağlantılar

- [Nuget oluşturma Nugetizer 3000 otomatik](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [İOS için 64-bit NuGets güncelleştiriliyor](http://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [Projenizde bir NuGet dahil olmak üzere](/visualstudio/mac/nuget-walkthrough/index.md)
