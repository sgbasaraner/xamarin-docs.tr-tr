---
title: "İzlenecek yol - yerel bildirimler Xamarin.iOS içinde kullanma"
description: "Bu bölümde biz yerel bildirimlerinin bir Xamarin.iOS uygulaması nasıl kullanılacağını size rehberlik. Oluşturma ve uygulama tarafından alındığında bir uyarı oluşturan pop bir bildirim yayımlama temellerini gösterecek."
ms.topic: article
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 846b292aed73a4980f4ce711ecefe4382fa7a321
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>İzlenecek yol - yerel bildirimler Xamarin.iOS içinde kullanma

_Bu bölümde biz yerel bildirimlerinin bir Xamarin.iOS uygulaması nasıl kullanılacağını size rehberlik. Oluşturma ve uygulama tarafından alındığında bir uyarı oluşturan pop bir bildirim yayımlama temellerini gösterecek._

> [!IMPORTANT]
> Bu bölümdeki bilgiler, iOS 9 ilgilidir ve önceki, bunu burada daha önceki iOS sürümlerini desteklemek için bırakıldı. 10 ve üzeri iOS için lütfen bkz. [Kullanıcı bildirim Framework Kılavuzu](~/ios/platform/user-notifications/index.md) hem yerel hem de uzak bildirim iOS cihazında desteklemek için.

## <a name="walkthrough"></a>İzlenecek yol

Yerel bildirimler eylemde gösterecektir basit bir uygulama oluşturmasına izin verin. Bu uygulama, üzerinde bir düğmeye sahip olur. Biz düğmeyi tıkladığınızda, yerel bir bildirim oluşturur. Belirtilen süre geçtikten sonra görünen bildirim göreceğiz.


1. Visual Studio'da Mac için yeni bir tek bir görünüm iOS çözümü oluşturun ve bunu `Notifications`.
1. Açık `Main.storyboard` dosyası ve bir düğme görünümü sürükleyin. Düğme adının **düğmesini**ve başlık verin **eklemek bildirim**. Bazı ayarlamak isteyebilirsiniz [kısıtlamaları](~/ios/user-interface/designer/designer-auto-layout.md) düğmesine bu noktada: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Bazı kısıtlamalar düğmesine ayarlama")
1. Düzen `ViewController` sınıfı ve aşağıdaki olay işleyicisini ViewDidLoad yöntemine ekleyin:

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

    Bu kod bir ses kullanan, simge rozet değerini 1 olarak ayarlar ve kullanıcı için bir uyarı görüntüler bir bildirim oluşturur.

1. Sonraki dosya düzenleme `AppDelegate.cs`, önce aşağıdaki kodu ekleyin. `FinishedLaunching` yöntemi. 8, iOS cihazı çalışıp çalışmadığını böylece biz olup olmadığını görmek için denetlediyseniz **gerekli** bildirimleri almak, kullanıcının izni istemek için:

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

                Window.RootViewController.PresentViewController(okayAlertController, true, null);

                // reset our badge
                UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
            }

    ```

1. Bildirim nedeniyle yerel bir bildirim burada başlatıldı durumu işlemek gerekir. Yöntemini Düzenle `FinishedLaunching` içinde `AppDelegate` aşağıdaki kod parçacığını eklemek için:


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

1. Son olarak, uygulamayı çalıştırın. İOS 8 bildirimleri izin vermek için istenir. Tıklatın **Tamam** ve ardından **bildirim eklemek** düğmesi. Kısa bir duraklamadan sonra uyarı iletişim kutusunda, aşağıdaki ekran görüntülerinde gösterildiği gibi görmeniz gerekir:

    ![](local-notifications-in-ios-walkthrough-images/image0.png "Bildirimleri gönderme olanağı onaylayan") ![ ] (local-notifications-in-ios-walkthrough-images/image1.png "eklemek bildirim düğmesini") ![ ] (local-notifications-in-ios-walkthrough-images/image2.png "bildirim uyarı iletişim kutusu")

## <a name="summary"></a>Özet

Bu kılavuzda oluşturmak ve iOS bildirimleri yayımlamak için çeşitli API'nin kullanmak nasıl oluşturulacağını gösterir. Ayrıca, kullanıcı için bazı uygulama belirli geri bildirim sağlamak için bir gösterge ile uygulamayı güncelleştirmek nasıl gösterilmektedir.


## <a name="related-links"></a>İlgili bağlantılar

- [Yerel bildirimler (örnek)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Yerel ve anında iletme bildirimleri Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
