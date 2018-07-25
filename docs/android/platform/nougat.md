---
title: Nougat özellikleri
description: Android Nougat için uygulamalar geliştirmek için Xamarin.Android kullanmaya başlamak nasıl.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 2e15de944f05fede802dbf52987d80a46fb890ef
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242159"
---
# <a name="nougat-features"></a>Nougat özellikleri

_Android Nougat için uygulamalar geliştirmek için Xamarin.Android kullanmaya başlamak nasıl._

Bu makalede Android Nougat içinde sunulan özelliklerin bir özetini Xamarin.Android Nougat Android geliştirme için hazırlamak üzere nasıl açıklar ve içinde Android Nougat özelliklerinin nasıl kullanılacağını gösteren örnek uygulamalar için bağlantılar sağlar sağlar. Xamarin.Android uygulamaları için.


## <a name="overview"></a>Genel Bakış

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) Android 6.0 Marshmallow için Google'nın izleme olduğu. Xamarin.Android için destek sağlar **Android 7.x bağlamaları** Xamarin Android 7.0 ve üzeri. Android Nougat çok sayıda yeni API için aşağıda açıklanan Nougat özellikler ekler; Bu API'ler, Xamarin.Android 7.0 kullandığınızda Xamarin.Android uygulamaları için kullanılabilir.

[![Android tabletleri ve telefonları Android Nougat çalışan Hero görüntülerini](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Android 7.x API'leri hakkında daha fazla bilgi için bkz: [geliştiriciler için Android 7.1](http://developer.android.com/preview/api-overview.html).
Xamarin.Android 7.0 bilinen sorunların bir listesi için lütfen bkz [sürüm notları](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android Nougat, Xamarin.Android geliştiricilerine birçok yeni ilgi çekici özellik sunar. Bu özellikler şunlardır:

-   **Çoklu pencere Destek** &ndash; bu geliştirme, kullanıcıların ekranda iki uygulama aynı anda açmak için mümkün kılar.

-   **Bildirim geliştirmeleri** &ndash; Android Nougat yeniden tasarlanan bildirimleri sistemde içeren bir *doğrudan yanıt* metin iletileri için doğrudan gelen bildirim hızlı yanıt vermesini kullanıcıların özelliği KULLANICI ARABİRİMİ. Ayrıca, uygulama için bildirimleri oluşturursa alınan iletileri, yeni *bildirimleri paketlenmiş* özelliği paket bildirimleri birlikte tek bir grup birden fazla ileti alındığında.

-   **Veri koruyucu** &ndash; uygulamaların hücresel veri kullanımını azaltma yardımcı olan yeni bir sistem hizmet bu özelliğidir; uygulamaların hücresel veri kullanımını denetim kullanıcılara sağlar.

Ayrıca, diğer birçok geliştirmeyi Android Nougat getirir yeni bir ağ güvenlik yapılandırma özelliği gibi uygulama geliştiricileri için ilgi halindeyken Doze, kanıtlama, yeni hızlı ayarları API'leri, birden çok yerel destek, ICU4J API'leri WebView geliştirmeleri, anahtar Java 8 dil özelliklerine erişim olanağı, kapsamlı dizin erişimi, bir işaretçi özel API, platform VR desteği, sanal dosyaları ve arka plan en iyi duruma getirme.

Bu makalede, yeni özellikleri denemek ve yeni Android Nougat platformunu hedeflemek için geçiş ya da özellik iş planlamak için Android Nougat ile uygulama oluşturmaya başlamak açıklanmaktadır.


## <a name="requirements"></a>Gereksinimler

Aşağıdaki yeni Android Nougat özelliklerini Xamarin tabanlı uygulamalarında kullanmak için gereklidir:

-   **Visual Studio veya Mac için Visual Studio** &ndash; Visual Studio, sürüm 4.2.0.628 kullanıyorsanız veya Xamarin için Visual Studio Araçları'nın sonraki gereklidir. Mac için Visual Studio Mac sürümü 6.1.0 veya daha sonra Visual Studio kullanıyorsanız.

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 veya üstü yüklü ve Mac için Visual Studio veya Visual Studio ile yapılandırılmış

-   **Android SDK'sı** -Android SDK 7.0 (API 24) veya daha sonra Android SDK Yöneticisi aracılığıyla yüklenmesi gerekir.

-   **Java Developer Kit** &ndash; Xamarin Android 7.0 geliştirme gerektirir [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya API düzeyi için 24 geliştiriyorsanız sonraki ya da daha büyük (JDK 8 de destekler API düzeylerini 24'dan önceki). Özel denetimleri veya Forms Önizleyiciyi kullanıyorsanız JDK 8 64-bit sürümü gereklidir.

> [!IMPORTANT]
> Xamarin.Android JDK 9 desteklemez.

Uygulamaları Xamarin C6SR4 veya üzeri ile Android Nougat güvenilir bir şekilde çalışması için yeniden oluşturulması gerekir olduğunu unutmayın. Android Nougat yalnızca bağlayabilirsiniz çünkü [NDK tarafından sağlanan yerel kitaplıkları](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), kitaplıkları gibi kullanarak mevcut uygulamaları **Mono.Data.Sqlite.dll** düzgün değillerse Android Nougat'ta çalışırken çökebilir. yeniden.



## <a name="getting-started"></a>Başlarken

Android Nougat Xamarin.Android ile kullanmaya başlamak için indirin ve bir Android Nougat proje oluşturmadan önce en son araç ve SDK paketlerini yükleyin:

1.  Xamarin Xamarin.Android güncelleştirmeleri yükleyin.

2.  Yükleme **Android 7.0 (API 24)** paketler ve araçlar veya üzeri.

3.  Android Nougat hedefleyen yeni bir Xamarin.Android projesi oluşturun.

4.  Bir öykünücü veya cihazın Android Nougat için yapılandırın.

Bu adımların her biri, aşağıdaki bölümlerde açıklanmıştır:


### <a name="install-xamarin-updates"></a>Xamarin güncelleştirmeleri yükle

Xamarin Android Nougat desteği eklemek için Visual Studio veya Visual Studio güncelleştirmeleri kanal için Mac için kararlı kanal değiştirmek ve en son güncelleştirmeleri uygulayın. Ayrıca yalnızca alfa veya Beta kanalı şu anda kullanılabilir olan özelliklerin ihtiyacınız varsa (Alpha ve Beta kanalları da Android 7.x için desteği) alfa veya Beta kanalı geçiş yapabilirsiniz. Güncelleştirmeler (sürümleri) kanal değiştirme hakkında daha fazla bilgi için bkz: [güncelleştirme kanalını değiştirme](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel).



### <a name="install-the-android-sdk"></a>Android SDK'sını yükleyin

Xamarin ile Android 7.0 bir proje oluşturmak için önce Android SDK Yöneticisi'ni yüklemek için kullanmalısınız **SDK Platform Android N (API 24)** veya üzeri. En son kurmalısınız **Android SDK Tools**:

1.  Android SDK Yöneticisi'ni başlatın (Mac için Visual Studio'da kullanmak **Araçlar > Android SDK Yöneticisi'ni Aç&hellip;**; Visual Studio'da kullanmak **Araçlar > Android > Android SDK Yöneticisi**).

2.  Yükleme **Android 7.0 (API 24)** veya daha sonra:

    [![Android SDK Yöneticisi'nde Android 7.0 paketleri seçme](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  En son Android SDK Araçları'nı yükleme:

    [![Android SDK Yöneticisi'nde en son Android SDK araçlarının seçme](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Android SDK Tools düzeltme 25.2.2 veya üzeri, Android SDK Platform araçları 24.0.3 veya üzeri ve Android SDK derleme araçları 24.0.2 yüklemeniz gerekir ya da daha sonra.

4.  Doğrulayın **Java Development Kit konumunu** için JDK 1.8 yapılandırılır:

    [![Araçlar Seçenekler altındaki JDK 8 yol yapılandırma](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Visual Studio'da bu ayarı görüntülemek için tıklayın **Araçlar > Seçenekler > Xamarin > Android ayarları**. Mac için Visual Studio'da **Tercihler > Proje > SDK konumları > Android**.



### <a name="start-a-xamarinandroid-project"></a>Bir Xamarin.Android projesi Başlat

Yeni bir Xamarin.Android projesi oluşturun. Xamarin ile Android geliştirmesi için yeni başlıyorsanız bkz [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android projeleri oluşturma hakkında bilgi edinmek için.

Bir Android projesi oluşturduğunuzda, hedef Android 7.0 veya üzeri sürüm ayarlarını yapılandırmanız gerekir. Örneğin, projeniz için Android 7.0 hedeflemek için hedef Android API düzeyini projenize yapılandırmalısınız **Android 7.0 (API 24 - Nougat)**. API 24 arasındaki sürümleri veya daha sonra hedef framework düzeyinizi ayarlayın önerilir. Android API düzeyi düzeylerini yapılandırma hakkında daha fazla bilgi için bkz. [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> Şu anda ayarlamalısınız **en düşük Android sürümü** için **Android 7.0 (API 24 - Nougat)** Android Nougat cihazlar veya öykünücüleri uygulamanızı dağıtmak için.



### <a name="configure-an-emulator-or-device"></a>Bir öykünücü veya cihaz yapılandırma

Bir öykünücüsü kullanıyorsanız, Android AVD Yöneticisi'ni başlatın ve aşağıdaki ayarları kullanarak yeni bir cihaz oluşturun:

-   Cihaz: Nexus 5 X, Nexus 6, Nexus 6P, Nexus Player Nexus 9 veya piksel C.
-   Hedef: Android 7.0 - API düzeyi 24
-   ABI: x86 veya x86\_64

Örneğin, bu sanal cihazı, Nexus 6 benzetmek için yapılandırılır:

[![Bir AVD nexus 6 cihaz, hedef Android 7.0 ve Intel Atom x86 CPU/ABI kullanarak yapılandırma](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

X 5, 6 veya 9 bir Nexus gibi fiziksel bir cihaz kullanıyorsanız, ya da (OTA) havadan Güncelleştirmeler üzerinden otomatik Cihazınızı güncelleştirin veya sistem görüntüsü indirebilir ve Cihazınızı doğrudan flash. Cihazınız Android Nougat için el ile güncelleştirme hakkında daha fazla bilgi için bkz. [Nexus Cihazlarında kullanılamaz OTA görüntülerde](https://developers.google.com/android/nexus/ota).

Nexus 5 cihazları Android Nougat tarafından desteklenmediğini unutmayın.



## <a name="new-features"></a>Yeni Özellikler

Android Nougat çeşitli yeni özellikler ve çok pencere desteği, bildirimler geliştirmelerini ve veri koruyucu gibi özellikler sunar. Aşağıdaki bölümlerde, bu özellikleri vurgulayın ve bağlantıları yardımcı olmak için bunları uygulamanızda kullanmaya başlama sağlayın.



### <a name="multi-window-mode"></a>Çoklu pencere modu

Çoklu pencere modu, iki uygulama aynı anda tam çok görevli desteğiyle kullanıcıların mümkün kılar. Bu uygulamalar, yan yana (yatay) veya bir yukarıda--diğer (dikey) bölünmüş ekran modunda çalıştırabilirsiniz.
Kullanıcılar, bunları yeniden boyutlandırmak için uygulamalar arasında bir ayırıcı sürükleyebilirsiniz ve Kes ve Yapıştır içerik uygulamalar arasında. İki uygulama çoklu pencere modu yansıtılırken seçili etkinlik seçilmemiş etkinlik duraklatıldı ancak yine de görünür ederken çalışmaya devam eder. Çoklu pencere modu Android etkinlik yaşam döngüsü değiştirmez.

[![Çoklu pencere hem dikey ve yatay modda çalışan örnek uygulamalar](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Xamarin.Android uygulamanıza etkinliklerini çoklu pencere modu'ı nasıl destekler yapılandırabilirsiniz. Örneğin, en düşük boyut ve varsayılan yükseklik ve genişlik uygulamanızın çoklu pencere modu ayarlamak öznitelikleri yapılandırabilirsiniz. Kullanabileceğiniz yeni `Activity.IsInMultiWindowMode` etkinlik çok pencere modunda olup olmadığını belirlemek için özellik. Örneğin:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) örnek uygulaması anlatan kullanıcı arabirimleri uygulamanızdan birden çok pencere yararlanmak C# kodu içerir.

Çoklu pencere modu hakkında daha fazla bilgi için bkz. [çok pencere desteği](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Gelişmiş bildirimleri

Yeniden tasarlanan bildirim sistemi Android Nougat tanıtır. Bu, kullanıcılar için hızlı bir şekilde yanıt bildirimleri doğrudan bildirimdeki UI gelen metin iletileri için mümkün kılan yeni bir doğrudan yanıt özelliği sunar. Android 7.0 ile birden fazla ileti alındığında iletileri birlikte tek bir grup birlikte bildirim başlatılıyor. Ayrıca, geliştiricilerin görünümleri, sistem süslemeleri Bildirimlerde yararlanın ve bildirimleri oluştururken yeni bildirim şablonları yararlanmak bildirim özelleştirebilirsiniz.


#### <a name="direct-reply"></a>Doğrudan yanıt

Bir kullanıcı bir bildirim gelen ileti aldığında, Android Nougat, içinde bildirim iletisini yanıtlama (yerine bir yanıt göndermesi için Mesajlaşma uygulamasını açın) mümkün kılar.
Bu satır içi yanıt özellik, kullanıcıların bir SMS veya metin iletisi bildirim arabiriminden doğrudan hızla yanıt mümkün kılar:

[![Satır içi doğrudan yanıt alanı içeren bir bildirim ekran görüntüsü](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Uygulamanızda bu özelliği desteklemek için eklemelisiniz *satır içi yanıt eylemleri* uygulamanıza bir [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) kullanıcıların metin doğrudan bildirimden UI yanıt verebilir böylece nesne.
Örneğin, yapılar aşağıdaki kod bir `RemoteInput` metin girişi almak için bekleyen bir yanıt eylemi hedefini oluşturur ve Giriş bir uzak etkin eylem oluşturur:

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

Bu eylem, bildirim eklenir:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[Mesajlaşma hizmeti](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) örnek uygulaması nasıl genişletileceği ile ilgili bildirimleri gösteren C# kodu içeren bir `RemoteInput` nesne. Satır içi yanıt ekleme hakkında daha fazla bilgi için eylemleri uygulamanıza Android 7.0 veya üzeri, Android görmek [bildirimleri yanıtlama](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) konu.


#### <a name="bundled-notifications"></a>İle birlikte gelen bildirimler

Android Nougat, bildirim iletileri (örneğin,. ileti konusuna göre) bir arada gruplandırabilir ve grubu yerine ayrı her bir mesaj görüntüler.
Bu *bildirimleri paketlenmiş* özellik mümkün kılar kapatmak veya bildirimleri bir eylem grubu arşiv kullanıcılar için. Kullanıcı, paket her bildirim ayrıntılı olarak görüntülemek için bildirim genişletmek için aşağı kaydırabilirsiniz:

[![Ekran örnek ile birlikte gelen bildirimler](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Uygulamanız ile birlikte gelen bildirimlerini desteklemek için kullanabilir [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) benzer bildirimleri paket için yöntemi. Android n ile birlikte gelen bildirim grupları hakkında daha fazla bilgi için bkz: Android [paketleme bildirimleri](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) konu.


#### <a name="custom-views"></a>Özel görünümler

Android Nougat sistem bildirim üstbilgileri, Eylemler ve Genişletilebilir düzenleri ile özel bir bildirim görünümleri oluşturmanıza olanak sağlar. Android özel bildirim Android Nougat görünümlerde hakkında daha fazla bilgi için bkz. [bildirim geliştirmeleri](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) konu.



### <a name="data-saver"></a>Veri koruyucu

Android Nougat ile başlayarak, kullanıcıların yeni bir etkinleştirebilirsiniz *veri koruyucu* blokları, veri kullanımı arka plan ayarlama. Bu ayar, mümkün olduğunda ön planda daha az veri kullanmak için uygulamanızı da bildirir. [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) uygulamanızı uygulamanızı veri koruyucu etkin olduğunda, veri kullanımını sınırlamak için çaba yapabilmeleri için veri koruyucu kullanıcı etkinleştirilmiş olup olmadığını denetleyebilir Android Nougat genişletilmiştir.

Android için yeni Android Nougat veri koruyucu özelliği hakkında daha fazla bilgi için bkz. [ağ veri kullanımını en iyi duruma getirme](https://developer.android.com/training/basics/network-ops/data-saver.html) konu.



### <a name="app-shortcuts"></a>Uygulama kısayolları

Android 7.1 sunulan bir *uygulama kısayolları* , kullanıcılar için hızlı başlangıç ortak ya da önerilen görevleri uygulamanızla mümkün kılan özellik.
Kısayol menüsünü etkinleştirmek için kullanıcının uzun-uygulama simgesine ikinci bir veya daha fazla basarsa &ndash; ile hızlı bir titreşim menü görünür.
Baskı serbest menüsünün kalmasına neden olur:

[![Örnek ekran bir Mesajlaşma uygulaması için bir uygulama kısayol menüsü](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Bu özellik kullanılabilir yalnızca API düzeyi 25 veya daha yüksek olur.
Android için yeni Android 7.1 uygulama kısayolları özelliği hakkında daha fazla bilgi için bkz. [uygulama kısayolları](https://developer.android.com/guide/topics/ui/shortcuts.html) konu.


### <a name="sample-code"></a>Örnek kod

Xamarin.Android örnekleri birkaç Android Nougat özelliklerden yararlanmak nasıl göstermek kullanılabilir:

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) çoklu pencere API Android Nougat kullanılabilir kullanımını gösterir. Örnek uygulama uygulama yaşam döngüsü ve davranışını nasıl etkilediğini görmek için çok windows moduna geçebilirsiniz.

-   [Mesajlaşma hizmeti](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) bildirimleri gönderen basit bir hizmet kullanarak `NotificationCompatManager`. Ayrıca bir bildirim ile genişleten bir `RemoteInput` uygulama açmak zorunda kalmadan doğrudan gelen bildirim metin yanıtlamak Android Nougat cihazlara izin vermek için nesne.

-   [Etkin bildirimleri](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) nasıl kullanılacağını gösteren `NotificationManager` API uygulamanız şu anda görüntüleme kaç bildirim söylemek için.

-   [Dizin Erişimi kapsamlı](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) belirli dizinleri kolayca erişmek için API kapsamlı dizin erişimi nasıl yapılacağı açıklanır. Tanımlamak zorunda alternatif olarak hizmet veren `READ_EXTERNAL_STORAGE` veya `WRITE_EXTERNAL_STORAGE` bildiriminizdeki izinleri.

-   [Önyükleme doğrudan](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) her zaman cihaz hem önce ve sonra tüm kullanıcı credentials(PIN/Pattern/Password) girilen önyüklenirken kullanılabilir olan bir cihaz şifrelenmiş Storage'a depoladığınız veriler gösterilmektedir.


## <a name="summary"></a>Özet

Bu makalede, Android Nougat sunulan ve yükleme ve en son araçları ve Xamarin.Android geliştirme paketleri Android Nougat'ta yapılandırma adımları açıklanmıştır. Android Nougat için uygulama oluşturmaya başlamanıza yardımcı olmak için kaynak kod örneği için bağlantılarla birlikte de Android Nougat içinde kullanılabilir anahtar özelliklerine genel bakış sağlanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Geliştiriciler için Android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 sürüm notları](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
