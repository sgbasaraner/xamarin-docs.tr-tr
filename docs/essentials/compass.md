---
title: 'Xamarin.Essentials: pusula'
description: Bu belgede cihazın manyetik Kuzey başlık izlemenize izin verir Xamarin.Essentials pusula sınıfında açıklanmaktadır.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 63818014a9b3bdbef479055cbbcfbf8d348080fc
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080406"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: pusula

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

Olay işleyicisi UI iş parçacığında çalıştırmak ve kullanıcı arabirimi öğeleri, olay işleyicisi erişmesi gerekirse kullanmak için kesin değildir, [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) UI iş parçacığında bu kodu çalıştırmak için yöntem.

## <a name="platform-implementation-specifics"></a>Platform uygulama özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android, pusula başlık almak için bir API sağlamaz. Biz accelerometer ve Google tarafından önerildiği manyetik Kuzey başlık hesaplamak için magnetometer kullanın. 

Algılayıcılar ayarlanması gerekir çünkü nadir durumlarda, belki de tutarsız sonuçlar, aygıtınızı bir Şekil 8 hareket taşınmasını içerir görürsünüz. Google haritalar açmak, konumunuz nokta'a dokunun ve seçmek için budur en iyi yolu biri **Ayarla pusula**.

Birden çok algılayıcılar uygulamanızdan aynı anda çalışan algılayıcı hızını ayarlayabilirsiniz unutmayın.

--------------

## <a name="api"></a>API

- [Kaynak kodu pusula](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Pusula API belgeleri](xref:Xamarin.Essentials.Compass)
