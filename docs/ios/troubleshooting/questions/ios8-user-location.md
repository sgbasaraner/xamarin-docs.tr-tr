---
title: Kullanıcı konumu iOS 8 çalışmıyor
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9BE92C99-C9C5-427E-ADE4-789DF258BACE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: f6597fc3a2476de2aa7f191810e5dbaf2f6903bb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776072"
---
# <a name="user-location-not-working-in-ios-8"></a>Kullanıcı konumu iOS 8 çalışmıyor

Bir metin düzenleyicisi içindeki: açın, Info.plist ayarlama ve aşağıdakileri ekleyin:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>You are about to use location!</string>

<key>NSLocationAlwaysUsageDescription</key>
<string>This will be called if location is used behind the scenes</string>
```

Ve MainViewController.cs içinde aşağıdaki çağırmanız gerekir:

```csharp
iPhoneLocationManager.RequestWhenInUseAuthorization ();
```

ÖRNEĞİN:

```cs
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    iPhoneLocationManager.RequestWhenInUseAuthorization ();
    iPhoneLocationManager.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) => {
        UpdateLocation (mainScreen, e.Locations [e.Locations.Length - 1]);
        };
} else {
    // this won't be called on iOS 6 (deprecated)
    iPhoneLocationManager.UpdatedLocation += (object sender, CLLocationUpdatedEventArgs e) => {
        UpdateLocation (mainScreen, e.NewLocation);
        };
}
```
