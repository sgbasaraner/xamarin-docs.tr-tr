---
title: Xamarin.Essentials ekran kilidi
description: ScreenLock sınıfı uygulama çalışırken, uyku dönmeden gelen ekran tutmak isteyebilirsiniz.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7175362dcb7f85746ea85447936d7fe3e2fd026b
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
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
