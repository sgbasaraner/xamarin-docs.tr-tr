---
title: 'Xamarin.Essentials: OrientationSensor'
description: OrientationSensor sınıfı bir cihaza üç boyutlu boşluk yönünü izlemenize izin verir.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: a15338795424885882ed9c86288342d196f6fda2
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353821"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**OrientationSensor** sınıfı, bir cihaza üç boyutlu boşluk yönünü izlemenize olanak tanır.

> [!NOTE]
> 3B alanda bir cihaz yönünü belirlemek için bu sınıftır. Cihazın video belirlemeniz gerekiyorsa görüntüleme dikey veya yatay modda, kullanın `Orientation` özelliği `ScreenMetrics` kullanılabilir nesne [ `DeviceDisplay` ](device-display.md) sınıfı.

## <a name="using-orientationsensor"></a>OrientationSensor kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

`OrientationSensor` Çağırarak etkin `Start` yöntemi çağırarak cihazın yön ve devre dışı değişiklikleri izlemek için `Stop` yöntemi. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Örnek Kullanım şu şekildedir:

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(object sender, OrientationSensorChangedEventArgs e)
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

`OrientationSensor` okumalar geri biçiminde bildirilen bir [ `Quaternion` ](xref:System.Numerics.Quaternion) 3B iki koordinat sistemi tabanlı cihaz yönünü açıklar:

Cihaz (genellikle bir telefon veya tablet) aşağıdaki eksenleri ile 3B bir koordinat sistemi vardır:

- Eksen noktaları dikey modda görüntü sağındaki X pozitif.
- Cihaz dikey modda üstüne pozitif Y ekseni işaret eder.
- Pozitif Z ekseni ekran dışında işaret eder.

Dünya koordinat sistemi 3B aşağıdaki ekseni bulunur:

- Pozitif X ekseni tanjantı yüzeyine dünya ve Doğu işaret eder.
- Pozitif Y ekseni de dünya ve Kuzey noktaları yüzeyine Eğim.
- Pozitif Z ekseni puanları ve dünya yüzeyine dikey.

`Quaternion` Cihazın dünya koordinat sistemi göreli koordinat sistemini döndürmeyi açıklar.

A `Quaternion` değeri bir ekseni etrafında döndürme için çok yakından ilgilidir. Eksen döndürme normalleştirilmiş vektör varsa (bir<sub>x</sub>,<sub>y</sub>,<sub>z</sub>), ve döndürme açısı Θ, sonra (X, Y, Z, W) dördey bileşenleri şunlardır:

(bir<sub>x</sub>·sin(Θ/2), bir<sub>y</sub>·sin(Θ/2), bir<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Sağ taraftaki koordinat sistemi, bunlar parmağınızı eğrisini döndürme eksen olumlu yönde işaret eden sağ taraftaki küçük resim ile belirtmek için döndürme pozitif açı yönü.

Örnekler:

* Cihaz, Kuzey işaret eden üst cihazın (dikey modda) ile yan yana ekranı ile düz bir tabloda gerektiği zaman, iki koordinat sistemi hizalanır. `Quaternion` Değer (0, 0, 0, 1) kimlik dördey temsil eder. Tüm dönüşümüne göre bu konumunun çözümlenebilir.

* Cihaz ekranı bakan ve Batı, işaret eden üst cihazın (dikey modda) düz bir tabloda gerektiği zaman `Quaternion` değerdir (0, 0, 0.707, 0.707). Cihaz dünya Z ekseni etrafında 90 derece döndürülmüş.

* Cihaz, cihaz üst (dikey modda) sky işaret ve Kuzey cihazın arka yüzler dik tutulurken, 90 derece X ekseni etrafında olmuştur. `Quaternion` Değeridir (0.707, 0, 0, 0.707).

* Cihaz bir tabloda sol kenarından olan ve üst Kuzey işaret cihazı Döndürülmüş konumlandırılmış olursa &ndash;90 derece Y ekseni etrafında (veya negatif Y ekseni etrafında 90 derece). `Quaternion` Değerdir (0,-0.707, 0, 0.707).

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [OrientationSensor kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [OrientationSensor API belgeleri](xref:Xamarin.Essentials.OrientationSensor)
