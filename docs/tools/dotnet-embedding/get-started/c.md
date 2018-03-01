---
title: "C ile çalışmaya başlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ea8335348416dc00074d83e09b74521da7abcb66
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-c"></a>C ile çalışmaya başlama


## <a name="requirements"></a>Gereksinimler

.NET katıştırma C ile kullanmak için bir Mac veya Windows makine çalışan gerekir:

* macOS 10,12 (Sierra) veya daha yenisi
* Xcode 8.3.2 veya daha yenisi

* Windows 7, 8, 10 veya üstü
* Visual Studio 2013 veya üzeri

* [Mono](http://www.mono-project.com/download/)


## <a name="installation"></a>Yükleme

Sonraki adımınız, .NET katıştırma araçları makinenize yükleyip olmaktır.

C ve Java oluşturucuları için ikili derlemeleri hala kullanılabilir olmayan ancak yakında çıkıyor.

Alternatif olarak oluşturmak mümkündür bizim git deposundan görmek [katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) belge yönergeler için.


## <a name="generation"></a>Oluşturma

C kodu oluşturmak üzere C dil hedeflemek için doğru bayrakları geçirme .NET katıştırma aracı çağırırsınız:

### <a name="windows"></a>Windows:

```csharp
$ build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

Visual Studio komut kabuğu'ndan belirli için Visual Studio sürümü, çağrıldığından emin olun atamak demektir.

### <a name="macos"></a>macOS

```csharp
$ mono build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="output-files"></a>Çıkış dosyaları

Tüm giderse iyi ile aşağıdaki çıkış sunulur:

```csharp
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

Bu yana `--compile` bayrağı araç geçti, .NET katıştırma de derlenmiş çıktı dosyaları yanındaki oluşturulan dosyaları bulabilirsiniz, paylaşılan bir kitaplık bir `libmanaged.dylib` macOS, dosya ve `managed.dll` Windows.

Dahil edebileceğiniz paylaşılan kitaplık tüketmeye `managed.h` ilgili için karşılık gelen C bildirimleri sağlar C üstbilgi dosyası yönetilen kitaplık API'leri ve yukarıda açıklanan bağlantısıyla paylaşılan kitaplık derlenir.

## <a name="further-reading"></a>Daha Fazla Bilgi

* [Embeddinator sınırlamaları](~/tools/dotnet-embedding/limitations.md)
* [Açık kaynak projesine katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Hata kodları ve açıklamaları](~/tools/dotnet-embedding/errors.md)
