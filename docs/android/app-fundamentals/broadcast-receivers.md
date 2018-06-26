---
title: Xamarin.Android alıcılar yayını
description: Bu bölümde bir yayın alıcı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/20/2018
ms.openlocfilehash: 6b2e316eaf67e51801be4fcd670e80ec81c8ff08
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935405"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Xamarin.Android alıcılar yayını

_Bu bölümde bir yayın alıcı kullanmayı açıklar._

## <a name="broadcast-receiver-overview"></a>Yayın alıcı genel bakış

A _yayın alıcı_ bir uygulamanın iletileri yanıtlayın izin veren bir Android bileşenidir (bir Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) Android işletim sistemi veya bir uygulama tarafından yayınlanan. Yayınları izleyin bir _yayımlama-abone olma_ modeli &ndash; yayımlanan ve olay ilginizi çekiyor mu bu bileşenler tarafından alınan yayın olaya neden olur. 

Android yayınları iki tür tanımlar:

* **Açık yayın** &ndash; belirli bir uygulama bu tür yayınlar hedefleyin. Açık bir yayın en yaygın kullanımı bir etkinlik başlatmaktır. Bir telefon numarasını aramak bir uygulamanın ihtiyacı olduğunda açık bir yayın örneği; Android ve telefon numarası boyunca geçişi çevrilecek telefon uygulaması hedefleyen amacına gönderir. Android, ardından telefon uygulama amacı yönlendirir.
* **Örtük yayın** &ndash; bu yayınları cihazdaki tüm uygulamalar için gönderilir. Örtük bir yayın örneğidir `ACTION_POWER_CONNECTED` hedefi. Bu amacı, Android cihazın pil şarj olduğunu algılar her zaman yayımlanır. Android, bu amacı, bu olay için kayıtlı tüm uygulamalar için yönlendirir.

Öğesinin bir alt yayın alıcıdır `BroadcastReceiver` türü ve onu geçersiz kılması gerekir [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) yöntemi. Android yürütülecek `OnReceive` ana iş parçacığında, bu nedenle bu yöntem tasarlanmış hızlı bir şekilde çalıştırmak için. İş parçacığı içinde olduğunda dikkat'in alınması gereken `OnReceive` yöntemi tamamlandığında Android işlemi sonlandırabilir olduğundan. Bir yayın alıcı uzun süre çalışan iş gerçekleştirmelisiniz sonra zamanlamak için önerilen bir _iş_ kullanarak `JobScheduler` veya _Firebase iş dağıtıcı_. Bir iş ile çalışma zamanlaması ayrı bir Kılavuzu'nda incelenecektir.

Bir _hedefi filtre_ Android düzgün iletileri yönlendirmek için bir yayın alıcı kaydetmek için kullanılır. Hedefi süzgeci çalışma zamanında belirlenebilir (Bu bazen olarak adlandırılır bir _bağlamı kayıtlı alıcı_ veya as _dinamik kayıt_) veya Android bildirimi (bir statikolaraktanımlanabilir_bildirimi kayıtlı alıcı_). Xamarin.Android sağlayan bir C# özniteliği, `IntentFilterAttribute`, statik olarak kaydedin (Bu açıklanır bu kılavuzun ilerleyen bölümlerinde daha ayrıntılı) hedefi filtre. Android 8. 0'dan başlayarak, onu örtük bir yayın için statik olarak kaydetmek bir uygulama için olası değil.

Bir uygulama çalışırken bildirimi kayıtlı bir alıcı yanıt verebilir sırada bağlam kayıtlı bir alıcı yalnızca yayınlara yanıt bildirimi kayıtlı alıcı bağlam kayıtlı alıcı arasındaki birincil fark olduğu Uygulama çalışmıyor olsa bile yayınlar.  

Bir yayın alıcı yönetme ve yayınları göndermek için iki API kümesi vardır:

1. **`Context`** &ndash; `Android.Content.Context` Sınıfı, sistem genelinde olaylarına yanıt vereceği yayın bir alıcı kaydetmek için kullanılabilir. `Context` Sistem genelinde yayınları yayımlamak için de kullanılır.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Bu bir API aracılığıyla kullanılabilir olan [Xamarin destek kitaplığı v4 NuGet paketi](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Bu sınıf, yayınları ve bunları kullanarak uygulama bağlamında yalıtılmış yayın alıcıları tutmak için kullanılır. Bu sınıf diğer uygulamaların yalnızca uygulama yayınlara yanıt veya özel alıcılar için ileti gönderme önlemek için yararlı olabilir.

Bir yayın alıcı iletişim kutuları görüntülenmeyebilir ve bir aktiviteyi yayın alıcı içinden başlatmak için kesinlikle önerilmez. Bir yayın alıcı kullanıcıya bildir gerekir, bir bildirim yayımlamalısınız.

Bir yayın alıcı içinde hizmetinden başlatmak veya bağlamak mümkün değildir. 

Bu kılavuz, yayın alıcı oluşturma ve böylece yayınları alabilirsiniz kaydetme ele alınacaktır.

## <a name="creating-a-broadcast-receiver"></a>Bir yayın alıcı oluşturma

Yayın alıcı içinde Xamarin.Android oluşturmak için bir alt uygulama gereken `BroadcastReceiver` sınıfı, kendisiyle ekleyin `BroadcastReceiverAttribute`ve geçersiz kılma `OnReceive` yöntemi:

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.

        String value = intent.GetStringExtra("key");
    }
}
```

Xamarin.Android sınıfı derlediğinde, AndroidManifest gerekli meta alıcı kaydetmek için veri ile güncelleştirir. Bir statik olarak kayıtlı yayın alıcısı için `Enabled` doğru şekilde ayarlanması gerekir `true`, aksi takdirde Android alıcı örneğini oluşturmak mümkün olmaz.
 
`Exported` Özelliği, yayın alıcı uygulamanın dışından gelen iletileri alıp alamayacağını denetler. Özellik açıkça ayarlanmazsa, özelliğin varsayılan değeri yayın alıcı ile ilişkili amacı filtreleri varsa göre Android tarafından belirlenir. Yayın alıcısı için en az bir hedefi filtre sonra Android, varsayın `Exported` özelliği `true`. Yayın alıcı ile ilişkili hiçbir hedefi filtrelerinin sonra Android, değer olduğunu olmadığını varsayar `false`. 

`OnReceive` Yöntemi bir başvuru alır `Intent` yayın alıcıya gönderilen. Bu yapar gönderenin yayın alıcıya değerleri geçirmek için hedefinin mümkün olur.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Statik olarak yayınlanan bir alıcı bir hedefi Filtresi ile kaydetme

Zaman bir `BroadcastReceiver` ile donatılmış [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android gerekli ekleyecek `<intent-filter>` Android öğesine derleme zamanında bildirim. Aşağıdaki kod parçacığında, bir aygıt (kullanıcı tarafından uygun Android izinleri verilmiş varsa) önyükleme tamamlandığında, çalışacak bir yayın alıcı örneğidir:

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

Özel amaçlar için yanıtlar bir hedefi filtresi oluşturmak mümkündür. Aşağıdaki örnek göz önünde bulundurun: 

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

Android 8.0 (API düzeyi 26) hedef uygulamaları ya da daha yüksek statik olarak örtük bir yayın için kayıt. Uygulamalar, açık bir yayın için hala statik olarak kaydedebilir. Bu kısıtlamanın dışında örtük yayınların küçük listesi verilmiştir. Bu özel durumlar açıklanmaktadır [örtük yayın özel durumları](https://developer.android.com/guide/components/broadcast-exceptions.html) Android belgelerinde Kılavuzu. Örtük yayınları ilginizi çekiyor mu uygulamaları kullanarak bu nedenle dinamik olarak gerçekleştirmelisiniz `RegisterReceiver` yöntemi. Bu sonraki açıklanmıştır.

### <a name="context-registering-a-broadcast-receiver"></a>Bir yayın alıcı bağlam kaydetme

Bağlam (dinamik kayıt olarak da bilinir) kaydı bir alıcı çağırarak gerçekleştirilir `RegisterReceiver` yöntemi ve yayın alıcı olmalıdır çağrısıyla kaydı `UnregisterReceiver` yöntemi. Kaynakları sızmasını önlemek için artık (etkinlik veya hizmet) bağlamının ilgili olduğunda alıcı kaydı önemlidir. Örneğin, bir hizmet güncelleştirmeleri kullanıcıya gösterilecek kullanılabilir bir etkinlik bildirmek için amacına yayın. Etkinlik başladığında, bu amaçlar için kaydetmeniz. Ne zaman etkinlik arka plan içine taşınır ve güncelleştirmeleri görüntülemek için kullanıcı Arabirimi artık görünür olduğundan kullanıcı artık görünür, onu alıcı kaydı. Aşağıdaki kod parçacığını kayıt ve aktivite bağlamında yayın bir alıcı kaydı örneğidir:

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity 
{
    MySampleBroadcastReceiver receiver;

    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver()

        // Code omitted for clarity
    }

    protected override OnResume() 
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }

    protected override OnPause() 
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

Etkinlik ön alana geldiğinde önceki örnekte kullanarak özel bir amaç için dinler yayın bir alıcı kaydeder `OnResume` yaşam döngüsü yöntemi. Arka plan içine etkinlik hareket ederken `OnPause()` yöntemi alıcı kaydı.

## <a name="publishing-a-broadcast"></a>Bir yayın yayımlama

Bir yayın hedefi nesne oluşturma ve dağıtma ile cihazda yüklü olan tüm uygulamalara yayımlanabilir `SendBroadcast` veya `SendOrderedBroadcast` yöntemi.  

1. **Context.SendBroadcast yöntemleri** &ndash; çeşitli uygulamalarını bu yöntem vardır.
   Bu yöntemler tüm sistemin yönelik amacı yayın. Yayın alıcıları thatwill belirsiz bir sırayla hedefi alırsınız. Bu, büyük bir bölümünü esneklik sağlar ancak kaydetmek ve amacı almak diğer uygulamalar için mümkün olduğu anlamına gelir. Bu bir güvenlik riski oluşturabilir. Uygulamalar, yetkisiz erişimi önlemek için ek güvenlik uygulamak gerekebilir. Olası bir çözüm kullanmaktır `LocalBroadcastManager` hangi yalnızca gönderme uygulamasının özel alanı içinde ileti. Bu kod parçacığını birini kullanarak amacına gönderilmesi nasıl bir örnektir `SendBroadcast` yöntemleri:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```

    Bu kod parçacığında kullanarak bir yayın gönderme başka bir örnektir `Intent.SetAction` eylemi belirlemek üzere yöntem:

    ```csharp 
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash; yöntem budur çok benzer `Context.SendBroadcast`, hedefi recievers kaydedilen düzende alıcılar anda yayımlanmış tek olacağını olan arasındaki fark.

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

[Xamarin destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) adlı bir yardımcı sınıfı sağlar [ `LocalBroadcastManager` ](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html). `LocalBroadcastManager` Gönderip yayınları cihazdaki diğer uygulamalardan istemediğiniz uygulamalar için tasarlanmıştır. `LocalBroadcastManager` İle kayıtlı bu yayın alıcıları ve bu uygulamanın bağlamında iletileri yalnızca yayımlayacak `LocalBroadcastManager`. Bu kod parçacığı ile yayın bir alıcı kaydı örneğidir `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

Cihazdaki diğer uygulamalar ile yayımlanan mesajlarını alamıyor `LocalBroadcastManager`. Bu kod parçacığını bir hedefi kullanarak gönderileceği gösterilmektedir `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
intent.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>İlgili bağlantılar

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [Hedefi](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Intentfilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Android yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications.md)
- [Android kitaplığı v4 destek](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
