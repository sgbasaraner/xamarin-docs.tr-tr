---
title: Xamarin.iOS bildirimleri
description: Bu bölümde, Xamarin.ios'ta yerel bildirimleri uygulamak gösterilmektedir. Bu iOS bildirim çeşitli kullanıcı Arabirimi öğelerini açıklar ve API tartışmak oluşturma ve bir bildirim görüntüleme ile söz konusu.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/13/2018
ms.openlocfilehash: ef5ce97736e60ff3a03bc0d496eadcae8c6f7e61
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038540"
---
# <a name="notifications-in-xamarinios"></a>Xamarin.iOS bildirimleri

> [!IMPORTANT]
> Bu bölümdeki bilgiler, iOS 9 ve önceki ilgilidir. İOS 10 ve üzeri, için lütfen bkz. [Kullanıcı bildirim framework Kılavuzu](~/ios/platform/user-notifications/index.md).

iOS, kullanıcıya bir bildirim alındı göstermek için üç yol vardır:

- **Ses veya Titreşim** -iOS kullanıcıları bilgilendirmek için bir ses çalabilir. Sesi devre dışıysa cihaz vibrate şekilde yapılandırılabilir.
- **Uyarılar** -ekranında bildirim hakkında bilgi içeren bir iletişim kutusu görüntülemek mümkündür.
- **Rozetleri** - bildirim yayımlandığında bir sayı (uygulama simgesine badged) görüntülenebilir.

iOS de sağlayan bir *bildirim Merkezi* , görüntüler tüm bildirimler, yerel ve uzak kullanıcı için. Kullanıcılar bu ekranın üst kısmından aşağı çekilerek erişebilir:

![Bildirim Merkezi](local-notifications-in-ios-images/image13.png "bildirim Merkezi")

## <a name="creating-local-notifications-in-ios"></a>İos'ta yerel bildirimleri oluşturma

iOS oluşturma ve yerel bildirimleri işlemek oldukça basit hale getirir.
İlk olarak, iOS 8 bildirimleri görüntülemek kullanıcının izin istemek için uygulamalar gerektirir. Aşağıdaki kod bir yerel Bildirim göndermeye çalışmadan önce uygulamanıza ekleme - [örnek bağlı](https://developer.xamarin.com/samples/monotouch/LocalNotifications/) yerleştirir **AppDelegate**'s **FinishedLaunching** yöntemi.

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![Yerel bir bildirim gönderme olanağı onaylayan](local-notifications-in-ios-images/image0-sml.png "yerel bir bildirim gönderme olanağı onaylanıyor")](local-notifications-in-ios-images/image0.png#lightbox)

Yerel bir bildirim zamanlamak için oluşturma bir `UILocalNotification` nesne, ayarlama `FireDate`ve zamanlama ile `ScheduleLocalNotification` metodunda `UIApplication.SharedApplication` nesne. Aşağıdaki kod parçacığı, gelecekte bir dakika yangın ve bir ileti içeren bir uyarı görüntüler bildirim zamanlama gösterilmektedir:

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

Aşağıdaki ekran görüntüsünde, bu uyarı nasıl göründüğünü gösterir:

[![](local-notifications-in-ios-images/image2-sml.png "Örnek uyarı")](local-notifications-in-ios-images/image2.png#lightbox)

Bir kullanıcı seçerseniz unutmayın *izin* bildirimleri sonra hiçbir şey görüntülenir.

Bir sayı ile uygulama simgesine rozet uygulamak istiyorsanız, aşağıdaki satır kodda gösterildiği gibi ayarlayabilirsiniz:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

Sipariş oyunda simgesi, sesle SoundName özelliğini ayarlayın bildirime aşağıdaki kod parçacığında gösterildiği gibi:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Bildirim sesini 30 saniyeden uzunsa iOS varsayılan Ses yerine yürütülür.

> [!IMPORTANT]
> Temsilci bildirimi iki kez ateşlenir iOS simülatöründe hata yoktur. Bu sorun, uygulama bir cihazda çalışırken oluşmamalıdır.

## <a name="handling-notifications"></a>Bildirimleri işleme

iOS uygulamaları uzak ve yerel bildirimleri hemen hemen aynı şekilde işler. Bir uygulama çalıştırırken `ReceivedLocalNotification` yöntemi veya `ReceivedRemoteNotification` metodunda `AppDelegate` sınıfı çağrılacağı ve bildirim bilgilerini bir parametre olarak geçirilir.

Bir uygulamanın farklı yollarla bildirim işleyebilir. Örneğin, uygulama yalnızca kullanıcılar hakkında bazı olay hatırlatmak için bir uyarı görüntülenebilir. Veya bildirim sunucusuna eşitleniyor dosyaları gibi bir işlem bitene kullanıcıya uyarıyı göstermek için kullanılabilir.

Aşağıdaki kod, bir yerel bildirim işlemek ve bir uyarı görüntüler ve rozeti numarasını sıfırlamak gösterilmektedir:

```csharp
public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
{
    // show an alert
    UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
    okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

    Window.RootViewController.PresentViewController(okayAlertController, true, null);

    // reset our badge
    UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
}
```

Uygulama çalışmıyorsa iOS sesi ve/veya uygunsa simgesine rozet güncelleştirin. Kullanıcı uyarı ile ilişkili uygulama başlatıldığında uygulama başlatılır ve `FinishedLaunching` uygulama temsilci yöntemi çağrılır ve bildirim bilgileri de aracılığıyla geçirilir `launchOptions` parametresi. Seçenekleri sözlük anahtarı içeriyorsa `UIApplication.LaunchOptionsLocalNotificationKey`, ardından `AppDelegate` uygulamayı yerel bir bildirimden başlatıldı bilir. Aşağıdaki kod parçacığı bu işlemi göstermektedir:

```csharp
// check for a local notification
if (launchOptions.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
{
    var localNotification = launchOptions[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
    if (localNotification != null)
    {
        UIAlertController okayAlertController = UIAlertController.Create(localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

        Window.RootViewController.PresentViewController(okayAlertController, true, null);

        // reset our badge
        UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
    }
}
```

Uzak bir bildirim `launchOptions` sahip bir `LaunchOptionsRemoteNotificationKey` ile ilişkili `NSDictionary` uzak bildirim yükü içeren. Bildirim yükü aracılığıyla ayıklamak `alert`, `badge`, ve `sound` anahtarları. Aşağıdaki kod parçacığı, uzak bildirimler almak gösterilmektedir:

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Özet

Bu bölümde, oluşturma ve bir bildirim Xamarin.ios'ta yayımlama gösterildi. Göster nasıl bir uygulama için bildirimleri geçersiz kılarak react `ReceivedLocalNotification` yöntemi veya `ReceivedRemoteNotification` yönteminde `AppDelegate`.

## <a name="related-links"></a>İlgili bağlantılar

- [Yerel bildirimler (örnek)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Yerel ve anında iletme bildirimleri geliştiricileri için](https://developer.apple.com/notifications/)
- [Yerel ve anında iletilen bildirim Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [Uıapplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
