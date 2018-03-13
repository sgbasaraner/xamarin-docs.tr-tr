---
title: "İOS anında iletme bildirimleri"
description: "Bu bölümde, iOS anında iletme bildirimlerini ele alınacaktır. Apple anında iletme bildirimleri ağ geçidi hizmeti ve iOS uygulamalarını yayımlama bildirimleri oynadığı rolü tanıtır. Güvenlik sertifikaları anında iletme bildirimlerini etkinleştirin ve tartışmak için gerekli oluşturma anlatılmıştır. Son olarak bu bölümde, uygulama sunucuları istemci mobil cihazları izlemek için gerçekleştirmeniz gerekir temizlik görevlerden bazılarını ele alınacaktır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 8e90bc3974247066a714cb44b6648a83cdb58cf5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="push-notifications-in-ios"></a>İOS anında iletme bildirimleri

_Bu bölümde, iOS anında iletme bildirimlerini ele alınacaktır. Apple anında iletme bildirimleri ağ geçidi hizmeti ve iOS uygulamalarını yayımlama bildirimleri oynadığı rolü tanıtır. Güvenlik sertifikaları anında iletme bildirimlerini etkinleştirin ve tartışmak için gerekli oluşturma anlatılmıştır. Son olarak bu bölümde, uygulama sunucuları istemci mobil cihazları izlemek için gerçekleştirmeniz gerekir temizlik görevlerden bazılarını ele alınacaktır._

> [!IMPORTANT]
> **Not:** bu bölümdeki bilgiler, iOS 9 ilgilidir ve önceki, bunu burada daha önceki iOS sürümlerini desteklemek için bırakıldı. 10 ve üzeri iOS için lütfen bkz. [Kullanıcı bildirim Framework Kılavuzu](~/ios/platform/user-notifications/index.md) hem yerel hem de uzak bildirim iOS cihazında desteklemek için.

Anında iletme bildirimleri kısa tutulması ve yalnızca mobil uygulama, sunucu uygulaması için bir güncelleştirme başvurmalıdır bildirmek için yeterli veri içerir. Örneğin, sunucu uygulaması yalnızca yeni e-posta geldiğinde yeni e-posta gelmiş mobil uygulama bildirmek. Bildirim yeni eposta içermemesi. Uygun olduğunda mobil uygulama sonra yeni e-postaları sunucudan almak

İOS bildirimleri gönderme merkezinde olan *Apple anında iletme bildirimi Ağ Geçidi Hizmeti (APNS)*. Bu, bir uygulama sunucusu yönlendirme bildirimler iOS cihazlarına sorumludur Apple tarafından sağlanan bir hizmettir.
Aşağıdaki resimde iOS anında iletme bildirimi topolojisini gösterir: ![ ] (remote-notifications-in-ios-images/image4.png "bu görüntüyü iOS anında iletme bildirimi topolojisini gösterir")

Uzak bildirimler kendilerini: JSON biçimine uyması dizeleri biçimlendirilmiş ve protokolleri belirtilen [bildirim yükü](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) bölümünü [yerel ve anında iletilen bildirim Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)içinde [iOS Geliştirici belgeleri](https://developer.apple.com/devcenter/ios/index.action).

Apple APNS için iki ortam korur: bir *korumalı alan* ve *üretim* ortamı. Korumalı alan ortamıdır geliştirme aşamasında test etmek için tasarlanmıştır ve bulunabilir `gateway.sandbox.push.apple.com` 2195 üzerinde TCP bağlantı noktası. Üretim ortamında dağıtmış olan ve bulunabilir uygulamalarında kullanılması amaçlanmıştır `gateway.push.apple.com` 2195 üzerinde TCP bağlantı noktası.

## <a name="requirements"></a>Gereksinimler

Anında iletme bildirimi APNS mimarisi tarafından dikte aşağıdaki kurallara uymanız gerekir:

-  **256 baytı ileti sınırı** -bildirim tüm ileti boyutu 256 baytı aşmamalıdır.
-  **Hiçbir giriş onay** -APNS isteğe bağlı olarak bir ileti hedeflenen alıcı yapılan herhangi bir bildirim gönderenle sağlamaz. Cihaz erişilebilir durumda değil ve birden çok sıralı bildirimler gönderilir, en son dışındaki tüm bildirimleri kaybolur. Yalnızca en son bildirim cihaza teslim edilecek.
-  **Her uygulama güvenli sertifika gerektiren** -APNS ile iletişim SSL üzerinden yapılması gerekir.


## <a name="creating-and-using-certificates"></a>Oluşturma ve sertifikaları kullanma

Her biri önceki bölümde belirtildiği ortamları, kendi sertifika gerektirir. Bu bölümde, bir sertifika oluşturmak, bir sağlama profiliyle ilişkilendirmek ve PushSharp ile kullanılmak üzere bir kişisel bilgi değişimi sertifikası almak nasıl ele alınacaktır.

1.  Oluşturmak için bir sertifika için iOS sağlama portalı Apple'nın Web sitesinde (bildirim soldaki uygulama kimlikleri menü öğesi) aşağıdaki ekran görüntüsünde gösterildiği gibi gidin:

    [![](remote-notifications-in-ios-images/image5new.png "İOS sağlama portalı elmalar Web sitesinde")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  Ardından, uygulama ID bölümüne gidin ve yeni bir uygulama kimliği aşağıdaki ekran görüntüsünde gösterildiği gibi oluşturun:

    [![](remote-notifications-in-ios-images/image6new.png "Uygulama kimlikleri bölümüne gidin ve yeni bir uygulama kimliği oluşturma")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  Tıkladığınızda  **+**  düğmesi görebilirsiniz için uygulama kimliği, açıklama ve bir paket tanımlayıcısı girmek sonraki ekran görüntüsünde gösterildiği gibi:

    [![](remote-notifications-in-ios-images/image7new.png "Uygulama kimliği için paket tanımlayıcısı ve açıklama girin")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Seçtiğinizden emin olun **açık uygulama kimliği** ve paket tanımlayıcısı ile bitmiyor bir `*` . Bu birden çok uygulama için iyi bir tanımlayıcı oluşturur ve anında iletme bildirimi sertifikalar için tek bir uygulama olmalıdır.

1. Uygulama Hizmetleri altında seçin **anında iletme bildirimleri**:

    [![](remote-notifications-in-ios-images/image8new.png "Anında iletme bildirimlerini seçin")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. Tuşuna basın **gönderme** yeni uygulama kimliği kaydı onaylamak için:

    [![](remote-notifications-in-ios-images/image9new.png "Yeni uygulama kimliği kaydı Onayla")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  Ardından, uygulama kimliği için sertifika oluşturmalısınız. Sol taraftaki gezinti bölmesinde Gözat **sertifikaları > tüm** seçip `+` düğmesi, aşağıdaki ekran görüntüsünde gösterildiği gibi:

    [![](remote-notifications-in-ios-images/image10new.png "Uygulama kimliği için sertifika oluştur")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  Bir geliştirme veya üretim sertifikası kullanmak mı istediğinizi seçin:

    [![](remote-notifications-in-ios-images/image11new.png "Bir geliştirme veya üretim sertifikası seçin")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. Ve ardından yeni oluşturduğumuz yeni uygulama Kimliğini seçin:

    [![](remote-notifications-in-ios-images/image12new.png "Yeni oluşturduğunuz yeni uygulama Kimliğini seçin")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  Bu oluşturma işlemi boyunca sürer yönergeleri görüntüler bir *sertifika imzalama isteği* kullanarak **Anahtarlık erişimi** Mac uygulaması

7.  Sertifikayı oluşturan, bu yapılandırma işleminin bir parçası olarak APNs ile kaydedebilir, uygulamayı imzalamak için kullanılmalıdır. Bu, oluşturma ve sertifika kullanan bir sağlama profili yükleme gerektirir.

8.  Bir geliştirme sağlama profili oluşturmak için gidin **sağlama profilleri** bölümünde ve yeni oluşturduğumuz uygulama kimliğini kullanarak oluşturmak için adımları izleyin.

9.  Sağlama profili oluşturduktan sonra açık **Xcode Düzenleyicisi** ve yenileyin. Sağlama profili oluşturduysanız iOS sağlama portalı profilini indirir ve el ile içeri aktarmak gerekli olabilir görünmez. Aşağıdaki ekran görüntüsünde, sağlama profili eklendi Düzenleyici örneği gösterilmektedir:

    [![](remote-notifications-in-ios-images/image13new.png "Bu ekran görüntüsü, sağlama profili eklendi Düzenleyici örneği gösterir")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  Bu noktada biz Xamarin.iOS projesi bu yeni sağlama profili oluşturulan kullanacak şekilde yapılandırmanız gerekir. Bu yapılır **proje seçenekleri** iletişim altında **iOS paket imzalama** sekmesinde, aşağıdaki ekran görüntüsü gösteren olarak:

    [![](remote-notifications-in-ios-images/image11.png "Bu yeni sağlama profili oluşturulduğunda kullanılacak Xamarin.iOS projesi yapılandırma")](remote-notifications-in-ios-images/image11.png#lightbox)



Bu noktada uygulamanın anında iletme bildirimleri ile çalışmak üzere yapılandırıldı. Ancak, yine sertifikayla gereken birkaç daha fazla adım vardır. Bu sertifikanın bir kişisel bilgi değişimi (PKCS12) sertifika gerektiren PushSharp ile uyumlu olmayan DER biçimdedir. Böylece PushSharp tarafından kullanılabilir sertifika dönüştürmek için son adımları gerçekleştirin:

1.  **Sertifika dosyasını karşıdan** -iOS sağlama portalı oturum açma seçtiğiniz sertifikalar sekmesi, doğru sağlama profili ve seçtiğiniz ilişkili sertifikayı seçin **karşıdan** .
1.  **Anahtarlık erişimi açmak** -bu uygulamadır parola yönetimi sistemi OS X için GUI arabirimidir.
1.  **Sertifika alma** - sertifika verilmiş zaten yüklü değilse, **dosya... Öğeleri içeri aktarma** Anahtarlık erişimi menüsünde. Yukarıda verilen sertifika gidin ve seçin.
1.  **Sertifika verme** - ilişkili özel anahtarı görünür olacak şekilde sertifika genişletin, anahtarını sağ tıklatın ve dışarı aktarma seçtiniz. Bir dosya adı ve dışarı aktarılan dosya için bir parola istenir.

Bu noktada biz sertifikalarla yapılır. Biz iOS uygulamalarını imzalamak için kullanılan bir sertifika oluşturduktan ve bu sertifikayı PushSharp ile bir sunucu uygulaması kullanılabilir bir biçime dönüştürülür. Sonraki iOS uygulamaları APNS ile nasıl etkileşim sırasında bakalım.

## <a name="registering-with-apns"></a>APNS ile kaydetme

Önce bir iOS uygulaması APNS ile kaydetmeniz gerekir uzak bildirimi alabilirsiniz. APNS benzersiz cihaz belirteci oluşturur ve, iOS uygulamasına döndürür. İOS uygulamasının cihaz belirteci alın ve kendisini uygulama sunucusuna kaydedin. Tüm Bu gerçekleştikten sonra kayıt işlemi tamamlandıktan ve uygulama sunucusu mobil cihaza anında iletme bildirimleri.

Uygulamada bu, genellikle olmaz ancak teorik olarak bir iOS uygulaması kendisini APNS ile kaydeder her zaman cihaz belirteci değişebilir. Bir iyileştirme bir uygulama ve en son cihaz belirteci önbelleğe değiştirdiğinizde, uygulama sunucusu yalnızca güncelleştirin. Aşağıdaki diyagramda kayıt ve cihaz belirtecini alma işlemi gösterilmektedir:

 ![](remote-notifications-in-ios-images/image12.png "Bu diyagramda kayıt ve cihaz belirtecini alma işlemi gösterilir")

Kayıt APNS ile işlenir `FinishedLaunching` yöntemini çağırarak uygulama temsilci sınıfının `RegisterForRemoteNotificationTypes` geçerli `UIApplication` nesnesi. Bir iOS uygulaması APNS ile kaydolduğunda, ne tür bir uzak bildirimleri almak ister misiniz de belirtmelisiniz. Bu uzak bildirim türleri numaralandırmada bildirilir `UIRemoteNotificationType`. Aşağıdaki kod parçacığını bir iOS uygulaması uzak rozet ve Uyarı bildirimlerini almak üzere nasıl kaydedebilirsiniz, bir örnek verilmiştir:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

    UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications ();
} else {
    UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
    UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
}
```

Yanıt alındığında APNS kayıt isteğini - arka planda gerçekleşir iOS yöntemi çağıracak `RegisteredForRemoteNotifications` içinde `AppDelegate` sınıfı ve kayıtlı cihaz belirteci geçirin. Belirteç bulunan bir `NSData` nesnesi. Aşağıdaki kod parçacığını cihaz belirteci almak sağlanan bu APNS gösterilmektedir:

```csharp
public override void RegisteredForRemoteNotifications (
UIApplication application, NSData deviceToken)
{
    // Get current device token
    var DeviceToken = deviceToken.Description;
    if (!string.IsNullOrWhiteSpace(DeviceToken)) {
        DeviceToken = DeviceToken.Trim('<').Trim('>');
    }

    // Get previous device token
    var oldDeviceToken = NSUserDefaults.StandardUserDefaults.StringForKey("PushDeviceToken");

    // Has the token changed?
    if (string.IsNullOrEmpty(oldDeviceToken) || !oldDeviceToken.Equals(DeviceToken))
    {
        //TODO: Put your own logic here to notify your server that the device token has changed/been created!
    }

    // Save new device token
    NSUserDefaults.StandardUserDefaults.SetString(DeviceToken, "PushDeviceToken");
}
```

Kayıt (gibi cihazı bağlı değil Internet'e) herhangi bir nedenle başarısız olursa, iOS çağıracak `FailedToRegisterForRemoteNotifications` uygulamanın sınıfı temsilci. Aşağıdaki kod parçacığını bir uyarı kaydı başarısız bildiren kullanıcıya görüntülemek nasıl gösterir:

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Cihaz belirteci kayıt tutma

Cihaz belirteçleri süresi dolacak veya zamanla değiştirin. Bu olgu nedeniyle bazı ev temizleme yapın ve bu süresi dolmuş ya da değiştirilmiş belirteçleri temizlemek sunucu uygulamalarını gerekir. Bir uygulama süresi dolmuş bir belirtece bir cihaza anında iletme bildirimi olarak gönderdiğinde, APNS kaydetmek ve süresi dolan belirtecini kaydedin. Sunucuları ne belirteçleri dolmuş bulmak için APNS sonra sorgulayabilir.

APNS sağlamak için kullanılan bir *geri bildirim hizmeti* -hangi belirteçleri süresi dolmuş hakkında anında iletme bildirimleri ve gönderir geri veri göndermek için oluşturulan sertifika üzerinden kimlik doğrulayan bir HTTPS uç noktası. Bu olduğundan Apple'nın kullanım ve kaldırıldı.

Bunun yerine, daha önce geri bildirim hizmeti tarafından bildirilen durum için yeni bir HTTP durum kodu vardır:

> 410 - cihaz belirteci artık konu için etkin değil.

Ayrıca, yeni `timestamp` yanıt gövdesinde JSON veri anahtarı olacaktır:

> Değer: durum üstbilgisi 410, bu anahtarın değeri, APNs onaylanıp cihaz belirteci artık konu için geçerli en son ne zaman.
>
> Cihaz sağlayıcınız ile daha sonraki bir zaman damgasına sahip bir belirteç kaydeder kadar bildirimleri göndermeyi durdurun.

## <a name="summary"></a>Özet

Bu bölüm iOS anında iletme bildirimlerini çevreleyen temel kavramları tanıtır. Apple anında iletilen bildirim Ağ Geçidi Hizmeti (APNS) rolü açıklanmıştır. Ardından oluşturma ve APNS için gerekli olan güvenlik sertifikaların kullanımını ele. Son olarak bu belgeyi tamamlandı uygulama sunucularının nasıl kullanabileceğiniz bir tartışma ile *geri bildirim Hizmetleri* izlemeyi durdurmak için cihaz belirteçleri süresi doldu.


## <a name="related-links"></a>İlgili bağlantılar

- [Yerel ve uzak bildirimler (örnek) gösteren bildirimler-](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [Yerel ve anında iletme bildirimleri geliştiricileri için](https://developer.apple.com/notifications/)
- [Uıapplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
