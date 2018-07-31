---
title: Xamarin.Essentials eşlemeleri
description: Xamarin.Essentials haritalar sınıfında belirli bir konuma veya placemark yüklü haritalar uygulamayı açmak için bir uygulama sağlar.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 445e2da84e9a9aaf1ce4d836af11cfba963b8cbb
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353920"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials: eşlemeleri

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Eşler** sınıfı belirli bir konuma veya placemark yüklü haritalar uygulamayı açmak için bir uygulama sağlar.

## <a name="using-maps"></a>MAPS'ı kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Haritalar işlevselliği çalışır çağırarak `OpenAsync` yöntemiyle `Location` veya `Placemark` ile isteğe bağlı açmak için `MapsLaunchOptions`.

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(location, options);
    }
}
```

İle açarken bir `Placemark` aşağıdaki bilgiler gereklidir:

* `CountryName`
* `AdminArea`
* `Thoroughfare`
* `Locality`

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var placemark = new Placemark
            {
                CountryName = "United States",
                AdminArea = "WA",
                Thoroughfare = "Microsoft Building 25",
                Locality = "Redmond"
            };
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>Uzantı Metotları

Bir başvuru zaten varsa bir `Location` veya `Placemark` yerleşik genişletme yöntemini kullanabilirsiniz `OpenMapsAsync` isteğe bağlı `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="platform-differences"></a>Platform farklılıklarını

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `MapDirectionsMode` desteklenmez ve hiçbir etkisi olmaz.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `MapDirectionsMode` Haritalar Uygulaması açıldığında varsayılan yönü modu ayarlamak için desteklenir.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `MapDirectionsMode` desteklenmez ve hiçbir etkisi olmaz.

--------------

## <a name="platform-implementation-specifics"></a>Platform uygulaması özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android kullanan `geo:` cihazda haritalar uygulamayı başlatmak için URI şeması. Bu, kullanıcının bu URI şeması destekleyen varolan bir uygulamadan seçmesine izin isteyebilir.  Google Haritalar ile bu düzenini destekleyen Xamarin.Essentials test edilir.

# <a name="iostabios"></a>[iOS](#tab/ios)

Hiçbir platform belirli uygulama ayrıntıları.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Hiçbir platform belirli uygulama ayrıntıları.

--------------

## <a name="api"></a>API

- [Haritalar kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Haritalar API belgeleri](xref:Xamarin.Essentials.Maps)
