---
title: "Gelişmiş Kullanıcı bildirimleri"
description: "Bu makalede tüm kullanıcılara bildirim iOS 10 ve bunları bir Xamarin.iOS uygulaması nasıl kullanacağınızı tarafından geliştirilmiştir yoldan yer almaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a5dbd65cc32ed63c0fa6f8abe3a13ffee4e9df63
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="enhanced-user-notifications"></a>Gelişmiş Kullanıcı bildirimleri

_Bu makalede tüm kullanıcılara bildirim iOS 10 ve bunları bir Xamarin.iOS uygulaması nasıl kullanacağınızı tarafından geliştirilmiştir yoldan yer almaktadır._

Yeni ios'a 10, kullanıcı bildirimi teslimi ve yerel ve uzak bildirimler işleme framework olanak sağlar. Bu çerçeve kullanarak, bir uygulama ya da uygulama uzantısı yerel bildirimler teslimini konum gibi koşulları veya günün saatini belirterek zamanlayabilirsiniz.

## <a name="about-user-notifications"></a>Kullanıcı bildirimleri hakkında

Yukarıda belirtildiği gibi yeni kullanıcı bildirimi framework teslim ve yerel ve uzak bildirimler işleme olanak sağlar. Bu çerçeve kullanarak, bir uygulama ya da uygulama uzantısı yerel bildirimler teslimini konum gibi koşulları veya günün saatini belirterek zamanlayabilirsiniz.

Ayrıca, uygulama veya uzantı alabilir (ve büyük olasılıkla değiştirmek) hem yerel hem de uzak bildirimler kullanıcının iOS cihazına teslim edilir.

Yeni Kullanıcı bildirim UI framework bir uygulama ya da uygulama kullanıcıya sunulduğunda hem yerel hem de uzak bildirimler görünümünü özelleştirmek için uzantı sağlar.

Bu çerçeve, bir uygulama bir kullanıcıya bildirimleri teslim aşağıdaki yöntemleri sağlar:

- **Görsel uyarılar** - burada bildirim aşağı dökümünü başlık olarak ekranın üstünde yapar.
- **Ses ve Vibrations** -bir bildirim ile ilişkili olabilir.
- **Uygulama simgesi Badging** - burada uygulamanın simgesi yeni içerik okunmamış e-posta iletilerinin sayısı gibi kullanılabilir olduğunu gösteren bir gösterge görüntüler.

Ayrıca, kullanıcının geçerli bağlam bağlı olarak, bir bildirim sunulan farklı yolu vardır:

- Cihaz kilidi ise bildirim aşağı ekranın üst kısmından bir başlık dökümünü yapar.
- Cihaz kilitli bildirim kullanıcının kilit ekranında görüntülenir.
- Kullanıcıya bir bildirim eksik olduğunu, bildirim merkezi açın ve kullanılabilir tüm bekleyen bildirimler görüntüleyin.

Bir Xamarin.iOS uygulaması gönderebilmesi için kullanıcı bildirimleri iki tür vardır:

- **Yerel bildirimler** -bu kullanıcıların cihazda yerel olarak yüklü uygulamalar tarafından gönderilir.
- **Uzak bildirimler** -bir uzaktan gönderilen sunucusu ve ya da kullanıcıya sunulan veya uygulamanın içeriğinin bir arka plan güncelleştirmesi tetikler.

### <a name="about-local-notifications"></a>Yerel bildirimler hakkında

Bir iOS uygulaması gönderebilirsiniz yerel bildirimleri, aşağıdaki özellikler ve öznitelikleri vardır:

- Kullanıcının cihazda yerel uygulamalar tarafından gönderilir. 
- Bunlar yapılandırılabilir tabanlı Tetikleyicileri zaman veya konum kullanın. 
- Uygulama bildirimi kullanıcının aygıtla zamanlar ve Tetikleme koşulu karşılandığında görüntülenir.
- Kullanıcı bir bildirim ile etkileşim, uygulama bir geri çağırma alır.

Yerel bildirimler bazı örnekler şunlardır:

- Takvim uyarıları
- Anımsatıcı uyarıları
- Konum kullanan Tetikleyicileri

Daha fazla bilgi için lütfen Apple'nın bkz [yerel ve uzak bildirim Programlama Kılavuzu](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) belgeleri.

### <a name="about-remote-notifications"></a>Uzak bildirimler hakkında

Bir iOS uygulaması gönderebilirsiniz uzak bildirimler aşağıdaki özellikler ve özniteliklere sahiptir:

- Uygulama ile iletişim kuran bir sunucu tarafı bileşeni vardır.
- Apple anında iletilen bildirim servisi (APNs), uzak bildirimler en yüksek çaba teslimini geliştiricinin bulut tabanlı sunucularından kullanıcının cihaza iletmek için kullanılır.
- Uygulama uzak bildirimi aldığında kullanıcıya görüntülenir.
- Kullanıcı bildirim ile etkileşim, uygulama bir geri çağırma alır.

Uzak bildirimler bazı örnekleri şunlardır:

- Haber uyarıları
- Spor güncelleştirmeleri
- Anlık Mesajlaşma iletileri

Bir iOS uygulaması için kullanılabilen uzak bildirimler iki tür vardır:

- **Kullanıcı yönelik** -bu cihazdaki kullanıcı için görüntülenir.
- **Sessiz güncelleştirmeleri** -bunlar arka planda bir iOS uygulaması içeriğini güncelleştirmek için bir mekanizma sağlar. Sessiz güncelleştirme alındığında, uygulamanın son içerik Kaldır sunucuları açılır ulaşmak.

Daha fazla bilgi için lütfen Apple'nın bkz [yerel ve uzak bildirim Programlama Kılavuzu](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) belgeleri.

### <a name="about-the-existing-notifications-api"></a>Varolan hakkında bildirimler API

10 iOS önce bir iOS uygulamasını kullanırsınız `UIApplication` bir bildirim sistemi ile kaydetmek için ve bu bildirim (ya da zaman veya konuma göre) nasıl tetiklenmesi zamanlamak için.

Bir geliştirici varolan bildirim API ile çalışırken karşılaşabileceğiniz birkaç sorunu vardır:

- Yerel veya uzak hangi kod çoğaltma için yol açabilecek bildirimler için gerekli farklı geri aramalar vardı.
- Bu sistemle zamanlanmış sonra uygulama bildirim denetimin sınırlı.
- Vardı destek düzeyleri farklı tüm Apple'nın mevcut platformları arasında.

### <a name="about-the-new-user-notification-framework"></a>Yeni Kullanıcı bildirim çerçevesi hakkında

Varolan yerini alır yeni kullanıcı bildirimi framework, Apple iOS 10 anlatılmıştır `UIApplication` yöntemi not ettiğiniz üstünde.

Kullanıcı bildirimi framework aşağıdakileri sağlar:

- Bağlantı noktası kodu mevcut çerçevesinden kolaylaşır önceki yöntemleriyle özellik eşliği içerir bilinen bir API.
- Kullanıcıya gönderilecek daha zengin bildirimler sağlayan bir genişletilmiş içerik seçenekleri kümesi içerir.
- Hem yerel hem de uzak bildirimler aynı kod ve geri aramalar tarafından işlenebilir.
- Kullanıcı bir bildirim ile etkileşim kurarken, bir uygulamaya gönderilir geri aramalar işleme işlemini basitleştirir.
- Gelişmiş Yönetim kaldırın ya da güncelleştirme bildirimleri özelliği de dahil olmak üzere bekleyen ve teslim bildirimler.
- Bildirimleri sunumu uygulama yeteneği ekler.
- Zamanlama ve uygulama Uzantılar içinde bildirimleri işleme yeteneği ekler.
- Bildirimler için yeni uzantı noktası ekler. 

Yeni kullanıcı bildirimi framework Apple dahil desteklediğini bir birleşik bildirimi API platformları katları arasında sağlar: 

- **iOS** -tam yönetmek ve bildirimleri zamanlamak için destek.
- **tvOS** -yerel ve uzak bildirimler için rozet uygulama simgeleri özelliği ekler.
- **watchOS** - kullanıcı eşlenmiş iOS cihazının bildirimleri için kendi Apple Watch iletme yeteneği ekler ve izleme uygulamaları yerel bildirimlerini doğrudan izleme yeteneği sağlar.

Daha fazla bilgi için lütfen Apple'nın bkz [UserNotifications Framework başvurusu](https://developer.apple.com/reference/usernotifications) ve [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui) belgeleri.

## <a name="preparing-for-notification-delivery"></a>Bildirim teslimi için hazırlama

Önce bir iOS, uygulama sistemi kaydedilmelidir kullanıcıya, bildirimleri uygulama gönderebilir ve kesinti kullanıcıya bir bildirim olduğu için bir uygulama açıkça bunları göndermeden önce izin istemeniz gerekir.

Bir uygulama için kullanıcının ayarlayabileceği bildirim isteklerini üç farklı düzeylerde vardır:

- Başlık görüntüler.
- Ses uyarılar.
- Uygulama simgesi badging.

Ayrıca, bu onay düzeyleri istenen ve yerel ve uzak bildirimleri için ayarlamanız gerekir.

Bildirim izni istenen aşağıdaki kodu ekleyerek uygulama başlatılır hemen `FinishedLaunching` yöntemi `AppDelegate` ve istenen bildirim türünü ayarlama (`UNAuthorizationOptions`):

```csharp
using UserNotifications;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    return true;
}
```

Ayrıca, bir Kullanıcı bildirim ayrıcalıklar kullanarak istediğiniz zaman bir uygulama için her zaman değiştirebilir **ayarları** cihaza uygulamanın. Uygulama için kullanıcının istenen bildirim ayrıcalıkları aşağıdaki kodu kullanarak bir bildirim sunmadan önce denetlemeniz gerekir:

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>Uzak bildirimler ortamını yapılandırma

Yeni iOS 10, geliştirici hangi ortamı anında iletilen bildirim çalışıyor geliştirme veya üretim olarak işletim sistemi bilgilendirmeniz. Bu bilgiler sağlamak için hata aşağıdakine benzer bir bildirim ile uygulama mağazası iTune gönderildiğinde reddedilme uygulama neden olabilir:

> Eksik anında iletme bildirimi yetkilendirme - uygulamanızı Apple anında iletilen bildirim hizmeti için bir API içerir ancak `aps-environment` yetkilendirme uygulamanın imzadan eksik.

Gerekli yetkilendirme sağlamak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Entitlements.plist` dosyasını **çözüm paneli** düzenlemek için açın.
2. Geçiş **kaynak** görünümü: 

    [![](enhanced-user-notifications-images/setup01.png "Kaynak Görünümü")](enhanced-user-notifications-images/setup01.png#lightbox)
3. Tıklatın  **+**  düğmesi yeni bir anahtar ekleyin.
4. Girin `aps-environment` için **özelliği**, bırakın **türü** olarak `String` ve ya da girin `development` veya `production` için **değeri**: 

    [![](enhanced-user-notifications-images/setup02.png "AP ortam özelliği")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Değişiklikleri dosyaya kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çift `Entitlements.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
3. Tıklatın  **+**  düğmesi yeni bir anahtar ekleyin.
4. Girin `aps-environment` için **özelliği**, bırakın **türü** olarak `String` ve ya da girin `development` veya `production` için **değeri**: 

    [![](enhanced-user-notifications-images/setup02w.png "AP ortam özelliği")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Değişiklikleri dosyaya kaydedin.

-----

### <a name="registering-for-remote-notifications"></a>Uzak bildirimleri için kaydediliyor

Uygulama gönderme ve uzak bildirimleri alma, bunu hala yapmanız gerekir _belirteci kayıt_ varolan kullanarak `UIApplication` API. Bu kayıt dinamik ağ bağlantısı erişimi uygulamaya gönderilir gerekli belirteci oluşturur APNs aygıta gerektirir. Bu belirteç uzak bildirimleri kaydı için geliştiricinin sunucu tarafı uygulamasına iletmek uygulamanın gerekir:

[![](enhanced-user-notifications-images/token01.png "Belirteç kayıt genel bakış")](enhanced-user-notifications-images/token01.png#lightbox)

Gerekli kaydı'nı başlatmak için aşağıdaki kodu kullanın:

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

Bildirim yükü o get'ın parçası sunucudan APNs için uzak bir bildirim göndererek gönderilen gibi dahil edilmek üzere Geliştirici sunucu tarafı uygulamaya gönderilir belirteci gerekir:

[![](enhanced-user-notifications-images/token02.png "Bildirim yükü bir parçası olarak dahil edilen belirteci")](enhanced-user-notifications-images/token02.png#lightbox)

Bildirim ve açmak veya bildirime yanıt vermek için kullanılan uygulama birbirine bağlar, anahtar belirteç görür.

Daha fazla bilgi için lütfen Apple'nın bkz [yerel ve uzak bildirim Programlama Kılavuzu](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) belgeleri.

## <a name="notification-delivery"></a>Bildirim teslimi

Uygulama ile tam olarak kayıtlı ve gelen gerekli izinleri istenen ve verilen kullanıcı tarafından uygulama artık bildirim gönderip almak hazırdır. 

### <a name="providing-notification-content"></a>Bildirim içerik sağlama

Yeni iOS 10, tüm bildirimler hem de içeren bir **başlık** ve **altyazısı** , her zaman görüntülenir ile **gövde** bildirim içerik. Ayrıca ekleme yeteneği, yenilikler **medya ekleri** bildirim içerik.

Yerel bir bildirim içeriği oluşturmak için aşağıdaki kodu kullanın:

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

Uzak bildirimler için işlem benzerdir:

```csharp
{
    "aps":{
        "alert":{
            "title":"Notification Title",
            "subtitle":"Notification Subtitle",
            "body":"This is the message body of the notification."
        },
        "badge":1
    }
}
```

### <a name="scheduling-when-a-notification-is-sent"></a>Zamanlama olduğunda bir bildirim gönderilir

Oluşturduğunuz bildirim içerikle uygulamanın ayarlayarak, bildirim kullanıcıya sunulur zamanlama için gerekir. bir *tetikleyici*. iOS 10 dört farklı tetikleyici türlerini sağlar:

- **Bildirim gönderme** - özel olarak uzaktan bildirimleri ile kullanılır ve APNs gönderir bildirim bir cihazda çalışan uygulama paketini geldiğinde tetiklenir.
- **Zaman aralığı** -yerel saat arasında bir saat zamanlanacak aralığı başlangıç şimdi ve gelecekte noktada bir bitiş bildirim sağlar. Örneğin, `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`
- **Takvim tarihi** -belirli bir tarih ve saat için zamanlanacak yerel bildirimleri sağlar.
- **Konuma göre** -iOS cihazı olduğunda girme veya belirli bir coğrafi konum bırakmak veya belirli bir yerde konumlandırıldığında herhangi Bluetooth işaretleri için kullanıldığında zamanlanacak yerel bildirimleri sağlar.

Yerel bir bildirim hazır olduğunda, uygulama çağırmak gerek duyduğu `Add` yöntemi `UNUserNotificationCenter` görünümünü kullanıcıya zamanlamak için nesne. Uzak bildirimler için sunucu tarafı uygulama bildirim yükü APNs için kullanıcının aygıtı açın paket gönderen gönderir.

Tüm parçaları araya getiren bir örnek yerel bildirim aşağıdaki gibi görünmelidir:

```csharp
using UserNotifications;
...

var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);

var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

## <a name="handling-foreground-app-notifications"></a>Ön plan uygulama bildirimleri işleme

Yeni iOS 10, bir uygulama bildirimleri farklı ön planda olduğunda ve bir uyarı tetiklendiğinde işleyebilir. Sağlayarak bir `UNUserNotificationCenterDelegate` ve uygulama `UserNotificationCenter` yöntemi, uygulama bildirimi görüntüleme sorumluluğunu üzerinden alabilir. Örneğin:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        #region Constructors
        public UserNotificationCenterDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void WillPresentNotification (UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
        {
            // Do something with the notification
            Console.WriteLine ("Active Notification: {0}", notification);

            // Tell system to display the notification anyway or use
            // `None` to say we have handled the display locally.
            completionHandler (UNNotificationPresentationOptions.Alert);
        }
        #endregion
    }
}
```

Bu kod içeriği yalnızca yazma `UNNotification` uygulama çıkış ve sistem isteyen standart uyarı bildirimi için görüntülenecek. 

Uygulama istediyseniz, ön planda olduğundan bildirim göster değil sistem varsayılanlarını kullan, geçirmek `None` tamamlama işleyicisi. Örnek:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

Yerinde bu kodu ile açmak `AppDelegate.cs` dosya düzenleme için ve değişiklik `FinishedLaunching` aramak için yöntemi aşağıdaki gibi:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    // Watch for notifications while the app is active
    UNUserNotificationCenter.Current.Delegate = new UserNotificationCenterDelegate ();

    return true;
}
```

Bu kodu özel ekleme `UNUserNotificationCenterDelegate` üstten geçerli `UNUserNotificationCenter` etkin durumdayken uygulamanın bildirim işleyebilmesi için ve ön planda.

## <a name="notification-management"></a>Bildirim Yönetimi

Yeni iOS 10, bildirim yönetim bekleyen ve teslim bildirimlerine erişim sağlar ve kaldırmak, güncelleştirme ya da bu bildirimler Yükselt özelliği ekler.

Bildirim yönetiminin önemli bir parçası olan _istek tanımlayıcısı_ , atanma bildirimi oluşturduğunuzda ve sistemiyle zamanlanmış. Uzak bildirimler için bu yeni atanan `apps-collapse-id` HTTP istek üstbilgisi alanındaki.

İstek tanımlayıcısı, uygulama üzerinde bildirim yönetimi gerçekleştirmek istediği bildirim seçmek için kullanılır.

### <a name="removing-notifications"></a>Bildirimleri kaldırma

Bekleyen bir bildirim sistemden kaldırmak için aşağıdaki kodu kullanın:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

Zaten teslim edilen bir bildirim kaldırmak için aşağıdaki kodu kullanın:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>Mevcut bir bildirim güncelleştiriliyor

Mevcut bir bildirim güncelleştirmek için (bir yeni tetikleyici gibi) değiştirme zamanı istenen parametrelere sahip yeni bir bildirim oluşturmak ve aynı tanımlayıcıyla isteği değiştirilmesi gereken bildirim olarak sisteme ekleyin. Örnek:


```csharp
using UserNotifications;
...

// Rebuild notification
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

// New trigger time
var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger (10, false);

// ID of Notification to be updated
var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

// Add to system to modify existing Notification
UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

Zaten teslim bildirimleri için varolan bildirim alın güncelleştirildi ve kullanıcı tarafından daha önce Okunmuş varsa ev ve kilit ekranında ve bildirim merkezinde listesinin en üstüne yükseltilmiş.

## <a name="working-with-notification-actions"></a>Bildirim eylemlerini ile çalışma

İOS 10, kullanıcıya gönderilen bildirimler statik değildir ve kullanıcı etkileşim kurabilen birkaç yolu bunlarla (Başlangıç özel eylemler için yerleşik) sağlayın.

Bir iOS uygulaması yanıt verebilir Eylemler üç tür vardır:

- **Varsayılan eylem** -uygulamasını açın ve verilen bildirim ayrıntılarını görüntülemek için bir bildirim kullanıcı dokunur durumunda olur.
- **Özel Eylemler** -bunlar iOS 8 eklendi ve kullanıcının uygulamayı başlatmak gerek kalmadan doğrudan bildirimden özel bir görev gerçekleştirmek hızlı bir yolunu sağlar. Özelleştirilebilir başlıkları düğmeleriyle listesini veya (burada uygulama fu için ön planda başlatıldıktan (burada uygulaması isteği gerçekleştirmek için kısa bir süre verilir) arka plan veya ön çalıştırabileceğiniz bir metin giriş alanı olarak sunulabilir lfill istek). Özel Eylemler, hem iOS hem de watchOS kullanılabilir.
- **Eylem yok sayın** -belirli bir bildirim kullanıcı atar, bu eylem uygulamaya gönderilir.

### <a name="creating-custom-actions"></a>Özel Eylemler oluşturma

Oluşturmak ve bir özel eylem sistemiyle kaydetmek için aşağıdaki kodu kullanın:

```csharp
// Create action
var actionID = "reply";
var title = "Reply";
var action = UNNotificationAction.FromIdentifier (actionID, title, UNNotificationActionOptions.None);

// Create category
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);
    
// Register category
var categories = new UNNotificationCategory [] { category };
UNUserNotificationCenter.Current.SetNotificationCategories (new NSSet<UNNotificationCategory>(categories)); 
```

Yeni bir oluştururken `UNNotificationAction`, benzersiz bir kimlik ve düğmesi görünür başlık atanır. Seçenekler (örneğin bir ön eylem olarak ayarlamak) eylemin davranışını ayarlamak için sağlanan ancak varsayılan olarak, bir arka plan eylem olarak eylem oluşturulur.

Oluşturulan eylemlerin her biri gerekir kategorisi ile ilişkilendirilecek. Yeni bir oluştururken `UNNotificationCategory`, benzersiz bir kimlik atanır, eylemlerin bir listesini bunu gerçekleştirebilirsiniz, hedefi kategorisinde eylemlerin amacı hakkında daha fazla bilgi ve kategori davranışını denetlemek için bazı seçenekler sağlamak için kimliklerinin bir listesi.

Son olarak, tüm kategorilerini sistemiyle kayıtlı `SetNotificationCategories` yöntemi.

### <a name="presenting-custom-actions"></a>Özel Eylemler sunma

Özel Eylemler ve kategorileri kümesi oluşturulur ve sisteme kayıtlı sonra yerel veya uzak bildirimler sunulabilir.

Uzak bildirim için ayarlanmış bir `category` yukarıda oluşturduğunuz kategorileri biriyle eşleşen uzak bildirim yükte. Örneğin:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

Yerel bildirimleri için ayarlamak `CategoryIdentifier` özelliği `UNMutableNotificationContent` nesnesi. Örneğin:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

Yeniden, bu kimliği yukarıda oluşturduğunuz kategorilerden birini ile eşleşmesi gerekir.

### <a name="handling-dismiss-actions"></a>İşleme kapatmak Eylemler

Kullanıcı bir bildirim atar yukarıda belirtildiği gibi yok sayın eylem uygulamaya gönderilebilir. Standart bir eylem olmadığından bir seçeneği kategori oluşturulduğunda ayarlanması gerekir. Örneğin:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>Eylem yanıtları işleme

Kullanıcının yukarıda oluşturulan kategorileri ve özel eylemler ile etkileşime giren, istenen görev yerine getirmek uygulamanın gerekir. Bu sağlayarak gerçekleştirilir bir `UNUserNotificationCenterDelegate` ve uygulama `UserNotificationCenter` yöntemi. Örneğin:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        ...

        #region Override Methods
        public override void DidReceiveNotificationResponse (UNUserNotificationCenter center, UNNotificationResponse response, Action completionHandler)
        {
            // Take action based on Action ID
            switch (response.ActionIdentifier) {
            case "reply":
                // Do something
                break;
            default:
                // Take action based on identifier
                if (response.IsDefaultAction) {
                    // Handle default action...
                } else if (response.IsDismissAction) {
                    // Handle dismiss action
                }
                break;
            }

            // Inform caller it has been handled
            completionHandler();
        }
        #endregion
    }
}
```

Geçirilen içinde `UNNotificationResponse` sınıfına sahip bir `ActionIdentifier` seçebilir ya da özellik varsayılan eylem veya eylem yok sayın. Kullanım `response.Notification.Request.Identifier` herhangi bir özel eylem için test etmek için.

`UserText` Özelliği giriş herhangi bir kullanıcı metin değerini içerir. `Notification` Özelliği, tetikleyici istekle içeren kaynak bildirimi ve bildirim içerik tutar. Uygulamayı yerel oluştu veya uzaktan bildirim tetikleyici türüne göre karar verebilirsiniz.

## <a name="working-with-service-extensions"></a>Hizmeti Uzantıları ile çalışma

Uzak bildirimler ile çalışırken _hizmeti uzantıları_ bildirim yükü içinde uçtan uca şifrelemeyi etkinleştirmek için bir yol sağlar. Hizmet, program.cs'ye veya kullanıcıya görüntülenmeden önce bir bildirim görünür içeriğini değiştirme ana amacı ile arka planda kullanıcı arabirimi uzantısı (IOS 10 kullanılabilir) uzantıları. 

[![](enhanced-user-notifications-images/extension01.png "Hizmeti uzantısının genel bakış")](enhanced-user-notifications-images/extension01.png#lightbox)

Hizmeti uzantılarını hızlı bir şekilde çalıştırmak için yöneliktir ve yalnızca sistem tarafından yürütmek için kısa bir süre verilir. Ayrılan sürede, görevini tamamlamak üzere hizmet uzantısı başarısız durumunda durumunda, bir geri dönüş yöntemi çağrılır. Geri dönüş başarısız olursa, özgün bildirim içerik kullanıcıya görüntülenir.

Bazı olası hizmeti uzantıları kullanımları şunları içerir:

- Uzak bildirim içeriğin uçtan uca şifreleme sağlar.
- Bunları zenginleştirmek için Uzak bildirimler ekleri ekleniyor.

### <a name="implementing-a-service-extension"></a>Hizmeti uzantısını uygulama

Bir hizmet uzantısı bir Xamarin.iOS uygulaması uygulamak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'da uygulamanın çözümü açın
2. Çözüm adına sağ tıklayın **çözüm paneli** seçip **Ekle** > **Yeni Proje Ekle**.
3. Seçin **iOS** > **uzantıları** > **bildirim hizmeti uzantıları** tıklatıp **sonraki** düğmesi: 

    [![](enhanced-user-notifications-images/extension02.png "Bildirim hizmeti uzantıları seçin")](enhanced-user-notifications-images/extension02.png#lightbox)
4. Girin bir **adı** tıklatın ve uzantı için **sonraki** düğmesi: 

    [![](enhanced-user-notifications-images/extension03.png "Uzantı için bir ad girin")](enhanced-user-notifications-images/extension03.png#lightbox)
5. Ayarlama **proje adı** ve/veya **çözüm adı** gerekli ve tıklatırsanız **oluşturma** düğmesi: 

    [![](enhanced-user-notifications-images/extension04.png "Proje adı ve/veya çözüm adı")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uygulamanın çözümü Visual Studio'da açın.
2. Çözüm adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle** > **Yeni Proje Ekle**.
3. Seçin **iOS** > **uzantıları** > **bildirim hizmeti uzantıları**: 

    [![](enhanced-user-notifications-images/extension01w.png "Bildirim hizmeti uzantıları seçin")](enhanced-user-notifications-images/extension01w.png#lightbox)
4. Girin bir **adı** tıklatın ve uzantı için **Tamam** düğmesi.

-----

> [!IMPORTANT]
> Not: Hizmet uzantısı paket tanımlayıcısı ile ana uygulamanın paket tanımlayıcısı eşleşmelidir `.appnameserviceextension` sonuna eklenir. Örneğin, ana uygulama paket tanıtıcısı olsaydı `com.xamarin.monkeynotify`, hizmet uzantısı paket tanıtıcısı olmalıdır `com.xamarin.monkeynotify.monkeynotifyserviceextension`. Uzantı çözüme eklendiğinde bu otomatik olarak ayarlanmalıdır. 

Bildirim hizmeti gerekli işlevselliği sağlamak için değiştirilmesi gereken uzantısı'nda bir ana sınıfı yok. Örneğin:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyChatServiceExtension
{
    [Register ("NotificationService")]
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Computed Properties
        public Action<UNNotificationContent> ContentHandler { get; set; }
        public UNMutableNotificationContent BestAttemptContent { get; set; }
        #endregion

        #region Constructors
        protected NotificationService (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            ContentHandler = contentHandler;
            BestAttemptContent = (UNMutableNotificationContent)request.Content.MutableCopy ();

            // Modify the notification content here...
            BestAttemptContent.Title = $"{BestAttemptContent.Title}[modified]";

            ContentHandler (BestAttemptContent);
        }

        public override void TimeWillExpire ()
        {
            // Called just before the extension will be terminated by the system.
            // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.

            ContentHandler (BestAttemptContent);
        }
        #endregion
    }
}
```

İlk yöntem `DidReceiveNotificationRequest`, bildirim tanımlayıcısı yanı sıra bildirim içerik geçirilen `request` nesnesi. Geçirilen içinde `contentHandler` kullanıcıya bildirim sunmak için çağrılması gerekir.

İkinci yöntem `TimeWillExpire`, yalnızca zaman hizmeti uzantı isteğini işlemek dolmak üzere önce çağrılır. Çağrılacak hizmet uzantısı başarısız olursa `contentHandler` ayrılan sürede, özgün içerik kullanıcıya görüntülenir.

### <a name="triggering-a-service-extension"></a>Bir hizmet uzantısı tetikleme

Bir hizmet oluşturulur ve uygulamayla teslim uzantısı ile bu cihaza gönderilen uzak bildirim yükü değiştirerek tetiklenebilir. Örneğin:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

Yeni `mutable-content` anahtarını belirtir hizmeti uzantı uzaktan bildirim içeriği güncelleştirmek için başlatılması gerekir. `encrypted-content` Anahtar hizmeti uzantı kullanıcıya sunmadan önce şifresini çözebilir şifrelenmiş verileri tutar.

Aşağıdaki örnek hizmeti uzantı göz atın:

```csharp
using UserNotification;

namespace myApp {
    public class NotificationService : UNNotificationServiceExtension {
    
        public override void DidReceiveNotificationRequest(UNNotificationRequest request, contentHandler) {
            // Decrypt payload
            var decryptedBody = Decrypt(Request.Content.UserInfo["encrypted-content"]);
            
            // Modify Notification body
            var newContent = new UNMutableNotificationContent();
            newContent.Body = decryptedBody;
            
            // Present to user
            contentHandler(newContent);
        }
        
        public override void TimeWillExpire() {
            // Handle out-of-time fallback event
            ...
        }
        
    }
}
```

Bu kod şifrelenmiş içerikten şifresini çözer `encrypted-content` anahtar, yeni bir `UNMutableNotificationContent`, ayarlar `Body` kullanır ve şifresi çözülmüş içerik özelliğine `contentHandler` kullanıcıya bildirim sunmak üzere.

## <a name="summary"></a>Özet

Bu makalede tüm kullanıcılara bildirim 10 iOS tarafından geliştirilmiştir yolları ele. Yeni kullanıcı bildirimi framework ve bir Xamarin.iOS uygulaması veya uygulama uzantısı kullanma sunulmuştur.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications Framework başvurusu](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Yerel ve uzak bildirim Programlama Kılavuzu](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
