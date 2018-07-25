---
title: Marshmallow özellikleri
description: Bu makalede, yardımcı Android 6.0 Marshmallow için uygulamalar geliştirmek için Xamarin.Android kullanarak kullanmaya başlayın.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 4f0bcd25662d3def719a89ccf833e845eb1728f2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242244"
---
# <a name="marshmallow-features"></a>Marshmallow özellikleri

_Bu makalede, yardımcı Android 6.0 Marshmallow için uygulamalar geliştirmek için Xamarin.Android kullanarak kullanmaya başlayın._

Bu makalede, Android 6.0 Marshmallow'daki yeni özelliklerin bir özetini sağlar, Xamarin.Android, Android Marshmallow geliştirme için hazırla açıklanmaktadır ve yeni Android Marshmallow kullanmak nasıl gösteren örnek uygulamalar için bağlantılar sağlar Xamarin.Android uygulamalarında özellikleri. 


## <a name="overview"></a>Genel Bakış

[Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html), sonraki önemli Android olan Android Lollipop sonra yayın.
Xamarin.Android, Android Marshmallow destekler ve içerir:

-   **API 23/Android 6.0 bağlamaları** &ndash; Android 6.0, çok sayıda yeni API için aşağıda açıklanan yeni özellikleri ekler; bu API, API düzeyi 23 hedeflediğinizde Xamarin.Android uygulamaları için kullanılabilir. Android 6.0 API'leri hakkında daha fazla bilgi için bkz. [Android 6.0 API'lerini](http://developer.android.com/preview/api-overview.html). 

[![Tablet ve telefonlarda Marshmallow çalışan Hero görüntülerini](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Marshmallow sürüm esas olarak "Lehçe ve kalite" odaklanan olsa da, Xamarin.Android geliştiricilerine ayrıca birçok yeni ilgi çekici özellik sağlar. Bu özellikler şunlardır: 

-   **Çalışma zamanı izinleri** &ndash; bu geliştirme, kullanıcıların çalışma zamanında bir olay temelinde güvenlik izinleri onaylamasını sağlar. 

-   **Kimlik doğrulaması geliştirmeleri** &ndash; Android Marshmallow ile başlayarak, uygulamaları artık parmak izi sensörlerden kullanıcılar ve yeni bir kimlik doğrulaması için kullanabilirsiniz *kimlik bilgisi onaylayın* özellik girme gereksinimini en aza indirir parola. 

-   **Uygulama bağlama** &ndash; sahip olma gereksinimini ortadan kaldırmak için bu özelliği yardımcı **uygulama Seçici** otomatik olarak uygulamalar web etki alanları ile ilişkilendirerek açılır penceresi. 

-   **Paylaşımı doğrudan** &ndash; tanımlayabileceğiniz *doğrudan paylaşım hedefleri* oluşturan paylaşımı hızlı ve kullanımı kolay kullanıcılar için; bu özellik, diğer uygulamalar ile içeriği paylaşmak kullanıcılarının sağlar. 

-   **Sesli etkileşimler** &ndash; bu yeni API uygulamanıza etkileşimli sesli özelliklerini oluşturmanıza olanak tanır. 

-   **4 K görüntü modu** &ndash; , Android Marshmallow için uygulamanızı isteyebilir 4 K görüntü çözünürlüğünde onu destekleyen donanım üzerinde. 

-   **Yeni ses özellikleri** &ndash; Marshmallow ile başlayarak, Android artık MIDI protokolünü destekler. Ayrıca, dijital ses yakalama ve kayıttan yürütme nesneleri oluşturmak için yeni sınıflar sağlar ve ses ve giriş cihazlarının ilişkilendirme için yeni API kancaları sunar. 

-   **Yeni görüntü özelliklerini** &ndash; Marshmallow uygulamaları yardımcı olan yeni bir sınıf ses ve video akışları eşitlenmiş işleme sağlar; bu sınıfın dinamik kayıttan yürütme hızı için de destek sağlar. 

-   **İş için Android** &ndash; Marshmallow şirkete, tek kullanıcılı cihazları için Gelişmiş denetimleri içerir. Sessiz yükleme destekler ve cihaz sahibi tarafından uygulamalar, sistem güncelleştirmeleri, gelişmiş sertifika yönetimi, veri kullanımı izleme, yönetim izinleri ve iş durumu bildirimlerini otomatik kabul kaldırın. 

-   **Malzeme tasarımı destek kitaplığı** &ndash; yeni *tasarım destek kitaplığı* tasarım bileşenleri ve malzeme Tasarım görünümüne uygulamanıza kolaylaştıran modeller sağlar. 

Ayrıca, çok sayıda çekirdek Android kitaplığı, güncelleştirmeleri Android M ile yayımlanan ve bu güncelleştirmeleri Android M ve önceki Android sürümleri için yeni özellikler sağlar.

Ayrıca, çok sayıda çekirdek Android kitaplığı, güncelleştirmeleri Android Marshmallow ile yayımlanan ve bu güncelleştirmeler hem Android Marshmallow hem de önceki Android sürümleri için yeni özellikler sağlar. Bu makalede, Android Marshmallow ile uygulama oluşturmaya başlamak açıklanır ve yeni özelliğine genel bakış Android 6. 0'vurgular sağlar. 

## <a name="requirements"></a>Gereksinimler

Aşağıdaki yeni Android Marshmallow özelliklerini Xamarin tabanlı uygulamalarında kullanmak için gereklidir: 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 veya daha sonra yüklenmeli ve Visual Studio veya Xamarin Studio ile yapılandırılmış.

-   **Mac için Visual Studio** veya **Visual Studio** &ndash; sürüm 5.9.7.22 Mac için Visual Studio'ya veya üstü gereklidir. Visual Studio, sürüm 3.11.1537 kullanıyorsanız veya, daha sonra Visual Studio için Xamarin araçları gereklidir. 

-   **Android SDK'sı** &ndash; Android SDK'sı (API 23) 6.0 veya daha sonra Android SDK Yöneticisi aracılığıyla yüklenmesi gerekir.

-   **Java Developer Kit** &ndash; Xamarin.Android gerektirir [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya API düzeyi için 24 geliştiriyorsanız sonraki ya da daha büyük (JDK 1.8 de destekler API düzeylerini Marshmallow dahil olmak üzere 24, daha önce). Özel denetimleri veya Forms Önizleyiciyi kullanıyorsanız, 64 bit JDK 1.8 sürümü gereklidir.

Kullanmaya devam edebilirsiniz [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özel olarak geliştirme veya önceki bir sürümü kullanıyorsanız. 


## <a name="getting-started"></a>Başlarken

Android Marshmallow Xamarin.Android ile kullanmaya başlamak için indirin ve bir Android Marshmallow proje oluşturmadan önce en son araç ve SDK paketlerini yükleyin: 

1.  En yeni Xamarin güncelleştirmelerinden yükleme **kararlı** kanal. 

2.  Android 6.0 Marshmallow SDK paketleri ve araçlarını yükleyin.

3.  Android 6.0 Marshmallow (API düzeyi 23) hedefleyen yeni bir Xamarin.Android projesi oluşturun. 

4.  Bir öykünücü veya cihazın Android Marshmallow için yapılandırın.

Bu adımların her biri, aşağıdaki bölümlerde açıklanmıştır:


### <a name="install-xamarin-updates"></a>Xamarin güncelleştirmeleri yükle

Xamarin Android 6.0 Marshmallow için destek içerir, böylece güncelleştirmek için güncelleştirme kanala değiştirmek **kararlı** ve tüm güncelleştirmeleri yükleyin. Güncelleştirmeleri kanaldan güncelleştirmeleri yükleme hakkında daha fazla bilgi için bkz. [güncelleştirmeleri kanal](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel). 


### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK'sını yükleyin

Android Marshmallow için bir Xamarin.Android projesi oluşturmak için önce Android 6.0 SDK'yı yüklemek için Android SDK Yöneticisi'ni kullanmanız gerekir:

-   Android SDK Yöneticisi'ni başlatın (Mac için Visual Studio'da kullanmak **Araçlar > SDK Yöneticisi**; Visual Studio'da kullanmak **Araçlar > Android > Android SDK Yöneticisi**) ve en son Android SDK Tools yükleyin:

    [![Android SDK Araçları Android SDK Yöneticisi'nde seçme](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   Ayrıca, en son yükleme **Android 6.0** SDK paketleri:

    [![Android SDK Yöneticisi'nde Android 6.0 SDK paketlerini seçme](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Android SDK Tools düzeltme 24.3.4 yüklemeniz gerekir ya da daha sonra.
Android 6.0 SDK'yı yüklemek için Android SDK Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz. [SDK Yöneticisi](http://developer.android.com/tools/help/sdk-manager.html).



### <a name="start-a-xamarinandroid-project"></a>Bir Xamarin.Android projesi Başlat

Yeni bir Xamarin.Android projesi oluşturun. Xamarin ile Android geliştirmesi için yeni başlıyorsanız bkz [Hello, Android](~/android/get-started/hello-android/index.md) Android projeleri oluşturma hakkında bilgi edinmek için. 

Bir Android projesi oluşturduğunuzda, hedef Android 6.0 MarshMallow sürüm ayarlarını yapılandırmanız gerekir. Projeniz için Marshmallow hedeflemek için projeniz için yapılandırma **API düzeyi 23 içindir (Xamarin.Android v6.0 desteği)**. Android API düzeyi düzeylerini yapılandırma hakkında daha fazla bilgi için bkz. [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md).



### <a name="configure-an-emulator-or-device"></a>Bir öykünücü veya cihaz yapılandırma

Bir öykünücüsü kullanıyorsanız, Android AVD Yöneticisi'ni başlatın ve aşağıdaki ayarları kullanarak yeni bir cihaz oluşturun:

-   Cihaz: Nexus 5, 6 veya 9.
-   Hedef: Android 6.0 - API düzeyi 23 içindir
-   ABI: x86

Örneğin, bu sanal cihazı, Nexus 5 benzetmek için yapılandırılır:

[![Bir AVD nexus 5 cihaz, Android 6.0 hedef ve Intel Atom (x86) kullanarak yapılandırma](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Nexus 5 gibi fiziksel bir cihaz kullanıyorsanız, 6 veya 9, Android Marshmallow önizleme görüntüsü yükleyebilirsiniz. Android Marshmallow için Cihazınızı güncelleştirme hakkında daha fazla bilgi için bkz. [donanım sistem görüntüleri](http://developer.android.com/preview/download.html#images).



## <a name="new-features"></a>Yeni Özellikler

Android kullanıcı deneyimini geliştiriyor, performansı artırmak ve hataları düzeltiyor Android Marshmallow değişikliklerin birçoğu odaklanır. Ancak, Marshmallow de Android platformu temelleri için kapsamlı bazı değişiklikler getirmiştir. Aşağıdaki bölümlerde, bu geliştirmeler vurgulayın ve uygulamanızda yeni Android Marshmallow özelliklerini kullanarak başlamanıza yardımcı olmak için bağlantılar sağlar. 



### <a name="runtime-permissions"></a>Çalışma zamanı izinleri

Android izinlerini sistem önemli ölçüde en iyi duruma getirilmiş ve bu yana Android Lollipop Basitleştirilmiş. Android Marshmallow içinde kullanıcıların izinleri bir zamanında yerine olay bağlamında yükleme saati verin. Bu özellik Android Marshmallow ve sonraki sürümlerde desteklemek için izinler çalışma zamanında (izinleri gerekmesi bağlamı) kullanıcıdan istemek için uygulamanızı tasarlayın. Bu değişiklik, kullanıcıların uygulamanızı kullanmaya başlamak kolaylaştırır çünkü hemen, yükleme ve uygulamanızı yükseltme sürecini kolaylaştırır. 

Bkz: [çalışma zamanı izinleri isteyen Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) çalışma zamanı izinleri Xamarin.Android uygulamalarında uygulama hakkında daha fazla ayrıntı (kod örnekleri dahil) için.
Xamarin Android Marshmallow (ve üzeri) çalışma zamanı izinleri nasıl çalıştığı gösterilmektedir. örnek bir uygulama da sağlar: [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Bu örnek uygulama aşağıdaki gösterir:

-   Nasıl kontrol edin ve çalışma zamanında izinlere ilişkin istek.
-   Android M cihazlar için izinleri nasıl.

Bu örnek uygulamayı kullanmak için:

1.  Dokunun **kamera** veya **kişiler** bir izinleri görüntülemek için düğmeler isteği iletişim.
2.  Kamera veya kişiler parçaları görüntülemesine izin verin.

Android Marshmallow yeni çalışma zamanı izinleri özellikler hakkında daha fazla bilgi için bkz. [sistem izinleriyle birlikte çalışma](https://developer.android.com/preview/features/runtime-permissions.html).



### <a name="authentication-enhancements"></a>Kimlik doğrulaması geliştirmeleri

Android Marshmallow parola gerekmemesi yardımcı olan iki kimlik doğrulaması geliştirmeleri içerir:

-   **Parmak izi kimlik doğrulaması** &ndash; parmak izini tarama, kullanıcıların kimliklerini doğrulamak için kullanır.

-   **Kimlik bilgisi onaylayın** &ndash; ne kadar süreyle cihaz kilitlendi üzerinde göre kullanıcıların kimliğini doğrular.

Sonraki bölümde açıklandığı örnek uygulamaları ve bağlantıları olur, bilinen bu yeni özellikler ile yardımcı olabilir.


#### <a name="fingerprint-authentication"></a>Parmak izi kimlik doğrulaması

Parmak izi donanım tarama destekleyen cihazlarda, yeni kullanabilirsiniz `FingerPrintManager` bir kullanıcının kimliğini doğrulamak için sınıf.
Android Marshmallow için parmak izi kimlik doğrulaması özelliği hakkında daha fazla bilgi için bkz: [parmak izi kimlik doğrulaması](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin uygulamanızda bir kullanıcının kimliğini doğrulamak için kayıtlı parmak izi kullanmayı gösteren örnek bir uygulama sağlar: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

Bu örnek uygulamayı kullanmak için:

1.  Touch **satın alma** parmak izi kimlik doğrulaması iletişim kutusunu açmak için düğmeyi.
2.  Kimlik doğrulaması yapmak için kayıtlı parmak tarayın.

Bu örnek uygulama bir cihazla parmak izi okuyucu gerektiğini unutmayın.
Bu uygulama, parmak izi (veya parolanızı) depolamaz.



#### <a name="voice-interactions"></a>Sesli etkileşimler

Uygulamanızdaki kullanıcıların kendi ses eylemleri onaylamak ve seçeneklerini listeden seçmek için kullanılacak Android Marshmallow içinde sunulan yeni sesli etkileşimlerin özelliği sağlar. Sesli etkileşimler hakkında daha fazla bilgi için bkz. [sesli etkileşim API bakış](https://developers.google.com/voice-actions/interaction/). 

Bkz: [Android uygulamanıza sesli etkileşimlerin ile bir konuşma ekleyin](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) sesli etkileşimlerin Xamarin.Android uygulamalarında uygulama hakkında daha fazla ayrıntı (kod örnekleri dahil) için.
Örnek uygulama tarafından kullanılabilir, bir Xamarin.Android uygulaması sesli etkileşim API nasıl kullanıldığı gösterilmektedir: [sesli etkileşimlerin](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).



#### <a name="confirm-credential"></a>Kimlik bilgisi onaylayın

Kullanarak yeni *kimlik bilgisi onaylayın* özellik Android Marshmallow unutmayın ve bunları ne kadar süreyle cihazını kilitlendi üzerinde temel kimlik doğrulaması uygulamaya özgü parolaları girin gerek kalmadan kullanıcıların ücretsiz.
Bunu yapmak için yeni kullandığınız `SetUserAuthenticationValidityDurationSeconds` yöntemi `KeyGenerator`. Kullanım `KeyGuardManager`'s `CreateConfirmDeviceCredentialIntent` uygulamanızda kullanıcının yeniden kimlik doğrulaması yöntemi. Android Marshmallow'deki bu yeni özellik hakkında daha fazla bilgi için bkz. [onaylayın kimlik bilgisi](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin uygulamanızda cihaz kimlik bilgilerini (örneğin, PIN, desen veya parola gibi) kullanmayı gösteren örnek bir uygulama sağlar: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

Bu örnek uygulamayı kullanmak için:

1.  Cihazınızda bir güvenli kilit ekranı ayarlama (**güvenli > Güvenlik > Screenlock**).
2.  Dokunun **satın alma** düğmesini tıklatın ve güvenli kilit ekranı kimlik bilgilerini doğrulayın.



### <a name="chrome-custom-tabs"></a>Chrome özel sekmeler

Uygulama geliştiricileri, bir kullanıcı bir URL dokunduğunda bir seçim yüz: uygulamayı bir tarayıcıyı başlatın veya temel bir uygulama içi tarayıcıyı kullanabilirsiniz bir `WebView`. İki seçenek de zorluklar &ndash; tarayıcı başlatma sırasında özelleştirilebilir olmayan ağır bağlam anahtar olduğunu `WebView`s durumu tarayıcı ile paylaşmayın. Ayrıca, kullanım `WebView`s ek bakım yükü ekleyebilirsiniz. 

*Özel sekmeler Chrome* kullanıcılarınızın uygulamanızı bırakmak zorunda kalmadan kolayca ve zarif bir şekilde Web siteleri Chrome'nın gücüyle görüntülemenize olanak sağlar. Bu özellik, uygulama kullanıcının web deneyimine üzerinde daha fazla denetim verir; Yerel arasında geçiş yapın ve web içeriği daha sorunsuz şekilde başvurmadan zorunda kalmadan bir `WebView`. Uygulamanızı, ayrıca Chrome nasıl görüneceğini ve aşağıdaki özelleştirerek hissettirir etkileyebilir: 

-   Araç çubuğu rengi

-   Giriş ve çıkış animasyonları

-   Chrome araç çubuğu ve taşma menüsü özel eylemler

-   (Daha hızlı yükleniyor) Chrome öncesi başlangıç ve içeriği önceden getirme

Xamarin.Android uygulamanıza bu özellikten yararlanmak için indirme ve yükleme [Android desteği özel sekmeler Kitaplığı](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Bu özellik hakkında daha fazla bilgi için bkz. [Chrome özel sekmeler](https://developer.chrome.com/multidevice/android/customtabs).



### <a name="material-design-support-library"></a>Malzeme tasarımı destek kitaplığı

Android Lollipop sunulan [malzeme tasarımı](http://www.google.com/design/spec/material-design/introduction.html) Android deneyimi yenilemek için yeni bir tasarım dili olarak (bkz [malzeme teması](~/android/user-interface/material-theme.md) malzeme tasarımı Xamarin.Android uygulamalarında kullanma hakkında bilgi için). Google Android Marshmallow ile sunulan *Android tasarım destek kitaplığı* uygulamayı kolaylaştırmak için Görünüm geliştiriciler malzeme benimsemek için tasarım. Bu kitaplık, aşağıdaki bileşenleri içerir:

-   **CoordinatorLayout** &ndash; yeni `CoordinatorLayout` pencere öğesi benzer, ancak daha güçlü bir `FrameLayout`. Kullanabileceğiniz `CoordinatorLayout` alt görünüm için bir kapsayıcı veya bir üst düzey düzen ve olarak sağlayan bir `layout_anchor` bağlantı görünümleri diğer görünümlerle göre kullanılan özniteliği.

-   **Araç çubukları daraltma** &ndash; yeni `CollapsingToolbarLayout` için bir sarmalayıcı olan çöken bir uygulamayı çubuğu `Toolbar`. (Unutmayın *uygulama çubuğunda* ne önceden olarak adlandırılan olduğu *Eylem çubuğu*.)

-   **Kayan eylem düğmesi** &ndash; uygulamanızın arabirimi birincil eylemini gösterir yuvarlak bir düğme.

-   **Etiketler için metin düzenleme kayan** &ndash; kullanan yeni bir `TextInputLayout` pencere öğesi (sarmalayan `EditText`) bir kullanıcının metin bastığında ipucu gizlenen bir kayan etiketi göstermek için.

-   **Gezinti görünümü** &ndash; yeni `NavigationView` pencere öğesi, gezinti çekmece gitmek kullanıcılar için daha kolay bir şekilde kullanmanıza yardımcı olur.

-   **Snackbar** &ndash; yeni `SnackBar` pencere öğesi olan tüm diğer öğeleri ekranında görünen ekranın alt kısmındaki kısa bir ileti görüntüler basit geri bildirim mekanizması (bir bildirim için benzer).

-   **Malzeme sekmeleri** &ndash; yeni `TabLayout` pencere öğesi, üst düzey Gezinti uygulamanıza şekilde sekmelerini görüntülemek için yatay sağlar.

Yararlanmak için [tasarım destek kitaplığı](http://developer.android.com/tools/support-library/features.html#design) Xamarin.Android uygulamanıza yükleyip Xamarin [Xamarin desteği kitaplığı tasarım](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet paketi.

Bkz: [Android desteği tasarım kitaplığı ile güzel malzeme tasarımı](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) malzeme tasarımı destek kitaplığı Xamarin.Android uygulamalarında kullanma hakkında daha fazla ayrıntı (kod örnekleri dahil) için.
Xamarin Xamarin.Android yeni Android tasarım kitaplık tanıtır ve örnek bir uygulama sağlar &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
Bu örnek, aşağıdaki tasarım kitaplığı özelliklerini gösterir:


-   Araç çubuğu daraltma
-   Kayan eylem düğmesi
-   Görünüm sabitleme
-   NavigationView
-   Snackbar

Tasarım Kitaplığı hakkında daha fazla bilgi için bkz. [Android tasarım destek kitaplığı](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) Android Geliştirici blogunda.


### <a name="additional-library-updates"></a>Ek Kitaplık güncelleştirmeleri

Android Marshmallow ek olarak, Google, birden çok çekirdek Android kitaplık için ilgili güncelleştirmeler duyurdu. Xamarin, birkaç Önizleme sürümü NuGet paketleri aracılığıyla bu güncelleştirmeler için Xamarin.Android destek sağlar: 

-   [Google Play Hizmetleri](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; yeni Google Play Hizmetleri en son sürümünü içeren *uygulama davetleri* mümkün kullanıcıların uygulamaları arkadaşlarınızla paylaşmak özellik. Bu özellik hakkında daha fazla bilgi için bkz. [genişletin uygulamanızın Reach ile Google'nın uygulama davetleri](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Android desteği kitaplıklarını kendim](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; bu Nuget'i yalnızca geriye dönük olarak uyumlu Android framework API sürümleri sağlarken kitaplığı API'leri için kullanılabilen özellikleri sunar. 

-   [Android takılabilir Kitaplığı](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; Google Play Hizmetleri bağlamaları bu NuGet içerir. Takılabilir Kitaplığı'nın en son sürümünü (daha kolay gezinme için özel uygulamalar dahil) yeni özellik Android Wear platformuna sunar. 


## <a name="summary"></a>Özet

Bu makalede, Android Marshmallow sunulan ve yükleme ve en son araçları ve Xamarin.Android geliştirme paketleri Marshmallow'da yapılandırma adımları açıklanmıştır. Xamarin.Android geliştirme için de en heyecan verici yeni Android Marshmallow özelliklerine genel bakış sağlanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK'yı edinme](https://developer.android.com/sdk/index.html#Other)
- [Özelliklere genel bakış](https://developer.android.com/preview/api-overview.html)
- [Sürüm Notları](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (örnek)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (örnek)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (örnek)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
