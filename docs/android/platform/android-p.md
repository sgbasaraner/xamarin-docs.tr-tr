---
title: Android P Önizleme
description: En son sürümünü Android uygulamaları geliştirmek için Xamarin.Android kullanmaya başlamak nasıl.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/27/2018
ms.openlocfilehash: a3eef6f2537a4b09f603787d7cdbf70a173fca80
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351873"
---
# <a name="android-p-preview"></a>Android P Önizleme

_En son sürümünü Android uygulamaları geliştirmek için Xamarin.Android kullanmaya başlamak nasıl._

![](~/media/shared/preview.png)

[Android P Geliştirici önizlemesi](https://developer.android.com/preview/) Google'dan kullanıma sunulmuştur. Yeni özellikler ve API'ler birkaç bu sürümde kullanılabilir yapıldığını ve bunların çoğu, en son Android cihazları yeni donanım özelliklerinden yararlanmak gereklidir.

![Android P hero görüntüsü](android-p-images/01-android-p-logo.png)

Bu makalede, Android P Önizleme için Xamarin.Android uygulamalar geliştirmeye başlamanıza yardımcı olmak üzere yapılandırılmıştır. Bu, bir öykünücü veya cihaz test için hazırlama gerekli güncelleştirmeleri yükleyin ve SDK'sını yapılandırmak nasıl açıklar. Ayrıca, Android P'daki yeni özelliklerin bir özetini sağlar ve bazı anahtar Android P özelliklerinin nasıl kullanıldığını gösteren bir örnek kaynak kodda sağlar.


## <a name="requirements"></a>Gereksinimler

Aşağıdaki Android P özelliklerini Xamarin tabanlı uygulamalarında kullanmak için gereklidir:

-   **Visual Studio** &ndash; sürümü Windows kullanıyorsanız, 15,8 Preview 5 veya Visual Studio'nun sonraki bir sürümü gereklidir.  Mac kullanıyorsanız, geçerli Beta sürümü veya üzeri Mac için Visual Studio'nun gereklidir.

-   **Xamarin.Android** &ndash; Xamarin.Android 9.0.0.17 veya sonrası ile Visual Studio yüklü olmalıdır.

-   **Java Developer Kit** &ndash; Xamarin Android 9.0 geliştirme gerektirir [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (ya da Microsoft'un dağıtımını önizlemesini deneyebilirsiniz [OpenJDK](~/android/get-started/installation/openjdk.md)).

-   **Android SDK'sı** &ndash; Android SDK API 28 ya da daha yeni Android SDK Yöneticisi aracılığıyla yüklenmesi gerekir.

## <a name="getting-started"></a>Başlarken

Android P Xamarin.Android ile kullanmaya başlamak için indirin ve ilk Android P projenizi oluşturmadan önce en son araç ve SDK paketlerini yükleyin:

1. Visual Studio 15,8 Preview 5 veya sonraki bir sürüme güncelleştirin. Mac için Visual Studio kullanıyorsanız, Mac için Visual Studio'ya geçiş [Beta](https://docs.microsoft.com/visualstudio/mac/update) kanal.

2. Yükleme **Android P (API 28)** veya üzeri paketler ve araçlar aracılığıyla SDK Yöneticisi.

3. Hedefleyen Android P (API 28) yeni bir Xamarin.Android projesi oluşturun.

4. Bir öykünücü veya cihazın Android P uygulamalarını test etme için yapılandırın.

Bu adımların her biri, aşağıdaki bölümlerde açıklanmıştır:


### <a name="update-visual-studio"></a>Visual Studio’yu güncelleştirme

Android P desteklemek için Visual Studio eklemek için Visual Studio 2017 sürüm 5 veya üzeri olarak anlatıldığı içinde 15,8 önizleme güncelleştirme [en son sürüme güncelleştirme Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/update-visual-studio).
Android P, Mac için Visual Studio için destek eklemek, güncelleştirmek Visual Studio 2017'in en son Beta sürümü için Mac için açıklandığı gibi [Mac için Visual Studio güncelleştirme](https://docs.microsoft.com/visualstudio/mac/update).


### <a name="install-the-android-sdk"></a>Android SDK'sını yükleyin

Xamarin.Android 9.0 ile bir proje oluşturmak için önce Android SDK Yöneticisi için SDK platform yüklemeye kullanmalısınız **Android P (API düzeyi 28)** veya üzeri.

1. SDK Yöneticisi'ni başlatın. Visual Studio'da **Araçlar > Android > Android SDK Yöneticisi**. Mac için Visual Studio'da **Araçlar > SDK Yöneticisi**.

2. Alt sağ üst köşedeki dişli simgesine tıklayın ve seçin **depo > Google (desteklenmiyor)**:

    [![İçin Google deposu ayarlama](android-p-images/vs/set-repo-sml.png)](android-p-images/vs/set-repo.png#lightbox)

3. Yükleme **Android P** olarak görüntülenen paket **Android SDK platformu 28** içinde **platformları** sekme ((Android SDK Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz. SDK'sı Setup)[~/android/get-started/installation/android-sdk.md]): 

    [![Android P paketlerini yükleme](android-p-images/vs/sdk-manager-sml.png)](android-p-images/vs/sdk-manager.png#lightbox)

4. Bir öykünücüsü kullanıyorsanız, destekleyen bir sanal cihaz oluşturma **API düzeyi 28**. Sanal cihaz oluşturma hakkında daha fazla bilgi için bkz. [yönetme sanal cihazların Android cihaz Yöneticisi'yle](~/android/get-started/installation/android-emulator/device-manager.md).



### <a name="start-a-xamarinandroid-project"></a>Bir Xamarin.Android projesi Başlat

Yeni bir Xamarin.Android projesi oluşturun. Xamarin ile Android geliştirmesi için yeni başlıyorsanız bkz [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android projeleri oluşturma hakkında bilgi edinmek için.

Bir Android projesi oluşturduğunuzda, hedef Android P sürüm ayarlarını yapılandırmanız gerekir veya üzeri. Örneğin, projeniz için Android P hedeflemek için hedef Android API düzeyini projenize yapılandırmalısınız **Android P (API 28)**. API 28 ya da daha sonra hedef çerçeve düzeyi de ayarlamanız önerilir. Android API düzeyi düzeylerini yapılandırma hakkında daha fazla bilgi için bkz. [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-a-device-or-emulator"></a>Bir cihaz veya öykünücü yapılandırın

Bir pikselin gibi fiziksel bir cihaz kullanıyorsanız cihazınızın Android P Önizleme yönergelerini takip ederek güncelleştirebilirsiniz [Android P Beta cihazları](https://developer.android.com/preview/devices/).

Bir öykünücüsü kullanıyorsanız, API düzeyi 28 x86 tabanlı bir görüntü kullanarak bir sanal cihaz oluşturma. Sanal cihazları oluşturmak ve yönetmek için Android cihaz Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz. [yönetme sanal cihazların Android cihaz Yöneticisi'yle](~/android/get-started/installation/android-emulator/device-manager.md).
Test ve hata ayıklama için Android öykünücüsünü kullanma hakkında daha fazla bilgi için bkz: [Google Android öykünücüsü ile hata ayıklama](~/android/deploy-test/debugging/android-sdk-emulator/index.md).



## <a name="new-features"></a>Yeni Özellikler

Android P birçok yeni özelliği tanıtır. Bu yeni özelliklerin bazıları, başkalarının daha da Android kullanıcı deneyimini geliştirmek için tasarlandığında en son Android cihazları tarafından sunulan yeni donanım özelliklerinden yararlanmak için tasarlanmıştır:

-   **Görüntüleme kesme Destek** &ndash; şekli ve konumunu bulmak için API'ler sağlar _kesme_ daha yeni Android cihazlarda ekranın üstünde.

-   **Bildirim geliştirmeleri** &ndash; bildirim iletilerini görüntüler ve yeni bir artık görüntüleyebilir `Person` sınıfı konuşma katılımcıları kolaylaştırmak için kullanılır.

-   **İç konumlandırma** &ndash; iç ayarlarında gezinme için Wi-Fi cihazları kullanmak için uygulamalarda mümkün kılar WiFi Round round-trip zamanı protokolü için Platform desteği.

-   **Birden fazla kamera Destek** &ndash; özelliği erişim akışlara aynı anda birden çok fiziksel kameralarından (örneğin, çift ön ve arka plan çift kameralar) sunar.


Aşağıdaki bölümlerde, bu özellikleri vurgulayın ve yardımcı olmak için kısa kod örnekleri, bunları uygulamanızda kullanmaya başlama sağlayın.

### <a name="display-cutout-support"></a>Görüntü kesme desteği

Edge edge ekranları ile çok sayıda yeni Android cihaz sahip bir *görüntüleme kesme* (veya "dişli") kamera ve hoparlör için ekranın üstünde.
Aşağıdaki ekran görüntüsünde, bir kesme öykünücü örneği sağlar:

[![Android öykünücüsü benzetimi kesme](android-p-images/02-example-cutout-sml.png)](android-p-images/02-example-cutout.png#lightbox)

Nasıl uygulama pencereniz içeriğinin bir görüntü kesme cihazlarla görüntüler yönetmek için Android P yeni eklemiştir [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) pencere düzeni özniteliği. Bu öznitelik aşağıdaki değerlerden birine ayarlanabilir:

-   [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; hiçbir zaman penceresi kesme alanı ile çakışıyor izin verilmez.

-   [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; penceresinde kesme alanına ancak ekran yalnızca kısa kenarlardaki genişletmek için izin verilen. 

-   [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; penceresinde kesme sistem çubuğu içinde yer alıyorsa kesme alanına genişletmek için izin verilen.

Örneğin, uygulama penceresinin kesme alanıyla örtüşmesini önlemek için Düzen Kesme moduna ayarlayın *hiçbir zaman*: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

Aşağıdaki örneklerde, bu kesme modları örnekler sağlanır. Sol taraftaki ilk ekran olmayan tam ekran modunda uygulama önemlidir. Merkezi ekran görüntüsünde, uygulama tam ekran gider ile `LayoutInDisplayCutoutMode` kümesine `LayoutInDisplayCutoutModeShortEdges`. Uygulamanın beyaz arka plan görüntü kesme alanına genişleten dikkat edin:

[![Örnek öykünücüsünde kesme modları görüntüleme](android-p-images/03-cutout-modes-sml.png)](android-p-images/03-cutout-modes.png#lightbox)

Son ekran görüntüsünde (yukarıda sağdaki), `LayoutInDisplayCutoutMode` ayarlanır `LayoutInDisplayCutoutModeShortNever` için tam ekran geçmeden önce.
Uygulamanın beyaz arka plan görüntü kesme alanına genişletmek için izin verilmiyor dikkat edin.

Cihazda kesme alanı hakkında daha ayrıntılı bilgi gerekiyorsa, yeni kullanabilirsiniz [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html) sınıfı. `DisplayCutout` içeriği görüntülemek için kullanılamaz görüntü alanını temsil eder. Böylece bu işlevsel olmayan alanda içeriğini görüntülemek uygulamanızı denemez kesme şekli ve konumunu almak için bu bilgileri kullanabilirsiniz.

Yeni Android P kesme özellikleri hakkında daha fazla bilgi için bkz. [görüntüleme kesme Destek](https://developer.android.com/preview/features#cutout).



### <a name="notifications-enhancements"></a>Bildirimleri geliştirmeleri

Android P Mesajlaşma deneyimini iyileştirmek üzere aşağıdaki geliştirmeleri sunar:

-   Bildirim kanallarını (sürümünde [Android Oreo](~/android/platform/oreo.md)) kanal grupları engelleme artık desteklemektedir.

-   Bildirim sistemine üç yeni-Not-rahatsız kategorisi (alarmlar, sistem seslerini ve ortam kaynaklarına öncelik) vardır. Ayrıca, görsel kesintiler (örneğin, Göstergeler, bildirim ışıklar, durum çubuğu görünümü ve tam ekran etkinliklerini başlatma) gizlemek için kullanılan yedi yeni-Not-rahatsız modu mevcuttur.

-   Yeni bir [kişi](https://developer.android.com/reference/android/app/Person.html) sınıfı, bir iletiyi gönderenin temsil etmek için eklendi. Bu sınıfın kullanımını, kişilere (kendi avatarlar ve bir URI'leri dahil) konuşmada ilgili belirleyerek her bildirim işlenmesi iyileştirmek için yardımcı olur.

-   Bildirimler artık resimleri görüntüleyebilirsiniz. 

Aşağıdaki örnek, bir görüntü içeren bir bildirim oluşturmak için yeni API'leri kullanmayı gösterir. Aşağıdaki ekran görüntüleri, SMS bildirimi gönderilir ve gömülü görüntü ile bir bildirim tarafından izlenir. Bildirimler (sağda görüldüğü gibi) genişletilir, ilk bildirim metni görüntülenir ve gömülü görüntü ikinci bildirim genişletilir:

[![Görüntü ile Örnek bildirim](android-p-images/04-example-notifications-sml.png)](android-p-images/04-example-notifications.png#lightbox)

Aşağıdaki örnek bir Android P bildiriminde görüntü ekleme gösterir ve yeni kullanımını gösterir `Person` sınıfı:

1. Oluşturma bir `Person` gönderen temsil eden nesne. Örneğin, gönderenin adı ve simgesi bulunan `fromPerson`:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. Oluşturma bir `Notification.MessagingStyle.Message` yeni görüntüyü geçirme göndermek için görüntüyü içeren [Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) yöntemi.
   Örneğin:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. İletiye bir `Notification.MessagingStyle` nesne. Örneğin:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. Bu stil bildirim oluşturucuya takın. Örneğin:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. Bildirim yayımlayın. Örneğin:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

Bildirimleri oluşturma hakkında daha fazla bilgi için bkz. [yerel bildirimleri](~/android/app-fundamentals/notifications/local-notifications.md).


### <a name="indoor-positioning"></a>İç konumlandırma

Android P IEEE 802.11mc için destek sağlar (olarak da bilinen _Round round-trip WiFi-zaman_ veya _WiFi RTT_), hangi mümkün kılar bir uzaklık algılamak uygulamalar için veya daha fazla Wi-Fi erişim noktası. Bu bilgileri kullanarak, uygulamanızın yararlanmak mümkündür *iç konumlandırma* ile bir doğruluk iki ölçütlerden. IEEE 801.11mc için donanım desteği sağlayan Android cihazlarda uygulamanıza akıllı Gereçleri veya bir mağazadaki Aç tarafından bırakma yönergeleri konum tabanlı denetim gibi Gezinti özellikleri sunmaktadır:

[![İç Gezinti WiFi RTT kullanma örneği](android-p-images/05-wifi-rtt-sml.png)](android-p-images/05-wifi-rtt.png#lightbox)

Yeni [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) sınıfı ve çeşitli yardımcı sınıfları sağlar uzaklık Wi-Fi cihazlara ölçme. Android P sunulan iç konumlandırma API'leri hakkında daha fazla bilgi için bkz. [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary).


### <a name="multi-camera-support"></a>Birden fazla kamera desteği

Çok sayıda yeni Android cihazları stereo görme, Gelişmiş görsel efektler ve geliştirilmiş yakınlaştırma özelliği gibi özellikler için yararlı olan çift-front ve/veya çift arka kamera vardır. Yeni bir Android P tanıtır [çok kamera](https://developer.android.com/preview/features#camera) API uygulamanızı kullanmayı mümkün kılan bir *mantıksal kamera* (veya *mantıksal çok kamera*) iki veya daha fazlası tarafından desteklenen fiziksel kamera.
Bir mantıksal çoklu kamera cihaz destekliyorsa, göz önünde destekleyip desteklemediğini görmek için her cihazdaki kameranın özelliklerini belirlemek için [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA).

Android P de içeren yeni bir [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) ilk yakalama sırasında gecikmelerini azaltmak ve gerekmemesi ve kamera akışını başlatmak için yardımcı olmak için kullanılan sınıf.

Birden fazla kamera hakkında daha fazla bilgi için destek Android P, bkz: [çok kamera desteği ve kamera güncelleştirmeleri](https://developer.android.com/preview/features#camera).


### <a name="other-features"></a>Diğer özellikler

Ayrıca, birçok yeni özellik Android P destekler:

-   Yeni [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) animasyonlu görüntüleri görüntüleme ve çizim için kullanılan sınıf.

-   Yeni bir [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) yerini alan sınıfı `BitmapFactory`. `ImageDecoder` kod çözme için kullanılan bir `AnimatedImageDrawable`.

-   Görüntüleri HDR (yüksek dinamik aralık) video ve HEIF (yüksek verimlilik resim dosyası biçimi) için destek.

-   [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) işleri ağ ile ilgili daha akıllıca işlemek için geliştirilmiştir. Yeni [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) yöntemi [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters) sınıfı için belirli bir işin tüm ağ istekleri gerçekleştirmek için en iyi ağ döndürür.

En son Android P özellikler hakkında daha fazla bilgi için bkz. [Android P özellikler ve API'ler](https://developer.android.com/preview/features).


## <a name="behavior-changes"></a>Davranış değişiklikleri

Hedef Android sürümü API düzeyini 28 ayarlandığında bile, yukarıda açıklanan yeni özellikler uygulanmamasının uygulamanızın davranışını etkileyen çeşitli platform değişiklikleri vardır. Bu değişikliklerin kısa bir özeti verilmiştir:

-  Uygulamalar, artık ön plan Hizmetleri kullanmadan önce ön plan izin istemesi gerekir.

-  Uygulamanız birden fazla işlem varsa, tek bir paylaşamaz [WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) işlemler arasında veri dizini.

-  Başka bir uygulamanın veri dizini yolu tarafından doğrudan erişim artık izin verilmez.

Android P hedefleyen uygulamalar için davranış değişiklikleri hakkında daha fazla bilgi için bkz: [davranış değişiklikleri](https://developer.android.com/preview/behavior-changes.html#p-apps).


## <a name="sample-code"></a>Örnek kod

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) nasıl ayarlanacağı gösterilmektedir Android P kesme modundan görüntülemek için Xamarin.Android örnek bir uygulama olan yeni kullanma `Person` sınıfı ve görüntü içeren bir bildirim gönderme.


## <a name="summary"></a>Özet

Bu makalede, Android P önizlemenin kullanıma sunulmasından ve yükleme ve en son araçları ve Xamarin.Android geliştirme paketleri Android P önizlemesi ile yapılandırma adımları açıklanmıştır. Android P adresinde kullanılabilir anahtar özelliklerine genel bakış için bu özelliklerin birkaç örnek kaynak kodda ile sağlanan. API belgelerine bağlantılar dahil ve uygulamalar için Android P. oluşturmada yardımcı olması için Android Geliştirici konuları kullanmaya başlayın Ayrıca, mevcut uygulamaları etkileyebilir en önemli Android P davranış değişiklikleri vurgulanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android P Geliştirici önizlemesi](https://developer.android.com/preview/)
