---
title: Xamarin.Forms eşleme
description: Bu makalede, bir aşina kullanıcılar için deneyimi eşlemeleri sağlamak için her platformda yerel eşleme API'leri kullanmak için Xamarin.Forms eşlem sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: d74ad52a2926fb30a528aeba29156259390c3edf
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947250"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms eşleme

_Xamarin.Forms her platformda yerel eşleme API'leri kullanır._

Xamarin.Forms.Maps her platformda yerel eşleme API'leri kullanır. Bu, kullanıcılar için bir hızlı, tanıdık haritalar deneyimi sağlar, ancak her platformları belirli API gereksinimlerine uyacak şekilde bazı yapılandırma adımları gerekli olup olmadığını gösterir.
Yapılandırıldıktan sonra `Map` denetim herhangi bir Xamarin.Forms öğe ortak kod gibi çalışır.

* [Haritalar başlatma](#Maps_Initialization) - kullanarak `Map` başlangıçta ek başlatma kodu gerektirir.
* [Platform yapılandırmasını](#Platform_Configuration) -maps çalışmaya yönelik bazı yapılandırmaların her platform gerektirir.
* [Haritalar C# kullanarak](#Using_Maps) -eşler ve C# kullanarak sabitler görüntüleme.
* [XAML içinde haritalar'ı kullanarak](#Using_Xaml) -XAML sahip bir eşleme görüntüleme.

Harita denetiminin içinde kullanılan [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) ve aşağıda da gösterilen örnek.

 [![Maps'a MobileCRM örnek](map-images/maps-zoom-sml.png "harita denetimi örnek")](map-images/maps-zoom.png#lightbox "harita denetimi örneği")

Haritası işlevini daha gelişmiş oluşturarak bir [özel Oluşturucu harita](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Haritalar başlatma

Eşlemeleri bir Xamarin.Forms uygulaması eklerken **Xamarin.Forms.Maps** olan bir çözümde her proje için eklemeniz gereken ayrı bir NuGet paketi.
Android, bu da Xamarin.Forms.Maps eklediğinizde, otomatik olarak indirilen GooglePlayServices üzerinde (başka bir NuGet) bağımlılığı vardır.

NuGet paketini yükledikten sonra bazı başlatma kodu her uygulama projesinde gerekli *sonra* `Xamarin.Forms.Forms.Init` yöntem çağrısı. İOS için aşağıdaki kodu kullanın:

```csharp
Xamarin.FormsMaps.Init();
```

Android'de aynı parametreleri geçmelidir `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Evrensel Windows Platformu (UWP) için aşağıdaki kodu kullanın:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Bu çağrı her platform için aşağıdaki dosyaları ekleyin:

-  **iOS** -AppDelegate.cs dosya `FinishedLaunching` yöntemi.
-  **Android** -MainActivity.cs dosya `OnCreate` yöntemi.
-  **UWP** -MainPage.xaml.cs dosyasında `MainPage` Oluşturucusu.

NuGet paketi eklenmiştir ve başlatma yöntemi her applcation içinde bir kez `Xamarin.Forms.Maps` ortak .NET Standard kitaplığı projesi ya da paylaşılan proje kodunu API'leri kullanılabilir.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Platform yapılandırma

Harita görüntülenmeden önce bazı platformlarda ek yapılandırma adımları gereklidir.

### <a name="ios"></a>iOS

İos'ta konumu hizmetlerine erişmek için aşağıdaki anahtarları ayarlamanız gerekir **Info.plist**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – uygulama kullanımda olduğunda için konum hizmetlerini kullanma
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – her zaman konum Hizmetleri kullanma
- iOS 10 ve daha önceki
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – uygulama kullanımda olduğunda için konum hizmetlerini kullanma
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – her zaman konum Hizmetleri kullanma    

İOS 11 ve önceki desteklemek için tüm üç anahtarları içerebilir: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, ve `NSLocationAlwaysUsageDescription`.

Bu anahtarlar için XML gösterimi **Info.plist** aşağıda gösterilmiştir. Güncelleştirmeniz gerekir `string` konum bilgileri uygulamanızı nasıl kullandığını yansıtacak şekilde değerleri:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist** girişleri eklenebilir **kaynak** düzenlerken görünümü **Info.plist** dosyası:

![İOS 8 için Info.plist](map-images/ios8-map-permissions.png "iOS 8 gerekli Info.plist girişleri")


### <a name="android"></a>Android

Kullanılacak [Google haritalar API v2](https://developers.google.com/maps/documentation/android/) Android'de bir API anahtarı oluşturmanız ve Android projenize ekleyin.
Xamarin doc'ndaki yönergeleri takip edin [Google haritalar API v2 anahtarı edinme](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Bu yönergeleri uyguladıktan sonra API anahtarı yapıştırın **Properties/AndroidManifest.xml** dosyası (kaynağı görüntüle ve Bul/update öğesi):

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

Geçerli bir API anahtarı haritalar denetim android'de gri bir kutu olarak görüntüler.

> [!NOTE]
> Google haritalar erişmek, APK için sırada SHA-1 parmak izlerini eklemek ve gerekir, APK imzalamak için kullandığınız her keystore (hata ayıklama ve yayın) adlarını paketini, unutmayın. Hata ayıklama ve yayın APK oluşturmak için başka bir bilgisayar için bir bilgisayar kullanıyorsanız, örneğin, hata ayıklama keystore ilk bilgisayarın'den SHA-1 sertifika parmak izi ve yayın anahtar deposu ' den SHA-1 sertifika parmak izini içermelidir İkinci bilgisayar. Ayrıca, anahtar kimlik bilgilerini düzenlemek unutmayın uygulamanın **paket adı** değişiklikler. Bkz: [Google haritalar API v2 anahtarı edinme](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

Android proje üzerinde sağ tıklatıp seçerek uygun izinleri etkinleştirmeniz gerekecektir **Seçenekleri > derleme > Android uygulaması** ve aşağıdaki yolunda:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Bunlardan bazıları, aşağıdaki ekran görüntüsünde gösterilmiştir:

![Android için gerekli izinler](map-images/android-map-permissions.png "Android için gereken izinler")

Uygulamaları eşleme verileri indirmek için bir ağ bağlantısı gerektirdiğinden, son iki gereklidir. Android hakkında okuyun [izinleri](http://developer.android.com/reference/android/Manifest.permission.html) daha fazla bilgi için.

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Haritalar Evrensel Windows platformu üzerinde kullanılacak yetkilendirme belirteci oluşturmanız gerekir. Daha fazla bilgi için [haritalar kimlik doğrulama anahtarı istek](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) MSDN'de.

Kimlik doğrulama belirteci ardından belirtilmelidir `FormsMaps.Init("AUTHORIZATION_TOKEN")` Bing Haritalar ile uygulama kimliğini doğrulamak için yöntem çağrısı.

<a name="Using_Maps" />

## <a name="using-maps"></a>MAPS'ı kullanma

Bkz: [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) MobileCRM örneğinde harita denetiminin kodda nasıl kullanılabileceğini örneği için. Basit bir `MapPage` sınıfı bu - bildirim gibi görünebilir, yeni bir `MapSpan` haritanın görünümü konumlandırmak için oluşturulur:

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>Eşleme Türü

Harita içeriğinin ayarlayarak da değiştirilebilir `MapType` özelliği, normal bir sokak harita (varsayılan), uydu görüntüleri veya her ikisinin bir birleşiminde göstermek için.

```csharp
map.MapType == MapType.Street;
```

Geçerli `MapType` değerler şunlardır:

-  Karma
-  Uydu
-  Sokak (varsayılan)


### <a name="map-region-and-mapspan"></a>Harita bölge ve MapSpan

Yukarıdaki kod parçacığında gösterildiği gibi sağladığı bir `MapSpan` harita Oluşturucusu örneğine ayarlar başlangıç görünümü (noktası merkezi ve yakınlaştırma düzeyi) yüklendiğinde harita. `MoveToRegion` Eşlem sınıfı yöntemi harita konumu veya yakınlaştırma düzeyi değiştirildi için kullanılabilecek. Yeni bir oluşturmanın iki yolu vardır `MapSpan` örneği:

-  **MapSpan.FromCenterAndRadius()** -bir aralık oluşturmak için statik yöntem bir `Position` belirterek bir `Distance` .
-  **Yeni MapSpan ()** -kullanan Oluşturucusu bir `Position` ve Santigrat enlem ve boylamdan görüntülenecek.


Konumun değiştirmeden harita yakınlaştırma düzeyini değiştirmek için yeni bir oluşturma `MapSpan` geçerli konumdan kullanarak `VisibleRegion.Center` harita denetiminin özelliği. A `Slider` şöyle harita yakınlaştırma (harita denetimi doğrudan yakınlaştırma şu anda kaydırıcı değeri güncelleştirilemiyor ancak) denetlemek için kullanılabilir:

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Yakınlaştırma eşlemeleriyle](map-images/maps-zoom-sml.png "harita yakınlaştırma denetimi")](map-images/maps-zoom.png#lightbox "harita yakınlaştırma denetimi")

### <a name="map-pins"></a>PIN eşleme

Konumlar ile harita üzerinde işaretlenebilir `Pin` nesneleri.

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

 `PinType` PIN kodunda (platforma bağlı olarak) işlenen biçimini etkiler aşağıdaki değerlerden birine ayarlanabilir:

-  Genel
-  Yerleştir
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>XAML kullanarak

MAPS Bu kod parçacığında gösterildiği gibi Xaml düzenleri da yerleştirilebilir.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
    x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
            x:Name="MyMap"
            IsShowingUser="true"
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

`MapRegion` Ve `Pins` kod kullanılarak ayarlanabilir `MyMap` başvurusu (veya her şeyi harita olarak adlandırılır). Unutmayın, ek bir `xmlns` ad alanı tanımını Xamarin.Forms.Maps denetimlerine başvuruda için gereklidir.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Özet

Xamarin.Forms.Maps Xamarin.Forms çözümü'nde her projeye eklenmelidir ayrı bir NuGet olur. Ek başlatma kodu, iOS, Android ve UWP için de bazı yapılandırma adımları olarak gereklidir.

Bir kez yapılandırılmış Maps API, yalnızca birkaç satır kod PIN işaretli eşlemeleri oluşturmak için kullanılabilir. Haritalar daha gelişmiş ile bir [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>İlgili bağlantılar

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Harita özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
