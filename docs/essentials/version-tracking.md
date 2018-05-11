---
title: Xamarin.Essentials sürüm izleme
description: Uygulamaları sürümünü denetlemek VersionTracking sınıfı sağlar ve yapı numaraları ilk ise gibi gibi ek bilgileri görmesini yanı sıra her zamankinden başlatılan uygulama zaman veya geçerli sürümü için önceki yapı bilgileri ve daha fazla bilgi alın.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41e0b704715b648e642f4a4c99554ff3f085a39a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials sürüm izleme

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**VersionTracking** sınıfı uygulamaları sürümünü denetlemek olanak sağlar ve yapı numaraları ilk ise gibi gibi ek bilgileri görmesini yanı sıra herhangi bir zamanda başlatılan uygulama zaman veya geçerli sürümü için önceki Al bilgi ve daha fazlasını oluşturun.

## <a name="using-version-tracking"></a>Sürüm izleme kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

İlk kullanışınızda **VersionTracking** sınıfı, geçerli sürümü izleme başlayacak. Çağırmalısınız `Track` erken yalnızca, uygulamanızda yüklenen geçerli sürüm bilgilerini izlenir emin olmak için her zaman:

```csharp
VersionTracking.Track();
```

İlk sonra `Track` çağrılır sürüm bilgileri okuyabilir:

```csharp

// First time ever launched application
var firstLaunch = VersionTracking.IsFirstLaunchEver;

// First time launching current version
var firstLaunchCurrent = VersionTracking.IsFirstLaunchForCurrentVersion;

// First time launching current build
var firstLaunchBuild = VersionTracking.IsFirstLaunchForCurrentBuild;

// Current app version (2.0.0)
var currentVersion = VersionTracking.CurrentVersion;

// Current build (2)
var currentBuild = VersionTracking.CurrentBuild;

// Previous app version (1.0.0)
var previousVersion = VersionTracking.PreviousVersion;

// Previous app build (1)
var previousBuild = VersionTracking.PreviousBuild;

// First version of app installed (1.0.0)
var firstVersion = VersionTracking.FirstInstalledVersion;

// First build of app installed (1)
var firstBuild = VersionTracking.FirstInstalledBuild;

// List of versions installed (1.0.0, 2.0.0)
var versionHistory = VersionTracking.VersionHistory;

// List of builds installed (1, 2)
var buildHistory = VersionTracking.BuildHistory;
```

## <a name="platform-implementation-specifics"></a>Platform uygulama özellikleri

Tüm sürüm bilgileri kullanılarak depolandığını [Tercihler](preferences.md) Xamarin.Essentials API ve bir dosya adı ile depolanan **[YOUR-APP-paket-ID] .xamarinessentials**.

Uygulama kaldırma neden olacak _LocalSettings_ve tüm sürüm izleme bilgilerini kaldırılacak.

## <a name="api"></a>API

- [Sürüm izleme kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Essentials/VersionTracking)
- [Sürüm izleme API belgeleri](xref:Xamarin.Essentials.VersionTracking)
