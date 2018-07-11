---
title: 'Xamarin.Essentials: MainThread'
description: Kod ana yürütme iş parçacığı üzerinde çalıştırılacak uygulamalar MainThread sınıfı sağlar.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831431"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**MainThread** sınıfı kod yürütme ana iş parçacığı üzerinde çalıştırılacak uygulamalar sağlar ve belirli bir kod bloğu belirlemek için ana iş parçacığı üzerinde çalışmakta olduğu.

## <a name="background"></a>Arka Plan

Çoğu işletim sistemi: iOS, Android ve evrensel Windows platformu gibi — tek iş parçacıklı model için kullanıcı arabirimi içeren kodu kullanın. Bu model, düzgün bir şekilde tuş vuruşları dahil olmak üzere, kullanıcı arabirimi olaylarının seri hale getirmek ve dokunma girişini gereklidir. Bu iş parçacığı genellikle adlandırılır _ana iş parçacığı_ veya _kullanıcı arabirimi iş parçacığı_ veya _UI iş parçacığı_. Bu model bir dezavantajı, kullanıcı arabirimi öğeleri erişen tüm kod uygulamanın ana iş parçacığı üzerinde çalıştırmanız gerektiğini olmasıdır. 

Uygulamaları bazen yürütme İkincil iş parçacığı üzerinde olay işleyicisi çağırma olayları kullanmanız gerekir. (Xamarin.Essentials sınıfları [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), ve [ `OrientationSensor` ](orientation-sensor.md) hız ile kullanıldığında bir ikincil iş parçacığı üzerinde tüm bilgileri döndürebilir.) Olay işleyicisi kullanıcı arabirimi öğeleri erişmesi gerekiyorsa, ana iş parçacığında kod çalıştırmanız gerekir. **MainThread** sınıfı, ana iş parçacığında bu kodu çalıştırmak uygulama olanak tanır.

## <a name="running-code-on-the-main-thread"></a>Ana iş parçacığı üzerinde çalışan kod

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Ana iş parçacığında kod çalıştırmak için statik çağrı `MainThread.BeginInvokeOnMainThread` yöntemi. Bağımsız değişken bir [ `Action` ](xref:System.Action) bağımsız değişkenler ve dönüş değeri ile yalnızca bir yöntemi olan nesne:

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Ana iş parçacığı üzerinde çalışması gereken kod için ayrı bir yöntemi tanımlamak da mümkündür:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

İçinde başvurarak ana iş parçacığı üzerinde bu yöntem çalıştırabilirsiniz `BeginInvokeOnMainThread` yöntemi:

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms adlı bir yöntem olan [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) aynı şeyi yapan `MainThread.BeginInvokeOnMainThread(Action)`. Xamarin.Forms uygulaması içinde her iki yöntem kullanabilirsiniz, ancak çağıran kod Xamarin.Forms üzerinde herhangi bir bağımlılık gereksinimini sahip olup olmadığını göz önünde bulundurun. Aksi halde `MainThread.BeginInvokeOnMainThread(Action)` büyük olasılıkla daha iyi bir seçenektir.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Kod ana iş parçacığı üzerinde çalışıp çalışmadığını belirleme

`MainThread` Sınıfı, belirli bir kod bloğunun ana iş parçacığı üzerinde çalışıp çalışmadığını belirlemek bir uygulama da sağlar. `IsMainThread` Özelliği döndürür `true` özelliği çağırma kodu ana iş parçacığı üzerinde çalışıyorsa. Bir program, bu özellik, ana iş parçacığı veya bir ikincil iş parçacığı için farklı kod çalıştırmak için kullanabilirsiniz:

```csharp
if (MainThread.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

Kod çağırmadan önce ikincil bir iş parçacığı üzerinde çalışıp çalışmadığı iade merak ediyor `BeginInvokeOnMainThread`, örneğin, şöyle:

```csharp
if (MainThread.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Kod bloğunun ana iş parçacığı üzerinde çalışıyorsa bu onay performansı iyileştirebilir şüpheli.

_Ancak, bu denetim gerekli değildir._ Platform uygulamalarını `BeginInvokeOnMainThread` ana iş parçacığı üzerinde çağrı yapılırsa, kontrol edin. Eğer çok az performans cezasına sebep olduğu `BeginInvokeOnMainThread` olduğunda, gerçekten gerekli değildir.

## <a name="api"></a>API

- [MainThread kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread API belgeleri](xref:Xamarin.Essentials.MainThread)
