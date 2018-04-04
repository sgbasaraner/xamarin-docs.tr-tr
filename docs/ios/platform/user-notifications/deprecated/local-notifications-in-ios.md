---
title: Xamarin.iOS bildirimleri
description: Bu bölümde Xamarin.iOS yerel bildirimleri uygulamak gösterilmektedir. Bu bir iOS bildirim çeşitli kullanıcı Arabirimi öğelerini açıklar ve API ele oluşturma ve bir bildirim görüntüleme ile söz konusu.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d4dd759f52e52f417441f69e6417fab536daf6d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="notifications-in-xamarinios"></a>Xamarin.iOS bildirimleri

_Bu bölümde Xamarin.iOS yerel bildirimleri uygulamak gösterilmektedir. Bu bir iOS bildirim çeşitli kullanıcı Arabirimi öğelerini açıklar ve API ele oluşturma ve bir bildirim görüntüleme ile söz konusu._

> [!IMPORTANT]
> Bu bölümdeki bilgiler, iOS 9 ilgilidir ve önceki, bunu burada daha önceki iOS sürümlerini desteklemek için bırakıldı. 10 ve üzeri iOS için lütfen bkz. [Kullanıcı bildirim Framework Kılavuzu](~/ios/platform/user-notifications/index.md) hem yerel hem de uzak bildirim iOS cihazında desteklemek için.

iOS kullanıcıya bir bildirim alındı göstermek için üç yol vardır:

-  **Ses veya titreşimi** -iOS, kullanıcıları bilgilendirmek için ses çalabilir. Ses devre dışıysa, cihazı Titret için yapılandırılabilir.
-  **Uyarıları** -ekranında bildirim hakkında bilgi içeren bir iletişim kutusu görüntüler mümkündür.
-  **Rozetleri** - bildirim yayımlandığında, bir sayı (uygulama simgesine badged) görüntülenebilir.


iOS de sağlayan bir *bildirim Merkezi* , görüntüler tüm bildirimler, yerel ve uzak kullanıcı. Kullanıcılar bu ekran üstten aşağı çekilerek erişebilir:

 ![](local-notifications-in-ios-images/image13.png "Bildirim Merkezi")

## <a name="creating-local-notifications-in-ios"></a>Yerel bildirimler iOS oluşturma

iOS oluşturma ve yerel bildirimler işlemek oldukça basit hale getirir.
İlk olarak, iOS 8 bildirimleri görüntülemek, kullanıcının izni istemek için uygulamaları gerektirir. Yerel bir bildirim göndermeye çalışmadan önce uygulamanız için aşağıdaki kodu ekleyin - ekli örnek yerleştirir **AppDelegate**'s **FinishedLaunching** yöntemi.

```csharp
var settings = UIUserNotificationSettings.GetSettingsForTypes(
  UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound
  , null);
UIApplication.SharedApplication.RegisterUserNotificationSettings (settings);
```

  [![](local-notifications-in-ios-images/image0-sml.png "Yerel bir bildirim gönderme olanağı onaylama")](local-notifications-in-ios-images/image0.png#lightbox)

Oluşturduğunuz yerel bir bildirim zamanlamak için bir `UILocalNotification` nesne, ayarlamak `FireDate`, ile zamanla `ScheduleLocalNotification` yöntemi `UIApplication.SharedApplication` nesnesi. Aşağıdaki kod parçacığını gelecekte bir dakika yangın ve bir uyarı iletisi ile görüntülemek bir bildirim zamanlamak nasıl yapacağınızı gösterir:

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

Aşağıdaki ekran görüntüsünde Göster bu uyarı benzer:

  [![](local-notifications-in-ios-images/image2-sml.png "Bir örnek uyarı")](local-notifications-in-ios-images/image2.png#lightbox)

Kullanıcı seçerseniz unutmayın *izin* bildirimleri sonra hiçbir şey görüntülenir.

Bir gösterge uygulama simgesi sayıyla uygulamak istiyorsanız, aşağıdaki satırı kodda gösterildiği gibi ayarlayabilirsiniz:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

Sipariş Oynat ' simgesi ile ses SoundName özelliğini ayarlayın bildirim aşağıdaki kod parçacığında gösterildiği gibi:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Bir bildirim bir ses çalınırsa Apple İnsan Arabirimi yönergelerine göre bu da bir rozet veya uyarı uygulamayı tanımlamak kullanıcının yardımcı olmak için bir uyarı tarafından eşlik etmelidir. Ses 30 saniyeden daha uzun olması durumunda, ayrıca, iOS varsayılan Ses yerine yürütülür.

> [!IMPORTANT]
> İki kez temsilci bildirim ateşlenir iOS benzeticisinde hata yoktur. Bu sorun, uygulama bir cihazda çalıştırıldığında, oluşmamalıdır.

## <a name="handling-notifications"></a>Bildirimleri işleme

iOS uygulamaları uzak ve yerel bildirimler neredeyse tam olarak aynı şekilde işler. Bir uygulamayı çalıştırırken `ReceivedLocalNotification` yöntemi veya `ReceivedRemoteNotification` AppDelegate sınıfı yöntemi çağrılır ve bildirim bilgi parametre olarak geçirilir.

Bir uygulama bir bildirim farklı şekilde işleyebilir. Örneğin, uygulama yalnızca bazı olay hakkında kullanıcılara anımsatmak için bir uyarı görüntülenebilir. Veya bildirim sunucusuna eşitleniyor dosyaları gibi bir işlem tamamlandı kullanıcı bir uyarı görüntülemek için kullanılabilir.

Aşağıdaki kod, yerel bir bildirim işlemek ve bir uyarı görüntüler ve rozet numarası sıfırlamak gösterilmektedir:

```csharp
 public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
        {
            // show an alert
            UIAlertController okayAlertController = UIAlertController.Create (notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
            okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
            viewController.PresentViewController (okayAlertController, true, null);

            // reset our badge
            UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
        }
```

Uygulama çalışmıyorsa, iOS sesi ve/veya geçerli simgesini rozet güncelleştirin. Kullanıcı uyarı ile ilgili uygulama başlatıldığında, uygulama başlatma ve uygulama temsilci FinishedLaunching yöntemi çağrılır ve bildirim bilgi içinde seçenekleri parametresi geçirilir. Seçenekler sözlük anahtar içeriyorsa `UIApplication.LaunchOptionsLocalNotificationKey`, sonra da uygulamayı yerel bildirimden başlatıldı AppDelegate bilir. Aşağıdaki kod parçacığını bu işlemi gösterir:

```csharp
// check for a local notification
if (options.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
 {
     var localNotification = options[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
     if (localNotification != null)
     {
        UIAlertController okayAlertController = UIAlertController.Create (localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
        viewController.PresentViewController (okayAlertController, true, null);

         // reset our badge
         UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
     }
 }
```

Seçenekler sözlük yerine içerecektir uzak bir bildirim ise bir `LaunchOptionsRemoteNotificationKey`, ve bu anahtar sonuç değeri olur bir `NSDictionary` uzaktan bildirim yükü olan nesne. Uyarı, rozet ve ses anahtarları yoluyla bildirim yükü ayıklayabilirsiniz. Aşağıdaki kod parçacığını uzak bildirimleri almak nasıl gösterir:

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Özet

Bu bölümde oluşturmak ve Xamarin.iOS içinde bir bildirim yayımlamak nasıl oluşturulacağını gösterir. Göster nasıl uygulama bildirimleri göndermek için geçersiz kılarak tepki `ReceivedLocalNotification` yöntemi veya `ReceivedRemoteNotification` yönteminde `AppDelegate`.


## <a name="related-links"></a>İlgili bağlantılar

- [Yerel bildirimler (örnek)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Yerel ve anında iletme bildirimleri geliştiricileri için](https://developer.apple.com/notifications/)
- [Yerel ve anında iletilen bildirim Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [Uıapplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
