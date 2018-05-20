---
title: Xamarin.Essentials pil
description: Pil sınıfı, cihazın pil bilgi denetleyip yapılan değişiklikleri izleyin olanak sağlar.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 171f65f0ab26faaacddbd8fef37056d7f892d973
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2018
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials pil

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Pil** sınıfı cihazın pil bilgi yapılan değişiklikleri izleyin denetleyip olanak sağlar.

## <a name="getting-started"></a>Başlarken

Erişim için **pil** işlevselliği aşağıdaki platform özel kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` İzni gereklidir ve Android projesinde yapılandırılması gerekir. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Battery)]
```

YA da güncelleştirme Android bildirim:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve aşağıdaki içine ekleyin **bildirim** düğümü.

```xml
<uses-permission android:name="android.permission.BATTERY" />
```

Veya Anroid projeye sağ tıklayın ve projenin özelliklerini açın. Altında **Android derleme bildirimi** Bul **gerekli izinler:** alan ve onay **pil** izni. Bu otomatik olarak güncelleştirilecek **AndroidManifest.xml** dosya.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulumu gerekmez.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulumu gerekmez.

-----

## <a name="using-battery"></a>Pil kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Geçerli pil bilgilere bakın:

```csharp
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or -1.0 if unable to determine.

var state = Battery.State;

switch (state)
{
    case BatteryState.Charging:
        // Currently charging
        break;
    case BatteryState.Full:
        // Battery is full
        break;
    case BatteryState.Discharging:
    case BatteryState.NotCharging:
        // Currently discharging battery or not being charged
        break;
    case BatteryState.NotPresent:
        // Battery doesn't exist in device (desktop computer)
    case BatteryState.Unknown:
        // Unable to detect battery state
        break;
}

var source = Battery.PowerSource;

switch (source)
{
    case BatteryPowerSource.Battery:
        // Being powered by the battery
        break;
    case BatteryPowerSource.Ac:
        // Being powered by A/C unit
        break;
    case BatteryPowerSource.Usb:
        // Being powered by USB cable
        break;
    case BatteryPowerSource.Wireless:
        // Powered via wireless charging
        break;
    case BatteryPowerSource.Unknown:
        // Unable to detect power source
        break;
}
```

Bir olay pil 's özelliklerinden herhangi birini değiştirmek tetiklenir:

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>Platform farklar

| Platform | Fark |
| --- | --- |
| iOS | Cihaz, API'ları sınamak için kullanılmalıdır. |
| iOS | Yalnızca Ac veya pil için PowerSource döndürür. |
| UWP | Yalnızca Ac veya pil için PowerSource döndürür. |

## <a name="api"></a>API

- [Pil kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Pil API belgeleri](xref:Xamarin.Essentials.Battery)
