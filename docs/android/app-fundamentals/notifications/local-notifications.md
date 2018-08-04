---
title: Yerel bildirimler
description: Bu bölümde Xamarin.Android yerel bildirimlerinin nasıl gösterir. Bir Android bildirimi çeşitli kullanıcı Arabirimi öğelerini açıklar ve API anlatılmaktadır oluşturma ve bir bildirim görüntüleme ile söz konusu.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 221fa9b70eeba2c4ca08433c627e5648470a7fac
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514537"
---
# <a name="local-notifications"></a>Yerel bildirimler

_Bu bölümde Xamarin.Android yerel bildirimlerinin nasıl gösterir. Bir Android bildirimi çeşitli kullanıcı Arabirimi öğelerini açıklar ve API anlatılmaktadır oluşturma ve bir bildirim görüntüleme ile söz konusu._

## <a name="local-notifications-overview"></a>Yerel bildirimler genel bakış

Bu konu, bir Xamarin.Android uygulamasına yerel bildirimlerinin nasıl açıklar. Bir Android bildirimi çeşitli bölümlerini açıklar, uygulama geliştiricilerinin kullanabileceği farklı bildirim stilleri açıklar ve bazı oluşturmak ve bildirimleri yayımlamak için kullanılan API'leri sunar.

Android, kullanıcı için bildirim simgelerini ve bildirim bilgileri görüntülemek için iki sistem tarafından denetlenen alanı sağlar. Bir bildirim ilk kez yayımlandığında simgesi görüntülenen *bildirim alanında*aşağıdaki ekran görüntüsünde gösterildiği gibi:

![Bir cihazda örnek bildirim alanı](local-notifications-images/01-notification-shade.png)

Bildirim ayrıntılarını almak için kullanıcı (Bu, bildirim içeriği ortaya çıkarmak için her bildirim simgesi genişletir) bildirim bölmesini açabilirsiniz ve bildirimler ile ilgili eylemleri gerçekleştirin. Aşağıdaki ekran görüntüsünde gösterildiği bir *bildirim çekmecesini* , yukarıda gösterilen bildirim alanına karşılık gelir:

[![Örnek bildirim çekmecesini üç bildirimleri görüntüleme](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Android bildirimlerini düzenleri iki tür kullanın:

-   ***Taban düzeni*** &ndash; compact, sabit sunu biçimi.

-   ***Genişletilmiş Düzen*** &ndash; daha fazla bilgi almak için büyük bir boyut için genişletebileceğiniz bir sunu biçimi.

Bu düzen türlerinin her biri (ve bunların nasıl oluşturulacağı) aşağıdaki bölümlerde açıklanmıştır.


### <a name="base-layout"></a>Taban düzeni

Tüm Android bildirimleri en azından aşağıdaki öğeleri içeren temel düzeni biçimi oluşturulmuştur:

1.  A *bildirim simgesi*, temsil eden kaynak uygulama ya da bildirim türünü uygulama farklı türde bildirim destekliyorsa.

2.  Bildirim *başlık*, ya da bildirim gönderen kişisel bir ileti adı.

3.  Bildirim iletisi.

4.  A *zaman damgası*.

Bu öğeler, aşağıdaki diyagramda gösterildiği gibi görüntülenir:

[![Bildirim öğelerinin konumu](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

Temel düzenleri yüksekliği 64 yoğunluklu bağımsız piksel (dp) sınırlıdır. Android, varsayılan olarak bu temel bildirim stili oluşturur.

İsteğe bağlı olarak, bildirimleri, uygulama veya gönderenin fotoğraf temsil eden büyük bir simge görüntüleyebilirsiniz. Büyük simge bir bildirim Android 5.0 ve üzeri kullanıldığında, küçük bildirim simgesine rozet büyük simgesinin üzerine görüntülenir:

![Basit bir bildirim fotoğrafı](local-notifications-images/04-simple-notification-photo.png)

Android 5.0 ile başlayarak, bildirimleri kilit ekranında görünebilir:

[![Örnek kilit ekranı bildirim](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

Kullanıcının çift-cihazın kilidini açmak ve bu bildirim kaynaklanan uygulamaya atlamak için kilit ekranı bildirim dokunabilirsiniz veya bildirimi kapatmak için. Kullanıcılar, kilit ekranı bildirimleri gösterilecek hassas içerik izin verilip verilmeyeceğini seçebilirsiniz ve uygulamaları kilit ekranında gösterilen denetlemek için bir bildirim görünürlük düzeyini ayarlayabilirsiniz.

Android 5.0 sunulan adlı yüksek öncelikli bildirim sunu biçimi *uyarı*. Uyarı bildirimleri, ekranın üst kısmından aşağı birkaç saniye boyunca kaydırın ve ardından geri bildirim alanına kadar geri Çekilme:

[![Örnek heads-up bildirimi](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

Uyarı bildirimleri kullanıcı önünde önemli bilgileri şu anda çalışan etkinlik durumunu kesintiye uğratmadan koymak için kullanıcı Arabirimi sistemi olun.

Android bildirimleri sıralanmış ve akıllı bir şekilde görüntülenen bildirim meta veriler için destek içerir. Bildirimleri kilit ekranı ve uyarı biçimde nasıl sunulduğu bildirim meta verileri de denetler. Uygulama bildirimi meta verileri aşağıdaki türde ayarlayabilirsiniz:

-   **Öncelik** &ndash; nasıl ve ne zaman bildirimler sunulur öncelik düzeyini belirler. Örneğin, Android 5.0, yüksek öncelikli bildirim uyarı bildirimleri olarak görüntülenir.

-   **Görünürlük** &ndash; karşılaşıldığında bildirim kilit ekranında görüntülenecek bildirim içerik miktarını belirtir.

-   **Kategori** &ndash; sistem cihazı içinde olduğunda gibi çeşitli durumlarda bildirimini işlemek nasıl bildirir *Rahatsız Etmeyin* modu.

**Not:** **görünürlük** ve **kategori** sunulan Android 5.0 ve önceki Android sürümlerinde kullanılamaz. Android 8.0 ile başlayan [bildirim kanallarını](#notif-chan) bildirimleri kullanıcıya nasıl sunulduğu denetlemek için kullanılır.


### <a name="expanded-layouts"></a>Genişletilmiş düzenleri

Android 4.1 ile başlayarak, bildirimleri daha fazla içerik görüntülemek için bildirim yüksekliğini genişletmek kullanıcının olanak tanıyan genişletilmiş Düzen stilleri ile yapılandırılabilir. Örneğin, aşağıdaki örnekte, sözleşme modunda bir genişletilmiş Düzen bildirimi gösterilmektedir:

![Sözleşme bildirim](local-notifications-images/07-contracted-notification.png)

Bu bildirim genişletildiğinde, iletinin tamamı gösterir:

![Genişletilmiş bildirim](local-notifications-images/08-expanded-notification.png)

Android için tek olay bildirimleri üç genişletilmiş Düzen stillerini destekler:

-   ***Büyük metin*** &ndash; izleyen iki nokta iletinin ilk satırında bir alıntı sözleşme modunda görüntülenir. Genişletilmiş modda tüm iletinin (yukarıdaki örnekte görüldüğü gibi) görüntüler.

-   ***Gelen Kutusu*** &ndash; sözleşme modunda yeni ileti sayısını görüntüler. Genişletilmiş modda ilk e-posta iletisi veya gelen iletilerinin listesini görüntüler.

-   ***Görüntü*** &ndash; sözleşme modunda, yalnızca ileti metni görüntüler. Genişletilmiş modda, metin ve resim görüntüler.

[Temel bildirim ötesinde](#beyond-the-basic-notification) (Bu makalenin ilerleyen bölümlerinde) nasıl oluşturulacağını açıklar *büyük metin*, *gelen*, ve *görüntü* bildirimleri.


## <a name="notification-creation"></a>Bildirim oluşturma

Android'de bir bildirim oluşturmak için kullandığınız [Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/) sınıfı. `Notification.Builder` Android bildirimi nesneleri oluşturmayı basitleştirmek için 3. 0'kullanıma sunulmuştur. Android eski sürümleriyle uyumlu bildirimleri oluşturmak için kullanabileceğiniz [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) sınıfı yerine `Notification.Builder` (bkz [Uyumluluk](#compatibility) için bu konunun devamındaki kullanma hakkında daha fazla bilgi `NotificationCompat.Builder`).
`Notification.Builder` bir bildirimde, gibi çeşitli seçenekleri ayarlamak için yöntemler sağlar:

-   Başlık, ileti metni ve bildirim simgesine dahil olmak üzere içerik.

-   Bildirim stili gibi *büyük metin*, *gelen*, veya *görüntü* stili.

-   Bildirim önceliğini: en düşük, düşük, varsayılan olarak yüksek veya en fazla.

-   Kilit ekranı bildirim görünürlüğünü: Genel, özel veya gizli anahtarı.

-   Yardımcı Android sınıflandırma ve bildirime filtre kategorisi meta verileri.

-   Bildirime dokunulduğunda başlatmak için bir Etkinliği gösterdiğine isteğe bağlı bir hedefi.

Bu seçenekler Oluşturucusu'nda ayarladıktan sonra ayarlarını içeren bir bildirim nesnesi oluşturur. Bu bildirim nesnesine geçirdiğiniz bildirim yayımlamayı *bildirim Yöneticisi*. Android tarafından sağlanan [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) bildirimleri yayımlama ve bunları kullanıcıya görüntülemek için sorumlu sınıfı. Bu sınıf bir başvuru bir etkinlik veya hizmeti gibi tüm bağlamından alınamıyor.


### <a name="how-to-generate-a-notification"></a>Bildirim oluşturma

Android'de bir bildirim oluşturmak için şu adımları izleyin:

1.  Örneği bir `Notification.Builder` nesne.

2.  Çeşitli yöntemleri çağırmak `Notification.Builder` nesne bildirim seçeneklerini ayarlayın.

3.  Çağrı [derleme](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) yöntemi `Notification.Builder` bildirim nesnesi örneklemek için nesne.

4.  Çağrı [bildirim](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) bildirimi yayımlamak için bildirim Yöneticisi yöntemi.

En az her bildirim için aşağıdaki bilgileri sağlamanız gerekir:

-   Küçük simge (24 x 24 boyutu dp)

-   Kısa bir başlık

-   Bildirimin metni

Aşağıdaki kod örneğinde nasıl kullanılacağı gösterilmektedir `Notification.Builder` basit bir bildirim oluşturmak için. Dikkat `Notification.Builder` yöntemleri destekler [yöntem zincirlemesinde](http://en.wikipedia.org/wiki/Method_chaining); diğer bir deyişle, sonraki yöntem çağrısının çağırmak için son yöntem çağrısının sonucunu kullanabilmeniz için her yöntemi Oluşturucu nesnesini döndürür:

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

Bu örnekte, yeni bir `Notification.Builder` çağrılan nesne `builder` olan örneği, başlık ve metin bildirimi ayarlanır ve bildirim simgesine gelen yüklenen **Resources/drawable/ic_notification.png**. Bildirim oluşturucunun çağrısı `Build` yöntemi, bu ayarlarla bir bildirim nesnesi oluşturur. Sonraki adım çağırmaktır `Notify` bildirim Yöneticisi yöntemi. Bildirim Yöneticisi'ni bulmak için arama `GetSystemService`, yukarıda gösterildiği gibi.

`Notify` Yöntemi, iki parametre kabul eder: bildirim tanımlayıcısı ve bildirim nesnesi. Bildirim tanımlayıcısı uygulamanıza bildirim tanımlayan benzersiz bir tamsayıdır. Bu örnekte, sıfır (0); bildirim tanımlayıcısı ayarlanır Ancak, bir üretim uygulamasında benzersiz bir tanımlayıcı her bildirim vermek istersiniz. Önceki bir çağrı tanımlayıcı değeri yeniden `Notify` son bildirim üzerine yazılmasına neden olur.

Bu kod bir Android 5.0 cihaz üzerinde çalıştığında, aşağıdaki örnekte gösterildiği gibi görünür bir bildirim oluşturur:

![Örnek kod için bildirim sonucu](local-notifications-images/09-hello-world.png)

Bildirim simgesine bildirim sol tarafında görüntülenen &ndash; bu görüntüsü bir daire içinde &ldquo;miyim&rdquo; alfa kanalına sahiptir; böylece Android gri döngüsel arka plan arkasındaki çizebilirsiniz. Simge bir alfa kanalı olmadan da sağlayabilirsiniz. Fotoğraf bir görüntü, simge olarak görüntülemek için bkz [büyük simge biçimi](#large-icon-format) bu konuda.

Zaman damgası otomatik olarak ayarlanmış, ancak çağırarak bu ayarı geçersiz kılabilirsiniz [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) bildirim oluşturucunun yöntemi. Örneğin, aşağıdaki kod örneğinde zaman damgası geçerli saate ayarlar:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>Ses ve titreşim etkinleştirme

Ayrıca ses çalınması için bildirim istiyorsanız, bildirim oluşturucunun çağırabilirsiniz [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) yöntemi ve geçişinde `NotificationDefaults.Sound` bayrağı:

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

Bu çağrıyı `SetDefaults` bildirim yayımlandığında sesi cihazın neden olur. Cihazın ses çal vibrate yerine isterseniz geçirebilirsiniz `NotificationDefaults.Vibrate` için `SetDefaults.` Çıkart ve cihaz vibrate aygıta istiyorsanız, her iki bayrakları geçirebilirsiniz `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Ses belirtmeden sesi etkinleştirin, varsayılan sistem bildirim sesi Android kullanır. Ancak, bildirim oluşturucunun çağırarak yürütülen ses değiştirebilirsiniz [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) yöntemi. Örneğin, uyarı yürütmek için (yerine varsayılan bildirim ses), uygulamanızın bildirimi ile ses URI için uyarı gelen ses alabileceğiniz [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) ve geçirin `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Alternatif olarak, sistem varsayılan zil ses için BİLDİRİMİNİZE kullanabilirsiniz:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Bir bildirim nesnesini oluşturduktan sonra bildirim nesnede bildirim özelliklerini ayarlamak mümkündür (bunları önceden ile yapılandırmak yerine `Notification.Builder` yöntemleri). Örneğin, çağırmak yerine `SetDefaults` etkinleştirme Titreşim bir bildirim yöntemi kullanılan bit bayrağı bildirimin, doğrudan değiştirebilirsiniz [varsayılanları](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) özelliği:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

Bu örnek bildirim yayımlandığında vibrate edilmesine neden olur.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>Bildirim güncelleştiriliyor

Yayımlandıktan sonra bildirim içeriğini güncelleştirmek istiyorsanız, var olan kullanabilirsiniz `Notification.Builder` yeni bir bildirim oluşturur ve bu bildirim tanımlayıcısı ile son bildirimin yayımlamak için nesne. Örneğin:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

Bu örnekte, mevcut `Notification.Builder` nesnesi, farklı bir başlık ve ileti ile yeni bir bildirim nesnesi oluşturmak için kullanılır.
Önceki bildirim tanımlayıcısını kullanarak yeni bir bildirim nesne yayımlanır ve bu daha önce yayımlanmış bildirim içeriğini güncelleştirir:

![Güncelleştirilmiş uyarı](local-notifications-images/12-updated-notification.png)

Önceki bildirim gövdesi yeniden &ndash; yalnızca başlık ve metin bildirimi bildirim çekmecede görüntülenirken bildirim değişiklikleri. Başlık metni "Örnek bildirimden" "Güncelleştirilen bildirim" ve "Hello World! ileti metnini değiştirir Bu benim ilk bildirimidir!" için "Bu iletiye değiştirildi."

Üç şeylerden biri oluşuncaya kadar bildirim görünür kalır:

-   Kullanıcı bildirimi atar (veya dokunur *Tümünü Temizle*).

-   Uygulama çağrıda `NotificationManager.Cancel`, geçen bildirim yayımlandığında, atanan benzersiz bildirim kimliği.

-   Uygulama çağrıları `NotificationManager.CancelAll`.

Android bildirimleri güncelleştirme hakkında daha fazla bilgi için bkz. [bildirim değiştirme](http://developer.android.com/training/notify-user/managing.html#Updating).


### <a name="starting-an-activity-from-a-notification"></a>Bir etkinlik bir bildirimden başlatılıyor

Android'de, bir bildirim ile ilişkilendirilmesi için ortak bir *eylem* &ndash; kullanıcı bildirime dokunduğunda başlatılan bir etkinlik. Bu etkinlik, başka bir uygulama veya hatta başka bir görev bulunabilir. Bildirim eylem eklemek için oluşturduğunuz bir [Pendingıntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) nesne ve ilişkilendirmek `PendingIntent` bildirimi. A `PendingIntent` hedefinin gönderen uygulama izinleri ile önceden tanımlanmış bir kod parçası çalıştırmak alıcı uygulamanın sağlayan özel bir tür. Kullanıcı bildirime dokunduğunda, Android tarafından belirtilen aktivitesi başlar `PendingIntent`.

Aşağıdaki kod parçacığı bir bildirim ile nasıl oluşturulacağı bir `PendingIntent` kaynak uygulamanın etkinlik başlayacaktır `MainActivity`:

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

Bu kod dışında önceki bölümde bildirim koda benzer bir `PendingIntent` bildirim nesnesine eklenmelidir. Bu örnekte, `PendingIntent` bildirim oluşturucunun için geçirilmeden önce kaynak uygulamanın etkinliği ile ilişkili olan [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) yöntemi. `PendingIntentFlags.OneShot` Bayrağı geçirildiğinde `PendingIntent.GetActivity` yöntemi böylece `PendingIntent` yalnızca bir kez kullanılır. Bu kod çalıştığında, aşağıdaki bildirim görüntülenir:

![İlk eylemi bildirimi](local-notifications-images/10-first-action-notification.png)

Bu bildirime dokunulduğunda aktivite kullanıcıya geri alır.

Uygulamanızı bir üretim uygulamasında işlemesi *geri yığın* kullanıcının bastığında **geri** bildirimi etkinliği içinde düğmesine ( Androidgörevlervegeriyığınıileilgilibilgisahibideğilseniz,bkz[ Görevler ve geri yığın](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
Çoğu durumda, geri bildirim etkinlik dışı gezinme kullanıcının uygulama dışında ve giriş ekranındaki dön döndürmelidir. Uygulama tarafından kullanılan geri yığını yönetmek için [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) sınıfı oluşturmak için bir `PendingIntent` geri yığın ile.

Başka bir gerçek aktivite veri göndermek için bildirim etkinlik gerekebilir noktadır. Örneğin, bildirimi, SMS mesajı kullanıma sunuldu ve bildirim etkinlik (bir ileti ekrana görüntüleme), ileti kullanıcıya görüntülenecek ileti kimliği gerektirir gösteriyor olabilir. Oluşturan etkinliğin `PendingIntent` kullanabilirsiniz [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) yöntemi verileri (örneğin, bir dize), ıntent'e ekleyebilirsiniz, böylece bu veriler bildirim etkinliğine geçirilir.

Aşağıdaki kod örneği nasıl kullanılacağı gösterilmektedir `TaskStackBuilder` geri yığını ve onu yönetmek için bir tek ileti dizesi olarak adlandırılan bir bildirim etkinliğini göndermek nasıl bir örnek içeren `SecondActivity`:

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

Bu kod örneğinde, uygulamanın iki etkinliklerden oluşur: `MainActivity` (yukarıdaki bildirim kodu içeren), ve `SecondActivity`, kullanıcının gördüğü bildirime dokunulduğunda sonra ekran. Bu kodu çalıştırdığınızda (önceki örneğe benzer) basit bir bildirim sunulur. Bildirim dokunma gereken kullanıcıya `SecondActivity` ekran:

![İkinci etkinlik ekran görüntüsü](local-notifications-images/11-second-activity.png)

Dize iletisi (amaç ait geçirilen `PutExtra` yöntemi) içinde alınan `SecondActivity` aracılığıyla bu kod satırı:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Bu bir alınan ileti, "Greetings gelen MainActivity!," görüntülenir `SecondActivity` ekranını yukarıdaki ekran görüntüsünde gösterildiği gibi. Kullanıcının bastığında **geri** düğmesini basılı `SecondActivity`, gezinti dışında uygulama ve uygulamanın başlatma önceki ekrana geri yönlendirir.

Intents oluşturma hakkında daha fazla bilgi için bkz. [Pendingıntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/).


<a name="notif-chan"></a>
<a name="notification-channels"></a>
## <a name="notification-channels"></a>Bildirim kanalları

Kullanabileceğiniz Android 8.0 (Oreo) ile başlayarak, *bildirim kanallarını* görüntülemek istediğiniz bildirim her türü için kullanıcı tarafından özelleştirilebilir bir kanal oluşturmak için özellik. Böylece tüm bildirimler aynı davranışı için bir kanal ek gönderilen bildirim kanallarını grubu bildirimleri sizin için sağlar. Örneğin, bir bildirim kanalı hemen ilgilenilmesi gereken bildirimleri için tasarlanmıştır ve bilgilendirici iletileri için kullanılan ayrı bir "sakin" kanal sahip olabilir.

**YouTube** ile Android Oreo yüklü uygulama iki bildirim kategorileri listeler: **indirme bildirimleri** ve **genel bildirimleri**:

[![Android Oreo, YouTube için bildirim ekranları](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

Bu kategorilerden her biri için bir bildirim kanalı karşılık gelir. YouTube uygulama uygulayan bir **indirme bildirimleri** kanal ve **genel bildirimleri** kanal. Kullanıcı dokunabilir **indirme bildirimleri**, uygulamanın bildirim kanalı indirmek için ayarları ekranı görüntüler:

[![YouTube uygulamanın bildirimler ekranında indir](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

Bu ekranda, kullanıcı davranışını değiştirebilirsiniz **indirme** bildirimleri kanal aşağıdakileri yaparak:

-   Önem düzeyini ayarlamak **Acil**, **yüksek**, **orta**, veya **düşük**, ses ve görsel kesinti düzeyini yapılandırır.

-   Bildirim nokta Aç veya kapat.

-   Yanıp sönen ışık Aç veya kapat.

-   Gösterin veya gizleyin kilit ekranında bildirimleri.

-   Geçersiz kılma **Rahatsız Etmeyin durumunu** ayarı.

**Genel bildirimleri** kanal benzer ayarlara sahiptir:

[![YouTube uygulamasının için genel bildirimleri ekranı](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

Uyarı, bildirim kanalları ile kullanıcı etkileşimini üzerinde mutlak denetim sahibi olmadığınız &ndash; yukarıdaki ekran görüntülerinde görüldüğü gibi kullanıcı cihazda herhangi bir bildirim kanalı ayarlarını değiştirebilirsiniz. Ancak, (aşağıda açıklanacaktır gibi), varsayılan değerleri yapılandırabilirsiniz. Bu örnekte gösterildiği gibi yeni bildirim kanalları özelliği kullanıcılara bildirim farklı türlerini üzerinde ayrıntılı denetim vermek mümkün kolaylaştırır.

Uygulamanıza bildirim kanallarını için destek eklemeniz gerekir? Android 8.0, uygulamanızı hedefliyorsanız *gerekir* bildirim kanallarını uygulayın.
Oreo cihazlarında bildirimi görüntülemek bir bildirim kanalı kullanmadan kullanıcıya bir yerel Bildirim göndermeye çalıştığında Oreo için hedeflenen bir uygulama başarısız olur. Android 8.0 hedefliyorsanız yoktur, Android 7.1 veya önceki çalıştırırken göstermesi gibi uygulamanızın Android 8.0, ancak aynı bildirim davranışı ile çalışmaya devam edecektir.


### <a name="creating-a-notification-channel"></a>Bir bildirim kanalı oluşturma

Bir bildirim kanalı oluşturmak için aşağıdakileri yapın:

1. Oluşturmak bir [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html) aşağıdaki nesnesi:

    - Bir paket içinde benzersiz olan bir kimliği dizesi. Aşağıdaki örnekte, dize `com.xamarin.myapp.urgent` kullanılır.

    - Kanal (değerinden 40 karakter) kullanıcıya görünen adı. Aşağıdaki örnekte, adı **Acil** kullanılır.

    - Nasıl interruptive bildirimlerini denetler kanal önemini nakledilir `NotificationChannel`. Önem derecesi olabilir `Default`, `High`, `Low`, `Max`, `Min`, `None`, veya `Unspecified`.

    Bu değerleri geçirmek [Oluşturucusu](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (Bu örnekte, `Resource.String.noti_chan_urgent` ayarlanır **Acil**):

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  Yapılandırma `NotificationChannel` ilk ayarları içeren nesne.
    Örneğin, aşağıdaki kod yapılandırır `NotificationChannel` bu kanalına gönderilen bildirimleri cihaz vibrate ve kilit ekranında varsayılan olarak görünür nesne:

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  Bildirim Yöneticisi bildirim kanalı nesnesine gönderme:

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```


### <a name="posting-to-a-notifications-channel"></a>Bir bildirim kanalı için gönderme

Bir bildirim kanalı için bir bildirim göndermek için aşağıdakileri yapın:

1.  Bildirim kullanarak yapılandırma `Notification.Builder`, kanal kimliği için geçen `SetChannelId` yöntemi. Örneğin:

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

Bilgilendirme iletileri için başka bir bildirim kanalı oluşturmak için yukarıdaki adımları tekrarlayabilirsiniz. Bu ikinci kanal varsayılan olarak, Titreşim devre dışı, varsayılan kilit ekranı görünürlük kümesine `Private`, bildirim önem ayarlanmış ve `Default`.

Bir tam kod örneği Android Oreo bildirim kanallarını için eylemde görmek [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) örnek; bu örnek uygulama iki kanallar yönetir ve ek bildirim seçeneklerini ayarlar.



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Temel bildirim

Bildirimleri varsayılan olarak Android no-frills temel düzeni biçimine ancak ek yaparak bu temel biçimi iyileştirebilecek `Notification.Builder` yöntemi çağırır. Bu bölümde, bir büyük fotoğrafı simge için bildirim eklemek öğreneceksiniz ve genişletilmiş Düzen bildirimleri oluşturma örnekleri göreceksiniz.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>Büyük simge biçimi

Android bildirimleri, genellikle (sol tarafında bildirim) kaynak uygulamanın simgesi görüntülenir. Bir resim veya fotoğraf bildirimleri ancak görüntüleyebilirsiniz (bir *büyük simge*) yerine standart küçük simge. Örneğin, bir Mesajlaşma uygulaması, uygulama simgesine yerine gönderen bir fotoğraf görüntüleyebilirsiniz.

İşte bir örnek basit bir Android 5.0 bildirim &ndash; yalnızca küçük bir uygulama simgesi görüntüler:

![Örnek normal bildirimi](local-notifications-images/13-sample-notification.png)

Burada da büyük bir simge görüntülemek için değiştirmeden sonra bildirim görüntüsü &ndash; bir Xamarin kod monkey bir görüntüden oluşturulan bir simge kullanır:

![Örnek büyük simge bildirimi](local-notifications-images/14-large-icon-sample.png)

Bir bildirim büyük simge biçiminde gösterildiğinde küçük uygulama simgesine rozet büyük simge sağ alt köşesindeki şirket olarak görüntülenen dikkat edin.

Bir bildirim büyük bir simge görüntü kullanmak için bildirim oluşturucunun çağrı [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) yöntemi ve bir bit eşlem resmi geçirin. Farklı `SetSmallIcon`, `SetLargeIcon` yalnızca bir bit eşlem kabul eder. Bir görüntü dosyası bir bit eşlem dönüştürmek için kullandığınız [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) sınıfı. Örneğin:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Bu kod örneği, görüntü dosyası açılır **Resources/drawable/monkey_icon.png**bit eşleme dönüştürür ve sonuçta elde edilen bit eşleme geçirir `Notification.Builder`. Genellikle, kaynak görüntü çözünürlüğünü küçük simge büyük &ndash; ancak daha büyük. Çok büyük bir görüntü bildirim geciktirmeye gereksiz yeniden boyutlandırma işlemlerini neden olabilir.
Android bildirim simgesi boyutları hakkında daha fazla bilgi için bkz. [bildirim simgeleri](http://developer.android.com/design/style/iconography.html#notification).


### <a name="big-text-style"></a>Büyük metin stili

*Büyük metin* stil Bildirimlerde uzun iletileri görüntülemek için kullandığınız bir genişletilmiş Düzen şablonunu gelir. Tüm genişletilmiş Düzen bildirimler gibi büyük metin bildirim başlangıçta compact sunu biçiminde görüntülenir:

![Örnek büyük metin bildirimi](local-notifications-images/15-big-text-notification.png)

Bu biçimde iki noktayla sonlandırıldı iletisi yalnızca bir alıntı gösterilir. Kullanıcı bildirime sürüklediğinde, tüm bildirim iletisini göstermek üzere genişletir:

![Genişletilmiş büyük metin bildirimi](local-notifications-images/16-big-text-expanded.png)

Bu Genişletilmiş düzeni biçimi de bildirim alt kısmındaki Özet metni içerir. Maksimum yüksekliğini *büyük metin* bildirimidir 256 dp.

Oluşturmak için bir *büyük metin* bildirim, örneği bir `Notification.Builder` önce olduğu gibi nesne örneği oluşturun ve Ekle bir [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) nesnesini `Notification.Builder` nesnesi. Örneğin:

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

Bu örnekte, ileti metni ve Özet metni depolanır `BigTextStyle` nesne (`textStyle`) için geçirilmeden önce `Notification.Builder.`


### <a name="image-style"></a>Görüntü stili

*Görüntü* stili (olarak da bilinir *büyük resmi* stili) bir bildirim gövdesinde bir resmi görüntülemek için kullanabileceğiniz genişletilmiş bildirim biçimi değil. Örneğin, bir ekran görüntüsü veya bir fotoğraf uygulaması kullanabilirsiniz *görüntü* son bir bildirim ile kullanıcıya sağlamak için stil görüntü bildirim yakalanan. Unutmayın maksimum yüksekliğini *görüntü* bildirimidir 256 dp &ndash; Android yeniden boyutlandırma bu yükseklik en fazla kısıtlama, kullanılabilir bellek sınırları dahilinde içine resmi.

Düzen bildirimleri gibi tüm Genişletilmiş *görüntü* bildirimler önce bir alıntı eşlik eden ileti metni görüntüler küçük bir biçimde görüntülenir:

![Görüntü sıkıştırılmış görüntü bildirimi gösterir](local-notifications-images/17-image-compact.png)

Kullanıcı sürüklediğinde *görüntü* bildirimi genişlediğinden genişler bir resim ortaya çıkarmak için. Örneğin, önceki bildirim genişletilmiş hali aşağıdadır:

![Genişletilmiş görüntü bildirim ortaya çıkarır resmi](local-notifications-images/18-image-expanded.png)

Bildirim compact biçiminde gösterildiğinde, bildirim metni görüntülediğinden emin Uyarısı (bildirim oluşturucunun için geçirilen metin `SetContentText` yöntemi, daha önce gösterildiği gibi). Ancak, bildirim görüntü açığa çıkarmak için genişletildiğinde, resmin üzerine Özet metni görüntüler.

Oluşturmak için bir *görüntü* bildirim, örneği bir `Notification.Builder` önceden olduğu gibi nesne oluşturma ve ekleme bir [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) içine nesne `Notification.Builder` nesne. Örneğin:

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

Gibi `SetLargeIcon` yöntemi `Notification.Builder`, [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) yöntemi `BigPictureStyle` bildirimi gövdesinde görüntülemek istediğiniz görüntünün bir bit eşlem gerektirir. Bu örnekte, [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) yöntemi `BitmapFactory` görüntü dosyasının konumu okuma **Resources/drawable/x_bldg.png** ve bir bit eşleme dönüştürür.

Bir kaynak olarak değil paketlenmiş görüntüleri de görüntüleyebilirsiniz. Örneğin, aşağıdaki örnek kod yerel SD karttan görüntüyü yükler ve görüntüler bir *görüntü* bildirim:

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

Bu örnekte, görüntü dosyası konumundaki **/sdcard/Pictures/my-tshirt.jpg** yüklenen, özgün boyutuna yarısını yeniden boyutlandırıldı ve bildirim kullanmak için bir bit eşlem dönüştürülür:

![Örnek T-shirt görüntüde bildirimi](local-notifications-images/19-tshirt-notification.png)

Resim dosyası boyutunu önceden bilmiyorsanız, bu çağrısını sarmalamak için iyi bir fikirdir [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) bir özel durum işleyicisinde &ndash; bir `OutOfMemoryError` özel durum görüntüsü için çok büyük ise Yeniden boyutlandırmak için android.

Yükleme ve büyük bir bit eşlem görüntülerinin kodunu çözme hakkında daha fazla bilgi için bkz. [yük büyük bit eşlemler verimli](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently).


### <a name="inbox-style"></a>Gelen kutusu stili

*Gelen* bildirimi gövdesinde ayrı satırlık metin (örneğin, bir e-posta gelen Özet) görüntülemek için hedeflenen bir genişletilmiş Düzen şablonunu biçimidir. *Gelen* biçimi bildirim ilk compact biçiminde görüntülenir:

![Örnek compact gelen bildirim](local-notifications-images/20-inbox-compact.png)

Kullanıcı bildirime sürüklediğinde, aşağıdaki ekran görüntüsünde görüldüğü gibi bir e-posta özeti açığa çıkarmak için genişletir:

![Genişletilmiş örnek gelen bildirim](local-notifications-images/21-inbox-expanded.png)

Oluşturmak için bir *gelen* bildirim, örneği bir `Notification.Builder` ekleyin ve nesne, önceden olduğu gibi bir [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) nesnesini `Notification.Builder`. Örneğin:

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

Bildirimi gövdesi için yeni satırlık metin eklemek, arama [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) yöntemi `InboxStyle` nesne (maksimum yüksekliğini *gelen* bildirimidir 256 dp). Aksine, Not *büyük metin* stili *gelen* stilini tek satırlık metin bildirimi gövdesinde destekler.

Ayrıca *gelen* metin satırlarını tek tek bir genişletilmiş biçimde görüntülemek için gereken her türlü bildirim için stili. Örneğin, *gelen* bildirim stili, birden fazla bekleyen bildirimler Özet bildirim halinde birleştirmek için kullanılabilir &ndash; tek bir güncelleştirebileceğiniz *gelen* yeni bildirim stili bildirim içeriği satırlarını (bkz [bildirim güncelleştiriliyor](#updating-a-notification) yukarıda), bunun yerine daha yeni ve çoğunlukla benzer bildirimler sürekli bir akış oluşturun. Bu yaklaşımı hakkında daha fazla bilgi için bkz: [bildirimlerinizi özetlemek](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications).


## <a name="configuring-metadata"></a>Yapılandırma meta verileri

`Notification.Builder` Öncelik, görünürlük ve kategorisi gibi bildirim hakkındaki meta verileri ayarlama için çağırabileceğiniz yöntemler içerir. Android, bu bilgileri kullanır &mdash; kullanıcı tercihi ayarlarıyla birlikte &mdash; nasıl ve ne zaman belirlemek için bildirimleri görüntüleyin.


### <a name="priority-settings"></a>Öncelik ayarları

Bildirim yayımlandığında öncelik ayarı bir bildirim iki sonuçlarını belirler:

-   Burada diğer bildirimler ile ilgili bildirim görüntülenir.
    Her bildirim yayımlandığında Örneğin, yüksek öncelikli bildirimler bildirimleri çekmecesinde daha düşük öncelikli bildirimleri yukarıda açmamasından sunulur.

-   Olup bildirim Heads-up bildirim biçimi (Android 5.0 ve üzeri) görüntülenir. Yalnızca *yüksek* ve *maksimum* öncelik bildirimler, uyarı bildirimleri olarak görüntülenir.

Xamarin.Android bildirim öncelikli ayarlamak için aşağıdaki sabit listeleri tanımlanmıştır:

-   `NotificationPriority.Max` &ndash; (Örneğin, gelen bir arama, bırakma tarafından bırakma yönergeleri veya Acil Durum uyarısı) bir Acil veya kritik koşulu kullanıcıyı uyarır. Android 5.0 ve üzeri cihazlarda, en yüksek öncelik bildirimler uyarı biçiminde görüntülenir.

-   `NotificationPriority.High` &ndash; Önemli olaylar (örneğin, önemli e-postalar veya gerçek zamanlı bir sohbet iletileri sıem'e) kullanıcıya bildirir. Android 5.0 ve üzeri cihazlarda, yüksek öncelikli bildirimler uyarı biçiminde görüntülenir.

-   `NotificationPriority.Default` &ndash; Önem derecesi orta düzeyde gereken koşulları kullanıcıya bildirir.

-   `NotificationPriority.Low` &ndash; Kullanıcı (örneğin, yazılım güncelleştirme anımsatıcıları veya sosyal ağ güncelleştirmeleri) bilgilendirilmek gerektiğini Acil olmayan bilgi.

-   `NotificationPriority.Min` &ndash; Arka plan bilgileri için kullanıcı yalnızca bildirimleri (örneğin, konum veya hava durumu bilgileri) bildirimleri görüntüleme.

Bir bildirim önceliğini ayarlamak için çağrı [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) yöntemi `Notification.Builder` nesnesinin içindeki öncelik düzeyi. Örneğin:

```csharp
builder.SetPriority (NotificationPriority.High);
```

Aşağıdaki örnek, yüksek öncelikli bildirim, "Önemli bir ileti!" bildirim çekmecesini üst kısmında görünür:

![Örneğin yüksek öncelikli bildirim](local-notifications-images/22-hi-priority-drawer.png)

Bu yüksek öncelikli bildirim olduğundan, kullanıcının geçerli etkinlik ekranı Android 5.0 üzerinde bir uyarı bildirimi olarak da görüntülenir:

![Örnek uyarı bildirimi](local-notifications-images/23-heads-up-example.png)

Sonraki örnekte, "güne ait düşündüğünüz" düşük öncelikli bildirim daha yüksek öncelikli pil düzeyi bildirim altında görüntülenir:

![Örnek düşük öncelikli bildirim](local-notifications-images/24-lo-priority-drawer.png)

Düşük öncelikli bildirim "Düşünce gün için" bildirim olduğu için Android bu Heads-up biçiminde görüntülenmez.


### <a name="visibility-settings"></a>Görünürlüğü ayarları

Android 5.0 ile başlayan *görünürlük* güvenli kilit ekranında görünen ne kadar bildirim içeriğini denetlemek ayarı kullanılabilir.
Xamarin.Android bildirim görünürlük ayarlamak için aşağıdaki sabit listeleri tanımlanmıştır:

-   `NotificationVisibility.Public` &ndash; Bildirim tam içeriği güvenli kilit ekranında görüntülenir.

-   `NotificationVisibility.Private` &ndash; Yalnızca temel bilgiler (örneğin, bildirim simgesine ve onu gönderilen bir uygulama adı) güvenli kilit ekranı görüntülenir, ancak ayrıntıları bildirimin geri kalanı gizlidir. Varsayılan olarak tüm bildirimler `NotificationVisibility.Private`.

-   `NotificationVisibility.Secret` &ndash; Hiçbir şey güvenli kilit ekranında, bile bildirim simgesi görüntülenir. Bildirim içeriği, yalnızca kullanıcının cihaz kilidini açarak sonra kullanılabilir.

Bir bildirim uygulamaları çağrı görünürlüğünü ayarlanacak `SetVisibility` yöntemi `Notification.Builder` nesnesinin görünürlüğünü ayarı. Örneğin, bu çağrı `SetVisibility` bildirim yapar `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Olduğunda bir `Private` bildirim gönderildiğinde, adı ve simgesi uygulamanın yalnızca güvenli kilit ekranında görüntülenir. Bildirim iletisi yerine, kullanıcı, "Bu bildirim görmek için kilit açma Cihazınızı" görür:

![Cihaz bildirim iletinizin kilidini aç](local-notifications-images/25-lockscreen-private.png)

Bu örnekte, **NotificationsLab** kaynak uygulama adıdır. Bildirim sonuç kısaltılmıştır bu sürümü yalnızca kilit ekranı güvenli olduğunda görünür (yani, PIN, desen veya parola güvenli) &ndash; kilit ekranı güvenli değilse, kilit ekranında bildirim tam içeriğini kullanılabilir.


### <a name="category-settings"></a>Kategori ayarlarını

Android 5.0 ile başlayarak, önceden tanımlanmış kategoriler sıralama ve filtreleme bildirimleri için kullanılabilir. Aşağıdaki numaralandırmalar Bu kategoriler için Xamarin.Android sağlar:

-   `Notification.CategoryCall` &ndash; Gelen telefon araması.

-   `Notification.CategoryMessage` &ndash; Gelen metin iletisi.

-   `Notification.CategoryAlarm` &ndash; Bir uyarı koşulu veya Zamanlayıcı zaman aşımı.

-   `Notification.CategoryEmail` &ndash; Gelen e-posta iletisi.

-   `Notification.CategoryEvent` &ndash; Bir takvim etkinliği.

-   `Notification.CategoryPromo` &ndash; Bir promosyon ileti veya Tanıtımı.

-   `Notification.CategoryProgress` &ndash; Arka plan işlemi ilerlemesini.

-   `Notification.CategorySocial` &ndash; Sosyal ağ güncelleştirme.

-   `Notification.CategoryError` &ndash; Bir arka plan işlemi veya kimlik doğrulama işlemi başarısız.

-   `Notification.CategoryTransport` &ndash; Medya kayıttan yürütme güncelleştirme.

-   `Notification.CategorySystem` &ndash; (Sistem ya da cihaz durumu) sistem kullanımı için ayrılmıştır.

-   `Notification.CategoryService` &ndash; Arka plan hizmetinin çalışıp çalışmadığını gösterir.

-   `Notification.CategoryRecommendation` &ndash; Çalışmakta olan uygulamayla ilgili bir öneri iletisi.

-   `Notification.CategoryStatus` &ndash; Cihaz hakkındaki bilgileri.

Bildirimleri sıralandığında, bildirimin öncelik, kategori ayarına göre önceliklidir. Ait olsa bile, yüksek öncelikli bildirim uyarı görüntülenecek `Promo` kategorisi. Bildirim kategorisi ayarlamak için çağrı `SetCategory` yöntemi `Notification.Builder` nesnesinin kategori ayarı. Örneğin:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

*Rahatsız Etmeyin* (Android 5.0 ile yeni) özellik bildirimleri kategorilerine göre filtreler. Örneğin, *Rahatsız Etmeyin* ekran **ayarları** muaf bildirimler kullanıcının telefon aramaları ve iletileri için izin verir:

![Ekran anahtarları rahatsız etmeyin](local-notifications-images/26-do-not-disturb.png)

Kullanıcı ne zaman yapılandırır *Rahatsız Etmeyin* (yukarıdaki ekran görüntüsünde gösterildiği gibi) aramaları hariç tüm kesintileri önlemek için bir kategori ayarı ile bildirimleri Android sağlayan `Notification.CategoryCall` cihazı sunulacak içinde *Rahatsız Etmeyin* modu. Unutmayın `Notification.CategoryAlarm` bildirimleri hiçbir zaman engellendiğini *Rahatsız Etmeyin* modu.


<a name="compatibility" />

## <a name="compatibility"></a>Uyumluluk

Bir uygulama oluşturuyorsanız, Android (kısa sürede API düzey 4), önceki sürümlerinde de çalıştırmak kullanacağınız [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) sınıfı yerine `Notification.Builder`. Bildirimlerle oluşturduğunuzda `NotificationCompat.Builder`, Android temel bildirim içerik eski cihazlarda doğru görüntülendiğini sağlar. Ancak, bazı gelişmiş bildirimi özellikleri eski Android sürümlerinde kullanılabilir olmadığından, kodunuzu genişletilmiş bildirim stilleri, kategoriler ve görünürlük düzeyleri aşağıda açıklandığı gibi uyumluluk sorunlarını açıkça işlemelidir.

Kullanılacak `NotificationCompat.Builder` uygulamanızda eklemelisiniz [Android desteği kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) projenizdeki NuGet.

Aşağıdaki kod örneği kullanarak temel bir bildirimi nasıl oluşturulacağı `NotificationCompat.Builder`:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();
```

Bu örnekte gösterildiği gibi önemli bildirim seçeneklerini yöntem çağrılarında gereksinimlerine aynı `Notification.Builder`. Ancak, daha karmaşık bildirimleri (sonraki bölümde açıklanmıştır) için aşağı uyumluluk sorunlarını işlemek kodunuzu vardır.

[LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) örnek nasıl kullanılacağını gösterir `NotificationCompat.Builder` bildirim alanından ikinci bir etkinlik başlatmak için. Bu örnek kod bölümünde açıklanan [Xamarin.Android kullanarak yerel bildirimleri](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) gözden geçirme.


### <a name="notification-styles"></a>Bildirim stilleri

Oluşturulacak *büyük metin*, *görüntü*, veya *gelen* stil ile bildirimleri `NotificationCompat.Builder`, uygulamanızı bu stiller uyumluluk sürümleri kullanmanız gerekir. Örneğin, kullanılacak *büyük metin* stil, örneği `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Benzer şekilde, uygulamanız kullanabilir `NotificationCompat.InboxStyle` ve `NotificationCompat.BigPictureStyle` için *gelen* ve *görüntü* stiller, sırasıyla.


### <a name="notification-priority-and-category"></a>Bildirim önceliğini ve kategorisini

`NotificationCompat.Builder` destekleyen `SetPriority` yöntemi (kullanılabilir Android 4.1 başlayarak). Ancak, `SetCategory` yöntemi *değil* tarafından desteklenen `NotificationCompat.Builder` kategorileri, Android 5.0 ile sunulan yeni bildirim meta veri sistemini bir parçası olduğundan.

Android, burada daha eski sürümlerini desteklemek üzere `SetCategory` olduğundan kullanılamıyor, kodunuzu API düzeyi koşullu olarak çağırmak için çalışma zamanında da denetleyebilirsiniz `SetCategory` zaman API düzeyi Android 5.0 (API düzey 21) değerinden büyük veya eşit:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

Bu örnekte, uygulama kullanıcının **hedef Framework'ü** Android 5.0 ayarlanır ve **en düşük Android sürümü** ayarlanır **Android 4.1 (API düzeyi 16)**. Çünkü `SetCategory` olan API düzey 21 ve sonraki sürümleri kullanılabilir, bu kod örneği çağıracak `SetCategory` yalnızca kullanılabilir olduğunda &ndash; değil çağıracak `SetCategory` API düzeyi olduğunda küçüktür
21.


### <a name="lockscreen-visibility"></a>Kilit ekranı görünürlük

Android, Android 5.0 (API düzey 21), önce kilit ekranı bildirimleri desteklemediği `NotificationCompat.Builder` desteklemediği `SetVisibility` yöntemi. İçin yukarıda açıklandığı gibi `SetCategory`, kodunuzun çalışma zamanı ve çağrı API düzeyinde denetleyebilirsiniz `SetVisiblity` yalnızca kullanılabilir olduğunda:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>Özet

Bu makalede, Android yerel bildirimleri oluşturma açıklanmıştır. Bir bildirim anatomisi açıklanan, nasıl kullanılacağı açıklanıyor `Notification.Builder` bildirimleri oluşturmak için nasıl büyük simge stil Bildirimlerde *büyük metin*, *görüntü* ve *gelen kutusu*  biçimleri, öncelik, görünürlük ve kategorisi gibi meta veri ayarları bildirim ayarlama ve nasıl bildirim etkinliği başlatın. Bu bildirim ayarlarının yeni uyarı, kilit ekranı, nasıl çalıştığını da bu makalede açıklanan ve *Rahatsız Etmeyin* Android 5.0 ile sunulan özellikler. Son olarak, size nasıl kullanacağınızı öğrendiniz `NotificationCompat.Builder` önceki sürümleriyle Android bildirim uyumluluğu korumak için.

Android için tasarlama bildirimleri hakkında yönergeler için bkz: [bildirimleri](http://developer.android.com/preview/notifications.html).


## <a name="related-links"></a>İlgili bağlantılar

- [NotificationsLab (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (örnek)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android kılavuzda yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Bildirimler](http://developer.android.com/design/patterns/notifications.html)
- [Kullanıcıya bildirme](http://developer.android.com/training/notify-user/index.html)
- [Bildirim](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
