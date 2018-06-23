---
title: Xamarin.Essentials Platform
description: Ana yürütme iş parçacığı üzerinde kodu çalıştırmak uygulamaların Platform sınıfı sağlar.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E0
author: charlespetzold
ms.author: chape
ms.date: 06/03/2018
ms.openlocfilehash: 82e248ee702e104dff98b342aec72179273fc34f
ms.sourcegitcommit: d9ecac62bcf743aff52408fbbd09c5509921a000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36329939"
---
# <a name="xamarinessentials-platform"></a>Xamarin.Essentials Platform

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Platform** sınıfı kodu yürütme ana iş parçacığı üzerinde çalışacak uygulamaları sağlar ve belirli bir kod bloğunu belirlemek için şu anda ana iş parçacığı üzerinde çalışıyor.

## <a name="background"></a>Arka Plan

Çoğu işletim sistemi — iOS, Android ve evrensel Windows platformu dahil — tek iş parçacığı model kullanıcı arabirimi içeren kodunu kullanın. Bu model, düzgün şekilde tuş vuruşları dahil olmak üzere kullanıcı arabirimi olayları seri hale getirmek ve dokunma girişini gereklidir. Bu iş parçacığı adlandırılırlar _ana iş parçacığı_ veya _kullanıcı arabirimi iş parçacığı_ veya _kullanıcı Arabirimi iş parçacığı_. Bu model bir dezavantajı, kullanıcı arabirimi öğeleri erişen tüm kod uygulamanın ana iş parçacığı üzerinde çalıştırmalısınız olmasıdır. 

Uygulamalar, bazen bir ikincil yürütme iş parçacığı üzerinde olay işleyicisi çağırma olayları kullanmanız gerekir. (Xamarin.Essentials sınıfları [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), ve [ `OrientationSensor` ](orientation-sensor.md) tüm bilgileri ile yüksek hız kullanıldığında ikincil bir iş parçacığı üzerinde döndürebilir.) Kullanıcı arabirimi öğeleri olay işleyicisi erişmesi gerekirse ana iş parçacığı üzerinde bu kodu çalıştırmanız gerekir. **Platform** sınıfı, ana iş parçacığında bu kodu çalıştırmak uygulamayı olanak tanır.

## <a name="running-code-on-the-main-thread"></a>Ana iş parçacığı üzerinde çalışan kod

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Ana iş parçacığı üzerinde kodu çalıştırmak için statik çağrı `Platform.BeginInvokeOnMainThread` yöntemi. Bağımsız değişken bir [ `Action` ](xref:System.Action) yalnızca bağımsız değişken içermeyen ve hiçbir değer döndürmeyen bir yöntemle nesne:

```csharp
Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Xamarin.Forms uygulaması türeyen bir sınıf içinde bu yöntemi çağırmak gerekiyorsa `Element` (türetilen herhangi bir sınıf içerir `View` veya `Page`), `Platform` sınıf adı ile tam olması gerekir ad alanı adı:

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Ana iş parçacığı üzerinde çalışmalıdır kodu için ayrı bir yöntem tanımlamak mümkündür:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

İçinde başvurarak bu yöntem ana iş parçacığı üzerinde sonra çalıştırabilirsiniz `BeginInvokeOnMainThread` yöntemi:

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms sahip olarak adlandırılan bir yöntem [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) aynı şeyi yapar `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)`. Her iki yöntem bir Xamarin.Forms uygulaması kullanabilmenize karşın, çağrıyı yapan kod Xamarin.Forms üzerinde herhangi bir bağımlılık gereksinimini sahip olup olmadığını göz önünde bulundurun. Aksi durumda, `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)` büyük olasılıkla daha iyi bir seçenektir.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Kod ana iş parçacığı üzerinde çalışıp çalışmadığını belirleme

`Platform` Sınıfı ayrıca belirli bir kod bloğunun ana iş parçacığı üzerinde çalışıp çalışmadığını belirlemek bir uygulama sağlar. `IsMainThread` Özelliği döndürür `true` özelliği çağırma kodu ana iş parçacığı üzerinde çalışıyorsa. Bir program bu özellik, ana iş parçacığı veya ikincil bir iş parçacığı için farklı kodu çalıştırmak için kullanabilirsiniz:

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

Size kodu çağırmadan önce ikincil bir iş parçacığı üzerinde çalışıp çalışmadığını denetleyip denetlemeyeceğini merak ediyor `BeginInvokeOnMainThread`, örneğin, şöyle:

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Kod bloğunu ana iş parçacığı üzerinde zaten çalışıyorsa, bu onay performansı iyileştirebilir şüpheleniyorsanız.

_Ancak, bu denetimi gerekli değildir._ Platform uygulamaları `BeginInvokeOnMainThread` çağrı ana iş parçacığı üzerinde yaptıysanız kontrol edin. Çağırırsanız çok az performans cezası olduğu `BeginInvokeOnMainThread` zaman onu gerçekten gerekli değildir.

## <a name="api"></a>API

- [Platform kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Platform)
- [Platform API belgeleri](xref:Xamarin.Essentials.Platform)
