---
title: Java ile çalışmaya başlama
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: e88e3c8d2dbabebcfff22687d8ea341233a776b5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-java"></a>Java ile çalışmaya başlama

Desteklenen tüm platformlar için temel kavramları kapsar Java için alma başlangıç sayfasına budur.

## <a name="requirements"></a>Gereksinimler

.NET katıştırma ihtiyacınız olacak Java ile kullanmak için:

* Java 1.8 veya üzeri
* [Mono 5.0](http://www.mono-project.com/download/)

Mac için:

* Xcode 8.3.2 veya daha yenisi

Windows için:

* Visual Studio 2017 C++ desteği
* Windows 10 SDK'sı

Android için:

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) veya daha yenisi
* [Android Studio 3.x](https://developer.android.com/studio/index.html) Java 1.8 ile

Kullanabileceğiniz [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/) düzenleyin ve C# kodunuzu derleyin.

> [!NOTE]
> Xcode, Visual Studio, Xamarin.Android, Android Studio ve Mono önceki sürümlerinde _olabilir_ çalışabilir, ancak sınanmamıştır ve desteklenmiyor.

## <a name="installation"></a>Yükleme

.NET katıştırma üzerinde şu anda kullanılabilir [NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```shell
nuget install Embeddinator-4000
```

Bu yerleştirir **Embeddinator 4000.exe** içine **paketleri/Embeddinator-4000/tools** dizin.

Ayrıca, .NET katıştırma kaynağından yapı, bkz: bizim [git deposu](https://github.com/mono/Embeddinator-4000/) ve [katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md) belge yönergeler için.

## <a name="platforms"></a>Platformlar

Java macOS, Windows ve Android için Önizleme durumu şu anda kullanılıyor.

Platform geçirerek seçili `--platform=<platform>` .NET katıştırma aracı için komut satırı bağımsız değişkeni. Şu anda `macOS`, `Windows`, ve `Android` desteklenir.

### <a name="macos-and-windows"></a>macOS ve Windows

Geliştirme için Java 1.8 destekleyen herhangi bir Java IDE kullanmanız mümkün olması gerekir. Hatta Android Studio bu amaçla kullanabileceğiniz isterseniz, [Burada gördüğünüz](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects). Tüm standart Java jar dosyasını gibi JAR dosyasını çıkışını kullanabilirsiniz.

### <a name="android"></a>Android

Lütfen, zaten ayarlandığı oluşturmak denemeden önce Android uygulamaları geliştirmek için emin olun .NET katıştırma kullanma. [Yönergeleri izleyerek](~/tools/dotnet-embedding/get-started/java/android.md) zaten başarıyla yerleşik ve bir Android uygulamasını bilgisayarınızdan dağıtılan olduğunu varsayın.

Android Studio geliştirme için önerilir, ancak diğer IDE desteği var olduğu sürece çalışmalıdır [AAR dosya biçimi](https://developer.android.com/studio/projects/android-library.html).

## <a name="further-reading"></a>Daha fazla bilgi

* [Android Başlarken](~/tools/dotnet-embedding/get-started/java/android.md)
* [Android'de geri aramalar](~/tools/dotnet-embedding/android/callbacks.md)
* [Ön Android araştırma](~/tools/dotnet-embedding/android/index.md)
* [.NET katıştırma sınırlamaları](~/tools/dotnet-embedding/limitations.md)
* [Açık kaynak projesine katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Hata kodları ve açıklamaları](~/tools/dotnet-embedding/errors.md)
