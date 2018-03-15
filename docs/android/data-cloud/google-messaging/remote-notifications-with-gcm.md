---
title: "Google bulut Mesajlaşma ile uzaktan bildirimleri"
description: "Bu kılavuz, bir Xamarin.Android uygulaması Google Cloud Messaging (anında iletme bildirimleri olarak da bilinir) uzaktan bildirimleri uygulamak için nasıl kullanılacağını hakkında adım adım bir açıklama sağlar. Google Cloud Messaging (GCM) ile iletişim kurmak için uygulamanız gereken çeşitli sınıflar açıklanmaktadır, Android bildirim GCM için erişim izinlerini ayarlamak nasıl açıklar ve bir örnek test programla uçtan uca ileti gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 823fad163e837adab5490446c23ab2f492679114
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Google bulut Mesajlaşma ile uzaktan bildirimleri

_Bu kılavuz, bir Xamarin.Android uygulaması Google Cloud Messaging (anında iletme bildirimleri olarak da bilinir) uzaktan bildirimleri uygulamak için nasıl kullanılacağını hakkında adım adım bir açıklama sağlar. Google Cloud Messaging (GCM) ile iletişim kurmak için uygulamanız gereken çeşitli sınıflar açıklanmaktadır, Android bildirim GCM için erişim izinlerini ayarlamak nasıl açıklar ve bir örnek test programla uçtan uca ileti gösterir._

## <a name="gcm-notifications-overview"></a>GCM bildirimleri genel bakış

Bu kılavuzda, uzak bildirimleri uygulamak için Google Cloud Messaging (GCM) kullanan bir Xamarin.Android uygulaması oluşturacağız (olarak da bilinen *anında iletme bildirimleri*). Uzak ileti için GCM kullanan çeşitli amacını ve dinleyici Hizmetleri uygulamak ve bir uygulama sunucusu taklit eden bir komut satırı programı bizim uygulamasıyla test edeceksiniz. 

Firebase bulut Mesajlaşma (FCM) GCM yeni sürümünü olduğuna dikkat edin &ndash; Google kesinlikle önerir GCM yerine FCM. GCM kullanıyorsanız, FCM için yükseltme önerilir. FCM hakkında daha fazla bilgi için bkz: [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Bu kılavuzda ile devam etmeden önce Google GCM sunucuları kullanmak için gerekli kimlik bilgilerini edinmeniz gerekir; Bu işlem açıklaması [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md). Özellikle, ihtiyacınız olacak bir *API anahtarı* ve *Gönderen Kimliği* bu kılavuzda sunulan kod örneği eklemeye. 

Xamarin.Android GCM özellikli istemci uygulaması oluşturmak için aşağıdaki adımları kullanacağız:

1.  GCM sunucuları ile iletişim için gereken ek paketler yükleyin.
2.  GCM sunuculara erişim için uygulama izinlerini yapılandırın.
3.  Google Play Hizmetleri'nin olup olmadığını denetlemek için kod uygulayın. 
4.  Bir kayıt belirteci için GCM ile görüşür kayıt hedefi hizmeti uygulayın.
5.  GCM kayıt belirteci güncelleştirmelerini dinleyen bir örnek kimliği dinleme hizmeti uygulayın.
6.  Uygulama sunucusu GCM üzerinden gelen uzak iletileri alan bir GCM dinleme hizmeti uygulayın.

Bu uygulama olarak bilinen yeni bir GCM özellik kullanır *konu ileti*. Konu ileti içinde uygulama sunucusu bir ileti bir konu yerine tek tek cihazların bir listesini gönderir. Bu konuya abone cihazlara anında iletme bildirimleri olarak konu ileti alabilir. Google GCM konu ileti hakkında daha fazla bilgi için bkz: [uygulama konu ileti](https://developers.google.com/cloud-messaging/topic-messaging). 

İstemci uygulaması hazır olduğunda, biz bizim istemci uygulaması GCM aracılığıyla bir anında iletme bildirimi gönderen bir komut satırı C# uygulaması uygulamanız. 

## <a name="walkthrough"></a>İzlenecek yol

Başlamak için adlı yeni bir boş çözüm oluşturalım **RemoteNotifications**. Ardından, yeni bir Android projesi temel alan bu çözüme ekleyelim **Android uygulaması** şablonu. Şimdi bu projenin adı **ClientApp**. (Xamarin.Android projeleri oluşturma ile bilmiyorsanız bkz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) **ClientApp** proje GCM aracılığıyla uzaktan bildirimleri alan Xamarin.Android istemci uygulaması için kod içerir. 

### <a name="add-required-packages"></a>Gerekli paketleri ekleme

Biz bizim istemci uygulaması kodu uygulamadan önce biz GCM ile iletişim için kullanacağız çeşitli paketler yüklemeniz gerekir. Ayrıca, zaten yüklü değilse biz bizim aygıt Google Play mağazası uygulamaya eklemeniz gerekir.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Xamarin Google Play Hizmetleri GCM Paketi Ekle

Google Cloud Messaging iletileri almak için [Google Play Hizmetleri](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) framework cihazda mevcut olmalıdır. Bu çerçeve bir Android uygulaması GCM sunucularından mesajlarını alamıyor. Android cihazı, açık durumdayken Google Play Hizmetleri arka planda sessizce GCM gelen iletiler için dinleme çalışır. Bu iletiler geldiğinde, Google Play Hizmetleri hedefleri iletileri dönüştürür ve bunlar için kayıtlı uygulamalar için bu hedefleri yayınlar. 

Visual Studio'da sağ **başvuruları > NuGet paketlerini Yönet...** ; Mac Visual Studio'da sağ **paketleri > paketleri Ekle...** . Arama **Xamarin Google Play Hizmetleri - GCM** bu pakete yükleyip **ClientApp** proje: 

[![Google Play hizmetlerini yükleme](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

Yüklediğinizde **Xamarin Google Play Hizmetleri - GCM**, **Xamarin Google Play Hizmetleri - Base** otomatik olarak yüklenir. Bir hata alırsanız, projenin değiştirme *Minimum Android hedef* dışında bir değere ayarlamak **SDK sürümüyle derleme** ve NuGet yüklemeyi yeniden deneyin. 

Ardından, düzenleme **MainActivity.cs** ve aşağıdakileri ekleyin `using` deyimleri:

```csharp
using Android.Gms.Common;
using Android.Util;
```

Bu türleri Google Play Hizmetleri GMS paketinde bizim kod için kullanılabilir hale getirir ve GMS bizim işlemleri izlemek için kullanacağız günlük işlevsellik ekler. 

#### <a name="google-play-store"></a>Google Play Store

GCM'den iletileri almak için Google Play mağazası uygulama cihaza yüklenmelidir. (Bir Google Play uygulaması bir aygıtta yüklü olduğunda, test Cihazınızda zaten yüklüyse olasıdır için Google Play Mağazası'da, yüklenir.) Google Play bir Android uygulama GCM'den mesajlarını alamıyor. Google Play mağazası uygulamasının aygıtınızda yüklü henüz yoksa ziyaret [Google Play](https://support.google.com/googleplay) indirmek ve Google Play yüklemek için web sitesi. 

Alternatif olarak, Android 2.2 veya sonraki sürümünü (, Google Play mağazası Android öykünücüsünde yüklemek zorunda değildir) bir test cihazı yerine çalıştıran Android öykünücüsünde kullanabilirsiniz. Ancak, bir öykünücü kullanırsanız, GCM'ye bağlanmak için Wi-Fi kullanmanız gerekir ve daha sonra bu kılavuzda açıklandığı gibi Wi-Fi Güvenlik Duvarı'nda çeşitli bağlantı noktaları açmalısınız. 

### <a name="set-the-package-name"></a>Paket adı ayarlama

İçinde [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md), biz bizim GCM özellikli uygulama için bir paket adı belirtilen (Bu paket adı da görevi gören *uygulama kimliği* bizim API anahtarı ve Gönderen Kimliği ile ilişkili). Özelliklerini açalım **ClientApp** proje ve paket adı bu dizeye ayarlayın. Bu örnekte, paket adı ayarlarız `com.xamarin.gcmexample`:

[![Paket adı ayarlama](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

İstemci uygulaması bu paket adı yoksa GCM kayıt belirtecinizi alamıyor olacağını unutmayın *tam olarak* Google Developer konsolunda girdiğimiz paket adı ile eşleşmesi. 

### <a name="add-permissions-to-the-android-manifest"></a>İzinleri eklemek için Android derleme bildirimi

Bir Android uygulamasını bildirimleri Google Cloud Messaging almadan önce yapılandırılmış aşağıdaki izinlerine sahip olmanız gerekir: 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; Kaydolun ve Google Cloud Messaging iletileri almak için bizim uygulamasına izin verir. (Ne yaptığını `c2dm` anlamına gelir? Bu anlamına gelir _Device Messaging buluta_, GCM için artık kullanım dışı öncül olduğu. 
    GCM hala kullanan `c2dm` birçok izni dizeleri.) 

-   `android.permission.WAKE_LOCK` &ndash; (İsteğe bağlı) İşlemcisinden aygıtı için bir ileti dinlerken uyku moduna geçmesini engeller. 

-   `android.permission.INTERNET` &ndash; İstemci uygulaması GCM ile iletişim kurabilmesi Internet erişimi verir. 

-   *paket_adı* `.permission.C2D_MESSAGE` &ndash; ile Android uygulamaları kaydeder ve özel olarak tüm C2D alma izni isteklerini (bulut aygıta) iletileri. *Paket_adı* önektir uygulama kimliği ile aynı 

Android bildirimde izinlerin yaparız. Düzenleyelim **AndroidManifest.xml** ve içeriği aşağıdaki XML ile değiştirin: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
    package="YOUR_PACKAGE_NAME" 
    android:versionCode="1" 
    android:versionName="1.0" 
    android:installLocation="auto">
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" />
    <permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" 
                android:protectionLevel="signature" />
    <application android:label="ClientApp" android:icon="@drawable/Icon">
    </application>
</manifest>
```

Yukarıdaki XML'de değiştirme *YOUR_PACKAGE_NAME* istemci uygulaması projenize için paket adı için. Örneğin, `com.xamarin.gcmexample`. 

### <a name="check-for-google-play-services"></a>Google Play hizmetlerini denetle

Bu kılavuz için tam kemikler uygulama ile tek bir oluşturuyoruz `TextView` kullanıcı arabiriminde. Bu uygulamayı doğrudan GCM ile etkileşimi göstermez. Bunun yerine, biz görmek için çıkış Gözcü penceresi nasıl bizim uygulama el sıkışmaları GCM ile ve geldikçe yeni bildirimleri için bildirim Tepsisi kontrol edeceğiz. 

İlk olarak, ileti alanı için bir düzen oluşturalım. Düzen **Resources.layout.Main.axml** ve içeriği aşağıdaki XML ile değiştirin: 

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

Kaydet **Main.axml** ve kapatın.

GCM başvurun denemeden önce Google Play Hizmetleri'nin kullanılabilir olduğunu doğrulamak için istiyoruz istemci uygulaması başlatıldığında. Düzen **MainActivity.cs** ve değiştirme ``count`` örneği aşağıdaki örneği değişken bildirimi ile değişken bildirimi: 

```csharp
TextView msgText;
```

Sonra aşağıdaki yöntemi ekleyin **MainActivity** sınıfı: 

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
            msgText.Text = "Sorry, this device is not supported";
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

Bu kod, Google Play Hizmetleri APK yüklü olup olmadığını görmek için aygıt denetler. Yüklenmemişse, bir APK Google Play mağazasından indirin (veya aygıtın sistem ayarlarını etkinleştir) kullanıcıya bildirir ileti alanında bir ileti görüntülenir. İstemci uygulaması başlatıldığında, bu denetimi gerçekleştirmek istiyoruz çünkü bu yöntem çağrısı sonunda ekleyeceğiz `OnCreate`. 


Ardından, yerine `OnCreate` aşağıdaki kod ile yöntemi:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

Bu kod, Google Play Hizmetleri APK varlığını denetler ve sonucu iletisini alan yazar. 

Şimdi tamamen yeniden oluşturun ve uygulamayı çalıştırın. Aşağıdaki ekran görüntüsü gibi görünen bir ekran görmeniz gerekir: 

[![Google Play Hizmetleri'nin kullanılabilir](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

Bu sonuç alamazsanız Google Play Hizmetleri APK Cihazınızı ve, üzerinde yüklü olduğunu doğrulayın **Xamarin Google Play Hizmetleri - GCM** paket eklenmiş olup, **ClientApp** proje açıklandığı gibi daha önce. Bir derleme hatası alırsanız çözümü temizleme ve projeyi yeniden oluşturmayı deneyin. 

Ardından, biz GCM başvurun ve geri kayıt belirtecinizi almak için kod yazacaksınız.

### <a name="register-with-gcm"></a>GCM ile kayıt

Uygulama uygulama sunucusundan uzak bildirimleri almak için önce GCM ile kayıt ve kayıt belirteç geri alın. GCM ile birlikte uygulamamız kaydı iş tarafından işlenen bir `IntentService` , oluşturuyoruz. Bizim `IntentService` aşağıdaki adımları gerçekleştirir: 

1.  Kullanan [InstanceId](https://developers.google.com/instance-id/) uygulama sunucusuna erişmek için istemci uygulamamıza yetkilendirmek güvenlik belirteçleri oluşturmak için API. Buna karşılık, biz geri bir kayıt GCM'den belirteci alın.

2.  (Uygulama sunucusu gerektiriyorsa,) kayıt belirtecinizi uygulama sunucusuna iletir.

3.  Bir veya daha fazla bildirim konu kanalları abone olur.

Biz bu uyguladıktan sonra `IntentService`, biz geri bir kayıt GCM'den belirteci alırsanız görmek için bunu test edeceğiz.

Adlı yeni bir dosya ekleme **RegistrationIntentService.cs** ve şablon kodu aşağıdakilerle değiştirin:


```csharp
using System;
using Android.App;
using Android.Content;
using Android.Util;
using Android.Gms.Gcm;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false)]
    class RegistrationIntentService : IntentService
    {
        static object locker = new object();

        public RegistrationIntentService() : base("RegistrationIntentService") { }

        protected override void OnHandleIntent (Intent intent)
        {
            try
            {
                Log.Info ("RegistrationIntentService", "Calling InstanceID.GetToken");
                lock (locker)
                {
                    var instanceID = InstanceID.GetInstance (this);
                    var token = instanceID.GetToken (
                        "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);

                    Log.Info ("RegistrationIntentService", "GCM Registration Token: " + token);
                    SendRegistrationToAppServer (token);
                    Subscribe (token);
                }
            }
            catch (Exception e)
            {
                Log.Debug("RegistrationIntentService", "Failed to get a registration token");
                return;
            }
        }

        void SendRegistrationToAppServer (string token)
        {
            // Add custom implementation here as needed.
        }

        void Subscribe (string token)
        {
            var pubSub = GcmPubSub.GetInstance(this);
            pubSub.Subscribe(token, "/topics/global", null);
        }
    }
}
```

Yukarıdaki örnek kodda değişiklik *YOUR_SENDER_ID* istemci uygulaması projenize Gönderen Kimliği sayısı. Projeniz için gönderen Kimliği almak için: 

1.  İçine oturum [Google Cloud Console](https://console.cloud.google.com/) ve projenizin adına çekme menüsünü seçin. İçinde **proje bilgileri** projeniz için görüntülenen bölmesi **proje Ayarları'na gidin**:

    [![XamarinGCM proje seçme](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  Üzerinde **ayarları** sayfasında, bulun **proje numarası** &ndash; projeniz için gönderen Kimliği budur:

    [![Görüntülenen Proje numarası](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

Biz başlatmak istediğiniz bizim `RegistrationIntentService` uygulamamıza çalışmaya başladığında. Düzen **MainActivity.cs** ve değiştirme `OnCreate` yöntemi böylece bizim `RegistrationIntentService` biz Google Play Hizmetleri'nin varlığını denetledikten sonra başlatılır: 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView(Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    if (IsPlayServicesAvailable ())
    {
        var intent = new Intent (this, typeof (RegistrationIntentService));
        StartService (intent);
    }
}
```

Artık her bölüm bir göz atalım `RegistrationIntentService` nasıl çalıştığını anlamak için. 

İlk olarak, biz açıklama bizim `RegistrationIntentService` hizmetimizi sistem tarafından örneği değil olduğunu belirtmek için aşağıdaki özniteliğiyle: 

```csharp
[Service (Exported = false)]
```

`RegistrationIntentService` Oluşturucusu çalışan iş parçacığı adları *RegistrationIntentService* daha kolay hata ayıklama yapma. 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

Çekirdek işlevselliğini `RegistrationIntentService` bulunan `OnHandleIntent` yöntemi. Şimdi bu kodu nasıl uygulamamıza GCM'ye kaydedilir görmek için yol.


##### <a name="request-a-registration-token"></a>Bir kayıt belirteci iste

`OnHandleIntent` önce Google çağırır [InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) GCM'den kaydı belirtecini istemek için yöntem. Biz bu kodda sarmalamak bir `lock` aynı anda gerçekleşen birden çok kayıt hedefleri olasılığını karşı koruma sağlamak için &ndash; `lock` bu hedefleri sıralı olarak işlenir sağlar. Biz kayıt belirtecinizi almak başarısız olursa, bir özel durum ve hata oturumunuzu. Kayıt başarılı olursa, `token` biz var geri GCM'den kayıt belirtecine ayarlayın: 

```csharp
static object locker = new object ();
...
try
{
    lock (locker)
    {
        var instanceID = InstanceID.GetInstance (this);
        var token = instanceID.GetToken (
            "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);
        ...
    }
}
catch (Exception e)
{
    Log.Debug ...
```

##### <a name="forward-the-registration-token-to-the-app-server"></a>Kayıt belirtecinizi uygulama sunucusuna iletme

Biz kayıt belirtecinizi alırsanız (diğer bir deyişle, hiçbir özel durum oluştu), sizi arayarak `SendRegistrationToAppServer` kullanıcının kayıt ilişkilendirmek için sunucu tarafı hesabı (varsa) ile belirteci, uygulamamız tarafından korunur. Bu uygulama üzerinde uygulama sunucusu tasarımı bağlı olduğu için boş bir yöntem burada sağlanır: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Bazı durumlarda, kullanıcının kayıt belirtecini uygulama sunucusu gerekmez; Bu durumda, bu yöntem atlanabilir. Uygulama sunucusu için bir kayıt belirtecinizi gönderildiğinde `SendRegistrationToAppServer` belirteç sunucuya gönderilen olup olmadığını belirten bir Boole değeri korumanız gerekir. Bu boolean yanlışsa `SendRegistrationToAppServer` belirteci uygulama sunucusuna gönderir &ndash; Aksi halde, belirteç zaten önceki çağrıda uygulama sunucusuna gönderildi. 


##### <a name="subscribe-to-the-notification-topic"></a>Bildirim konuya abone olma

Ardından, diyoruz bizim `Subscribe` yöntemi belirtmek için bir bildirim konuya abone olmak istiyoruz GCM için. İçinde `Subscribe`, diyoruz [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) istemci uygulamamıza altındaki tüm iletileri abone olmak için API `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

Bildirim iletileri için uygulama sunucusu göndermelidir `/topics/global` bunları almak için varsa. Konu adı altında Not `/topics` uygulama sunucusu ve istemci uygulaması bu adları kabul sürece, istediğiniz herhangi bir şey olabilir. (Burada, adı seçtik `global` uygulama sunucusu tarafından desteklenen tüm konularda iletileri almak istiyoruz belirtmek için.) 

Google sunucu tarafında Mesajlaşma GCM konu hakkında daha fazla bilgi için bkz [konuları ileti gönderme](https://developers.google.com/cloud-messaging/topic-messaging). 


#### <a name="implement-an-instance-id-listener-service"></a>Bir örnek kimliği dinleme hizmeti uygulama

Kayıt belirteçleri benzersiz ve güvenli; Ancak, istemci uygulaması (veya GCM) uygulama yeniden veya bir güvenlik sorunu durumunda kayıt belirtecinizi yenilemeniz gerekebilir. Bu nedenle, biz uygulamalıdır bir `InstanceIdListenerService` GCM'den belirteci yenileme istekleri yanıtlar. 

Adlı yeni bir dosya ekleme **InstanceIdListenerService.cs** ve şablon kodu aşağıdakilerle değiştirin: 

```csharp
using Android.App;
using Android.Content;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
    class MyInstanceIDListenerService : InstanceIDListenerService
    {
        public override void OnTokenRefresh()
        {
            var intent = new Intent (this, typeof (RegistrationIntentService));
            StartService (intent);
        }
    }
}
```

Ek açıklama `InstanceIdListenerService` hizmetinin sistem tarafından örneği değil olduğunu ve GCM kayıt belirtecinizi alabilir belirtmek için aşağıdaki özniteliğiyle (olarak da bilinir *kimliği örneği*) yenileme istekleri: 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

`OnTokenRefresh` Hizmetimizi yönteminde başlatır `RegistrationIntentService` böylece yeni kayıt belirtecinizi ele geçirebilir.


#### <a name="test-registration-with-gcm"></a>GCM ile kayıt testi

Şimdi tamamen yeniden oluşturun ve uygulamayı çalıştırın. Bir kayıt belirtecinizi başarıyla GCM'den alırsanız, kayıt belirtecinizi çıktı penceresinde görüntülenmesi gerekir. Örneğin: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>Aşağı Akış iletilerini işleme 

Biz bugüne kadarki uyguladık kodu yalnızca "Kurulum" kodudur; Google Play hizmetleri yüklenir ve Uzaktan bildirimlerini almak için istemci uygulamamıza hazırlamak için GCM ve uygulama sunucusu ile görüşür olmadığını denetler. Ancak, aslında alan ve aşağı akış bildirim iletileri işleyen kodu uygulamak henüz. Bunu yapmak için biz uygulamalıdır bir *GCM dinleme hizmeti*. Bu hizmet uygulama sunucusundan konu iletileri alır ve yerel olarak bunları bildirimleri olarak yayınlar. Biz bu hizmet uygulandıktan sonra böylece bizim uygulama düzgün çalışıyorsa görebiliriz GCM'ye iletileri göndermek için bir test programı oluşturacağız. 


#### <a name="add-a-notification-icon"></a>Bir bildirim simgesi ekleme

İlk bildirim alanında bizim bildirim başlatıldığında görünecek küçük bir simge ekleyelim. Kopyalayabilirsiniz [bu simgeyi](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) projenize veya kendi özel simge oluşturun. Simge dosyası adı **ic_stat_button_click.png** ve kopyalayın **kaynakları/drawable** klasör. Kullanmayı unutmayın **Ekle > varolan öğeyi...**  bu simge dosyası projenize eklemek için.


#### <a name="implement-a-gcm-listener-service"></a>GCM dinleyicisi hizmet ekleme

Adlı yeni bir dosya ekleme **GcmListenerService.cs** ve şablon kodu aşağıdakilerle değiştirin:

```csharp
using Android.App;
using Android.Content;
using Android.OS;
using Android.Gms.Gcm;
using Android.Util;

namespace ClientApp
{
    [Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
    public class MyGcmListenerService : GcmListenerService
    {
        public override void OnMessageReceived (string from, Bundle data)
        {
            var message = data.GetString ("message");
            Log.Debug ("MyGcmListenerService", "From:    " + from);
            Log.Debug ("MyGcmListenerService", "Message: " + message);
            SendNotification (message);
        }

        void SendNotification (string message)
        {
            var intent = new Intent (this, typeof(MainActivity));
            intent.AddFlags (ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity (this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                .SetSmallIcon (Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle ("GCM Message")
                .SetContentText (message)
                .SetAutoCancel (true)
                .SetContentIntent (pendingIntent);

            var notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
            notificationManager.Notify (0, notificationBuilder.Build());
        }
    }
}
```

Her bölüm bir göz atalım bizim `GcmListenerService` nasıl çalıştığını anlamak için. 

İlk olarak, biz açıklama `GcmListenerService` bu hizmet sistem tarafından oluşturulmasını değil ve GCM iletileri alan belirtmek için bir hedefi filtresi eklediğimiz olduğunu belirtmek için bir özniteliği olan: 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

Zaman `GcmListenerService` GCM'den, bir ileti alır `OnMessageReceived` yöntemi çağrılır. Bu yöntem geçirilen bileşeninden ileti içeriği ayıklar `Bundle`, ileti içeriği (ki bu çıktı penceresinde görüntüleyebilmeniz) kaydeder ve çağrıları `SendNotification` alınan ileti içeriği içeren yerel bir bildirim başlatmak için: 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

`SendNotification` Yöntemi kullanan `Notification.Builder` bildirim ve ardından oluşturmak için kullandığı `NotificationManager` bildirim başlatmak için. Etkili bir şekilde, bu uzak bildirim iletisi kullanıcıya sunulan için yerel bir bildirim dönüştürür.
Kullanma hakkında daha fazla bilgi için `Notification.Builder` ve `NotificationManager`, bkz: [yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications.md).


#### <a name="declare-the-receiver-in-the-manifest"></a>Alıcı bildiriminde bildirme

Biz GCM'den iletileri almak için önce kimliğinizi Android bildirimindeki GCM dinleyicisi bildirmeniz gerekir. Düzenleyelim **AndroidManifest.xml** ve değiştirme `<application>` aşağıdaki XML bölümle: 

```xml
<application android:label="RemoteNotifications" android:icon="@drawable/Icon">
    <receiver android:name="com.google.android.gms.gcm.GcmReceiver" 
              android:exported="true" 
              android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="YOUR_PACKAGE_NAME" />
        </intent-filter>
    </receiver>
</application>
```

Yukarıdaki XML'de değiştirme *YOUR_PACKAGE_NAME* istemci uygulaması projenize için paket adı için. İzlenecek yol Örneğimizde, paket adı olan `com.xamarin.gcmexample`. 

Her ayar bu XML yaptığı bakalım:

|Ayar|Açıklama|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|Uygulamamıza yakalar ve gelen anında iletme bildirimi iletileri işleyen bir GCM alıcı uygulayan bildirir.|
|`com.google.android.c2dm.permission.SEND`|Yalnızca GCM sunucuları uygulamaya doğrudan iletiler gönderebilir bildirir.|
|`com.google.android.c2dm.intent.RECEIVE`|Uygulamamıza GCM yayın iletileri işleme reklam amaçlı Filtresi.|
|`com.google.android.c2dm.intent.REGISTRATION`|Uygulamamıza yeni kayıt hedefleri işleme reklam amaçlı filtre (diğer bir deyişle, biz bir örnek kimliği dinleme hizmeti uyguladık).|

Alternatif olarak tasarlamanız `GcmListenerService` XML'de; belirtme yerine bu öznitelikler ile burada bunları belirttiğimiz **AndroidManifest.xml** böylece kod örnekleri izlemek daha kolay olur. 


### <a name="create-a-message-sender-to-test-the-app"></a>Uygulamayı Test etmek için bir ileti göndereni oluşturma

Şimdi bir C# Masaüstü konsol uygulama projesi ekleyin ve bunu **MessageSender**. Bu konsol uygulamasını Uygulama sunucusu benzetimini yapmak için kullanacağız &ndash; bildirim iletileri göndereceğiniz **ClientApp** GCM aracılığıyla. 


#### <a name="add-the-jsonnet-package"></a>Json.NET Paketi Ekle

Bu konsol uygulamasını istemci uygulamasına gönderilecek istiyoruz bildirim iletisini içeren bir JSON yükü oluşturmakta olduğumuz. Kullanacağız **Json.NET** içinde paketini **MessageSender** GCM tarafından gerekli JSON nesnesi oluşturmak daha kolay. Visual Studio'da sağ **başvuruları > NuGet paketlerini Yönet...** ; Mac Visual Studio'da sağ **paketleri > paketleri Ekle...** . 

Şimdi arama **Json.NET** paketini ve projede yükleyin: 

[![Json.NET paketi yükleniyor](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>System.Net.Http bir başvuru ekleyin

Biz de bir başvuru eklemeniz gerekir `System.Net.Http` böylece biz örneği bir `HttpClient` GCM'ye bizim sınama iletisi göndermek için. İçinde **MessageSender** proje, sağ **başvuruları > Başvuru Ekle** ve görene kadar aşağı kaydırın **System.Net.Http**. Bir onay işareti yanına koyma **System.Net.Http** tıklatıp **Tamam**. 


#### <a name="implement-code-that-sends-a-test-message"></a>Bir sınama iletisi gönderir kodları uygulama

İçinde **MessageSender**, düzenleme **Program.cs** ve içeriğini aşağıdaki kodla değiştirin:

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json.Linq;

namespace MessageSender
{
    class MessageSender
    {
        public const string API_KEY = "YOUR_API_KEY";
        public const string MESSAGE = "Hello, Xamarin!";

        static void Main (string[] args)
        {
            var jGcmData = new JObject();
            var jData = new JObject();

            jData.Add ("message", MESSAGE);
            jGcmData.Add ("to", "/topics/global");
            jGcmData.Add ("data", jData);

            var url = new Uri ("https://gcm-http.googleapis.com/gcm/send");
            try
            {
                using (var client = new HttpClient())
                {
                    client.DefaultRequestHeaders.Accept.Add(
                        new MediaTypeWithQualityHeaderValue("application/json"));

                    client.DefaultRequestHeaders.TryAddWithoutValidation (
                        "Authorization", "key=" + API_KEY);

                    Task.WaitAll(client.PostAsync (url,
                        new StringContent(jGcmData.ToString(), Encoding.Default, "application/json"))
                            .ContinueWith(response =>
                            {
                                Console.WriteLine(response);
                                Console.WriteLine("Message sent: check the client device notification tray.");
                            }));
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Unable to send GCM message:");
                Console.Error.WriteLine(e.StackTrace);
            }
        }
    }
}
```

Yukarıdaki kodda değişiklik *YOUR_API_KEY* istemci uygulaması projenize API anahtarı. 

Bu test uygulama sunucusu GCM'ye aşağıdaki JSON biçimli iletisini gönderir:

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM, buna karşılık, istemci uygulamanız bu iletiyi iletir. Şimdi yapı **MessageSender** ve burada çalıştırabileceğimiz, komut satırından bir konsol penceresi açın.



### <a name="try-it"></a>Deneyin!

Şimdi biz istemci uygulamamıza test etmeye hazırsınız. Bir öykünücü kullanıyorsanız veya Cihazınızı GCM ile Wi-Fi iletişim kuruyorsa, Güvenlik Duvarı'nda geçmesine GCM iletileri için aşağıdaki TCP bağlantı noktalarını açmanız gerekir: 5228, 5229 ve 5230.

İstemci uygulamanızı başlatın ve çıktı penceresi izleyin. Sonra `RegistrationIntentService` başarıyla bir kayıt alan GCM belirtecine, çıktı penceresinde belirteç aşağıdaki benzeyen bir günlük çıkışı olmadan görüntülenmelidir:

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

Bu noktada istemci uygulaması uzaktan bildirim iletisini almak hazırdır. Komut satırından çalıştırma **MessageSender.exe** program istemci uygulamasına bir "Hello, Xamarin" bildirim iletisini göndermek için.
Değil henüz oluşturduysanız **MessageSender** projesi, bunu şimdi yapın.

Çalıştırmak için **MessageSender.exe** Visual Studio altında bir komut istemi açın, değiştirmek **MessageSender/bin/Debug** dizin ve doğrudan komutu çalıştırın:

```cmd
MessageSender.exe
```

Çalıştırmak için **MessageSender.exe** Mac için Visual Studio altında bir Terminal oturumu açın, değiştirmek **MessageSender/bin/Debug** dizin ve çalıştırmak için kullanım mono **MessageSender.exe** 

```bash
mono MessageSender.exe
```

Bu iletinin geri istemci uygulamanızı aşağı kaydırın ve GCM aracılığıyla yaymak bir dakika sürebilir. İletiyi başarıyla alınırsa, biz aşağıdaki çıktı penceresinde benzeyen bir çıktı göreceksiniz: 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

Ayrıca, yeni bir bildirim simgesi bildirim tepsisinde göründükten bildirim: 

[![Aygıtta Notiication simgesi görüntülenir](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

Bildirimleri görüntülemek için bildirim Tepsisi açtığınızda, bizim uzaktan bildirim görmeniz gerekir:

[![Bildirim iletisi görüntülenir](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

Tebrikler, uygulamanız kendi ilk uzaktan bildirim aldı.

Uygulama zorla durduruldu ise, GCM iletileri'nin artık alınan unutmayın. Uygulama bildirimleri zorla durduran sonra devam etmek için el ile yeniden olması gerekir. Bu Android İlkesi hakkında daha fazla bilgi için bkz: [başlatma durdurulmuş uygulamaları denetimlere](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) ve bu [yığın taşması post](http://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267). 

 
## <a name="summary"></a>Özet

Bu kılavuz uzak bildirimler bir Xamarin.Android uygulaması'nda uygulama adımlarını ayrıntılı. GCM iletişimleri için gerekli ek paketler yükleme açıklanan ve uygulama izinleri GCM sunucularına erişimi için yapılandırma konusunda açıklanmıştır. Google Play Hizmetleri'nin olup olmadığını denetlemek nasıl, kayıt hedefi hizmeti ve bir kayıt belirteci için GCM ile görüşür örnek kimliği dinleme hizmeti uygulama ve nasıl GCM dinleyici uygulanacağı gösteren kod örneği sağlanan Uzak bildirim iletilerini alan ve işleyen hizmeti. Son olarak, istemci uygulamamıza GCM aracılığıyla test bildirimleri göndermek için bir komut satırı test programı uygulanmadı. 


## <a name="related-links"></a>İlgili bağlantılar

- [GCM RemoteNotifications (örnek)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
