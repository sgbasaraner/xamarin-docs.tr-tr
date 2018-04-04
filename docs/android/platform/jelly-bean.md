---
title: Jelly Çekirdeklere özellikleri
description: 'Bu belgede Android 4.1 sunulan geliştiriciler için yeni özellikler üst düzey bir genel bakış sağlar. Bu özellikler şunlardır: Gelişmiş bildirimleri, büyük dosyalar, multimedya, eşler arası ağ bulma, animasyonları, yeni izinler güncelleştirmeleri paylaşmak için Android ışını güncelleştirmeler.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1d8068ccfc8d0f159a88704370261ec5f20d8b7c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="jelly-bean-features"></a>Jelly Çekirdeklere özellikleri

_Bu belgede Android 4.1 sunulan geliştiriciler için yeni özellikler üst düzey bir genel bakış sağlar. Bu özellikler şunlardır: Gelişmiş bildirimleri, büyük dosyalar, multimedya, eşler arası ağ bulma, animasyonları, yeni izinler güncelleştirmeleri paylaşmak için Android ışını güncelleştirmeler._



## <a name="overview"></a>Genel Bakış

Android 4.1 (API düzeyi 16) olarak da bilinen "Jelly Çekirdeklere", sürüm 9 Temmuz 2012 oluştu. Bu makalede Xamarin.Android kullanan geliştiriciler için Android 4.1'deki yeni özelliklerin bazıları üst düzey bir giriş sağlar. Sunulan bu yeni özelliklerin bir etkinlik, kamera için yeni ses efekti ve uygulama yığın gezinti için geliştirilmiş destek başlatmak için animasyonları geliştirmeler bazılarıdır. Artık, kesme ve yapıştırma amaçları ile mümkündür.

Android uygulamaları kararlılığını kararsız içerik sağlayıcılar üzerindeki bağımlılığı ayırma olanağı geliştirildi. Yalnızca bunları kullanmaya etkinlik tarafından erişilebilir olmamasını hizmetleri de yalıtılmış olabilir.

Destek ağ hizmeti bulma kullanmak için Bonjour, UPnP veya çok noktaya yayın DNS hizmetleri göre eklenmiştir. Artık, metni, eylem düğmeleri ve büyük görüntüleri biçimlendirdiğiniz için daha zengin bildirimleri de mümkündür.

Son olarak yeni izinlerinden Android 4.1 eklenmiştir.



## <a name="requirements"></a>Gereksinimler

Xamarin.Android uygulamaları geliştirmek için Jelly Çekirdeklere Xamarin.Android kullanılması 4.2.6 ya da daha yüksek ve Android 4.1 (API düzeyi 16) yüklenmesi Android SDK Yöneticisi üzerinden aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Android 4.1 Android SDK Yöneticisi'nde seçme](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>Yenilikler



### <a name="animations"></a>Animasyon

Etkinlikler yakınlaştırma animasyonları veya özel animasyon kullanarak başlatılmasını `ActivityOptions` sınıfı. Aşağıdaki yeni yöntemleri animasyonlarına desteklemek için sağlanır:

-   `MakeScaleUpAnimation` – Bu bir başlangıç konumu ve boyutu ekranında etkinliği penceresinden ölçeklendirir bir animasyon oluşturur.
-   `MakeThumbnailScaleUpAnimation` – Küçük resim görüntüsünden ekranında belirtilen konumdan Bu ölçeklendirme yaparken bir animasyon oluşturur.
-   `MakeCustomAnimation` – Bu uygulamada kaynaklardan bir animasyon oluşturur. Etkinlik açıldığında için bir animasyon ve etkinlik durduğunda için başka bir yoktur.


Yeni `TimeAnimator` sınıfı bir arabirim sunar `TimeAnimator.ITimeListener` , bildirebilir bir uygulama bir çerçeve animasyonda her değiştiğinde. Örneğin, aşağıdaki uygulanması göz önünde bulundurun `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

Ve sınıf örneği kullanmak için şimdi `TimeAnimator` oluşturulduğu ve dinleyici ayarlayın:

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

Olarak `TimeAnimator` örneğinin çalıştığından, çağıracaktır `ITimeAnimator.ITimeListener`, hangi sonra oturum nasıl çalıştığını ve ne kadar süreyle, son yedeklemeden bu yana yöntemi edilmiş olarak çağrılmış Animator'da uzun olmuştur.



### <a name="application-stack-navigation"></a>Uygulama yığın gezinme

Android 4.1 Android 3. 0'tanıtılan uygulama yığın Gezinti artırır. Belirterek `ParentName` özelliği `ActivityAttribute`, Android, uygun üst etkinlik açabilir, kullanıcı bastığında [düğmesi yukarı](http://developer.android.com/design/patterns/navigation.html#up-vs-back) eylem çubuğunda - Android tarafındanbelirtilenetkinliğiörneğioluşturulmaz`ParentName`özelliği. Bu, belirli bir görevi yapmak etkinlikleri hiyerarşisini korumak uygulamalar sağlar.

Çoğu uygulama ayarı için `ParentName` faaliyete Android uygulama yığınını; gezinmek için doğru davranışı sağlamak yeterli bilgi içindir Android gerekli geri yığının hedefleri bir dizi her üst etkinliğinin oluşturarak oluşturur. Ancak, bu yapay uygulama yığını olduğundan, yapay her etkinlik doğal etkinlik olurdu kaydedilmiş durumu olmayacaktır. Yapay üst etkinlik kaydedilen bir duruma sağlamak için bir etkinlik geçersiz kılabilir `OnPrepareNavigationUpTaskStack` yöntemi. Bu yöntem alan bir `TaskStackBuilder` hedefi koleksiyonu olacaktır örneği nesneleri Android geri yığını oluşturmak için kullanır. Yapay etkinlik oluşturulduğunda uygun durum bilgilerini alacak böylece etkinlik bu hedefleri değiştirebilir.

Daha karmaşık senaryolar için Gezinti yukarı davranışını işlemek ve geri yığını oluşturmak için kullanılabilir etkinlik sınıf üzerinde yeni yöntemleri vardır:

-   `OnNavigateUp` – Bu yöntemi geçersiz kılarak özel bir eylemi gerçekleştirmek mümkündür zaman <span class="ui">yukarı</span> düğmesine basıldığında.
-   `NavigateUpTo` – Bu yöntem çağırma geçerli etkinliğinden tarafından verilen bir hedefi belirtilen etkinlik gitmek uygulama oluşturulmasına neden olur.
-   `ParentActivityIntent` – Bu, geçerli etkinliği üst etkinliğini başlatacak amacına elde etmek için kullanılır.
-   `ShouldUpRecreateTask` – Bu yöntem, üst etkinlik kadar gitmek için yapay geri yığını oluşturulmalıdır varsa sorgulamak için kullanılır. Döndürür `true` yapay yığın oluşturulması gerekiyorsa. 
-   `FinishAffinity` – Bu yöntem çağırma geçerli etkinliği ve aynı görev benzeşimi sahip tüm etkinlikler altındaki geçerli görev tamamlanır.
-   `OnCreateNavigateUpTaskStack` – Yapay yığını nasıl oluşturulduğunu üzerinde tam denetime sahip olmasını gerekli olduğunda bu yöntem geçersiz kılındı.




### <a name="camera"></a>Kamera

Yeni bir arabirimi `Camera.IAutoFocusMoveCallback`, otomatik odak başlatıldığında veya taşıma durduruldu algılamak için kullanılabilir. Bu yeni arabirim örneği aşağıdaki kod parçacığında görülebilir:

```csharp
public class AutoFocusCallbackActivity : Activity, Camera.IAutoFocusCallback
{
    public void OnAutoFocus(bool success, Camera camera)
    {
        // camera is an instance of the camera service object.

        if (success)
        {
            // Auto focus was successful - do something here.
        }
        else
        {
            // Auto focus didn't happen for some reason - react to that here.
        }
    }
}
```

Yeni sınıf `MediaActionSound` çeşitli medya eylemler için ses üretmek için API'nin kümesi sağlar. Kamera ile enum tarafından tanımlanan bunlar oluşabilecek çeşitli eylemler vardır `Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` – Odaklanma tamamlandığında çalınır bu ses.
-   `MediaActionSoundType.ShutterClick` – Hala görüntü resim alındığında bu ses yürütülür.
-   `MediaActionSoundType.StartVideoRecording` – Bu ses kullanılan video kaydı başlangıcını gösterir.
-   `MediaActionSoundType.StopVideoRecording` – Bu ses, video kaydı sonunu belirtmek üzere yürütülür.


Nasıl kullanılacağına ilişkin bir örnek `MediaActionSound` sınıfı aşağıdaki kod parçacığında görülen:

```csharp
var mediaActionPlayer = new MediaActionSound();

// Preload the sound for a shutter click.
mediaActionPlayer.Load(MediaActionSoundType.ShutterClick);
var button = FindViewById<Button>(Resource.Id.MyButton);

// Play the sound on a button click.
button.Click += (sender, args) => mediaActionPlayer.Play(MediaActionSoundType.ShutterClick);

// This releases the preloaded resources. Don’t make any calls on
// mediaActionPlayer after this.
mediaActionPlayer.Release();
```



### <a name="connectivity"></a>Bağlantı



#### <a name="android-beam"></a>Android ışını

Android ışını birbirleri ile iletişim kurmak iki Android cihazları sağlayan bir temel NFC teknolojisidir. Android 4.1 büyük dosya aktarımı için daha iyi destek sağlar. Yeni yöntemi kullanırken `NfcAdapter.SetBeamPushUris()` Android değiştirmek alternatif taşıma mekanizmaları (örneğin, Bluetooth) arasında bir hızlı aktarım hızı elde etmek için.



#### <a name="network-services-discovery"></a>Ağ Hizmetleri bulma

Android 4.1 yeni API'nin çok noktaya yayın DNS tabanlı hizmet bulma için içerir.
Bu algılamak ve yazıcı, kamera ve ortam aygıtları gibi diğer cihazlar için Wi-Fi bağlanmak için bir uygulama sağlar. Bu yeni API'nin olan `Android.Net.Nsd` paket.

Diğer hizmetler tarafından kullanılan bir hizmet oluşturmak için `NsdServiceInfo` sınıfı, bir hizmetin özelliklerini tanımlayan nesne oluşturmak için kullanılır. Bu nesne için sağlanmıştır `NsdManager.RegisterService()` uygulaması birlikte `NsdManager.ResolveListener`. Uygulamaları `NsdManager.ResolveListener` başarılı kaydı bildirmek için ve hizmet kaydı için kullanılır.

Ağ ve uyarlamasını Hizmetleri bulmak için `Nsd.DiscoveryListener` geçirilen `NsdManager.discoverServices()`.



#### <a name="network-usage"></a>Ağ kullanımı

Yeni bir yöntemi `ConnectivityManager.IsActiveNetworkMetered` ölçülen bir ağa bağlı olup olmadığını denetlemek bir aygıt sağlar. Bu yöntem, doğru bir şekilde veri işlemleri için pahalı ücretleri olabilir kullanıcılar bilgilendirerek veri kullanımı yönetmenize yardımcı olmak için kullanılabilir.



#### <a name="wifi-direct-service-discovery"></a>WiFi doğrudan hizmet bulma

`WifiP2pManager` Sınıfı desteklemek için Android 4.0 sunulmuştur *zeroconf*. Zeroconf (sıfır yapılandırma ağ iletişimi) cihazlar (bilgisayarlar, yazıcılar, telefonları) ağlara otomatik olarak bağlanmaya izin veren bir teknikler İnsan ağ işleçleri veya özel yapılandırma sunucularına etkileşimine kümesidir.

Jelly Çekirdeklere içinde `WifiP2pManager` kullanarak aygıtları bulabilir *Bonjour* veya *Upnp*. Bonjour zeroconf Apple uygulamasıdır. UPnP de zeroconf destekleyen ağ protokolleri ayarlanır. Eklenen aşağıdaki yöntemlerden `WiFiP2pManager` Wi-Fi hizmet bulma desteklemek için:

-   `AddLocalService()` – Bu yöntem kullanılır eş tarafından bulma için bir uygulamayı bir hizmet olarak Wi-Fi bildir.
-   `AddServiceRequest(` ) – Bu yöntem Framework'e hizmet bulma isteği göndermektir. Wi-Fi hizmet bulma başlatmak için kullanılır.
-   `SetDnsSdResponseListeners()` – Bu yöntem, Bonjour bulma istekleri için bir yanıt alma çağrılacak geri aramaları kaydetme için kullanılır.
-   `SetUpnpServiceResponseListener()` – Bu yöntem, Upnp bulma isteklerine yanıt alma çağrılacak geri aramaları kaydetme için kullanılır.




### <a name="content-providers"></a>İçerik sağlayıcılar

`ContentResolver` Sınıfının yeni bir yöntemi aldı `AcquireUnstableContentProvider`. Bu yöntem, bir "kararsız" içerik sağlayıcı almak bir uygulama sağlar. Normalde, bir uygulama bir içerik sağlayıcı edinir ve içerik sağlayıcı çöküyor, bu nedenle uygulama kullanacak. Bu yöntem çağrısı ile bir uygulama kilitleniyor içerik sağlayıcısına çökmesi durumunda değil. Bunun yerine, `Android.OS.DeadObjectionException` içerik sağlayıcısına yerine geçti uygulamanın bildirmek için çağrılarından içerik sağlayıcısı oluşturulur. Diğer uygulamalardan içerik sağlayıcılarla kullanılırken "kararsız" içerik sağlayıcı yararlıdır – başka bir uygulama buggy kodundan başka bir uygulama etkileyecek daha az olasıdır.



### <a name="copy-and-paste-with-intents"></a>Kopyala ve Yapıştır amaçları ile

`Intent` Sınıfı şimdi olabilir bir `ClipData` ilişkili nesne `Intent.ClipData` özelliği. Amacıyla aktarılacak panodan ek veriler için bu yöntem sağlar. Örneği `ClipData` bir veya daha fazla içerebilir `ClipData.Item`. `ClipData.Item`kişinin türlerinden birini öğeleri:

-   **Metin** – bu metin, her iki HTML herhangi bir dize veya biçimini yerleşik Android stili tarafından desteklenen herhangi bir dize yayılır.
-  **Hedefi** – tüm `Intent` nesnesi.
-   **URI** – Bu, bir HTTP yer işareti veya içerik sağlayıcısına URI gibi herhangi bir URL olabilir.




### <a name="isolated-services"></a>Yalıtılmış Hizmetleri

Yalıtılmış bir hizmeti, kendi özel işlem altında çalışan ve kendi hiçbir izinlerine sahip bir hizmettir. Hizmet ile yalnızca iletişim olduğunda hizmeti başlatılıyor ve kendisine hizmeti API'si bağlama. Bir hizmet olarak yalıtılmış özelliği ayarlanarak bildirmeniz mümkündür `IsolatedProcess="true"` içinde `ServiceAttribute` , hizmet sınıfı daireler donatır.


### <a name="media"></a>Medya

Yeni `Android.Media.MediaCodec` sınıfı, alt düzey media codec bileşenleri için bir API sağlar. Uygulamalar düşük düzey hangi codec bileşenleri cihazda kullanılabilir olduğunu bulmak için sistemi sorgulayabilirsiniz.

Yeni `Android.Media.Audiofx.AudioEffect` alt sınıfların, yakalanan ses ön işleme ek ses desteklemek için eklenmiştir:

-   `Android.Media.Audiofx.AcousticEchoCanceler` – Bu sınıf, yakalanan ses sinyal uzak taraftan sinyal kaldırmak için ses önceden işlemek için kullanılır. Örneğin, bir sesli iletişimi uygulamasından Yankı kaldırılıyor.
-   `Android.Media.Audiofx.AutomaticGainControl` – Bu sınıf, yakalanan sinyal artırma veya çıkış sinyal sabit böylece giriş sinyali azaltmayı normalleştirmek için kullanılır.
-   `Android.Media.Audiofx.NoiseSuppressor` – Bu sınıf arka plan gürültü yakalanan sinyalden kaldırır.


Tüm cihazlar bu etkilerle destekleyecektir. Yöntem `AudioEffect.IsAvailable` söz konusu ses efekti uygulamayı çalıştırmayı aygıtta destekleyip desteklemediğini görmek için bir uygulama tarafından çağrılmalıdır.

`MediaPlayer` Sınıfı şimdi gapless kayıttan yürütmeyi destekleyen `SetNextMediaPlayer()` yöntemi. Bu yeni yöntemi geçerli media player, kayıttan yürütme tamamlandığında başlatmak için İleri MediaPlayer belirtir.

Aşağıdaki yeni sınıflar, burada medya yürütülür seçmek için standart düzenekler ve kullanıcı Arabirimi sağlar:

-   `MediaRouter` – Bu sınıf medya kanaldan dış konuşmacılar cihaza veya diğer aygıtlar yönlendirme denetlemek uygulamaları sağlar.
-   `MediaRouterActionProvider` ve `MediaRouteButton` – bu sınıfların tutarlı bir kullanıcı Arabirimi seçerek ve media çalma sağlamak.




### <a name="notifications"></a>Bildirimler

Android 4.1 uygulamaların daha fazla esneklik ve bildirimleri görüntüleme ile denetim sağlar. Uygulamaları şimdi daha büyük ve daha iyi bildirimleri kullanıcılara göster. Yeni bir yöntemi `NotificationBuilder.SetStyle()` bildirimlerini ayarlanacak yeni üç yeni stil biri için izin verir:

-   `Notification.BigPictureStyle` – Bu görüntüyü bunları olacaktır bildirimleri oluşturacak bir yardımcı sınıfıdır. Aşağıdaki resimde, büyük bir görüntü içeren bir bildirim örneği gösterilmiştir:


 [![BigPictureStyle bildirim örnek ekran görüntüsü](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` – Bu, birden çok e-posta gibi metin satırı olacak bildirimleri oluşturacak bir yardımcı sınıfıdır. Bu yeni bir bildirim stili örneği aşağıdaki ekran görüntüsünde görebilirsiniz:


 [![BigTextStyle bildirim örnek ekran görüntüsü](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` – Bu bu ekran görüntüsünde gösterildiği gibi bir e-posta iletisinden parçacıkları dizelerinin listesini içeren bildirimi oluşturacak bir yardımcı sınıfı.


 [![Notification.InboxStyle bildirim örnek ekran görüntüsü](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

Bildirim normal ya da daha büyük stili kullanırken en fazla iki eylem düğmeleri bir bildirim iletisi alt kısmındaki eklemek mümkündür.
Buna örnek olarak, eylem düğmeleri bildirim alt kısmında görünür olduğu aşağıdaki ekran görüntüsünde görülebilir:

 [![Eylem düğmesi bir bildirim iletisi gösterilen örnek ekran görüntüsü](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

`Notification` Sınıfı, bir bildirim için beş öncelik düzeyleri birini belirtmek üzere bir geliştirici izin yeni sabitleri aldı. Bu bir bildirimi kullanılarak ayarlanabilir `Priority` özelliği.



### <a name="permissions"></a>İzinler

Aşağıdaki yeni izinler eklenmiştir:

-   `READ_EXTERNAL_STORAGE` -Uygulama dış depolama birimine salt okunur erişim gerektirir. Şu anda tüm uygulamalar varsayılan olarak okuma erişimine sahip, ancak uygulama okuma erişimi açıkça talep Android gelecek sürümlerini gerektirir.
-   `READ_USER_DICTIONARY` -Kullanıcının word sözlüğüne bir okuma erişimi sağlar.
-   `READ_CALL_LOG` -Arama günlüğünü okuyarak gelen ve giden çağrıları hakkında bilgi edinmek bir uygulama sağlar.
-   `WRITE_CALL_LOG` -Telefonda çağrısı günlüğüne yazılması bir uygulama sağlar.
-   `WRITE_USER_DICTIONARY` -Kullanıcının word sözlüğe yazmak bir uygulama sağlar.


Dikkat edilecek önemli bir değişiklik `READ_EXTERNAL_STORAGE` – şu anda bu izni Android tarafından otomatik olarak verilir. Android gelecek sürümlerinde önce bu izni istemek için bir uygulama izni gerektirir.



## <a name="summary"></a>Özet

Bu makalede Android 4.1 (API düzeyi 16) içinde kullanılabilen yeni API bazıları kullanıma sunuldu. Bazı değişiklikler animasyonları ve etkinliğin başlatma animasyon için vurgulanmış ve yeni API'nin Bonjour veya UPnP gibi protokolleri kullanarak diğer aygıtlar, ağ bulma için sunulmuştur. API diğer değişiklikler de kesme ve yapıştırma hedefleri, üzerinden veri yalıtılmış Hizmetleri veya "kararsız" içerik sağlayıcıları kullanabilme özelliği gibi vurgulanmış.

Bu makalede daha sonra güncelleştirmeleri bildirimleri tanıtmak için oluştu ve bazı Android 4.1 ile sunulan yeni izinler ele alınan


## <a name="related-links"></a>İlgili bağlantılar

- [Zaman animasyon örneği (örnek)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 API'leri](http://developer.android.com/about/versions/android-4.1.html)
- [Görevleri ve geri yığınları](http://developer.android.com/guide/components/tasks-and-back-stack.html)
- [Yedekleme ve geri gezinme](http://developer.android.com/design/patterns/navigation.html)
