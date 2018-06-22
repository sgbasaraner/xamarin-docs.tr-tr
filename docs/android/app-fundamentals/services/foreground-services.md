---
title: Ön plan Hizmetleri
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: 3088fa4b5cfa21ac57533ef331ffcc15414e14b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763754"
---
# <a name="foreground-services"></a>Ön plan Hizmetleri

Bağımlı bir hizmet veya başlatılan hizmet özel türde bir ön plan hizmetidir. Bazen Hizmetleri kullanıcılar etkin şekilde farkında olması gereken görevleri gerçekleştirir, bu hizmetler olarak da bilinir _ön plan Hizmetleri_. Ön plan hizmet kullanıcı ile yönergeleri yürüten veya taramasını sağlayan bir uygulama örneğidir. Uygulama arka planda olsa bile, yine hizmet düzgün çalışması için yeterli kaynaklara sahip olduğunu ve kullanıcının uygulamaya erişmek için hızlı ve kullanışlı bir yol önemlidir. Bir Android uygulaması için bu bir ön plan hizmeti "Normal" hizmet daha yüksek önceliği almanız gerekir ve bir ön plan hizmet sağlamalısınız anlamına gelir bir `Notification` çalıştığı sürece, Android görüntülenir.
 
Ön plan hizmetini başlatmak için uygulama hizmetini başlatmak için Android bildiren amacına gönderme gerekir. Ardından hizmetin kendisini Android ön plan hizmetiyle olarak kaydetmeniz gerekir. Android 8.0 (veya sonrası) çalıştıran uygulamaları kullanması gereken `Context.StartForegroundService` Android daha eski bir sürümü ile cihazlarda çalıştırılan uygulamalar kullanırken hizmetini başlatmak için yöntemi `Context.StartService`

Bu C# genişletme yöntemi, bir ön plan hizmetinin nasıl başlatılacağı örneğidir. Android 8.0 ve üzeri kullanacağı `StartForegroundService` yöntemi, aksi takdirde eski `StartService` yöntemi kullanılır.  

```csharp
public static void StartForegroundServiceComapt<T>(this Context context, Bundle args = null) where T : Service
{
    var intent = new Intent(context, typeof(T));
    if (args != null) 
    {
        intent.PutExtras(args);
    }

    if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.O)
    {
        context.StartForegroundService(intent);
    }
    else
    {
        context.StartService(intent);
    }
}
```

## <a name="registering-as-a-foreground-service"></a>Ön plan hizmet olarak kaydetme

Bir ön plan Hizmet başladıktan sonra onu kendisi ile Android çağırarak kaydetmeniz gerekir [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/). Hizmet ile başladıysanız `Service.StartForegroundService` yöntemi ancak Android hizmetini durdurun ve uygulamayı vermeyen olarak bayrak kendisini kaydetmez.

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

Hizmet çağrısıyla işlemi durduruldu durumunda `StopSelf` veya `StopService`, durum çubuğu uyarısı kaldırılacak.

## <a name="related-links"></a>İlgili bağlantılar

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Yerel Bildirimler](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
