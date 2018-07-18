---
title: Xamarin.iOS için uygulama yaşam döngüsü Tanıtımı
description: Bu belgede, çeşitli yaşam döngüsü olaylarını bu olayların ne zaman ve nasıl işlenir gösteren bir iOS uygulamasına uygulama Temsilcide tarafından işlenen inceler.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/17/2018
ms.openlocfilehash: 53ae6947cf1483fabe415d6f6521d9384bddb46f
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111179"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Xamarin.iOS için uygulama yaşam döngüsü Tanıtımı

Bu makalede ve [örnek kod](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/) ios'ta dört uygulama durumlarını ve rolü gösterir `AppDelegate` bildiren durumlarını ne zaman değiştirildiğini, uygulama içinde yöntemleri. Uygulama durumu değiştiğinde uygulama güncelleştirmeleri konsola yazdırır:

[![](application-lifecycle-demo-images/image3-sml.png "Örnek uygulama")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "Uygulama durumu değiştiğinde uygulama güncelleştirmeleri konsola yazdırır")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>İzlenecek yol

1. Açık **yaşam döngüsü** projesi **LifecycleDemo** çözüm.
1. Açık yukarı `AppDelegate` sınıfı. Günlüğe kaydetme, uygulama durumu ne zaman değiştirildiğini göstermek için yaşam döngüsü yöntemler eklenmiştir:

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. Cihaz veya simülatör uygulamasını başlatın. `OnActivated` uygulama başlatıldığında çağrılır. Uygulama artık bulunduğu _etkin_ durumu.
1. Simülatör veya arka plan uygulamaya getirmek için cihaz giriş düğmesine basın. `OnResignActivation` ve `DidEnterBackground` uygulama geçişler olarak adlandırılan `Active` için `Inactive` ve `Backgrounded` durumu. Arka planda yürütülmesi için hiçbir uygulama kod kümesi olduğundan, bir uygulama olarak kabul edilir _askıya_ bellekte.
1. Önplana geri getirmek için uygulamaya geri gidin. `WillEnterForeground` ve `OnActivated` hem de çağrılır:

    ![](application-lifecycle-demo-images/image4.png "Konsola yazdırılmasını durum değişiklikleri")

    Uygulama ön, arka planından geçtiğini ve ekranda görüntülenen metni değişiklikleri görünüm denetleyicisi kod aşağıdaki satırı çalıştırılır:

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. Tuşuna **giriş** arka plan uygulamaya koymak düğmesi. Ardından, çift dokunmayla **giriş** düğmesini uygulama değiştirici taşıyın. İPhone X ekranının altındaki yukarı çekin:

    [![Uygulama değiştirici](application-lifecycle-demo-images/app-switcher-sml.png "uygulama değiştirici")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. Uygulama uygulama değiştirici bulun ve yukarı doğru çekin (makinesinde köşesinde kırmızı görünmezse kadar iOS 11, uzun) kaldırmak için:

    [![Remove çalışan bir uygulamanın en çok kullanılan](application-lifecycle-demo-images/app-switcher-swipe-sml.png "Kaldır çalışan bir uygulamanın en fazla geçirme")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS uygulama sona erer. Unutmayın `WillTerminate` uygulama zaten olduğu için çağrılmaz _askıya_ arka planda.

## <a name="related-links"></a>İlgili bağlantılar

- [LifecycleDemo (örnek)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
