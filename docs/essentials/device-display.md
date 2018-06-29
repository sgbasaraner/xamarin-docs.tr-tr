---
title: 'Xamarin.Essentials: Cihaz bilgilerini görüntüle'
description: Bu belgede, uygulamanın çalıştığı cihaz için ekran ölçümleri sağlar Xamarin.Essentials DeviceDisplay sınıfında açıklanmaktadır.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3060d56e14fb0d3801a96ec0fe6e24c9efda4dac
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080319"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Cihaz bilgilerini görüntüle

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

**DeviceDisplay** sınıfı ayrıca herhangi bir ölçüm değişiklikleri ekran olduğunda tetiklenen için abone bir olay sunar:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [DeviceDisplay kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API belgeleri](xref:Xamarin.Essentials.DeviceDisplay)
