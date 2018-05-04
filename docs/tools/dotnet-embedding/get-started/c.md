---
title: C ile çalışmaya başlama
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/19/2018
ms.openlocfilehash: 1313d7156a1fd75fd40e2aff65404aef5ab023bb
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-c"></a>C ile çalışmaya başlama

## <a name="requirements"></a>Gereksinimler

.NET katıştırma C ile kullanmak için Mac veya Windows makine çalışması gerekir:

### <a name="macos"></a>MacOS

* macOS 10,12 (Sierra) veya daha yenisi
* Xcode 8.3.2 veya daha yenisi
* [Mono](http://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 veya üstü
* Visual Studio 2015 veya sonraki

## <a name="installing-net-embedding-from-nuget"></a>.NET Nuget'ten katıştırma yükleme

Aşağıdaki adımları [yönergeleri](~/tools/dotnet-embedding/get-started/install/install.md) yüklemek ve .NET katıştırma projeniz için yapılandırmak için.

Yapılandırmanız komut çağırma (büyük olasılıkla farklı sürüm numaraları ve yollar ile) gibi görünür:

### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>Oluşturma

### <a name="output-files"></a>Çıkış dosyaları

Tüm giderse iyi ile aşağıdaki çıkış sunulur:

```shell
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

Bu yana `--compile` bayrağı araç geçti, .NET katıştırma de derlenmiş çıktı dosyaları yanındaki oluşturulan dosyaları bulabilirsiniz, paylaşılan bir kitaplık bir **libmanaged.dylib** macOS ve dosyada**managed.dll** Windows.

Paylaşılan kitaplık tüketmeye dahil edebileceğiniz **managed.h** ilgili için karşılık gelen C bildirimleri sağlar C üstbilgi dosyası yönetilen kitaplık API'leri ve yukarıda açıklanan bağlantısıyla paylaşılan kitaplık derlenir.

## <a name="further-reading"></a>Daha Fazla Bilgi

* [.NET katıştırma sınırlamaları](~/tools/dotnet-embedding/limitations.md)
* [Açık kaynak projesine katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Hata kodları ve açıklamaları](~/tools/dotnet-embedding/errors.md)
