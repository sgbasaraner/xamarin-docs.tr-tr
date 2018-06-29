---
title: 'Xamarin.Essentials: OrientationSensor'
description: OrientationSensor sınıfı bir aygıt üç boyutlu alanı yönünü izlemenize olanak sağlar.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c7bbc849e5fa0b901f5b54e77d548b28bc2a72c6
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080407"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**OrientationSensor** sınıfı, bir aygıt üç boyutlu alanı yönünü izlemenize olanak sağlar.

> [!NOTE]
> Bu sınıf, bir cihaz 3B uzaydaki yönünü belirlemek için ' dir. Cihaz video belirlemeniz gerekiyorsa görüntüleme dikey veya yatay modunda, kullanın `Orientation` özelliği `ScreenMetrics` nesnesi kullanılabilir [ `DeviceDisplay` ](device-display.md) sınıfı.

## <a name="using-orientationsensor"></a>OrientationSensor kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

`OrientationSensor` Çağırarak etkin `Start` yöntemi çağrılarak cihazın yönlendirmesini ve devre dışı değişiklikleri izlemek için `Stop` yöntemi. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Örnek Kullanım şöyledir:

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Orientation.X}, Y: {data.Orientation.Y}, Z: {data.Orientation.Z}, W: {data.Orientation.W}");
        // Process Orientation quaternion (X, Y, Z, and W)
    }

    public void ToggleOrientationSensor()
    {
        try
        {
            if (OrientationSensor.IsMonitoring)
                OrientationSensor.Stop();
            else
                OrientationSensor.Start(speed);
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

`OrientationSensor` okumalar geri biçiminde bildirilen bir [ `Quaternion` ](xref:System.Numerics.Quaternion) iki 3B koordinat sistemi tabanlı aygıt yönünü açıklar:

Aygıt (genellikle bir telefon veya tablet) aşağıdaki eksenli 3B koordinat sistemi vardır:

- X ekseni noktaları görüntüleme dikey modunda sağındaki pozitif.
- Aygıt, dikey modunda üstündeki pozitif Y ekseni işaret ediyor.
- Pozitif Z ekseni dışında ekran işaret eder.

3B dünya koordinat sistemi aşağıdaki eksenleri sahiptir:

- Pozitif X ekseni dünya yüzeye tanjantı ve Doğu işaret eder.
- Pozitif Y ekseni de Kuzey noktaları ve dünya yüzeyinde teğet olur.
- Pozitif Z ekseni noktaları ve dünya yüzeyinin dik ' dir.

`Quaternion` Cihazın koordinat sistemi dünya koordinat sistemi göre dönüşünü açıklar.

A `Quaternion` değeri çok yakın bir ekseni etrafında döndürme ilgili. Bir ekseni döndürme normalleştirilmiş vektör ise (bir<sub>x</sub>,<sub>y</sub>,<sub>z</sub>), ve döndürme açısını Θ, sonra (X, Y, Z W) dördey bileşeni vardır:

(bir<sub>x</sub>·sin(Θ/2), bir<sub>y</sub>·sin(Θ/2), bir<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Sağ taraftaki koordinat sistemleri, bunlar parmakları eğrisini döndürme eksen pozitif yönde işaret sağ taraftaki Flash ile belirtmek için pozitif açıları dönüş yönü.

Örnekler:

* Cihaz, Kuzey, işaret eden üstündeki (dikey modunda) cihazı ile kullanıma yönelik kendi Ekranlı Düz tablo üzerinde gerektiği zaman iki koordinat sistemi hizalanır. `Quaternion` Değeri (0, 0, 0, 1) kimliğini dördey temsil eder. Tüm döndürmeleri bu konumuna göre çözümlenebilir.

* Cihaz düz tablo üzerinde karşılıklı kendi ekran ve Batı, işaret eden üst aygıtı (dikey modu) gerektiği zaman `Quaternion` değerdir (0, 0, 0.707, 0.707). Cihaz dünya Z ekseni etrafında 90 derece döndürülmüş.

* Aygıt (dikey modunda) üst sky işaret ve aygıtın arkasında Kuzey bakarken dik tutulurken, cihaz 90 derece X ekseni etrafında olmuştur. `Quaternion` Değerdir (0.707, 0, 0, 0.707).

* Cihaz sol kenarı üzerinde bir tablodur ve üst Kuzey, işaret cihazı Döndürülmüş konumlandırılmış varsa &ndash;90 derece Y ekseni etrafında (veya negatif Y ekseni etrafında 90 derece). `Quaternion` Değerdir (0,-0.707, 0, 0.707).

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Algılayıcı hızı](xref:Xamarin.Essentials.SensorSpeed)

- **Hızlı** – algılayıcı verilerini (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) mümkün olduğunca hızlı alın.
- **Oyun** – (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) oyunlar için uygun oranı.
- **Normal** – varsayılan hızı ekran yönünü değişiklikleri için uygun.
- **UI** – genel kullanıcı arabirimi için uygun oranı.

Olay işleyicisi UI iş parçacığında çalıştırmak ve kullanıcı arabirimi öğeleri, olay işleyicisi erişmesi gerekirse kullanmak için kesin değildir, [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) UI iş parçacığında bu kodu çalıştırmak için yöntem.

## <a name="api"></a>API

- [OrientationSensor kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor API belgeleri](xref:Xamarin.Essentials.OrientationSensor)
