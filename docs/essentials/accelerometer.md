---
title: 'Xamarin.Essentials: ivme ölçer'
description: İvme ölçer sınıfını Xamarin.Essentials içinde gösteren üç boyutlu boşluk cihazın ivmesini cihazın ivme ölçer algılayıcı izlemenize izin verir.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 15e2cb69806f281e88e226b7bcd87a20e149d508
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947315"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: ivme ölçer

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**İvme ölçer** sınıfı gösteren üç boyutlu boşluk cihazın ivmesini cihazın ivme ölçer algılayıcı izlemenize olanak tanır.

## <a name="using-accelerometer"></a>İvme ölçer kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

İşlevselliği ivme ölçer çalışır çağırarak `Start` ve `Stop` hızlandırma değişiklikleri dinlemek için yöntemleri. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Örnek Kullanım şu şekildedir:

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAcceleromter()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
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

İvme ölçer okumalar geri G. içinde bildirilen Bir G Yerçekimi birimi zorla dünya gravitational alana göre sarf eşit olduğu (9.81 m/sn ^ 2).

Koordinat sistemi varsayılan yönünü telefon ekranın göre tanımlanır. Cihazın ekran yönüne değiştiğinde eksenleri takas değil.

X ekseni yatay ve noktaları sağındaki Y ekseni dikey olan ve yukarı bakan ve Z ekseni ön nominal ekranın dışına işaret eder. Bu sistemde koordinatlarını ekran arkasında negatif Z değerleri vardır.

Örnekler:

* Cihaz, düz bir tabloda bulunan ve sağa doğru sol taraftaki gönderdiniz x hızlandırma değeri pozitif olur.

* Cihaz düz bir tabloda gerektiği zaman hızlandırma +1.00 G değerdir veya (+ 9.81 m/sn ^ 2), cihazın ivmesini için karşılık gelen (0 m/sn ^ 2) yer çekimi kuvveti eksi (-9.81 m/sn ^ 2) ve normalleştirilmiş G. olduğu gibi

* Ne zaman cihaz düz bir tabloda bulunan ve m/sn'lik bir hızlandırmalı sky doğru gönderilir ^ 2 hızlandırma değeri olan cihazın ivmesini için karşılık gelen bir + 9.81 eşittir (+ m/sn ^ 2) yer çekimi kuvveti eksi (-9.81 m/sn ^ 2) ve normalleştirilmiş G. içinde 

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [İvme ölçer kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [İvme ölçer API belgeleri](xref:Xamarin.Essentials.Accelerometer)
