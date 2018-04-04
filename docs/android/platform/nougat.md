---
title: Nougat özellikleri
description: Xamarin.Android uygulamaları için Android Nougat geliştirmek için kullanmaya başlamak nasıl.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: fe544f8ac677987f8921ccb1c11b8930811b9553
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="nougat-features"></a>Nougat özellikleri

_Xamarin.Android uygulamaları için Android Nougat geliştirmek için kullanmaya başlamak nasıl._

Bu makalede Android Nougat içinde sunulan özellikler ana hattı Android Nougat geliştirme için Xamarin.Android için nasıl hazırlanılacağını açıklamaktadır ve Android Nougat özelliklerinin nasıl kullanılacağını gösteren örnek uygulamalar için bağlantılar sağlar sağlar Xamarin.Android uygulamaları.


## <a name="overview"></a>Genel Bakış

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) Google izleme Android 6.0 Marshmallow için değil. Xamarin.Android için destek sağlar **Android 7.x bağlamaları** Xamarin Android 7.0 ve üzeri. Android Nougat aşağıda açıklanan Nougat özellikleri için birçok yeni API'leri ekler; Xamarin.Android 7.0 kullandığınızda bu API'leri Xamarin.Android uygulamaları için kullanılabilir.

[![Android tabletleri ve telefonları Android Nougat çalıştıran kahramanı görüntülerini](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Android 7.x API'ler hakkında daha fazla bilgi için bkz: [geliştiriciler için Android 7.1](http://developer.android.com/preview/api-overview.html).
Xamarin.Android 7.0 ile ilgili bilinen sorunlar listesi için lütfen bkz [sürüm notları](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android Nougat Xamarin.Android geliştiricilerine ilgi birçok yeni özellik sağlar. Bu özellikler şunlardır:

-   **Çok pencere desteği** &ndash; bu geliştirme, aynı anda iki uygulamalar ekranında açmak kullanıcılar için mümkün kılar.

-   **Bildirim geliştirmeleri** &ndash; Android Nougat yeniden tasarlanan bildirimleri sistemde içeren bir *doğrudan yanıt* metin iletileri için doğrudan bildirimden hızlı yanıt vermek kullanıcıların özelliği KULLANICI ARABİRİMİ. Ayrıca, uygulamanız için bildirimleri oluşturursa alınan iletileri, yeni *bildirimleri paketlenmiş* özelliği paket bildirimleri birlikte tek bir grup olarak birden fazla ileti alındığında.

-   **Veri koruyucu** &ndash; bu özellik, uygulamaların hücresel veri kullanımı azaltılmasına yardımcı olan yeni bir sistem hizmeti; kullanıcılara nasıl uygulamaların hücresel veri kullanabileceğini üzerinde denetim sağlar.

Ayrıca, diğer birçok iyileştirme Android Nougat getiren yeni bir ağ güvenlik yapılandırması özelliği gibi uygulama geliştiriciler için ilgi hareket Doze, kanıtlama, hızlı ayarları API'leri yeni çok yerel destek, ICU4J API'leri WebView geliştirmeleri, anahtar Java 8 dil özelliklerine erişimi, kapsamlı dizin erişimi, bir özel işaretçi API, platform VR desteği, sanal dosyaları ve arka plan en iyi duruma getirme işlemleri.

Bu makalede, yeni özellikleri denemek ve yeni Android Nougat platformu hedeflemek için geçiş ya da özellik iş planlamak için Android Nougat uygulamalarla oluşturmaya başlamak açıklanmaktadır.


## <a name="requirements"></a>Gereksinimler

Aşağıdaki Xamarin tabanlı uygulamalarda yeni Android Nougat özellikleri kullanmak için gereklidir:

-   **Visual Studio veya Mac için Visual Studio** &ndash; Visual Studio, sürüm 4.2.0.628 kullanıyorsanız veya Visual Studio için Xamarin sonraki gereklidir. Mac için Visual Studio for Mac, sürüm 6.1.0 veya daha sonra Visual Studio kullanıyorsanız.

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 veya üstü yüklü ve Mac için Visual Studio veya Visual Studio ile yapılandırılmış

-   **Android SDK** -Android SDK 7.0 (API 24) veya sonrası Android SDK Yöneticisi aracılığıyla yüklü olmalıdır.

-   **Java Geliştirme Seti** &ndash; Xamarin Android 7.0 geliştirme gerektirir [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya API düzeyi 24 geliştiriyorsanız sonraki ya da daha büyük (JDK 8 de destekler API düzeylerini 24'den önceki). Özel denetimler ya da formları genele gitmeyi kullanıyorsanız JDK 8'in 64 bit sürümü gereklidir.

> [!IMPORTANT]
> Xamarin.Android JDK 9 desteklemez.

Uygulamaları Xamarin C6SR4 veya sonrası ile Android Nougat güvenilir bir şekilde çalışması için yeniden oluşturulması gerekir olduğunu unutmayın. Android Nougat yalnızca bağlayabilirsiniz çünkü [NDK tarafından sağlanan yerel kitaplıkları](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), kitaplıklar gibi kullanarak mevcut uygulamaları **Mono.Data.Sqlite.dll** düzgün değillerse Android Nougat üzerinde çalışırken çökebilir yeniden.



## <a name="getting-started"></a>Başlarken

Android Nougat Xamarin.Android ile kullanmaya başlamak için indirin ve Android Nougat proje oluşturmadan önce SDK paketlerini ve en son araçları yüklemeniz gerekir:

1.  Xamarin en son Xamarin.Android güncelleştirmeleri yükleyin.

2.  Yükleme **Android 7.0 (API 24)** paketler ve Araçlar ya da daha sonra.

3.  Yeni bir Xamarin.Android projesi hedefleyen Android Nougat oluşturun.

4.  Bir öykünücü veya aygıt için Android Nougat yapılandırın.

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır:


### <a name="install-xamarin-updates"></a>Xamarin güncelleştirmeleri yükle

Android Nougat Xamarin desteği eklemek için Visual Studio veya Visual Studio güncelleştirmeleri kanalda Mac için kararlı kanala değiştirmek ve en son güncelleştirmeleri uygulayın. Yalnızca alfa veya Beta kanalda şu anda kullanılabilen özellikleri de gerekiyorsa (alfa ve Beta kanalları ayrıca Android 7.x için desteği) alfa veya Beta kanal geçiş yapabilirsiniz. Güncelleştirmeler (sürümler) kanal değiştirme hakkında daha fazla bilgi için bkz: [güncelleştirmeleri kanal değiştirme](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).



### <a name="install-the-android-sdk"></a>Android SDK'sını yükleyin

Xamarin Android 7.0 ile bir proje oluşturmak için önce Android SDK Yöneticisi'ni yüklemek için kullanmanız gerekir **SDK Platform Android N (API 24)** veya sonraki bir sürümü. En son de yüklemelisiniz **Android SDK Araçları**:

1.  Android SDK Yöneticisi'ni başlatın (Mac için Visual Studio'da kullanın **Araçlar > Open Android SDK Manager&hellip;**; Visual Studio'da kullanım **Araçlar > Android > Android SDK Manager**).

2.  Yükleme **Android 7.0 (API 24)** ya da daha sonra:

    [![Android 7.0 paketleri Android SDK Yöneticisi'nde seçme](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  En son Android SDK Araçları'nı yükleyin:

    [![Android SDK Yöneticisi'nde en yeni Android SDK Araçları seçme](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Android SDK Araçları Düzeltme 25.2.2 veya üzeri, Android SDK platformu araçları 24.0.3 veya üstü ve Android SDK derleme araçlarını 24.0.2 yüklemeniz gerekir ya da daha sonra.

4.  Doğrulayın **Java Geliştirme Seti konumu** için JDK 1.8 yapılandırılır:

    [![Araçlar Seçenekler altında JDK 8 yolu yapılandırma](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Visual Studio'da bu ayarı görüntülemek için tıklatın **Araçlar > Seçenekler > Xamarin > Android ayarları**. Mac için Visual Studio'da sırasıyla **Tercihler > projeleri > SDK konumları > Android**.



### <a name="start-a-xamarinandroid-project"></a>Start a Xamarin.Android Project

Yeni bir Xamarin.Android projesi oluşturun. Xamarin Android geliştirme yeniyseniz, bkz: [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android projeleri oluşturma hakkında bilgi edinmek için.

Bir Android projesi oluşturduğunuzda, hedef Android 7.0 veya üzeri sürüm ayarlarını yapılandırmanız gerekir. Örneğin, projeniz için Android 7.0 hedeflemek için projenizi için hedef Android API düzeyini yapılandırmalısınız **Android 7.0 (API 24 - Nougat)**. API 24 ya da daha sonra hedef framework düzeyinizi ayarlayın önerilir. Android API düzeyi düzeyleri yapılandırma hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> Şu anda ayarlamalısınız **Minimum Android sürümü** için **Android 7.0 (API 24 - Nougat)** Android Nougat aygıtları veya Öykünücüler uygulamanızı dağıtmak için.



### <a name="configure-an-emulator-or-device"></a>Bir öykünücü veya cihaz yapılandırma

Bir öykünücü kullanıyorsanız, Android AVD Yöneticisi'ni başlatın ve aşağıdaki ayarları kullanarak yeni bir cihaz oluşturma:

-   Device: Nexus 5X, Nexus 6, Nexus 6P, Nexus Player, Nexus 9, or Pixel C.
-   Hedef: Android 7.0 - API düzeyi 24
-   ABI: x86 veya x86\_64

Örneğin, bu sanal aygıt Nexus 6 benzetmek için yapılandırılır:

[![Nexus 6 cihaz, Android 7.0 hedef ve Intel Atom x86 CPU/ABI kullanarak AVD yapılandırma](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Bir Nexus X 5, 6 veya 9 gibi bir fiziksel cihaz kullanıyorsanız, ya da Cihazınızı otomatik hava (OTA) Güncelleştirmeler üzerinden güncelleştirebilir veya bir sistem görüntüsünü karşıdan yüklemek ve Cihazınızı doğrudan flash. Android Nougat Cihazınızı el ile güncelleştirme hakkında daha fazla bilgi için bkz: [OTA görüntüleri Nexus cihazlar için](https://developers.google.com/android/nexus/ota).

Not nexus 5 cihazlar Android Nougat tarafından desteklenmez.



## <a name="new-features"></a>Yeni Özellikler

Android Nougat çeşitli yeni özellikler ve çok pencere desteği, bildirimler geliştirmeleri ve veri koruyucu gibi özellikler sunar. Aşağıdaki bölümlerde bu özellikler vurgulayın ve bağlantıları yardımcı olmak için bunları uygulamanızda kullanmaya başlama sağlayın.



### <a name="multi-window-mode"></a>Çok pencere modu

Çok pencere modu, kullanıcıların iki uygulamalar aynı anda tam görevli desteğiyle açmak mümkün kılar. Bu uygulamalar, yan yana (yatay) veya bir yukarıda--diğer (dikey) bölünmüş ekran modunda çalıştırabilirsiniz.
Kullanıcılar, bunları yeniden boyutlandırmak için uygulamalar arasında bir ayırıcı sürükleyebilirsiniz ve kesme ve içeriği uygulamalar arasında. İki uygulama çok pencere modunda sunulduğunda seçili etkinliği seçili etkinlik duraklatıldı ancak hala görünür durumdayken çalışmaya devam eder. Çok pencere modu Android etkinlik yaşam döngüsü değiştirmez.

[![Örnek uygulamalar dikey ve yatay çok pencere modunda çalışıyor](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Xamarin.Android uygulamanıza etkinliklerini çok pencere modu'ı nasıl desteklediğini yapılandırabilirsiniz. Örneğin, en küçük boyut ve varsayılan genişliği ve yüksekliği, uygulamanızın kümesini çok penceresi modunda öznitelikleri yapılandırabilirsiniz. Kullanabileceğiniz yeni `Activity.IsInMultiWindowMode` etkinliklerinizi çok pencere modunda olup olmadığını belirlemek için özellik. Örneğin:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) örnek uygulaması kullanıcı arabirimleri, uygulamanız ile birden çok pencere yararlanmak nasıl gösteren C# kod içerir.

Çok pencere modu hakkında daha fazla bilgi için bkz: [çok pencere desteği](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Gelişmiş bildirimleri

Android Nougat yeniden tasarlanan bildirim sistemi tanıtır. Kullanıcılar için hızlı bir şekilde yanıt gelen metin iletileri doğrudan bildirim kullanıcı Arabirimi için bildirimlere mümkün kılan yeni bir doğrudan yanıt özellik sunar. Android 7.0 ile birden fazla ileti alındığında iletiler birlikte tek bir grup olarak gönderilebilir bildirim başlatılıyor. Ayrıca, geliştiricilerin görünümleri, sistem düzenlemelerinin Bildirimlerde yararlanır ve bildirimleri oluştururken yeni bildirim şablonları yararlanmak bildirim özelleştirebilirsiniz.


#### <a name="direct-reply"></a>Doğrudan Yanıtla

Bir kullanıcı gelen ileti için bir bildirim aldığında, Android Nougat, içinde bildirim iletisini yanıtlama (yerine bir yanıt göndermek üzere ileti uygulamasını açın) mümkün kılar.
Bu satır içi yanıt özellik, kullanıcıların bir SMS veya metin iletisi bildirim arabiriminden doğrudan hızla yanıt mümkün kılar:

[![Bir satır içi doğrudan yanıt alan içeren bir bildirim ekran görüntüsü](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Uygulamanızda bu özelliği desteklemek için eklemelisiniz *satır içi yanıt eylemlerini* uygulamanıza bir [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) böylece kullanıcıların doğrudan bildirimden kullanıcı Arabirimi metin yanıt nesnesi.
Örneğin, aşağıdaki derlemeleri kod bir `RemoteInput` metin girişi almak için bekleyen bir yanıt eylemini hedefini oluşturur ve bir uzak giriş etkin eylem oluşturur:

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

Bu eylem için bildirim eklenir:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[İleti gönderme hizmeti](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) örnek uygulamasını bildirimleri ile genişletmek nasıl gösteren C# kodu içeren bir `RemoteInput` nesnesi. Satır içi yanıt ekleme hakkında daha fazla bilgi için eylemleri uygulamanıza Android 7.0 veya üzeri, Android görmek [bildirimlerini yanıtlama](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) konu.


#### <a name="bundled-notifications"></a>İle birlikte gelen bildirimleri

Android Nougat bildirim iletileri (örneğin,. ileti konusuna göre) gruplamak ve her ayrı ileti yerine Grup görüntüleyin.
Bu *bildirimleri paketlenmiş* özelliği mümkün kılar kapatmak veya bir eylem bildirimleri grubunun arşiv kullanıcı. Kullanıcı, her bir bildirim ayrıntılı olarak görüntülemek için bildirimler paket genişletmek için aşağı kaydırabilirsiniz:

[![Ekran örnek ile birlikte gelen bildirimler](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

İle birlikte gelen bildirimlerini desteklemek için uygulamanızı kullanabilirsiniz [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) benzer bildirimleri gruplanacağını yöntemi. Android Android N ile birlikte gelen bildirim grupları hakkında daha fazla bilgi için bkz: [paketleme bildirimleri](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) konu.


#### <a name="custom-views"></a>Özel görünümler

Android Nougat sistem bildirim üstbilgileri, Eylemler ve Genişletilebilir düzenleri özel bildirim görünümleri oluşturmak mümkün hale getirir. Android Android Nougat içindeki özel bildirim görünümler hakkında daha fazla bilgi için bkz: [bildirim geliştirmeleri](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) konu.



### <a name="data-saver"></a>Veri koruyucu

Android Nougat ile başlayarak, kullanıcıların yeni bir etkinleştirebilirsiniz *veri koruyucu* blokları veri kullanımı arka plan ayarlama. Bu ayar ayrıca, mümkün olduğunda ön planda daha az veri kullanmak için uygulamanızı işaret eder. [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) uygulamanızı yapabilmesi uygulamanızı çaba veri koruyucu etkinleştirildiğinde, kendi veri kullanımı sınırlamak için kullanıcı veri koruyucu etkinleştirilmiş olup olmadığını denetleyebilmeniz içinde Android Nougat genişletilmiştir.

Android Android Nougat yeni veri koruyucu özelliği hakkında daha fazla bilgi için bkz: [ağ veri kullanımı en iyi duruma getirme](https://developer.android.com/training/basics/network-ops/data-saver.html) konu.



### <a name="app-shortcuts"></a>Uygulama kısayolları

Android 7.1 sunulan bir *uygulama kısayolları* kullanıcılar için hızlı başlangıç ortak ya da önerilen görevleri uygulamanız ile mümkün kılan özellik.
Kısayol menüsünü etkinleştirmek için kullanıcının uzun-uygulama simgesini ikinci bir veya daha fazla bilgi için basarsa &ndash; ile hızlı titreşimi menüsü görüntülenir.
Tuşuna serbest menüsünün kalmasına neden olur:

[![İleti bir uygulama için bir uygulama kısayol menüsünün örnek ekran](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Bu özellik kullanılabilir yalnızca API düzeyi 25 veya daha yüksek olur.
Android Android 7.1 yeni uygulama kısayolları özelliği hakkında daha fazla bilgi için bkz: [uygulama kısayolları](https://developer.android.com/guide/topics/ui/shortcuts.html) konu.


### <a name="sample-code"></a>Örnek kod

Android Nougat özelliklerden yararlanmak nasıl göstermek birkaç Xamarin.Android örnekler vardır:

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) içinde Android Nougat kullanılabilir çok penceresi API kullanımını gösterir. Örnek uygulamayı uygulamanın yaşam döngüsü ve davranışı nasıl etkilediğini görmek için birden çok windows moduna geçiş yapabilirsiniz.

-   [İleti hizmeti](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) bildirimleri gönderen basit bir hizmeti kullanarak `NotificationCompatManager`. Aynı zamanda bildirim ile genişletir bir `RemoteInput` Android Nougat cihazların bir uygulamayı açmak zorunda kalmadan doğrudan bildirimden metin yanıt vermesine izin vermek için nesne.

-   [Etkin bildirimleri](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) nasıl kullanılacağı ortaya `NotificationManager` API uygulamanız şu anda görüntüleme kaç bildirim söyleyin.

-   [Dizin Erişimi kapsamlı](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) belirli dizinleri kolayca erişmek için kapsamlı dizin erişimi API kullanımı gösterilmiştir. Bu tanımlamak zorunda alternatif olarak hizmet veren `READ_EXTERNAL_STORAGE` veya `WRITE_EXTERNAL_STORAGE` bildiriminizi izinleri.

-   [Önyükleme doğrudan](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) cihaz her ikisi de önce ve sonra tüm kullanıcı credentials(PIN/Pattern/Password) girilir önyüklenirken, her zaman kullanılabilir olan aygıt şifrelenmiş bir depoda verileri depolamak nasıl gösterir.


## <a name="summary"></a>Özet

Bu makalede, Android Nougat tanıtılan ve yüklemek ve en yeni araç ve xamarin Android geliştirme için paketler üzerinde Android Nougat yapılandırmak nasıl açıklanmıştır. Uygulamalar için Android Nougat oluşturmada başlamanıza yardımcı olmak için kaynak kod örneği için bağlantılar ile birlikte de Android Nougat içinde kullanılabilir anahtar özelliklerine genel bakış sağlanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Geliştiriciler için Android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 sürüm notları](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
