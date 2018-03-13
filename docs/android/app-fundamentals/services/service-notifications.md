---
title: Hizmet bildirimleri
description: "Bu kılavuz, bir Android hizmeti bilgileri kullanıcıya gönderilmesi için yerel bildirimleri nasıl kullanabilirsiniz açıklanır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 3c06fca9c6d8c3cd91889007bd1879149771622b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="service-notifications"></a>Hizmet bildirimleri

_Bu kılavuz, bir Android hizmeti bilgileri kullanıcıya gönderilmesi için yerel bildirimleri nasıl kullanabilirsiniz açıklanır._


## <a name="service-notifications-overview"></a>Hizmet bildirim genel bakış

Android uygulama ön planda olsa bile kullanıcıya bilgi görüntülemek bir uygulama hizmeti bildirimlerine izin verin. Eylemler bir uygulamadan bir etkinlik görüntüleme gibi kullanıcı için sağlamak bir bildirim mümkündür. Aşağıdaki kod örneği, bir hizmetin nasıl bir kullanıcıya bir bildirim gönderme gösterir:

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

Bu ekran görüntülenir bildirim örneğidir:

[![Bildirim simgesi durum çubuğunda görüntülenir](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

Kullanıcı slayt bildirim ekranın en yukarıdan aşağı tam bildirimi görüntülenir:

![Bildirim tepsisinde görüntülenen notication](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>Bir bildirim güncelleştiriliyor

Bir bildirim güncelleştirmek için hizmet aynı bildirim kimliğini kullanarak bildirim yayımlar Android görüntülemek veya durum çubuğunda bildirim gerekirse güncelleştirin.

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

Bildirimleri hakkında daha fazla bilgi bulunur [yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications.md) bölümünü [Android bildirimleri](~/android/app-fundamentals/notifications/index.md) Kılavuzu.


## <a name="related-links"></a>İlgili bağlantılar

- [Android yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications.md)
