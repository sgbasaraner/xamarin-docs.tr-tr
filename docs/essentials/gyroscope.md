---
title: 'Xamarin.Essentials: jiroskop'
description: Xamarin.Essentials jiroskop sınıfında cihazın üç birincil eksen etrafında döndürme ölçer cihazın jiroskop algılayıcı izlemenize izin verir.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2f2961c6cb78293891e186e7e0f749a7aa2fb8fc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783021"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: jiroskop

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Jiroskop** sınıfı cihazın üç birincil eksen etrafında döndürme olan cihazın jiroskop algılayıcı izlemenize olanak sağlar.

## <a name="using-gyroscope"></a>Jiroskop kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Jiroskop işlevselliği çalışır çağırarak `Start` ve `Stop` jiroskop değişiklikleri dinlemek için yöntemleri. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Örnek Kullanım şöyledir:

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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Algılayıcı hızı](xref:Xamarin.Essentials.SensorSpeed)

- **Hızlı** – algılayıcı verilerini (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) mümkün olduğunca hızlı alın.
- **Oyun** – (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) oyunlar için uygun oranı.
- **Normal** – varsayılan hızı ekran yönünü değişiklikleri için uygun.
- **UI** – genel kullanıcı arabirimi için uygun oranı.

## <a name="api"></a>API

- [Jiroskop kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Jiroskop API belgeleri](xref:Xamarin.Essentials.Gyroscope)
