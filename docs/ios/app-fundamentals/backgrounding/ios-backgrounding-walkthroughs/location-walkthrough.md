---
title: İzlenecek yol - Xamarin.iOS arka plan konumu
description: Bu belge backgrounded bir Xamarin.iOS uygulaması'nda konum bilgilerini kullanmak nasıl bir kılavuz sağlar. Gerekli Kurulum, kullanıcı arabirimi ve uygulama durumlarını açıklar.
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: aef39ef435bbbad6f643b2376832d8f8132d6a4c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784100"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>İzlenecek yol - Xamarin.iOS arka plan konumu

Bu örnekte, biz iOS bizim geçerli konumu hakkında bilgi yazdırır konumu uygulama oluşturmak için kalacaklarını: Enlem ve boylam ekranına diğer parametreleri. Bu uygulamayı uygulama etkin veya Backgrounded durumdayken düzgün konumu güncelleştirmelerinin nasıl yapılacağı gösterilmektedir.

Bu kılavuzda bazı anahtar kavramlar, bir uygulama bir arka plan gerekli uygulaması olarak kaydetme, uygulama backgrounded olduğunda kullanıcı Arabirimi güncelleştirmeleri askıya alma ve çalışma dahil backgrounding açıklar `WillEnterBackground` ve `WillEnterForeground` `AppDelegate` yöntemleri .

## <a name="application-set-up"></a>Uygulama ayarlama


1. İlk olarak, yeni bir oluşturma **iOS > Uygulama > tek görünüm uygulaması (C#)**. Bu çağrı _konumu_ ve iPad ve iPhone seçilmedi emin olun.

1. Arka plan gerekli uygulama iOS olarak konumu uygulama niteler. Uygulamayı düzenleyerek bir konum uygulaması olarak kaydetme **Info.plist** proje dosyası.

    Çözüm Gezgini altında çift tıklatın **Info.plist** dosyasını açın ve listenin alt kısmına kaydırın. Her ikisi için de işaretleyin **arka plan modlarını etkinleştir** ve **konumu güncelleştirmeleri** onay kutuları.

    Mac için Visual Studio'da, onu şöyle bir şey gibi görünür:

    [![](location-walkthrough-images/image7.png "Arka plan modlarını etkinleştir ve konum güncelleştirmeleri onay kutularını işaretleyin")](location-walkthrough-images/image7.png#lightbox)

    Visual Studio'da **Info.plist** aşağıdaki anahtar/değer çifti ekleyerek el ile güncelleştirilmesi gerekir:

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. Uygulama kayıtlı, aygıttan konum verileri elde edebilirsiniz. İOS içinde `CLLocationManager` sınıf konum bilgilerine erişmek için kullanılan ve konum güncelleştirmeleri sağlamak olaylar oluşturabilir.

1. Kod içinde adlı yeni bir sınıf oluşturma `LocationManager` çeşitli ekranları ve kod konumu güncelleştirmelere abone olmak için tek bir yer sağlar. İçinde `LocationManager` sınıfı, bir örneğini olun `CLLocationManager` adlı `LocMgr`:

    ```csharp
    public class LocationManager
    {
        protected CLLocationManager locMgr;

        public LocationManager () {
            this.locMgr = new CLLocationManager();
            this.locMgr.PausesLocationUpdatesAutomatically = false;

            // iOS 8 has additional permissions requirements
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                locMgr.RequestAlwaysAuthorization (); // works in background
                //locMgr.RequestWhenInUseAuthorization (); // only in foreground
            }

            if (UIDevice.CurrentDevice.CheckSystemVersion (9, 0)) {
                locMgr.AllowsBackgroundLocationUpdates = true;
            }
        }

        public CLLocationManager LocMgr {
            get { return this.locMgr; }
        }
    }
    ```

    Yukarıdaki kod üzerinde bir dizi özellikler ve izinleri ayarlar [CLLocationManager](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/) sınıfı:

    - `PausesLocationUpdatesAutomatically` – Sistem konumu güncelleştirmeleri duraklatmak izin verilip verilmediğine bağlı olarak ayarlanabilir bir Boole değeri budur. Bazı cihazlarda, varsayılan olarak `true`, hangi arka plan yaklaşık 15 dakika sonra konum güncelleştirmeleri almayı durdurmak için cihazın neden olabilir.
    - `RequestAlwaysAuthorization` -Uygulama kullanıcı seçeneği konumu arka planda erişilmesine izin vermek için bu yöntemi geçirmelisiniz. `RequestWhenInUseAuthorization` Kullanıcı yalnızca uygulama ön planda olduğunda erişilecek konumuna izin ver seçeneği vermek isterseniz, ayrıca geçirilebilir.
    - `AllowsBackgroundLocationUpdates` – Askıya zaman konumu güncelleştirmeleri almak uygulama izin vermek için ayarlanan iOS 9 sunulan bir Boolean özelliği budur.

    > [!IMPORTANT]
    > iOS 8 (ve büyük) de bir girişe gerektirir **Info.plist** kullanıcı yetkilendirme isteği bir parçası olarak göstermek için dosya.

1. Bir anahtar ekleyin `NSLocationAlwaysUsageDescription` veya `NSLocationWhenInUseUsageDescription` konumu veri erişimi istekleri uyarısında kullanıcıya görüntülenen bir dizeyle.

1. iOS 9 gerektirir kullanırken `AllowsBackgroundLocationUpdates` **Info.plist** anahtar içerir `UIBackgroundModes` değerle `location`. Tamamladıysanız bu kılavuzun 2. adım, bu zaten gereken Info.plist dosyanızdaki bırakıldı.


1. İçinde `LocationManager` sınıfı, adlı bir yöntem oluşturma `StartLocationUpdates` aşağıdaki kod ile. Bu kodu nasıl gösterir konumu güncelleştirmeleri almaya başlamak `CLLocationManager`:

    ```csharp
    if (CLLocationManager.LocationServicesEnabled) {
        //set the desired accuracy, in meters
        LocMgr.DesiredAccuracy = 1;
        LocMgr.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) =>
        {
            // fire our custom Location Updated event
            LocationUpdated (this, new LocationUpdatedEventArgs (e.Locations [e.Locations.Length - 1]));
        };
        LocMgr.StartUpdatingLocation();
    }
    ```

    Bu yöntemde gerçekleştiği birkaç önemli nokta vardır. İlk olarak, uygulama cihaz üzerinde konum verilerine erişimi olup olmadığını görmek için bir onay gerçekleştirin. Biz çağırarak doğrulayın `LocationServicesEnabled` üzerinde `CLLocationManager`. Bu yöntem döndürülecek **false** kullanıcı konumu ile ilgili bilgi için uygulama erişimi reddediliyorsa.

1. Ardından, güncelleştirmek için ne sıklıkta konum yöneticisine bildirir. `CLLocationManager` Filtreleme ve konum verileri, güncelleştirmelerinin sıklığını dahil olmak üzere yapılandırmak için birçok seçenek sağlar. Bu örnekte, ayarlamak `DesiredAccuracy` konumu ölçer tarafından değiştiğinde güncelleştirmek için. Konum güncelleştirme sıklığı ve diğer tercihlerinizi yapılandırma hakkında daha fazla bilgi için başvurmak [CLLocationManager sınıf başvurusu](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) Apple belgelerinde.

1. Son olarak, arama `StartUpdatingLocation` üzerinde `CLLocationManager` örneği. Bu ilk düzeltme geçerli konumu alma ve güncelleştirmeleri gönderme başlatmak için konum yöneticisine bildirir

Şu ana kadar konum yöneticisine istiyoruz almak için veri türleri yapılandırılmış oluşturulmuştur ve ilk konum belirledi. Şimdi kullanıcı arabiriminde konum verileri işlemek kod gerekir. Bu alan için özel bir olay yapabileceğimiz bir `CLLocation` bağımsız değişken olarak:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

Konum güncelleştirmelerini abone olmak için sonraki adımdır `CLLocationManager`ve özel Yükselt `LocationUpdated` konumun bağımsız değişken olarak geçirme yeni konum verileri kullanılabilir hale geldiğinde olay. Bunu yapmak için yeni bir sınıf oluşturun **LocationUpdateEventArgs.cs**. Bu kod, ana uygulama içinde erişilebilir olduğundan ve olay oluşturulduğunda aygıt konumu döndürür:

```csharp
public class LocationUpdatedEventArgs : EventArgs
{
    CLLocation location;

    public LocationUpdatedEventArgs(CLLocation location)
    {
       this.location = location;
    }

    public CLLocation Location
    {
       get { return location; }
    }
}
```

## <a name="user-interface"></a>Kullanıcı Arabirimi

1. Konum bilgileri görüntüler ekran oluşturmak için iOS Tasarımcısı'nı kullanın. Çift **Main.storyboard** başlamak için dosya.

    Şeridinde konum bilgileri yer tutucular olarak görev yapması için ekran birkaç etiketleri sürükleyin. Bu örnekte, enlem ve boylam, yüksekliği, indirmelere ve hız için etiketleri vardır.

    Düzen aşağıdakine benzemelidir:

    ![](location-walkthrough-images/image8.png "Bir örnek UI düzende iOS Tasarımcısı")

1. Çözüm defterinde çift `ViewController.cs` dosyasını ve arama ve LocationManager yeni bir örneğini oluşturmak üzere düzenleyebilirsiniz `StartLocationUpdates`üzerindeki.
  Kod aşağıdaki gibi değiştirin:

    ```csharp
    #region Computed Properties
    public static bool UserInterfaceIdiomIsPhone {
                get { return UIDevice.CurrentDevice.UserInterfaceIdiom == UIUserInterfaceIdiom.Phone; }
            }

    public static LocationManager Manager { get; set;}
    #endregion

    #region Constructors
    public ViewController (IntPtr handle) : base (handle)
    {
    // As soon as the app is done launching, begin generating location updates in the location manager
        Manager = new LocationManager();
        Manager.StartLocationUpdates();
    }

    #endregion
    ```

    Hiçbir veri görüntülenmesine karşın bu uygulama başlangıç üzerinde konumu güncelleştirmeleri başlatır.

1. Konum güncelleştirmeleri alınan, ekran konumu bilgilerle güncelleştirin. Aşağıdaki yöntem konumdan alır bizim `LocationUpdated` olay ve kullanıcı Arabiriminde gösterir:

    ```csharp
    #region Public Methods
    public void HandleLocationChanged (object sender, LocationUpdatedEventArgs e)
    {
        // Handle foreground updates
        CLLocation location = e.Location;

        LblAltitude.Text = location.Altitude + " meters";
        LblLongitude.Text = location.Coordinate.Longitude.ToString ();
        LblLatitude.Text = location.Coordinate.Latitude.ToString ();
        LblCourse.Text = location.Course.ToString ();
        LblSpeed.Text = location.Speed.ToString ();

        Console.WriteLine ("foreground updated");
    }
    #endregion
    ```

Hala abone olmak ihtiyacımız `LocationUpdated` olay AppDelegate ve kullanıcı arabirimini güncelleştirmek için yeni yöntemini çağırın. Aşağıdaki kodu ekleyin `ViewDidLoad,` hemen sonra `StartLocationUpdates` çağırın:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


Şimdi uygulamayı çalıştırdığınızda, aşağıdakine benzer görünmelidir:

[![](location-walkthrough-images/image5.png "Bir örnek uygulamayı çalıştırma")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>Etkin ve arka plan durumları işleme

1. Ön planda ve etkin durumdayken uygulamanın konumu güncelleştirmeleri çıktısı olduğu. Uygulama arka girdiğinde olanlar göstermek için geçersiz kılma `AppDelegate` ön ve arka plan arasında geçiş yaptığında uygulama konsola böylece uygulama izleme yöntemleri durum değişiklikleri:

    ```csharp
    public override void DidEnterBackground (UIApplication application)
    {
        Console.WriteLine ("App entering background state.");
    }

    public override void WillEnterForeground (UIApplication application)
    {
        Console.WriteLine ("App will enter foreground");
    }
    ```

    Aşağıdaki kodu ekleyin `LocationManager` sürekli güncelleştirilen yeri yazdırmak için konum bilgileri doğrulamak için uygulama çıktısı, veri arka planda hala kullanılabilir:

    ```csharp
    public class LocationManager
    {
        public LocationManager ()
        {
        ...
        LocationUpdated += PrintLocation;
        }
        ...

        //This will keep going in the background and the foreground
        public void PrintLocation (object sender, LocationUpdatedEventArgs e) {
        CLLocation location = e.Location;
        Console.WriteLine ("Altitude: " + location.Altitude + " meters");
        Console.WriteLine ("Longitude: " + location.Coordinate.Longitude);
        Console.WriteLine ("Latitude: " + location.Coordinate.Latitude);
        Console.WriteLine ("Course: " + location.Course);
        Console.WriteLine ("Speed: " + location.Speed);
        }
    }
    ```

1. Kod bir kalan sorun olmadığından: uygulama backgrounded olduğunda kullanıcı arabirimini güncelleştirme girişimi neden iOS onu sonlandıracak. Uygulama arka plan içine gittiğinde, konum güncelleştirmelerini aboneliği ve UI güncelleştirme durdurmak kod gerekir.

    iOS sağlar bize bildirimleri uygulama hakkında geçiş farklı bir uygulama için olduğunda durumları. Bu durumda, biz abone olabilirsiniz `ObserveDidEnterBackground` bildirim.

    Aşağıdaki kod parçacığını UI güncelleştirmeleri durdurmak ne zaman bilmek görünüm izin vermek için bir bildirim kullanmayı gösterir. Bu gidecek `ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    Uygulama çalışırken, çıktı aşağıdakine benzer görünecektir:

    ![](location-walkthrough-images/image6.png "Konsolunda konumu çıktı örneği")

1. Uygulama ön planda çalışırken konumu güncelleştirmeleri ekranına yazdırır ve uygulama çıkış penceresine veri arka planda çalışırken yazdırmak devam eder.

Yalnızca bir bekleyen sorunu kalır: ekran UI güncelleştirmeleri uygulamayı ilk yüklendi ancak zaman uygulama ön yeniden geçtiğini bilerek hiçbir şekilde sahip başlatılır. Backgrounded uygulama geri ön alınırsa, kullanıcı Arabirimi güncelleştirmeleri devam olmaz.

Bu sorunu gidermek için kullanıcı Arabirimi güncelleştirmeleri uygulama etkin durumuna girdiğinde, ateşlenir başka bir bildirim içinde başlatmak için bir çağrı iç içe:

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

Şimdi UI uygulamayı ilk kez başlatıldığında ve uygulama her zaman güncelleştirme Sürdür ön alana gelir güncelleştirme başlayacaktır.

Bu kılavuzda, konum verilerini ekran ve uygulama çıkış penceresine yazdırır iyi çalışan, arka plan duyarlı iOS uygulama yerleşik.


## <a name="related-links"></a>İlgili bağlantılar

- [Konum (Bölüm 4) (örnek)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Çekirdek konum Framework başvurusu](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
