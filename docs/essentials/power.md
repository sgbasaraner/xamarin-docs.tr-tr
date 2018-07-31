---
title: 'Xamarin.Essentials: Güç enerji tasarrufu durumu'
description: Cihaz düşük güç modunda çalışıp çalışmadığını belirlemek için enerji tasarrufu durumunu almak bir program Power class sağlar.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 760a305280269734034a817182a8c2a07894ca2b
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353496"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Güç enerji tasarrufu durumu

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Güç** sınıfı cihaz düşük güç modunda çalışıp çalışmadığını gösteren cihazın enerji tasarrufu durumu hakkında bilgi sağlar. Uygulamalar, cihazın enerji tasarrufu durumu üzerinde ise arka plan işlemesi kaçınmanız gerekir.

## <a name="background"></a>Arka Plan

Pille çalıştıran cihazlara bir enerji koruyucu düşük güç moduna koyabilirsiniz. Pil % 20 kapasitenin altında düştüğünde bazen cihazlar bu moduna otomatik olarak, örneğin, etkinleştirildi. İşletim sistemi, pil deplete eğilimindedir etkinlikleri düşürerek enerji tasarrufu modu için yanıt verir. Uygulamalar, arka plan işlemesi veya diğer güçlü etkinlikleri enerji tasarrufu modu açık olduğunda önleyerek yardımcı olabilir.

Android cihazları için **güç** sınıfı anlamlı bilgiler yalnızca Android sürüm 5.0 (Lollipop) ve üzeri döndürür.

## <a name="using-the-power-class"></a>Güç sınıfını kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

' Using static cihaz enerji tasarrufu geçerli durumunu elde `Power.EnergySaverStatus` özelliği:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Bu özellik bir üyesini döndürür `EnergySaverStatus` ya da sabit listesi `On`, `Off`, veya `Unknown`. Özellik döndürürse `On`, uygulama arka plan işlemesi veya çok fazla gücü tüketebilir diğer etkinlikleri kaçınmanız gerekir.

Uygulama, bir olay işleyicisi de yüklemeniz gerekir. **Güç** sınıfı enerji tasarrufu durumu değiştiğinde tetikleyen bir olay gösterir:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Enerji tasarrufu durumu değişirse `On`, uygulama arka plan işlemleri gerçekleştirme durdurmanız gerekir. Durumu değişirse `Unknown` veya `Off`, uygulama arka plan işleme devam edebilirsiniz.

## <a name="api"></a>API

- [Güç kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Güç API belgeleri](xref:Xamarin.Essentials.Power)
