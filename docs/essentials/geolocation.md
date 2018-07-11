---
title: 'Xamarin.Essentials: coğrafi konum'
description: Bu belgede cihazın geçerli coğrafi konum koordinatlarını almak için API'leri sağlayan Xamarin.Essentials coğrafi konum sınıfında açıklanmaktadır.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 11749107403fc99e1d49b63ee3b50ff105abaa57
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848757"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials: coğrafi konum

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Coğrafi konum** sınıfı, cihazın geçerli coğrafi konum koordinatlarını almak üzere API'ler sağlar.

## <a name="getting-started"></a>Başlarken

Erişim için **coğrafi konum** işlevselliği, aşağıdaki platforma özgü Kurulum gereklidir:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Kaba ve ince konumu izinler gereklidir ve Android projede yapılandırılması gerekir. Ayrıca, uygulamanızı Android 5.0 (API düzey 21) hedefleyen veya, sonraki bildirmelidir uygulamanızı donanım özellikleri bildirim dosyasında kullanır. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Ya da Android bildirimini güncelleştir:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve içine aşağıdaki **bildirim** düğüm:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Android projesine sağ tıklayın ve proje özelliklerini açın. Altında **Android bildirim** Bul **gerekli izinler:** alan ve onay **ACCESS_COARSE_LOCATION** ve **ACCESS_FINE_LOCATION**izinleri. Bu otomatik olarak güncelleştirecektir **AndroidManifest.xml** dosya.

# <a name="iostabios"></a>[iOS](#tab/ios)

Uygulamanızın **Info.plist** içermelidir `NSLocationWhenInUseUsageDescription` cihazın konumuna erişmek için anahtar.

Plist Düzenleyicisi'ni açın ve eklemek **gizlilik - konum zaman içinde kullanımı açıklaması** özelliği ve kullanıcıya görüntülemek için bir değer girin.

El ile veya dosyasını düzenleyin ve aşağıdakileri ekleyin:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ayarlamalısınız `Location` uygulama izni. Bu açarak yapılabilir **Package.appxmanifest** ve selecing **özellikleri** sekmesi ve denetimi **konumu**.

-----

## <a name="using-geolocation"></a>Coğrafi konum kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Geoloation API aynı zamanda kullanıcı için izinler gerekli olduğunda ister.

En son bilinen alabilirsiniz [konumu](xref:Xamarin.Essentials.Location) arayarak cihazın `GetLastKnownLocationAsync` yöntemi. Bu genellikle daha sonra tam sorgu yapmaktan hızlıdır, ancak daha az doğru.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

Yükseklik her zaman büyük/küçük harf kullanılamaz. Kullanılabilir değilse, `Altitude` özelliği olabilir `null` veya değerin sıfır olması. Yükseklik varsa, metre Deniz olarak değerdir. 

Geçerli cihazın sorgulamak için [konumu](xref:Xamarin.Essentials.Location) koordinatları `GetLocationAsync` kullanılabilir. Tam olarak geçirmek en iyi `GeolocationRequest` ve `CancellationToken` olduğundan bu cihazın konumunu almak için biraz zaman alabilir.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

## <a name="geolocation-accuracy"></a>Coğrafi konum doğruluğu

Aşağıdaki tabloda, her platformun doğruluğu özetlenmektedir:

### <a name="lowest"></a>En düşük

| Platform | (Metre) cinsinden uzaklık |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000 - 5000 |

### <a name="low"></a>Düşük

| Platform | (Metre) cinsinden uzaklık |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300 - 3000 |

### <a name="medium-default"></a>Orta (varsayılan)

| Platform | (Metre) cinsinden uzaklık |
| --- | --- |
| Android | 100 - 500 |
| iOS | 100 |
| UWP | 30-500 |

### <a name="high"></a>Yüksek

| Platform | (Metre) cinsinden uzaklık |
| --- | --- |
| Android | 0 - 100 |
| iOS | 10 |
| UWP | < 10 = |

### <a name="best"></a>En iyi

| Platform | (Metre) cinsinden uzaklık |
| --- | --- |
| Android | 0 - 100 |
| iOS | ~0 |
| UWP | < 10 = |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>İki konum arasındaki uzaklığı

[ `Location` ](xref:Xamarin.Essentials.Location) Ve [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) sınıflarını tanımla `CalculateDistance` iki coğrafi konumlar arasındaki uzaklık hesaplayın olanak tanıyan yöntemler. Bu hesaplanan uzaklık yollar veya diğer yollarla dikkate almaz ve yalnızca kısa yüzeyi boyunca iki noktaları, dünya arasında olarak da bilinen uzaklıktır _great daire uzaklık_ veya mikroişlemciye, uzaklık "olarak crow uçarak."

Örnek buradadır:

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

`Location` Oluşturucusu bu sırayla enlem ve boylam bağımsız değişkeni vardır. Enlem değerleri ekvatorun kuzeyinde olduğunu ve pozitif boylam değerleri asal Meridyen doğuya doğru pozitif. Son bağımsız değişkeni kullanın `CalculateDistance` mil veya kilometre belirtmek için. `Location` Sınıfı da tanımlar `KilometersToMiles` ve `MilesToKilometers` iki birim arasında dönüştürmek için yöntemleri.

## <a name="api"></a>API

- [Coğrafi konum kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Coğrafi konum API belgeleri](xref:Xamarin.Essentials.Geolocation)
