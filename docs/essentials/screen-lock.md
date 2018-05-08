---
title: Xamarin.Essentials ekran kilidi
description: ScreenLock sınıfı uygulama çalışırken, uyku dönmeden gelen ekran tutmak isteyebilirsiniz.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 66702e9f8f5e6ad07f8f7c96f739edf1b2e66617
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials ekran kilidi

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**ScreenLock** sınıfı uygulama çalışırken, uyku dönmeden gelen ekran tutmak için isteyebilir.

## <a name="using-screenlock"></a>ScreenLock kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Ekran kilidi işlevselliği çalışır çağırarak `RequestActive` ve `RequestRelease` ekran kapatma istemesini yöntemleri.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (ScreenLock.IsMonitoring)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease;
    }
}
```

## <a name="api"></a>API

- [Ekran kilidi kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Essentials/ScreenLock)
- [Ekran kilidi API belgeleri](xref:Xamarin.Essentials.ScreenLock)
