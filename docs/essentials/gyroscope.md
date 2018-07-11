---
title: 'Xamarin.Essentials: jiroskop'
description: Xamarin.Essentials jiroskop sınıfında cihazın üç birincil ekseni etrafında döndürme ölçer cihazın jiroskop algılayıcı izlemenize izin verir.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c83b3a9d8a7801e531006f50f8db2e1ad23e48c
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947221"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: jiroskop

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Jiroskop** sınıfı, cihazın üç birincil ekseni etrafında döndürme olan cihazın jiroskop algılayıcı izlemenize olanak tanır.

## <a name="using-gyroscope"></a>Jiroskop kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Jiroskop işlevselliği çalışır çağırarak `Start` ve `Stop` ilgili jiroskop değişiklikleri dinlemek için yöntemleri. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Örnek Kullanım şu şekildedir:

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z
        Console.WriteLine($"Reading: X: {data.AngularVelocity.X}, Y: {data.AngularVelocity.Y}, Z: {data.AngularVelocity.Z}");
    }

    public void ToggleGyroscope()
    {
        try
        {
            if (Gyroscope.IsMonitoring)
              Gyroscope.Stop();
            else
              Gyroscope.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Jiroskop kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Jiroskop API belgeleri](xref:Xamarin.Essentials.Gyroscope)
