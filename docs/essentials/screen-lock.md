---
title: Xamarin.Essentials ekran kilidi
description: ScreenLock sınıfı uygulama çalışırken, uyku dönmeden gelen ekran tutmak isteyebilirsiniz.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 0bdf75825d9c6dc594749fe7aa1e133207cfa0fa
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2018
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
        if (ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [Ekran kilidi kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Ekran kilidi API belgeleri](xref:Xamarin.Essentials.ScreenLock)
