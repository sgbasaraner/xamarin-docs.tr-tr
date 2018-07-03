---
title: İzlenecek yol - Xamarin.ios'ta yerel bildirimleri kullanma
description: Bu bölümde bir Xamarin.iOS uygulaması yerel bildirimleri kullanma gösterilecektir. Bu, oluşturma ve uygulama tarafından alındığında, bir uyarı oluşturan açılır bir bildirimin yayımlanması temellerini gösterir.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: cf1e44ba4176922234fc1b6b9bfe5c463611cc7b
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403435"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>İzlenecek yol - Xamarin.ios'ta yerel bildirimleri kullanma

_Bu bölümde bir Xamarin.iOS uygulaması yerel bildirimleri kullanma gösterilecektir. Bu, oluşturma ve uygulama tarafından alındığında, bir uyarı oluşturan açılır bir bildirimin yayımlanması temellerini gösterir._

> [!IMPORTANT]
> Bu bölümdeki bilgiler, iOS 9 ilgilidir ve önceki, burada daha önceki iOS sürümlerini desteklemek için bırakıldı. İOS 10 ve üzeri, için lütfen bkz. [Kullanıcı bildirim Framework Kılavuzu](~/ios/platform/user-notifications/index.md) hem yerel hem de uzak bildirim, bir iOS cihazında destekleme.

## <a name="walkthrough"></a>İzlenecek yol

Yerel bildirimler uygulamada gösteren basit bir uygulama oluşturmasına izin verin. Bu uygulama, tek bir düğmeye üzerinde olacaktır. Biz düğmesine tıkladığınızda, yerel bir bildirim oluşturun. Belirtilen süre geçtikten sonra görünen bildirim göreceğiz.


1. Mac için Visual Studio, yeni tek görünüm iOS çözümü oluşturur ve onu çağırmak `Notifications`.
1. Açık `Main.storyboard` dosyası ve bir düğmeyi görünümü sürükleyin. Düğmeyi adlandırın **düğmesi**ve başlık verin **ekleme bildirimi**. Bazı ayarlamak isteyebilirsiniz [kısıtlamaları](~/ios/user-interface/designer/designer-auto-layout.md) düğmesine bu noktada: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Düğme bazı kısıtlamaları ayarlama")
1. Düzen `ViewController` sınıfı ve aşağıdaki olay işleyicisini ViewDidLoad yöntemi ekleyin:

    ```csharp
    button.TouchUpInside += (sender, e) =>
    {
        // create the notification
        var notification = new UILocalNotification();

        // set the fire date (the date time in which it will fire)
        notification.FireDate = NSDate.FromTimeIntervalSinceNow(60);

        // configure the alert
        notification.AlertAction = "View Alert";
        notification.AlertBody = "Your one minute alert has fired!";

        // modify the badge
        notification.ApplicationIconBadgeNumber = 1;

        // set the sound to be the default sound
        notification.SoundName = UILocalNotification.DefaultSoundName;

        // schedule it
        UIApplication.SharedApplication.ScheduleLocalNotification(notification);
    };
    ```

    Bu kod, bir ses kullanan, simgesine rozet değerini 1 olarak ayarlar ve kullanıcıya bir uyarı görüntüler bir bildirim oluşturur.

1. Sonraki dosya düzenleme `AppDelegate.cs`, önce aşağıdaki kodu ekleyin `FinishedLaunching` yöntemi. 8, iOS cihaz çalışıp çalışmadığını, böylece biz olup olmadığını görmek için denetlediyseniz **gerekli** bildirimleri almak kullanıcının izin istemek için:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. Hala `AppDelegate.cs`, bir bildirim alındığında çağrılacak aşağıdaki yöntemi ekleyin:

    ```csharp
    public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
            {
                // show an alert
                UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
                okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

                UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(okayAlertController, true, null);

                // reset our badge
                UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
            }

    ```

1. Burada, bildirim nedeniyle yerel bir bildirim başlatıldı durumu işlemek ihtiyacımız var. Yöntemini Düzenle `FinishedLaunching` içinde `AppDelegate` aşağıdaki kod parçacığını eklemek için:


    ```csharp
    // check for a notification

    if (launchOptions != null)
    {
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
    }

    ```

1. Son olarak, uygulamayı çalıştırın. İOS 8, bildirimleri izin istenir. Tıklayın **Tamam** ve ardından **bildirimi ekleme** düğmesi. Kısa bir duraklamadan sonra uyarı iletişim kutusunda, aşağıdaki ekran görüntülerinde gösterildiği gibi görmeniz gerekir:

    ![](local-notifications-in-ios-walkthrough-images/image0.png "Bildirimler gönderme olanağı onaylayan") ![ ] (local-notifications-in-ios-walkthrough-images/image1.png "bildirim Ekle düğmesi") ![ ] (local-notifications-in-ios-walkthrough-images/image2.png "bildirim uyarı iletişim kutusu")

## <a name="summary"></a>Özet

Bu izlenecek yolda nasıl oluşturmak ve iOS bildirimleri yayımlamak için çeşitli API'nin kullanılacağını gösterdi. Ayrıca, uygulama simgesine rozet kullanıcıya birkaç uygulama belirli geri bildirim sağlamak için güncelleştirilecek nasıl gösterilmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [Yerel bildirimler (örnek)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Yerel ve anında iletme bildirimi Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
