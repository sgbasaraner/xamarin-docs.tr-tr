---
title: "Marshmallow özellikleri"
description: "Bu makale size yardımcı olur Android 6.0 Marshmallow için uygulama geliştirmek için Xamarin.Android kullanarak kullanmaya başlayın."
ms.topic: article
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: b28ca68701394a8b7b0b543a5ae646910e7c8361
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="marshmallow-features"></a>Marshmallow özellikleri

_Bu makale size yardımcı olur Android 6.0 Marshmallow için uygulama geliştirmek için Xamarin.Android kullanarak kullanmaya başlayın._

Bu makalede Android 6.0 Marshmallow'deki yeni özelliklerin bir özetini sağlar, Android Marshmallow geliştirme için Xamarin.Android için nasıl hazırlanılacağını açıklamaktadır ve yeni Android Marshmallow kullanmak nasıl çalışılacağını örnek uygulamalar için bağlantılar sağlar Xamarin.Android uygulamaları özellikleri. 

<a name="overview" />

## <a name="overview"></a>Genel Bakış

[Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html), sonraki ana Android olan Android Lolipop sonra sürüm.
Xamarin.Android Android Marshmallow destekler ve içerir:

-   **API 23/Android 6.0 bağlamaları** &ndash; Android 6.0 aşağıda açıklanan yeni özellikleri için birçok yeni API'leri ekler; API düzeyi 23 hedeflediğinizde bu API'leri Xamarin.Android uygulamaları için kullanılabilir. Android 6.0 API'lerini hakkında daha fazla bilgi için bkz: [Android 6.0 API'lerini](http://developer.android.com/preview/api-overview.html). 

[![Kahramanı görüntülerini tabletler ve telefonlardan Marshmallow çalıştırma](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png)

Marshmallow yayın çoğunlukla "Lehçe ve kalite" odaklanan rağmen Xamarin.Android geliştiricileri için de ilgi birçok yeni özellik sağlar. Bu özellikler şunlardır: 

-   **Çalışma zamanı izinleri** &ndash; bu geliştirme, kullanıcıların çalışma zamanında olay temelinde güvenlik izinleri onaylamak mümkün kılar. 

-   **Kimlik doğrulaması geliştirmeleri** &ndash; Android Marshmallow ile başlayarak, uygulamalar artık parmak izi algılayıcılar kullanıcıları ve yeni bir kimliğini doğrulamak için kullanabileceğiniz *kimlik bilgisi onaylayın* özelliği, girmek için gereken en aza indirir parolalar. 

-   **Uygulama bağlama** &ndash; sahip olmanın gerekliliğini ortadan kaldırmak için bu özelliği yardımcı olur **uygulama Seçici** otomatik olarak uygulamaları ile web etki alanlarına ilişkilendirerek açılır. 

-   **Paylaşımı doğrudan** &ndash; tanımlayabileceğiniz *doğrudan paylaşımı hedefleri* olduğundan olun paylaşımı hızlı ve kolay anlaşılır kullanıcılar için; bu özellik, diğer uygulamalarla içeriği paylaşmak uers sağlar. 

-   **Sesli etkileşimleri** &ndash; bu yeni API uygulamanıza konuşma sesli özelliklerini oluşturmanıza olanak sağlar. 

-   **4 K görüntü modu** &ndash; içinde Android Marshmallow uygulamanızı isteyebileceği 4 K onu destekleyen donanım üzerinde çözümleme görüntüler. 

-   **Yeni ses özellikleri** &ndash; Marshmallow ile başlayarak, Android şimdi MIDI protokolünü destekler. Ayrıca, dijital ses yakalama ve kayıttan yürütme nesneleri oluşturmak için yeni sınıflar sağlar ve ses ve giriş aygıtları ilişkilendirme yeni API kancaları sunar. 

-   **Yeni Video Özellikleri** &ndash; Marshmallow sağlar uygulamaları yardımcı olan yeni bir sınıf ses ve video akışları eşitlenmiş işlemek; Bu sınıf için dinamik yürütme hızını de destek sağlar. 

-   **İş için Android** &ndash; Marshmallow şirketin, tek kullanıcılı cihazlar için Gelişmiş denetimleri içerir. Sessiz yüklemeyi destekler ve cihaz sahibi tarafından uygulamalar, sistem güncelleştirmeleri, geliştirilmiş sertifika yönetimi, verileri kullanım izleme, izinleri yönetimi ve iş durumu bildirimleri otomatik kabul kaldırmak. 

-   **Malzeme tasarım destek kitaplığı** &ndash; yeni *tasarım destek kitaplığı* tasarım bileşenleri ve uygulamanıza malzeme Tasarım görünümüne oluşturmanızı kolaylaştırır modeller sağlar. 

Ayrıca, çok sayıda çekirdek Android kitaplığı güncelleştirme ile Android M yayımlanan ve bu güncelleştirmeleri Android M ve önceki sürümleri, Android için yeni özellikler sağlar.

Ayrıca, çok sayıda çekirdek Android kitaplığı güncelleştirme ile Android Marshmallow yayımlanan ve bu güncelleştirmeleri Android Marshmallow ve önceki sürümleri, Android için yeni özellikler sağlar. Bu makalede, Android Marshmallow uygulamalarla oluşturmaya başlamak açıklar ve yeni özelliğine genel bakış Android 6. 0'vurgular sağlar. 


<a name="requirements" />

## <a name="requirements"></a>Gereksinimler

Aşağıdaki Xamarin tabanlı uygulamalarda yeni Android Marshmallow özellikleri kullanmak için gereklidir: 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 or later must be installed and configured with either Visual Studio or Xamarin Studio.

-   **Mac için Visual Studio** veya **Visual Studio** &ndash; Visual Studio sürümü 5.9.7.22 Mac için kullanıyorsanız veya sonraki sürümü gereklidir. Visual Studio, sürüm 3.11.1537 kullanıyorsanız ya da daha sonra Visual Studio için Xamarin araçları gereklidir. 

-   **Android SDK** &ndash; Android SDK 6.0 (API 23) veya sonrası Android SDK Yöneticisi aracılığıyla yüklü olmalıdır.

-   **Java Geliştirme Seti** &ndash; Xamarin.Android gerektirir [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya API düzeyi 24 geliştiriyorsanız sonraki ya da daha büyük (JDK 1.8 de destekler API düzeylerini Marshmallow dahil olmak üzere 24'den önceki). Özel denetimler ya da formları genele gitmeyi kullanıyorsanız JDK 1.8 64-bit sürümü gereklidir.

Kullanmaya devam edebilirsiniz [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özellikle geliştirme veya önceki bir sürümünü kullanıyorsanız. 

<a name="gettingstarted" />

## <a name="getting-started"></a>Başlarken

Android Marshmallow Xamarin.Android ile kullanmaya başlamak için indirin ve Android Marshmallow proje oluşturmadan önce SDK paketlerini ve en son araçları yüklemeniz gerekir: 

1.  En son Xamarin güncelleştirmeleri indirip yükleyebilirsiniz **kararlı** kanal. 

2.  Android 6.0 Marshmallow SDK paketleri ve araçlarını yükler.

3.  Yeni bir Xamarin.Android projesi hedefleyen Android 6.0 Marshmallow (API düzeyi 23) oluşturun. 

4.  Bir öykünücü veya cihaz Android Marshmallow için yapılandırın.

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır:

<a name="updates" />

### <a name="install-xamarin-updates"></a>Xamarin güncelleştirmeleri yükle

Xamarin Android 6.0 Marshmallow için desteği içeren güncelleştirmek için güncelleştirme kanala değiştirme **kararlı** ve tüm güncelleştirmeleri yükleyin. Güncelleştirmeleri kanaldan güncelleştirmeleri yükleme hakkında daha fazla bilgi için bkz: [güncelleştirmeleri kanalı değiştirmek](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/). 

<a name="sdkpreview" />

### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK'sını yükleyin

Android Marshmallow için bir Xamarin.Android projesi oluşturmak için önce Android 6.0 SDK'sını yüklemek için Android SDK Yöneticisi'ni kullanmanız gerekir:

-   Android SDK Yöneticisi'ni başlatın (Mac için Visual Studio'da kullanın **Araçlar > SDK Manager**; Visual Studio'da kullanın **Araçlar > Android > Android SDK Manager**) ve en son Android SDK araçlarını yükleyin:

    [![Android SDK Yöneticisi'nde Android SDK Araçları seçme](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png)

-   Ayrıca, en son yükleme **Android 6.0** SDK paketler:

    [![Android 6.0 SDK paketleri Android SDK Yöneticisi'nde seçme](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png)

Android SDK Araçları Düzeltme 24.3.4 yüklemelisiniz veya sonraki bir sürümü.
Android 6.0 SDK'sını yüklemek için Android SDK Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz: [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html).


<a name="xaproject" />

### <a name="start-a-xamarinandroid-project"></a>Start a Xamarin.Android Project

Yeni bir Xamarin.Android projesi oluşturun. Xamarin Android geliştirme yeniyseniz, bkz: [Hello, Android](~/android/get-started/hello-android/index.md) Android projeleri oluşturma hakkında bilgi edinmek için. 

Bir Android projesi oluşturduğunuzda, hedef Android 6.0 MarshMallow için sürüm ayarlarını yapılandırmanız gerekir. Projeniz için Marshmallow hedeflemek için projeniz için yapılandırma **API düzeyi 23 (Xamarin.Android v6.0 desteği)**. Android API düzeyi düzeyleri yapılandırma hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).


<a name="emudev" />

### <a name="configure-an-emulator-or-device"></a>Bir öykünücü veya cihaz yapılandırma

Bir öykünücü kullanıyorsanız, Android AVD Yöneticisi'ni başlatın ve aşağıdaki ayarları kullanarak yeni bir cihaz oluşturma:

-   Aygıt: Nexus 5, 6 veya 9.
-   Hedef: Android 6.0 - API düzeyi 23
-   ABI: x86

Örneğin, bu sanal aygıt Nexus 5 benzetmek için yapılandırılır:

[![Nexus 5 cihaz, Android 6.0 hedef ve Intel Atom (x86) kullanarak bir AVD yapılandırma](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png)

Nexus 5 gibi fiziksel bir aygıtı kullanıyorsanız, 6 veya 9, Android Marshmallow önizleme görüntüsünü yükleyebilirsiniz. Android Marshmallow için Cihazınızı güncelleştirme hakkında daha fazla bilgi için bkz: [donanım sistemi görüntüleri](http://developer.android.com/preview/download.html#images).


<a name="newfeatures" />

## <a name="new-features"></a>Yeni Özellikler

Android Marshmallow içinde sunulan değişiklikler çoğunu Android kullanıcı deneyimini iyileştirme, performansı artırma ve hataları düzelttikten odaklanır. Ancak, Marshmallow de Android platformu temelleri kapsamlı bazı değişiklikler sunmuştur. Aşağıdaki bölümlerde bu geliştirmeler vurgulayın ve uygulamanızda yeni Android Marshmallow özellikleri kullanarak başlamanıza yardımcı olmak için bağlantılar sağlar. 


<a name="permissions" />

### <a name="runtime-permissions"></a>Çalışma zamanı izinleri

Android izinleri sistem önemli ölçüde en iyi duruma getirilmiş ve Android Lolipop bu yana basitleştirilmiştir. Android Marshmallow içinde kullanıcılara izinler çalışma zamanında yerine olay temelinde yükleme saati vermiş olursunuz. Android Marshmallow ve daha sonra bu özelliği desteklemek için izinler (bağlamında, burada izinler gerekir) çalışma zamanında kullanıcıdan istemek için uygulamanızı tasarlayın. Bu değişiklik, kullanıcıların uygulamanızı kullanmaya başlamak kolaylaştırır çünkü hemen uygulamanızı yükseltme ve yükleme işlemini kolaylaştırır. 

Bkz: [Android Marshmallow çalışma zamanı izinleri isteyen](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) çalışma zamanı izinleri Xamarin.Android uygulamalarda uygulama hakkında daha fazla ayrıntı (kod örnekleri dahil) için.
Xamarin Android Marshmallow (ve daha sonra) çalışma zamanı izinlerin nasıl çalıştığını gösteren örnek bir uygulama da sağlar: [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Bu örnek uygulama şunlar gösterilmektedir:

-   Nasıl denetleneceği ve çalışma zamanında izin isteyin.
-   Android M aygıtları izinlerini bildirmeyi.

Bu örnek uygulamayı kullanmak için:

1.  Dokunun **kamera** veya **kişiler** bir izinleri görüntülemek için düğmeler isteği iletişim.
2.  Kamera veya kişiler parçaları görüntülemesine izin verin.

Android Marshmallow yeni çalışma zamanı izinleri özellikleri hakkında daha fazla bilgi için bkz: [sistem izinleriyle çalışma](https://developer.android.com/preview/features/runtime-permissions.html).


<a name="authentication" />

### <a name="authentication-enhancements"></a>Kimlik doğrulaması geliştirmeleri

Android Marshmallow parolaları gereksinimini ortadan kaldırabilirsiniz iki kimlik doğrulaması geliştirmeleri içerir:

-   **Kimlik doğrulama için parmak izi** &ndash; kullanıcıların kimliklerini doğrulamak için bir parmak izi tarama kullanır.

-   **Kimlik bilgisi onaylayın** &ndash; ne kadar süreyle aygıt kilitlendi üzerinde temel kullanıcıların kimliğini doğrular.

Sonraki bölümde açıklandığı örnek uygulamaları ve bağlantıları, hale bilinen bu yeni özellikleri ile yardımcı olabilir.


<a name="fingerprint" />

#### <a name="fingerprint-authentication"></a>Parmak izi kimlik doğrulaması

Donanım tarama parmak izi destekleyen cihazlara, yeni kullanabilirsiniz `FingerPrintManager` bir kullanıcının kimliğini doğrulamak için sınıf.
Android Marshmallow için parmak izi kimlik doğrulama özelliği hakkında daha fazla bilgi için bkz: [parmak izi kimlik doğrulama](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin kayıtlı parmak izi, uygulamanızda bir kullanıcının kimliğini doğrulamak için nasıl kullanılacağını gösteren örnek bir uygulama sağlar: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

Bu örnek uygulamayı kullanmak için:

1.  Touch **satın alma** parmak izi kimlik doğrulama iletişim kutusunu açmak için düğmesi.
2.  Kimlik doğrulaması için kayıtlı parmak içinde tarayın.

Bu örnek uygulama parmak izi okuyucu aygıtla gerektirdiğini unutmayın.
Bu uygulama, parmak izi (veya parolanızı) depolamaz.


<a name="voice" />

#### <a name="voice-interactions"></a>Sesli etkileşimleri

Android Marshmallow içinde sunulan yeni ses etkileşim özelliği, kullanıcıların, uygulamanızın eylemleri onayla ve seçenekleri listesinden seçmek için kendi sesli kullanmasına izin verir. Sesli etkileşimleri hakkında daha fazla bilgi için bkz: [sesli etkileşim API genel bakış](https://developers.google.com/voice-actions/interaction/). 

Bkz: [Android uygulamanızı sesli etkileşimleri bir konuşma eklemek](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) sesli etkileşimleri Xamarin.Android uygulamalarda uygulama hakkında daha fazla ayrıntı (kod örnekleri dahil) için.
Örnek bir uygulama kullanılabilir bir Xamarin.Android uygulaması sesli etkileşim API kullanma gösterilmektedir: [sesli etkileşimleri](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).


<a name="confirmcred" />

#### <a name="confirm-credential"></a>Kimlik bilgisi onaylayın

Kullanarak yeni *kimlik bilgisi onaylayın* özellik Android Marshmallow unutmayın ve bunları ne kadar süreyle cihazlarını kilitlendi üzerinde temel kimlik doğrulaması uygulamaya özgü parolaları girin gerek kalmadan kullanıcıların boşaltabilirsiniz.
Bunu yapmak için yeni kullandığınız `SetUserAuthenticationValidityDurationSeconds` yöntemi `KeyGenerator`. Kullanım `KeyGuardManager`'s `CreateConfirmDeviceCredentialIntent` , uygulamanızın içinde kullanıcının yeniden kimlik doğrulama yöntemi. Android Marshmallow'deki bu yeni özellik hakkında daha fazla bilgi için bkz: [onaylayın kimlik bilgisi](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin uygulamanızda aygıt kimlik bilgisi (örneğin, PIN, desen veya parola) kullanmayı gösteren örnek bir uygulama sağlar: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

Bu örnek uygulamayı kullanmak için:

1.  Cihazınızı güvenli kilit ekranında kurulumu (**güvenli > Güvenlik > Screenlock**).
2.  Dokunun **satın alma** düğmesini tıklatın ve güvenli kilit ekranı kimlik bilgilerini doğrulayın.


<a name="chrometabs" />

### <a name="chrome-custom-tabs"></a>Chrome özel sekmeler

Uygulama geliştiriciler yönde seçim bir kullanıcı bir URL dokunur zaman: uygulama bir tarayıcıyı başlatın veya temel bir uygulama tarayıcı kullanabilirsiniz bir `WebView`. Her iki seçenek zorluklara &ndash; tarayıcı başlatma sırasında özelleştirilebilir değil ağır içerik anahtarı olan `WebView`s tarayıcı ile durumu paylaşmıyor. Ayrıca, kullanımı `WebView`s ek bakım yükü ekleyebilirsiniz. 

*Chrome özel sekmeler* uygulamanızı bırakın, kullanıcılarınızın gerek kalmadan Chrome gücünü ile Web siteleri kolayca ve zarif bir şekilde görüntülemenize olanak sağlar. Bu özellik, uygulama kullanıcının web deneyimi üzerinde daha fazla denetim sağlar; Yerel arasında geçişler kolaylaştırır ve web içeriği daha sorunsuz şekilde çözümlemelere zorunda kalmadan bir `WebView`. Uygulamanızı ayrıca Chrome nasıl göründüğünü ve aşağıdaki özelleştirerek hissi etkileyebilir: 

-   Araç çubuğu rengi

-   Animasyon çıkmak ve girin

-   Chrome araç çubuğu ve taşma menüsü özel eylemler

-   Chrome öncesi başlangıç ve içeriği (Hızlı yükleme için) önceden getirme

Xamarin.Android uygulamanız bu özelliğin avantajlarından yararlanmak için karşıdan yükleyip kurun [Android destek özel sekmeler Kitaplığı](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Bu özellik hakkında daha fazla bilgi için bkz: [Chrome özel sekmeler](https://developer.chrome.com/multidevice/android/customtabs).


<a name="designlib" />

### <a name="material-design-support-library"></a>Malzeme tasarım destek kitaplığı

Sunulan android Lolipop [malzeme tasarım](http://www.google.com/design/spec/material-design/introduction.html) Android deneyimi yenilemek için yeni bir tasarım dili olarak (bkz [malzeme tema](~/android/user-interface/material-theme.md) malzeme tasarım Xamarin.Android uygulamalarında kullanma hakkında bilgi için). Google Android Marshmallow ile sunulan *Android tasarım destek kitaplığı* uygulamayı kolaylaştırmak için Görünüm geliştiriciler malzeme benimsemek için tasarım. Bu kitaplığı aşağıdaki bileşenleri içerir:

-   **CoordinatorLayout** &ndash; yeni `CoordinatorLayout` pencere öğesi benzerdir, ancak daha güçlü bir `FrameLayout`. Kullanabilirsiniz `CoordinatorLayout` alt görünümleri için bir kapsayıcı veya bir üst düzey düzeni ve olarak sağlayan bir `layout_anchor` bağlantı görünümleri diğer görünümlerle göre için kullanılan özniteliği.

-   **Araç çubukları daraltma** &ndash; yeni `CollapsingToolbarLayout` için sarmalayıcı olan çöken bir uygulamayı çubuğu `Toolbar`. (Unutmayın *uygulama çubuğunu* ne önceden olarak adlandırılan olan *Eylem çubuğu*.)

-   **Eylem düğmesi kayan** &ndash; uygulamanızın arabirimde birincil eylem gösterir yuvarlak bir düğme.

-   **Etiketleri için metin düzenleme kayan** &ndash; yeni kullanan `TextInputLayout` pencere öğesi (hangi sarmalayan `EditText`) kullanıcı metin bastığında ipucu gizli olduğunda bir kayan etiketi göstermek için.

-   **Gezinti görünümü** &ndash; yeni `NavigationView` pencere öğesi Gezinti çekmecesini gitmek kullanıcılar için daha kolay bir şekilde kullanmanıza yardımcı olur.

-   **Snackbar** &ndash; yeni `SnackBar` pencere öğesi yukarıdaki tüm diğer öğeler ekranda görünen ekranın altındaki kısa bir ileti görüntüler basit geri bildirim mekanizması (bir bildirim benzer) olduğunu.

-   **Malzeme sekmeleri** &ndash; yeni `TabLayout` pencere öğesi, en üst düzey Gezinti uygulamanızda uygulamak için yol olarak sekmeler görüntüleme için yatay düzen sağlar.

Yararlanmak için [tasarım destek kitaplığı](http://developer.android.com/tools/support-library/features.html#design) Xamarin.Android uygulamanıza yükleyip Xamarin [Xamarin destek kitaplığı tasarımı](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet paketi.

Bkz: [Android destek tasarım kitaplığı güzel malzeme tasarımıyla](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) Xamarin.Android uygulamaları malzeme tasarım destek kitaplıkta kullanma hakkında daha fazla ayrıntı (kod örnekleri dahil) için.
Xamarin Xamarin.Android yeni Android tasarım kitaplıkta demos örnek bir uygulama sağlar &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
Bu örnek tasarım kitaplığı aşağıdaki özellikleri gösterir:


-   Araç çubuğu daraltma
-   Kayan eylem düğmesi
-   Görünüm sabitleme
-   NavigationView
-   Snackbar

Tasarım Kitaplığı hakkında daha fazla bilgi için bkz: [Android tasarım destek kitaplığı](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) Android Geliştirici günlüğündeki.


<a name="libraries" />

### <a name="additional-library-updates"></a>Ek Kitaplığı güncelleştirmeleri

Android Marshmallow ek olarak, Google birkaç çekirdek Android kitaplıklarına ilgili güncelleştirmeler bildirme. Xamarin birkaç Önizleme sürümü NuGet paketleri yoluyla bu güncelleştirmeleri Xamarin.Android destek sağlar: 

-   [Google Play Hizmetleri](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; Google Play Hizmetleri'nin en son sürümünü içeren yeni *uygulama başvurulmasını* özelliği, kullanıcıların kendi uygulama arkadaşlarınızla paylaşmak mümkün kılar. Bu özellik hakkında daha fazla bilgi için bkz: [uygulama Google başvurulmasını genişletin bilgisayarınızı uygulamanın Menziliniz](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Android desteği kitaplıkları](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; bu NuGets yalnızca geriye dönük olarak uyumlu Android framework API sürümleri sağlarken kitaplığı API'leri için kullanılabilen özellikleri sunar. 

-   [Android Wearable Kitaplığı](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; bu NuGet Google Play Hizmetleri'nin bağlamaları içerir. Wearable kitaplığının en son sürümü yeni özellikler (özel uygulamalar için daha kolay gezinme dahil) Android takmak platformuna getirir. 


## <a name="summary"></a>Özet

Bu makalede, Android Marshmallow tanıtılan ve yüklemek ve en yeni araç ve xamarin Android geliştirme için paketler üzerinde Marshmallow yapılandırmak nasıl açıklanmıştır. Xamarin Android geliştirme için de en heyecan verici yeni Android Marshmallow özelliklerine genel bakış sağlanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK'sı Al](https://developer.android.com/sdk/index.html#Other)
- [Özelliklere genel bakış](https://developer.android.com/preview/api-overview.html)
- [Sürüm Notları](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (örnek)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (örnek)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (örnek)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
