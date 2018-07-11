---
title: 'Xamarin.Essentials: tercihleri'
description: Bu belgede, uygulama tercihleri bir anahtar/değer deposuna kaydeder Xamarin.Essentials tercihleri sınıfında açıklanmaktadır. Bu sınıf ve depolanabilen veri türlerini nasıl kullanılacağını açıklar.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ca6d4f1ec60a80b483c79dd75267144e67d80c0b
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831770"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: tercihleri

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Tercihleri** sınıfı bir anahtar/değer deposuna uygulama tercihleri yardımcı olur.

## <a name="using-preferences"></a>Tercihler kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Kaydetmek için bir değer için bir verilen _anahtar_ tercihler:

```csharp
Preferences.Set("my_key", "my_value");
```

Aksi takdirde bir değer tercihleri ya da varsayılan almak için aşağıdakileri ayarlayın:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Kaldırılacak _anahtar_ gelen tercihleri:

```csharp
Preferences.Remove("my_key");
```

Tüm tercihleri kaldırmak için:

```csharp
Preferences.Clear();
```

Bu yöntemlerin yanı sıra her bir isteğe bağlı olarak ele `sharedName` tercih için ek bir kapsayıcı oluşturmak için kullanılabilir. Aşağıdaki platform uygulaması ayrıntıları okuyun.

## <a name="supported-data-types"></a>Desteklenen veri türleri

Aşağıdaki veri türleri desteklenir **tercihleri**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **Tarih/saat**

## <a name="implementation-details"></a>Uygulama Ayrıntıları

Değerleri `DateTime` tarafından tanımlanan iki yöntemi kullanarak (büyük tamsayı) 64 bit ikili biçimde depolanmış `DateTime` sınıfı: [ `ToBinary` ](xref:System.DateTime.ToBinary) kodlamak için kullanılan yöntemi `DateTime` değeri ve [ `FromBinary` ](xref:System.DateTime.FromBinary(System.Int64)) yöntemi değerin kodunu çözer. Değerleri bu yöntemleri için kodu çözülmüş yapılan ayarlamaları için belgelere bakın bir `DateTime` olduğu diğer bir deyişle Eşgüdümlü Evrensel Saat (UTC) değeri depolanır.

## <a name="platform-implementation-specifics"></a>Platform uygulaması özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

İçinde depolanan tüm verileri [paylaşılan tercihleri](https://developer.android.com/training/data-storage/shared-preferences.html). Hayır ise `sharedName` belirtilen varsayılan paylaşılan tercihleri kullanılır, adı almak için kullanılan başka bir **özel** tercihleri belirtilen ada sahip paylaşılan.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) iOS cihazlarında değerleri depolamak için kullanılır. Hayır ise `sharedName` belirtilen `StandardUserDefaults` olan kullanıldığında, aksi takdirde adı yeni oluşturmak için kullanılan `NSUserDefaults` için kullanılan belirtilen ada sahip `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) cihazda değerleri depolamak için kullanılır. Hayır ise `sharedName` belirtilen `LocalSettings` olan kullanıldığında, başka ad içinde yeni bir kapsayıcı oluşturmak için kullanılan `LocalSettings`.

--------------

## <a name="limitations"></a>Sınırlamalar

Bir dize depolarken, bu API küçük miktarlarda metini depolamak için tasarlanmıştır.  Performans, çok miktarda metin depolamak için kullanmayı denerseniz subpar olabilir.

## <a name="api"></a>API

- [Tercihler kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Tercihler API belgeleri](xref:Xamarin.Essentials.Preferences)
