---
title: 'Xamarin.Essentials: Uygulama bilgileri'
description: Bu belgede, uygulamanız ile ilgili bilgi sağlayan Xamarin.Essentials appInfo sınıfında açıklanmaktadır. Örneğin, uygulama adı ve sürümü kullanıma sunar.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 25765fbbcbd2475bfcbc72ca5c4feefa8ef19d08
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782111"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: Uygulama bilgileri

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

- [AppInfo kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API belgeleri](xref:Xamarin.Essentials.AppInfo)
