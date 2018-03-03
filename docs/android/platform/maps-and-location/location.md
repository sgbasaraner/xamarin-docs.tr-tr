---
title: Konum Hizmetleri
description: "Bu kılavuz konumu-tanıma Android uygulamalarında tanıtır ve Android konum hizmeti API'si gibi kullanılabilir Sigortalı konumu sağlayıcısı Google konum Hizmetleri API'si ile kullanarak kullanıcının konumunu alma gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: beb690fe495d142bb4b0424ad752101fc46da590
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="location-services"></a>Konum Hizmetleri

_Bu kılavuz konumu-tanıma Android uygulamalarında tanıtır ve Android konum hizmeti API'si gibi kullanılabilir Sigortalı konumu sağlayıcısı Google konum Hizmetleri API'si ile kullanarak kullanıcının konumunu alma gösterilmektedir._

## <a name="location-services-overview"></a>Konum hizmetleri genel bakış

Android hücre kule konumu, Wi-Fi ve GPS gibi çeşitli konumu teknolojileri erişim sağlar. Her konuma technology ayrıntılarını aracılığıyla soyutlanır *konumu sağlayıcıları*, kullanılan sağlayıcı bağımsız olarak aynı şekilde konumları elde etmek için uygulamaların izin verme. Bu kılavuz Sigortalı konumu sağlayıcısı, Google Play akıllıca hangi sağlayıcıları kullanılabilir ve cihazı nasıl kullanıldığını bağlı aygıtları konum elde etmek için en iyi yolu belirleyen Hizmetleri, bir parçası tanıtır. Android konum hizmeti API'si ve sistem konumu ile iletişim kurmak gösterilmiştir kullanarak hizmet bir `LocationManager`. Android konum Hizmetleri API kullanarak Kılavuzu ikinci bölümü ele `LocationManager`.
 
Genel altın kural, eski Android konum hizmeti API'si yalnızca gerekli olduğunda geri dönmeden Sigortalı konumu sağlayıcısı kullanmak uygulamalar tercih etmelisiniz.

## <a name="location-fundamentals"></a>Konumu temelleri

Android, konum verilerle çalışmak için seçtiğiniz hangi API önemli değil, birkaç kavram aynı kalır. Bu bölüm, yerleşim sağlayıcıları ve konumu ile ilgili izinleri tanıtır.

### <a name="location-providers"></a>Konum sağlayıcıları

Çeşitli teknolojiler, kullanıcının konumunu saptamak için dahili olarak kullanılır. Kullanılan donanım türüne bağlıdır *konumu sağlayıcısı* veri toplama iş için seçilmiş. Android üç konumu sağlayıcıları kullanır:

-   **GPS sağlayıcısı** &ndash; GPS en doğru konumun sağlar, en güç kullanır ve en iyi şekilde dışarıda çalışır. Bu sağlayıcı GPS ve Yardımlı GPS bileşimini kullanır ([aGPS](http://en.wikipedia.org/wiki/Assisted_GPS)), cep telefonu towers tarafından toplanan GPS verileri döndürür.

-   **Ağ sağlayıcı** &ndash; hücre towers tarafından toplanan aGPS verileri de dahil olmak üzere, WiFi ve hücresel veri bileşimini sağlar. GPS sağlayıcı daha az güç kullanır, ancak değişen doğruluk konum verileri döndürür.

-   **Pasif sağlayıcısı** &ndash; konum verileri bir uygulama oluşturmak için diğer uygulamalar veya hizmetler tarafından istenen sağlayıcıları kullanma "Paketle" bir seçenek. Çalışması için sabit konumu güncelleştirmeler gerektirmeyen uygulamalar için güç tasarrufu seçeneğini ideal ancak daha az güvenilir budur.

Konum sağlayıcıları her zaman kullanılabilir değil. Örneğin, biz GPS uygulamamız için kullanmak isteyebilirsiniz ancak GPS ayarlarında kapalı olabilir veya aygıt GPS hiç olmayabilir. Belirli bir sağlayıcıyı kullanılabilir durumda değilse, bu sağlayıcıyı döndürebilir seçme `null`.

### <a name="location-permissions"></a>Konum izinleri

Konum algılayan bir uygulama, GPS, Wi-Fi ve hücresel veri almak için bir cihazın donanım algılayıcılar erişim gerekir. Erişim, uygulamanın Android derleme bildirimi için uygun izinler aracılığıyla denetlenir.
İki izinleri kullanılabilir &ndash; uygulamanızın gereksinimleri ve seçtiğiniz API'sinin bağlı olarak, bir izin vermek istediğiniz:

-   `ACCESS_FINE_LOCATION` &ndash; Bir uygulama erişime GPS izin verir.
    İçin gerekli *GPS sağlayıcısı* ve *pasif sağlayıcısı* seçenekleri (*pasif sağlayıcı başka bir uygulama veya hizmet tarafından toplanan GPS verilere erişmek için izniniz gerekiyor*). İsteğe bağlı izinlerini *ağ sağlayıcısı*.

-   `ACCESS_COARSE_LOCATION` &ndash; Cep ve Wi-Fi konuma bir uygulama erişim sağlar. İçin gerekli *ağ sağlayıcısı* varsa `ACCESS_FINE_LOCATION` ayarlı değil.

API sürümü 21 (Android 5.0 Lolipop) hedef uygulamalar için veya üzeri, etkinleştirebilirsiniz `ACCESS_FINE_LOCATION` ve GPS donanıma sahip değilseniz cihazlarda hala Çalıştır. Açıkça eklemeniz gerekir, uygulamanızın GPS donanım gerektiriyorsa, bir `android.hardware.location.gps` `uses-feature` Android bildirim öğesine. Daha fazla bilgi için bkz: Android [kullanır özelliği](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) öğe başvurusu.

İzinleri ayarlamak için genişletme **özellikleri** klasöründe **çözüm paneli** çift tıklayın ve **AndroidManifest.xml**. İzinler altında listelenen **gerekli izinler**:

[![Android bildirim gerekli izinler ayarlarının ekran görüntüsü](location-images/location-01-xs.png)](location-images/location-01-xs.png)

Ya da bu izinleri ayarlama, Android uygulamanızı yerleşim sağlayıcıları erişmek için kullanıcıdan izin gerektiğini bildirir. API düzeyi 22 (Android 5.1) çalıştırın veya uygulamanın yüklü olduğu her zaman bu izinleri vermek için kullanıcıdan düşük ister aygıtlar. API çalıştıran cihazlarda 23 (Android 6.0) düzeyinde veya üzeri, uygulama istek konumu sağlayıcısının yapmadan önce bir çalıştırma izni denetimi yapmanız gerekir. 

> [!NOTE]
>Not: Ayarlama `ACCESS_FINE_LOCATION` her iki konumu kaba ve hassas verilere erişimin anlamına gelir. Her iki, yalnızca izinleri ayarlamak hiçbir zaman olmalıdır *en az* uygulamanızı gerektirir çalışmaya izin.

Bu kod parçacığında nasıl bir uygulama için izni olup olmadığını denetleyin, örneğidir `ACCESS_FINE_LOCATION` izin:

```csharp
 if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.AccessFineLocation) == Permission.Granted)
{
    StartRequestingLocationUpdates();
    isRequestingLocationUpdates = true;
}
else
{
    // The app does not have permission ACCESS_FINE_LOCATION 
}
```

Uygulamalar, nerede kullanıcı izni vermek değil (veya izin iptal edilmiş) senaryosu dayanıklı ve düzgün bir şekilde bu durumu düzeltmek için bir yöntem vardır. Lütfen bakın [izinleri Kılavuzu](~/android/app-fundamentals/permissions.md) çalıştırma izni uygulama hakkında daha fazla ayrıntı Xamarin.Android içinde olup olmadığını denetler.


## <a name="using-the-fused-location-provider"></a>Konum Sigortalı sağlayıcısını kullanma

Sigortalı konumu sağlayıcısı pil verimli bir şekilde en iyi konum bilgileri sağlamak için çalışma zamanı sırasında konumu sağlayıcı seçin etkili çünkü aygıttan konumu güncelleştirmeleri almak Android uygulamaları için önerilen yöntemdir. Örneğin, outdoors taramasını bir kullanıcı ile GPS okuma en iyi konumunu alır. Kullanıcı ardından içeride anlatılmaktadır, GPS çalıştığı kötü (varsa), Sigortalı konumu sağlayıcısı otomatik olarak hangi works daha iyi içeride WiFi için geçiş yapabilirsiniz.
 
Konum Sigortalı sağlayıcısı API çeşitli bölge sınırlaması ve etkinlik izleme de dahil olmak üzere konum algılayan uygulamaları güçlendirmeniz diğer araçları sağlar. Bu bölümde, biz odağı ayarlama temelleri üzerinde kalacaklarını `LocationClient`sağlayıcılar oluşturma ve kullanıcının konumunu alma.

Konum Sigortalı sağlayıcısı parçası olan [Google Play Hizmetleri](http://developer.android.com/google/play-services/index.html). Google Play Hizmetleri paketi yüklenmiş ve düzgün çalışması için uygulamada Sigortalı konumu sağlayıcısı API için yapılandırılmış ve aygıtın yüklü Google Play Hizmetleri APK olmalıdır.

Önce bir Xamarin.Android uygulaması Sigortalı konumu sağlayıcıyı kullanabilir bunu eklemeniz gerekir **Xamarin.GooglePlayServices.Maps** projeye.

### <a name="checking-if-google-play-services-is-installed"></a>Google Play Hizmetleri'nin yüklü olup olmadığını denetleme

Xamarin.Android kilitlenme Google Play Hizmetleri'nin yüklü değilse Sigortalı konumu sağlayıcısı kullanmaya çalışırsa (veya zaman aşımına uğramış) sonra bir çalışma zamanı özel oluşacak.  Google Play Hizmetleri'nin yüklü değilse, ardından uygulamayı geri yukarıda açıklanan Android konumu hizmete girecektir. Google Play Hizmetleri'nin eski ise, uygulama Google Play Hizmetleri yüklü olan sürümü güncelleştirmek için bunları isteyen kullanıcı için bir mesaj görüntüleyebilir.

Bu kod parçacığında nasıl Android aktivite programlı olarak Google Play Hizmetleri'nin yüklü olup olmadığını kontrol edebilirsiniz, bir örnek verilmiştir:

```csharp
bool IsGooglePlayServicesInstalled()
{
    var queryResult = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
    if (queryResult == ConnectionResult.Success)
    {
        Log.Info("MainActivity", "Google Play Services is installed on this device.");
        return true;
    }

    if (GoogleApiAvailability.Instance.IsUserResolvableError(queryResult))
    {
        // Check if there is a way the user can resolve the issue
        var errorString = GoogleApiAvailability.Instance.GetErrorString(queryResult);
        Log.Error("MainActivity", "There is a problem with Google Play Services on this device: {0} - {1}",
                  queryResult, errorString);
                  
        // Alternately, display the error to the user.
    }

    return false;
}
```

### <a name="fusedlocationproviderclient"></a>FusedLocationProviderClient

Konum Sigortalı sağlayıcı ile etkileşim kurmak için bir Xamarin.Android uygulaması örneği olmalıdır `FusedLocationProviderClient`. Bu sınıf konumu güncelleştirmelere abone olmak ve cihaz son bilinen konumunu almak için gereken yöntemleri gösterir.

`OnCreate` Yöntemdir etkinliğin bir başvuru almak için uygun bir yer `FusedLocationProviderClient`, aşağıdaki kod parçacığında gösterildiği gibi:

```csharp
public class MainActivity: AppCompatActivity
{
    FusedLocationProviderClient fusedLocationProviderClient;

    protected override void OnCreate(Bundle bundle) 
    {
        fusedLocationProviderClient = LocationServices.GetFusedLocationProviderClient(this);
    }
}
```

### <a name="getting-the-last-known-location"></a>Son bilinen konumu alınıyor

`FusedLocationProviderClient.GetLastLocationAsync()` Yöntemi hızlı bir şekilde ek yükü en az kodlama ile cihazın son bilinen konumunu almak bir Xamarin.Android uygulaması için basit ve engelleyici olmayan bir yol sağlar.

Bu kod parçacığında nasıl kullanılacağını gösterir `GetLastLocationAsync` yöntemi cihaz konumunu almak için:

```csharp
async Task GetLastLocationFromDevice()
{
    // This method assumes that the necessary run-time permission checks have succeeded.
    getLastLocationButton.SetText(Resource.String.getting_last_location);
    Android.Locations.Location location = await fusedLocationProviderClient.GetLastLocationAsync();

    if (location == null)
    {
        // Seldom happens, but should code that handles this scenario
    }
    else
    {
        // Do something with the location 
        Log.Debug("Sample", "The latitude is " + location.Latitude);
    }
}
```

### <a name="subscribing-to-location-updates"></a>Konuma abone güncelleştirir

Ayrıca konumu güncelleştirmeleri bir Xamarin.Android uygulaması kullanarak Sigortalı konumu Sağlayıcısı'ndan abone olabilirsiniz `FusedLocationProviderClient.RequestLocationUpdatesAsync` Bu kod parçacığında gösterildiği gibi yöntemi:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
Bu yöntemi iki parametre alır:

-   **`Android.Gms.Location.LocationRequest`** &ndash; A `LocationRequest` nesnesidir nasıl nasıl Sigortalı konumu sağlayıcısı çalışması bir Xamarin.Android uygulaması parametreleri geçirir. `LocationRequest` Bilgileri gibi sık istekler yapılacak ya da ne kadar önemli bir doğru konuma güncelleştirme olmalı nasıl tutar. Örneğin, bir önemli konumu isteği GPS ve sonuç olarak daha fazla güç konumu belirlenirken kullanılacak cihazın neden olur. Bu kod parçacığını nasıl oluşturulacağını gösterir bir `LocationRequest` yaklaşık her beş denetimi için bir konum yüksek doğruluk ile dakika için bir konum güncelleştirme (ancak istekler arasında iki dakikadan daha önce). Konum Sigortalı sağlayıcısı kullanır bir `LocationRequest` olarak çalışırken aygıt konumunu belirlemek için kullanacağı konumu sağlayıcısını için yönergeler:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; Konum güncelleştirmeleri almak için bir alt kümesi bir Xamarin.Android uygulaması gerekir `LocationProvider` soyut sınıf. Bu sınıf, belki de çağrılan iki yöntem uygulama konumu bilgilerle güncelleştirmek için Sigortalı konumu sağlayıcısı tarafından açık. Bu aşağıdaki daha ayrıntılı olarak açıklanmıştır.

Bir Xamarin.Android uygulaması konumu güncelleştirmesi bildirmek için Sigortalı konumu sağlayıcısı çağıracağı `LocationCallBack.OnLocationResult(LocationResult result)`. `Android.Gms.Location.LocationResult` Parametresi güncelleştirme konum bilgilerini içerir.

Sigortalı konumu sağlayıcısı konum verileri kullanılabilirlik içinde bir değişiklik algılarsa, çağrı `LocationProvider.OnLocationAvaibility(LocationAvailability
locationAvailability)` yöntemi. Varsa `LocationAvailability.IsLocationAvailable` özelliği döndürür `true`, aygıt konumu sonuçları tarafından bildirilen varsayılabilir sonra `OnLocationResult` olarak doğru ve olarak güncel gerektirdiği şekilde `LocationRequest`. Varsa `IsLocationAvailable` konumu sonuç tarafından dönüş sonra yanlış `OnLocationResult`.

Bu kod parçacığını bir örnek uygulamasıdır `LocationCallback` nesnesi:

```csharp
public class FusedLocationProviderCallback : LocationCallback
{
    readonly MainActivity activity;

    public FusedLocationProviderCallback(MainActivity activity)
    {
        this.activity = activity;
    }

    public override void OnLocationAvailability(LocationAvailability locationAvailability)
    {
        Log.Debug("FusedLocationProviderSample", "IsLocationAvailable: {0}",locationAvailability.IsLocationAvailable);
    }

    public override void OnLocationResult(LocationResult result)
    {
        if (result.Locations.Any())
        {
            var location = result.Locations.First();
            Log.Debug("Sample", "The latitude is :" + location.Latitude);
        }
        else
        {
            // No locations to work with.
        }
    }
}
```

## <a name="using-the-android-location-service-api"></a>Android konum hizmeti API'si kullanma

Android konum hizmeti Android konumu bilgilerini kullanarak daha eski bir API'dir. Konum verileri donanım algılayıcılar tarafından toplanır ve uygulama ile erişilen bir sistem hizmet tarafından toplanan bir `LocationManager` sınıfı ve bir `ILocationListener`.

Konum hizmeti Google Play Hizmetleri'nin yüklü olmayan aygıtlar üzerinde çalışması gereken uygulamalar için uygundur.

Konum hizmeti özel türüdür [hizmet](http://developer.android.com/guide/components/services.html) sistemi tarafından yönetilir. Sistem hizmeti, cihaz donanım ile etkileşim kurar ve her zaman çalışır durumda. Uygulamamız konumu güncelleştirmelerinde içine dokunun, biz konumu güncelleştirmeleri kullanarak sistem konum hizmeti abone bir `LocationManager` ve `RequestLocationUpdates` çağırın.

Android konum hizmeti kullanarak kullanıcının konumunu almak için çeşitli adımları içerir:

1.  Bir başvuru `LocationManager` hizmet.
2.  Uygulama `ILocationListener` konumu değiştiğinde arabirimi ve tanıtıcı olaylar.
3.  Kullanım `LocationManager` belirtilen sağlayıcı için istek konumu güncelleştirmeleri. `ILocationListener` Önceki adımdaki gelen geri aramalar almak için kullanılan `LocationManager`.
4.  Uygulama artık güncelleştirmeleri almak, uygun olduğunda konumu güncelleştirmeleri durdurun.

### <a name="location-manager"></a>Konum Yöneticisi

Sistem konumu hizmeti örneği ile erişmek için `LocationManager` sınıfı. `LocationManager` Bize sistem konumu hizmeti ile etkileşim kurmanızı ve üzerinde yöntemleri çağırmak sağlar özel bir sınıftır. Bir uygulama için bir başvuru alabilirsiniz `LocationManager` çağırarak `GetSystemService` ve aşağıda gösterildiği gibi bir hizmet türünün geçirme:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` bir başvuru almak için uygun bir yerdir `LocationManager`.
Tutmak için iyi bir fikirdir `LocationManager` sınıfı değişken, böylece biz etkinlik yaşam döngüsü çeşitli yerlerinde çağırabilirsiniz.

### <a name="request-location-updates-from-the-locationmanager"></a>İstek konumu güncelleştirmelerini LocationManager

Uygulama için bir başvuru olduğunda `LocationManager`, bildirmek gereken `LocationManager` hangi tür konumu bilgileri, gereklidir ve bu bilgileri ne sıklıkta olduğundan güncelleştirilecek. Çağırarak bunu `RequestionLocationUpdates` üzerinde `LocationManager` nesne ve bazı güncelleştirmeleri ve konum güncelleştirmeleri alacak bir geri arama ölçütlerini geçirme. Bu geri çağırma uygulamalıdır türüdür `ILocationListener` arabirimi (Bu kılavuzun ilerleyen bölümlerinde daha ayrıntılı açıklanmıştır).

`RequestionLocationUpdates` Yöntemi uygulamanız konumu güncelleştirmeleri almaya başlamak istediğiniz hizmet sistem konumu söyler. Bu yöntem, sağlayıcı, aynı zamanda zaman ve uzaklık eşikleri güncelleştirme sıklığını denetlemek için belirtmenizi sağlar. Örneğin, aşağıda yöntemi istekleri konum altındaki her 2000 milisaniye GPS konumu Sağlayıcısı'ndan güncelleştirir ve yalnızca konumu birden fazla 1 metre değiştiğinde:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Bir uygulama konumu güncelleştirmeleri yalnızca sıklıkta uygulamanın iyi gerçekleştirmek gerekli istemeniz gerekir. Bu, pil ömrünün korur ve kullanıcı için daha iyi bir deneyim oluşturur.

### <a name="responding-to-updates-from-the-locationmanager"></a>Güncelleştirmeleri LocationManager yanıt

Bir uygulama güncelleştirmelerini istedi sonra `LocationManager`, uygulayarak hizmetinden bilgi alabileceği [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/) arabirimi. Bu arabirim hizmet konumu ve konum sağlayıcısı için dinleme dört yöntemlerini sağlar `OnLocationChanged`. Sistem çağıracak `OnLocationChanged` kullanıcının konumunu değiştiğinde konumu güncelleştirmelerini isterken ayarlanan ölçütlere göre konumu değişiklik olarak nitelemek için yeterli. 

Aşağıdaki kod yöntemleri gösterir `ILocationListener` arabirimi:

```csharp
public class MainActivity : AppCompatActivity, ILocationListener
{
    TextView latitude;
    TextView longitude;
    
    public void OnLocationChanged (Location location)
    {
        // called when the location has been updated.
    }
    
    public OnProviderDisabled(string locationProvider)
    {
        // called when the user disables the provider
    }
    
    public OnProviderEnabled(string locationProvider)
    {
        // called when the user enables the provider
    }
    
    public OnStatusChanged(string locationProvider, Availability status, Bundle extras)
    {
        // called when the status of the provider changes (there are a variety of reasons for this)
    }
}
```

### <a name="unsubscribing-to-locationmanager-updates"></a>LocationManager güncelleştirmeleri aboneliği

Sistem kaynaklarını korumak için bir uygulama için konum güncelleştirmeleri mümkün olan en kısa sürede aboneliği. `RemoveUpdates` Yöntemi söyler `LocationManager` uygulamamız için güncelleştirmeleri gönderilmesini durdurmak için.  Örnek olarak, bir etkinlik çağırabilir `RemoveUpdates` içinde `OnPause` yöntemi böylece biz bir uygulama, güç tasarrufu mümkün değil gereken konum güncelleştirmeleri kendi etkinliğini ekranda olmamasına karşın:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Uygulamanızı sırasında arka planda konumu güncelleştirmeleri almak gerekirse, konum hizmeti sisteme abone olan özel bir hizmet oluşturmak istersiniz. Başvurmak [Android hizmetleriyle Backgrounding](~/android/app-fundamentals/services/index.md) daha fazla bilgi için Kılavuzu.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>LocationManager için en iyi konumu sağlayıcısı belirleme

Yukarıdaki uygulama GPS konumu sağlayıcı olarak ayarlar. Ancak, GPS gibi cihazın içeride veya GPS alıcısı yok, her durumda, kullanılamayabilir. Bu durumda, sonucu olan bir `null` dönüş sağlayıcısı.

GPS kullanılamadığında çalışmak için uygulamanızı almak için kullandığınız `GetBestProvider` yöntemi için en iyi uygulama başlatma kullanılabilir (aygıt tarafından desteklenen ve kullanıcı etkin) konum sağlayıcısındaki isteyin. Belirli bir sağlayıcıyı bilgilerinde yerine, size `GetBestProvider` - gibi doğruluk ve güç - sağlayıcısıyla gereksinimlerini bir [ `Criteria` nesne](https://developer.xamarin.com/api/type/Android.Locations.Criteria/). `GetBestProvider` Belirtilen ölçüt için en iyi sağlayıcısı döndürür.

Aşağıdaki kod, en iyi kullanılabilir sağlayıcıyı almak ve konum güncelleştirmelerini isterken kullanmanız gösterilmektedir:

```csharp
Criteria locationCriteria = new Criteria();   
locationCriteria.Accuracy = Accuracy.Coarse;
locationCriteria.PowerRequirement = Power.Medium;

locationProvider = locationManager.GetBestProvider(locationCriteria, true);

if(locationProvider != null)
{
    locationManager.RequestLocationUpdates (locationProvider, 2000, 1, this);
}
else
{
    Log.Info(tag, "No location providers available");
}
```

> [!NOTE]
>  Kullanıcı tüm yerleşim sağlayıcıları, devre dışı bırakılırsa `GetBestProvider` döndürülecek `null`. Bu kodu gerçek bir cihazda nasıl çalıştığını görmek için GPS, Wi-Fi ve cep telefonu ağlar altında etkinleştirmek mutlaka **Google Ayarları > konumu > modu** bu ekran görüntüsünde gösterildiği gibi:

[![Android telefonla ayarları konumu modu ekranı](location-images/location-02.png)](location-images/location-02.png)

Aşağıdaki ekran görüntüsünde konumu çalışan kullanılarak uygulama gösterir `GetBestProvider`:

[![Enlem ve boylam sağlayıcısı görüntüleme GetBestProvider uygulama](location-images/location-03.png)](location-images/location-03.png)

Aklınızda `GetBestProvider` sağlayıcı dinamik olarak değiştirmez. Bunun yerine, bir kez etkinlik yaşam döngüsü sırasında en iyi kullanılabilir sağlayıcıyı belirler. Ayarlandıktan sonra sağlayıcı durumu değişirse, ek kod, uygulamanın gerektirir `ILocationListener` yöntemleri &ndash; `OnProviderEnabled`, `OnProviderDisabled`, ve `OnStatusChanged` &ndash; ilgili her olasılığı işlemek için Sağlayıcı anahtarı.

## <a name="summary"></a>Özet

Bu kılavuzda ele alınan Android konum hizmeti ve Google konum Hizmetleri API Sigortalı konumu sağlayıcıdan kullanarak kullanıcının konumunu alma.


## <a name="related-links"></a>İlgili bağlantılar

- [Konum (örnek)](https://developer.xamarin.com/samples/Location/)
- [FusedLocationProvider (örnek)](https://developer.xamarin.com/samples/FusedLocationProvider/)
- [Google Play Hizmetleri](http://developer.android.com/google/play-services/index.html)
- [Ölçüt sınıfı](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)
- [LocationManager sınıfı](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/)
- [LocationListener sınıfı](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)
- [LocationClient API](http://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener API](http://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
