---
title: 'Xamarin.Essentials: Numara Çeviricisi'
description: Bir telefon numarası çevirici'açmak bir uygulamanın Xamarin.Essentials Telefon Çeviricisi sınıfında sağlar
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 34a6c80836d8cb42b1f8fd95718fe248d4701c0f
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130799"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Numara Çeviricisi

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Telefon Çeviricisi** sınıfı Çevirici'bir telefon numarası açmak için bir uygulama sağlar.

## <a name="using-phone-dialer"></a>Numara Çeviricisi'ni kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Numara Çeviricisi işlevselliği çalışır çağırarak `Open` yöntemi ile Çeviricisi'ni açmak için bir telefon numarası. Zaman `Open` API belirtilen ülke kodu dayalı sayısını biçimlendirmek otomatik olarak deneyecek istendi.

```csharp
public class PhoneDialerTest
{
    public async Task PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Kaynak kodu Numara Çeviricisi](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [Telefon Çevirici API belgeleri](xref:Xamarin.Essentials.PhoneDialer)
