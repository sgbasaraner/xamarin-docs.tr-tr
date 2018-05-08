---
title: Xamarin.Essentials uygulama bilgileri
description: AppInfo sınıfı uygulamanız hakkında bilgi sağlar.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5b235fdeb666c3f8f2c1b52a9691c931724ce2b8
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials uygulama bilgileri

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**AppInfo** sınıfı uygulamanız hakkında bilgi sağlar.

## <a name="using-appinfo"></a>AppInfo kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Aşağıdaki bilgiler, API aracılığıyla sunulur:

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="api"></a>API

- [AppInfo kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Essentials/AppInfo)
- [AppInfo API belgeleri](xref:Xamarin.Essentials.AppInfo)
