---
title: "Bölüm 28 özeti. Konum ve eşlemeleri"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7361f65fecfed9d61b9df7088f9021ffa0192ad8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Bölüm 28 özeti. Konum ve eşlemeleri

Xamarin.Forms destekleyen bir [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) türetilen öğesi `View`. Maps kullanarak söz konusu özel platform gereksinimleri nedeniyle ayrı bir derlemede uygulanan **Xamarin.Forms.Maps**ve farklı bir ad alanı içermelidir: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Coğrafi koordinat sistemi

Bir coğrafi koordinat sistemi dünya gibi küresel (veya neredeyse küresel) nesnesinde konumlarını tanımlar. Her ikisi de bir koordinat oluşan bir *enlem* ve *boylam* açıları ifade.

Mükemmel bir daire olarak adlandırılan `equator` dünya ekseni kavramsal genişleten iki kutupları arasında midway değil.

### <a name="parallels-and-latitude"></a>Parallels ve enlem

Bir açının Kuzey ve güneyinden ekvatora dünya işaretlerinin Merkezi'nden olarak bilinen eşit enlem satırlık ölçülen *parallels*. Ekvator'un 0 derecede bu aralıktan Kuzey ve Güney kutupları 90 derece için. Kural tarafından ekvatora north of latitudes pozitif değerler ve negatif değerler güneyinden ekvatora olanlardır.

### <a name="longitude-and-meridians"></a>Boylam ve meridians

Güney kutbu'na Kuzey kutbu'na gelen mükemmel daireye yarısının olan eşit boylam satırları olarak da bilinen *meridians*. Meridyeninden İngiltere'nın göre bunlar. Kurala göre 180 derece 0 dereceye pozitif değerler longitudes Meridyeninden doğuya doğru olduğunda ve longitudes Meridyeninden doğuya doğru negatif değerler 0 dereceye & #x 2013; 180 derece.

### <a name="the-equirectangular-projection"></a>Equirectangular projeksiyonu

Herhangi bir düz eşlemesini dünya deformasyonları tanıtır. Enlem ve boylam tüm satırlar düz ve enlem ve boylam açıları eşit farklılıkları harita üzerinde eşit uzaklıkta karşılık geliyorsa, sonuç ise bir *equirectangular projeksiyon*. Bunlar yatay uzatılır çünkü bu haritada kutupları yakın alanlara deforme eder.

### <a name="the-mercator-projection"></a>Mercator izdüşümünü

Popüler *Mercator izdüşümünü* yatay uzatma Ayrıca bu alanlar dikey yayarak için dengelemek çalışır. Bu alanlar kutupları yakın gerçekten olduklarından, ancak herhangi bir yerel oldukça yakın gerçek alanı ile uyumlu çok büyük göründüğü bir harita sonuçlanır.

### <a name="map-services-and-tiles"></a>Harita Hizmetleri ve döşeme

Harita hizmetlerini kullanan adlı Mercator izdüşümünü çeşitlemesi `Web Mercator`. Harita Hizmetleri bit eşlem döşeme konumu ve yakınlaştırma düzeyi temelinde bir istemciye sunar.

## <a name="getting-the-users-location"></a>Kullanıcının konumunu alma

Xamarin.Forms `Map` sınıfları kullanıcının coğrafi konum elde etmek için bir özellik içermiyor, ancak Eşlemleriyle, bu nedenle bir bağımlılık hizmeti çalışma işlemesi gerekir, bu genellikle tercih edilir.

### <a name="the-location-tracker-api"></a>API konumu İzleyicisi

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) çözüm konumu İzleyicisi API kodunu içerir. [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) Yapısı enlem ve boylam yalıtır. [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) Arabirimi başlatmak ve konumu İzleyicisi'ni ve yeni bir konum kullanılabilir olduğunda bir olay duraklatmak için iki yöntem tanımlar.

#### <a name="the-ios-location-manager"></a>İOS konumu Yöneticisi

İOS uygulaması `ILocationTracker` olan bir [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) sağlayan bir sınıf kullanma iOS [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/).

#### <a name="the-android-location-manager"></a>Android konumu Yöneticisi

Android uygulaması `ILocationTracker` olan bir [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) Android yararlanır sınıfı [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) sınıfı.

#### <a name="the-windows-runtime-geo-locator"></a>Windows çalışma zamanı coğrafi Bulucu

Windows çalışma zamanı uyarlamasını `ILocationTracker` olan bir [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) UWP yararlanır sınıfı [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534).

### <a name="display-the-phones-location"></a>Görüntü telefonun konumu

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) örnek metin hem equirectangular harita üzerinde telefonun konumu göstermek için konum İzleyicisi'ni kullanır.

### <a name="the-required-overhead"></a>Gerekli ek yükü

Bazı ek için gerekli olan **WhereAmI** konumu İzleyicisi'ni kullanmak için. Öncelikle, tüm projelerde **WhereAmI** çözüm karşılık gelen projelerinde başvurular olmalıdır **Xamarin.FormsBook.Platform**ve her **WhereAmI** proje çağırmalısınız `Toolkit.Init` yöntemi.

Konum izinleri formunda bazı ek platforma özgü ek gereklidir.

#### <a name="location-permission-for-ios"></a>İOS için konum izni

İOS, **info.plist** dosyası, kullanıcıya, kullanıcının konumunu alma izni soruluyor soru metnini içeren öğeleri içermelidir.

#### <a name="location-permissions-for-android"></a>Android için konum izinleri

Kullanıcının konumuna elde android uygulamaları AndroidManifest.xml dosyasında bir ACCESS_FILE_LOCATION izninizin olması gerekir.

#### <a name="location-permissions-for-the-windows-runtime"></a>Windows çalışma zamanı için konum izinleri

Bir Windows veya Windows Phone Uygulama olmalıdır bir `location` aygıt yeteneği Package.appxmanifest dosyasında olarak işaretlenmiş.

## <a name="working-with-xamarinformsmaps"></a>Xamarin.Forms.Maps ile çalışma

Birkaç gereksinimi kullanarak söz konusu `Map` sınıfı.

### <a name="the-nuget-package"></a>NuGet paketi

**Xamarin.Forms.Maps** uygulama çözüme NuGet kitaplık eklenebilir. Sürüm numarası aynı olmalıdır **Xamarin.Forms** paket şu anda yüklü.

### <a name="initializing-the-maps-package"></a>Maps paket başlatılıyor

Uygulama projeleri çağırmalısınız `Xamarin.FormsMaps.Init` yapılan bir çağrı yaptıktan sonra yöntemi `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Harita Hizmetleri etkinleştirme

Çünkü `Map` kullanıcının konumunu elde edebilirsiniz, uygulama bu bölümde açıklanan şekilde kullanıcı için izni almanız gerekir:

#### <a name="enabling-ios-maps"></a>İOS etkinleştirme eşlemeleri

Bir iOS uygulaması kullanarak `Map` info.plist dosyasında iki satır gerekiyor.

#### <a name="enabling-android-maps"></a>Android etkinleştirme eşlemeleri

Bir yetkilendirme anahtarı, Google harita hizmetlerini kullandığınız için gereklidir. Bu anahtar eklenen **AndroidManifest.xml** dosya. Ayrıca, **AndroidManifest.xml** dosyası gerektirir `manifest` etiketleri kullanıcının konumunu alma söz konusu.

#### <a name="enabling-windows-runtime-maps"></a>Windows çalışma zamanı etkinleştirme eşlemeleri

Windows çalışma zamanı uygulama Bing Haritalar kullanmak için bir yetkilendirme anahtarı gerektirir. Bu anahtar için bağımsız değişken olarak geçirilen `Xamarin.FormsMaps.Init` yöntemi. Uygulama için konum hizmetlerini de etkinleştirilmesi gerekir.

### <a name="the-unadorned-map"></a>Asıl eşleme

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) örnek oluşur bir [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) dosya ve [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) arka plan kod dosyası çeşitli tanıtım programlar gezinme sağlar.

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) dosyasını nasıl görüntüleneceğini gösterir [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) görünümü. Varsayılan olarak, bu Şehir Roma gösterir, ancak harita kullanıcı tarafından yönetilebilir.

Yatay ve dikey kaydırmayı devre dışı bırakmak için ayarlanmış [ `HasScrollEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasScrollEnabled/) özelliğine `false`. Yakınlaştırma devre dışı bırakmak için ayarlanmış [ `HasZoomEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasZoomEnabled/) için `false`. Bu özellikleri tüm platformlarda çalışmayabilir.

### <a name="streets-and-terrain"></a>Streets ve Terrain

Ayarlayarak eşlemeleri farklı türlerde görüntüleyebilirsiniz `Map` özelliği [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) türü [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/), üç üyesi olan bir numaralandırma:

- [`Street`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Street/), varsayılan
- [`Satellite`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Satellite/)
- [`Hybrid`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Hybrid/)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) dosyası eşleme türü seçmek için bir radyo düğmesi kullanmayı gösterir. Bunu kullanır [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı ve bir sınıfın temel [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) dosya.

### <a name="map-coordinates"></a>Harita koordinatlarına

Bir program, geçerli alan elde `Map` aracılığıyla görüntüleme [ `VisibleRegion` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.VisibleRegion/) özelliği. Bu özellik *değil* bağlanabilir bir özelliği tarafından yedeklenir ve ne zaman değişti, büyük olasılıkla bir süreölçer özelliği izlemek istediği bir programın bu amaç için kullanması gereken şekilde göstermek için hiçbir bildirim mekanizması.

`VisibleRegion` tür [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/), dört salt okunur özellikler sınıfıyla:

- [`Center`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Center/) türü [`Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)
- [`LatitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LatitudeDegrees/) tür `double`, görüntülenen harita alanının yüksekliğini belirten
- [`LongitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LongitudeDegrees/) tür `double`, görüntülenen harita alanının genişliğini belirten
- [`Radius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Radius/) tür [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/), harita üzerinde görünür en büyük döngüsel alanının boyutunu belirten

`Position` ve `Distance` hem yapıları şunlardır. `Position` aracılığıyla ayarlanan iki salt okunur özellikleri tanımlar [ `Position` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Position.Position/p/System.Double/System.Double/):

- [`Latitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Latitude/)
- [`Longitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Longitude/)

`Distance` Ölçüm ve İngilizce birimleri arasında dönüştürme birim bağımsız uzaklığı sağlamak için tasarlanmıştır. A `Distance` değeri çeşitli yollarla oluşturulabilir:

- [`Distance` Oluşturucu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Distance.Distance/p/System.Double/) metre cinsinden uzaklık ile
- [`Distance.FromMeters`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMeters/p/System.Double/) statik yöntemi
- [`Distance.FromKilometers`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromKilometers/p/System.Double/) statik yöntemi
- [`Distance.FromMiles`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMiles/p/System.Double/) statik yöntemi

Değer, üç özelliklerinden kullanılabilir:

- [`Meters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Meters/) türü `double`
- [`Kilometers`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Kilometers/) türü `double`
- [`Miles`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Miles/) türü `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) dosyasını içeren birkaç `Label` görüntüleme için öğeleri `MapSpan` bilgi. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) arka plan kodu dosya bilgileri harita kullanıcı yönetir gibi güncel tutmak için bir zamanlayıcı kullanır.

### <a name="position-extensions"></a>Konum uzantıları

Adlı bu kitap için yeni bir kitaplık [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) harita özgü ancak platformdan bağımsız türler içerir. [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Sınıfına sahip bir `ToString` yöntemi `Position`ve iki arasındaki uzaklığı hesaplamak için bir yöntem `Position` değerleri.

### <a name="setting-an-initial-location"></a>Başlangıç konumu ayarlama

Çağırabilirsiniz [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion/p/Xamarin.Forms.Maps.MapSpan/) yöntemi `Map` programlı olarak harita üzerinde bir konum ve yakınlaştırma düzeyini ayarlamak için. Bağımsız değişkeni türünde `MapSpan`. Oluşturabileceğiniz bir `MapSpan` aşağıdakilerden birini kullanarak nesnesi:

- [`MapSpan` Oluşturucu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.MapSpan.MapSpan/p/Xamarin.Forms.Maps.Position/System.Double/System.Double/) ile bir `Position`ve enlem ve boylam aralık
- [`MapSpan.FromCenterAndRadius`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius/p/Xamarin.Forms.Maps.Position/Xamarin.Forms.Maps.Distance/) ile bir `Position` ve RADIUS

Yeni bir oluşturmak mümkündür `MapSpan` yöntemleri kullanarak mevcut bir gelen [ `ClampLatitude` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.ClampLatitude/p/System.Double/System.Double/) veya [ `WithZoom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.WithZoom/p/System.Double/).

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) dosya ve [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) arka plan kod dosyasına nasıl kullanılacağını gösteren `MoveToRegion` , durumu Wyoming görüntülenecek yöntemi.

Alternatif olarak kullanabileceğiniz [ `Map` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Map.Map/p/Xamarin.Forms.Maps.MapSpan/) ile bir `MapSpan` konumunu harita nesnesi. [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) dosya XAML'de San Francisco Xamarin'ın genel merkez görüntülenecek tamamen bunu nasıl gösterir.

### <a name="dynamic-zooming"></a>Dinamik Yakınlaştırma

Kullanabileceğiniz bir `Slider` dinamik olarak bir harita yakınlaştırma için. [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) dosya ve [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) arka plan kod dosyasına dayalı bir harita RADIUS değiştirme Göster `Slider` değeri.

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) dosya ve [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) arka plan kod dosyasına Göster Android daha iyi çalışır alternatif bir yaklaşım, ancak her iki yaklaşım da Windows üzerinde çalışır Platform.

### <a name="the-phones-location"></a>Telefonunuzun konumu

[ `IsShowingUser` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.IsShowingUser/) Özelliği `Map` üç platformlarda biraz farklı şekilde çalışır [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) dosyası gösterir:

- İOS, mavi bir nokta telefonun konumu gösterir ancak, el ile var. gitmeniz gerekir
- Android, bir simge görüntülenir olduğunda gönderilen taşır telefonun konuma eşleme
- UWP iOS benzer, ancak bazı durumlarda otomatik olarak konuma yönlendirir

**MapDemos** proje çalışır dayalı bir simge tabanlı düğmesi tanımlayarak Android yaklaşım taklit etmek üzere [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) dosya ve [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) arka plan kod dosyası.

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) dosya ve [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) arka plan kod dosyasına telefonun konuma gitmek için bu düğmeyi kullanın.

### <a name="pins-and-science-museums"></a>PIN ve Bilim müzelerin

Son olarak, `Map` sınıfı tanımlayan bir [ `Pins` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.Pins/) türündeki özelliği `IList<Pin>`. [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) Sınıfı dört özellikleri tanımlar:

- [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Label/) tür `string`, gerekli özelliği
- [`Address`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Address/) tür `string`, isteğe bağlı bir kullanıcı tarafından okunabilen adresi
- [`Position`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Position/) tür `Position`, PIN harita üzerinde görüntülenir burada belirten
- [`Type`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Type/) tür [ `PinType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.PinType/), kullanılmayan bir numaralandırması

**MapDemos** projesini içeren dosyayı [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), Amerika Birleşik Devletleri'nde Bilim müzelerin listeler ve [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) ve [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) bu verileri seri durumdan çıkarmak için sınıflar.

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) dosya ve [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) arka plan kodu dosya görüntü PIN'ler için bu Bilim müzelerin eşlemesindeki. Kullanıcı bir PIN dokunur seçtiğinizde, adresi ve bir Web sitesi müze için görüntüler.

### <a name="the-distance-between-two-points"></a>İki nokta arasındaki uzaklığı

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Sınıfı içeren bir [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) iki coğrafi konumlar arasındaki uzaklığı basitleştirilmiş bir hesaplama yöntemiyle.

Bu kullanılan [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) dosya ve [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) uzaklığı müze kullanıcının konumundan da görüntülemek için arka plan kod dosyası:

[![Üçlü sayfasının ekran görüntüsü yerel müzelerin](images/ch28fg28-small.png "uzaklığı bir konuma")](images/ch28fg28-large.png "bir konuma uzaklığı")

Program ayrıca dinamik olarak harita konumunu temelinde sayısını sınırlamak nasıl gösterir.

## <a name="geocoding-and-back-again"></a>Coğrafi kodlama ve yeniden

[ **Xamarin.Forms.Maps** ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) derleme de içeren bir [ `Geocoder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Geocoder/) ile sınıf bir [ `GetPositionsForAddressAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync/p/System.String/) dönüştürür yöntemi sıfır veya daha fazla olası coğrafi konumlar ve başka bir yöntem metin adresine [ `GetAddressesForPositionAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync/p/Xamarin.Forms.Maps.Position/) ters yönde dönüştürür.

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) dosya ve [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) arka plan kod dosyası bu tesis göstermektedir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 28 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Bölüm 28 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Harita denetimi](~/xamarin-forms/user-interface/map.md)
