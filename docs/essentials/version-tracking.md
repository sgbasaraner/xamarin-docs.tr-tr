---
title: 'Xamarin.Essentials: Sürüm izleme'
description: Uygulama sürümü denetlemek Xamarin.Essentials VersionTracking sınıfında sağlar ve ilk grubundaymış gibi gibi ek bilgileri görmeye yanı sıra derleme numaraları hiç olmadığı kadar başlatılan uygulama zaman veya geçerli sürümü için önceki derlemeyi Al bilgi ve daha fazlası.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2c092d6767045f0af956c5dab74801077dadb51f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815654"
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials: Sürüm izleme

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**VersionTracking** sınıfı uygulama sürümünü denetleme olanak sağlar ve ilk grubundaymış gibi gibi ek bilgileri görmeye yanı sıra derleme numaraları hiç olmadığı kadar başlatılan uygulama zaman ya da geçerli sürümü için önceki Al bilgi ve daha fazlasını oluşturun.

## <a name="using-version-tracking"></a>Sürüm izleme kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

İlk kullanışınızda **VersionTracking** sınıfı, geçerli sürümü izleme başlayacak. Çağırmalısınız `Track` uygulamanızın yüklü geçerli sürüm bilgilerini izlenir emin olmak için her zaman yalnızca erkenden:

```csharp
VersionTracking.Track();
```

İlk sonra `Track` çağrılır sürüm bilgilerini okuyabilir:

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

## <a name="platform-implementation-specifics"></a>Platform uygulaması özellikleri

Tüm sürüm bilgilerini kullanarak depolanan [tercihleri](preferences.md) Xamarin.Essentials API'SİNDE ve dosya adını ile depolanan **[YOUR-APP-PAKETİNİ-ID] .xamarinessentials**.

Uygulama kaldırma neden _LocalSettings_ve tüm sürüm izleme bilgileri kaldırılacak.

## <a name="api"></a>API

- [Sürüm izleme kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [Sürüm izleme API belgeleri](xref:Xamarin.Essentials.VersionTracking)
