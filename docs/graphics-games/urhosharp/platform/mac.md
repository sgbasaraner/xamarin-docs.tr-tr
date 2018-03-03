---
title: "UrhoSharp Mac desteği"
description: "Mac belirli kurulumu ve özellikleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: cdaca45c3132abf3f52d407d4ce59c680248fce7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="urhosharp-mac-support"></a>UrhoSharp Mac desteği

_Mac belirli kurulumu ve özellikleri_

Taşınabilir sınıf kitaplığı Urho olan ve çeşitli platform oyun mantığınızı için kullanılmak üzere aynı API sağlar, hala Urho platform belirli sürücünüzü ve bazı durumlarda başlatmak için gerekir, platform belirli özelliklerden yararlanmak istersiniz .

Sayfalarında varsayımında `MyGame` sınıfıdır `Application` sınıfı.

# <a name="macos-x"></a>MacOS X

**Desteklenen mimariler:** x86/x86-64 32 bit ve 64 bit için.

# <a name="creating-a-project"></a>Proje Oluşturma

Bir konsol projesi oluşturun, Urho NuGet başvuru ve varlıkları (veri dizinini içeren dizinler) bulabilir emin olun.

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Örnek

[Tam örnek](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


