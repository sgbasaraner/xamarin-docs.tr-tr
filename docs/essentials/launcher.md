---
title: Xamarin.Essentials Başlatıcısı
description: Xamarin.Essentials Başlatıcısı sınıfında sistem tarafından bir URI'yı açmak için bir uygulama sağlar.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 252bb873c1494265aafb2285057490ca29ce7419
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573639"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: Başlatıcısı

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Başlatıcısı** sınıfı sistem tarafından bir URI'yı açmak için bir uygulama sağlar. Bu genellikle ayrıntılı zaman başka bir uygulamanın özel URI düzenleri bağlama kullanılır. Bir Web tarayıcısı açın aradığınız sonra başvurmanız gerekir **[tarayıcı](open-browser.md)** API.

## <a name="using-launcher"></a>Başlatıcı kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Başlatıcı işlevi çağrısı kullanılacak `OpenAsync` yöntemi ve geçirin bir `string` veya `Uri` açın. İsteğe bağlı olarak, `CanOpenAsync` yöntemi, URI şeması cihazdaki bir uygulama tarafından işlenebilen olmadığını denetlemek için kullanılabilir.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="api"></a>API

- [Başlatıcı kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Başlatıcı API belgeleri](xref:Xamarin.Essentials.Launcher)
