---
title: Uygulamanızdaki Google haritalar API'si kullanılarak
description: Google haritalar API'si v2 Özellikleri Xamarin.Android uygulamanıza gerçekleştirme.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: a0e010a8300eb4b4452737e34d2f55a35ab95428
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935145"
---
# <a name="using-the-google-maps-api-in-your-application"></a>Uygulamanızdaki Google haritalar API'si kullanma

Maps uygulamayı kullanarak harikadır, ancak bazen, doğrudan uygulamanızda eşlemeleri dahil etmek istediğiniz. Google sunduğu yanı sıra yerleşik uygulama, eşleyen bir [Android için yerel eşleme API](https://developers.google.com/maps/documentation/android/).
Maps API eşleme deneyimi üzerinde daha fazla denetim sağlamak istediğiniz durumlar için uygundur. Maps API ile mümkündür noktalar şunlardır:

-  Harita görüş programlı.
-  Ekleme ve işaretçileri özelleştirme.
-  Bir harita yer paylaşımları ile yorumlama.

Artık kullanım dışı Google haritalar Android API'si v1, Google haritalar Android API v2 parçası olan [Google Play Hizmetleri](http://developer.android.com/google/play-services/index.html).
Bu nedenle, Google haritalar Android API'si bir Xamarin.Android uygulaması'nda kullanmak da mümkündür önce bazı zorunlu Önkoşullar karşılamak gereklidir.


## <a name="google-maps-api-prerequisites"></a>Google API Önkoşullar eşlemeleri

Birden çok öğe haritalar API'si kullanmadan önce yapılandırılması gereken dahil olmak üzere:

-  Google Play hizmetlerini SDK'sını yükleyin
-  Bir öykünücü ile Google API oluşturma
-  Bir Maps API anahtarı edinme
-  Gerekli izinleri belirtin



### <a name="install-the-google-play-services-sdk"></a>Google Play hizmetlerini SDK'sını yükleyin

Google Play Hizmetleri'nin Google +, uygulama içi faturalama ve haritalar gibi çeşitli Google özelliklerden yararlanmak için Android uygulamalarına izin veren Google sayfası bir teknolojidir. Bu özellikler, içerdiği arka plan Hizmetleri olarak Android cihazlarda erişilebilir [Google Play Hizmetleri APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en).

Android uygulamaları Google Play Hizmetleri istemci kitaplığı Google Play Hizmetleri ile etkileşim. Bu kitaplık eşlemeleri gibi tek tek Hizmetleri için sınıflar ve arabirimler içerir. Aşağıdaki diyagramda bir Android uygulaması ve Google Play Hizmetleri arasındaki ilişki gösterilmektedir:

![Google Play Mağazası'nin Google Play Hizmetleri APK güncelleştirme gösteren diyagram](maps-api-images/play-services-diagram.png)

Android haritalar API'si Google Play Hizmetleri'nin bir parçası olarak sağlanır.
Bir Xamarin.Android uygulaması haritalar API'si kullanmadan önce Google Play Hizmetleri SDK'sı yüklü ve bağlı olmalıdır. Google Play Hizmetleri SDK'sı Android SDK Yöneticisi aracılığıyla yüklenir. Aşağıdaki ekran görüntüsü, Android SDK Google Play Hizmetleri İstemcisi bulunabilir Yöneticisi'nde where gösterir:

![Google Play Hizmetleri ek özellikler Android SDK Yöneticisi'nde altında görünür](maps-api-images/image01.png)

> [!NOTE]
> Google Play hizmetlerini APK tüm aygıtlarda bulunmayabilir lisanslı bir üründür. Yüklenmemişse, Google haritalar cihazda çalışmaz.


#### <a name="binding-google-play-services"></a>Bağlama Google Play Hizmetleri

Google Play Hizmetleri istemci kitaplığı yüklendikten sonra bir Xamarin.Android Java bağlama kitaplığı tarafından bağlı olması gerekir. Bunu yapmanın iki yolu vardır:

-  **Google Play Hizmetleri eşlemeleri NuGet paketini kullanmak** -en kolay yaklaşım (yalnızca Xamarin.Android 4.8 kullanılabilir veya sonrası) budur.
   Yükleme [Xamarin Google Play Hizmetleri eşlemeleri NuGet](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps); bu da herhangi bir Google Play Hizmetleri bağımlılık paket yükler.
   Bu kılavuzun geri kalanında bu yaklaşım üzerinde odaklanmıştır.

-  **Google Play Hizmetleri istemci kitaplığı el ile bağlama** -bu daha karmaşık bir yaklaşım ve Xamarin.Android 4.4 veya Xamarin.Android 4.6 Google Play Hizmetleri SDK'sı bağlamak için tek yoludur.
   Google Play Hizmetleri istemci kitaplığı el ile bağlama bu belgenin kapsamı dışında olsa da, bunu yapmak nasıl bir örneği bulunabilir [harita ve konum Demo v3 örnek](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3) github'da.


#### <a name="adding-the-google-play-services-map-package"></a>Google Play Hizmetleri harita paketi ekleme

Google Play Hizmetleri harita paketini eklemek için sağ tıklatın **başvuruları** klasörü tıklatın ve Çözüm Gezgini'nde projenizin **NuGet paketlerini Yönet...** :

![Çözüm Gezgini gösteren NuGet paketlerini Yönet bağlam menüsü öğesini References altında](maps-api-images/image02.png)

Bu açılır **NuGet Paket Yöneticisi**. Tıklatın **Gözat** ve girin **Xamarin Google Play Hizmetleri haritalar** arama alanında. Seçin **Xamarin.GooglePlayServices.Maps** tıklatıp **yükleme**. (Bu paketi daha önce yüklemiş olduğu tıklatmak **güncelleştirme**.):

[![Seçili Xamarin.GooglePlayServices.Maps Paketle NuGet Paket Yöneticisi](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Aşağıdaki bağımlılık paketleri yüklenmiş dikkat edin:

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**



### <a name="create-an-emulator-with-google-apis"></a>Bir öykünücü Google API'leri ile oluşturma

Önerilmemesine rağmen Android haritalar API'si desteklemek için öykünücü Kurulum mümkündür. Öykünücü Google API düzeyi 17 (Android 4.2.2) hedeflemek için yapılandırılması gerekir veya daha yüksek. Aşağıdaki ekran görüntüsünde bir öykünücü görüntünüzün için API düzey 19 yapılandırılır: 

![Android Emulator Manager API düzeyi 19 için yapılandırılmış bir AVD ile](maps-api-images/image04.png)



### <a name="obtain-a-google-maps-api-key"></a>Bir Google haritalar API'si anahtarı edinme

Son adım, Google haritalar API'si anahtarı (eski Google haritalar v1'den bir API anahtarı yeniden kullanamazsınız unutmayın) almaktır. Edinme ve API anahtarını Xamarin.Android ile kullanma hakkında daha fazla bilgi için bkz: [bir Google haritalar API'si anahtarı edinme](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
 


### <a name="specify-the-required-permissions"></a>Gerekli izinleri belirtin

Aşağıdaki izinler belirtilmelidir **AndroidManifest.XML** Google haritalar Android API için:

-  **Ağ durumu erişimi** &ndash; haritalar API'si sağlayabilmelidir harita kutucukları indirebilirsiniz varsa denetleyin.

-  **Internet erişimi** &ndash; harita kutucukları indirmek ve API erişimi için Google Play sunucuları ile iletişim kurmak Internet erişimi gereklidir.

-  **OpenGL ES v2** &ndash; uygulama OpenGL ES v2 gereksinimini bildirmeniz gerekir.

-  **Google haritalar API'si anahtarı** &ndash; API anahtarı, uygulama kayıtlı ve Google Play Hizmetleri kullanma yetkisi doğrulamak için kullanılır. Bkz: [Google haritalar API'si anahtarı edinme](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) bu anahtarı hakkında ayrıntılı bilgi için.

-  **Harici depolama alanına yazmak** &ndash; Google haritalar Android API'si dış depolama birimine yüklenen döşeme önbelleğe.

-  **Google Web tabanlı hizmetlere erişim** &ndash; uygulamanın Android haritalar API'si geri Google web hizmetlerine erişmek için izinler gerekir.

-  **Google Play Hizmetleri bildirimleri izinlerini** &ndash; uygulamayı Google Play Hizmetleri'nin Uzak bildirim alma izni verilmesi gerekir.

-  **Konum sağlayıcılarına erişim** &ndash; bunlar isteğe bağlı izinlerdir.
   İzin verir `GoogleMap` cihazın konumu haritada görüntülenecek sınıf.


Aşağıdaki kod parçacığında eklenmeli ayarları örneğidir **AndroidManifest.XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="14" android:targetSdkVersion="17" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- We need to be able to download map tiles and access Google Play Services-->
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Allow the application to access Google web-based services. -->
    <uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />

    <!-- Google Maps for Android v2 will cache map tiles on external storage -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    <!-- Google Maps for Android v2 needs this permission so that it may check the connection state as it must download data -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />


    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
    </application>
</manifest>
```


## <a name="the-googlemap-class"></a>GoogleMap sınıfı

Önkoşulları dikkate sonra, uygulama geliştirmeye başlayın ve Android haritalar API'si kullanma zamanı geldi. [GoogleMap](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap) bir Xamarin.Android uygulaması görüntülemek ve Android için Google Haritalar ile etkileşim kurmak için kullanacağı ana API bir sınıftır. Bu sınıf, aşağıdaki sorumlulukları vardır:

-  Uygulama Google web hizmeti ile yetkilendirmek için Google Play Hizmetleri ile etkileşim.

-  İndirme, önbelleğe alma ve harita kutucukları görüntüleme.

-  Kullanıcı Arabirimi denetimlerini gibi görüntüleme kaydırma ve kullanıcıya yakınlaştırma.

-  Eşlemeleri işaretçileri ve geometrik şekiller çizme.

`GoogleMap` İki yoldan biriyle bir etkinliğe eklendi:

-  **MapFragment** - [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) için konağı olarak davranan özel bir parçası olan `GoogleMap` nesnesi. `MapFragment` 12 veya daha yüksek Android API düzeyini gerektirmektedir.
   Android eski sürümleri kullanabilir [SupportMapFragment](http://developer.android.com/reference/com/google/android/gms/maps/SupportMapFragment.html).

-  **MapView** - [MapView](https://developer.xamarin.com/api/type/Android.GoogleMaps.MapView/) için bir konak olarak davranıp özel bir görünüm alt sınıfıdır bir `GoogleMap` nesnesi. Kullanıcılar, bu sınıfın tüm etkinlik yaşam döngüsü yöntemleri iletmek gerekir `MapView` sınıfı.

Bu kapsayıcıların kullanıma bir `Map` örneğini döndüren özelliği `GoogleMap`. Tercih için verilmelidir [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) sınıf olarak bir geliştirici el ile uygulamalıdır tutar Demirbaş kod azaltır daha basit bir API.


### <a name="adding-a-mapfragment-to-an-activity"></a>Etkinlik için bir MapFragment ekleme

Aşağıdaki ekran görüntüsünde çok basit örneğidir `MapFragment`:

[![Bir eşleme parçası görüntüleyen bir cihazın ekran görüntüsü](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Benzer şekilde diğer parça sınıfları, bu eklemek için iki yolu vardır `MapFragment` bir etkinlik için:

-   **Bildirimli olarak** - `MapFragment` etkinliği için XML düzeni dosyası eklenebilir. Aşağıdaki XML parçacığını nasıl kullanılacağına ilişkin bir örnek göstermektedir `fragment` öğe:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **Programlı şekilde** - `MapFragment` sonraki bölümde açıklandığı gibi program aracılığıyla eklenebilir.

Programlı olarak eklemek için bir `MapFragment`, etkinliklerinizi uygulamalıdır `IOnMapReadyCallback` arabirimi. Çünkü başlatılması bir `GoogleMap` nesne API Google Play ile iletişim kurar gibi tamamlanması biraz zaman alabilir, uygulamanızı bildiren bir geri çağırma sağlamalısınız zaman `GoogleMap` hazırdır.

İlk olarak, ekleyin `IOnMapReadyCallback` için `Activity` sınıf bildiriminin.
Örneğin:

```csharp
public class MapWithMarkersActivity : Activity, IOnMapReadyCallback
```

İleri ' `OnCreate` yöntemi Ekle `MapFragment` aşağıdaki kod örneğinde gösterildiği gibi ( `GoogleMapOptions` sınıfı bu kılavuzda açıklanan):

```csharp
_mapFragment = FragmentManager.FindFragmentByTag("map") as MapFragment;
if (_mapFragment == null)
{
    GoogleMapOptions mapOptions = new GoogleMapOptions()
        .InvokeMapType(GoogleMap.MapTypeSatellite)
        .InvokeZoomControlsEnabled(false)
        .InvokeCompassEnabled(true);

    FragmentTransaction fragTx = FragmentManager.BeginTransaction();
    _mapFragment = MapFragment.NewInstance(mapOptions);
    fragTx.Add(Resource.Id.map, _mapFragment, "map");
    fragTx.Commit();
}
_mapFragment.GetMapAsync(this);
```

A `GoogleMap` kullanarak edinilen gerekir `GetMapAsync`önceki kod örneğinde sonunda gösterildiği gibi &ndash; bu eşlemeleri sistem ve Görünüm otomatik olarak başlatır. (Bu yöntem kullanmayan Not `await` / `async` semantiği &ndash; `Async` davranışı Android tarafından gerçekleştirilir.) Zaman `GoogleMap` nesne hazır, Android uygulamanızın çağırır `OnMapReady` yöntemi (bir parçası olarak uygulamayan `IOnMapReadyCallback` arabirimi). Örneğin:

```csharp
public void OnMapReady (GoogleMap map)
{
    _map = map;
}
```

Yukarıdaki kod örneğinde, `OnMapReady` geri çağırma başlatır `_map` oluşturulan için değişken `GoogleMap` nesnesi.

Bu sonucu kullanmak nasıl bir örnek olarak, `OnResume` olduğu olarak adlandırılan, bu olup olmadığını kontrol edebilirsiniz `_map` null olmayan bir değer. Varsa `_map` ayarlanmış bir `GoogleMap` nesnesi `OnResume` yöntemleri işaretleyicileri ekleyin ve bir belirtilen boylam ve enlem kendi kamera taşımak için üzerindeki çağırabilirsiniz. Tam kod örneği için bkz: [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo).



### <a name="map-types"></a>Eşlem türleri

Google eşlemeleri API'SİNDEN eşlemeleri beş farklı tür vardır:

-  **Normal** -Varsayılan eşleme türü budur. Yollar ve (örneğin, bina ve köprüleri) ilgi çekici man-made bazı noktalar birlikte önemli doğal özellikleri gösterir.

-  **Uydu** -uydu fotoğraf bu haritada gösterir.

-  **Karma** - uydu fotoğraf bu haritada gösterir ve yol eşler.

-  **Terrain** -Bu öncelikle bazı yollar topografik özelliklerle gösterir.

-  **Hiçbiri** -bu haritada tüm kutucukları yüklemez, boş bir kılavuz çizilir.


Aşağıdaki resimde üç farklı türlerinden eşlemelerinin, soldan sağa (normal, karma, terrain) görebilirsiniz:

[![Üç örnek ekran görüntüleri eşleyin: Normal, karma ve Terrain](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType` Özelliği ayarlayın ya da hangi tür eşlemesi görüntülenme şeklini değiştirmek için kullanılır. Aşağıdaki kod parçacığını bir uydu bağlantıları'nı görüntülemek nasıl gösterir.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MapType = GoogleMap.MapTypeSatellite;
}
```


### <a name="googlemap-properties"></a>GoogleMap özellikleri

`GoogleMap` işlevselliği ve harita görünümünü denetleyebilirsiniz çeşitli özellikleri tanımlar. İlk durumunu yapılandırmak için tek yönlü bir `GoogleMap` geçirmektir bir [GoogleMapOptions](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMapOptions.html) nesnesini oluştururken bir `MapFragment`. Aşağıdaki kod parçacığını kullanarak bir örneğidir bir `GoogleMapOptions` nesnesini oluştururken bir `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
_mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, _mapFragment, "map");
fragTx.Commit();
```

Yapılandırmak için başka bir şekilde bir `GoogleMap` nesnesidir değerlerini ayarlayarak [UiSettings](http://developer.android.com/reference/com/google/android/gms/maps/UiSettings.html) eşleme nesnesinin özelliği. Sonraki kod örneği nasıl yapılandırılacağını göstermektedir bir `GoogleMap` yakınlaştırma denetimlerini ve pusula görüntülemek için:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.UiSettings.ZoomControlsEnabled = true;
    _map.UiSettings.CompassEnabled = true;
}
```


## <a name="interacting-with-the-map"></a>Harita ile etkileşim kurma

Android haritalar API'si görüş değiştirmek, işaretleyicileri ekleyin, özel yer paylaşımları yerleştirin veya geometrik şekiller çizmek için bir etkinlik sağlayan API'nin sağlar. Bu bölümde Xamarin.Android, bu görevlerden bazılarını gerçekleştirmek nasıl ele alınacaktır.

### <a name="changing-the-viewpoint"></a>Görüş değiştirme

Düz düzlem olarak, ekranda Mercator izdüşümünü temel Maps modelled. Harita görünümünü budur bir *kamera* bu düzlemi üzerinde doğrudan aşağı görünümlü. Kamera konumunu değiştirme konum, yakınlaştırma, eğim ve şifrelemeyle tarafından denetlenebilir. [CameraUpdate](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdate) sınıfı kamera konumunu değiştirmek için kullanılır. `CameraUpdate` nesnelerine ait değil doğrudan örneklerin, bunun yerine haritalar API'si sağlar [CameraUpdateFactory](http://developer.android.com/reference/com/google/android/gms/maps/CameraUpdateFactory.html) sınıfı.

Bir kez bir `CameraUpdate` nesnesi oluşturulamadı, ya da bir parametre geçirilir [GoogleMap.MoveCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#moveCamera%28com.google.maps.CameraUpdate%29) veya [GoogleMap.AnimateCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#animateCamera%28com.google.maps.CameraUpdate%29) yöntemleri. `MoveCamera` Yöntemi güncelleştirmeleri anında hatayla harita `AnimateCamera` yöntemi bir kesintisiz, animasyonlu geçiş sağlar.

Bu kod parçacığını nasıl kullanılacağını basit örneğidir `CameraUpdateFactory` oluşturmak için bir `CameraUpdate` , Artır harita yakınlaştırma düzeyini tarafından:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Maps API sağlayan bir [CameraPosition](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) hangi toplama tüm kamera konumu için olası değerler. Bu sınıfın bir örneği için sağlanan [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) döndürülecek yöntemi bir `CameraUpdate` nesnesi. Maps API de içeren [CameraPosition.Builder](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) oluşturmak için bir fluent API sağlayan sınıf `CameraPosition` nesneleri.
Aşağıdaki kod parçacığını oluşturmanın bir örneği gösterir bir `CameraUpdate` gelen bir `CameraPosition` ve kamera konumunu değiştirmek için kullandığı bir `GoogleMap`:

```csharp
LatLng location = new LatLng(50.897778, 3.013333);
CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
builder.Target(location);
builder.Zoom(18);
builder.Bearing(155);
builder.Tilt(65);
CameraPosition cameraPosition = builder.Build();
CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(cameraUpdate);
}
```

Önceki kod parçacığında, belirli bir konuma harita üzerinde tarafından temsil edilen bir [LatLng](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/LatLng) sınıfı. Yakınlaştırma düzeyi 18'e ayarlanır. Şifrelemeyle Kuzey saat yönünde pusula ölçüsüdür. Dikey 25 derece açı özelliği görüş açısı denetler ve eğimini belirtir. Aşağıdaki ekran görüntüsü gösterildiği `GoogleMap` önceki kod yürütme sonra:

[![Örnek Google bir Eğimli ile belirtilen bir konuma gösteren eşleme açı görüntüleme](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>Haritada çizme

Android haritalar API'si, aşağıdaki öğeleri bir haritada çizim için API'nin sağlar:

-  **İşaretçileri** -bir eşleme üzerinde tek bir yerde tanımlamak için kullanılan özel simgeler şunlardır.

-  **Yer paylaşımları** -konumlar veya harita alanı koleksiyonu tanımlamak için kullanılan bir görüntü budur.

-  **Çizgiler, çokgenler ve daireler** -etkinliklerin Haritası şekilleri ekleme izin ver API'leri bunlar.


#### <a name="markers"></a>İşaretçileri

Maps API sağlayan bir [işaret](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) tüm verileri bir harita üzerinde tek bir yerde hakkında yalıtan sınıfı. Varsayılan olarak Google haritalar tarafından sağlanan standart bir simge kullanırlar. Bir işaretçi görünümünü özelleştirmek ve kullanıcı tıklamalarına yanıt verme için mümkündür.


##### <a name="adding-a-marker"></a>Bir işaretçi ekleme

Bir işaretçi bir eşlemesine eklemek için ise gerekli yeni bir [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) nesnesi ve ardından arama [AddMarker](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) yöntemi bir `GoogleMap` örneği. Bu yöntem döndürülecek bir [işaret](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) nesnesi.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    _map.AddMarker(markerOpt1);
}
```

İşaretin başlığı görüntülenir bir *bilgi penceresi* zaman kullanıcı dokunur üzerinde işaretçisi. Aşağıdaki ekran görüntüsünde, bu işaret nasıl göründüğünü gösterir:

[![Bir işaretçi ve bilgi penceresi Vimy çıkıntı için örnek Google Haritası](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>Bir işaretçi özelleştirme

Çağırarak işaretleyicisi tarafından kullanılan simge özelleştirmek mümkün mü `MarkerOptions.InvokeIcon` işaret eşlemeye eklerken yöntemi.
Bu yöntem alır bir [BitmapDescriptor](http://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptor.html) simgesi işlemek için gerekli verileri içeren nesne. [BitmapDescriptorFactory](https://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory.html) sınıfı oluşturulmasını kolaylaştırmak için bazı yardımcı yöntemler sağlar bir `BitmapDescriptor`. Aşağıdaki listede bu yöntemlerden bazıları sunar:

-   `DefaultMarker(float colour)` &ndash; Varsayılan Google haritalar işaretçisi kullanır, ancak renkli değiştirin.

-   `FromAsset(string assetName)` &ndash; Özel bir simge varlıklar klasöründeki belirtilen dosyasını kullanın.

-   `FromBitmap(Bitmap image)` &ndash; Belirtilen bit eşlem simgesi olarak kullanın.

-   `FromFile(string fileName)` &ndash; Özel simge dosyanın belirtilen yolda oluşturun.

-   `FromResource(int resourceId)` &ndash; Özel bir simge belirtilen kaynak oluşturun.

Aşağıdaki kod parçacığını bir mavi renkli varsayılan işaret oluşturmanın bir örneği gösterilmektedir:

```csharp
mapFrag.GetMapAsync(this);
...
if (_map != null)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    markerOpt1.InvokeIcon(BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan));
    _map.AddMarker(markerOpt1);
}
```


#### <a name="info-windows"></a>Bilgileri Windows

*Bilgileri windows* belirli bir işaret dokunduğunuzda kullanıcıya bilgi görüntülemek için bu açılan özel windows şunlardır. Varsayılan olarak bilgi penceresi işaretin başlık içeriğini görüntüler. Başlık atanmamış, hiçbir bilgi penceresi görünür. Aynı anda yalnızca bir bilgi penceresi gösterilebilir.

Bilgi penceresi uygulayarak özelleştirmek mümkün mü [GoogleMap.IInfoWindowAdapter](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) arabirimi. Bu arabirimde iki önemli yöntemler şunlardır:

-  `public View GetInfoWindow(Marker marker)` &ndash; Bu yöntem, özel bilgi penceresi için bir işaretçi almak için çağrılır. Döndürürse `null` , varsayılan penceresi işleme kullanılır. Bu yöntem bir görünüm döndürürse, bu görünüm bilgisi pencere çerçevesi içinde yer alır.

-  `public View GetInfoContents(Marker marker)` &ndash; GetInfoWindow döndürürse, bu yöntem yalnızca çağrılacağı `null` . Bu yöntem döndürebilir bir `null` kullanılacak bilgileri pencere içeriğini varsayılan çizmeye ise, değer. Aksi takdirde, bu yöntem bilgileri pencerenin içeriği olan bir görünümü döndürmelidir.

Bir bilgi penceresi dinamik bir görünüm değil - Android bunun yerine ve görünümü için statik bir bit eşlem dönüştürmek görüntüde görüntüler. Bu bir bilgi penceresinde herhangi bir touch olayları veya hareketleri yanıt veremez veya kendisini otomatik olarak güncelleştirilecektir anlamına gelir. Bir bilgi penceresi güncelleştirmek için çağırmak ise gerekli [GoogleMap.ShowInfoWindow](http://developer.android.com/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) yöntemi.

Aşağıdaki resimde bazı özelleştirilmiş bilgileri windows bazı örnekleri gösterilmektedir. Sol taraftaki görüntü görüntünün sağdaki onun penceresi ve özelleştirilmiş içeriği sahipken, özelleştirilmiş içeriğiyle sahiptir:

![Örnek işaret windows Melbourne, simge ve popülasyon dahil için. Pencerenin sağ yuvarlanmış köşeleri.](maps-api-images/marker-infowindows.png)


#### <a name="ground-overlays"></a>Plan yer paylaşımları

Belirli bir konuma bir harita tanımlayın, işaretçileri aksine bir [GroundOverlay](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlay.html) konumlar veya harita alanı koleksiyonu tanımlamak için kullanılan bir görüntü.


##### <a name="adding-a-groundoverlay"></a>Bir GroundOverlay ekleme

Bir plan katmana haritaya ekleme, bir işaretçi bir harita ekleme ile çok benzer. İlk olarak, bir [GroundOverlayOptions](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlayOptions.html) nesnesi oluşturulur. Bu nesne daha sonra bir parametre olarak geçirilen `GoogleMap.AddGroundOverlay` döndürülecek yöntemi bir `GroundOverlay` nesnesi. Bu kod parçacığını bir yerdeki katmana haritaya ekleme, bir örnek verilmiştir:

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = _map.AddGroundOverlay(groundOverlayOptions);
```

Aşağıdaki ekran görüntüsü bu katmana bir haritada gösterir:

[![Bir Kutupsal ayı üzerini kaplamış şekilde görüntüsü örnek Haritası](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>Çizgiler, daireler ve çokgenler

Bir harita eklenebilir geometrik şekiller üç basit türler şunlardır:

-  **Çoklu satır** -bağlı çizgi dilimleri bir dizi budur. Bir harita yola işaretlemek veya gerekli herhangi bir şekli form yükleyebilir.

-  **Çokgen** -bir harita alanları işaretleme için kapalı bir şekil budur.

-  **Daire** -bu bir daire harita üzerinde çizer.



##### <a name="polylines"></a>Kullansa

A [çoklu çizgi](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Polyline) ardışık listesi `LatLng` her çizgi parçasının köşeleri belirten nesneleri. Bir çoklu çizgi ilk oluşturma tarafından oluşturulan bir `PolylineOptions` nesne ve noktaları ekleme. `PolylineOption` Nesne için geçirilen sonra bir `GoogleMap` çağırarak nesne `AddPolyline` yöntemi.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.
myMap.AddPolyline(rectOptions);
```


##### <a name="polygons"></a>Çokgenler

`Polygon`s çok benzer `Polyline`s, açık olmayan ancak sona erdi. `Polygon`s kapalı döngü ve doldurulmuş kendi iç sahiptir.
`Polygon`s tam ile aynı şekilde oluşturulan bir `Polyline`, dışında [GoogleMap.AddPolygon](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) yöntemi çağrılır.

Farklı bir `Polyline`, `Polygon` kendini kapatıyor. Zaman `AddPolygon` çağrıldığında yöntemi otomatik olarak kapanacak ilk ve son noktaları bağlayan bir çizgi çizme tarafından Çokgen kapalı. Aşağıdaki kod parçacığını önceki kod parçacığında olarak aynı alanının üzerinde dolu bir dikdörtgen oluşturacak `Polyline` örnek.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon
myMap.AddPolygon(rectOptions);
```


##### <a name="circles"></a>Daireler

Daireler ilk örneği tarafından oluşturulan bir [CircleOption](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/CircleOptions) merkezi ve dairenin RADIUS göstergeleri belirleyeceksiniz nesnesidir. Daire harita üzerinde çağırarak çizilir [GoogleMap.AddCircle](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap#addCircle(com.google.android.gms.maps.model.CircleOptions)).
Aşağıdaki kod parçacığını bir daire çizmek nasıl gösterir:

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);
_map.AddCircle (circleOptions);
```


## <a name="responding-to-events"></a>Olaylara yanıt verme

Bir kullanıcı bir haritası olabilir etkileşimleri üç tür vardır:

-  **İşaretçi tıklatın** -kullanıcı, üzerinde bir işaret.

-  **İşaretçi Sürükle** -kullanıcı uzun-mparger üzerinde tıklamıştır

-  **Bilgi penceresi tıklayın** -kullanıcı bir bilgi penceresi tıklamıştır.

Bu olayların her biri aşağıdaki daha ayrıntılı olarak açıklanmıştır.


### <a name="marker-click-events"></a>İşaretçi tıklama olayları

Kullanıcı üzerinde bir işaret tıkladığında `MarkerClick` olay gerçekleşti ve bir `GoogleMap.MarkerClickEventArgs` geçirildi. Bu sınıf, iki özellik içerir:

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; Bu özellik ayarlanmalıdır `true` olayın olay işleyicisi tüketmiş olduğunu belirtmek için. Bu ayarlanırsa `false` sonra varsayılan davranışa olay işleyicisi özel davranışa ek olarak meydana gelir.

-  `P0` &ndash; Bu kötü name parametresi yükseltilmiş işaret başvurusudur `MarkerClick` olay.


Bu kod parçacığını bir örneği gösterilmektedir bir `MarkerClick` harita üzerinde yeni bir konuma kamera konumunu değiştirir:

```csharp
private void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;
    Marker marker = markerClickEventArgs.P0;
    if (marker.Id.Equals(MyMarkerId)) // The ID of a specific marker the user clicked on.
    {
        _map.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(new LatLng(20.72110, -156.44776), 13));
    }
    else
    {
        Toast.MakeText(this, String.Format("You clicked on Marker ID {0}", marker.Id), ToastLength.Short).Show();
    }
}
```


### <a name="marker-drag-events"></a>İşaretçi sürükleme olayları

Kullanıcı işaretçisini sürükleyin istediğinde bu olay tetiklenir. Varsayılan olarak, işaretçileri sürüklenebilir değildir. Bir işaretçi ayarlayarak olarak sürüklenebilir ayarlanabilir `Marker.Draggable` özelliğine `true` veya çağırma `MarkerOptions.Draggable` yöntemiyle `true` bir parametre olarak.

İlk işaret sürüklemek için kullanıcı üzerine uzun süre tıklatın ve kendi parmak haritada tutun. Ekran kendi parmak sürüklediğinizde, işaret taşınır. Kullanıcının parmak ekranı kaldırır, işaret yerinde kalır.

Aşağıdaki listede sürüklenebilir bir işaretçisi gerçekleştirilecektir çeşitli olayları anlatmaktadır:

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; Kullanıcı ilk işaret sürüklendiğinde bu olay tetiklenir.

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; İşaretin sürüklenen gibi bu olay tetiklenir.

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; Bu olay tetiklenir olduğunda ne zaman kullanıcı tamamlandı işaret sürükleme.

Her biri `EventArgs` adlı tek bir özellik içeren `P0` başvuru diğer bir deyişle `Marker` sürüklenen nesne.


### <a name="info-window-click-events"></a>Bilgi penceresi tıklama olayları

Aynı anda yalnızca bir bilgi penceresi görüntülenir. Kullanıcı bir harita bilgi penceresinde tıkladığında harita nesnesi oluşturacak bir `InfoWindowClick` olay. Aşağıdaki kod parçacığını bir işleyicisini olaya wire gösterilmektedir:

```csharp
private bool SetupMapIfNeeded()
{
    if (_map == null)
    {
        _map = _mapFragment.Map;
        if (_map != null)
        {
            _map.InfoWindowClick += MapOnInfoWindowClick;
            return true;
        }
        return false;
    }
    return true;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.P0;
    // Do something with marker.
}
```

Bir bilgi penceresi statik olduğunu geri çağırma `View` harita üzerinde bir görüntü olarak çizilir. Düğmeler, onay kutularını veya bilgi penceresinin yerleştirilen metin görünümler gibi herhangi bir pencere öğeleri etkisiz durumda olacaktır ve herhangi bir tam sayı kullanıcı etkinliklerini için yanıt veremiyor.



## <a name="related-links"></a>İlgili bağlantılar

- [Google Play Hizmetleri](http://developer.android.com/google/play-services/index.html)
- [Google Android API v2 eşlemeleri](https://developers.google.com/maps/documentation/android/)
- [APK Google Play Hizmetleri](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Bir Google haritalar API'si anahtarı edinme](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
