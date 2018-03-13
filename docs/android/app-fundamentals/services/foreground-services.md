---
title: "Ön plan Hizmetleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 96e8d1a3658a515b6b1d37cf0fdd93157954c01d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="foreground-services"></a>Ön plan Hizmetleri

Bazı Hizmetleri kullanıcıların etkin olarak farkında olduklarından bazı görevleri gerçekleştiren, bu hizmetler olarak da bilinir _ön plan Hizmetleri_. Ön plan hizmet kullanıcı ile yönergeleri yürüten veya taramasını sağlayan bir uygulama örneğidir. Uygulama arka planda olsa bile, yine hizmet düzgün çalışması için yeterli kaynaklara sahip olduğunu ve kullanıcının uygulamaya erişmek için hızlı ve kullanışlı bir yol önemlidir. Bir Android uygulaması için bu bir ön plan hizmeti "Normal" hizmet daha yüksek önceliği almanız gerekir ve bir ön plan hizmet sağlamalısınız anlamına gelir bir `Notification` çalıştığı sürece, Android görüntülenir.
 
Bir ön plan hizmet oluşturulur ve diğer hizmet olarak başlatıldı. Hizmet başlatma sırasında kendisi ile Android ön plan hizmet olarak kaydeder.
 
Bu kılavuz, bir ön plan hizmeti kaydetmek için ve bu işlem sona erdiğinde hizmetini durdurmak için alınması gereken ek adımlar ele alınacaktır.

## <a name="registering-as-a-foreground-service"></a>Ön plan hizmet olarak kaydetme

Bağımlı bir hizmet veya başlatılan hizmet özel türde bir ön plan hizmetidir. Hizmeti başlatıldıktan sonra çağırır [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/) kendisini ön plan hizmet olarak Android ile kaydetmek için yöntem.   

`StartForeground` her ikisi de zorunlu iki parametreleri alır:
 
* Hizmet tanımlamak üzere uygulama içinde benzersiz olan bir tamsayı değeri.
* A `Notification` çalıştığı sürece, Android için durum çubuğunu görüntüleyecek nesnesi.

Hizmetin çalıştığını sürece android bildirim için durum çubuğunda görüntüler. En azından, bildirim hizmeti çalıştıran kullanıcının görsel bir ipucu sağlar. İdeal olarak, bildirim, uygulama veya büyük olasılıkla bazı eylem düğmeleri uygulama denetlemek için kısayol kullanıcıyla sağlamalıdır. Bu, müzik çalar örneğidir &ndash; görüntülenen bildirim müzik, önceki şarkıyı geri sarma ya da sonraki şarkıya atlamak için Duraklat/play için düğmeler olabilir. 

Bu kod parçacığını bir hizmeti bir ön plan hizmet olarak kaydetme, bir örnek verilmiştir:   

```csharp
// This is any integer value unique to the application.
public const int SERVICE_RUNNING_NOTIFICATION_ID = 10000;

public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
{
    // Code not directly related to publishing the notification has been omitted for clarity.
    // Normally, this method would hold the code to be run when the service is started.
    
    var notification = new Notification.Builder(this)
        .SetContentTitle(Resources.GetString(Resource.String.app_name))
        .SetContentText(Resources.GetString(Resource.String.notification_text))
        .SetSmallIcon(Resource.Drawable.ic_stat_name)
        .SetContentIntent(BuildIntentToShowMainActivity())
        .SetOngoing(true)
        .AddAction(BuildRestartTimerAction())
        .AddAction(BuildStopServiceAction())
        .Build();

    // Enlist this instance of the service as a foreground service
    StartForeground(SERVICE_RUNNING_NOTIFICATION_ID, notification);
}
```

Önceki bildirim, aşağıdakine benzer bir durum çubuğu uyarısı görüntülenir:

![Durum çubuğunda bildirim gösteren görüntü](foreground-services-images/foreground-services-01.png "durum çubuğunda bildirim gösteren görüntü")

Bu ekran, kullanıcının hizmet denetlemesine izin ver iki eylemleri ile bildirim tepsisinde genişletilmiş bildirim gösterir:

![Genişletilmiş bildirim gösteren görüntü](foreground-services-images/foreground-services-02.png "genişletilmiş bildirim gösteren görüntü.")

Bildirimleri hakkında daha fazla bilgi bulunur [yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications.md) bölümünü [Android bildirimleri](~/android/app-fundamentals/notifications/index.md) Kılavuzu.

## <a name="unregistering-as-a-foreground-service"></a>Ön plan hizmet olarak kaydı siliniyor

Bir hizmetin kendisini ön plan hizmet olarak yöntemini çağırarak XML'deki listeleyebilirsiniz `StopForeground`. `StopForeground` hizmetini durdurur, ancak bu hizmet gerekirse kapatılabilir Android sinyalleri ve bildirim simgesini kaldırır.

Görüntülenen durum çubuğu uyarısı geçirerek de kaldırılabilir `true` yöntemi için: 

```csharp
StopForeground(true);
```

Hizmet çağrısıyla işlemi durduruldu durumunda `StopSelf` veya `StopService`, durum çubuğu notificaiton benzer şekilde kaldırılacak sonra.


## <a name="related-links"></a>İlgili bağlantılar

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Yerel Bildirimler](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
