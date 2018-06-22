---
title: İleti Firebase ile uzaktan bildirimleri bulut
description: Bu kılavuz, bir Xamarin.Android uygulaması Firebase Cloud Messaging (anında iletme bildirimleri olarak da bilinir) uzaktan bildirimleri uygulamak için nasıl kullanılacağını hakkında adım adım bir açıklama sağlar. Firebase bulut Mesajlaşma (FCM) ile iletişim için gereken, Android bildirim FCM erişimi için yapılandırma örnekleri sağlar ve aşağı akış Firebase kullanarak Mesajlaşma gösterir çeşitli sınıfları uygulamak nasıl gösterir Konsolu.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: e2f25504b971a0332dc51dc9b017c9c83222ec57
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044944"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>İleti Firebase ile uzaktan bildirimleri bulut

_Bu kılavuz, bir Xamarin.Android uygulaması Firebase Cloud Messaging (anında iletme bildirimleri olarak da bilinir) uzaktan bildirimleri uygulamak için nasıl kullanılacağını hakkında adım adım bir açıklama sağlar. Firebase bulut Mesajlaşma (FCM) ile iletişim için gereken, Android bildirim FCM erişimi için yapılandırma örnekleri sağlar ve aşağı akış Firebase kullanarak Mesajlaşma gösterir çeşitli sınıfları uygulamak nasıl gösterir Konsolu._

## <a name="fcm-notifications-overview"></a>FCM bildirimleri genel bakış

Bu kılavuzda, temel bir uygulama olarak adlandırılan **FCMClient** FCM Mesajlaşma essentials göstermek için oluşturulur. **FCMClient** Google Play Hizmetleri'nin varlığını denetler, FCM kayıt belirteçleri alır, Firebase konsolundan gönderdiğiniz uzak bildirimler görüntüler ve konu iletileri abone olur:

[![Örnek uygulamasının ekran görüntüsü](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

Aşağıdaki konular incelenecek:

1.  Arka plan bildirimleri

2.  Konu iletileri

3.  Ön plan bildirimleri

Bu gözden geçirme sırasında işlevselliği için artımlı olarak ekleyeceksiniz **FCMClient** çalıştırmak bir cihaz veya öykünücü FCM ile nasıl etkileşim kurduğu anlamak için. Dinamik uygulama işlemleri FCM sunucularıyla tanığı için günlüğü kullanır ve bildirimleri Firebase konsol bildirimleri GUI girin FCM iletilerden nasıl oluşturulduğu uyun. 

## <a name="requirements"></a>Gereksinimler

Bu kılavuzda ile devam etmeden önce Google FCM sunucuları kullanmak için gerekli kimlik bilgilerini edinmeniz gerekir; Bu işlem açıklaması [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). Özellikle, yüklemelidir **google services.json** bu kılavuzda sunulan örnek kod ile kullanmak üzere bir dosya. Henüz bir proje Firebase konsolunda oluşturduğunuz değil, (veya henüz indirilmemiştir **google services.json** dosyası), bkz: [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Örnek uygulamayı çalıştırmak için bir Android test cihaz veya Firebase ile uyumlu olan öykünücü gerekir. Firebase Cloud Messaging'i destekleyen Android 2.3 (Zencefilli kurabiye) veya üzeri çalıştıran istemciler ve bu aygıtların ayrıca olmalıdır yüklü Google Play Store uygulaması (Google Play Hizmetleri 9.2.1 veya üzeri gereklidir). Google Play mağazası uygulamasının aygıtınızda yüklü henüz yoksa ziyaret [Google Play](https://support.google.com/googleplay) indirmek ve yüklemek için web sitesi. Alternatif olarak, Android 2.3 veya üstünü (, Android SDK öykünücüsü kullanıyorsanız Google Play Store yüklemeniz gerekmez) bir test cihazı yerine çalıştıran Android SDK öykünücüsü kullanabilirsiniz. 

## <a name="start-an-app-project"></a>Bir uygulama projesi Başlat

Başlamak için adlı yeni bir boş Xamarin.Android projesi oluşturma **FCMClient**. Xamarin.Android projeleri oluşturma ile bilmiyorsanız bkz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md). Yeni uygulama oluşturulduktan sonra sonraki adıma paket adını ayarlayın ve FCM ile iletişim için kullanılan birkaç NuGet paketlerini yüklemektir. 

### <a name="set-the-package-name"></a>Paket adı ayarlama

İçinde [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), FCM özellikli uygulama için bir paket adı belirtildi. Bu paket adı da görevi gören *uygulama kimliği* API anahtarıyla ilişkili. Bu paket adını kullanmak üzere uygulamayı yapılandırır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Özelliklerini açın **FCMClient** projesi. 

2.  İçinde **Android derleme bildirimi** sayfasında, paket adı ayarlayın. 

Aşağıdaki örnekte, paket adı ayarlamak `com.xamarin.fcmexample`: 

[![Paket adı ayarlama](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

Güncelleştirdiğiniz sırada **Android derleme bildirimi**, emin olmak için de onay `Internet` izin etkinleştirilir. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Özelliklerini açın **FCMClient** projesi. 

2.  İçinde **Android uygulaması** sayfasında, paket adı ayarlayın. 

Aşağıdaki örnekte, paket adı ayarlamak `com.xamarin.fcmexample`: 

[![Paket adı ayarlama](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

Güncelleştirdiğiniz sırada **Android derleme bildirimi**, emin olmak için de onay `INTERNET` izni etkinse (altında **gerekli izinleri**). 

-----

İstemci uygulaması bu paket adı yoksa FCM kayıt belirtecinizi alamıyor olacağını unutmayın *tam olarak* Firebase konsola girilen paket adı ile eşleşmesi. 

### <a name="add-the-xamarin-google-play-services-base-package"></a>Xamarin Google Play Hizmetleri temel Paketi Ekle

Google Play hizmetleri üzerinde Firebase Cloud Messaging bağımlı olduğundan dolayı [Xamarin Google Play Hizmetleri - Base](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) NuGet paketi Xamarin.Android projeye eklenmelidir. Sürüm 29.0.0.2 gerekir ya da daha sonra.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio'da sağ **başvuruları > NuGet paketlerini Yönet...** . 

2.  Tıklatın **Gözat** sekmesinde ve arama **Xamarin.GooglePlayServices.Base**. 

3.  Bu pakete yükleyin **FCMClient** proje: 

    [![Google Play Hizmetleri Bankası yükleme](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Mac için Visual Studio'da sağ **paketleri > paketleri Ekle...** . 

2.  Arama **Xamarin.GooglePlayServices.Base**. 

3.  Bu pakete yükleyin **FCMClient** proje: 

    [![Google Play Hizmetleri Bankası yükleme](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

NuGet yüklenmesi sırasında bir hata alırsanız, kapatmak **FCMClient** proje, yeniden açın ve NuGet yükleme işlemini yeniden deneyin. 

Yüklediğinizde **Xamarin.GooglePlayServices.Base**, tüm gerekli bağımlılıkları da yüklenir. Düzen **MainActivity.cs** ve aşağıdakileri ekleyin `using` deyimi: 

```csharp
using Android.Gms.Common;
```

Bu bildirimi yapar `GoogleApiAvailability` sınıfını **Xamarin.GooglePlayServices.Base** kullanımına **FCMClient** kodu. 
`GoogleApiAvailability` Google Play Hizmetleri'nin olup olmadığını denetlemek için kullanılır. 

### <a name="add-the-xamarin-firebase-messaging-package"></a>Xamarin paket Mesajlaşma Firebase Ekle

FCM iletileri almak için [Xamarin Firebase - ileti](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) uygulama projesi için NuGet paketi eklenir. Bu paketi bir Android uygulamasını FCM sunucularından mesajlarını alamıyor.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio'da sağ **başvuruları > NuGet paketlerini Yönet...** . 

2.  Denetleme **dahil et** arayın ve **Xamarin.Firebase.Messaging**. 

3.  Bu pakete yükleyin **FCMClient** proje: 

    [![Xamarin Mesajlaşma Firebase yükleme](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Mac için Visual Studio'da sağ **paketleri > paketleri Ekle...** . 

2.  Denetleme **Göster ön sürüm paketlerini** arayın ve **Xamarin.Firebase.Messaging**. 

3.  Bu pakete yükleyin **FCMClient** proje: 

    [![Xamarin Mesajlaşma Firebase yükleme](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----
 
Yüklediğinizde **Xamarin.Firebase.Messaging**, tüm gerekli bağımlılıkları da yüklenir.

Ardından, düzenleme **MainActivity.cs** ve aşağıdakileri ekleyin `using` deyimleri:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

İlk iki deyimleri türlerinde olun **Xamarin.Firebase.Messaging** NuGet paketi kullanılabilir **FCMClient** kodu. **Android.Util** FMS işlemleri izlemek için kullanılan günlük işlevsellik ekler. 


### <a name="add-the-google-services-json-file"></a>Google Hizmetleri JSON dosyası ekleme

Sonraki adım eklemektir **google services.json** projenizin kök dizinine dosyası: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Kopya **google services.json** proje klasörüne.

2.  Ekleme **google services.json** uygulama projesine (tıklatın **tüm dosyaları göster** içinde **Çözüm Gezgini**, sağ tıklatın **google-services.json**seçeneğini belirleyip **Proje Ekle**). 

3.  Seçin **google services.json** içinde **Çözüm Gezgini** penceresi.

4.  İçinde **özellikleri** kümesi bölmesi, **yapı eylemi** için **GoogleServicesJson** (varsa **GoogleServicesJson** yapı eylemi gösterilmez, kaydedin ve çözümü kapatın, sonra yeniden):

    [![Derleme eylemi GoogleServicesJson için ayarlama](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Kopya **google services.json** proje klasörüne.

2.  Ekleme **google services.json** uygulama projesi için. 

3.  Sağ **google services.json**.

4.  Ayarlama **yapı eylemi** için **GoogleServicesJson**: 

    [![Derleme eylemi GoogleServicesJson için ayarlama](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)
 
-----
 

Zaman **google services.json** projeye eklendi (ve **GoogleServicesJson** yapı eylemi ayarlanır), oluşturma işleminin istemci kimliği ve API anahtarını ayıklar ve bu kimlik bilgileri birleştirilmiş ekler / oluşturulan **AndroidManifest.xml** konumunda bulunan **obj/Debug/android/AndroidManifest.xml**. Bu birleştirme işlemi, tüm izinleri ve FCM sunucu bağlantısı için gereken diğer FCM öğeleri otomatik olarak ekler. 


## <a name="check-for-google-play-services"></a>Google Play hizmetlerini denetle

Uygulamanın kullanıcı Arabirimi için bir başlangıç düzeni ilk oluşturulur. Düzen **Resources/layout/Main.axml** ve içeriğini aşağıdaki XML ile değiştirin: 

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

Bu `TextView` , Google Play Hizmetleri'nin yüklü olup olmadığını belirten iletileri görüntülemek için kullanılır. Değişiklikleri kaydetmek **Main.axml**. Düzen **MainActivity.cs** ve aşağıdaki örnek değişken bildirimi ekleyin `MainActivity` sınıfı: 

```csharp
TextView msgText;
```

Google önerir Android uygulamalarını Google Play Hizmetleri özellikleri erişmeden önce Google Play Hizmetleri APK varlığını denetle (daha fazla bilgi için bkz: [denetlemek için Google Play hizmetlerini](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)). Aşağıdaki örnekte, `OnCreate` yöntemi, FCM Hizmetleri'ni kullanmak üzere uygulamayı denemeden önce Google Play Hizmetleri'nin kullanılabilir durumda olmadığını doğrulayın. Aşağıdaki yöntemi ekleyin `MainActivity` sınıfı: 

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

Bu kod, Google Play Hizmetleri APK yüklü olup olmadığını görmek için aygıt denetler. Bu yüklü değilse, bir ileti görüntülenir `TextBox` bir APK Google Play mağazasından indirin (veya aygıtın sistem ayarlarını etkinleştir) kullanıcıya bildirir. 

Değiştir `OnCreate` aşağıdaki kod ile yöntemi: 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);
    IsPlayServicesAvailable ();
}
```

`IsPlayServicesAvailable` sonunda çağrılan `OnCreate` böylece Google Play Hizmetleri'nin çalıştığı her zaman uygulama başlatıldığında denetleyin. Uygulamanız varsa, bir `OnResume` yöntemi, çağrısı `IsPlayServicesAvailable` gelen `OnResume` de. Tamamen yeniden oluşturun ve uygulamayı çalıştırın. Tüm yapılandırılırsa düzgün bir şekilde, aşağıdaki ekran görüntüsü gibi görünen bir ekran görmeniz gerekir: 

[![Google Play Hizmetleri'nin kullanılabilir olduğunu gösterir](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

Bu sonuç alamazsanız Google Play Hizmetleri APK aygıtınızda yüklü olduğunu doğrulayın (daha fazla bilgi için bkz: [ayarı yukarı Google Play Hizmetleri](https://developers.google.com/android/guides/setup)). Ayrıca, eklediğiniz doğrulayın **Xamarin.Google.Play.Services.Base** paketini, **FCMClient** proje daha önce açıklandığı gibi.


## <a name="add-the-instance-id-receiver"></a>Örnek kimliği alıcı ekleyin

Genişleten bir hizmet eklemek için sonraki adımdır `FirebaseInstanceIdService` oluşturma, döndürme, işlenecek ve Firebase kayıt belirteçleri güncelleştiriliyor. (Kayıt belirteçleri hakkında daha fazla bilgi için bkz: [FCM kaydı](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).) `FirebaseInstanceIdService` FCM cihaza iletileri gönderebilmesi için hizmet gereklidir. Zaman `FirebaseInstanceIdService` hizmeti uygulama otomatik olarak FCM ileti alma ve uygulamayı backgrounded her durumda bunları bildirimleri olarak görüntülemek istemci uygulamasına ekledi. 

### <a name="declare-the-receiver-in-the-android-manifest"></a>Android bildirimindeki alıcı bildirme

Düzen **AndroidManifest.xml** ve aşağıdaki Ekle `<receiver>` elemanlara `<application>` bölümü: 

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

-   Bildiren bir `FirebaseInstanceIdReceiver` sağlayan uygulama bir [benzersiz tanımlayıcı](https://developers.google.com/instance-id/) her uygulama örneği. Bu alıcı, ayrıca kimliğini doğrular ve Eylemler yetkilendirir. 

-   Bir iç bildirir `FirebaseInstanceIdInternalReceiver` hizmetler güvenli bir şekilde başlatmak için kullanılan uygulama.

`FirebaseInstanceIdReceiver` Olan bir `WakefulBroadcastReceiver` , alan `FirebaseInstanceId` ve `FirebaseMessaging` olayları ve bunları öğesinden türetilen sınıfı için teslim `FirebaseInstanceIdService`. 

### <a name="implement-the-firebase-instance-id-service"></a>Firebase örnek kimliği hizmet ekleme

Uygulama ile FCM kaydetme işlemlerini özel değere göre işlenir `FirebaseInstanceIdService` sağlayan hizmet. 
`FirebaseInstanceIdService` Aşağıdaki adımları gerçekleştirir: 

1.  Kullanan [örnek kimliği API'sini](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) FCM ve uygulama sunucusuna erişmek için istemci uygulamasını yetkilendirmek güvenlik belirteçleri oluşturmak için. Buna karşılık, uygulama geri bir kayıt FCM belirtecini alır. 

2.  Uygulama sunucusu gerektiriyorsa, kayıt belirtecinizi uygulama sunucusuna iletir.

Adlı yeni bir dosya ekleme **MyFirebaseIIDService.cs** ve kendi şablon kodu aşağıdakilerle değiştirin: 

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

Bu hizmet uygulayan bir `OnTokenRefresh` kayıt belirteç başlangıçta oluşturulan veya değiştirilen çağrılan yöntem. Zaman `OnTokenRefresh` çalışır, son belirtecinden alır `FirebaseInstanceId.Instance.Token` (zaman uyumsuz olarak FCM tarafından güncelleştirilir) özelliği. Böylece çıktı penceresinde görüntülenebilir Bu örnekte, yenilenen belirteci kaydedilir: 

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` seyrek çağrılır: belirtecin aşağıdaki koşullarda güncelleştirmek için kullanılır: 

-   Ne zaman uygulama yüklendiğinde veya kaldırıldığında.

-   Ne zaman kullanıcı uygulama verilerini siler. 

-   Uygulama örnek kimliği zaman siler

-   Ne zaman belirteç güvenliğinin aşılmış.

Google göre [örnek kimliği](https://developers.google.com/instance-id/guides/android-implementation) , belgeler, FCM örnek kimliği hizmet isteği uygulama kendi belirtecini düzenli aralıklarla (genellikle, 6 ayda bir) yenileyin. 

`OnTokenRefresh` Ayrıca çağırır `SendRegistrationToAppServer` kullanıcının kayıt ilişkilendirmek için sunucu tarafı hesabı (varsa) ile belirteci, uygulama tarafından korunur: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Bu uygulama üzerinde uygulama sunucusu tasarımı bağımlı olduğundan boş yöntem gövdesi bu örnekte sağlanır. Uygulama sunucunuza FCM kayıt bilgileri gerekiyorsa değiştirin `SendRegistrationToAppServer` uygulamanız tarafından tutulan herhangi bir sunucu tarafı hesabı kullanıcının FCM örnek kimliği belirtecini ilişkilendirmek için. (Belirteç istemci uygulamasına donuk olduğunu unutmayın.) 

Uygulama sunucusu için bir belirteç gönderildiğinde `SendRegistrationToAppServer` belirteç sunucuya gönderilen olup olmadığını belirten bir Boole değeri korumanız gerekir. Bu boolean yanlışsa `SendRegistrationToAppServer` belirteci uygulama sunucusuna gönderir &ndash; Aksi halde, belirteç zaten önceki çağrıda uygulama sunucusuna gönderildi. Bazı durumlarda (Bu gibi `FCMClient` örnek), uygulama sunucusu belirteç gerekmez; bu nedenle, bu yöntem bu örnek için gerekli değildir. 

## <a name="implement-client-app-code"></a>Uygulama istemci uygulaması kodu

Alıcı hizmetlerini karşılandığından, bu hizmetleri yararlanmak için istemci uygulaması kodu yazılabilir. Aşağıdaki bölümlerde, kayıt belirtecinizi oturum için kullanıcı Arabirimi bir düğme eklenir (olarak da bilinir *örnek kimliği belirteci*), ve daha fazla kod eklenir `MainActivity` görüntülemek için `Intent` gelen uygulama başlatıldığında bilgi bir bildirim: 

[![Uygulama ekranına eklenen günlüğü belirteci düğmesi](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>Günlük belirteçleri

Bu adımda eklediğiniz kodun yalnızca gösterim amacıyla tasarlanmıştır &ndash; bir üretim istemci uygulaması kayıt belirteçleri oturum gerek yoktur. Düzen **Resources/layout/Main.axml** ve aşağıdakileri ekleyin `Button` bildirimi hemen sonra `TextView` öğe: 

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Düzen **MainActivity.cs** ve aşağıdaki üye bildirimi ekleyin `MainActivity` sınıfı:

```csharp
const string TAG = "MainActivity";
```

Sonuna aşağıdaki kodu ekleyin `OnCreate` yöntemi: 

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Bu kod geçerli belirteç çıkış penceresine oturum zaman **günlük belirteci** düğmesi dokunduğunuz. 

### <a name="handle-notification-intents"></a>Tanıtıcı bildirim hedefleri

Kullanıcı tarafından verilen bir bildirim zaman dokunur **FCMClient**, bu bildirim eşlik eden herhangi bir veri ileti kullanılabilir hale `Intent` ek özellikler. Düzen **MainActivity.cs** ve en üst kısmına aşağıdaki kodu ekleyin `OnCreate` yöntemi (çağırmadan önce `IsPlayServicesAvailable`): 

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

Uygulamanın Başlatıcısı `Intent` bu kodu eşlik eden herhangi bir veri oturum açacak şekilde kullanıcı, bildirim iletisini dokunur durumlarda tetiklenir `Intent` çıkış penceresine. Farklı bir `Intent` harekete gerekir, `click_action` alanı bildirim iletisi için ayarlanmalıdır `Intent` (Başlatıcısı `Intent` durumlarda kullanılan `click_action` belirtilir). 


## <a name="background-notifications"></a>Arka plan bildirimleri

Derleme ve çalıştırma **FCMClient** uygulama. **Günlük belirteci** düğmesi görüntülenir:

[![Oturum belirteci düğmesi görüntülenir](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

Dokunun **günlük belirteci** düğmesi. IDE çıktı penceresinde aşağıdakine benzer bir ileti görüntülenir: 

[![Çıktı penceresinde görüntülenen örnek ID simgesi](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

İle uzun bir dize olarak etiketlenmiş **belirteci** Firebase konsoluna Yapıştır bir örnek kimliği belirteç &ndash; seçin ve bu dizesini Pano'ya kopyalayın. Bir örnek kimliği belirteci görmüyorsanız üstüne aşağıdaki satırı ekleyin `OnCreate` doğrulamak için yöntem **google services.json** doğru ayrıştırıldığında:

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

`google_app_id` Çıkış penceresine kaydedilip değeri eşleşmelidir `mobilesdk_app_id` kayıtlı değer **google services.json**. 

### <a name="send-a-message"></a>İleti gönderme

Oturum [Firebase konsol](https://console.firebase.google.com), projenizi seçin, **bildirimleri**, tıklatıp **ilk ileti gönder**: 

[![Bilgisayarınızı ilk ileti düğmesi Gönder](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

Üzerinde **oluşturma iletisi** sayfasında, ileti metnini girin ve seçin **tek aygıt**. IDE çıktı penceresinden örnek kimliği belirteci kopyalayın ve yapıştırın **FCM kayıt belirtecinizi** Firebase konsolunun alan: 

[![Bir iletişim kutusu oluşturma](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Android cihaz (veya öykünücü), Android dokunarak uygulama arka plan **genel bakış** düğmesi ve giriş ekranı temas. Cihaz hazır olduğunda tıklatın **iletisi gönder** Firebase konsolunda: 

[![İleti düğmesi Gönder](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

Zaman **gözden geçirme ileti** iletişim kutusu gösterilir, tıklatın **Gönder**.
Bildirim simgesini cihaz (veya öykünücü) bildirim alanında görünmelidir: 

[![Bildirim simgesi görüntülenir](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

İletiyi görüntülemek için bildirim simgesine açın. Bildirim iletisi, tam olarak ne içine yazılmış olmalıdır **ileti metni** Firebase konsolunun alan: 

[![Bildirim iletisi cihazda görüntülenir](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

Geri dönmek için bildirim simgesine dokunun **FCMClient** uygulama. `Intent` Gönderilen ek özellikler **FCMClient** IDE çıktı penceresinde listelenir: 

[![Anahtar, ileti kimliği ve Daralt anahtar hedefi ek özellikler listeleri](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

Bu örnekte, **gelen** anahtarını uygulama Firebase proje sayısını ayarlayın (Bu örnekte, `41590732`) ve **collapse_key** paket adını ayarlayın ( **com.xamarin.fcmexample**). Bir ileti almazsanız silmeyi deneyin **FCMClient** cihaz (veya öykünücü) uygulama ve yukarıdaki adımları yineleyin. 


> [!NOTE]
> Zorla-Kapat uygulama, FCM bildirimleri teslim durdurur. Android arka plan hizmet yayınları yanlışlıkla veya gereksiz yere durdurulmuş uygulamaları bileşenlerinin başlatmasını engeller. (Bu davranış hakkında daha fazla bilgi için bkz: [başlatma durdurulmuş uygulamaları denetimlere](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) Bu nedenle, gerekli uygulama her zaman el ile kaldırmak için çalıştırmak ve hata ayıklama oturumu Durdur &ndash; bu iletileri alınacak devam edebilmesi için yeni bir belirteç oluşturmak için FCM zorlar.

### <a name="add-a-custom-default-notification-icon"></a>Bir özel varsayılan bildirim simgesi ekleme

Önceki örnekte, bildirim simgesine uygulama simgesi ayarlanır. Aşağıdaki XML bildirimlerine yönelik özel varsayılan bir simge yapılandırır. Android bildirim simgesine değil açıkça ayarlandığı tüm bildirim iletileri için bu özel varsayılan simgesini görüntüler. 

Özel varsayılan bildirim simgesi eklemek için simgeyi ekleyin **kaynakları/drawable** dizini Düzenle **AndroidManifest.xml**ve aşağıdaki Ekle `<meta-data>` biröğeye`<application>`bölümü: 

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

Bu örnekte, bildirim simgesine bulunan adresindeki **kaynakları/drawable/ŞA\_stat\_ŞA\_notification.png** özel varsayılan bildirim simgesi olarak kullanılacak. Özel varsayılan bir simge devre dışı bırakırsanız **AndroidManifest.xml** ve hiçbir simge bildirim yükü ayarlandığında, Android (yukarıdaki bildirim simgesine ekran görüntüsünde görüldüğü gibi) bu uygulama simgesi bildirim simgesi olarak kullanır. 

## <a name="handle-topic-messages"></a>Konu iletilerini işleme

Bugüne kadarki yazılan kod kayıt belirteçleri işler ve uygulamaya uzaktan bildirim işlevsellik ekler. Sonraki örnek dinler kod ekler *konu iletileri* kullanıcıya uzak olarak bildirimleri gönderir. Konu iletileri belirli bir konuya abone bir veya daha fazla cihazlara gönderilen FCM iletileri edilir. Konu iletileri hakkında daha fazla bilgi için bkz: [konu ileti](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

### <a name="subscribe-to-a-topic"></a>Bir konuya abone olma

Düzen **Resources/layout/Main.axml** ve aşağıdakileri ekleyin `Button` hemen sonra önceki bildirimi `Button` öğe:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

Bu XML ekler bir **abone bildirimi** Düzen düğmesi.
Düzen **MainActivity.cs** ve sonuna aşağıdaki kodu ekleyin `OnCreate` yöntemi: 

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Bu kod bulur **abone bildirimi** düzeni düğmesini ve onun tıklatın işleyici çağıran kodu atar `FirebaseMessaging.Instance.SubscribeToTopic`, abone konusunda geçen _haber_. Kullanıcı ne zaman dokunur **abone ol** uygulama düğmesi, aboneliği _haber_ konu. Aşağıdaki bölümde bir _haber_ konu ileti Firebase konsol bildirimleri GUI gönderilir. 

### <a name="send-a-topic-message"></a>Konu ileti gönderme

Uygulamaları kaldırmak, yeniden oluşturmak ve yeniden çalıştırın. Tıklatın **Bildirimlere abone olma** düğmesi:

[![Bildirimleri düğmesine abone olma](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

Uygulama başarıyla abone olursa görmelisiniz **konu eşitleme başarılı** IDE içinde çıktı penceresinde: 

[![Çıktı penceresi konu başarılı eşitleme iletisi gösterir](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

Bir konu ileti göndermek için aşağıdaki adımları kullanın:

1.  Firebase konsolunda **yeni ileti**. 

2.  Üzerinde **oluşturma iletisi** sayfasında, ileti metnini girin ve seçin **konu**. 

3.  İçinde **konu** aşağı açılır menüsünde, yerleşik konu seçmek **haber**: 

    [![Haber konu seçme](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Android cihaz (veya öykünücü), Android dokunarak uygulama arka plan **genel bakış** düğmesi ve giriş ekranı temas. 

5.  Cihaz hazır olduğunda tıklatın **iletisi gönder** Firebase konsolunda. 

6.  Görmek için IDE çıktı penceresini denetleyin **/konuları/haber** günlük çıkışı: 

    [![/Topic/news iletiden gösterilir](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

Bu ileti çıktı penceresinde görülür, bildirim simgesine ayrıca Android cihazda bildirim alanında görüntülenmelidir. Konu iletiyi görüntülemek için bildirim simgesine açın: 

[![Konu ileti bildirim olarak görünür](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

Bir ileti almazsanız silmeyi deneyin **FCMClient** cihaz (veya öykünücü) uygulama ve yukarıdaki adımları yineleyin. 

## <a name="foreground-notifications"></a>Ön plan bildirimleri

Foregrounded uygulamalarında bildirimleri almak için uygulamanız gereken `FirebaseMessagingService`. Bu hizmet de, veri yüklerini alma ve Yukarı Akış ileti göndermek için gereklidir. Aşağıdaki örnekler genişleten bir hizmet uygulamak nasıl çalışılacağını `FirebaseMessagingService` &ndash; elde edilen uygulama ön planda çalışırken uzak bildirimler işleyebilen olacaktır. 

### <a name="implement-firebasemessagingservice"></a>Uygulama FirebaseMessagingService

`FirebaseMessagingService` Hizmeti içeren bir `OnMessageReceived` gelen iletileri işlemek için yazma yöntemi. Unutmayın `OnMessageReceived` bildirim iletileri için çağrılan *yalnızca* uygulama ön planda çalışırken. Uygulama arka planda çalıştırırken (Bu kılavuzda daha önce gösterildiği gibi) otomatik olarak oluşturulan bir bildirim görüntülenir. 

Adlı yeni bir dosya ekleme **MyFirebaseMessagingService.cs** ve kendi şablon kodu aşağıdakilerle değiştirin: 

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

Unutmayın `MESSAGING_EVENT` hedefi filtre bildirilmelidir, böylece yeni FCM iletileri yönlendirilir `MyFirebaseMessagingService`: 

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

İstemci uygulaması FCM bir ileti aldığında `OnMessageReceived` geçirilen bileşeninden ileti içeriği ayıklar `RemoteMessage` çağırarak nesne kendi `GetNotification` yöntemi. Ardından, böylece IDE çıktı penceresinde görüntülenebilir ileti içeriği kaydeder: 

```csharp
Log.Debug(TAG, "From: " + message.From);
Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
```

> [!NOTE]
> Kesme noktası ayarlarsanız `FirebaseMessagingService`, hata ayıklama oturumunun olabilir veya bu kesme noktaları FCM iletileri nasıl teslim nedeniyle isabet değil.
 

### <a name="send-another-message"></a>Başka bir ileti gönderme

Uygulamaları kaldırmak, yeniden oluşturmak, yeniden çalıştırın ve başka bir ileti göndermek için şu adımları izleyin:

1.  Firebase konsolunda **yeni ileti**. 

2.  Üzerinde **oluşturma iletisi** sayfasında, ileti metnini girin ve seçin **tek aygıt**. 

3.  IDE çıktı penceresinden belirteç dizesini kopyalayın ve yapıştırın **FCM kayıt belirtecinizi** eskisi Firebase konsolunun alan. 

4.  Uygulama ön planda çalıştığından emin olun ve ardından **iletisi gönder** Firebase konsolunda: 

    [![Konsolundan başka bir ileti gönderme](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  Zaman **gözden geçirme ileti** iletişim kutusu gösterilir, tıklatın **Gönder**.

6.  Gelen ileti IDE çıkış penceresine kaydedilir:

    [![İleti gövdesi çıkış penceresine yazdırılan](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notifications-sender"></a>Yerel bildirimler göndereni Ekle

Kalan Bu örnekte, gelen FCM ileti uygulama ön planda çalışırken başlatılır yerel bir bildirim dönüştürülecek. Düzen **MyFirebaseMessageService.cs** ve aşağıdakileri ekleyin `using` deyimleri: 

```csharp
using FCMClient;
using System.Collections.Generic;
```

Aşağıdaki yöntemi ekleyin `MyFirebaseMessagingService`: 

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (string key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }
    var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new Notification.Builder(this)
        .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
        .SetContentTitle("FCM Message")
        .SetContentText(messageBody)
        .SetAutoCancel(true)
        .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}
```

Bu bildirim arka plan bildirimlerin ayırt etmek için bu kodu bildirimleri applicatiion simgesinden farklı bir simge ile işaretler. Ekleme [ŞA\_stat\_ŞA\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) için **kaynakları/drawable** ve olmasını **FCMClient** projesi. 

`SendNotification` Yöntemi kullanan `Notification.Builder` bildirim oluşturmak için ve `NotificationManager` bildirim başlatmak için kullanılır. Bildirim tutan bir `PendingIntent` uygulamasını açın ve içine geçirilen dize içeriğini görüntülemek kullanıcı izni verdiği `messageBody`. Uygulamanızla hedeflemek istediğiniz Android sürümlerini bağlı olarak kullanmak isteyebilirsiniz `NotificationCompat.Builder` yerine. Hakkında daha fazla bilgi için `Notification.Builder`, bkz: [yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications.md). 

Çağrı `SendNotification` yöntemi sonunda `OnMessageReceived` yöntemi: 

```csharp
SendNotification(message.GetNotification().Body, message.Data);
```

Bu değişiklikler sonucunda `SendNotification` uygulama ön planda ve bildirim bildirim alanında görünür bir bildirim alındığında çalışacak. Uygulama backgrounded, `SendNotification` çalışmaz ve bunun yerine, (Bu kılavuzda daha önce gösterilen) bir arka plan bildirim başlatılacak. 

### <a name="send-the-last-message"></a>Son ileti gönderme

Uygulamaları kaldırmak, yeniden oluşturmak, yeniden çalıştırın ve sonra son ileti göndermek için aşağıdaki adımları kullanın:

1.  Firebase konsolunda **yeni ileti**. 

2.  Üzerinde **oluşturma iletisi** sayfasında, ileti metnini girin ve seçin **tek aygıt**. 

3.  IDE çıktı penceresinden belirteç dizesini kopyalayın ve yapıştırın **FCM kayıt belirtecinizi** eskisi Firebase konsolunun alan. 

4.  Uygulama ön planda çalıştığından emin olun ve ardından **iletisi gönder** Firebase konsolunda: 

    [![Ön plan ileti gönderme](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

Bu süre, çıktı penceresinde kaydedilen iletinin yeni bir bildirim de paketlenmiş &ndash; uygulama ön planda çalıştığı sırada bildirim simgesine bildirim tepsisinde görünür: 

[![Ön plan ileti için bildirim simgesi](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

Bildirim açtığınızda, Firebase konsol bildirimleri GUI gönderilen son ileti görmeniz gerekir: 

[![Ön plan simgesiyle gösterilen ön bildirimi](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>FCM bağlantısı kesiliyor

Konuyu aboneliğinizi iptal etmek için çağrı [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) yöntemi [FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) sınıfı. Örneğin, aboneliğinizi iptal etmek _haber_ daha önce abone için konu bir **Unsubscribe** düğme, aşağıdaki işleyici kodu düzeniyle eklenebiliyordu:

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

FCM değerlerinin aygıttan kaydını kaldırmak için örnek kimliği çağırarak silme [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) yöntemi [FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) sınıfı. Örneğin:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

Bu yöntem çağrısı siler örnek kimliği ve ilişkili veriler. Sonuç olarak, aygıta FCM veri düzenli gönderme durdurulur.

 
## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki sorunların ve Firebase bulut Mesajlaşma ile Xamarin.Android kullanırken kaynaklanabilecek geçici çözümler.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp başlatılmadı

Bazı durumlarda, bu hata iletisi görebilirsiniz:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Bu çözüm temizleme ve projeyi yeniden derlemeyi geçici çalışabilen, bilinen bir sorundur (**Yapı > temiz çözüm**, **Yapı > çözümü yeniden derle**). Daha fazla bilgi için bkz [forum tartışmasına](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process).


## <a name="summary"></a>Özet

Bu kılavuz bir Xamarin.Android uygulamasına bulut Firebase Mesajlaşma uzak bildirimler uygulama adımlarını ayrıntılı. FCM iletişimleri için gerekli gerekli paketleri yüklemek nasıl açıklanan ve Android derleme bildirimi FCM sunucularına erişimi için yapılandırma konusunda açıklanmıştır. Google Play Hizmetleri'nin olup olmadığını denetlemek üzere verilmektedir örnek kodu sağlanır. Uygulama kaydı belirteci için FCM ile görüşür bir örnek kimliği dinleme hizmeti gösterilen ve uygulama backgrounded karşın bu kodu arka plan bildirimleri nasıl oluşturduğunu açıklanmıştır. Örnek uygulama almak ve uygulama ön planda çalışırken uzak bildirimler görüntülemek için kullanılan bir ileti dinleme hizmeti için sağlanan ve konu iletileri abone olunacağı açıklanmaktadır. 


## <a name="related-links"></a>İlgili bağlantılar

- [FCMNotifications (örnek)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
