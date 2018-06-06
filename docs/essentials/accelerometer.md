---
title: 'Xamarin.Essentials: Accelerometer'
description: Xamarin.Essentials Accelerometer sınıfında üç boyutlu alanı aygıtı ivmesini gösterir ve cihazın accelerometer algılayıcı izlemenize izin verir.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 99529f08348254dff7577b7e82da739fabd63a14
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781871"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: Accelerometer

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Accelerometer** sınıfı üç boyutlu alanı aygıtı ivmesini gösteren cihazın accelerometer algılayıcı izlemenize olanak sağlar.

## <a name="using-accelerometer"></a>Accelerometer kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Accelerometer işlevselliği çalışır çağırarak `Start` ve `Stop` hızlandırma değişiklikleri dinlemek için yöntemleri. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Örnek Kullanım şöyledir:

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

Accelerometer okumalar geri G. içinde bildirilen Bir G Yerçekimi birimi zorla dünya gravitational alana göre exerted eşit olduğu (9.81 m/s ^ 2).

Koordinat sistemi varsayılan yönünü telefon ekran göre tanımlanır. Cihazın ekran yönünü değiştiğinde eksenleri takas değil.

X ekseni yatay ve nokta sağdaki Y ekseni dikeydir ve yukarıyı ve Z ekseni ekranın ön yüz dışına doğru işaret eder. Bu sistemde negatif Z değerleri ekran arkasında koordinatlara sahip.

Örnekler:

* Cihaz düz tablo üzerinde arasındadır ve sağa doğru sol taraftaki gönderilen x hızlandırma değeri pozitif olur.

* Aygıt bir tablo üzerinde düz gerektiği zaman hızlandırmasını +1.00 G değerdir veya (+ 9.81 m/s ^ 2), cihaz ivmesini karşılık gelen (0 m/s ^ 2) yer çekimi zorla eksi (-9.81 m/s ^ 2) ve G. olduğu gibi normalleştirilmiş

* Ne zaman cihaz düz tablo üzerinde arasındadır ve bir ivmesini m/s ile sky doğru gönderilen ^ 2 hızlandırma değeri olan aygıt ivmesini karşılık gelen bir + 9.81 eşit (+ m/s ^ 2) yer çekimi zorla eksi (-9.81 m/s ^ 2) ve G. normalleştirilmiş 

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Algılayıcı hızı](xref:Xamarin.Essentials.SensorSpeed)

- **Hızlı** – algılayıcı verilerini (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) mümkün olduğunca hızlı alın.
- **Oyun** – (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) oyunlar için uygun oranı.
- **Normal** – varsayılan hızı ekran yönünü değişiklikleri için uygun.
- **UI** – genel kullanıcı arabirimi için uygun oranı.

## <a name="api"></a>API

- [Accelerometer kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Accelerometer API belgeleri](xref:Xamarin.Essentials.Accelerometer)
