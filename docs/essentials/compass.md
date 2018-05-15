---
title: Xamarin.Essentials pusula
description: Pusula sınıfı cihazın manyetik Kuzey başlık izlemenize olanak sağlar.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 39f141424ddd247458b9c8b35ae02ab29e2c2206
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials pusula

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Pusula** sınıfı cihazın manyetik Kuzey başlık izlemenize olanak sağlar.

## <a name="using-compass"></a>Pusula kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Pusula işlevselliği çalışır çağırarak `Start` ve `Stop` pusula değişiklikleri dinlemek için yöntemleri. Tüm değişiklikler geri aracılığıyla gönderilen `ReadingChanged` olay. Aşağıda bir örnek verilmiştir:

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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Algılayıcı hızı](xref:Xamarin.Essentials.SensorSpeed)

- **Hızlı** – algılayıcı verilerini (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) mümkün olduğunca hızlı alın.
- **Oyun** – (kullanıcı Arabirimi iş parçacığı üzerinde döndürülecek garantili) oyunlar için uygun oranı.
- **Normal** – varsayılan hızı ekran yönünü değişiklikleri için uygun.
- **UI** – genel kullanıcı arabirimi için uygun oranı.

## <a name="platform-implementation-specifics"></a>Platform uygulama özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android, pusula başlık almak için bir API sağlamaz. Biz accelerometer ve Google tarafından önerildiği manyetik Kuzey başlık hesaplamak için magnetometer kullanın. 

Algılayıcılar ayarlanması gerekir çünkü nadir durumlarda, belki de tutarsız sonuçlar, aygıtınızı bir Şekil 8 hareket taşınmasını içerir görürsünüz. Google haritalar açmak, konumunuz nokta'a dokunun ve seçmek için budur en iyi yolu biri **Ayarla pusula**.

Birden çok algılayıcılar uygulamanızdan aynı anda çalışan algılayıcı hızını ayarlayabilirsiniz unutmayın.

--------------

## <a name="api"></a>API

- [Kaynak kodu pusula](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Pusula API belgeleri](xref:Xamarin.Essentials.Compass)
