---
title: 'Xamarin.Essentials: Magnetometer'
description: Xamarin.Essentials Magnetometer sınıfında cihazın yönlendirmesini dünya manyetik alan göre gösterir ve cihazın magnetometer algılayıcı izlemenize izin verir.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2c02188b5282949559e0abc5fa1b61b6b451fc8e
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080405"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: Magnetometer

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Magnetometer** sınıfı dünya manyetik alan göreli cihazın yönünü gösteren cihazın magnetometer algılayıcı izlemenize olanak sağlar.

## <a name="using-magnetometer"></a>Magnetometer kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Magnetometer işlevselliği çalışır çağırarak `Start` ve `Stop` magnetometer değişiklikleri dinlemek için yöntemleri. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Örnek Kullanım şöyledir:

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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Algılayıcı hızı](xref:Xamarin.Essentials.SensorSpeed)

- **Hızlı** – algılayıcı verilerini (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) mümkün olduğunca hızlı alın.
- **Oyun** – (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) oyunlar için uygun oranı.
- **Normal** – varsayılan hızı ekran yönünü değişiklikleri için uygun.
- **UI** – genel kullanıcı arabirimi için uygun oranı.

Olay işleyicisi UI iş parçacığında çalıştırmak ve kullanıcı arabirimi öğeleri, olay işleyicisi erişmesi gerekirse kullanmak için kesin değildir, [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) UI iş parçacığında bu kodu çalıştırmak için yöntem.

## <a name="api"></a>API

- [Magnetometer kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Magnetometer API belgeleri](xref:Xamarin.Essentials.Magnetometer)
