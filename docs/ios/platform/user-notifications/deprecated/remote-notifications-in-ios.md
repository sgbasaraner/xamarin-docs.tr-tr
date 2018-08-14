---
title: İOS anında iletme bildirimleri
description: Bu belge iOS 9 ve önceki anında iletme bildirimleri ile nasıl çalışılacağını açıklar. Bu sertifika, Apple anında iletme bildirimleri Ağ Geçidi Hizmeti (APNS) ve daha fazlası ile kaydetme açıklanır.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f11f5d1cbde0f5eae27215af8eb6544be46c0206
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "39654821"
---
# <a name="push-notifications-in-ios"></a>İOS anında iletme bildirimleri

> [!IMPORTANT]
> Bu bölümdeki bilgiler, iOS 9 ilgilidir ve önceki, burada daha önceki iOS sürümlerini desteklemek için bırakıldı. İOS 10 ve üzeri, için lütfen bkz. [Kullanıcı bildirim Framework Kılavuzu](~/ios/platform/user-notifications/index.md) hem yerel hem de uzak bildirim, bir iOS cihazında destekleme.

Anında iletme bildirimleri, kısa tutulması gereken ve yalnızca bu sunucu uygulaması için bir güncelleştirme başvurmalısınız mobil uygulamaya bildirmek için yeterli veri içerir. Örneğin, sunucu uygulaması yalnızca yeni bir e-posta geldiğinde, yeni e-posta gelmiş mobil uygulamaya bildirmek. Bildirim yeni eposta içermemesi. Uygun durumlarda mobil uygulama sonra yeni e-postaları sunucudan alması

İOS bildirimleri anında iletme Merkezi'nde olan *Apple anında iletme bildirimi Ağ Geçidi Hizmeti (APNS)*. Bu, iOS cihazları için uygulama sunucusundan yönlendirme bildirimleri sorumlu Apple tarafından sağlanan bir hizmettir.
Aşağıdaki resimde iOS anında iletme bildirimi topolojisini gösterir: ![](remote-notifications-in-ios-images/image4.png "bu görüntüyü iOS anında iletme bildirimi topolojisini gösterir")

Uzak bildirimler kendilerini: JSON biçimli biçime bağlı dizeleri ve protokolleri belirtilen [bildirim yükü](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) bölümünü [yerel ve anında iletilen bildirim Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)içinde [iOS Geliştirici belgeleri](https://developer.apple.com/devcenter/ios/index.action).

Apple APNS, iki ortam tutar: bir *korumalı alan* ve *üretim* ortam. Sanal ortam geliştirme aşamasında test etmek için tasarlanmıştır ve adresinde bulunabilir `gateway.sandbox.push.apple.com` 2195 üzerinde bir TCP bağlantı noktası. Üretim ortamına dağıtmış olan ve altında bulunabilir uygulamalarda kullanılmak üzere tasarlanmıştır `gateway.push.apple.com` 2195 üzerinde bir TCP bağlantı noktası.

## <a name="requirements"></a>Gereksinimler

Anında iletme bildirimi, APNS bir mimari tarafından dikte aşağıdaki kurallara uymanız gerekir:

-  **İleti sınırı 256 bayt** -bildirim tüm ileti boyutu 256 baytı aşmamalıdır.
-  **Hiçbir giriş onay** -APNS ile isteğe bağlı olarak bir ileti için hedeflenen alıcı yapılan herhangi bir bildirim gönderen sağlamaz. Cihazı ulaşılamaz durumdaysa ve birden çok sıralı bildirimler gönderilir, en son dışındaki tüm bildirimler kaybolur. Cihaz için yalnızca en son bildirim teslim edilir.
-  **Her uygulama güvenli bir sertifika gerektirir** -APNS ile iletişim SSL üzerinden yapılması gerekir.


## <a name="creating-and-using-certificates"></a>Oluşturma ve sertifikaları kullanma

Önceki bölümde belirtilen ortamların her birinde kendi sertifika gerektirir. Bu bölümde, bir sertifika oluşturun, bir sağlama profili ile ilişkilendirin ve ardından PushSharp ile kullanmak için bir kişisel bilgi değişimi sertifikası almak nasıl ele alınacaktır.

1.  Oluşturmak için bir sertifika iOS sağlama portalı Apple'nın Web sitesinde (soldaki uygulama kimlikleri menü öğesi bir bildirim) aşağıdaki ekran görüntüsünde gösterildiği gibi gidin:

    [![](remote-notifications-in-ios-images/image5new.png "İOS sağlama portalı elma Web sitesinde")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  Ardından, uygulama kimliklerinin bölümüne gidin ve aşağıdaki ekran görüntüsünde gösterildiği gibi yeni bir uygulama kimliği oluşturun:

    [![](remote-notifications-in-ios-images/image6new.png "Uygulama kimlikleri bölümüne gidin ve yeni bir uygulama kimliği oluşturma")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  Tıkladığınızda **+** düğme oluşturabileceksiniz uygulama kimliği, açıklama ve bir paket grubu tanımlayıcısı girmek sonraki ekran görüntüsünde gösterildiği gibi:

    [![](remote-notifications-in-ios-images/image7new.png "Uygulama kimliği için açıklama ve bir paket grubu tanımlayıcısı girin")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Seçtiğinizden emin olun **açık uygulama kimliği** ve paket tanımlayıcısı ile bitmiyor bir `*` . Bu, birden çok uygulama için iyi bir tanımlayıcı oluşturur ve tek bir uygulama için anında iletme bildirimi sertifikaların olması gerekir.

1. Uygulama Hizmetleri altında seçin **anında iletme bildirimleri**:

    [![](remote-notifications-in-ios-images/image8new.png "Anında iletme bildirimleri seçin")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. Basın **Gönder** kaydı yeni uygulama Kimliğini onaylamak için:

    [![](remote-notifications-in-ios-images/image9new.png "Yeni uygulama Kimliğini kaydı Onayla")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  Ardından, uygulama kimliği için sertifika oluşturmalısınız. Sol gezinti bölmesindeki göz atın **sertifikaları > tüm** seçip `+` düğmesi, aşağıdaki ekran görüntüsünde gösterildiği gibi:

    [![](remote-notifications-in-ios-images/image10new.png "Uygulama Kimliği sertifikası oluşturma")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  Geliştirme veya üretim sertifikası kullanmak mı istediğinizi seçin:

    [![](remote-notifications-in-ios-images/image11new.png "Geliştirme veya üretim sertifikası seçin")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. ' İ tıklatın ve ardından yeni oluşturduğumuz yeni uygulama Kimliğini seçin:

    [![](remote-notifications-in-ios-images/image12new.png "Yeni oluşturduğunuz yeni uygulama Kimliğini seçin")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  Bu oluşturma işlemi boyunca sürer yönergeleri görüntüler bir *sertifika imzalama isteği* kullanarak **Anahtarlık erişimi** mac'inizde uygulama

7.  Sertifika oluşturulduktan sonra bu yapı işleminin bir parçası olarak APNs ile birlikte kaydedebilir, böylece uygulamayı imzalamak için kullanılmalıdır. Bu, oluşturma ve sertifikayı kullanan bir sağlama profili yükleme gerektirir.

8.  Bir geliştirme sağlama profili oluşturmak için gidin **sağlama profilleri** bölümünde ve biraz önce oluşturduğumuz uygulama kimliğini kullanarak oluşturmak için adımları izleyin.

9.  Sağlama profili oluşturduktan sonra açık **Xcode Düzenleyicisi** ve yenileyin. Sağlama profili oluşturduysanız bu profili iOS sağlama portalı indirip el ile içeri aktarmak gerekli olabilir görünmüyor. Aşağıdaki ekran görüntüsünde sağlama profili ile eklenen Düzenleyici örneği gösterilmektedir:

    [![](remote-notifications-in-ios-images/image13new.png "Bu ekran görüntüsü Düzenleyici örneği eklenen sağlama profili ile gösterilmektedir.")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  Bu noktada biz bunu yeni sağlama profili oluşturulmuş kullanmak için bir Xamarin.iOS projesi yapılandırmanız gerekir. Bu yapılır **proje seçenekleri** iletişim altında **iOS paket grubu imzalama** sekmesinde, aşağıdaki ekran görüntüsünde gösterildiği gibi:

    [![](remote-notifications-in-ios-images/image11.png "Bu yeni sağlama profili oluşturulmuş kullanmak için bir Xamarin.iOS projesi yapılandırma")](remote-notifications-in-ios-images/image11.png#lightbox)

Bu noktada uygulama, anında iletme bildirimleri ile çalışacak şekilde yapılandırılmıştır. Ancak hala var. sertifikayla gereken birkaç adım daha. Bu sertifikanın bir kişisel bilgi değişimi (PKCS12) sertifika gerektiren PushSharp ile uyumlu olmayan DER biçiminde olduğundan. Sertifika PushSharp tarafından kullanılabilir şekilde dönüştürmek için son aşağıdaki adımları gerçekleştirin:

1.  **Sertifika dosyası indirme** -iOS hazırlama portalı oturum açma seçtiğiniz sertifikalar sekmesi, seçtiğiniz ve doğru bir sağlama profili ile ilişkili sertifika seçin **indirme** .
1.  **Anahtarlık erişimi açın** -bu uygulamasıdır OS X parolası yönetim sisteminde bir GUI arabirimidir.
1.  **Sertifika içeri aktarma** - abonelik zaten yüklü değilse, **dosya... Öğeleri içeri aktarma** Anahtarlık erişimi menüsünde. Yukarıda verilen sertifikayı gidin ve seçin.
1.  **Sertifika dışarı aktarma** - sertifika ilişkili özel anahtarı görünür olacak şekilde genişletin, anahtara sağ tıklayın ve dışarı aktarma seçtiniz. Bir dosya adı ve dışarı aktarılan dosya için bir parola istenir.

Bu noktada sertifikalarla hazırız. Biz iOS uygulamalarını imzalamak için kullanılan bir sertifika oluşturduktan ve bu sertifikanın PushSharp ile bir sunucu uygulamasında kullanılabilen bir biçime dönüştürülür. Sonraki iOS uygulamaları APNS ile nasıl etkileşime konumunda göz atalım.

## <a name="registering-with-apns"></a>APNS ile kaydetme

Önce bir iOS uygulaması APNS ile birlikte kaydetmelisiniz uzak bildirim alabilir. APNS benzersiz cihaz belirtecini oluşturmak ve, iOS uygulamasına dönün. İOS uygulama sonra cihaz belirteci alın ve ardından kendisini uygulama sunucusuna kaydedin. Tüm Bu gerçekleştikten sonra kayıt işlemi tamamlandıktan ve uygulama sunucusu mobil aygıta anında iletme bildirimleri.

Uygulamada bu, genellikle olmaz ancak teorik olarak, bir iOS uygulamasına APNS ile kendisini kaydeder her zaman cihaz belirteci değişebilir. Bir iyileştirme uygulamanın en son cihaz belirteci önbellek ve değiştirdiğinizde, yalnızca güncelleştirme uygulama sunucusu. Aşağıdaki diyagramda kayıt ve cihaz belirteci alma işlemi gösterilmektedir:

 ![](remote-notifications-in-ios-images/image12.png "Bu diyagram kayıt ve cihaz belirteci alma işlemi gösterilmektedir.")

APNS ile kayıt işlenir `FinishedLaunching` uygulama temsilci sınıfının çağırma yöntemi `RegisterForRemoteNotificationTypes` geçerli `UIApplication` nesne. Bir iOS uygulamasına APNS'ye kaydeder olduğunda ne tür bir uzak bildirimler almak istersiniz da belirtmelisiniz. Bu uzak bildirim türleri sabit listesinde bildirilen `UIRemoteNotificationType`. Aşağıdaki kod parçacığı, uzak bir uyarı ve gösterge bildirimleri almak için bir iOS uygulamasına nasıl kaydedebilirsiniz, bir örnek verilmiştir:

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

Yanıt alındığında APNS kayıt isteğini - arka planda gerçekleşir iOS yöntemini çağıracaksınız `RegisteredForRemoteNotifications` içinde `AppDelegate` sınıfı ve kayıtlı cihaz belirteci geçirin. Belirteç içindeki bir `NSData` nesne. Aşağıdaki kod parçacığı, sağlanan bu APNS cihaz belirtecini almayı gösterir:

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

İOS kaydını (gibi cihaz bağlı değil Internet'e) herhangi bir nedenle başarısız olursa, çağıran `FailedToRegisterForRemoteNotifications` uygulamayı sınıfı temsilci. Aşağıdaki kod parçacığını nasıl kullanıcıya kayıt başarısız bildiren bir uyarı görüntüleneceğini gösterir:

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Cihaz belirteci temizlik

Cihaz belirteçleri süresi dolacak veya zamanla değişir. Bu olgu nedeniyle sunucu uygulamaları merkezi temizleme yapmak ve bu süresi dolmuş veya değiştirilmiş belirteçleri temizlemek gerekir. Bir uygulama süresi dolmuş bir belirtece sahip bir cihaza anında iletme bildirimi olarak gönderir, APNS kaydedebilir ve süresi dolan belirtecini kaydetme. Sunucuları, sonra APNS hangi belirteçleri dolmuş bulmak için sorgu.

APNS sağlamak için kullanılan bir *geri bildirim hizmeti* -ne belirteçlerin süresi dolmuş hakkında anında iletme bildirimleri ve gönderen geri veri göndermek için oluşturulan sertifika aracılığıyla kimlik doğrulaması gerçekleştiren bir HTTPS uç noktası. Bu olduğundan Apple tarafından kullanım dışı ve kaldırıldı.

Bunun yerine, daha önce geri bildirim hizmeti tarafından bildirilen durum için yeni bir HTTP durum kodu yok:

> 410 - cihaz belirteci artık konu için etkin değil.

Ayrıca, yeni `timestamp` yanıt gövdesinde JSON veri anahtarını olacaktır:

> Değer: durum üstbilgisi 410, bu anahtarın değeri, APNs onaylanan cihaz belirteci artık geçerli konunun son süresi.
>
> Sağlayıcınız ile sonraki bir zaman damgasına sahip bir belirteç cihazı kaydeder kadar bildirimler gönderme durdurun.

## <a name="summary"></a>Özet

Bu bölümde iOS anında iletme bildirimleri çevreleyen temel kavramları tanıtır. Bu, Apple anında iletilen bildirim Ağ Geçidi Hizmeti (APNS) rolü açıklanmıştır. Sonra APNS için gerekli olan güvenlik sertifikaların kullanımını ve oluşturma ele. Son olarak bu belgenin tamamlandı uygulama sunucuları nasıl kullanabileceğiniz hakkında ayrıntılı bilgi ile *geri bildirim Hizmetleri* izlemeyi durdurmak için cihaz belirteçleri süresi doldu.


## <a name="related-links"></a>İlgili bağlantılar

- [Yerel ve uzak bildirimler (örnek) gösteren bildirimler-](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [Yerel ve anında iletme bildirimleri geliştiricileri için](https://developer.apple.com/notifications/)
- [Uıapplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
