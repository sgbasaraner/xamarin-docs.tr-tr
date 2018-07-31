---
title: 'Xamarin.Essentials: Compass'
description: Bu belgede cihazın manyetik Kuzey başlık izlemenize imkan tanıyan Xamarin.Essentials Compass sınıfında açıklanmaktadır.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c3fe98c384a87bdc08ce94e7537d1a6343767561
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353889"
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
    SensorSpeed speed = SensorSpeed.UI;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(object sender, CompassChangedEventArgs e)
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
            // Some other exception has occurred
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

## <a name="low-pass-filter"></a>Düşük geçiş filtresi

Nasıl nedeniyle Android compass değerler güncelleştirildi ve hesaplanan değerleri kesintisiz gerekli olabilir. A _düşük geçirmek filtre_ uygulanabilir açıları Sinüs ve Kosinüs değerlerini alır ve ayarlayarak açılabilir `ApplyLowPassFilter` özelliği `Compass` sınıfı:

```csharp
Compass.ApplyLowPassFilter = true;
```

Bu, yalnızca Android platformunda uygulanır. Daha fazla bilgi okuyun [burada](https://github.com/xamarin/Essentials/pull/354#issuecomment-405316860).

--------------

## <a name="api"></a>API

- [Kaynak kodu compass](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Pusula API belgeleri](xref:Xamarin.Essentials.Compass)
