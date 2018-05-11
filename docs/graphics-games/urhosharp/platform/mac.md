---
title: UrhoSharp Mac desteği
description: Mac belirli kurulumu ve UrhoSharp özellikleri.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: c7210cbd5586df9018c2779bf0b92aa7c1c56e89
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="urhosharp-mac-support"></a>UrhoSharp Mac desteği

_Mac belirli kurulumu ve özellikleri_

Taşınabilir sınıf kitaplığı Urho olan ve çeşitli platform oyun mantığınızı için kullanılmak üzere aynı API sağlar, hala Urho platform belirli sürücünüzü ve bazı durumlarda başlatmak için gerekir, platform belirli özelliklerden yararlanmak istersiniz .

Sayfalarında varsayımında `MyGame` sınıfıdır `Application` sınıfı.

## <a name="macos"></a>MacOS

**Desteklenen mimariler:** x86/x86-64 32 bit ve 64 bit için.

## <a name="creating-a-project"></a>Proje Oluşturma

Bir konsol projesi oluşturun, Urho NuGet başvuru ve varlıkları (veri dizinini içeren dizinler) bulabilir emin olun.

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Örnek

[Tam örnek](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


