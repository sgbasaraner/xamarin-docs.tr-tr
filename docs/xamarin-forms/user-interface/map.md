---
title: eşleme
description: Xamarin.Forms her platformda yerel eşlemesi API'lerini kullanır.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: f78b16a99d8bc828e26bb6aecdb67d4ba07e18d4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="map"></a>eşleme

_Xamarin.Forms her platformda yerel eşlemesi API'lerini kullanır._

Xamarin.Forms.Maps her platformda yerel eşlemesi API'lerini kullanır. Bu kullanıcılar için hızlı ve tanıdık eşlemeleri deneyimi sağlar, ancak her platformları belirli API gereksinimlerine uyacak şekilde bazı yapılandırma adımları gereklidir anlamına gelir.
Bir kez yapılandırılmış `Map` kontrol ortak kodun başka herhangi bir Xamarin.Forms öğenin gibi çalışır.

* [Eşlemeleri başlatma](#Maps_Initialization) - kullanarak `Map` başlangıçta ek başlatma kodu gerektirir.
* [Platform Yapılandırması](#Platform_Configuration) -her platform çalışmaya eşlemeleri için bazı yapılandırma gerektirir.
* [MAPS C# kullanarak](#Using_Maps) -eşler ve C# kullanarak sabitler görüntüleme.
* [XAML'de haritalar kullanılarak](#Using_Xaml) -bir eşleme XAML ile görüntüleme.

Harita denetiminin içinde kullanılan [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) aşağıda gösterilen örnek.

 [![MAPS MobileCRM örnekteki](map-images/maps-zoom-sml.png "Harita Denetim örnek")](map-images/maps-zoom.png#lightbox "Harita Denetim örneği")

Harita daha gelişmiş işlevlere oluşturarak bir [özel Oluşturucu eşleme](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>MAPS başlatma

MAPS bir Xamarin.Forms uygulaması eklerken **Xamarin.Forms.Maps** olan bir çözümdeki her projeye eklemelisiniz ayrı bir NuGet paketi.
Android, bu da Xamarin.Forms.Maps eklediğinizde, otomatik olarak karşıdan GooglePlayServices üzerinde (başka bir NuGet) bir bağımlılığa sahiptir.

NuGet paketini yükledikten sonra bazı başlatma kod her uygulama projesi gereklidir *sonra* `Xamarin.Forms.Forms.Init` yöntem çağrısı. İOS için aşağıdaki kodu kullanın:

```csharp
Xamarin.FormsMaps.Init();
```

Android aynı parametreleri geçmelidir `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Windows çalışma zamanı (WinRT) ve evrensel Windows Platformu (UWP) için aşağıdaki kodu kullanın:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Bu çağrı her platform için aşağıdaki dosyaları ekleyin:

-  **iOS** -AppDelegate.cs dosya `FinishedLaunching` yöntemi.
-  **Android** -MainActivity.cs dosya `OnCreate` yöntemi.
-  **WinRT ve UWP** -MainPage.xaml.cs dosyasında `MainPage` Oluşturucusu.

NuGet paketi eklendi ve başlatma yöntemi her applcation içinde bir kez `Xamarin.Forms.Maps` API'leri ortak PCL ya da paylaşılan proje kod içinde kullanılabilir.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Platform yapılandırması

Harita görüntüler önce bazı platformlarda ek yapılandırma adımları gereklidir.

### <a name="ios"></a>iOS

İOS 7 Harita Denetim "yalnızca works", bu nedenle uzun olarak `FormsMaps.Init()` çağrı yapılır.

İOS 8 iki anahtar eklenmesi gerekir **Info.plist** dosya: [ `NSLocationAlwaysUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) ve [ `NSLocationWhenInUseUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26). XML gösterimi aşağıda gösterilen - güncelleştirmeniz gerekir `string` uygulamanızı konum bilgileri nasıl kullanarak yansıtmak için değerler:

```xml
<key>NSLocationAlwaysUsageDescription</key>
    <string>Can we use your location</string>
<key>NSLocationWhenInUseUsageDescription</key>
    <string>We are using your location</string>
```

**Info.plist** girişleri da eklenebilir **kaynak** düzenlerken Görünüm **Info.plist** dosyası:

![İOS 8 için Info.plist](map-images/ios8-map-permissions.png "iOS 8 gerekli Info.plist girişleri")


### <a name="android"></a>Android

Kullanılacak [Google haritalar API'si v2](https://developers.google.com/maps/documentation/android/) Android bir API anahtarı oluşturmak ve bunu Android projenize eklemeniz gerekir.
Xamarin belge'ndaki yönergeleri izleyin [Google haritalar API'si v2 anahtarı alma](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Bu yönergeleri uyguladıktan sonra API anahtarını yapıştırın **Properties/AndroidManifest.xml** dosyası (kaynağı görüntüle ve Bul/güncelleştirme aşağıdaki öğeyi):

```xml
<meta-data android:name="com.google.android.maps.v2.API_KEY"
            android:value="AbCdEfGhIjKlMnOpQrStUvWValueGoesHere" />
```

Geçerli bir API anahtarı eşlemeleri denetim Android üzerinde gri bir kutu olarak görüntüler.

> [!NOTE]
> Karşıya yüklenen herhangi bir uygulama sürümünü imzalamak için kullanılan anahtar deposu dosyasının kullanarak başka bir anahtar oluşturmak unutmayın Google Play mağazası. Anahtarı geliştirme için oluşturmak ve hata ayıklama çalışmaz ve Google Play'den yüklenen uygulamasını harita görünümü bozuk. Ayrıca uygulamanın anahtar Eğer yeniden oluşturulması unutmayın **paket adı** değişiklikler.

Ayrıca uygun izinleri Android projeye sağ tıklayıp seçerek etkinleştirmeniz gerekir **Seçenekleri > Yapı > Android uygulaması** ve aşağıdaki ticking:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Bunlardan bazıları aşağıdaki ekran gösterilir:

![Android için gerekli izinlere](map-images/android-map-permissions.png "Android için gereken izinler")

Son iki gerekli olduğu uygulamalar harita verilerini indirmek için bir ağ bağlantısı gerektirir. Android hakkında okuyun [izinleri](http://developer.android.com/reference/android/Manifest.permission.html) daha fazla bilgi için.

### <a name="windows-runtime-and-universal-windows-platform"></a>Windows çalışma zamanı ve evrensel Windows platformu

Windows çalışma zamanı ve evrensel Windows platformu eşlemeleri kullanmak için bir yetkilendirme belirteci oluşturmanız gerekir. Daha fazla bilgi için bkz: [eşlemeleri kimlik doğrulama anahtarı isteği](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) MSDN'de.

Kimlik doğrulama belirteci sonra belirtilmelidir `FormsMaps.Init("AUTHORIZATION_TOKEN")` Bing Haritalar ile uygulama kimliğini doğrulamak için yöntem çağrısı.

<a name="Using_Maps" />

## <a name="using-maps"></a>Eşlemelerini kullanma

Bkz: [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) MobileCRM örnek kodda harita denetiminin nasıl kullanılabileceği bir örnek için. Basit bir `MapPage` sınıfı bu - bildirimin gibi görünebilir, yeni bir `MapSpan` haritanın görünüm konumlandırmak için oluşturulur:

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

Harita içeriğinin ayarlayarak da değiştirilebilir `MapType` normal sokak haritası (varsayılan), uydu görüntüler veya her ikisinin birleşimini göstermek için özellik.

```csharp
map.MapType == MapType.Street;
```

Geçerli `MapType` değerler şunlardır:

-  Karma
-  Uydu
-  Sokak (varsayılan)


### <a name="map-region-and-mapspan"></a>Harita bölge ve MapSpan

Yukarıdaki kod parçacığında gösterildiği gibi sağladığı bir `MapSpan` map Oluşturucusu örneğine ayarlar ilk görünümü (noktası merkezi ve yakınlaştırma düzeyi) yüklendiğinde harita. `MoveToRegion` Eşleme sınıf yöntemi kullanılabilecek eşleme konumu ya da Yakınlaştırma düzeyini değiştirme hedefi. Yeni bir oluşturmanın iki yolu vardır `MapSpan` örneği:

-  **MapSpan.FromCenterAndRadius()** - static method to create a span from a  `Position` and specifying a  `Distance` .
-  **Yeni MapSpan ()** -kullanan Oluşturucusu bir `Position` ve enlem ve boylam görüntülenecek Santigrat.


Konumun değiştirmeden harita yakınlaştırma düzeyini değiştirmek için yeni bir oluşturma `MapSpan` geçerli konumundan kullanarak `VisibleRegion.Center` harita denetiminin özelliği. A `Slider` (doğrudan eşleme denetiminde yakınlaştırma şu anda kaydırıcıyı değeri güncelleştirilemiyor ancak) harita yakınlaştırma şöyle denetlemek için kullanılabilir:

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Yakınlaştırma Eşlemleriyle](map-images/maps-zoom-sml.png "harita yakınlaştırma denetimi")](map-images/maps-zoom.png#lightbox "harita yakınlaştırma denetimi")

### <a name="map-pins"></a>PIN eşleme

Konumları işaretlenir ile harita üzerinde `Pin` nesneleri.

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

 `PinType` PIN kodunda (platformuna bağlı olarak) çizilir şekilde etkileyebilir aşağıdaki değerlerden birini ayarlanabilir:

-  Genel
-  Yerleştir
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>XAML kullanma

MAPS bu parçacığında gösterildiği gibi Xaml düzenleri de yerleştirilebilir.

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

`MapRegion` Ve `Pins` kodu kullanılarak ayarlanabilir `MyMap` başvurusu (veya her eşleme olarak adlandırılır). Unutmayın ek bir `xmlns` ad alanı tanımını Xamarin.Forms.Maps denetimlerine başvuruda için gereklidir.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Özet

Xamarin.Forms.Maps Xamarin.Forms çözümdeki her projeye eklenmelidir ayrı bir NuGet ' dir. Ek başlatma kodu, iOS, Android, WinRT ve UWP iyi bazı yapılandırma adımlarını olarak gereklidir.

Bir kez yapılandırılan eşlemeleri API, yalnızca birkaç kodunun PIN işaretli eşlemeleri işlemek için kullanılabilir. Eşlemeleri daha gelişmiş ile bir [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>İlgili bağlantılar

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Harita özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
