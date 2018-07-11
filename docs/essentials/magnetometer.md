---
title: 'Xamarin.Essentials: Magnetometer'
description: Xamarin.Essentials Magnetometer sınıfında dünyanın manyetik alan göreli cihazın yönünü belirten cihazın magnetometer algılayıcı izlemenize izin verir.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 52790f78c2d78347a35f111b3c4db63900c24ec7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947367"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: Magnetometer

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Magnetometer** sınıfı dünyanın manyetik alan göreli cihazın yönünü gösteren cihazın magnetometer algılayıcı izlemenize olanak tanır.

## <a name="using-magnetometer"></a>Magnetometer kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Magnetometer işlevselliği çalışır çağırarak `Start` ve `Stop` magnetometer değişiklikleri dinlemek için yöntemleri. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Örnek Kullanım şu şekildedir:

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometerr_ReadingChanged(MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
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

Tüm veriler microteslas döndürülür.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Magnetometer kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Magnetometer API belgeleri](xref:Xamarin.Essentials.Magnetometer)
