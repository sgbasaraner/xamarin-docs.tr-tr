---
title: 'Xamarin.Essentials: Numara Çeviricisi'
description: Xamarin.Essentials Telefon Çeviricisi sınıfında en iyi duruma getirilmiş sistem tercih edilen tarayıcıda veya dış tarayıcı web bağlantısı açmak için bir uygulama sağlar.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6733e43ed4174d1dd78b2e8f70268eb54adadb98
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831403"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Numara Çeviricisi

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Telefon Çeviricisi** sınıfı bir web bağlantısına en iyi duruma getirilmiş sistem tercih edilen tarayıcıda veya dış tarayıcı açmak için bir uygulama sağlar.

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
