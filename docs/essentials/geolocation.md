---
title: Xamarin.Essentials coğrafi konum
description: Coğrafi konuma sınıfı cihazın geçerli coğrafi konum koordinatlarını alma için API'ler sağlar.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ead498113f432e766fbd77ae2f01bc67c2273b60
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials coğrafi kodlama

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Coğrafi konuma** sınıfı, cihazın geçerli coğrafi konum koordinatlarını alma için API'ler sağlar.

## <a name="getting-started"></a>Başlarken

Erişim için **coğrafi konuma** işlevselliği aşağıdaki platform özel kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Kaba ve ince konumu izinleri gereklidir ve Android projesinde yapılandırılması gerekir. Ayrıca, uygulamanızı Android 5.0 (API düzeyi 21) hedefleyen veya, yüksek, bildirmelidir uygulamanızı donanım özellikleri bildirim dosyasında kullanır. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

YA da güncelleştirme Android bildirim:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve aşağıdaki içine ekleyin **bildirim** düğümü.

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Veya Anroid projeye sağ tıklayın ve projenin özelliklerini açın. Altında **Android derleme bildirimi** Bul **gerekli izinler:** alan ve onay **ACCESS_COARSE_LOCATION** ve **ACCESS_FINE_LOCATION**izinleri. Bu otomatik olarak güncelleştirilecek **AndroidManifest.xml** dosya.

# <a name="iostabios"></a>[iOS](#tab/ios)

Uygulamanızı anahtarlara sahip için gereklidir, **Info.plist** NSLocationWhenInUseUsageDescription cihazın konum erişebilmek için.

Plist Düzenleyicisi'ni açın ve eklemek **gizlilik - konum zaman içinde kullanım kullanım açıklaması** özelliğini ve kullanıcı görüntülemek için bir değer doldurun.

VEYA el ile dosyasını düzenleyin ve aşağıdakileri ekleyin:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ayarlamalısınız `Location` uygulama izni. Bu tarafından açın yapılabilir **Package.appxmanifest** ve selecing **yetenekleri** sekmesi ve denetleme **konumu**.

-----

## <a name="using-geolocation"></a>Coğrafi konuma kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Geoloation API de izinleri gerektiğinde ister.

Son bilinen alabilirsiniz [konumu](xref:Xamarin.Essentials.Location) arayarak cihazın `GetLastKnownLocationAsync` yöntemi. Bu, genellikle tam bir sorgu yapmaktan hızlıdır, ancak daha az doğru olabilir.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

Geçerli cihazın sorgulamak için [konumu](xref:Xamarin.Essentials.Location) koordinatları `GetLocationAsync` kullanılabilir. Tam olarak geçirmek için önerilen `GeolocationRequest` ve `CancellationToken` bu yana cihazın konumunu almak için biraz zaman alabilir.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

## <a name="geolocation-accuracy"></a>Coğrafi konuma doğruluğu

Aşağıdaki tabloda her platform doğruluğu özetlenmektedir

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
| UWP | < = 10 |

### <a name="best"></a>En iyi

| Platform | (Metre) cinsinden uzaklık |
| --- | --- |
| Android | 0 - 100 |
| iOS | ~0 |
| UWP | < = 10 |

## <a name="api"></a>API

- [Coğrafi konuma kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Coğrafi konuma API belgeleri](xref:Xamarin.Essentials.Geolocation)
