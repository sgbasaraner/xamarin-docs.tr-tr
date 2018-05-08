---
title: Xamarin.Essentials pusula
description: Pusula sınıfı cihazın manyetik Kuzey başlık izlemenize olanak sağlar.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 9e0d0a709006a0d185a0c79b7b03d1672f06c4b6
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
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

- [Kaynak kodu pusula](https://github.com/xamarin/Essentials/tree/master/Essentials/Compass)
- [Pusula API belgeleri](xref:Xamarin.Essentials.Compass)
