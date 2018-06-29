---
title: 'Xamarin.Essentials: Güç enerji tasarrufu durumu'
description: Güç sınıfı cihaz düşük güç modunda çalışıp çalışmadığını belirlemek için enerji tasarrufu durumu elde etmek için bir program sağlar.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080414"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Güç enerji tasarrufu durumu

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Güç** sınıfı cihazı düşük güç modunda çalışıp çalışmadığını gösteren cihazın enerji tasarrufu durumu hakkında bilgi sağlar. Cihazın enerji tasarrufu durumu açıksa uygulamaları arka plan işleme kaçınmalısınız.

## <a name="background"></a>Arka Plan

Pille çalıştıran cihazlar düşük güç enerji tasarrufu moduna. Pil % 20 kapasite düştüğünde bazen aygıtlar bu moduna otomatik olarak, örneğin, geçiş. İşletim sistemi, pil deplete eğilimindedir etkinlikleri düşürerek enerji tasarrufu moduna yanıt verir. Uygulamalar, arka plan işleme veya diğer güçlü etkinlikler enerji tasarrufu modu açık olduğunda kaçınarak yardımcı olabilir.

Android cihazlar için **güç** sınıfı anlamlı bilgiler yalnızca Android sürüm 5.0 (Lolipop) ve yukarıdaki döndürür.

## <a name="using-the-power-class"></a>Güç sınıfını kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Statik kullanarak cihazı geçerli enerji tasarrufu durumunu elde `Power.EnergySaverStatus` özelliği:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Bu özellik bir üyesini döndürür `EnergySaverStatus` da numaralandırma `On`, `Off`, veya `Unknown`. Özelliği döndürürse `On`, uygulama arka plan işleme veya çok fazla güç tüketebilir diğer etkinlikler kaçınmalısınız.

Uygulama olay işleyicisi de yüklemeniz gerekir. **Güç** sınıfı enerji tasarrufu durumu değiştiğinde tetikleyen bir olay sunar:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanaged += OnEnergySaverStatusChanaged;
    }

    private void OnEnergySaverStatusChanaged(EnergySaverStatusChanagedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Enerji tasarrufu durumu geçerse `On`, uygulama arka plan işlemi gerçekleştirme durdurmanız gerekir. Durum geçerse `Unknown` veya `Off`, uygulama arka plan işleme devam edebilirsiniz.

## <a name="api"></a>API

- [Güç kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Güç API belgeleri](xref:Xamarin.Essentials.Power)
