---
title: 'Xamarin.Essentials: pil'
description: Bu belgede cihazın pil bilgileri ve değişiklikleri izleyin kontrol etmenizi sağlayan Xamarin.Essentials pil sınıfında açıklanmaktadır.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1ed0ef5e013967545e739733c887702325f60c3f
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855061"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: pil

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Pil** sınıfı cihazın pil bilgileri ve değişiklikleri izleyin olanak tanır.

## <a name="getting-started"></a>Başlarken

Erişim için **pil** işlevselliğini aşağıdaki platforma özgü Kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` İzni gereklidir ve Android projede yapılandırılması gerekir. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Battery)]
```

YA da Android bildirimini güncelleştir:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve içine aşağıdaki **bildirim** düğümü.

```xml
<uses-permission android:name="android.permission.BATTERY" />
```

Anroid projeye sağ tıklayın ve proje özelliklerini açın. Altında **Android bildirim** Bul **gerekli izinler:** alan ve onay **pil** izni. Bu otomatik olarak güncelleştirecektir **AndroidManifest.xml** dosya.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulum gerekli.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulum gerekli.

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

Pilin özelliklerinden herhangi birini değiştirdiğinizde, bir olay tetiklenir:

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

## <a name="platform-differences"></a>Platform farklılıklarını

# <a name="androidtabandroid"></a>[Android](#tab/android)

Hiçbir platform farklılıklarını.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Cihaz API'leri test etmek için kullanılmalıdır. 
* Yalnızca döndürür `Ac` veya `Battery` için `PowerSource`. 
* Titreşim iptal etmek mümkün değildir.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* Yalnızca döndürür `Ac` veya `Battery` için `PowerSource`. 

-----

## <a name="api"></a>API

- [Pil kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Pil API belgeleri](xref:Xamarin.Essentials.Battery)
