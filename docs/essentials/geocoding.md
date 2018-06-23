---
title: 'Xamarin.Essentials: coğrafi kodlama'
description: Xamarin.Essentials coğrafi kodlama sınıfında API'ler hem geocode placemark konumsal koordinatlarına sağlar ve bir placemark ters geocode koordinatları.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 001cca2524e495d64c6781d8a2fc5cb58e771e6e
ms.sourcegitcommit: 0be3d10bf08d1f76eab109eb891ed202615ac399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321450"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: coğrafi kodlama

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Coğrafi kodlama** sınıfı placemark konumsal koordinatlarına geocode için API'ler sağlar ve bir placemark geocode coordincates ters.

## <a name="getting-started"></a>Başlarken

Erişim için **coğrafi kodlama** işlevselliği aşağıdaki platform özel kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Ek kurulumu gerekmez.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulumu gerekmez.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Bing Haritalar API'si anahtarı coğrafi kodlama funcationality kullanmak için gereklidir. Kaydolmak için ücretsiz bir [Bing Haritalar](https://www.bingmapsportal.com/) hesabı. Altında **Hesabımı > My anahtarları** yeni bir anahtar oluşturun ve uygulama türüne göre bilgileri doldurun (olacağı **ortak Windows uygulaması (UWP, 8.x ve önceki sürümler)** UWP uygulamalar için).

Önceden herhangi çağırmadan önce uygulamanızın yaşam içinde **coğrafi kodlama** yöntemlerini API anahtarını ayarlayın:

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Coğrafi kodlama kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Alma [konumu](xref:Xamarin.Essentials.Location) koordinatları adresi:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

Alma [placemarks](xref:Xamarin.Essentials.Placemark) koordinatları var olan bir dizi için:

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

## <a name="api"></a>API

- [Coğrafi kodlama kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Coğrafi kodlama API belgeleri](xref:Xamarin.Essentials.Geocoding)
