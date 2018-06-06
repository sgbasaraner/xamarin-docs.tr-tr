---
title: Uygulama yaşam döngüsü gösteri Xamarin.iOS için
description: Bu belgede, bu olayların ne zaman ve nasıl işleneceğini gösteren uygulama temsilcisi tarafından bir iOS uygulaması işlenmiş çeşitli yaşam döngüsü olayları inceler.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 64c695065012e4bf796c219c260324d9b6278ca5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783590"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Uygulama yaşam döngüsü gösteri Xamarin.iOS için

Bu bölümde, şu dört uygulama durumları ve rolünü gösteren bir uygulama incelemek için kalacaklarını `AppDelegate` durumları değiştiği uygulama bildiren içinde yöntemleri. Uygulama durumu değiştiğinde uygulama güncelleştirmeleri konsola yazdırılır:

 [![](application-lifecycle-demo-images/image3.png "Örnek uygulaması")](application-lifecycle-demo-images/image3.png#lightbox)

 [![](application-lifecycle-demo-images/image4.png "Uygulama durumu değiştiğinde uygulama güncelleştirmeleri konsola yazdırma")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>İzlenecek yol


  1. Açık _yaşam döngüsü_ proje _LifecycleDemo_ çözümü.
  1. Açık `AppDelegate` sınıfı. Günlüğe kaydetme bize ne zaman uygulama durumu değişti bildirin yaşam döngüsü yöntemleri ekledik olduğunu unutmayın:

            ```chsarp
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

  1. Uygulamayı benzetici veya cihazı başlatın. `OnActivated` uygulama başlatıldığında çağrılır. Uygulama olduğunu _etkin_ durumu.
  1. Simulator veya arka plan uygulamaya getirmek için cihaz giriş düğmesine basın. `OnResignActivation` ve `DidEnterBackground` uygulama geçişler olarak adlı `Active` için `Inactive` ve içine `Backgrounded` durumu. Biz uygulamamız arka planda yürütmek için herhangi bir kod verilmedi olduğundan, uygulama olarak kabul edilir _askıya_ bellekte.
  1. Ön alana geri getirmek için uygulamasına geri gidin. `WillEnterForeground` ve `OnActivated` her ikisi de çağrılır:

        ![](application-lifecycle-demo-images/image4.png "Konsola yazdırılır durum değişiklikleri")

    Aşağıdaki kod satırını uygulama arka planından ön girdiğini bize bildiren bizim görünüme denetleyiciye eklediğimiz olduğunu unutmayın:

        ```csharp
            UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
                    label.Text = "Welcome back!";
                });
        ```

1. Tuşuna **giriş** arka uygulamasına yerleştirilecek düğmesi. Ardından, çift dokunmayla **giriş** düğmesi uygulama değiştirici getirmek için:
    
    ![](application-lifecycle-demo-images/app-switcher-.png "Uygulama değiştirici")
  
1. Uygulama değiştirici uygulamayı bulun ve yukarı doğru çekin kaldırmak için:
    
    ![](application-lifecycle-demo-images/app-switcher-swipe-.png "Yukarı doğru çekin çalışan bir uygulamanın kaldırmak için") 
    
iOS uygulama sonlandırılır. Aklınızda `WillTerminate` biz bir uygulama sonlandırma çünkü çağrılmaz _askıya_ arka planda.

Biz iOS uygulama durumları ve geçişleri anladığınıza göre biz iOS backgrounding için kullanılabilir farklı seçenekler göz atın.



## <a name="related-links"></a>İlgili bağlantılar

- [LifecycleDemo(Part2) (örnek)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
