---
title: 'Xamarin.Essentials: Ekran kilidi'
description: Bu belgede, uygulama çalışırken, uyku dönülüyor gelen ekranın tutmak isteyebilirsiniz Xamarin.Essentials ScreenLock sınıfında açıklanmaktadır.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848576"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: Ekran kilidi

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**ScreenLock** sınıfı, uygulama çalışırken, uyku dönülüyor gelen ekranın tutmak isteyebilir.

## <a name="using-screenlock"></a>ScreenLock kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Ekran kilitleme işlevselliği çalışır çağırarak `RequestActive` ve `RequestRelease` ekranın kapatılması istek için yöntem.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [Ekran kilidi kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Ekran kilidi API belgeleri](xref:Xamarin.Essentials.ScreenLock)
