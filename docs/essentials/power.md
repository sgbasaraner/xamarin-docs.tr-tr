---
title: 'Xamarin.Essentials: Güç enerji tasarrufu durumu'
description: Cihaz düşük güç modunda çalışıp çalışmadığını belirlemek için enerji tasarrufu durumunu almak bir program Power class sağlar.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831526"
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
        Power.EnergySaverStatusChanaged += OnEnergySaverStatusChanaged;
    }

    private void OnEnergySaverStatusChanaged(EnergySaverStatusChanagedEventArgs e)
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
