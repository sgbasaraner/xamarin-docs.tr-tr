---
title: Konum Hizmetleri
description: Bu kılavuz konum tanıma Android uygulamalarında tanıtır ve konum çarpım sağlayıcıyı yanı sıra Android konum hizmeti API'si ile Google konum Hizmetleri API'si kullanarak kullanıcının konumunu alma gösterir.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/22/2018
ms.openlocfilehash: 2cd49441ec9ba15cd7fd2647fa74b39b32ea8acd
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780670"
---
# <a name="location-services"></a>Konum Hizmetleri

_Bu kılavuz konum tanıma Android uygulamalarında tanıtır ve konum çarpım sağlayıcıyı yanı sıra Android konum hizmeti API'si ile Google konum Hizmetleri API'si kullanarak kullanıcının konumunu alma gösterir._

## <a name="location-services-overview"></a>Konum hizmetleri genel bakış

Android hücre tower konumu, Wi-Fi ve GPS gibi çeşitli konumu teknolojileri erişim sağlar. Her bir konum teknolojiyi ayrıntılarını aracılığıyla soyutlanır *yerleşim sağlayıcıları*, kullanılan sağlayıcı bağımsız olarak aynı şekilde konumları almak uygulama izin verme. Bu kılavuz Google Play akıllıca hangi sağlayıcıları kullanılabilir ve cihazı nasıl kullanıldığını bağlı cihazları konumunu almak için en iyi yolu belirleyen hizmetlerin bir parçası olan çarpım konumu sağlayıcısı tanıtır. Android konum hizmeti API'si ve sistem konumu ile iletişim kurmak nasıl gösterir kullanarak hizmet bir `LocationManager`. Android konum Hizmetleri API'si kullanılarak Kılavuzu ikinci bölümü keşfediyor `LocationManager`.
 
Bir genel kural karşısında, uygulamalar eski Android konum hizmeti API'si yalnızca gerekli olduğunda dönülüyor çarpım konumu sağlayıcısı kullanmaya tercih etmelisiniz.

## <a name="location-fundamentals"></a>Konumu temelleri

Seçtiğiniz konum verilerle çalışmak için hangi API ne olursa olsun, Android birkaç kavram aynı kalır. Bu bölüm, yerleşim sağlayıcıları ve konumu ile ilgili izinleri tanıtır.

### <a name="location-providers"></a>Yerleşim sağlayıcıları

Çeşitli teknolojiler, kullanıcının konumunu saptamak için dahili olarak kullanılır. Kullanılan donanımın türüne bağlıdır *konumu sağlayıcısı* veri toplama iş için seçildi. Android üç yerleşim sağlayıcıları kullanır:

-   **GPS sağlayıcısı** &ndash; GPS en doğru konumu sağlar, kullanan en çok, gücü ve dışarıda en iyi şekilde çalışır. Bu sağlayıcı, GPS ve Yardımlı GPS bileşimini kullanır ([aGPS](http://en.wikipedia.org/wiki/Assisted_GPS)), hangi hücresel towers tarafından toplanan GPS verileri döndürür.

-   **Ağ sağlayıcısı** &ndash; hücre towers tarafından toplanan aGPS veriler dahil olmak üzere, WiFi ve hücresel veri bir birleşimini sağlar. GPS sağlayıcılarına kıyasla daha az güç kullanır, ancak çeşitli doğruluğu, konumu döndürür.

-   **Pasif sağlayıcısı** &ndash; konum verileri bir uygulama oluşturmak için diğer uygulamalar veya hizmetler tarafından istenen sağlayıcılarını kullanarak bir "Paketle" seçeneği. Çalışmak için sabit bir konum güncelleştirmeleri gerektirmeyen uygulamalar için ideal güç tasarrufu seçeneği ancak daha az güvenilir budur.

Yerleşim sağlayıcıları her zaman kullanılabilir değildir. Örneğin, GPS uygulamamız için kullanılacak isteyebilirsiniz ancak ayarlarında GPS kapatılabilir veya cihaz GPS hiç sahip olmayabilir. Belirli bir sağlayıcı kullanılabilir durumda değilse, bu sağlayıcı döndürebilir seçme `null`.

### <a name="location-permissions"></a>Yeri izinleri

Konum kullanan bir uygulama, GPS, Wi-Fi ve hücresel veri almak için bir cihazın donanım sensörlerini erişim gerekir. Erişim, uygulamanın Android bildirim uygun izinleri ile denetlenir.
Kullanılabilir iki izinleri &ndash; uygulamanızın gereksinimlerini ve seçtiğiniz API bağlı olarak, bir izin vermek istediğiniz:

-   `ACCESS_FINE_LOCATION` &ndash; GPS bir uygulama erişim sağlar.
    Gerekli *GPS sağlayıcısı* ve *pasif sağlayıcısı* seçenekleri (*pasif sağlayıcısı gereken başka bir uygulama veya hizmet tarafından toplanan GPS veri erişim izni*). İsteğe bağlı izinlerini *ağ sağlayıcısı*.

-   `ACCESS_COARSE_LOCATION` &ndash; Bir uygulama hücresel ve Wi-Fi konuma erişim sağlar. Gerekli *ağ sağlayıcısı* varsa `ACCESS_FINE_LOCATION` ayarlı değil.

API sürümü (Android 5.0 Lollipop) 21 hedefleyen uygulamalar için veya üzeri etkinleştirebilirsiniz `ACCESS_FINE_LOCATION` ve GPS donanıma sahip değilseniz cihazlarda çalışan yine de. Açıkça eklemeniz gerekir, uygulamanızı GPS donanım gerektirip gerektirmediğini bir `android.hardware.location.gps` `uses-feature` Android bildirim öğesi. Daha fazla bilgi için bkz: Android [kullandığı özellik](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) öğe başvurusu.

İzinleri ayarlamak için genişletin **özellikleri** klasöründe **çözüm bölmesi** ve çift **AndroidManifest.xml**. İzinler altında listelenen **gerekli izinler**:

[![Android bildirimi gerekli izinler ayarlarının ekran görüntüsü](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Ya da bu izinleri ayarlama, Android uygulamanız için yerleşim sağlayıcıları erişmek için kullanıcıdan izin gerektiğini bildirir. API düzeyi 22 (Android 5.1) çalıştırın veya düşük uygulamasının yüklü olduğu her zaman bu izinleri vermek için kullanıcıdan cihazlar. API çalıştıran cihazlarda düzeyi 23 içindir (Android 6.0) veya üzeri uygulama sağlayıcının konumu isteği yapmadan önce bir çalışma zamanı izni denetimi gerçekleştirmeniz gerekir. 

> [!NOTE]
>Not: Ayarlama `ACCESS_FINE_LOCATION` kaba ve ince konum verileri hem de erişiminiz olduğu anlamına gelir. Her iki izinler, yalnızca ayarlamak hiçbir zaman olmalıdır *en az* çalışmaya uygulamanız için gerekli izni.

Bu kod parçacığı bir uygulama için izni olduğunu kontrol etmek nasıl bir örneğidir `ACCESS_FINE_LOCATION` izin:

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

Uygulamalar, nerede kullanıcı izni vermek değil (veya izni iptal edilmiş) senaryo dayanıklı ve düzgün bir şekilde bu durumu düzeltmek için bir yönteme sahip. Lütfen [izinleri Kılavuzu](~/android/app-fundamentals/permissions.md) çalışma zamanı izni uygulama konusunda daha fazla ayrıntı için Xamarin.Android denetler.


## <a name="using-the-fused-location-provider"></a>Konum çarpım sağlayıcısını kullanma

Konum çarpım sağlayıcısı pil verimli bir şekilde en iyi konumu bilgileri sağlamak için çalışma zamanı sırasında sağlayıcının konumu seçin etkili çünkü CİHAZDAN konum güncelleştirmeleri almak Android uygulamaları için tercih edilen yoludur. Örneğin, geçici bir çözüm açık yürüyen bir kullanıcı ile GPS okuma en iyi konumu alır. Ardından kullanıcı içeride gösterilmektedir, burada GPS çalışır kötü (varsa), konum çarpım sağlayıcısı otomatik olarak daha iyi çalışır içeride WiFi için geçiş yapabilirsiniz.
 
Konum çarpım sağlayıcısı API'si çeşitli diğer araçları konum algılamalı uygulamalar, bölge sınırlaması ve etkinlik izleme gibi olanağı sağlar. Bu bölümde, odağı kurma temelleri hakkında kullanacağız `LocationClient`, sağlayıcıları oluşturma ve kullanıcının konumunu alma.

Konum çarpım sağlayıcısı parçasıdır [Google Play Hizmetleri](http://developer.android.com/google/play-services/index.html).
Google Play Hizmetleri paketi yüklü ve düzgün çalışması için uygulamada çarpım konumu sağlayıcısı API'si için yapılandırılmış ve bir cihaz Google Play Hizmetleri yüklü apk'yı olması gerekir.

Önce bir Xamarin.Android uygulaması, bir konum çarpım sağlayıcıyı kullanabilir, eklemelisiniz **Xamarin.GooglePlayServices.Maps** projeye paket. Ayrıca, aşağıdaki `using` deyimleri, aşağıda açıklanan sınıfları başvuran tüm kaynak dosyaları eklenmelidir:

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Google Play Hizmetleri yüklü olup olmadığını denetleme

Bir Xamarin.Android Google Play Hizmetleri yüklü olmadığında çarpım konumu sağlayıcı kullanmaya çalışırsa, kilitlenme (veya süresi dolmuş) sonra bir çalışma zamanı özel durumu oluşacaktır.  Google Play Hizmetleri yüklü değilse, ardından uygulamayı yukarıda açıklanan Android konum hizmeti için dönmesi. Google Play Hizmetleri, eski ise, uygulamanın Google Play Hizmetleri'nın yüklü sürümü güncelleştirmek için bunları isteyen kullanıcı bir ileti görüntüleyebilir.

Bu kod parçacığı nasıl Android etkinliği programlı olarak Google Play Hizmetleri yüklü olup olmadığını kontrol edebilirsiniz, bir örnek verilmiştir:

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

Konum çarpım sağlayıcı ile etkileşim kurmak için bir Xamarin.Android uygulaması bir örneğine sahip olması `FusedLocationProviderClient`. Bu sınıf, konum güncelleştirmelere abone olmak ve cihazın bilinen son konumunu almak için gereken yöntemleri sunar.

`OnCreate` Yöntemdir etkinliğin bir başvuru almak için uygun bir yere `FusedLocationProviderClient`, aşağıdaki kod parçacığında gösterildiği gibi:

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

### <a name="getting-the-last-known-location"></a>Son bilinen konum alınıyor

`FusedLocationProviderClient.GetLastLocationAsync()` Yöntemi hızlı bir şekilde ek yükü en az kodlama ile cihazın bilinen son konumunu almak bir Xamarin.Android uygulaması için basit, engelleyici olmayan bir yol sağlar.

Bu kod parçacığı nasıl kullanılacağını gösterir `GetLastLocationAsync` yönteminin cihazın konumunu almak için:

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

Bir Xamarin.Android uygulaması kullanarak çarpım konumu sağlayıcısından de konum güncelleştirmeleri için abone olabilirsiniz `FusedLocationProviderClient.RequestLocationUpdatesAsync` Bu kod parçacığında gösterildiği gibi yöntemi:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
Bu yöntem iki parametre alır:

-   **`Android.Gms.Location.LocationRequest`** &ndash; A `LocationRequest` nesnedir nasıl çarpım konumu sağlayıcısı nasıl çalışmalıdır bir Xamarin.Android uygulaması parametrelerini geçirir. `LocationRequest` Bilgileri gibi sık istekler yapılacak veya olmalıdır ne kadar önemli bir doğru konuma güncelleştirme nasıl tutar. Örneğin, bir önemli konumu isteği, GPS ve sonuç olarak daha fazla güç konumu belirlenirken kullanılacak cihazın neden olur. Bu kod parçacığı nasıl oluşturulacağını gösterir. bir `LocationRequest` yaklaşık her beş denetimi için bir konum ile yüksek doğruluk dakika için bir konum güncelleştirme (ancak erken istekler arasında iki dakikadan değil). Konum çarpım sağlayıcı kullanacak bir `LocationRequest` olarak çalışırken, cihaz konumu belirlenemiyor kullanmak hangi konuma sağlayıcısı için yönergeler:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; Konum güncelleştirmeleri almak için alt sınıf bir Xamarin.Android uygulaması gerekir `LocationProvider` soyut sınıf. Bu sınıf, belki de çağrılır uygulamayı güncelleştirme konum bilgileri ile çarpım konumu sağlayıcısı tarafından iki yöntem ortaya çıkar. Bu aşağıda daha ayrıntılı olarak açıklanmıştır.

Konumu çarpım sağlayıcının konumu güncelleştirmesi bir Xamarin.Android uygulamaya bildirmek için çağırır `LocationCallBack.OnLocationResult(LocationResult result)`. `Android.Gms.Location.LocationResult` Parametresi güncelleştirme konum bilgileri içerir.

Konum çarpım sağlayıcısı konum verileri kullanılabilirliğini içinde bir değişiklik algıladığında, çağrı `LocationProvider.OnLocationAvaibility(LocationAvailability
locationAvailability)` yöntemi. Varsa `LocationAvailability.IsLocationAvailable` özelliği döndürür `true`, cihaz konumu sonuçları tarafından bildirilen varsayılabilir sonra `OnLocationResult` doğru ve olarak güncel gerektirdiği `LocationRequest`. Varsa `IsLocationAvailable` konumu sonuç tarafından iade sonra yanlış `OnLocationResult`.

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

## <a name="using-the-android-location-service-api"></a>Android konum hizmeti API'sini kullanma

Android konum hizmeti Android'de konum bilgilerini kullanmak için eski bir API'dir. Konum verileri donanım sensörlerini tarafından toplanır ve uygulama ile erişilebilen bir sistem hizmet tarafından toplanan bir `LocationManager` sınıfı ve `ILocationListener`.

Konum hizmeti, Google Play Hizmetleri yüklü olmayan cihazlarda çalıştırılması gereken uygulamalar için idealdir.

Konum hizmeti özel bir türüdür [hizmet](http://developer.android.com/guide/components/services.html) sistem tarafından yönetilir. Sistem hizmeti, cihaz donanım ile etkileşime geçer ve her zaman çalışır durumda. Konum güncelleştirmeleri uygulamamızdaki uygulamasına dokunun, biz konum güncelleştirmeleri kullanarak sistem konum hizmeti abone olur bir `LocationManager` ve `RequestLocationUpdates` çağırın.

Android konum hizmeti kullanarak kullanıcının konumunu almak için çeşitli adımları içerir:

1.  Bir başvuru almak `LocationManager` hizmeti.
2.  Uygulama `ILocationListener` konumu değiştiğinde arabirim ve tutamacı olayları.
3.  Kullanım `LocationManager` belirtilen sağlayıcı için istek konum güncelleştirmeleri. `ILocationListener` Önceki adımdan gelen geri çağırmaları almak için kullanılan `LocationManager`.
4.  Uygulama artık güncelleştirmelerini almaya uygun olduğunda konum güncelleştirmeleri durdurun.

### <a name="location-manager"></a>Konum Yöneticisi

Sistem konum hizmeti örneğiyle birlikte erişip `LocationManager` sınıfı. `LocationManager` Sistem konumu hizmeti ile etkileşime geçmek ve üzerinde yöntemleri çağırmak aracılığıyla almamızı sağlar özel bir sınıftır. Bir uygulama için bir başvuru alabilirsiniz `LocationManager` çağırarak `GetSystemService` ve aşağıda gösterildiği gibi bir hizmet türüne geçirme:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` bir başvuru almak için uygun bir yerdir `LocationManager`.
Tutmak için iyi bir fikirdir `LocationManager` sınıfı değişken, böylece biz etkinlik yaşam döngüsü çeşitli noktalarında çağırabilirsiniz.

### <a name="request-location-updates-from-the-locationmanager"></a>Konum güncelleştirmeleri LocationManager isteği

Uygulama için bir başvuruya sahip sonra `LocationManager`, bildirmeniz gereken `LocationManager` konum bilgilerini ne tür, gerekli ve ne sıklıkta bu bilgileri güncelleştirilecek. Çağrı yaparak bunu `RequestionLocationUpdates` üzerinde `LocationManager` nesne ve güncelleştirmeleri ve konum güncelleştirmeleri alacak bir geri çağırma bazı ölçütleri geçirirsiniz. Bu geri çağırma uygulamalıdır türüdür `ILocationListener` arabirimi (Bu kılavuzun devamında daha ayrıntılı açıklanmıştır).

`RequestionLocationUpdates` Yöntemi uygulamanızı konum güncelleştirmeleri almaya başlamak istediğiniz sistem konum hizmeti söyler. Bu yöntem, sağlayıcı, hem de saat ve uzaklık eşikleri güncelleştirme sıklığını denetlemek için belirtmenize olanak tanır. Örneğin, GPS konumu Sağlayıcısı'ndan istekleri konum aşağıda yöntemi her 2000 milisaniye güncelleştirir ve yalnızca konumu birden fazla 1 metre değiştiğinde:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Bir uygulama, konum güncelleştirmeleri yalnızca gerektiği sıklıkta iyi gerçekleştirmek uygulamanın istemeniz gerekir. Bu durum pil ömrünü korur ve kullanıcı için daha iyi bir deneyim oluşturur.

### <a name="responding-to-updates-from-the-locationmanager"></a>Güncelleştirmeleri LocationManager yanıt

Uygulama güncelleştirmeleri istedi sonra `LocationManager`, uygulayarak hizmetten bilgi alabileceği [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/) arabirimi. Bu arabirim için hizmet konumu ve konum sağlayıcısına dinleme dört yöntemler sağlar `OnLocationChanged`. Sistem'ın arayacağı `OnLocationChanged` kullanıcının konumu değiştiğinde konum güncelleştirmelerini isterken ayarlamak ölçütlere göre konumu değişiklik olarak nitelemek için yeterli. 

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

Sistem kaynaklarını korumak için bir uygulama için konum güncelleştirmeleri olabildiğince çabuk aboneliği. `RemoveUpdates` Yöntemi söyler `LocationManager` güncelleştirmeleri uygulamamıza göndermeyi durdurun.  Örneğin, bir etkinlik çağırabilir `RemoveUpdates` içinde `OnPause` yöntemi biz bir uygulama bildirimi tasarrufu yapabilir, böylece etkinleştirilmesine gerek konum güncelleştirmeleri, etkinlik ekranında değilken:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Uygulamanız arka plandayken konum güncelleştirmeleri almak gerekiyorsa, sistemde konum hizmeti abone olan özel bir hizmet oluşturmak isteyebilirsiniz. Başvurmak [Android hizmetler ile arka planda işleme](~/android/app-fundamentals/services/index.md) Kılavuzu daha fazla bilgi için.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>LocationManager için en iyi konumu sağlayıcısı belirleme

Yukarıdaki uygulama GPS konumu sağlayıcı olarak ayarlar. Ancak, GPS gibi cihaz içeride veya GPS alıcısı yok, her durumda, kullanılabilir olmayabilir. Bu durumda, sonucu olan bir `null` dönüş sağlayıcısı.

GPS kullanılamadığı zamanlarda çalışmak için uygulamanızı almak için kullandığınız `GetBestProvider` uygulama başlatılırken en iyi kullanılabilir (cihaz tarafından desteklenen ve kullanıcı özellikli) konum sağlayıcısında soran yöntemi. Belirli bir sağlayıcı olarak geçirme yerine size `GetBestProvider` - gibi doğruluk ve güç - sağlayıcısıyla gereksinimlerini bir [ `Criteria` nesne](https://developer.xamarin.com/api/type/Android.Locations.Criteria/). `GetBestProvider` Belirtilen ölçüt için en iyi sağlayıcı döndürür.

Aşağıdaki kod, en iyi kullanılabilir sağlayıcısı alın ve konum güncelleştirmelerini isterken kullanma gösterilmektedir:

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
>  Tüm yerleşim sağlayıcıları, kullanıcı devre dışı bırakmışsa `GetBestProvider` döndüreceği `null`. Bu kod bir gerçek cihazda nasıl çalıştığını görmek için GPS, Wi-Fi ve altında hücresel ağlar sağlamak mutlaka **Google ayarlar > Konum > modu** bu ekran görüntüsünde gösterildiği gibi:

[![Android telefonda ayarları konumu modu ekranı](location-images/location-02.png)](location-images/location-02.png#lightbox)

Konum çalışan kullanılarak uygulama aşağıdaki ekran görüntüsünde gösterilmiştir `GetBestProvider`:

[![Enlem ve boylam sağlayıcısı görüntüleme GetBestProvider uygulama](location-images/location-03.png)](location-images/location-03.png#lightbox)

Aklınızda `GetBestProvider` sağlayıcısı dinamik olarak değiştirmez. Bunun yerine bir kez etkinlik yaşam döngüsü sırasında en iyi kullanılabilir sağlayıcıyı belirler. Sağlayıcı Durumu ayarlandıktan sonra değişirse, uygulama içinde ek kod gerektirir `ILocationListener` yöntemleri &ndash; `OnProviderEnabled`, `OnProviderDisabled`, ve `OnStatusChanged` &ndash; ilgili her olasılığı işlemek için Sağlayıcı anahtarı.

## <a name="summary"></a>Özet

Bu kılavuzda hem Android konum hizmeti hem de Google konum Hizmetleri API'si çarpım konumu sağlayıcıdan kullanılarak kullanıcının konumunu alma kapsamında.


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
