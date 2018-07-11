---
title: 'Xamarin.Essentials: Uygulama bilgileri'
description: Bu belgede, uygulamanızla ilgili bilgi sağlayan Xamarin.Essentials appInfo sınıfında açıklanmaktadır. Örneğin, uygulama adı ve sürümü gösterir.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e79b3003f41b8de22950624e44e8c9e0e7e7e31
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831513"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: Uygulama bilgileri

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**AppInfo** sınıfı uygulamanız hakkında bilgi sağlar.

## <a name="using-appinfo"></a>AppInfo kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>Uygulama bilgilerini alma:

Aşağıdaki bilgileri, API aracılığıyla sunulur:

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

## <a name="displaying-application-settings"></a>Uygulama ayarlarını görüntüleme

**AppInfo** sınıfı ayrıca bir uygulama için işletim sistemi tarafından tutulan ayarları sayfasının görüntüler:

```csharp
// Display settings page
AppInfo.OpenSettings();
```

Bu ayarları sayfası, kullanıcının uygulama izinlerini değiştirmek ve diğer platforma özgü görevleri sağlar.

## <a name="api"></a>API

- [AppInfo kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API belgeleri](xref:Xamarin.Essentials.AppInfo)
