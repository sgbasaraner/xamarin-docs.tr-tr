---
title: "Azure Mobile Apps anında iletme bildirimleri gönderme"
description: "Azure bildirim hub'ları, mobil anında iletme bildirimleri herhangi arka ucundan mobil herhangi bir platform için farklı platform bildirim sistemleri ile iletişim kurmak zorunda kalmadan bir arka uç karmaşıklığını ortadan sırasında göndermek için ölçeklenebilir bir gönderim altyapısı sağlar. Bu makalede, Azure Notification Hubs bir Xamarin.Forms uygulaması için bir Azure Mobile Apps örnekten anında iletme bildirimleri göndermek için nasıl kullanılacağı açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: f0f767179a9280d7a6c6d7ce8125696d5e664cba
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Azure Mobile Apps anında iletme bildirimleri gönderme

_Azure bildirim hub'ları, mobil anında iletme bildirimleri herhangi arka ucundan mobil herhangi bir platform için farklı platform bildirim sistemleri ile iletişim kurmak zorunda kalmadan bir arka uç karmaşıklığını ortadan sırasında göndermek için ölçeklenebilir bir gönderim altyapısı sağlar. Bu makalede, Azure Notification Hubs bir Xamarin.Forms uygulaması için bir Azure Mobile Apps örnekten anında iletme bildirimleri göndermek için nasıl kullanılacağı açıklanmaktadır._

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure bildirim hub'ı ve Xamarin.Forms, göre anında [Xamarin Üniversitesi](https://university.xamarin.com/)**

Anında iletme bildirimi, bir uygulamaya bir mobil cihazda uygulama katılımı ve kullanımı artırmak için bir arka uç sistemden bir ileti gibi bilgiler sunmak için kullanılır. Bile kullanıcının etkin bir şekilde hedeflenen uygulama kullanmadığında bildirim sırasında her zaman, gönderilebilir.

Arka uç sistemleri anında iletme bildirimleri mobil cihazlara Platform bildirim sistemlerinin (PNS) aracılığıyla aşağıdaki çizimde gösterildiği gibi gönderin:

[![](azure-images/pns.png "Platform bildirim sistemleri")](azure-images/pns-large.png#lightbox "Platform bildirim sistemleri")

Anında iletme bildirimi göndermek için arka uç sistem için bir istemci uygulaması örneğini bildirim göndermek için platforma özgü PNS'ye bağlanır. Platformlar arası anında iletme bildirimleri gereklidir, arka uç her platforma özgü PNS API ve protokolü kullanmanız gerekir çünkü bu önemli ölçüde arka uç karmaşıklığını artırır.

Azure bildirim hub'ları bu karmaşıklık farklı platform bildirim sistemleri ayrıntılarını özetleyen tarafından aşağıdaki çizimde gösterildiği gibi tek bir API çağrısı ile gönderilmek üzere bir platformlar arası bildirim izin vererek kaldırın:

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

Anında iletme bildirimi göndermek için arka uç sistemi yalnızca Kişiler Azure bildirim hangi sırayla farklı platform bildirim sistemleri ile iletişim kurar, bu nedenle arka uç karmaşıklığını azaltmak hub'ı, bu gönderir anında iletme bildirimleri kodu.

Azure Mobile Apps bildirim hub'ları kullanarak anında iletme bildirimleri için yerleşik destek vardır. Xamarin.Forms uygulaması için bir Azure Mobile Apps örnekten bir anında iletme bildirimi gönderme işlem aşağıdaki gibidir:

1. Xamarin.Forms uygulaması bir işleyici döner PNS ile kaydeder.
1. Azure Mobile Apps örnek hedeflenecek aygıt belirten bir bildirim kendi Azure bildirim Hub'ına gönderir.
1. Azure bildirim hub'ı cihaz için uygun PNS bildirim gönderir.
1. PNS belirtilen aygıta bildirim gönderir.
1. Xamarin.Forms uygulaması bildirim işler ve görüntüler.

Örnek uygulama verileri bir Azure Mobile Apps örneğinde depolanan bir Yapılacaklar listesi uygulaması gösterir. Azure Mobile Apps örneğine eklenen yeni bir öğe her zaman bir anında iletme bildirimi Xamarin.Forms uygulamaya gönderilir. Aşağıdaki ekran görüntüleri alınan anında iletme bildirimi görüntüleme her platform göster:

[![](azure-images/screenshots.png "Örnek bir anında iletme bildirimi alma uygulama")](azure-images/screenshots-large.png#lightbox "örnek uygulama anında bildirim alma")

Azure Notification Hubs hakkında daha fazla bilgi için bkz: [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/) ve [Xamarin.Forms uygulamanıza anında iletme bildirimleri ekleme](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/).

## <a name="azure-and-platform-notification-system-setup"></a>Azure ve Platform bildirim sistemi kurulumu

Bir Azure bildirim hub'ı bir Azure Mobile Apps örneğine tümleştirmek için işlem aşağıdaki gibidir:

1. Azure Mobile Apps örneği oluşturun. Daha fazla bilgi için bkz: [Azure mobil uygulaması tüketen](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Bildirim hub'ı yapılandırın. Daha fazla bilgi için bkz: [bir bildirim hub'ı yapılandırma](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#create-hub).
1. Anında iletme bildirimleri göndermek için Azure Mobile Apps örneği güncelleştirin. Daha fazla bilgi için bkz: [anında iletme bildirimleri göndermek için sunucu projesi güncelleştirme](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications).
1. Her PNS ile kaydedin.
1. Bildirim hub'ı her PNS ile iletişim kurmak için yapılandırın.

Aşağıdaki bölümler, her platform için ek kurulum yönergeleri sağlar.

### <a name="ios"></a>iOS

Apple anında iletilen bildirim servisi (APNS) bir Azure bildirim Hub'ndan kullanmak için aşağıdaki ek adımları gerçekleştirilebilmesi gerekir:

1. Sertifika imzalama isteği için anında iletme sertifikanın Anahtarlık erişimi aracıyla oluşturur. Daha fazla bilgi için bkz: [bildirim sertifikası için sertifika imzalama isteği dosyası oluşturma](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate) Azure Belge Merkezi'nde üzerinde.
1. Apple Geliştirici Merkezi'ndeki anında iletme bildirimi desteği Xamarin.Forms uygulamayı kaydedin. Daha fazla bilgi için bkz: [anında iletme bildirimleri için uygulamanızı kaydetme](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications) Azure Belge Merkezi'nde üzerinde.
1. Bir anında iletme bildirimleri etkin sağlama profili Xamarin.Forms uygulaması için Apple Geliştirici Merkezi'nde oluşturun. Daha fazla bilgi için bkz: [uygulama için bir sağlama profili oluşturun](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app) Azure Belge Merkezi'nde üzerinde.
1. Bildirim hub'ı APNS ile iletişim kurmak için yapılandırın. Daha fazla bilgi için bkz: [APNS için bildirim hub'ı yapılandırma](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns).
1. Xamarin.Forms uygulamayı yeni bir uygulama kimliği ve sağlama profili kullanacak şekilde yapılandırın. Daha fazla bilgi için bkz: [Xamarin Studio'da iOS projesi yapılandırma](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio) veya [iOS projesi Visual Studio'da yapılandırma](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio) Azure Belge Merkezi'nde üzerinde.

### <a name="android"></a>Android

Firebase bulut Mesajlaşma (FCM) bir Azure bildirim Hub'ndan kullanmak için aşağıdaki ek adımları gerçekleştirilebilmesi gerekir:

1. FCM kaydolun. Bir sunucu API anahtarı ve bir istemci kimliği olan otomatik olarak oluşturulur ve paketlenmiş bir `google-services.json` indirilen dosya. Daha fazla bilgi için bkz: [etkinleştirmek Firebase bulut Mesajlaşma (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm).
1. Bildirim hub'ı FCM ile iletişim kurmak için yapılandırın. Daha fazla bilgi için bkz: [Mobile Apps geri bitiş FCM kullanarak anında iletme istekleri göndermek için yapılandırma](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm).

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Bir Azure bildirim Hub'den Windows bildirim Hizmeti'ni (WNS) kullanmak için aşağıdaki ek adımları gerçekleştirilebilmesi gerekir:

1. Windows bildirim Hizmeti'ni (WNS) kaydolun. Daha fazla bilgi için bkz: [WNS ile birlikte Windows uygulamanızı anında iletme bildirimleri için kaydetme](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns) Azure Belge Merkezi'nde üzerinde.
1. Bildirim hub'ı WNS ile iletişim kurmak için yapılandırın. Daha fazla bilgi için bkz: [WNS için bildirim hub'ı yapılandırma](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns) Azure Belge Merkezi'nde üzerinde.

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Xamarin.Forms uygulamaya anında iletme bildirimi desteği ekleme

Aşağıdaki bölümlerde her platforma özgü projesinde anında iletme bildirimlerini desteklemek için gereken uygulama açıklanmaktadır.

### <a name="ios"></a>iOS

Bir iOS uygulaması anında iletme bildirimi destek uygulamaya yönelik işlem aşağıdaki gibidir:

1. Kayıt ile Apple anında iletilen bildirim servisi (APNS) içinde `AppDelegate.FinishedLaunching` yöntemi. Daha fazla bilgi için bkz: [Apple anında iletilen bildirim sistemi ile kaydetme](#ios_register).
1. Uygulama `AppDelegate.RegisteredForRemoteNotifications` kayıt yanıtı işlemek için yöntem. Daha fazla bilgi için bkz: [kayıt yanıtı işleme](#ios_registration_response).
1. Uygulama `AppDelegate.DidReceiveRemoteNotification` gelen anında iletme bildirimleri işlemek için yöntem. Daha fazla bilgi için bkz: [gelen anında iletme bildirimleri işleme](#ios_process_incoming).

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Apple anında bildirim hizmeti ile kaydetme

Bir iOS uygulaması anında iletme bildirimleri almak için önce ile Apple anında bildirim hizmeti (bir benzersiz cihaz belirteci oluştur ve uygulamaya döndürür APNS), kaydetmeniz gerekir. Kayıt çağrıldığında `FinishedLaunching` geçersiz kılması `AppDelegate` sınıfı:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    ...
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert, new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ...
}
```

Bir iOS uygulaması APNS ile kaydettiğinde anında iletme bildirimleri almak istediğiniz türlerini belirtmeniz gerekir. `RegisterUserNotificationSettings` Yöntemi kaydeder uygulama alabilir, bildirim türleri ile `RegisterForRemoteNotifications` yöntemi APNS anında iletme bildirimleri almak için kaydediliyor.

> [!NOTE]
> Çağrı başarısız `RegisterUserNotificationSettings` yöntemi sessizce uygulama tarafından alınan anında iletme bildirimleri neden olur.

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>Kayıt yanıtı işleme

APNS kayıt isteğini arka planda gerçekleşir. Yanıt alındığında iOS çağıracak `RegisteredForRemoteNotifications` geçersiz kılması `AppDelegate` sınıfı:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyAPNS}
    };

    // Register for push with the Azure mobile app
    Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
    push.RegisterAsync(deviceToken, templates);
}
```

Bu yöntem, JSON olarak basit bildirim iletisi şablonu oluşturur ve bildirim hub'ından şablon bildirimleri almak için cihaz kaydeder.

> [!NOTE]
> `FailedToRegisterForRemoteNotifications` Geçersiz kılma uygulanan ağ bağlantısının olmaması gibi durumlarla işlemek için. Kullanıcıların uygulama çalışırken başlayabilir bu önemlidir, çünkü çevrimdışı.

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>Gelen anında iletme bildirimleri işleme

`DidReceiveRemoteNotification` Geçersiz kılması `AppDelegate` sınıfı uygulama çalışıyorsa ve bir bildirim alındığında çağrılır gelen anında iletme bildirimleri işlemek için kullanılır:

```csharp
public override void DidReceiveRemoteNotification(
    UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
    NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

    string alert = string.Empty;
    if (aps.ContainsKey(new NSString("alert")))
        alert = (aps[new NSString("alert")] as NSString).ToString();

    // Show alert
    if (!string.IsNullOrEmpty(alert))
    {
      var notificationAlert = UIAlertController.Create("Notification", alert, UIAlertControllerStyle.Alert);
      notificationAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Cancel, null));
      UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(notificationAlert, true, null);
    }
}
```

`userInfo` Sözlüğünü içeren `aps` anahtar değeri, `alert` kalan bildirim veri sözlüğü. Bu sözlük alınır, ile `string` iletişim kutusunda görüntülenen bildirim iletisini.

> [!NOTE]
> Anında iletme bildirimi geldiğinde uygulama çalışmıyorsa, uygulama başlatılacak ancak `DidReceiveRemoteNotification` yöntemi bildirim işlem olmaz. Bunun yerine, bildirim yükü almak ve uygun şekilde gelen yanıt `WillFinishLaunching` veya `FinishedLaunching` geçersiz kılar.

APNS hakkında daha fazla bilgi için bkz: [iOS anında iletme bildirimleri](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md).

### <a name="android"></a>Android

Android uygulamada anında iletme bildirimi destek uygulamaya yönelik işlem aşağıdaki gibidir:

1. Ekleme [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet paketini Android projesine ve uygulamanın hedef sürüm Android 7.0 veya üzeri ayarlayın.
1. Ekleme `google-services.json` Firebase konsolundan Android projesi kökündeki indirilir ve yapı eylemi kümesine dosya **GoogleServicesJson**. Daha fazla bilgi için bkz: [Google Hizmetleri JSON dosyası ekleme](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).
1. Android bildirimindeki alıcı bildirme tarafından register Firebase bulut Mesajlaşma (FCM) ile dosya ve uygulama tarafından `FirebaseRegistrationService.OnTokenRefresh` yöntemi. Daha fazla bilgi için bkz: [Firebase bulut Mesajlaşma ile kaydetme](#android_register_fcm).
1. Azure bildirim hub'ı kaydolduğunuz `AzureNotificationHubService.RegisterAsync` yöntemi. Daha fazla bilgi için bkz: [Azure bildirim Hub'ıyla kayıt](#android_register_azure).
1. Uygulama `FirebaseNotificationService.OnMessageReceived` gelen anında iletme bildirimleri işlemek için yöntem. Daha fazla bilgi için bkz: [anında iletilen bildirim içeriğini görüntüleme](#android_displaying_notification).

Bulut Firebase Mesajlaşma hakkında daha fazla bilgi için bkz: [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) ve [Firebase bulut Mesajlaşma ile uzaktan bildirimleri](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>Firebase ile kaydetme bulut Mesajlaşma

Bir Android uygulaması anında iletme bildirimleri almak için önce bir kayıt belirteç oluşturmak ve uygulamaya dönmek FCM ile kaydetmeniz gerekir. Kayıt belirteçleri hakkında daha fazla bilgi için bkz: [FCM kaydı](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration).

Bu, tarafından gerçekleştirilir:

- Android bildirimindeki alıcı bildirme. Daha fazla bilgi için bkz: [Android bildirim içinde alıcı bildirme](#declaring_a_receiver).
- Firebase örnek kimliği hizmetini uygulama. Daha fazla bilgi için bkz: [Firebase örnek kimliği hizmeti uygulama](#implementing-firebase-instance-id-service).

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Android bildirimindeki alıcı bildirme

Düzen **AndroidManifest.xml** ve aşağıdaki Ekle `<receiver>` elemanlara `<application>` öğe:

```xml
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    <category android:name="${applicationId}" />
  </intent-filter>
</receiver>
```

Bu XML aşağıdaki işlemleri gerçekleştirir:

- Bir iç bildirir `FirebaseInstanceIdInternalReceiver` hizmetler güvenli bir şekilde başlatmak için kullanılan uygulama.
- Bildiren bir `FirebaseInstanceIdReceiver` uygulamasında, her bir uygulama örneği için benzersiz bir tanımlayıcı sağlar. Bu alıcı, ayrıca kimliğini doğrular ve Eylemler yetkilendirir.

`FirebaseInstanceIdReceiver` Alan `FirebaseInstanceId` ve `FirebaseMessaging` olayları ve türetilmiş sınıf teslim `FirebasesInstanceIdService`.

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Firebase örnek kimliği hizmeti uygulama

Uygulama ile FCM kaydetme gerçekleştirilir öğesinden bir sınıf türetme tarafından `FirebaseInstanceIdService` sınıfı. Bu sınıf, FCM erişmek için istemci uygulaması yetkilendirmek güvenlik belirteçleri oluşturmak için sorumludur. Örnek uygulamasında `FirebaseRegistrationService` sınıfı türer `FirebaseInstanceIdService` sınıfı ve aşağıdaki kod örneğinde gösterilir:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    const string TAG = "FirebaseRegistrationService";

    public override void OnTokenRefresh()
    {
        var refreshedToken = FirebaseInstanceId.Instance.Token;
        Log.Debug(TAG, "Refreshed token: " + refreshedToken);
        SendRegistrationTokenToAzureNotificationHub(refreshedToken);
    }

    void SendRegistrationTokenToAzureNotificationHub(string token)
    {
        // Update notification hub registration
        Task.Run(async () =>
        {
            await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
        });
    }
}
```

`OnTokenRefresh` Yöntemi uygulama kaydı belirteci FCM aldığında çağrılır. Belirteçten yöntemi alır `FirebaseInstanceId.Instance.Token` tarafından FCM zaman uyumsuz olarak güncelleştirilen özelliği. `OnTokenRefresh` Yöntemi seyrek çağrılır, uygulamanın yüklenmesi veya kaldırılması uygulama örnek kimliği siler, kullanıcı uygulama verileri sildiğinde, belirteç yalnızca güncelleştirildiğinden ya da güvenlik belirtecinin bırakıldı güvenliği aşılmış. Ayrıca, uygulamanın kendi belirtecini düzenli aralıklarla, genellikle 6 ayda yenileme FCM örnek kimliği hizmet ister.

`OnTokenRefresh` Yöntemini de çağırır `SendRegistrationTokenToAzureNotificationHub` Azure bildirim Hub'ıyla kullanıcının kayıt belirtecini ilişkilendirmek için kullanılan yöntem.

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>Azure bildirim hub'ı ile kaydetme

`AzureNotificationHubService` SAX `RegisterAsync` kullanıcının kayıt belirtecini Azure bildirim Hub'ıyla ilişkilendirir yöntemi. Aşağıdaki örnekte gösterildiği kod `RegisterAsync` tarafından çağrılan yöntem `FirebaseRegistrationService` sınıfı kullanıcının kayıt belirteci değiştiğini:

```csharp
public class AzureNotificationHubService
{
    const string TAG = "AzureNotificationHubService";

    public static async Task RegisterAsync(Push push, string token)
    {
        try
        {
            const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBody}
            };

            await push.RegisterAsync(token, templates);
            Log.Info("Push Installation Id: ", push.InstallationId.ToString());
        }
        catch (Exception ex)
        {
            Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
        }
    }
}
```

Bu yöntem, JSON ve kayıtları Firebase kayıt belirteci kullanarak bildirim hub'ından, şablon bildirimleri almak için basit bildirim iletisi şablonu oluşturur. Bu, Azure bildirim Hub'ından gönderilen bildirimlere kayıt belirtecinizi tarafından temsil edilen aygıt hedeflediğini sağlar.

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>Anında iletme bildirimi içeriğini görüntüleme

Anında iletme bildirimi içeriğini görüntüleme elde edilir öğesinden bir sınıf türetme tarafından `FirebaseMessagingService` sınıfı. Bu sınıf geçersiz kılınabilir içerir `OnMessageReceived` uygulama FCM bir bildirim aldığında çağrılır, yöntemi, sağlanan uygulama ön planda çalışıyor. Örnek uygulamasında `FirebaseNotificationService` sınıfı türer `FirebaseMessagingService` sınıfı ve aşağıdaki kod örneğinde gösterilir:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseNotificationService : FirebaseMessagingService
{
    const string TAG = "FirebaseNotificationService";

    public override void OnMessageReceived(RemoteMessage message)
    {
        Log.Debug(TAG, "From: " + message.From);

        // Pull message body out of the template
        var messageBody = message.Data["message"];
        if (string.IsNullOrWhiteSpace(messageBody))
            return;

        Log.Debug(TAG, "Notification message body: " + messageBody);
        SendNotification(messageBody);
    }

    void SendNotification(string messageBody)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
            .SetContentTitle("New Todo Item")
            .SetContentText(messageBody)
            .SetContentIntent(pendingIntent)
            .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
            .SetAutoCancel(true);

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }
}
```

Uygulama FCM bir bildirim aldığında `OnMessageReceived` yöntemi içerik ileti ayıklar ve çağırır `SendNotification` yöntemi. Bu yöntem uygulama çalışırken bildirim alanında görünen bildirim başlatılır yerel bir bildirim iletisi içeriği dönüştürür.

##### <a name="handling-notification-intents"></a>Bildirim hedefleri işleme

Bir kullanıcı bir bildirim dokunur, bildirim iletisini eşlik eden herhangi bir veri içinde kullanılabilir hale getirileceğini `Intent` ek özellikler. Bu veriler, aşağıdaki kod ile ayıklanabilir:

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

Uygulamanın Başlatıcısı `Intent` bu kodu eşlik eden herhangi bir veri oturum açacak şekilde kullanıcı, bildirim iletisini dokunur durumlarda tetiklenir `Intent` çıkış penceresine.

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Bir evrensel Windows Platformu (UWP) önce uygulama Windows bildirim Hizmeti'ni (bir bildirim kanalı döndürmesi WNS ile), kaydetmelisiniz anında iletme bildirimleri alabilirsiniz. Kayıt tarafından çağrıldığında `InitNotificationsAsync` yönteminde `App` sınıfı:

```csharp
private async Task InitNotificationsAsync()
{
    var channel = await PushNotificationChannelManager
          .CreatePushNotificationChannelForApplicationAsync();

    const string templateBodyWNS =
        "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

    JObject headers = new JObject();
    headers["X-WNS-Type"] = "wns/toast";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyWNS},
        {"headers", headers} // Needed for WNS.
    };

    await TodoItemManager.DefaultManager.CurrentClient.GetPush()
          .RegisterAsync(channel.Uri, templates);
}
```

Bu yöntem, anında bildirim kanalı alır, bir bildirim iletisi şablonu JSON olarak oluşturur ve bildirim hub'ından şablon bildirimleri almak için cihaz kaydeder.

`InitNotificationsAsync` Yöntemi çağrıldıktan `OnLaunched` geçersiz kılması `App` sınıfı:

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

Bu, anında iletme bildirimi kaydı oluşturulur veya uygulama, bu nedenle WNS anında iletme kanal her zaman etkin olduğundan emin olmanın her başlatıldığında yenilenir sağlar.

Anında iletme bildirimi alındığında uygulamanın otomatik olarak görüntülenecek bir *bildirim* – iletisini içeren engelleyici olmayan bir pencere.

## <a name="summary"></a>Özet

Bu makalede Azure Notification Hubs bir Xamarin.Forms uygulaması için bir Azure Mobile Apps örnekten anında iletme bildirimleri göndermek için nasıl kullanılacağı gösterilmektedir. Azure bildirim hub'ları, mobil anında iletme bildirimleri herhangi arka ucundan mobil herhangi bir platform için farklı platform bildirim sistemleri ile iletişim kurmak zorunda kalmadan bir arka uç karmaşıklığını ortadan sırasında göndermek için ölçeklenebilir bir gönderim altyapısı sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Bir Azure mobil uygulaması kullanma](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure bildirim hub'ları](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Xamarin.Forms uygulamanıza anında iletme bildirimleri ekleme](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [İOS anında iletme bildirimleri](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
