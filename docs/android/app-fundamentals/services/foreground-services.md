---
title: Ön plan Hizmetleri
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: 47e1eda2f701b654f81f664050847677fba8bcc5
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986040"
---
# <a name="foreground-services"></a>Ön plan Hizmetleri

Bağlı hizmet veya başlangıç hizmeti özel bir tür bir ön plan hizmetidir. Bazen Hizmetleri kullanıcıların etkin bir şekilde farkında olması gereken görevler gerçekleştirir, bu hizmetler olarak da bilinir _ön plan Hizmetleri_. Kullanıcı yönergeleri ile sürüş veya yürüyen sağlayan bir uygulama bir ön plan hizmet örneğidir. Uygulama arka planda olsa bile, yine de hizmet düzgün çalışması için yeterli kaynaklara sahip olduğunu ve kullanıcının uygulamaya erişmek için hızlı ve kullanışlı bir yol olduğunu önemlidir. Bir Android uygulaması için bir ön plan hizmeti "Normal" hizmet daha yüksek önceliğe alması gereken ve ön plan hizmetini sağlamalıdır başka bir deyişle bir `Notification` hizmeti çalışıyor olduğu sürece, Android görüntülenir.
 
Ön plan hizmetini başlatmak için uygulama hizmeti başlatmak için Android söyleyen bir hedefi gönderme gerekir. Ardından hizmet kendisi Android ön plan hizmet olarak kaydetmeniz gerekir. Android 8.0 (veya üzeri) çalışan uygulamaların kullanması gereken `Context.StartForegroundService` eski Android sürümüne sahip cihazlarda çalışan uygulamaları kullanırken hizmeti başlatmak için yöntemi `Context.StartService`

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

Ön plan hizmeti başlatıldıktan sonra kendisi ile Android çağırarak kaydetmelisiniz [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/). Hizmet ile başladıysanız `Service.StartForegroundService` yöntemi ancak Android Hizmeti Durdur ve uygulamanın yanıt vermemesine olarak bayrak kendisini kaydetmez.

`StartForeground` ikisi de zorunludur, iki parametre alır:
 
* Hizmetinizi tanımlayabilmek için uygulamada benzersiz bir tamsayı değeri.
* A `Notification` nesne hizmeti çalışıyor olduğu sürece, Android için durum çubuğunda görüntülenir.

Hizmet çalışıyor sürece android bildirim için durum çubuğunda görüntülenir. En azından, bildirim hizmeti çalıştıran kullanıcı için görsel bir ipucu sağlar. İdeal olarak, kullanıcının uygulamayı ya da büyük olasılıkla bazı komut düğmeleri uygulama denetlemek için bir kısayol bildirim sağlamanız gerekir. Bu, müzik çalar örneğidir &ndash; görüntülenen bildirim düğmelerini duraklatma/play müzik, önceki şarkıyı geri veya İleri şarkıyı atlamak için olabilir. 

Bu kod parçacığı, bir hizmet ön plan hizmet olarak kaydetme, bir örnek verilmiştir:   

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

![Durum çubuğunda bildirim gösteren resim](foreground-services-images/foreground-services-01.png "durum çubuğunda bildirim gösteren resim")

Bu ekran görüntüsünde genişletilmiş bildirim hizmeti denetlemek kullanıcının olanak tanıyan iki eylemleri olan bildirim tepsisinde gösterilmiştir:

![Genişletilmiş bildirim gösteren resim](foreground-services-images/foreground-services-02.png "genişletilmiş bildirim gösteren görüntü.")

Bildirimleri hakkında daha fazla bilgi kullanılabilir [yerel bildirimleri](~/android/app-fundamentals/notifications/local-notifications.md) bölümünü [Android bildirimlerini](~/android/app-fundamentals/notifications/index.md) Kılavuzu.

## <a name="unregistering-as-a-foreground-service"></a>Ön plan hizmet olarak kaydı siliniyor

Bir hizmet kendi ön plan hizmet olarak yöntemi çağrılarak serbest listeleyebilirsiniz `StopForeground`. `StopForeground` Hizmet durdurur, ancak bu hizmet gerekirse kapatılabilir Android sinyaller ve bildirim simgesini kaldırır.

Görüntülenen durum çubuğu uyarısı geçirerek de kaldırılabilir `true` yöntemi: 

```csharp
StopForeground(true);
```

Bir çağrı ile hizmet durdurulduysa MFC'yi `StopSelf` veya `StopService`, durum çubuğu uyarısı kaldırılacak.

## <a name="related-links"></a>İlgili bağlantılar

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForeground](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Yerel Bildirimler](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
