---
title: UrhoSharp Mac desteği
description: Bu belge UrhoSharp macOS desteği açıklanır. Bir proje oluşturmayı açıklar ve bazı örnek kod bir bağlantı sağlar.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: aae7b09231ae0e8f88bb9435f50fadd2ff822c1a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783349"
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


