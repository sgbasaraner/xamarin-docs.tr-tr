---
title: 'Xamarin.Essentials: Compass'
description: Bu belgede cihazın manyetik Kuzey başlık izlemenize imkan tanıyan Xamarin.Essentials Compass sınıfında açıklanmaktadır.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cf41948c55c742140896bfb48d9bb4abf25c8d68
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947419"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: Compass

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Compass** sınıfı, cihazın manyetik Kuzey başlık izlemenize olanak tanır.

## <a name="using-compass"></a>Compass kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Compass işlevselliği çalışır çağırarak `Start` ve `Stop` compass değişiklikleri dinlemek için yöntemleri. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Aşağıda bir örnek verilmiştir:

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(CompassChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: {data.HeadingMagneticNorth} degrees");
        // Process Heading Magnetic North
    }

    public void ToggleCompass()
    {
        try
        {
            if (Compass.IsMonitoring)
              Compass.Stop();
            else
              Compass.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Some other exception has occured
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>Platform uygulaması özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android, pusula almak için bir API sağlamaz. Biz, ivme ölçer ve magnetometer Google tarafından önerildiği manyetik Kuzey başlığı hesaplamak için kullanır. 

Algılayıcılar ayarlanması gerektiği için ender durumlarda, belki de tutarsız sonuçlar Cihazınızı bir Şekil 8 hızda taşımak kapsamaktadır görürsünüz. Google haritalar'ı açın, konumunuz için nokta dokunun ve budur en iyi yolu **Ayarla compass**.

Uygulamanızdan aynı anda birden çok sensörlerden çalıştıran algılayıcı hızını ayarlayabilirsiniz dikkat edin.

--------------

## <a name="api"></a>API

- [Kaynak kodu compass](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Pusula API belgeleri](xref:Xamarin.Essentials.Compass)
