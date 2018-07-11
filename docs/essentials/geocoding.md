---
title: 'Xamarin.Essentials: coğrafi kodlama'
description: Xamarin.Essentials coğrafi kodlama sınıfında API'ler için her iki geocode konumsal koordinatlara bir placemark sağlar ve bir placemark için coğrafi koordinatları ters.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 063adba82d96e7fcc64d7ec49a0c0133e1cef8ef
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831454"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: coğrafi kodlama

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Coğrafi kodlama** sınıfı bir placemark konumsal koordinatlara geocode için API'ler sağlar ve bir placemark için geocode coordincates ters.

## <a name="getting-started"></a>Başlarken

Erişim için **coğrafi kodlama** işlevselliğini aşağıdaki platforma özgü Kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Ek kurulum gerekli.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulum gerekli.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Bing Haritalar API'si anahtarı, coğrafi kodlama funcationality kullanmak için gereklidir. Kaydolmak için ücretsiz [Bing Haritalar](https://www.bingmapsportal.com/) hesabı. Altında **Hesabımı > anahtarlarımı** yeni bir anahtar oluşturun ve uygulama türüne göre bilgileri doldurun (olacağı **genel Windows uygulama (UWP, 8.x ve önceki sürümler)** UWP uygulamaları için).

İçinde erkenden herhangi çağırmadan önce uygulamanızın yaşam **coğrafi kodlama** yöntemleri API anahtarını ayarlayın:

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Coğrafi kodlama kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Başlama [konumu](xref:Xamarin.Essentials.Location) koordinatları adresi:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occured in geocoding
}
```

Yükseklik her zaman büyük/küçük harf kullanılamaz. Kullanılabilir değilse, `Altitude` özelliği olabilir `null` veya değerin sıfır olması. Yükseklik varsa, metre Deniz olarak değerdir. 

Başlama [placemarks](xref:Xamarin.Essentials.Placemark) koordinatları var olan bir dizi için:

```csharp
try
{
    var lat = 47.673988;
    var lon = -122.121513;

    var placemarks = await Geocoding.GetPlacemarksAsync(lat, lon);

    var placemark = placemarks?.FirstOrDefault();
    if (placemark != null)
    {
        var geocodeAddress =
            $"AdminArea:       {placemark.AdminArea}\n" +
            $"CountryCode:     {placemark.CountryCode}\n" +
            $"CountryName:     {placemark.CountryName}\n" +
            $"FeatureName:     {placemark.FeatureName}\n" +
            $"Locality:        {placemark.Locality}\n" +
            $"PostalCode:      {placemark.PostalCode}\n" +
            $"SubAdminArea:    {placemark.SubAdminArea}\n" +
            $"SubLocality:     {placemark.SubLocality}\n" +
            $"SubThoroughfare: {placemark.SubThoroughfare}\n" +
            $"Thoroughfare:    {placemark.Thoroughfare}\n";

        Console.WriteLine(geocodeAddress);
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

## <a name="distance-between-two-locations"></a>İki konum arasındaki uzaklığı

[ `Location` ](xref:Xamarin.Essentials.Location) Ve [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) sınıfları iki konum arasındaki mesafeyi hesaplamak için gereken yöntemleri tanımlar. Makaleye göz atın [ **Xamarin.Essentials: coğrafi konum** ](geolocation.md#calculate-distance) örneği.

## <a name="api"></a>API

- [Coğrafi kodlama kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Coğrafi kodlama API'si belgeleri](xref:Xamarin.Essentials.Geocoding)
