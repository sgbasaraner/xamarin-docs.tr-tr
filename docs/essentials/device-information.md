---
title: Xamarin.Essentials aygıt bilgileri
description: Cihaz uygulama hakkında bilgi üzerinde çalıştığı Deviceınfo sınıf sağlar.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a398f2697b03cec26f2c786b11e7a3c332cb5f63
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials aygıt bilgileri

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Deviceınfo** sınıfı üzerinde uygulamayı çalıştırdığınız aygıt hakkında bilgi sağlar.

## <a name="using-deviceinfo"></a>Deviceınfo kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Aşağıdaki bilgiler, API aracılığıyla sunulur:

```csharp
// Device Model (SMG-950U)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## <a name="platformsxrefxamarinessentialsdeviceinfoplatforms"></a>[Platformlar](xref:Xamarin.Essentials.DeviceInfo.Platforms)

`DeviceInfo.Platform` işletim sistemine eşleyen bir sabit dize hatalarla ilintilidir. Değerleri ile denetlenebilir `Platforms` sınıfı:

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – desteklenmeyen

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Deyimleri](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` karşılık gelen cihaz türü için uygulama eşlemeleri bir sabit dize çalıştığı. Değerleri ile denetlenebilir `Idioms` sınıfı:

- **DeviceInfo.Idioms.Phone** – telefon
- **DeviceInfo.Idioms.Tablet** – Tablet
- **DeviceInfo.Idioms.Desktop** – Masaüstü
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Idioms.Unsupported** – desteklenmeyen

## <a name="device-type"></a>Cihaz Türü

`DeviceInfo.DeviceType` Uygulama işletim sistemlerini çalıştıran bir fiziksel veya sanal aygıt olup olmadığını belirlemek için bir numaralandırma hatalarla ilintilidir. Bir sanal aygıt benzeticisi veya öykünücüsü ' dir.

## <a name="api"></a>API

- [Deviceınfo kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Deviceınfo API belgeleri](xref:Xamarin.Essentials.DeviceInfo)
