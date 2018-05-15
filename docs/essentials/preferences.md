---
title: Xamarin.Essentials tercihleri
description: Tercihler sınıfı uygulama tercihlerini bir anahtar/değer deposuna kaydeder.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 882ff8e0f10948f4f88303f8bcc3d45b3cecf5fd
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials tercihleri

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Tercihler** uygulama Tercihler anahtar/değer deposunu sınıfı yardımcı olur.

## <a name="using-secure-storage"></a>Güvenli Depolama kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

İçin bir değer kaydetmek için bir verilen _anahtar_ tercihlerinde:

```csharp
Preferences.Set("my_key", "my_value");
```

Bir değeri tercihleri veya varsayılan değilse almak için ayarlayın:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Kaldırmak için _anahtar_ tercihlerden:

```csharp
Preferences.Remove("my_key");
```

Tüm tercihleri kaldırmak için:

```csharp
Preferences.Clear();
```

Ek olarak bu yöntemlerden her bir isteğe bağlı olarak ele `sharedName` tercih için ek kapsayıcılar oluşturmak için kullanılabilir. Aşağıdaki platform uygulama ayrıntıları okuyun.

## <a name="supported-data-types"></a>Desteklenen veri türleri

Aşağıdaki veri türlerini desteklenen **Tercihler**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**

## <a name="platform-implementation-specifics"></a>Platform uygulama özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

İçine depolanan tüm verileri [paylaşılan tercihleri](https://developer.android.com/training/data-storage/shared-preferences.html). Öyle değilse `sharedName` belirtilen varsayılan paylaşılan tercihleri kullanılır, adı almak için kullanılan başka bir **özel** Tercihler belirtilen ada sahip paylaşılan.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) iOS cihazlarda değerlerini depolamak için kullanılır. Öyle değilse `sharedName` belirtilen `StandardUserDefaults` olan kullanıldığında, başka adlı yeni bir oluşturmak için kullanılan `NSUserDefaults` için kullanılan belirtilen ada sahip `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) cihazda değerlerini depolamak için kullanılır. Öyle değilse `sharedName` belirtilen `LocalSettings` olan kullanıldığında, başka ad içinde yeni bir kapsayıcı oluşturmak için kullanılan `LocalSettings`.

--------------

## <a name="limitations"></a>Sınırlamalar

Bir dize depolarken, bu API küçük miktarda metin depolamak için tasarlanmıştır.  Performans metni büyük miktarlarda depolamak için kullanmayı denerseniz subpar olabilir.

## <a name="api"></a>API

- [Tercihler kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Tercihler API belgeleri](xref:Xamarin.Essentials.Preferences)
