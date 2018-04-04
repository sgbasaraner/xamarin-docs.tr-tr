---
title: Yerel bildirimler
description: Bu bölümde Xamarin.Android yerel bildirimleri uygulamak gösterilmektedir. Bir Android bildirim çeşitli kullanıcı Arabirimi öğelerini açıklar ve API anlatılmaktadır oluşturma ve bir bildirim görüntüleme ile söz konusu.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 97c8372656f0cbfa5b8f7bb12d15b00feac4b5c3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="local-notifications"></a>Yerel bildirimler

_Bu bölümde Xamarin.Android yerel bildirimleri uygulamak gösterilmektedir. Bir Android bildirim çeşitli kullanıcı Arabirimi öğelerini açıklar ve API anlatılmaktadır oluşturma ve bir bildirim görüntüleme ile söz konusu._

## <a name="local-notifications-overview"></a>Yerel bildirimler genel bakış

Bu konuda, bir Xamarin.Android uygulaması'nda yerel bildirimleri uygulamak açıklanmaktadır. Android bildirim çeşitli kısımlarını açıklar, uygulama geliştiricilerinin kullanabileceği farklı bildirim stilleri açıklar ve bazı oluşturmak ve bildirimleri yayımlamak için kullanılan API'ler sunar.

Android, kullanıcı için bildirim simgelerini ve bildirim bilgilerini görüntülemek için iki sistem denetimli alanları sağlar. Bir bildirim ilk yayımlandığında, simgesi görüntülenir *bildirim alanında*aşağıdaki ekran görüntüsünde gösterildiği gibi:

![Bir cihazda örnek bildirim alanı](local-notifications-images/01-notification-shade.png)

Bildirim hakkındaki ayrıntıları almak için kullanıcı (hangi bildirim içerik ortaya çıkarmak üzere her bildirim simgesine genişletir) bildirim çekmecesini açabilir ve bildirimleri ile ilişkili herhangi bir eylem gerçekleştirin. Aşağıdaki ekran görüntüsü gösterildiği bir *bildirim çekmecesini* , yukarıda görüntülenen bildirim alanına karşılık gelir:

[![Örnek bildirim çekmecesini üç bildirimleri görüntüleme](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Android bildirimlerini düzenleri iki tür kullanın:

-   ***Temel düzeni*** &ndash; compact, sabit sunu biçimi.

-   ***Genişletilmiş düzeni*** &ndash; daha fazla bilgi almak için daha büyük bir boyutu genişletebilirsiniz bir sunu biçimi.

Bu düzen türleri (ve bunların nasıl oluşturulacağı) aşağıdaki bölümlerde açıklanmıştır.


### <a name="base-layout"></a>Temel düzeni

Tüm Android bildirimleri, en azından aşağıdaki öğeleri içeren temel düzeni biçimi oluşturulur:

1.  A *bildirim simgesi*, temsil eden kaynak uygulama ya da bildirim türü uygulama farklı türdeki bildirimlerin destekliyorsa.

2.  Bildirim *başlık*, veya gönderenin bildirim kişisel ileti ise adı.

3.  Bildirim iletisi.

4.  A *zaman damgası*.

Bu öğeler, aşağıdaki çizimde gösterildiği gibi görüntülenir:

[![Bildirim öğelerinin konumu](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

Temel düzenleri yükseklik 64 yoğunluğu bağımsız piksel (dp) sınırlıdır. Android, varsayılan olarak bu temel bildirim stili oluşturur.

İsteğe bağlı olarak, bildirimler uygulama veya gönderenin fotoğraf temsil eden büyük bir simge görüntüleyebilirsiniz. Büyük simge Android 5.0 ve daha sonra bir bildirim kullanıldığında, küçük bildirim simgesine büyük simgesinin üzerine bir gösterge görüntülenir:

![Basit bildirim fotoğraf](local-notifications-images/04-simple-notification-photo.png)

Android 5.0 ile başlayarak, bildirimler kilit ekranı üzerinde görünebilir:

[![Örnek kilit ekranı bildirim](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

Kullanıcı çift-cihazın kilidini açmak ve bu bildirim kaynaklanan uygulama atlamak için kilit ekranı bildirim dokunabilirsiniz veya sağdan bildirim yok sayın. Uygulamaları kilit ekranı üzerinde gösterilen denetlemek için bir bildirim görünürlük düzeyini ayarlayabilir ve kullanıcıların kilit ekranı bildirimler gösterilecek hassas içerikleri izin verilip verilmeyeceğini seçebilirsiniz.

Android 5.0 sunulan adlı yüksek öncelikli bildirim sunu biçimine *ekran göstergesi*. Ekran göstergesi bildirimleri aşağı birkaç saniye ekranın üst kısmından kaydırın ve geri bildirim alanında kadar geri Çekilme:

[![Örnek heads-up bildirim](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

Ekran göstergesi bildirimleri sistem şu anda çalışan etkinlik durumunu kesintiye uğratmadan kullanıcı önünde önemli bilgileri koymak için kullanıcı Arabirimi olun.

Böylece bildirimleri sıralanır ve akıllıca görüntülenen android bildirim meta verileri için destek içerir. Bildirim meta veri bildirimleri kilit ekranı ve ekran göstergesi biçiminde nasıl sunulacağının kontrol eder. Uygulamaları aşağıdaki bildirim meta veri türleri ayarlayabilirsiniz:

-   **Öncelik** &ndash; nasıl ve ne zaman bildirimleri sunulan öncelik düzeyini belirler. Örneğin, Android 5.0, yüksek öncelikli bildirimleri ekran göstergesi bildirimleri olarak görüntülenir.

-   **Görünürlük** &ndash; bildirim kilit ekranı göründüğünde görüntülenecek bildirim içerik miktarını belirtir.

-   **Kategori** &ndash; bildirim aygıtı içinde olduğunda gibi çeşitli durumlarda nasıl ele alınacağını sistemine bildirir *rahatsız* modu.

**Not:** **görünürlük** ve **kategori** Android 5.0 ve kullanılabilir olmayan Android önceki sürümlerinde de tanıtılan. Android 8.0 ile başlayan [bildirim kanalları](#notif-chan) bildirimleri kullanıcıya nasıl sunulacağının denetlemek için kullanılır.


### <a name="expanded-layouts"></a>Genişletilmiş düzenleri

Android 4.1 ile başlayarak, bildirimleri daha fazla içerik görüntülemek için bildirim yüksekliğini genişletmek kullanıcı izin genişletilmiş düzeni stilleri ile yapılandırılabilir. Örneğin, aşağıdaki örnekte, bir genişletilmiş düzeni bildirimi sözleşme modunda gösterilmektedir:

![Sözleşme bildirimi](local-notifications-images/07-contracted-notification.png)

Bu bildirim genişletildiğinde tüm ileti ortaya çıkarır:

![Genişletilmiş bildirim](local-notifications-images/08-expanded-notification.png)

Android tek olay bildirimleri için üç genişletilmiş düzeni stillerini destekler:

-   ***Büyük metin*** &ndash; iki noktalarla ve ardından iletinin ilk satırında bir alıntı sözleşme modunda görüntüler. Genişletilmiş modunda tüm ileti (yukarıdaki örnekte görüldüğü gibi) görüntüler.

-   ***Gelen Kutusu*** &ndash; sözleşme modunda yeni ileti sayısını görüntüler. Genişletilmiş modunda ilk e-posta iletisi veya gelen iletilerinin listesini görüntüler.

-   ***Görüntü*** &ndash; sözleşme modda, yalnızca ileti metni görüntüler. Genişletilmiş modunda metin ve resim görüntüler.

[Temel bildirim ötesinde](#beyond-the-basic-notification) (Bu makalenin ilerisinde yer) nasıl oluşturulacağını açıklar *büyük metin*, *gelen*, ve *görüntü* bildirimleri.


## <a name="notification-creation"></a>Bildirim oluşturma

Android bildirim oluşturmak için kullandığınız [Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/) sınıfı. `Notification.Builder` Android bildirim nesneleri oluşturma işlemini basitleştirmek için 3. 0'sunulmuştur. Android eski sürümleriyle uyumlu bildirimleri oluşturmak için kullanabileceğiniz [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) sınıfına `Notification.Builder` (bkz [Uyumluluk](#compatibility) için bu konunun devamındaki kullanma hakkında daha fazla bilgi `NotificationCompat.Builder`).
`Notification.Builder` bir bildirim gibi çeşitli seçenekleri ayarlamak için yöntemler sağlar:

-   Başlık, ileti metni ve bildirim simgesine dahil olmak üzere içerik.

-   Bildirim stilini gibi *büyük metin*, *gelen*, veya *görüntü* stili.

-   Bildirim öncelik: en düşük, düşük, varsayılan, yüksek veya en büyük.

-   Kilit ekranı bildirim görünürlüğünü: Genel, özel veya gizli anahtarı.

-   Android yardımcı kategori meta verileri sınıflandırmak ve bildirim filtre uygulayabilirsiniz.

-   Bildirim dokunduğunuz zaman başlatmak için bir Etkinliği gösterdiğine isteğe bağlı bir hedefi.

Bu seçenekler Oluşturucusu'nda ayarladıktan sonra ayarları içeren bir bildirim nesnesi oluşturur. Bu bildirim nesnesi geçirdiğiniz bildirim yayımlamak için *bildirim Yöneticisi*. Android sağlar [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) bildirimleri yayımlama ve kullanıcıya görüntüleme sorumlu sınıfı. Bu sınıf için bir başvuru, bir etkinlik veya bir hizmet gibi tüm bağlamından alınamıyor.


### <a name="how-to-generate-a-notification"></a>Bir bildirim oluşturmak nasıl

Android bir bildirim oluşturmak için şu adımları izleyin:

1.  Örneği bir `Notification.Builder` nesnesi.

2.  Çeşitli yöntemleri çağırmak `Notification.Builder` bildirim seçeneklerini ayarlamak için nesne.

3.  Çağrı [yapı](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) yöntemi `Notification.Builder` bildirim nesnesi örneği oluşturmak için nesne.

4.  Çağrı [bildirim](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) bildirim yayımlamak için bildirim Yöneticisi'nin yöntemi.

En az her bildirim için aşağıdaki bilgileri sağlamanız gerekir:

-   Küçük bir simge (24 x 24 dp boyutunda)

-   Kısa bir başlık

-   Bildirim metin

Aşağıdaki kod örneğinde nasıl kullanılacağını anlatan `Notification.Builder` temel bir bildirim oluşturmak için. Dikkat `Notification.Builder` yöntemlerini desteklemek [yöntemi zincirleme](http://en.wikipedia.org/wiki/Method_chaining); diğer bir deyişle, sonraki yöntem çağrısı çağırmak için son yöntem çağrısının sonucunu kullanabilmek için her yöntemi Oluşturucu nesnesini döndürür:

```csharp
// Instantiate the builder and set notification elements:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Bu örnekte, yeni bir `Notification.Builder` adlı nesne `builder` olan örneği, başlık ve metin bildirimi ayarlamak ve bildirim simgesine gelen yüklenir **Resources/drawable/ic_notification.png**. Bildirim oluşturucunun çağrısı `Build` yöntemi bu ayarlarla bir bildirim nesnesi oluşturur. Sonraki adım çağırmaktır `Notify` bildirim Yöneticisi yöntemi. Bildirim Yöneticisi'ni bulmak için arama `GetSystemService`, yukarıda gösterildiği gibi.

`Notify` Yöntemi iki parametre kabul eder: bildirim tanımlayıcısı ve bildirim nesnesi. Bildirim tanımlayıcısı uygulamanıza bildirim tanımlayan benzersiz bir tamsayı değil. Bu örnekte, sıfır (0); bildirim tanımlayıcısı ayarlanır Ancak, bir üretim uygulamasında her bildirim benzersiz bir tanımlayıcı vermek istediğiniz. Önceki tanımlayıcı değeri çağrıda yeniden `Notify` son bildirimin üzerine yazılmasına neden olur.

Bu kod bir Android 5.0 cihazında çalıştığında, aşağıdaki gibi görünen bir bildirim oluşturur:

![Örnek kod için bildirim sonucu](local-notifications-images/09-hello-world.png)

Bildirim simgesini bildirim sol tarafında görüntülenir &ndash; bu görüntüsü bir daire içinde &ldquo;ı&rdquo; Android gri döngüsel arka plan arkasındaki çizebilirsiniz böylece alfa kanalı olur. Simge bir alfa kanal olmadan da sağlayabilir. Bir fotoğraf görüntüsü simge olarak görüntülemek için bkz: [büyük simge biçimi](#large-icon-format) bu konuda daha sonra.

Zaman damgası otomatik olarak ayarlanmış, ancak çağırarak bu ayarı geçersiz kılabilirsiniz [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) bildirim Oluşturucu yöntemi. Örneğin, aşağıdaki kod örneğinde zaman damgası geçerli saate ayarlar:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>Ses ve titreşim etkinleştirme

Ayrıca bir ses çalmak için bildirim istiyorsanız, bildirim oluşturucunun çağırabilirsiniz [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) yöntemi ve geçişinde `NotificationDefaults.Sound` bayrağı:

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

Bu çağrıyı `SetDefaults` bildirim yayımlandığında sesi aygıta neden olur. Titret yerine ses Çıkart aygıta istiyorsanız geçirebilirsiniz `NotificationDefaults.Vibrate` için `SetDefaults.` ses Çıkart ve cihazı Titret aygıta istiyorsanız, her iki bayrakları geçirebilirsiniz `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Ses belirtmeden ses devre dışı bırakırsanız, Android varsayılan sistem bildirim ses kullanır. Bununla birlikte, bildirim oluşturucunun çağırarak çalınan ses değiştirebilirsiniz [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) yöntemi. Örneğin, uyarı yürütmek için bildirim (yerine varsayılan bildirim ses), ses, URI alarmı gelen ses alabilirsiniz [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) ve ona geçirin `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Alternatif olarak, sistem varsayılan zil ses bildirim için kullanabilirsiniz:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Bir bildirim nesnesi oluşturduktan sonra bildirim nesnesinde bildirim özelliklerini ayarlamak mümkündür (önceden ile yapılandırmak yerine `Notification.Builder` yöntemleri). Örneğin, çağırmak yerine `SetDefaults` bir bildirim titreşimi etkinleştirmek için yöntemi bildirimin 's bit bayrağı doğrudan değiştirebilirsiniz [varsayılan olarak](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) özelliği:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

Bu örnek, bildirim yayımlandığında Titret aygıta neden olur.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>Bir bildirim güncelleştiriliyor

Yayımlandıktan sonra bir bildirim içeriğini güncelleştirmek istiyorsanız, mevcut yeniden kullanabilir `Notification.Builder` yeni bir bildirim nesnesi oluşturun ve bu bildirim tanımlayıcısı ile son bildirimin yayımlamak için nesne. Örneğin:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

Bu örnekte, varolan `Notification.Builder` nesnesi farklı başlık ve ileti ile yeni bir bildirim nesnesi oluşturmak için kullanılır.
Yeni bildirim nesnesi önceki bildirim tanımlayıcısını kullanarak yayımlanır ve bu içeriği önceden yayımlanan bildirim güncelleştirir:

![Güncelleştirilmiş bildirim](local-notifications-images/12-updated-notification.png)

Önceki bildirim gövdesi yeniden &ndash; yalnızca başlık ve metin bildirimi bildirim çekmecesini görüntülenirken bildirim değişiklikleri. Başlık metni "Örnek bildirimden" "Güncelleştirilmiş bildirimi" ve "Hello World! ileti metnini değiştirir Bu my ilk bildirimidir!" için bu iletiye "değiştirildi."

İşlemlerden birini işlem yapılana kadar bir uyarı görünür:

-   Kullanıcı bildirimi atar (veya dokunur *Tümünü Temizle*).

-   Uygulama için bir çağrı yapar `NotificationManager.Cancel`, bildirim yayımlandığında, atanmış benzersiz bildirim Kimliği'nde geçen.

-   Uygulama çağrıları `NotificationManager.CancelAll`.

Android bildirimlerini güncelleştirme hakkında daha fazla bilgi için bkz: [bir bildirim değiştirme](http://developer.android.com/training/notify-user/managing.html#Updating).


### <a name="starting-an-activity-from-a-notification"></a>Bir etkinlik bildirimden başlatılıyor

Android içinde ilişkilendirilecek bir bildirim için ortak bir *eylem* &ndash; kullanıcı bildirime dokunur yükleyen başlatılan bir etkinlik. Bu etkinlik, başka bir uygulama ya da başka bir görev bulunabilir. Bildirim eylem eklemek için oluşturduğunuz bir [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) nesne ve ilişkilendirmek `PendingIntent` bildirim. A `PendingIntent` alıcı uygulamanın gönderen uygulama izinleriyle önceden tanımlanmış paylaştırılabilen bir kod çalışmasına izin veren hedefinin özel bir tür. Kullanıcı bildirimi dokunur, Android tarafından belirtilen aktivitesi başladığı `PendingIntent`.

Aşağıdaki kod parçacığını içeren bir bildirim oluşturmak üzere verilmektedir bir `PendingIntent` kaynak uygulama etkinliğini başlayacaktır `MainActivity`:

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
Notification.Builder builder = new Notification.Builder(this)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Bu kod çok dışında önceki bölümde bildirim kodu benzer bir `PendingIntent` bildirim nesnesine eklendi. Bu örnekte, `PendingIntent` bildirim oluşturucunun için geçirilmeden önce kaynak uygulama etkinlik ile ilişkili olan [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) yöntemi. `PendingIntentFlags.OneShot` Bayrağı iletilir `PendingIntent.GetActivity` yöntemi böylece `PendingIntent` yalnızca bir kez kullanılır. Bu kod çalıştığında, aşağıdaki bildirim görüntülenir:

![İlk eylem bildirim](local-notifications-images/10-first-action-notification.png)

Bu bildirim dokunarak aktivite kullanıcıya geri alır.

Uygulamanızı bir üretim uygulamasında işlemelidir *geri yığın* kullanıcı bastığında **geri** bildirimi etkinliği içinde düğmesini (Android görevleri ve geri yığını ile bilmiyorsanız, bakın[ Görevleri ve geri yığın](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
Çoğu durumda, bir kullanıcının uygulama dışı ve geri giriş ekranı geri bildirim etkinlik dışı gezinme döndürmelidir. Uygulama tarafından kullanılan geri yığın yönetmek için [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) sınıfı oluşturmak için bir `PendingIntent` geri yığın ile.

Başka bir gerçek aktivite bildirimi etkinliği veri göndermeniz gerekebilir noktadır. Örneğin, bildirim bir kısa mesaj geldi ve bildirim etkinlik (bir ileti ekran görüntüleme), gerektirir ileti kullanıcıya görüntülenecek iletiyi Kimliğini gösterebilir. Oluşturur etkinlik `PendingIntent` kullanabilirsiniz [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) yöntemi verileri (örneğin, bir dize), hedefi ekleyebilirsiniz, böylece bu verileri bildirim etkinliğe geçirilir.

Aşağıdaki kod örneği nasıl kullanılacağını anlatan `TaskStackBuilder` geri yığını ve yönetmek için tek bir ileti dize olarak adlandırılan bir bildirim etkinliğini göndermek nasıl bir örnek içeren `SecondActivity`:

```csharp
// Setup an intent for SecondActivity:
Intent secondIntent = new Intent (this, typeof(SecondActivity));

// Pass some information to SecondActivity:
secondIntent.PutExtra ("message", "Greetings from MainActivity!");

// Create a task stack builder to manage the back stack:
TaskStackBuilder stackBuilder = TaskStackBuilder.Create(this);

// Add all parents of SecondActivity to the stack:
stackBuilder.AddParentStack (Java.Lang.Class.FromType (typeof (SecondActivity)));

// Push the intent that starts SecondActivity onto the stack:
stackBuilder.AddNextIntent (secondIntent);

// Obtain the PendingIntent for launching the task constructed by
// stackbuilder. The pending intent can be used only once (one shot):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    stackBuilder.GetPendingIntent (pendingIntentId, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including
// the pending intent:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my second action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Bu kod örneği, iki etkinliklerini uygulama oluşur: `MainActivity` (yukarıdaki bildirim kodu içeren), ve `SecondActivity`, kullanıcının göreceği bildirim dokunarak sonra ekranı. Bu kodu çalıştırdığınızda (önceki örneğe benzer) basit bir bildirim görüntülenir. Bildirim dokunma alır kullanıcıya `SecondActivity` ekran:

![İkinci etkinlik ekran görüntüsü](local-notifications-images/11-second-activity.png)

Dize ileti (hedefi ait geçirilen `PutExtra` yöntemi) içinde alınan `SecondActivity` Bu kod satırı aracılığıyla:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Alınan bu ileti, "Tebrikler gelen MainActivity!," görüntülenir `SecondActivity` yukarıdaki ekran görüntüsünde gösterildiği gibi ekran. Kullanıcı bastığında **geri** sırasında düğmesini `SecondActivity`, uygulama dışı ve uygulama başlatma önceki ekrana geri gezinti yol açar.

Amaçlar oluşturma hakkında daha fazla bilgi için bkz: [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/).


<a name="notif-chan" />

## <a name="notification-channels"></a>Bildirim kanalları

Kullanabileceğiniz Android 8.0 (Oreo) ile başlayarak, *bildirim kanalları* görüntülemek istediğiniz bildirim her türü için kullanıcı özelleştirilebilir bir kanal oluşturmak için özellik. Böylece tüm bildirimler için kanal Sergi aynı davranışı gönderilen bildirim kanalları, Grup bildirimleri göndermek için sizin için mümkün kılar. Örneğin, hemen ilgilenilmesi gereken bildirimleri için amaçlanan bir bildirim kanalı ve bilgilendirici iletileri için kullanılan ayrı bir "sessiz" kanalı olabilir.

**YouTube** ile Android Oreo yüklü app iki bildirim kategorileri listeler: **karşıdan bildirimleri** ve **genel bildirimleri**:

[![Android Oreo YouTube için bildirim ekranları](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

Bu kategorilerin her biri için bir bildirim kanalı karşılık gelir. YouTube uygulama uygulayan bir **karşıdan bildirimleri** kanal ve **genel bildirimleri** kanal. Kullanıcı dokunabilirsiniz **karşıdan bildirimleri**, uygulamanın bildirim kanalı indirmek için ayarları ekranı görüntüler:

[![Bildirimleri ekran YouTube uygulamasının için karşıdan yükle](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

Bu ekranda, kullanıcı davranışını değiştirebilir **karşıdan** bildirimleri kanal aşağıdakileri yaparak:

-   Önem düzeyini ayarlamak **Acil**, **yüksek**, **orta**, veya **düşük**, ses ve görsel kesinti düzeyini yapılandırır.

-   Bildirim nokta Aç veya kapat.

-   Yanıp sönen ışık Aç veya kapat.

-   Göster veya gizle kilit ekranında bildirim.

-   Geçersiz kılma **Rahatsız Etmeyin** ayarı.

**Genel bildirimleri** kanal benzer ayarlara sahip:

[![YouTube uygulamasının için genel bildirimleri ekranı](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

Mutlak denetiminiz nasıl, bildirim kanalları kullanıcıyla etkileşim olmadığını bildirimi &ndash; yukarıdaki ekran görüntülerinde görüldüğü gibi kullanıcı cihazda hiçbir bildirim kanalı ayarlarını değiştirebilir. Ancak, (aşağıda açıklanacaktır gibi), varsayılan değerleri yapılandırabilirsiniz. Bu örneklerin gösterdiği gibi yeni bildirim kanalları özelliği kullanıcılar bildirimleri farklı türde üzerinde hassas denetime olanak kolaylaştırır.

Uygulamanıza bildirim kanalları için destek eklemeniz gerekir? Android 8.0, uygulamanızı hedefleme varsa *gerekir* bildirim kanalları uygulayın.
Bir bildirim kanalı kullanmadan kullanıcıya bir yerel Bildirim göndermeye çalıştığında Oreo için hedeflenen bir uygulama bildirimi Oreo cihazlarda görüntülenecek başarısız olur. Android 8.0 hedefliyorsanız yok Android 7.1 veya önceki sürümlerde çalışan sergilemesine gibi uygulamanızın hala Android 8.0, ancak aynı bildirim davranışı ile çalışır.


### <a name="creating-a-notification-channel"></a>Bir bildirim kanalı oluşturma

Bir bildirim kanalı oluşturmak için aşağıdakileri yapın:

1. Oluşturmak bir [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html) aşağıdaki nesnesiyle:

    - Paket içinde benzersiz bir kimlik dizesi. Aşağıdaki örnekte, dize `com.xamarin.myapp.urgent` kullanılır.

    - Kanal (değerinden 40 karakter) kullanıcıya görünen adı. Aşağıdaki örnekte, adı **Acil** kullanılır.

    - Nasıl interruptive bildirimleri denetimleri kanal önemini nakledilir `NotificationChannel`. Önem olabilir `Default`, `High`, `Low`, `Max`, `Min`, `None`, veya `Unspecified`.

    Bu değerleri geçirmek [Oluşturucusu](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (Bu örnekte, `Resource.String.noti_chan_urgent` ayarlanır **Acil**):

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  Yapılandırma `NotificationChannel` ilk ayarlarla nesnesi.
    Örneğin, aşağıdaki kodu yapılandırır `NotificationChannel` ; böylece bu kanala gönderilen bildirimler cihazı Titret ve kilit ekranı üzerinde varsayılan olarak görünür nesne:

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  Bildirim Yöneticisi bildirim kanalı nesnesine gönder:

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```


### <a name="posting-to-a-notifications-channel"></a>Bir bildirim kanalı gönderme

Bir bildirim kanalı için bir bildirim göndermek için aşağıdakileri yapın:

1.  Bildirim kullanarak yapılandırma `Notification.Builder`, kanal kimliğindeki geçen `SetChannelId` yöntemi. Örneğin:

    ```csharp
    Notification.Builder builder = new Notification.Builder (this)
        .SetContentTitle ("Attention!")
        .SetContentText ("This is an urgent notification message!")
        .SetChannelId (URGENT_CHANNEL);
    ```

2.  Derleme ve bildirim yöneticisinin kullanarak bildirim vermek [bildirim](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/p/System.Int32/Android.App.Notification/) yöntemi:

    ```csharp
    const int notificationId = 0;
    notificationManager.Notify (notificationId, builder.Build());
    ```

Bilgilendirici iletileri için başka bir bildirim kanalı oluşturmak için yukarıdaki adımları yineleyebilirsiniz. Bu ikinci kanal varsayılan olarak, Titreşim devre dışı bırakmak, varsayılan kilit ekranı görünürlük kümesine `Private`ve bildirim önem düzeyini belirlemek `Default`.

Bir tam kod örneği Android Oreo bildirim kanalları için eylem bkz [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) örnek; bu örnek uygulama iki kanalı yönetir ve ek bildirim seçeneklerini ayarlar.



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Temel bildirim

Bildirimleri varsayılan Android no-frills temel düzeni biçiminde, ancak ek yaparak bu temel biçimi geliştirebilirsiniz `Notification.Builder` yöntem çağrıları. Bu bölümde, bir büyük fotoğraf simgesi, bildirim eklemek öğreneceksiniz ve genişletilmiş düzeni bildirimleri oluşturma örnekleri görürsünüz.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>Büyük simge biçimi

Android bildirimleri gönderen uygulama simgesi (sol tarafta bildirimin) genellikle görüntüler. Bir görüntü veya fotoğraf bildirimleri ancak görüntüleyebilirsiniz (bir *büyük simge*) yerine standart küçük simgesi. Örneğin, bir Mesajlaşma uygulaması bir fotoğraf uygulaması simgesi yerine gönderen görüntüleyebilirsiniz.

Temel bir Android 5.0 bildirim bir örneği burada verilmiştir &ndash; yalnızca küçük uygulama simgesi görüntüler:

![Örnek normal bildirim](local-notifications-images/13-sample-notification.png)

Burada da bir ekran görüntüsünü büyük simge görüntülenecek değiştirmeye sonra bildirim &ndash; Xamarin kod monkey görüntüden oluşturulan bir simge kullanır:

![Örnek büyük simge bildirim](local-notifications-images/14-large-icon-sample.png)

Bir bildirim büyük simge biçiminde sunulduğunda, küçük uygulama simgesini büyük simge sağ alt köşesindeki üzerinde bir gösterge olarak görüntülenen dikkat edin.

Büyük simge bir bildirim olarak görüntüyü kullanmak için bildirim oluşturucunun çağrısı [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) yöntemi ve görüntüyü bir bit eşlem geçirin. Farklı `SetSmallIcon`, `SetLargeIcon` yalnızca bir bit eşlem kabul eder. Bir bitmap içine bir görüntü dosyasına dönüştürmek için kullandığınız [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) sınıfı. Örneğin:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Bu örnek kod, görüntü dosyası açar **Resources/drawable/monkey_icon.png**bir bit eşlem dönüştürür ve sonuçta elde edilen bitmap geçirir `Notification.Builder`. Genellikle, kaynak görüntü çözünürlüğü küçük simge büyük &ndash; ancak çok büyük. Çok büyük bir görüntü bildirim nakil geciktirebilir gereksiz yeniden boyutlandırma işlemlerini neden olabilir.
Android bildirim simgesine boyutları hakkında daha fazla bilgi için bkz: [bildirim simgelerini](http://developer.android.com/design/style/iconography.html#notification).


### <a name="big-text-style"></a>Büyük metin stili

*Büyük metin* stili bildirimleri uzun iletileri görüntülemek için kullandığınız bir genişletilmiş Düzen şablonu olduğundan. Tüm genişletilmiş düzeni bildirimleri gibi büyük metin bildirimi başlangıçta compact Sunusu biçiminde görüntülenir:

![Örnek büyük metin bildirimi](local-notifications-images/15-big-text-notification.png)

Bu biçimde iki dönemlere göre sonlandırıldı yalnızca bir alıntı iletisi gösterilir. Kullanıcı bildirimi sürüklendiğinde, tüm bildirim iletisi ortaya çıkarmak üzere genişletir:

![Genişletilmiş büyük metin bildirimi](local-notifications-images/16-big-text-expanded.png)

Bu Genişletilmiş düzeni biçimi de bildirim alt Özet metni içerir. En fazla yüksekliği *büyük metin* bildirimidir 256 dp.

Oluşturmak için bir *büyük metin* bildirim, örneği bir `Notification.Builder` nesne, önceden olduğu gibi örneği ve ekleme bir [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) nesnesini `Notification.Builder` nesne. Örneğin:

```csharp
// Instantiate the Big Text style:
Notification.BigTextStyle textStyle = new Notification.BigTextStyle();

// Fill it with text:
string longTextMessage = "I went up one pair of stairs.";
longTextMessage += " / Just like me. ";
//...
textStyle.BigText (longTextMessage);

// Set the summary text:
textStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (textStyle);

// Create the notification and publish it ...
```

Bu örnekte, ileti metni ve Özet metni depolanmış `BigTextStyle` nesne (`textStyle`) için geçirilen önce `Notification.Builder.`


### <a name="image-style"></a>Görüntü stili

*Görüntü* stili (olarak da bilinir *büyük resmi* stili) bir bildirim gövdesinde görüntüyü görüntülemek için kullanabileceğiniz bir genişletilmiş bildirim biçimi. Örneğin, bir ekran uygulaması veya bir fotoğraf uygulaması kullanabilirsiniz *görüntü* kullanıcı son içeren bir bildirim sağlamak için stili görüntü bildirim yakalanan. Unutmayın en fazla yüksekliği *görüntü* bildirimidir 256 dp &ndash; Android yeniden boyutlandırmak, kullanılabilir belleğin sınırları içinde bu maksimum yüksekliği kısıtlama içine sığması için resmin.

Düzen bildirimleri gibi tüm Genişletilmiş *görüntü* bildirimler eşlik eden ileti metni bir alıntı görüntüler compact bir biçimde önce görüntülenir:

![Hiçbir görüntü Compact görüntü bildirimi gösterir](local-notifications-images/17-image-compact.png)

Ne zaman kullanıcının sürüklediği *görüntü* bildirim, onu genişletir görüntü ortaya çıkarmak için. Örneğin, önceki bildirim genişletilmiş bir sürümü şöyledir:

![Genişletilmiş görüntü bildirim ortaya çıkarır görüntüsü](local-notifications-images/18-image-expanded.png)

Bildirim compact biçiminde görüntülendiğinde, bildirim metni görüntüleyen bildirimi (bildirim oluşturucunun için geçirilen metin `SetContentText` yöntemi, daha önce gösterildiği gibi). Ancak, bildirim görüntü ortaya çıkarmak için genişletildiğinde, görüntünün üstünde Özet metni görüntüler.

Oluşturmak için bir *görüntü* bildirim, örneği bir `Notification.Builder` önceki gibi nesnesi oluşturma ve ekleme bir [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) içine nesne `Notification.Builder` nesne. Örneğin:

```csharp
// Instantiate the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Convert the image to a bitmap before passing it into the style:
picStyle.BigPicture (BitmapFactory.DecodeResource (Resources, Resource.Drawable.x_bldg));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create the notification and publish it ...
```

Gibi `SetLargeIcon` yöntemi `Notification.Builder`, [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) yöntemi `BigPictureStyle` bildirim gövdesinde görüntülenmesini istediğiniz görüntünün bir bit eşlem gerektirir. Bu örnekte, [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) yöntemi `BitmapFactory` görüntü dosyası bulunan okuma **Resources/drawable/x_bldg.png** ve bit eşlem dönüştürür.

Bir kaynak olarak değil paketlenir görüntüleri de görüntüleyebilirsiniz. Örneğin, aşağıdaki örnek kod yerel SD kartı'ndan bir görüntü yükler ve görüntüler bir *görüntü* bildirim:

```csharp
// Using the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Read an image from the SD card, subsample to half size:
BitmapFactory.Options options = new BitmapFactory.Options();
options.InSampleSize = 2;
string imagePath = "/sdcard/Pictures/my-tshirt.jpg";
picStyle.BigPicture (BitmapFactory.DecodeFile (imagePath, options));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("Check out my new T-Shirt!");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create notification and publish it ...
```

Bu örnekte, görüntü dosyası konumunda **/sdcard/Pictures/my-tshirt.jpg** yüklenen, özgün boyutuna yarısı yeniden boyutlandırılabilir ve bildirim kullanmak için bir bit eşlem dönüştürülür:

![Bildirim örnek ısı resmi](local-notifications-images/19-tshirt-notification.png)

Görüntü dosyasının boyutunu önceden bilmiyorsanız, onu çağrısı sarmalamak için iyi bir fikirdir [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) bir özel durum işleyici içinde &ndash; bir `OutOfMemoryError` özel durum görüntüsü için çok büyük ise Yeniden boyutlandırmak için android.

Yükleme ve büyük bit eşlem görüntüleri kod çözme hakkında daha fazla bilgi için bkz: [yük büyük bit eşlemler verimli bir şekilde](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently).


### <a name="inbox-style"></a>Gelen kutusu stili

*Gelen* bildirim gövdesinde ayrı satırlık metin (örneğin, bir e-posta gelen kutusu Özet) görüntülemek için amaçlanan bir genişletilmiş Düzen şablonunu biçimidir. *Gelen* biçimi bildirim compact biçiminde önce görüntülenir:

![Örnek compact gelen bildirim](local-notifications-images/20-inbox-compact.png)

Kullanıcı bildirimi sürüklendiğinde, aşağıdaki ekran görüntüsünde görülen bir e-posta özeti ortaya çıkarmak üzere genişletir:

![Genişletilmiş örnek gelen bildirim](local-notifications-images/21-inbox-expanded.png)

Oluşturmak için bir *gelen* bildirim, örneği bir `Notification.Builder` nesnesi, önceki gibi ve ekleme bir [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) nesnesine `Notification.Builder`. Örneğin:

```csharp
// Instantiate the Inbox style:
Notification.InboxStyle inboxStyle = new Notification.InboxStyle();

// Set the title and text of the notification:
builder.SetContentTitle ("5 new messages");
builder.SetContentText ("chimchim@xamarin.com");

// Generate a message summary for the body of the notification:
inboxStyle.AddLine ("Cheeta: Bananas on sale");
inboxStyle.AddLine ("George: Curious about your blog post");
inboxStyle.AddLine ("Nikko: Need a ride to Evolve?");
inboxStyle.SetSummaryText ("+2 more");

// Plug this style into the builder:
builder.SetStyle (inboxStyle);
```

Bildirim Gövde metnin yeni satır eklemek için arama [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) yöntemi `InboxStyle` nesne (en fazla yüksekliği *gelen* bildirimidir 256 dp). Aksine unutmayın *büyük metin* stili *gelen* stilini bildirim gövdesinde tek tek satırlık metin destekler.

Aynı zamanda *gelen* tek tek satırlık metin bir genişletilmiş biçimde görüntülemek için gereken her türlü bildirim için stili. Örneğin, *gelen* bildirim stili, bir Özet bildirim içine birden fazla bekleyen bildirim birleştirmek için kullanılabilir &ndash; tek bir güncelleştirme *gelen* stil yeni ile bildirimi bildirim içeriğin satırlarına (bkz [bir bildirim güncelleştirme](#updating-a-notification) yukarıda), bunun yerine daha yeni, çoğunlukla benzer bildirimler sürekli akışı oluştur. Bu yaklaşımı hakkında daha fazla bilgi için bkz: [bildirimlerinizi özetlemek](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications).


## <a name="configuring-metadata"></a>Meta verileri yapılandırma

`Notification.Builder` Öncelik, görünürlük ve kategori gibi bildirim hakkındaki meta verileri ayarlamak için çağırabilirsiniz yöntemleri içerir. Android kullanan bu bilgileri &mdash; kullanıcı tercihi ayarlarıyla birlikte &mdash; nasıl ve ne zaman belirlemek için bildirimler görüntülemek için.


### <a name="priority-settings"></a>Öncelik ayarları

Bildirim yayımlandığında bir bildirim öncelik ayarı iki sonucunu belirler:

-   Burada bildirim diğer bildirimleri ile ilgili olarak görüntülenir.
    Her bir bildirim yayımlandığında Örneğin, yüksek öncelikli bildirimleri bildirim çekmecesini daha düşük öncelikli işler için bildirimleri yukarıda bakılmaksızın sunulur.

-   Bildirim Heads-up bildirim biçimi (Android 5.0 ve sonrasındaki) görüntülenip görüntülenmeyeceğini. Yalnızca *yüksek* ve *maksimum* öncelik bildirimler ekran göstergesi bildirimleri olarak görüntülenir.

Xamarin.Android bildirim önceliğini ayarlamak için aşağıdaki numaralandırmaları tanımlar:

-   `NotificationPriority.Max` &ndash; (Örneğin, gelen bir arama, dönüş tarafından Aç yönergeleri veya Acil Durum uyarısı) bir Acil veya Kritik durum kullanıcıyı uyarır. Android 5.0 ve üzeri cihazlarda, en yüksek öncelik bildirimleri ekran göstergesi biçiminde görüntülenir.

-   `NotificationPriority.High` &ndash; Önemli olayları (örneğin, önemli e-postalar veya gerçek zamanlı sohbet iletilerini varış) kullanıcıya bildirir. Android 5.0 ve üzeri cihazlarda, yüksek öncelikli bildirimleri ekran göstergesi biçiminde görüntülenir.

-   `NotificationPriority.Default` &ndash; Orta önem düzeyine sahip koşulların kullanıcıyı uyarır.

-   `NotificationPriority.Low` &ndash; Kullanıcı (örneğin, yazılım güncelleştirme anımsatıcıları veya sosyal ağ güncelleştirmelerini) hakkında bilgi sahibi olması gerektiğini Acil olmayan bilgi.

-   `NotificationPriority.Min` &ndash; Kullanıcı yalnızca bildirimler arka plan bilgileri için bildirimleri (örneğin, konum veya hava durumu bilgileri) görüntüleme.

Bir bildirim önceliğini ayarlamak için arama [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) yöntemi `Notification.Builder` nesnesinin içindeki öncelik düzeyi. Örneğin:

```csharp
builder.SetPriority (NotificationPriority.High);
```

Aşağıdaki örnek, yüksek öncelikli bildirim, "Önemli bir ileti!" bildirim çekmecesini üstünde görünür:

![Örnek yüksek öncelikli bildirimi](local-notifications-images/22-hi-priority-drawer.png)

Bu yüksek öncelikli bildirim olduğundan, kullanıcının geçerli etkinlik ekranı Android 5.0 üzerinde bir ekran göstergesi bildirim olarak da görüntülenir:

![Örnek ekran göstergesi bildirim](local-notifications-images/23-heads-up-example.png)

Sonraki örnekte, "gün için zorlayıcı" düşük öncelikli bildirim altında daha yüksek öncelikli pil düzeyi bildirimi görüntülenir:

![Örnek düşük öncelikli bildirimi](local-notifications-images/24-lo-priority-drawer.png)

Düşük öncelikli bildirim "Düşünce günü için" bildirim olduğu için Android, Heads-up biçiminde görüntülenmez.


### <a name="visibility-settings"></a>Görünürlüğü ayarları

Android 5.0 ile başlayan *görünürlük* ne kadar bildirim içerik üzerinde güvenli kilit ekranı görünür denetlemek ayarı mevcuttur.
Xamarin.Android bildirim görünürlük ayarlamak için aşağıdaki numaralandırmaları tanımlar:

-   `NotificationVisibility.Public` &ndash; Bildirim tam içeriğini üzerinde güvenli kilit ekranı görüntülenir.

-   `NotificationVisibility.Private` &ndash; Yalnızca önemli bilgiler (örneğin, bildirim simgesine ve deftere uygulama adı) güvenli kilit ekranı görüntülenir, ancak bildirim ait ayrıntıları kalan gizlidir. Varsayılan olarak tüm bildirimleri `NotificationVisibility.Private`.

-   `NotificationVisibility.Secret` &ndash; Hiçbir şey güvenli kilit ekranı üzerinde bile bildirim simgesi görüntülenir. Yalnızca kullanıcının aygıtın kilidini açarak sonra bildirim içerik mevcut değildir.

Uygulamalar çağrısı bir bildirim görünürlüğünü ayarlamak için `SetVisibility` yöntemi `Notification.Builder` görünürlük ayarında geçirme nesnesi. Örneğin, bu çağrıyı `SetVisibility` bildirim yapar `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Zaman bir `Private` bildirim nakledilir, yalnızca adı ve uygulamanın simgesi üzerinde güvenli kilit ekranı görüntülenir. Bildirim iletisi yerine kullanıcı "Bu bildirim görmek için Unlock Cihazınızı" görür:

![Aygıt bildirim iletisi kilidini aç](local-notifications-images/25-lockscreen-private.png)

Bu örnekte, **NotificationsLab** kaynak uygulama adıdır. Bildirim Redaksiyonu yapılmış bu sürümü yalnızca kilit ekranı güvenli olduğunda görüntülenir (yani, PIN, desen ya da parola güvenli) &ndash; kilit ekranı güvenli değilse, bildirim tam içeriğini kilit ekranı üzerinde kullanılabilir.


### <a name="category-settings"></a>Kategori ayarları

Android 5.0 ile başlayarak, önceden tanımlanmış kategoriler sıralama ve filtreleme bildirimleri için kullanılabilir. Xamarin.Android aşağıdaki sabit listeleri için bu kategorilerden sağlar:

-   `Notification.CategoryCall` &ndash; Gelen telefon araması.

-   `Notification.CategoryMessage` &ndash; Gelen metin iletisi.

-   `Notification.CategoryAlarm` &ndash; Bir uyarı koşulu veya Zamanlayıcı süre sonu.

-   `Notification.CategoryEmail` &ndash; Gelen e-posta iletisi.

-   `Notification.CategoryEvent` &ndash; Bir takvim olayı.

-   `Notification.CategoryPromo` &ndash; Promosyon iletisi veya tanıtım.

-   `Notification.CategoryProgress` &ndash; Arka plan işlemi devam ediyor.

-   `Notification.CategorySocial` &ndash; Sosyal ağ güncelleştirmesi.

-   `Notification.CategoryError` &ndash; Arka plan işlemi veya kimlik doğrulama işlemi hata oluştu.

-   `Notification.CategoryTransport` &ndash; Medya kayıttan yürütme güncelleştirme.

-   `Notification.CategorySystem` &ndash; (Sistem veya aygıt durum) sistemin kullanılmak üzere ayrılmış.

-   `Notification.CategoryService` &ndash; Bir arka plan hizmetinin çalışmadığını gösterir.

-   `Notification.CategoryRecommendation` &ndash; Şu anda çalışan uygulama ile ilişkili bir öneri iletisi.

-   `Notification.CategoryStatus` &ndash; Aygıt hakkında bilgi.

Bildirimleri sıralandığında bildirim ait öncelik, kategori ayarına göre önceliklidir. Ait olsa bile Örneğin, yüksek öncelikli bildirim ekran göstergesi görüntülenir `Promo` kategorisi. Bir bildirim kategorisini ayarlamak için arama `SetCategory` yöntemi `Notification.Builder` nesnesi, kategori ayarında geçirme. Örneğin:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

*Rahatsız* özelliği (Android 5.0 ile yeni) bildirimleri kategorisine göre filtreler. Örneğin, *rahatsız* ekran **ayarları** muaf bildirimleri kullanıcıya telefon aramaları ve iletileri olanak sağlar:

![Ekran anahtarları rahatsız etmeyin](local-notifications-images/26-do-not-disturb.png)

Kullanıcı ne zaman yapılandırır *rahatsız* (yukarıdaki ekran görüntüsünde gösterildiği gibi) aramaları hariç tüm kesintileri engellemek için bildirimler kategori ayarı olan Android sağlar `Notification.CategoryCall` cihazı sunulacak içinde *rahatsız* modu. Unutmayın `Notification.CategoryAlarm` bildirimleri hiçbir zaman engellenir *rahatsız* modu.


<a name="compatibility" />

## <a name="compatibility"></a>Uyumluluk

Bir uygulama, oluşturuyorsanız, Android (gibi erken API düzey 4), önceki sürümlerinde de çalıştırmak kullanacağınız [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) sınıfına `Notification.Builder`. Bildirimleri ile oluşturduğunuzda `NotificationCompat.Builder`, Android temel bildirim içerik eski cihazlarda düzgün görüntülenmesini sağlar. Ancak, bazı gelişmiş bildirim özellikleri Android eski sürümlerinde kullanılabilir olmadığından, kodunuzu açıkça genişletilmiş bildirim stiller, kategoriler ve aşağıda açıklandığı gibi görünürlük düzeyleri için uyumluluk sorunları işlemelidir.

Kullanılacak `NotificationCompat.Builder` , uygulamanızda eklemelisiniz [Android destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) projenizdeki NuGet.

Aşağıdaki kod örneği kullanarak bir temel bildirim oluşturmak nasıl gösterir `NotificationCompat.Builder`:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();
```

Bu örnekte gösterildiği gibi önemli bildirim seçenekleri için yöntem çağrılarını gereksinimlerine aynı `Notification.Builder`. Ancak, daha karmaşık bildirimler (sonraki bölümde açıklanmıştır) için aşağı uyumluluk sorunlarını gidermek kodunuzu vardır.

[LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) örnek nasıl kullanılacağını gösteren `NotificationCompat.Builder` bir bildirim ikinci etkinliğinden başlatmak için. Bu örnek kod açıklaması [Xamarin.Android yerel bildirimlerini kullanarak](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) gözden geçirme.


### <a name="notification-styles"></a>Bildirim stilleri

Oluşturmak için *büyük metin*, *görüntü*, veya *gelen* stil bildirimlerle `NotificationCompat.Builder`, uygulamanızı bu stiller uyumluluk sürümlerini kullanmanız gerekir. Örneğin, kullanılacak *büyük metin* stil, örneği `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Benzer şekilde, uygulamanızı kullanabilirsiniz `NotificationCompat.InboxStyle` ve `NotificationCompat.BigPictureStyle` için *gelen* ve *görüntü* stiller, sırasıyla.


### <a name="notification-priority-and-category"></a>Bildirim önceliğini ve kategorisini

`NotificationCompat.Builder` destekleyen `SetPriority` yöntemi (kullanılabilir Android 4.1 başlayarak). Ancak, `SetCategory` yöntemi *değil* tarafından desteklenen `NotificationCompat.Builder` kategorileri Android 5.0 ile sunulan yeni bildirim meta verileri sisteminin bir parçası olduğundan.

Android, where eski sürümleri desteklemek için `SetCategory` olan kullanılamıyor, kodunuzu koşullu çağırmak için çalışma zamanında API düzeyini kontrol edebilirsiniz `SetCategory` API düzeyi zaman eşit veya bundan büyük Android 5.0 (API düzeyi 21):

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

Bu örnekte, uygulama kullanıcının **hedef Framework** Android 5.0 ayarlanır ve **Minimum Android sürümü** ayarlanır **Android 4.1 (API düzeyi 16)**. Olduğundan `SetCategory` olan kullanılabilir API düzeyinde 21 ve daha sonra bu kod örneği çağırır `SetCategory` yalnızca kullanılabilir olduğu zaman &ndash; değil çağıracak `SetCategory` API düzeyi olduğunda küçüktür
21.


### <a name="lockscreen-visibility"></a>Kilit ekranı görünürlüğü

Android kilit ekranı bildirimler Android 5.0 (API düzeyi 21), önce desteklemediği `NotificationCompat.Builder` desteklemediği `SetVisibility` yöntemi. İçin yukarıda açıklandığı şekilde `SetCategory`, kodunuzun çalışma zamanı ve çağrı API düzeyinde denetleyebilirsiniz `SetVisiblity` yalnızca kullanılabilir olduğu zaman:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>Özet

Bu makalede nasıl Android yerel bildirimler oluşturulacağı açıklanmıştır. Bir bildirim anatomisi açıklanan, nasıl kullanılacağı açıklanmıştır `Notification.Builder` bildirimleri oluşturmak için büyük simge stili bildirimlerini nasıl *büyük metin*, *görüntü* ve *gelen kutusu*  biçimleri, öncelik, görünürlük ve kategori gibi meta veri ayarları nasıl bildirim kurulur ve bildirim etkinliği başlatmak nasıl. Bu makalede ayrıca yeni ekran göstergesi ile kilit ekranı, bu bildirim ayarlarının nasıl çalıştığını açıklanan ve *rahatsız* Android 5.0 ile sunulan özellikler. Son olarak, de nasıl kullanılacağı hakkında bilgi edindiniz `NotificationCompat.Builder` bildirim Android önceki sürümleriyle uyumluluğunu korumak için.

Android için tasarlama bildirimleri hakkında yönergeler için bkz: [bildirimleri](http://developer.android.com/preview/notifications.html).


## <a name="related-links"></a>İlgili bağlantılar

- [NotificationsLab (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (örnek)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android kılavuzda yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Bildirimler](http://developer.android.com/design/patterns/notifications.html)
- [Kullanıcıya bildirme](http://developer.android.com/training/notify-user/index.html)
- [bildirim](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
