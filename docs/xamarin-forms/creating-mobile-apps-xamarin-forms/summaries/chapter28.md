---
title: Bölüm 28 özeti. Konum ve haritalar
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 28 özeti. Konum ve haritalar'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a02239906f5a30c068cb7eebd31308ad188696b3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998104"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Bölüm 28 özeti. Konum ve haritalar

Xamarin.Forms destekleyen bir [ `Map` ](xref:Xamarin.Forms.Maps.Map) türetilen öğesi `View`. Haritalar kullanan özel platform gereksinimleri nedeniyle ayrı bir derlemede uygulanmış **Xamarin.Forms.Maps**ve farklı bir ad alanı içerir: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Coğrafi koordinat sistemi

Bir coğrafi koordinat sistemi dünya gibi küresel (veya neredeyse küresel) nesnesindeki konumlarını tanımlar. Her ikisi de bir koordinat oluşan bir *enlem* ve *boylam* açıların cinsinden ifade edilir.

Büyük daire adlı `equator` , kavramsal olarak dünya ekseni genişleten iki kutupları arasında midway olduğu.

### <a name="parallels-and-latitude"></a>Parallels ve enlem

Satırları eşit enlem olarak bilinen bir açının Kuzey veya yer işaretlerinin Merkezi'nden ekvatorun dünya ölçülen *parallels*. Bu aralıktan ekvatorun 0 derece Kuzey ve Güney kutupları 90 derece için. Kural olarak, ekvatorun kuzeyinde latitudes pozitif değerler ve negatif değerler ekvatorun dünya olanlardır.

### <a name="longitude-and-meridians"></a>Boylam ve meridyenler

Güney kutbu için mükemmel daire Kuzey kutup gelen yarısının olan satırları eşit boylam olarak da bilinen *meridyenleri*. Greenwich, İngiltere'deki asal Meridyen göre şunlardır. Kural gereği, longitudes asal Meridyen doğuya doğru pozitif değeri 0 derece 180 derece ve doğuya longitudes negatif değerleri 0 dereceye &ndash;180 derece.

### <a name="the-equirectangular-projection"></a>Equirectangular projeksiyonu

Herhangi bir düz eşlemesini dünya deformasyonları tanıtır. Enlem ve boylamdan tüm satırları düz ve enlem ve boylam açıları eşit farklılıkları için harita üzerinde eşit uzaklıkta karşılık geliyorsa, sonucu bir *equirectangular projeksiyon*. Bu harita, çünkü bunlar yatay uzatılır kutupları yakın alanlara deforme eder.

### <a name="the-mercator-projection"></a>Mercator izdüşümünü

Popüler *Mercator izdüşümünü* yatay uzatma de bu alanlar dikey yayarak için telafi dener. Bu, bir harita alanları kutupları yakın gerçekten olduklarından, ancak herhangi bir yerel oldukça yakın gerçek alan ile uyumlu çok büyük göründüğü sonuçlanır.

### <a name="map-services-and-tiles"></a>Harita Hizmetleri ve kutucukları

Harita kullanmanıza adlı Mercator izdüşümünü çeşitlemesi `Web Mercator`. Harita Hizmetleri, konum ve yakınlaştırma düzeyini temel alan bir istemciye bit eşlem kutucukları sunun.

## <a name="getting-the-users-location"></a>Kullanıcının konumunu alma

Xamarin.Forms `Map` sınıfları kullanıcının coğrafi konum elde etmek için bir özellik içermiyor, ancak haritaları ile bu nedenle bağımlı hizmet çalışma işlemesi gerekir, bu genellikle tercih edilir.

### <a name="the-location-tracker-api"></a>API konumu İzleyicisi

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) çözüm konumu İzleyicisi API kodunu içerir. [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) Yapısı, enlem ve boylam kapsüller. [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) Arabirimi başlatmak ve konumu İzleyicisi ve yeni bir konum kullanılabilir olduğunda bir olay duraklatmak için iki yöntem tanımlar.

#### <a name="the-ios-location-manager"></a>İOS konumu Yöneticisi

İOS uygulaması `ILocationTracker` olduğu bir [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) sağlar sınıfını kullanmak iOS [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/).

#### <a name="the-android-location-manager"></a>Android konumu Yöneticisi

Android uygulaması `ILocationTracker` olduğu bir [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) kullanır Android sınıfında [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) sınıfı.

#### <a name="the-windows-runtime-geo-locator"></a>Windows çalışma zamanı coğrafi Bulucu

Windows çalışma zamanı uygulamasını `ILocationTracker` olduğu bir [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) kullanır UWP sınıfı [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534).

### <a name="display-the-phones-location"></a>Telefonun konumunu görüntülemek

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) örnek telefonun konum, hem metin hem de bir equirectangular haritada görüntülemek için konumu İzleyicisi'ni kullanır.

### <a name="the-required-overhead"></a>Gerekli ek yükü

Bazı ek yükler için gerekli olan **WhereAmI** konumu İzleyicisi'ni kullanmak için. İlk olarak, tüm projeleri **WhereAmI** çözüm içinde karşılık gelen projelerine başvurular içermelidir **Xamarin.FormsBook.Platform**ve her **WhereAmI** proje çağırmalıdır `Toolkit.Init` yöntemi.

Bazı ek platforma özgü yükünü yeri izinleri biçiminde gereklidir.

#### <a name="location-permission-for-ios"></a>İOS için konum izni

İOS, **Info.plist** dosyası, bu kullanıcının konumunu alma izin vermek için soran soru metnini içeren öğeleri içermelidir.

#### <a name="location-permissions-for-android"></a>Android için yeri izinleri

Kullanıcının konumuna elde android uygulamaları AndroidManifest.xml dosyasında bir ACCESS_FILE_LOCATION iznine sahip olmalıdır.

#### <a name="location-permissions-for-the-windows-runtime"></a>Windows çalışma zamanı için yeri izinleri

Bir Windows veya Windows Phone Uygulama olmalıdır bir `location` cihaz yeteneği Package.appxmanifest dosyasını işaretlenmiş.

## <a name="working-with-xamarinformsmaps"></a>Xamarin.Forms.Maps ile çalışma

Çeşitli gereksinimleri kullanarak söz konusu `Map` sınıfı.

### <a name="the-nuget-package"></a>NuGet paketi

**Xamarin.Forms.Maps** NuGet kitaplığı uygulama çözüme eklenmesi gerekir. Sürüm numarası aynı olmalıdır **Xamarin.Forms** paket şu anda yüklü.

### <a name="initializing-the-maps-package"></a>Haritalar paket başlatılıyor

Uygulama projeleri çağırmalıdır `Xamarin.FormsMaps.Init` yöntemi çağrısını yapmadan sonra `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Harita Hizmetleri'ni etkinleştirme

Çünkü `Map` kullanıcının konumuna elde edebilir, bu bölümde açıklanan şekilde kullanıcı izinlerini uygulama edinmeniz gerekir:

#### <a name="enabling-ios-maps"></a>İOS etkinleştirme eşler.

Bir iOS uygulaması kullanarak `Map` Info.plist iki satırı gerekiyor.

#### <a name="enabling-android-maps"></a>Android etkinleştirme eşler.

Yetkilendirme anahtarı, Google harita hizmetleri kullanmak için gereklidir. Bu anahtar eklenen **AndroidManifest.xml** dosya. Ayrıca, **AndroidManifest.xml** dosyası gerektirir `manifest` kullanıcının konumuna alınırken ilgili etiketler.

#### <a name="enabling-windows-runtime-maps"></a>Windows çalışma zamanı etkinleştirme eşler.

Bir Windows çalışma zamanı uygulaması, Bing Haritalar'ı kullanmak için yetkilendirme anahtarı gerektirir. Bu anahtar için bağımsız değişken olarak geçirilen `Xamarin.FormsMaps.Init` yöntemi. Uygulama için konum Hizmetleri etkinleştirilmiş olması da gerekir.

### <a name="the-unadorned-map"></a>Eksiz eşleme

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) örnek oluşur bir [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) dosya ve [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) arka plan kod dosyası çeşitli tanıtım programlar gezinme sağlar.

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) dosyasını nasıl görüntüleneceğini gösterir [ `Map` ](xref:Xamarin.Forms.Maps.Map) görünümü. Varsayılan olarak, city Roma gösterir, ancak harita kullanıcı tarafından yönetilebilir.

Yatay ve dikey kaydırmayı devre dışı bırakmak için ayarlanmış [ `HasScrollEnabled` ](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) özelliğini `false`. Yakınlaştırmayı devre dışı bırakmak için ayarlanmış [ `HasZoomEnabled` ](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) için `false`. Bu özellikler, tüm platformlarda çalışmayabilir.

### <a name="streets-and-terrain"></a>Sokaklar ve Arazi

Haritalar farklı türde ayarlayarak görüntüleyebilirsiniz `Map` özelliği [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) türü [ `MapType` ](xref:Xamarin.Forms.Maps.MapType), üç üyesi olan bir sabit listesi:

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street), varsayılan
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) dosyası eşleme türü seçmek için bir radyo düğmesi kullanmayı gösterir. Bunu kullanır [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı ile bir sınıf temel alarak [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) dosya.

### <a name="map-coordinates"></a>Harita koordinatlarına göre

Bir program, geçerli alan elde `Map` aracılığıyla görüntüleme [ `VisibleRegion` ](xref:Xamarin.Forms.Maps.Map.VisibleRegion) özelliği. Bu özellik *değil* bağlanabilir bir özellik tarafından desteklenir ve ne zaman değişti, büyük olasılıkla bir zamanlayıcı özelliği izlemek istediği bir program bu amaç için kullanması gereken şekilde göstermek için bildirim mekanizması yoktur.

`VisibleRegion` tür [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan), dört salt okunur özelliklere sahip bir sınıfı:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center) türü [`Position`](xref:Xamarin.Forms.Maps.Position)
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees) tür `double`, harita görüntülenen alanının yüksekliğini belirten
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) tür `double`, harita görüntülenen alan genişliğini belirten
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius) tür [ `Distance` ](xref:Xamarin.Forms.Maps.Distance), harita üzerinde görünür büyük döngüsel alanı boyutunu belirten

`Position` ve `Distance` hem yapılardır. `Position` aracılığıyla ayarlanan iki salt okunur özelliklerini tanımlar [ `Position` Oluşturucusu](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double)):

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` Ölçüm ve İngilizce birimleri arasında dönüştürerek bir birim bağımsız mesafe sağlamak amacını taşımaktadır. A `Distance` değeri çeşitli yollarla oluşturulabilir:

- [`Distance` Oluşturucu](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double)) ölçümleri bir uzaklığı ile
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) statik yöntemi
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) statik yöntemi
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) statik yöntemi

Değer, üç özelliklerinden kullanılabilir:

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters) türü `double`
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers) türü `double`
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles) türü `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) dosyasını içeren birkaç `Label` öğeleri görüntülemek için `MapSpan` bilgileri. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) arka plan kod dosyası bilgileri kullanıcı Haritası işleyen olarak güncel tutmak için bir zamanlayıcı kullanır.

### <a name="position-extensions"></a>Konum uzantıları

Yeni bir kitaplık için adlı kitabın [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) eşlemeye özgü ancak platformdan bağımsız türleri içerir. [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Sınıfında bir `ToString` yöntemi `Position`ve ikisi aralıklarını hesaplamak için bir yöntem `Position` değerleri.

### <a name="setting-an-initial-location"></a>Bir başlangıç konumu ayarlama

Çağırabilirsiniz [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) yöntemi `Map` programlı olarak harita üzerinde bir konum ve yakınlaştırma düzeyi ayarlamak için. Bağımsız değişken türünde `MapSpan`. Oluşturabileceğiniz bir `MapSpan` aşağıdakilerden birini kullanarak:

- [`MapSpan` Oluşturucu](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) ile bir `Position`ve enlem ve boylam yayılma
- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) ile bir `Position` ve RADIUS

Yeni bir mümkündür `MapSpan` yöntemleri kullanarak var olan bir gelen [ `ClampLatitude` ](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) veya [ `WithZoom` ](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double)).

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) dosya ve [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) arka plan kod dosyası nasıl kullanılacağını gösterir `MoveToRegion` , durumu Wyoming görüntülemek için yöntemi.

Alternatif olarak kullanabileceğiniz [ `Map` Oluşturucusu](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) ile bir `MapSpan` harita konumu başlatmak için nesne. [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) dosyasını Xamarin'in headquarters San Francisco içinde görüntülemek için tamamen XAML içinde bunun nasıl yapılacağını gösterir.

### <a name="dynamic-zooming"></a>Dinamik Yakınlaştırma

Kullanabileceğiniz bir `Slider` dinamik olarak bir harita yakınlaştırma. [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) dosya ve [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) arka plan kod dosyası show dayalı bir harita yarıçapını değiştirme `Slider` değeri.

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) dosya ve [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) arka plan kod dosyası Android'de daha iyi çalışır, alternatif bir yaklaşım gösterir, ancak her iki yaklaşım da Windows üzerinde çalışır. Platform.

### <a name="the-phones-location"></a>Telefonun konumu

[ `IsShowingUser` ](xref:Xamarin.Forms.Maps.Map.IsShowingUser) Özelliği `Map` biraz farklı üç platformlarında çalışır [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) dosyası gösterir:

- İOS, mavi bir nokta telefonun konumu gösterir. ancak, el ile var. gitmeniz gerekir
- Android'de bir simge görüntülenir olduğunda gönderilen taşıma telefonun konumuna eşleme
- UWP için iOS benzer, ancak bazı durumlarda otomatik olarak konumuna gider.

**MapDemos** proje temel bir simge tabanlı düğmesi tanımlayarak Android yaklaşım taklit girişimlerini [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) dosya ve [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) arka plan kod dosyası.

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) dosya ve [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) arka plan kod dosyası telefonun konuma gitmek için bu düğmeyi kullanın.

### <a name="pins-and-science-museums"></a>PIN ve Bilim müzelerin

Son olarak, `Map` sınıfı tanımlayan bir [ `Pins` ](xref:Xamarin.Forms.Maps.Map.Pins) türünün özelliği `IList<Pin>`. [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) Sınıf dört özellikleri tanımlar:

- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) tür `string`' gerekli özelliği
- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) tür `string`, bir isteğe bağlı okunabilir adresi
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) tür `Position`, PIN haritada görüntülendiği belirten
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) tür [ `PinType` ](xref:Xamarin.Forms.Maps.PinType), kullanılmayan bir sabit listesi

**MapDemos** proje dosyasını içeren [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), Amerika Birleşik Devletleri'nde bilimi müzelerin listeler ve [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) ve [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) bu verileri seri durumdan çıkarmak için sınıflar.

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) dosya ve [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) arka plan kod dosyası görünen pin eşlemesindeki bu bilimi müzelerin için. Kullanıcı bir PIN dokunduğunda, adresi ve bir Web sitesi için müze görüntüler.

### <a name="the-distance-between-two-points"></a>İki nokta arasındaki uzaklığı

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Sınıfı içeren bir [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) yöntemi ile basitleştirilmiş bir hesaplama iki coğrafi konumlar arasındaki uzaklık.

Bu kullanılan [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) dosya ve [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) uzaklığı için müze kullanıcının konumundan da görüntülemek için arka plan kod dosyası:

[![Üçlü sayfasının ekran görüntüsü yerel müzelerin](images/ch28fg28-small.png "uzaklığı bir konuma")](images/ch28fg28-large.png#lightbox "konumuna uzaklık")

Program, ayrıca dinamik olarak harita konumunu temel alarak sayısını kısıtlamak nasıl gösterir.

## <a name="geocoding-and-back-again"></a>Coğrafi kodlama ve yeniden

[ **Xamarin.Forms.Maps** ](xref:Xamarin.Forms.Maps) derlemeyi de içeren bir [ `Geocoder` ](xref:Xamarin.Forms.Maps.Geocoder) sınıfıyla birlikte bir [ `GetPositionsForAddressAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) dönüştüren yöntemi sıfır veya daha olası coğrafi konumlar ve başka bir yöntem metin adresine [ `GetAddressesForPositionAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) diğer yönde dönüştürür.

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) dosya ve [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) arka plan kod dosyası, bu özelliği gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 28 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Bölüm 28 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Harita denetimi](~/xamarin-forms/user-interface/map.md)
