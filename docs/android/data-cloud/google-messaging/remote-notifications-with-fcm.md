---
title: Uzak bildirimler Firebase ile bulut Mesajlaşma
description: Bu izlenecek yol, bir Xamarin.Android uygulamasına Firebase Cloud Messaging (anında iletme bildirimleri olarak da bilinir) uzak bildirimler uygulamak için nasıl kullanılacağını adım adım bir açıklama sağlar. Firebase Cloud Messaging (FCM) ile iletişim için gereklidir, Android bildirim FCM erişim için yapılandırma örnekleri sağlar ve aşağı akış Firebase kullanarak Mesajlaşma gösterir çeşitli sınıfları uygulamak nasıl gösterir Konsolu.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: 36ac1be1274ff90d573aa53e5c86ae0a97709505
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514433"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Uzak bildirimler Firebase ile bulut Mesajlaşma

_Bu izlenecek yol, bir Xamarin.Android uygulamasına Firebase Cloud Messaging (anında iletme bildirimleri olarak da bilinir) uzak bildirimler uygulamak için nasıl kullanılacağını adım adım bir açıklama sağlar. Firebase Cloud Messaging (FCM) ile iletişim için gereklidir, Android bildirim FCM erişim için yapılandırma örnekleri sağlar ve aşağı akış Firebase kullanarak Mesajlaşma gösterir çeşitli sınıfları uygulamak nasıl gösterir Konsolu._

## <a name="fcm-notifications-overview"></a>FCM bildirimleri genel bakış

Bu izlenecek yolda temel bir uygulama olarak adlandırılan **FCMClient** FCM Mesajlaşma temel bilgileri göstermek için oluşturulur. **FCMClient** Google Play Hizmetleri varlığını denetler, FCM kayıt belirteçlerini alır, Firebase konsolundan gönderdiğiniz uzak bildirimler görüntüler ve konu iletilerine abone olur:

[![Örnek uygulamasının ekran görüntüsü](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

Aşağıdaki konu başlıkları incelenecek:

1.  Arka plan bildirimleri

2.  Konu iletileri

3.  Ön plan bildirimleri

Bu kılavuz boyunca işlevselliği için artımlı olarak ekleyeceksiniz **FCMClient** ve uygulamayı bir cihaz veya öykünücüsü'nü FCM ile nasıl etkileşim kurduğunu anlama. Dinamik uygulama işlemi FCM sunucularıyla Tanık için günlüğü kullanır ve bildirimleri Firebase Konsolu bildirimleri GUI girdiğiniz FCM iletileri nasıl oluşturulduğunu gözlemleyeceksiniz.

## <a name="requirements"></a>Gereksinimler


İle kendinizi alıştırın yararlı [iletileri farklı türde](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) gönderilebilen Firebase Cloud Messaging ile. İletinin yükü nasıl bir istemci uygulaması alır ve iletiyi belirler.

Bu adım adım kılavuza devam etmeden önce Google'nın FCM sunucularını kullanmak için gerekli kimlik bilgilerini edinmeniz gerekir; Bu işlem açıklanan [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm).
Özellikle, yüklemelidir **google-services.json** bu kılavuzda sunulan örnek kod ile kullanılacak dosya. Bir proje Firebase konsolunda oluşturmadınız varsa (veya değil henüz indirdiyseniz **google-services.json** dosya), bkz: [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

Örnek uygulamayı çalıştırmak için bir test Android cihaz veya öykünücü Firebase ile uyumlu olan gerekir. Firebase Cloud Messaging, Android 4.0 veya üzeri sürümde çalışan istemcileri destekler ve bu cihazlar da olmalıdır yüklü Google Play Store uygulamasını (Google Play Hizmetleri 9.2.1 veya üzeri gereklidir). Google Play Store uygulaması Cihazınızda yüklü henüz yoksa ziyaret [Google Play](https://support.google.com/googleplay) indirmek ve yüklemek için web sitesi. Alternatif olarak, bir sınama cihazında (, Android SDK öykünücüsü kullanıyorsanız, Google Play Store yüklemeniz gerekmez) yerine yüklü olan Google Play Hizmetleri ile Android SDK öykünücüsü'nü kullanabilirsiniz.

## <a name="start-an-app-project"></a>Bir uygulama projesi Başlat

Başlamak için adlı bir yeni boş Xamarin.Android projesi oluşturmanız **FCMClient**. Xamarin.Android projeleri oluşturma ile ilgili bilgi sahibi değilseniz bkz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).
Yeni uygulama oluşturulduktan sonra sonraki adıma paket adı ayarlayın ve FCM ile iletişim için kullanılan birkaç NuGet paketlerini yüklemektir.

### <a name="set-the-package-name"></a>Paket adı ayarlayın

İçinde [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), FCM özellikli uygulama için bir paket adı belirtildi. Bu paket adı da görür [ *uygulama kimliği* ](./firebase-cloud-messaging.md#fcm-in-action-app-id) ilişkili [API anahtarı](firebase-cloud-messaging.md#fcm-in-action-api-key). Bu paket adını kullanmak üzere uygulamayı yapılandırır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Özelliklerini açın **FCMClient** proje.

2.  İçinde **Android bildirim** sayfasında, paket adı ayarlayın.

Aşağıdaki örnekte, paket adı kümesine `com.xamarin.fcmexample`:

[![Paket adı ayarlama](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

Güncelleştirdiğiniz sırada **Android bildirim**, emin olmak için de onay `Internet` izin etkinleştirilir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Özelliklerini açın **FCMClient** proje.

2.  İçinde **Android uygulaması** sayfasında, paket adı ayarlayın.

Aşağıdaki örnekte, paket adı kümesine `com.xamarin.fcmexample`:

[![Paket adı ayarlama](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

Güncelleştirdiğiniz sırada **Android bildirim**, emin olmak için de onay `INTERNET` izin etkin (altında **gerekli izinler**).

-----

> [!IMPORTANT]
> Bu paket adı yoksa, FCM kayıt belirtecinizi almak istemci uygulaması derlenemez *tam olarak* Firebase konsolunda girildi paket adı ile eşleşmesi.

### <a name="add-the-xamarin-google-play-services-base-package"></a>Xamarin Google Play Hizmetleri temel paketi ekleme

Firebase Cloud Messaging için Google Play hizmetleri üzerinde bağlı olduğundan [Xamarin Google Play Hizmetleri - temel](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) NuGet paketini Xamarin.Android projeye eklenmesi gerekiyor. Sürüm 29.0.0.2 gerekir veya üzeri.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio'da sağ **başvuruları > NuGet paketlerini Yönet...** .

2.  Tıklayın **Gözat** sekmesinde ve arama **Xamarin.GooglePlayServices.Base**.

3.  Bu pakete yükleyin **FCMClient** proje:

    [![Google Play Hizmetleri temel yükleme](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Mac için Visual Studio'da sağ **paketleri > paketleri Ekle...** .

2.  Arama **Xamarin.GooglePlayServices.Base**.

3.  Bu pakete yükleyin **FCMClient** proje:

    [![Google Play Hizmetleri temel yükleme](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

NuGet'ın yüklemesi sırasında bir hata alırsanız, kapatmak **FCMClient** project, yeniden açın ve NuGet yükleme işlemini yeniden deneyin.

Yüklediğinizde **Xamarin.GooglePlayServices.Base**, tüm gerekli bağımlılıkları da yüklenir. Düzen **MainActivity.cs** ve aşağıdakileri ekleyin `using` deyimi:

```csharp
using Android.Gms.Common;
```

Bu bildirimi yapar `GoogleApiAvailability` sınıfını **Xamarin.GooglePlayServices.Base** kullanılabilir **FCMClient** kod.
`GoogleApiAvailability` Google Play Hizmetleri olup olmadığını denetlemek için kullanılır.

### <a name="add-the-xamarin-firebase-messaging-package"></a>Firebase Xamarin Messaging paketini ekleyin

FCM iletileri alacak şekilde [Xamarin Firebase - Mesajlaşma](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet paketi uygulama projesine eklenmesi gerekir. Bu paket olmadan bir Android uygulaması FCM sunuculardan mesaj alamıyor.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio'da sağ **başvuruları > NuGet paketlerini Yönet...** .

2. Arama **Xamarin.Firebase.Messaging**.

3.  Bu pakete yükleyin **FCMClient** proje:

    [![Xamarin Mesajlaşma Firebase yükleme](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Mac için Visual Studio'da sağ **paketleri > paketleri Ekle...** .

2.  Denetleme **ön sürüm paketleri Göster** araması **Xamarin.Firebase.Messaging**.

3.  Bu pakete yükleyin **FCMClient** proje:

    [![Xamarin Mesajlaşma Firebase yükleme](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

Yüklediğinizde **Xamarin.Firebase.Messaging**, tüm gerekli bağımlılıkları da yüklenir.

Ardından, Düzenle **MainActivity.cs** ve aşağıdakileri ekleyin `using` ifadeleri:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

İlk iki deyim türlerinde olun **Xamarin.Firebase.Messaging** NuGet paketi için kullanılabilir **FCMClient** kod. **Android.Util** FMS işlemleri izlemek için kullanılan günlük işlevsellik ekler.

### <a name="add-googleplayservices-json"></a>Google Hizmetleri JSON dosyası ekleme

Sonraki adım eklemektir **google-services.json** dosyayı projenizin kök dizine:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Kopyalama **google-services.json** proje klasörüne.

2.  Ekleme **google-services.json** uygulama projesine (tıklayın **tüm dosyaları göster** içinde **Çözüm Gezgini**, sağ tıklayın **google-services.json**, ardından **Proje Ekle**).

3.  Seçin **google-services.json** içinde **Çözüm Gezgini** penceresi.

4.  İçinde **özellikleri** bölmesinde, **derleme eylemi** için **GoogleServicesJson** (varsa **GoogleServicesJson** yapı eylemi gösterilmez Kaydet ve çözümü kapatın ve yeniden):

    [![Derleme eylemi için GoogleServicesJson ayarlama](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Kopyalama **google-services.json** proje klasörüne.

2.  Ekleme **google-services.json** uygulama projesi.

3.  Sağ **google-services.json**.

4.  Ayarlama **derleme eylemi** için **GoogleServicesJson**:

    [![Derleme eylemi için GoogleServicesJson ayarlama](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

Zaman **google-services.json** projeye eklendi (ve **GoogleServicesJson** derleme eylemi), istemci kimliği yapıya ayıklar ve [API anahtarı](./firebase-cloud-messaging.md#fcm-in-action-api-key) ve ardından Bu kimlik bilgileri birleştirilmiş/oluşturulan ekler **AndroidManifest.xml** konumunda bulunan **obj/Debug/android/AndroidManifest.xml**. Bu birleştirme işlemi, izinleri ve FCM sunucularına bağlantı oluşturmak için gereken diğer FCM öğeleri otomatik olarak ekler.


## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Google Play Hizmetleri'ni denetleyin ve bir bildirim kanalı oluşturmak

Google Android uygulamaları Google Play Hizmetleri özellikleri erişmeden önce Google Play Hizmetleri APK varlığını denetlemek önerir (daha fazla bilgi için [denetlemek için Google Play Hizmetleri](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)).

Bir başlangıç düzeni için uygulamanın UI önce oluşturulur. Düzen **Resources/layout/Main.axml** ve içeriğini aşağıdaki XML ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

Bu `TextView` Google Play Hizmetleri yüklü olup olmadığını belirten bir ileti görüntülemek için kullanılır. Değişiklikleri kaydetmek **Main.axml**.


Düzen **MainActivity.cs** ve aşağıdaki örnek değişkenleri ekleyip `MainActivity` sınıfı:

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

Değişkenleri `CHANNEL_ID` ve `NOTIFICATION_ID` yönteminde kullanılan [ `CreateNotificationChannel` ](#create-notification-channel-code) için eklenecek `MainActivity` bu kılavuzda daha sonra.


Aşağıdaki örnekte, `OnCreate` yöntemi, FCM hizmetlerini kullanmak uygulamayı denemeden önce Google Play Hizmetleri kullanılabilir olmadığını doğrulayın.
Aşağıdaki yöntemi ekleyin `MainActivity` sınıfı:

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "This device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

Bu kod, cihaz Google Play Hizmetleri apk'yı yüklü olup olmadığını denetler. Yüklenmezse, bir ileti görüntülenir `TextBox` kullanıcı bir APK Google Play Store ' indirin (veya cihazın sistem ayarlarında etkinleştirin) bildirir.

<a name="create-notification-channel-code"></a>Android 8.0 (API düzeyi 26) veya üzeri sürümde çalışan uygulamaların oluşturmalısınız bir [ _bildirim kanalı_ ](~/android/app-fundamentals/notifications/local-notifications.md) kendi bildirimleri yayımlama.  Aşağıdaki yöntemi ekleyin `MainActivity` sınıfı (gerekirse) bildirim kanalı oluşturur:

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var channel = new NotificationChannel(MyFirebaseMessagingService.CHANNEL_ID,
                                          "FCM Notifications",
                                          NotificationImportance.Default)
                  {

                      Description = "Firebase Cloud Messages appear in this channel"
                  };

    var notificationManager = (NotificationManager)GetSystemService(Android.Content.Context.NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Değiştirin `OnCreate` yöntemini aşağıdaki kod ile:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();

    CreateNotificationChannel();
}
```

`IsPlayServicesAvailable` sonunda çağrılır `OnCreate` böylece çalışmaları her zaman uygulama başlatılır, Google Play Hizmetleri denetleyin. Yöntem `CreateNotificationChannel` Android 8 çalıştıran cihazlar için bir bildirim kanalı var olduğundan emin olmak için çağrılan ya da daha büyük. Uygulamanızın varsa bir `OnResume` yöntemi çağırmanız `IsPlayServicesAvailable` gelen `OnResume` de. Tamamen yeniden oluşturun ve uygulamayı çalıştırın. Tüm yapılandırılırsa düzgün bir şekilde, aşağıdaki ekran görüntüsü gibi görünen bir ekran görmeniz gerekir:

[![Uygulamanın Google Play Hizmetleri kullanılabilir olduğunu belirtir](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

Bu sonucu alamazsanız, Google Play Hizmetleri APK Cihazınızda yüklü olduğunu doğrulayın (daha fazla bilgi için [ayarı yukarı Google Play Hizmetleri](https://developers.google.com/android/guides/setup)).
Ayrıca, eklediğiniz doğrulayın **Xamarin.Google.Play.Services.Base** paketini, **FCMClient** proje daha önce açıklandığı gibi.


## <a name="add-the-instance-id-receiver"></a>Örnek kimliği alıcı Ekle

Genişleten bir hizmet eklemek için sonraki adımdır `FirebaseInstanceIdService` oluşturma, döndürme, işlenecek ve güncelleştirilmesi [Firebase kayıt belirteçleri](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token). `FirebaseInstanceIdService` Hizmet, FCM cihaza ileti göndermek için gereklidir. Zaman `FirebaseInstanceIdService` hizmeti, istemci uygulamaları için eklenir, uygulamayı otomatik olarak FCM iletileri almak ve uygulama backgrounded her bildirim görüntüler.

### <a name="declare-the-receiver-in-the-android-manifest"></a>Android bildirim alıcısında bildirme

Düzen **AndroidManifest.xml** ve aşağıdakileri ekleyin `<receiver>` elementini `<application>` bölümü:

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

Bu XML şunları yapar:

-   Bildiren bir `FirebaseInstanceIdReceiver` sağlayan uygulama bir [benzersiz tanımlayıcı](https://developers.google.com/instance-id/) her uygulama örneği. Bu alıcı, ayrıca doğrular ve Eylemler yetkisi verir.

-   Bir iç bildirir `FirebaseInstanceIdInternalReceiver` hizmetlerini güvenli bir şekilde başlatmak için kullanılan uygulama.

-   [Uygulama kimliği](./firebase-cloud-messaging.md#fcm-in-action-app-id) depolanan **google-services.json** dosya [projeye eklenen](#add-googleplayservices-json). Xamarin.Android Firebase bağlamaları belirtecin yerini alacak `${applicationId}` uygulama Kimliğine sahip; uygulama kimliği sağlamak için istemci uygulama tarafından hiçbir ek kod gereklidir

`FirebaseInstanceIdReceiver` Olduğu bir `WakefulBroadcastReceiver` alımların `FirebaseInstanceId` ve `FirebaseMessaging` olayları ve öğesinden türetilen sınıf teslim `FirebaseInstanceIdService`.

### <a name="implement-the-firebase-instance-id-service"></a>Firebase örnek kimlik hizmetinin uygulayın

Uygulama FCM ile kaydetme işlemlerini özel değere göre işlenir `FirebaseInstanceIdService` sağlayan hizmet.
`FirebaseInstanceIdService` Aşağıdaki adımları gerçekleştirir:

1.  Kullanan [örnek kimliği API'sini](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) FCM ve uygulama sunucusuna erişmek için istemci uygulamasını Yetkilendir güvenlik belirteçleri oluşturmak için. Buna karşılık, uygulamayı geri alır bir [kayıt belirtecinizi](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token) FCM öğesinden.

2.  Uygulama sunucusu gerektiriyorsa, kayıt belirtecinizi uygulama sunucusuna iletir.

Adlı yeni bir dosya ekleme **Myfirebaseııdservice.cs** ve şablon kodunu şununla değiştirin:

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

Bu hizmet uygulayan bir `OnTokenRefresh` kayıt belirtecinizi ilk kez oluşturulduğunda veya değiştirildiğinde, çağrılan yöntem. Zaman `OnTokenRefresh` çalıştığında, en son belirtecinden alır `FirebaseInstanceId.Instance.Token` (FCM ile zaman uyumsuz olarak güncelleştirilir) özelliği. Bu örnekte, yenilenmiş bir belirteç kaydedilir, böylece çıkış penceresinde görüntülenebilir:

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` nadiren çağrılır: aşağıdaki koşullarda belirtecini güncelleştirmek için kullanılır:

-   Ne zaman uygulama yüklenmeden veya kaldırılmadan.

-   Ne zaman kullanıcıyı uygulama verilerini siler.

-   Ne zaman uygulama örnek kimliği siler.

-   Belirteç güvenliğini tehlikeye girdiği ne zaman.

Google'nın göre [örnek kimliği](https://developers.google.com/instance-id/guides/android-implementation) , belgeler, FCM örnek kimliği hizmet isteği uygulama, belirteci düzenli aralıklarla (genellikle, 6 ayda bir) yenileyin.

`OnTokenRefresh` Ayrıca çağırır `SendRegistrationToAppServer` kullanıcının kayıt ilişkilendirmek için sunucu tarafı hesabı (varsa) ile belirteç, uygulama tarafından korunur:

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Bu uygulama uygulama sunucusunun tasarımına bağlıdır çünkü bu örnekte, bir boş yöntem gövdesinde sağlanır. Uygulama sunucunuza FCM kayıt bilgileri gerektiriyorsa, değişiklik `SendRegistrationToAppServer` kullanıcının FCM örneği kimlik belirteci, uygulamanız tarafından korunan herhangi bir sunucu tarafı hesabı ile ilişkilendirmek için. (Belirteç istemci uygulamaları için donuktur unutmayın.)

Uygulama sunucusu için bir belirteç gönderildiğinde `SendRegistrationToAppServer` belirteç sunucuya gönderilip gönderilmediğini belirten bir Boole değeri korumanız gerekir. Bu boolean false ise `SendRegistrationToAppServer` belirteci uygulama sunucusuna gönderir &ndash; Aksi takdirde, belirteç zaten önceki çağrıda uygulama sunucusuna gönderildi. Bazı durumlarda (Bunun gibi `FCMClient` örnek), uygulama sunucusu belirteç gerekmez; bu nedenle, bu yöntem bu örnek için gerekli değildir.

## <a name="implement-client-app-code"></a>İstemci uygulaması kodu uygulama

Alıcı Hizmetleri yerinde olduğuna göre istemci uygulama kodu bu hizmetlerden faydalanmak için yazılabilir. Aşağıdaki bölümlerde, kayıt belirtecinizi oturum için kullanıcı Arabirimi için bir düğme eklenir (olarak da adlandırılan *örneği kimlik belirteci*), ve daha fazla kod eklendiğinden `MainActivity` görüntülemek için `Intent` uygulama başlatıldığı zaman bilgi bir bildirim:

[![Uygulama ekrana eklenen günlüğü belirteci düğmesi](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokes"></a>Günlük tokes

Bu adımda eklediğiniz kodun yalnızca gösterim amaçlıdır &ndash; üretim istemci uygulaması kayıt belirteçleri oturum gerek yoktur. Düzen **Resources/layout/Main.axml** ve aşağıdakileri ekleyin `Button` bildirimi hemen sonra `TextView` öğesi:

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Sonuna aşağıdaki kodu ekleyin `MainActivity.OnCreate` yöntemi:

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Bu kod, çıkış penceresinde geçerli belirteç oturum zaman **Oturum belirteci** düğmeye dokunulduğunda.

### <a name="handle-notification-intents"></a>Tanıtıcı bildirim hedefleri

Kullanıcı tarafından verilen bir bildirim zaman dokunduğunda **FCMClient**, bu bildirim eşlik eden herhangi bir veri ileti kullanılabilir yapıldığında `Intent` ek özellikler. Düzen **MainActivity.cs** ve üstüne aşağıdaki kodu ekleyin `OnCreate` yöntemi (çağırmadan önce `IsPlayServicesAvailable`):

```csharp
if (Intent.Extras != null)
{
    foreach (var key in Intent.Extras.KeySet())
    {
        var value = Intent.Extras.GetString(key);
        Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
    }
}
```

Uygulama Başlatıcısı `Intent` kullanıcı bu kodu eşlik eden herhangi bir veri oturum açacak şekilde, bildirim iletisini dokunduğunda tetiklenir `Intent` çıkış penceresine. Farklı bir `Intent` harekete gerekir, `click_action` alanı bildirim iletisi için ayarlanmalıdır `Intent` (Başlatıcısı `Intent` durumlarda kullanılan `click_action` belirtilir).


## <a name="background-notifications"></a>Arka plan bildirimleri

Derleme ve çalıştırma **FCMClient** uygulama. **Oturum belirteci** düğmesi görüntülenir:

[![Oturum belirteci düğmesi görüntülenir](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

Dokunun **Oturum belirteci** düğmesi. IDE çıkış penceresinde aşağıdakine benzer bir ileti görüntülenir:

[![Çıktı penceresinde görüntülenen örnek kimlik belirteci](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

Uzun bir dize ile etiketlenmiş **belirteci** Firebase konsolunda yapıştıracağınız örneği kimliği belirteç &ndash; seçin ve bu dizesini panoya kopyalayın. Bir örnek kimliği belirteci görmüyorsanız en üstüne aşağıdaki satırı ekleyin `OnCreate` doğrulamak için yöntem **google-services.json** doğru şekilde ayrıştırıldı:

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

`google_app_id` Çıkış penceresine kaydedilip değer eşleşmelidir `mobilesdk_app_id` kayıtlı değer **google-services.json**.

### <a name="send-a-message"></a>Bir ileti gönderin

Oturum [Firebase konsolunda](https://console.firebase.google.com), projenizi seçin, **bildirimleri**, tıklatıp **ilk ileti gönder**:

[![Gönder düğmesine bilgisayarınızı ilk ileti](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

Üzerinde **oluşturma iletisi** sayfasında ileti metni girin ve seçin **tek cihaz**. IDE çıktı penceresinden örneği kimlik belirteci kopyalayın ve yapıştırın **FCM kayıt belirtecinizi** Firebase konsolunda alanı:

[![Bir iletişim kutusu oluşturma](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Android cihaz (veya öykünücü üzerinde), Android dokunarak uygulamayı arka plan **genel bakış** düğmesi ve giriş ekranına temas. Cihaz hazır olduğunda tıklayın **iletisi gönder** Firebase konsolunda:

[![Gönder düğmesine iletisi](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

Zaman **gözden geçirme ileti** iletişim kutusu görüntülenir, tıklayın **Gönder**.
Cihaz (veya öykünücü) bildirim alanında bildirim simgesine görünmelidir:

[![Bildirim simgesi gösterilir](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

İletiyi görüntülemek için bildirim simgesine açın. Bildirim iletisi, tam olarak ne içine yazılmış olmalıdır **ileti metni** Firebase konsolunda alanı:

[![Cihazda bildirim iletisi görüntülenir](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

Başlatmak için bildirim simgesine dokunun **FCMClient** uygulama. `Intent` Gönderilen ek özellikler **FCMClient** IDE çıkış penceresinde listelenir:

[![Anahtar, ileti kimliği ve anahtarı Daralt hedefi ek özellikler listesi](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

Bu örnekte, **gelen** anahtarını uygulamasını Firebase projesi sayısını ayarlayın (Bu örnekte, `41590732`) ve **collapse_key** paket adını ayarlayın ( **com.xamarin.fcmexample**).
Bir ileti almazsanız silmeyi deneyin **FCMClient** cihaz (veya öykünücü) şirket uygulama ve yukarıdaki adımları yineleyin.


> [!NOTE]
> Zorla-close uygulama FCM bildirimleri teslim durdurur. Android, arka plan hizmeti yayınları yanlışlıkla veya gereksiz yere durdurulan uygulama bileşenlerinin başlatmasını engeller. (Bu davranışı hakkında daha fazla bilgi için bkz: [başlatma denetimleri durdurulmuş uygulamaları](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) Bu nedenle, gerekli uygulama her zaman el ile kaldırmak için çalıştırın ve bir hata ayıklama oturumundan durdurmak &ndash; FCM iletileri alınabilmesi devam edebilmesi için yeni bir belirteç oluşturmak için bu zorlar.

### <a name="add-a-custom-default-notification-icon"></a>Özel varsayılan bildirim simgesi Ekle

Önceki örnekte, uygulama simgesine bildirim simgesine ayarlanır. Aşağıdaki XML özel varsayılan bir simge bildirimleri için yapılandırır. Android bildirim simgesine değil açıkça ayarlandığı tüm bildirim iletileri için bu özel varsayılan simge görüntüler.

Özel varsayılan bir bildirim simge eklemek için simge eklemek **kaynakları/drawable** dizini Düzenle **AndroidManifest.xml**ve aşağıdakileri ekleyin `<meta-data>` öğesine`<application>`bölümü:

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

Bu örnekte, bildirim simgesine yer adresindeki **kaynakları/drawable/IC\_stat\_IC\_notification.png** özel varsayılan bildirim simgesi olarak kullanılacak. İçinde özel varsayılan bir simge yapılandırılmadıysa **AndroidManifest.xml** ve herhangi bir simge bildirim yükteki ayarlanır, Android (bildirim simgesi yukarıdaki ekran görüntüsünde görüldüğü gibi) bu uygulama simgesi bildirim simgesine kullanır.

## <a name="handle-topic-messages"></a>Konu iletileri işleme

Şimdiye kadarki yazılan kod kayıt belirteçlerini işler ve uygulamaya uzaktan bildirim işlevsellik ekler. Sonraki örnekte dinleyen kod ekler *konusu iletilerinde* ve bunları olarak uzak bildirimler kullanıcıya iletir. Konusu iletilerinde belirli bir konuya abone olma bir veya daha fazla cihazlara gönderilen FCM iletileri ' dir. Konu iletileri hakkında daha fazla bilgi için bkz. [konu Mesajlaşma](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

### <a name="subscribe-to-a-topic"></a>Bir konuya abone olma

Düzen **Resources/layout/Main.axml** ve aşağıdakileri ekleyin `Button` bildiriminden hemen sonra önceki `Button` öğesi:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

Bu XML ekler bir **bildirime nasıl abone** düzene düğmesi.
Düzen **MainActivity.cs** ve sonuna aşağıdaki kodu ekleyin `OnCreate` yöntemi:

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Bu kod bulur **bildirime nasıl abone** düzenini düğmesi ve çağıran kod, tıklama işleyicisi atar `FirebaseMessaging.Instance.SubscribeToTopic`, abone olunan konudaki geçen _haber_. Kullanıcı ne zaman dokunduğunda **abone ol** düğmesi uygulamayı aboneliği _haber_ konu. Aşağıdaki bölümde, bir _haber_ Firebase Konsolu bildirimleri GUI konu ileti gönderilir.

### <a name="send-a-topic-message"></a>Bir konu ileti gönderin

Uygulamayı kaldırma, yeniden oluşturmak ve yeniden çalıştırın. Tıklayın **bildirimlerine abone olma** düğmesi:

[![Bildirimleri düğmesi için abone olun](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

Uygulama başarıyla abone, görmelisiniz **konu eşitleme başarılı** IDE'de çıkış penceresi:

[![Çıkış penceresi konu eşitleme başarılı iletisi gösterir.](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

Konusuna ileti göndermek için aşağıdaki adımları kullanın:

1.  Firebase konsolunda **yeni ileti**.

2.  Üzerinde **oluşturma iletisi** sayfasında ileti metni girin ve seçin **konu**.

3.  İçinde **konu** aşağı açılır menüsünde, yerleşik bir konu seçmek **haber**:

    [![Haber konu seçme](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Android cihaz (veya öykünücü üzerinde), Android dokunarak uygulamayı arka plan **genel bakış** düğmesi ve giriş ekranına temas.

5.  Cihaz hazır olduğunda tıklayın **iletisi gönder** Firebase konsolunda.

6.  Görmek için IDE çıktı penceresini denetleyin **/konuları/haber** günlük çıktı:

    [![/Topic/news ileti gösterilir](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

Bu ileti çıkış penceresinde ortaya çıktığında, bildirim simgesine da Android cihazda bildirim alanında görüntülenmelidir. Konu iletiyi görüntülemek için bildirim simgesine açın:

[![Konu ileti bildirim olarak görünür.](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

Bir ileti almazsanız silmeyi deneyin **FCMClient** cihaz (veya öykünücü) şirket uygulama ve yukarıdaki adımları yineleyin.

## <a name="foreground-notifications"></a>Ön plan bildirimleri

Foregrounded uygulamalarında bildirimleri almak için uygulamanız gereken `FirebaseMessagingService`. Bu hizmet ayrıca, veri yüklerini almak ve Yukarı Akış ileti göndermek için gereklidir. Aşağıdaki örnekler genişleten bir hizmeti uygulamak nasıl çalışılacağını `FirebaseMessagingService` &ndash; edilen uygulama ön planda çalışırken uzak bildirimler işlemek mümkün olacaktır.

### <a name="implement-firebasemessagingservice"></a>FirebaseMessagingService uygulayın

`FirebaseMessagingService` Hizmeti, alma ve Firebase gelen iletileri işlemek için sorumludur. Her uygulama bu tür ve geçersiz kılma öğesinin alt sınıfı olmalıdır `OnMessageReceived` gelen iletileri işlemek için. Uygulama ön planda olduğunda `OnMessageReceived` geri çağırma her zaman ileti işleme.

> [!NOTE]
> Uygulamalar, 10 saniye içinde bir gelen Firebase Cloud iletisini işlemek yeterlidir. Bu kitaplığı kullanarak arka plan yürütme için zamanlanmalıdır uzun süren herhangi bir iş [Android İş Zamanlayıcısı](~/android/platform/android-job-scheduler.md) veya [Firebase iş dağıtıcı](~/android/platform/firebase-job-dispatcher.md).

Adlı yeni bir dosya ekleme **MyFirebaseMessagingService.cs** ve şablon kodunu şununla değiştirin:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

Unutmayın `MESSAGING_EVENT` hedefi filtre bildirilmesi gerekir böylece yeni FCM iletileri yönlendirilmiş `MyFirebaseMessagingService`:

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

İstemci uygulaması FCM bir ileti aldığında `OnMessageReceived` geçirilen bileşeninden ileti içeriği ayıklar `RemoteMessage` çağırarak kendi `GetNotification` yöntemi. Ardından, böylece IDE çıkış penceresinde görüntülenen ileti içeriğini kaydeder:

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> Kesme noktası ayarlarsanız `FirebaseMessagingService`, hata ayıklama oturumu olabilir veya bu kesme noktalarına FCM iletileri nasıl teslim nedeniyle karşılaşabilirsiniz değil.


### <a name="send-another-message"></a>Başka bir ileti gönderin

Uygulamayı kaldırma, yeniden oluşturmak, yeniden çalıştırın ve başka bir ileti göndermek için şu adımları izleyin:

1.  Firebase konsolunda **yeni ileti**.

2.  Üzerinde **oluşturma iletisi** sayfasında ileti metni girin ve seçin **tek cihaz**.

3.  IDE çıktı penceresinden belirteç dizesini kopyalayın ve yapıştırın **FCM kayıt belirtecinizi** Firebase konsolunda önceki gibi alanı.

4.  Uygulama ön planda çalıştığından emin olun ve ardından tıklayın **iletisi gönder** Firebase konsolunda:

    [![Konsoldan başka bir ileti gönderme](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  Zaman **gözden geçirme ileti** iletişim kutusu görüntülenir, tıklayın **Gönder**.

6.  Gelen ileti, IDE çıkış penceresinde kaydedilir:

    [![İleti gövdesi çıkış penceresinde gösterilip](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notification-sender"></a>Bir yerel bildirim gönderen Ekle

Kalan Bu örnekte, uygulama ön planda çalışırken başlatılan bir yerel bildirim gelen FCM ileti dönüştürülür. Düzen **MyFirebaseMessageService.cs** ve aşağıdakileri ekleyin `using` ifadeleri:

```csharp
using FCMClient;
using System.Collections.Generic;
```

Aşağıdaki yöntemi ekleyin `MyFirebaseMessagingService`:

<a name="sendnotification-method"></a>
```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (var key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }

    var pendingIntent = PendingIntent.GetActivity(this,
                                                  MainActivity.NOTIFICATION_ID,
                                                  intent,
                                                  PendingIntentFlags.OneShot);

    var notificationBuilder = new  NotificationCompat.Builder(this, MainActivity.CHANNEL_ID)
                              .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                              .SetContentTitle("FCM Message")
                              .SetContentText(messageBody)
                              .SetAutoCancel(true)
                              .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(MainActivity.NOTIFICATION_ID, notificationBuilder.Build());
}
```

Bu bildirim arka plan bildirimler ayırt etmek için bildirimleri uygulama simgesinden farklı bir simge ile bu kodu işaretler. Dosya ekleme [IC\_stat\_IC\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) için **kaynakları/drawable** ve olmasını **FCMClient** proje .

`SendNotification` Yöntemi kullanan ` NotificationCompat.Builder` bildirim oluşturmak için ve `NotificationManagerCompat` bildirim başlatmak için kullanılır. Bildirim tutan bir `PendingIntent` eklemenize olanak tanıyan uygulamasını açın ve yöntemlere geçirilen dize içeriğini görüntülemek kullanıcı `messageBody`. Hakkında daha fazla bilgi için `NotificationCompat.Builder`, bkz: [yerel bildirimleri](~/android/app-fundamentals/notifications/local-notifications.md).

Çağrı `SendNotification` sonunda yöntemi `OnMessageReceived` yöntemi:

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

Bu değişiklik sonucunda `SendNotification` uygulama ön planda ve bildirim alanında bildirim görünür bir bildirim alındığında çalışacak.

Bir uygulama arka planda gerektiğinde [iletinin yükü](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) ileti nasıl işlendiğini belirler:

* **Bildirim** &ndash; iletileri gönderilir **sistem tepsisi**. Yerel bir bildirim Burada görünecek. Kullanıcı bildirime dokunduğunda, uygulamayı başlatır.
* **Veri** &ndash; iletileri tarafından işleneceğini `OnMessageReceived`.
* **Her ikisi de** &ndash; hem bildirim hem de veri yükü olan iletiler için sistem tepsisindeki gönderilir. Uygulamayı başlattığında veri yükü görünür `Extras` , `Intent` uygulamasını başlatmak için kullanıldı.

Bu örnekte, uygulama backgrounded, `SendNotification` ileti veri yükü varsa çalışır. Aksi takdirde, (Bu kılavuzda daha önce gösterilen) bir arka plan bildirim başlatılır.

### <a name="send-the-last-message"></a>Son ileti gönder

Uygulamayı kaldırma, yeniden oluşturmak, yeniden çalıştırın ve ardından son ileti göndermek için aşağıdaki adımları kullanın:

1.  Firebase konsolunda **yeni ileti**.

2.  Üzerinde **oluşturma iletisi** sayfasında ileti metni girin ve seçin **tek cihaz**.

3.  IDE çıktı penceresinden belirteç dizesini kopyalayın ve yapıştırın **FCM kayıt belirtecinizi** Firebase konsolunda önceki gibi alanı.

4.  Uygulama ön planda çalıştığından emin olun ve ardından tıklayın **iletisi gönder** Firebase konsolunda:

    [![Ön plan ileti gönderme](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

Bu kez, çıktı penceresinde günlüğe kaydedilen iletinin yeni bir bildirim de paketlenmiş &ndash; uygulama ön planda çalışırken bildirim simgesine bildirim tepsisinde görünür:

[![Ön plan ileti için bildirim simgesi](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

Bildirim açtığınızda, Firebase Konsolu bildirimleri GUI gönderilen son ileti görmeniz gerekir:

[![Ön plan simgesiyle gösterilen ön bildirimi](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>FCM bağlantısı kesiliyor

Bir konu aboneliği kaldırmak için çağrı [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) metodunda [FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) sınıfı. Örneğin, aboneliğinizi iptal etmek _haber_ konusuna abone daha önce bir **Unsubscribe** düğme, aşağıdaki işleyici kodu düzeniyle eklenebiliyordu:

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

CİHAZDAN FCM özelliği tamamen kaldırmak için örnek kimliği çağırarak Sil [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) metodunda [Firebaseınstanceıd](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) sınıfı. Örneğin:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

Bu yöntem çağrısının siler örnek kimliği ve ilişkili veriler. Sonuç olarak, cihaza FCM veri düzenli gönderme durdurulur.


## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki sorunların ve Firebase Cloud Messaging ile Xamarin.Android kullanırken oluşabilecek geçici çözümler.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp başlatılmadı

Bazı durumlarda, bu hata iletisini görebilirsiniz:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Bu çözüm temizleme ve projeyi yeniden oluşturmayı geçici olarak çalışabilen, bilinen bir sorundur (**Yapı > çözümü Temizle**, **Yapı > çözümü yeniden derle**). Daha fazla bilgi için bkz. Bu [forum tartışmasına](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process).


## <a name="summary"></a>Özet

Bu kılavuzda bir Xamarin.Android uygulamasına Firebase Cloud Messaging uzak bildirimler uygulamaya yönelik adımlar ayrıntılı olarak. FCM iletişimleri için gereken gerekli paketleri yüklemek nasıl açıklanan ve Android bildirim FCM sunuculara erişim için yapılandırma konusunda açıklanmıştır. Bu, Google Play Hizmetleri varlığını denetlemek nasıl gösterir örnek kod sağlanır. Nasıl FCM ile bir kayıt belirteci için belirleyici bir örnek kimliği dinleme hizmeti uygulanacağı gösterilmiştir ve uygulama backgrounded sırada bu kod arka plan bildirimleri nasıl oluşturduğunu açıklanmıştır. Örnek uygulama almak ve uzak bildirimler uygulama ön planda çalışırken görüntülemek için kullanılan bir ileti dinleme hizmeti için sağlanan ve konu iletilerine abone olunacağı açıklanmaktadır.


## <a name="related-links"></a>İlgili bağlantılar

- [FCMNotifications (örnek)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [FCM iletileri hakkında](https://firebase.google.com/docs/cloud-messaging/concept-options)
