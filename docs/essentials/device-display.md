---
title: 'Xamarin.Essentials: Cihaz bilgilerini görüntüle'
description: Bu belgede uygulamanın çalıştığı cihazın ekran ölçümler sağlayan Xamarin.Essentials DeviceDisplay sınıfında açıklanmaktadır.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cb42da4c8c2d0e381a5b00f7e60da6f427d19c66
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353834"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Cihaz bilgilerini görüntüle

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**DeviceDisplay** sınıfı üzerinde uygulamayı çalıştırdığınız cihazın ekran ölçümler hakkında bilgi sağlar.

## <a name="using-devicedisplay"></a>DeviceDisplay kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Ekran ölçümleri

Temel cihaz bilgilerine ek olarak **DeviceDisplay** sınıfı cihazın ekran ve yönlendirme hakkında bilgi içerir.

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

**DeviceDisplay** sınıfı da herhangi bir ölçüm değişiklikleri ekran olduğunda tetiklenen abone bir olay gösterir:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanged += OnScreenMetricsChanged;
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
