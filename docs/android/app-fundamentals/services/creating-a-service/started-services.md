---
title: Xamarin.Android ile başlatılan Hizmetleri
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: c0aeeaad3798dd840e69c6da6d7857298f4da3c1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="started-services-with-xamarinandroid"></a>Xamarin.Android ile başlatılan Hizmetleri

## <a name="started-services-overview"></a>Başlatılan Hizmetleri'ne Genel Bakış

Başlatılan Hizmetleri doğrudan görüş veya sonuçları istemciye sağlamadan bir iş birimine genellikle gerçekleştirin. Bir iş birimine örneği bir sunucuya bir dosya yükler bir hizmettir. İstemci, aygıt bir dosyadan bir Web sitesine yüklemek için bir hizmet için bir istek hale getirir. (Uygulama ön planda hiçbir etkinlik sahip olsa bile) hizmet sessizce dosyayı karşıya yükler ve karşıya yükleme tamamlandığında, kendisini sonlanır. Başlatılan hizmet uygulama kullanıcı Arabirimi iş parçacığı çalışacağını hayata geçirmek önemlidir. Bu kullanıcı Arabirimi iş parçacığı engeller işlerini yapmak için bir hizmet olması durumunda onu gerekir oluşturun ve gerektiği gibi iş parçacıklarının dispose anlamına gelir.

Bağımlı bir hizmet farklı olarak hiçbir "saf" başlatılan hizmet ve istemcileri arasındaki iletişim kanalı yok. Bu bir başlangıç hizmeti bazı farklı yaşam döngüsü yöntemlerinden ilişkili hizmet daha gerçekleştireceksiniz anlamına gelir. Aşağıdaki listede ortak bir başlatılan hizmet yaşam döngüsü yöntemleri vurgular:

* `OnCreate` &ndash; Hizmet ilk kez başlatıldığında bir kez çağrılır. Başlatma kodu nereye uygulanmalıdır budur.
* `OnBind` &ndash; Ancak başlatılan hizmet kendisiyle ilişkili bir istemci genellikle yok, bu yöntem tüm hizmet sınıfları tarafından uygulanmalıdır. Bu nedenle, yalnızca bir başlangıç hizmeti döndürür `null`. Buna karşılık, uygulamak ve döndürmek (bağlı hizmet ve başlatılan hizmet birleşimi olan) bir karma hizmet sahip bir `Binder` istemci için.
* `OnStartCommand` &ndash; Hizmet yanıt için bir çağrı olarak ya da başlatmak her istek için adlı `StartService` veya bir sistem yeniden başlatması. Hizmeti herhangi bir uzun süre çalışan görev burada başlayabilirsiniz budur. Yöntem bir `StartCommandResult` gösteren değer nasıl veya yetersiz bellek nedeniyle bir kapatma sonrasında hizmetin yeniden başlatılması sistem işlemelidir. Bu çağrı ana iş parçacığı üzerinde gerçekleştirilir. Bu yöntem aşağıdaki daha ayrıntılı olarak açıklanmıştır.
* `OnDestroy` &ndash; Hizmet yok, bu yöntem çağrılır. Tüm son gerçekleştirmek için kullanılan temizleme gerekli.

Başlatılan bir hizmet için önemli bir yöntemdir `OnStartCommand` yöntemi. Her durumda çağrılacak hizmet bazı iş yapmak için bir istek alır. Aşağıdaki kod parçacığını örneğidir `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

İlk parametre bir `Intent` gerçekleştirmek için iş hakkındaki meta verileri içeren bir nesne. İkinci parametre içeren bir `StartCommandFlags` yöntem çağrısının hakkında bazı bilgiler sağlayan değeri. Bu parametre iki olası değerden birini içerir:

* `StartCommandFlag.Redelivery` &ndash; Bunun anlamı `Intent` re teslim önceki `Intent`. Bu değer hizmet döndürülen olduğunuzda sağlanan `StartCommandResult.RedeliverIntent` ancak bunu doğru şekilde kapatılabilir önce durduruldu.
* `StartCommandFlag.Retry` &dash; Bu değer, önceki bir zaman alınan `OnStartCommand` çağrı başarısız oldu ve Android hizmeti aynı amacıyla önceki başarısız girişim olarak yeniden başlatmak çalışıyor.
 
Son olarak, üçüncü parametre istek tanımlar uygulaması için benzersiz olan bir tamsayı değerdir. Birden çok arayanlar aynı hizmet nesnesi çağırabilir mümkündür. Bu değer, bir hizmeti başlatmak için belirtilen bir isteğin sahip bir hizmet durdurma isteği ilişkilendirmek için kullanılır. Bölümünde daha ayrıntılı incelenecektir [hizmetin durdurulması](#Stopping_the_Service). 

Değer `StartCommandResult` hizmeti tarafından hangi hizmet kaynak kısıtlamaları nedeniyle sonlandırılırsa yapmak, Android için bir öneri olarak döndürülür. Üç olası değerler için `StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)** &ndash; This value tells Android that it is not necessary to restart the service that it has killed. Bu örnek olarak, bir uygulamada bir galeri küçük resim oluşturur bir hizmeti kullanmayı düşünün. Hizmet sonlandırılırsa, küçük resim hemen yeniden oluşturmak önemli değil &ndash; küçük resim uygulama İleri çalıştırıldığında yeniden oluşturulabilir.
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash; bu Android hizmetini yeniden başlatmak için ancak hizmet başlatıldı son hedefi teslim değil bildirir. İşlemek için hiçbir bekleyen hedefleri varsa sonra bir `null` hedefi parametresi için sağlanan. Bunun bir örneğini bir müzik çalar uygulaması olabilir; Hizmet hazır müzik için yeniden başlatılır, ancak son şarkının yürütülür. 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)** &ndash; This value is will tell Android to restart the service and re-deliver the last `Intent`. Buna örnek olarak, bir uygulama için bir veri dosyası indiren bir hizmetidir. Hizmet sonlandırılırsa, veri dosyası hala indirilmesi gerekir. Döndürerek `StartCommandResult.RedeliverIntent`, Android, hizmete ayrıca sağlar (olan indirmek için dosyanın URL'sini) hedefi hizmet yeniden başlatıldığında. Bu yüklemeyi yeniden başlatın veya sürdürün (tam uygulama kodunun bağlı olarak) etkinleştirir.

Dördüncü bir değeri var. `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`. Bu değer tarafından döndürülen `OnStartCommand` ve Android hizmeti nasıl devam açıklar sonlandırıldı. Bu değer genellikle bir hizmeti başlatmak için kullanılmaz.

Anahtar yaşam döngüsü olayları başlatılan hizmetinin bu diyagramda gösterilmektedir: 

![Yaşam döngüsü yöntemleri çağrılmadan sırayı gösteren bir diyagram](started-services-images/started-service-01.png "yaşam döngüsü yöntemleri çağrılmadan sipariş gösteren diyagram.")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>Hizmeti durduruluyor

Başlatılan hizmet süresiz olarak çalışmaya devam edecek; Yeterli sistem kaynakları var olduğu sürece android hizmetin çalışmasını tutar. İstemci hizmetini durdurmanız gerekir ya da kendi iş tamamlandığında hizmeti kendisini vermeyebilir. Bir hizmeti durdurmak için iki yolu vardır: 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)** &ndash; A client (such as an Activity) can request a service stop by calling the `StopService` method: 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)** &ndash; A service may shut itself down by invoking the `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>Bir hizmeti durdurmak için startId kullanma

Birden çok arayanlar, bir hizmetin başlatılması isteyebilir. Bekleyen başlatma isteği varsa, hizmetin kullanabileceği `startId` içine geçirilen `OnStartCommand` erken durduruldu hizmetin önlemek için. `startId` Son çağrısına karşılık gelir `StartService`ve her kez çağırıldığında arttırılır. Bu nedenle, bir sonraki istek için `StartService` henüz bir çağrı sonuçlandı değil `OnStartCommand`, hizmet çağırabilirsiniz `StopSelfResult`, en son değeri geçirme `startId` onu aldı (yalnızca çağırmak yerine `StopSelf`). Çağrı, `StartService` henüz karşılık gelen çağrıyı sonuçlandı değil `OnStartCommand`, sistem hizmeti, çünkü durdurmaz `startId` kullanılan `StopSelf` çağrısı değil en son sürüme karşılık `StartService` çağırın.


## <a name="related-links"></a>İlgili bağlantılar

- [StartedServicesDemo (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/StartedServicesDemo/)
- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service)
- [Android.App.StartCommandFlags](https://developer.xamarin.com/api/type/Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](https://developer.xamarin.com/api/type/Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Android.Content.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent)
- [Android.OS.Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Android.Widget.Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
- [Durum çubuğu simgeleri](http://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
