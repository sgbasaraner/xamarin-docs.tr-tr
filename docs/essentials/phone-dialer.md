---
title: 'Xamarin.Essentials: Numara Çeviricisi'
description: Xamarin.Essentials Telefon Çeviricisi sınıfında en iyi duruma getirilmiş sistem tercih edilen tarayıcıda veya dış tarayıcı web bağlantıyı açmak bir uygulama sağlar.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6733e43ed4174d1dd78b2e8f70268eb54adadb98
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782859"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Numara Çeviricisi

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Telefon Çeviricisi** sınıfı en iyi duruma getirilmiş sistem tercih edilen tarayıcıda veya dış tarayıcı web bağlantıyı açmak bir uygulama sağlar.

## <a name="using-phone-dialer"></a>Numara Çeviricisi kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Numara Çeviricisi işlevselliği çalışır çağırarak `Open` Numara Çeviricisi ile açmak için bir telefon numarası ile yöntemi. Zaman `Open` API ülke kodu belirtilmişse göre sayıyı biçimlendirmek otomatik olarak başlatacak istendi.

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

- [Kaynak kodu Çeviricisi](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [Telefon Çeviricisi API belgeleri](xref:Xamarin.Essentials.PhoneDialer)
