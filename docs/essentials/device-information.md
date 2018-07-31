---
title: 'Xamarin.Essentials: Cihaz bilgileri'
description: Bu belgede uygulamayı çalıştıran cihaz hakkındaki bilgileri sağlayan Xamarin.Essentials Deviceınfo sınıfında açıklanmaktadır.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 18fe081372cc190e5ead2045f36d63652f8702c3
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353808"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: Cihaz bilgileri

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Deviceınfo** sınıfı üzerinde uygulamayı çalıştırdığınız cihaz hakkındaki bilgileri sağlar.

## <a name="using-deviceinfo"></a>Deviceınfo kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Aşağıdaki bilgileri, API aracılığıyla sunulur:

```csharp
// Device Model (SMG-950U, iPhone10,6)
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

`DeviceInfo.Platform` işletim sistemine eşleyen bir sabit dize belirtilirler. Değerleri ile denetlenebilir `Platforms` sınıfı:

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – desteklenmeyen

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Deyimleri](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` koreluje s cihaz türü için uygulama eşlemeleri bir sabit dize çalışıyor. Değerleri ile denetlenebilir `Idioms` sınıfı:

- **DeviceInfo.Idioms.Phone** – telefon
- **DeviceInfo.Idioms.Tablet** – Tablet
- **DeviceInfo.Idioms.Desktop** – Masaüstü
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Idioms.Unsupported** – desteklenmeyen

## <a name="device-type"></a>Cihaz Türü

`DeviceInfo.DeviceType` Uygulama bir fiziksel veya sanal cihazda çalışıp çalışmadığını belirlemek için bir numaralandırma ilişkilendirir. Sanal cihaz simülatörü veya öykünücü ' dir.

## <a name="platform-implementation-specifics"></a>Platform uygulaması özellikleri

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS geliştiricileri belirli iOS cihaz adını almak için bir API kullanıma sunmuyor. Donanım tanımlayıcı gibi döndürülür _iPhone10, 6_ iPhone X ifade eder. Bu tanımlayıcıların ilişkin bir eşleme, Apple tarafından sağlanan değil, ancak bulunabilir [Wiki iPhone](https://www.theiphonewiki.com/wiki/Models) (resmi olmayan kaynak kaynağı).

--------------

## <a name="api"></a>API

- [Deviceınfo kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Deviceınfo API belgeleri](xref:Xamarin.Essentials.DeviceInfo)
