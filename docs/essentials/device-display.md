---
title: Xamarin.Essentials aygıt bilgilerini görüntüle
description: Cihazın ekran ölçümler uygulama hakkında bilgi üzerinde çalıştığı DeviceDisplay sınıf sağlar.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 570fb6cf3eadfbbc7e189f875498d3cea0bc1514
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials aygıt bilgilerini görüntüle

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**DeviceDisplay** sınıfı üzerinde uygulamayı çalıştırdığınız cihazın ekran ölçümleri hakkında bilgi sağlar.

## <a name="using-devicedisplay"></a>DeviceDisplay kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Ekran ölçümleri

Temel aygıt bilgilerine ek olarak **DeviceDisplay** sınıfı cihazın ekran ve yönlendirme hakkında bilgiler içerir.

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

**DeviceDisplay** de ortaya çıkarır ve ölçüm değişiklikleri ekran herhangi bir olay tetikler için abone olay sınıfı:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChanagedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [DeviceDisplay kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Essentials/DeviceDisplay)
- [DeviceDisplay API belgeleri](xref:Xamarin.Essentials.DeviceDisplay)
