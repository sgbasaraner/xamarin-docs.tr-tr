---
title: "Lolipop özellikleri"
description: "Bu makalede Android 5.0 (Lolipop) sunulan yeni özellikler üst düzey bir genel bakış sağlar. Bu özellikler animasyonları, görünüm gölgeleri ve drawable tonlamak gibi yeni destekleyen özellikler yanı sıra, malzeme tema olarak adlandırılan yeni bir kullanıcı arabirimi stili içerir. Android 5.0 Gelişmiş bildirimleri, iki yeni UI pencere öğeleri, yeni bir iş Zamanlayıcısı ve bir dizi yeni API depolama, ağ, bağlantı ve çoklu ortam yeteneklerini geliştirmek için de içerir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: de6829a0a698133ad9002ead1cd7c534a30b1f6c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="lollipop-features"></a>Lolipop özellikleri

_Bu makalede Android 5.0 (Lolipop) sunulan yeni özellikler üst düzey bir genel bakış sağlar. Bu özellikler animasyonları, görünüm gölgeleri ve drawable tonlamak gibi yeni destekleyen özellikler yanı sıra, malzeme tema olarak adlandırılan yeni bir kullanıcı arabirimi stili içerir. Android 5.0 Gelişmiş bildirimleri, iki yeni UI pencere öğeleri, yeni bir iş Zamanlayıcısı ve bir dizi yeni API depolama, ağ, bağlantı ve çoklu ortam yeteneklerini geliştirmek için de içerir._

## <a name="lollipop-overview"></a>Lolipop genel bakış

Android 5.0 (Lolipop) yeni bir tasarım dil tanıtır *malzeme tasarım*, ve onunla bir destekleyen uygulamalar daha kolay ve daha sezgisel kullanmak için kolaylaştıran yeni özellikler cast. Malzeme tasarımıyla Android 5.0, Android telefonlar yalnızca bir facelift verir; Ayrıca yeni bir tasarım kuralları kümesi Android tabanlı tabletler, masaüstü bilgisayarları, Gözcü ve akıllı TV için sağlar. Bu tasarım kurallar Basitlik ve minimalism yapma çalışırken vurgulamak kullanıcıların hızla yardımcı olmak ve sezgisel arabirimi anlamak için tanıdık tactile öznitelikleri (örneğin, gerçekçi yüzey ve kenar yardımlar) kullanın.

*Malzeme tema* Android bu UI tasarım kurallara somut halidir. Bu makalede, malzeme temanın destekleme özelliklerini kapsayan başlar:

-   **Animasyon** &ndash; *Touch geri bildirim* animasyonları *etkinlik geçiş* animasyonları *görüntülemek durum geçişi* animasyonları ve bir *etkisi ortaya*.

-   **Gölgeleri ve ayrıcalık görüntüle** &ndash; görünümleri şimdi sahip bir `elevation` özellik; görünümlerle daha yüksek `elevation` değerleri cast arka plan üzerinde büyük gölgeleri.

-   **Renk özellikleri** &ndash; *Drawable tonlamak* görüntü varlıklarının kendi rengi değiştirerek yeniden kullanmak mümkün hale getirir ve *belirgin renk ayıklama* dinamik olarak yardımcı olur Tema renkleri görüntüdeki uygulamanızı temel.

Birçok malzeme özellikleri zaten içinde yerleşik olarak bulunan tema Android 5.0 UI deneyimi, diğerleri açıkça uygulamalara eklenmelidir sırada. Örneğin, bazı standart görünümler (düğme gibi) uygulamaları çoğu görünüm gölgeleri etkinleştirmelisiniz sırada dokunma geri bildirim animasyonları zaten içeriyor.

UI geliştirmeleri malzeme tema duruma ek olarak Android 5.0 Bu makalede kapsanan çeşitli diğer yeni özellikler de içerir:

-   **Gelişmiş bildirimleri** &ndash; Android 5.0 bildirimleri önemli ölçüde güncelleştirildi yeni bir görünüm, kilit ekranı bildirimler için destek ve yeni bir ile *ekran göstergesi* bildirim sunu biçimi.

-   **Yeni kullanıcı Arabirimi pencere öğeleri** &ndash; yeni `RecyclerView` pencere öğesi uygulamaların büyük veri kümeleri ve karmaşık bilgi ve yeni iletmek kolaylaştırır `CardView` pencere öğesi metin görüntüleme için Basitleştirilmiş kart benzeri sunu biçimine sağlar ve görüntüler.

-   **Yeni API** &ndash; Android 5.0 Bluetooth bağlantısı, daha kolay Depolama Yönetimi ve çoklu ortam ve kamera cihazların daha esnek denetimi geliştirilmiş yeni API birden çok ağ desteği ekler. Özellik zamanlama yeni bir iş görevlerini zaman uyumsuz olarak çalıştırmak kullanılabilir programlanan zamanlarda. Bu özellik, pil ömrünün artırmak için örneğin, Aygıt bağlandığında yerinde ve şarj yapılacak görevleri zamanlama yardımcı olur.


## <a name="requirements"></a>Gereksinimler

Aşağıdaki Xamarin tabanlı uygulamalarda yeni Android 5.0 özellikleri kullanmak için gereklidir:

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-   **Android SDK** &ndash; Android 5.0 (API 21) veya sonrası Android SDK Yöneticisi aracılığıyla yüklü olmalıdır.

-   **Java Geliştirme Seti** &ndash; Xamarin.Android gerektirir [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya API düzeyi 24 geliştiriyorsanız sonraki ya da daha büyük (JDK 1.8 de destekler API düzeylerini Lolipop dahil olmak üzere 24'den önceki). Özel denetimler ya da formları genele gitmeyi kullanıyorsanız JDK 1.8 64-bit sürümü gereklidir.

Kullanmaya devam edebilirsiniz [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özellikle geliştirme veya önceki bir sürümünü kullanıyorsanız.


## <a name="setting-up-an-android-50-project"></a>Android 5.0 projesinde ayarlama

Bir Android 5.0 projesi oluşturmak için SDK paketlerini ve en son araçları yüklemeniz gerekir. Android 5.0 hedefleyen bir Xamarin.Android projesi ayarlamak için aşağıdaki adımları kullanın:

1. Xamarin.Android Araçları'nı yükleyin ve Xamarin lisansınız etkinleştirin. Bkz: [Kurulum ve yükleme](~/android/get-started/installation/index.md) Xamarin.Android yükleme hakkında daha fazla bilgi için.

2. Mac için Visual Studio kullanıyorsanız, en son Android 5.0 güncelleştirmeleri yükleyin.

3. Android SDK Yöneticisi'ni başlatın (Mac için Visual Studio'da kullanın **Araçları &gt; Open Android SDK Manager&hellip;**) ve Android SDK Araçları 23.0.5 yükleyin ya da daha sonra:

    [![Android SDK Yöneticisi'nde Android SDK Araçları seçme](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   Ayrıca, en son Android 5.0 SDK paketlerin (API 21 veya üstü) yükleyin:

    [![Android SDK Yöneticisi'nde Android 5.0 SDK paketleri yükleniyor](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Android SDK Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz: [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html).

4. Yeni bir Xamarin.Android projesi oluşturun. Xamarin Android geliştirme yeniyseniz, bkz: [Hello, Android](~/android/get-started/hello-android/index.md) Android projeleri oluşturma hakkında bilgi edinmek için. Bir Android projesi oluşturduğunuzda, Android 5.0 için sürüm ayarlarını yapılandırdığınızdan emin olun.
   Mac için Visual Studio'da gidin **proje seçenekleri &gt; yapı &gt; genel** ve **hedef framework** için **Android 5.0 (Lolipop)** veya Daha sonra:

    ![Hedef Framwework için Android 5.0 Lolipop ayarlama](lollipop-images/target-framework.png)

   Altında **proje seçenekleri &gt; yapı &gt; Android uygulaması**, en az ve Android sürümü için hedef **otomatik - kullanım hedef framework sürümü**:

    ![En az ve hedef Android sürümlerini otomatik olarak ayarlama](lollipop-images/minimum-android-version.png)

5. Bir öykünücü veya bir Android cihaz uygulamanızı test etmek için yapılandırın. Bir öykünücü kullanıyorsanız bkz [Android öykünücüsü Kurulumu](~/android/get-started/installation/android-emulator/index.md) Xamarin Studio veya Visual Studio ile Android öykünücüsünde kullanmak için yapılandırma hakkında bilgi edinmek için. Bir Android cihaz kullanıyorsanız, bkz: [ayarı yukarı Önizleme SDK](https://developer.android.com/preview/setup-sdk.html) Android 5.0 için Cihazınızı güncelleştirmek hakkında bilgi edinmek için. Çalıştıran ve Xamarin.Android uygulamalarında hata ayıklama için Android Cihazınızı yapılandırmak için bkz: [ayarlamak yukarı geliştirme için cihazı](~/android/get-started/installation/set-up-device-for-development.md).

Not: Android M Önizleme hedefleme mevcut bir Android projesini güncelleştiriyorsanız güncelleştirmeniz gerekir **hedef Framework** ve **Android sürümü** yukarıda açıklanan değerler için.

## <a name="important-changes"></a>Önemli değişiklikler

Daha önce yayımlanan Android uygulamaları Android 5.0 değişikliklerden etkilenir. Özellikle, Android 5.0 yeni bir çalışma zamanı ve önemli ölçüde değiştirilen bildirim biçimi kullanır.

### <a name="android-runtime"></a>Android çalışma zamanı

Android 5.0 yeni Android çalışma zamanı (resim) Dalvik yerine varsayılan çalışma zamanı olarak kullanır. Resim birkaç önemli yeni özellikleri uygular:

-   **Tamamlanan-in-time (Uygulama Nesne AĞACI) derleme** &ndash; Uygulama Nesne AĞACI, uygulamayı ilk başlatılmadan önce uygulama kodu derleme uygulama performansı geliştirebilir. Bir uygulama yüklendiğinde, resim bir hedef cihaz için yürütülebilir bir derlenmiş uygulaması oluşturur.

-   **Çöp toplama (GC) geliştirilmiş** &ndash; resim GC yenilikleri de uygulama performansı artırır. Şimdi çöp toplama bir GC Duraklat iki yerine kullanır ve daha zamanında eşzamanlı GC işlemlerini tamamlayın.

-   **Uygulama hata ayıklama geliştirilmiş** &ndash; resim raporları kilitlenme ve özel durumları çözümlenmesinde yardımcı olmak için tanılama daha fazla ayrıntı sağlar.

Var olan uygulamaları resim altında değişiklik olmadan çalışmalıdır &ndash; teknikleri önceki Dalvik çalışma zamanına benzersiz yararlanan uygulamalar dışında hangi çalışmayabilir altında resim. Bu değişiklikler hakkında daha fazla bilgi için bkz: [doğrulama uygulama davranışı üzerinde Android çalışma zamanı (resim)](http://developer.android.com/guide/practices/verifying-apps-art.html).


### <a name="notification-changes"></a>Bildirim değişiklikleri

Bildirimleri önemli ölçüde Android 5.0 ile değiştirilmiştir:

-   **Ses Efekti ve titreşim işlenir farklı** &ndash; bildirim sesleri ve vibrations tarafından işlenmesini şimdi `Notification.Builder` yerine `Ringtone`, `MediaPlayer`, ve `Vibrator`.

-   **Yeni renk düzenini** &ndash; malzeme tema uygun olarak, bildirimler arka plan beyaz ya da çok açık koyu metinle işlenir. Ayrıca, bildirim simgelerini alfa kanallarında sistem renk düzenleriyle koordine etmek için Android tarafından değiştirilebilir. 

-   **Kilit ekranı bildirimler** &ndash; bildirimler artık cihaz kilit ekranı üzerinde görüntülenir.

-   **Ekran göstergesi** &ndash; yüksek öncelikli bildirimler artık küçük bir kayan pencere (ekran göstergesi bildirim) içinde görünür zaman cihaz kilidinin açık olduğundan ve ekran açıktır.

Çoğu durumda, var olan uygulama bildirimi işlevi Android 5.0 için bağlantı noktası oluşturma, aşağıdaki adımları gerektirir:

1.  Kullanmak için kodunuzu Dönüştür `Notification.Builder` (veya `NotificationsCompat.Builder`) bildirimleri oluşturmak için. 

2.  Varolan bildirim varlıklarınızı yeni malzeme Tema rengi düzeninde görüntülenebilir olduğunu doğrulayın.

3.  Kilit ekranı üzerinde sunulduğunda bildirimlerinizi olmalıdır hangi görünürlük karar verin. Bir bildirim ortak değilse, hangi içerik kilit ekranı üzerinde gösterilmesi gerekir?

4.  Yeni Android 5.0 ile doğru şekilde işlenir bildirimlerinizi kategorisini ayarlandı *rahatsız* modu.

Kayıttan yürütme durumu Aktarım denetimleri, görüntü media bildirimlerinizi sunmak kullanırsanız `RemoteControlClient`, veya arama `ActivityManager.GetRecentTasks`, bkz: [önemli davranış değişiklikleri](http://developer.android.com/preview/api-overview.html#Behaviors) bildirimlerinizi Android için güncelleştirme hakkında daha fazla bilgi için 5.0.

Android bildirimleri oluşturma hakkında daha fazla bilgi için bkz: [yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications.md). [Uyumluluk](~/android/app-fundamentals/notifications/local-notifications.md#compatibility) bu makalenin bölümüne aşağı uyumlu bildirimleri oluşturma açıklanmaktadır Android önceki sürümleriyle.


## <a name="material-theme"></a>Malzeme tema

Yeni Android 5.0 malzeme tema büyük değişiklikler Görünüm ve kullanımında Android UI getirir. Görsel öğeleri artık kalın grafikler, tipografi ve yazdırma tabanlı tasarımının açık renkleri tactile yüzeyleri kullanın. Malzeme tema örnekleri aşağıdaki ekran görüntülerinde gösterilen:

[![Malzeme tema giriş ekranı, uygulamaları ekran ve ayar ekran ekran görüntüleri](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 sol tarafta gösterilen giriş ekranı kutlayan. Uygulama listesinin ilk ekran center ekran görüntüsü, sağdaki ekran ise **ayarları** ekran. Google [malzeme tasarım](https://material.io/guidelines/material-design/introduction.html) belirtimi yeni malzeme teması kavramı arkasındaki temel tasarım kuralları açıklar.

Malzeme tema, uygulamanızda kullanabileceğiniz üç yerleşik özellikleri içerir: `Theme.Material` koyu tema (varsayılan), `Theme.Material.Light` tema ve `Theme.Material.Light.DarkActionBar` tema: 

[![Koyu ekran görüntüleri, açık ve DarkActionBar temaları](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Xamarin.Android uygulamaları malzeme tema özelliklerini kullanma hakkında daha fazla bilgi için bkz: [malzeme tema](~/android/user-interface/material-theme.md).


## <a name="animations"></a>Animasyon

Android 5.0 dokunma geri bildirim animasyonları, etkinlik geçiş animasyonlarını ve uygulama arabirimleri kullanmak için daha sezgisel hale getirmek için görünüm durumuna geçiş animasyonlarını sağlar. Ayrıca, Android 5.0 kullanabileceğiniz uygulamaları *etkisi ortaya* görünümleri göster veya gizlemek için animasyonları. Kullanabileceğiniz *hareket eğri* nasıl hızlı bir şekilde yapılandırmak için ayarları veya yavaş animasyonları işlenir.


### <a name="touch-feedback-animations"></a>Geri bildirim animasyonları dokunma

Bir görünüm işlemdeki zaman dokunma geri bildirim animasyonları görsel geribildirim kullanıcılara sağlar. Touched olduklarında örneğin düğmeleri şimdi ripple etkisi görüntülemek &ndash; Android 5.0 varsayılan dokunma geri bildirim animasyonda budur. Ripple animasyon yeni tarafından gerçekleştirilen `RippleDrawable` sınıfı. Ripple etkisi görünümü sınırları son veya Görünüm sınırları genişletmek için yapılandırılabilir. Örneğin, aşağıdaki ekran görüntüleri dizisini bir düğme ripple aslında dokunma animasyon sırasında gösterilmektedir:

![Çerçeve ekran görüntüleri düğmesinde ripple animasyonun tarafından çerçeve](lollipop-images/touch-animation.png)

Kalan dizisi (soldan sağa) ripple etkisi düğmesi kenarına nasıl yayılan gösterilmiştir düğmesine ilk dokunma kişiyle sol tarafta ilk görüntüde oluşursa. Ripple animasyon sona erdiğinde, görünüm özgün görünümünü verir. Varsayılan ripple animasyon saniyenin bir yerde alır, ancak animasyonun uzunluğunu daha uzun veya kısaysa süreler boyunca özelleştirilebilir.

Geri bildirim animasyonları Android 5.0 touch hakkında daha fazla bilgi için bkz: [Touch geri bildirim özelleştirme](http://developer.android.com/training/material/animations.html#Touch).


### <a name="activity-transition-animations"></a>Etkinlik geçiş animasyonlarını

Bir etkinlik diğerine geçiş yaptığında etkinlik geçiş animasyonlarını kullanıcılara visual sürekliliği duygusu verin. Uygulamaları geçiş animasyonlarını üç tür belirtebilirsiniz:

-   **Geçiş girin** &ndash; bir etkinlik Sahne girdiğinde için.

-   **Exit geçiş** &ndash; bir etkinlik Sahne çıktığında için.

-   **Öğe geçiş paylaşılan** &ndash; iki etkinlikler için ortak bir görünüm sonraki ilk etkinlik geçişler olarak değiştiği için.

Örneğin, aşağıdaki ekran görüntüleri dizisini paylaşılan öğesi geçiş gösterilmektedir:

[![Çerçeve tarafından paylaşılan öğesi geçiş animasyonun çerçeve ekran görüntüleri](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

Paylaşılan öğesi (bir tırtıl fotoğraf) ilk etkinlik birkaç görünümlerde biridir; İkinci etkinlik ilk etkinlik geçişleri ikinci olarak yalnızca görünümünde olmasını büyütür.

#### <a name="enter-transition-animation-types"></a>Geçiş animasyon türleri girin

ENTER geçişler için Android 5.0 animasyonları üç türlerini sağlar:

-   **Animasyon açılımı** &ndash; Sahne merkezi görünümünden büyütür.

-   **Animasyon slayt** &ndash; bir görünüm Sahne kenarlarına birinden taşır.

-   **Animasyon silinerek** &ndash; Sahne görünüme kaybolur.

#### <a name="exit-transition-animation-types"></a>Çıkış geçiş animasyon türleri

Çıkış geçişleri için Android 5.0 animasyonları üç türlerini sağlar:

-   **Animasyon açılımı** &ndash; Sahne merkezi için bir görünüm küçültür.

-   **Animasyon slayt** &ndash; bir görünüm Sahne kenarlarına birine taşır.

-   **Animasyon silinerek** &ndash; Sahne dışında bir görünüm kaybolur.

#### <a name="shared-element-transition-animation-types"></a>Öğe geçiş animasyon türleri paylaşılan

Paylaşılan öğesi geçişleri animasyonları, birden çok tür gibi destekler:

-   Bir görünüm düzeni veya küçük sınırlarına değiştirme.

-   Ölçek ve bir görünüm dönüşünü değiştirme.

-   Bir görünüm için boyut ve ölçek türünü değiştirme.

Etkinlik geçiş animasyonlarını Android 5.0 hakkında daha fazla bilgi için bkz: [özelleştirme etkinlik geçişleri](http://developer.android.com/training/material/animations.html#Transitions).


### <a name="view-state-transition-animations"></a>Görünüm durumu geçiş animasyonlarını

Android 5.0 bir görünüm durumunu değiştirdiğinde çalıştırmak animasyonlarını mümkün kılar. Aşağıdaki teknikler birini kullanarak görünüm durumu geçişleri animasyon oluşturabilirsiniz:

-   Belirli bir görünümle ilişkili durum değişiklikleri animasyon drawables oluşturun. Yeni `AnimatedStateListDrawable` sınıfı, Görünüm durumu değişiklikleri arasında animasyonları görüntüle drawables oluşturmanıza olanak sağlar.

-   Bir görünüm durumunu değiştirdiğinde çalıştırılan animasyon işlevselliği tanımlayın. Yeni `StateListAnimator` sınıfı, bir görünüm durumunu değiştirdiğinde çalıştırılan bir Animator'da tanımlamanıza olanak sağlar.

Görünüm durumu geçiş animasyonlarını Android 5.0 hakkında daha fazla bilgi için bkz: [hale getirmeyi görünüm durumu değişiklikleri](http://developer.android.com/training/material/animations.html#ViewState).


### <a name="reveal-effect"></a>Etkili Göster

*Etkisi ortaya* ortaya veya bir görünüm gizlemek için bu değişiklikleri RADIUS kırpma daire şeklindedir. Kırpma dairenin ilk ve son RADIUS ayarlayarak bu etkiyi kontrol edebilirsiniz. Aşağıdaki ekran görüntüleri dizisi ekranın açığa efekti animasyonu Merkezi'nden gösterilmektedir:

[![Çerçeve tarafından açığa animasyon çerçeve ekran görüntüleri](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

Sonraki dizisi ekranın sol alt köşeden gerçekleşir açığa efekti animasyonu gösterilmektedir:

[![Çerçeve ekran görüntüleri kırpma animasyonun tarafından çerçeve](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

Animasyon ters çevrilebilir ortaya; diğer bir deyişle, kırpma daire görünümü gizlemek için küçültmek yerine görünümü ortaya çıkarmak üzere büyütün.

Android 5.0 açığa etkisi hakkında daha fazla bilgi için bkz: [ortaya efekti kullanmak](http://developer.android.com/training/material/animations.html#Reveal).


### <a name="curved-motion"></a>Eğri hareket

Bu animasyon özelliklerine ek olarak, Android 5.0 zaman ve hareket Eğriler animasyonların belirtmenizi sağlayan yeni API'ler de sağlar. Android 5.0 bu Eğriler zamana bağlı ve uzamsal taşıma sırasında bir animasyon kesinti için kullanır. Üç Eğriler Android 5.0 ile tanımlanır:

-   **Hızlı\_çıkışı\_doğrusal\_içinde** &ndash; hızlı bir şekilde hızlandırır ve animasyonun sonuna kadar hızlandırmak devam eder.

-   **Hızlı\_çıkışı\_yavaş\_içinde** &ndash; Accelerates hızla ve yavaş animasyonun sonuna doğru decelerates.

-   **Doğrusal\_çıkışı\_yavaş\_içinde** &ndash; başlangıcı en yüksek hız ve yavaş animasyonun sonuna decelerates.

Kullanabileceğiniz yeni `PathInterpolator` nasıl hareket ilişkilendirme gerçekleşir belirlemek için sınıf. `PathInterpolator` Belirtilen denetim noktalarına ve hareket Eğriler göre animasyon yolları arasında çapraz geçiş yapan bir ara olur. Eğri hareket ayarları Android 5.0 ile belirtme hakkında daha fazla bilgi için bkz: [kullanım eğri hareket](http://developer.android.com/training/material/animations.html#CurvedMotion).


## <a name="view-shadows--elevation"></a>Görünüm gölgeleri & yükseltme

Android 5.0 içinde belirttiğiniz *ayrıcalık* yeni ayarlayarak bir görünümün `Z` özelliği. Büyüktür `Z` değeri float arka plan üstüne daha yüksek görünmesini görünüm arka plan üzerinde büyük bir gölge cast görünüme neden olur. Bir görünüm ilk ayrıcalıkların yapılandırarak ayarlayabileceğiniz kendi `elevation` yerleşiminde özniteliği.

Aşağıdaki örnek tarafından boş bir cast gölgeleri gösterilmektedir `TextView` ayrıcalık öznitelik 2dp, 4dp ve 6dp, sırasıyla ayarlandığında denetleme:

[![Gölgeleri progessively büyük ekran görüntüleri görüntüleme](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

Animasyonları bir görünümün görünümün arka plan üstüne geçici olarak artmaya görünmesini sağlamak için kullanılabilmesi için veya gölge ayarlarını görüntüle (yukarıda gösterildiği gibi) statik olabilir. Kullanabileceğiniz `ViewPropertyAnimator` bir görünüm ayrıcalıkların animasyon sınıfı. Bir görünüm ayrıcalıkların düzenini toplamıdır `elevation` ayarı artı bir `translationZ` üzerinden ayarlayabilirsiniz özelliği bir `ViewPropertyAnimator` yöntem çağrısı.

Android 5.0 görünüm gölge hakkında daha fazla bilgi için bkz: [gölgeleri tanımlama ve kırpma görünümleri](http://developer.android.com/training/material/shadows-clipping.html).


## <a name="color-features"></a>Renk özellikleri

Android 5.0 uygulamaları renkte yönetmek için iki yeni özellik sağlar:

-   *Drawable tonlamak* yerleşim özniteliği değiştirerek görüntü varlıklarının renkleri alter olanak tanır.

-   *Belirgin renk ayıklama* dinamik olarak görüntülenen bir görüntü ile renk paleti koordine etmek için uygulamanızın renk temasını özelleştirmenize olanak sağlar.


### <a name="drawable-tinting"></a>Drawable tonlamak

Yeni bir Android 5.0 düzenleri tanıması `tint` farklı renkler görüntülemek için bu varlıkları birden fazla sürümünü oluşturmak zorunda kalmadan drawables rengini ayarlamak için kullanabileceğiniz özniteliği. Bu özelliği kullanmak için bir bit eşlem bir alfa maskesi ve kullanım olarak tanımladığınız `tint` varlık rengini tanımlamak için öznitelik. Varlıklar bir kere oluşturup, tema eşleşecek şekilde düzeninizi renk mümkün kılar.

Aşağıdaki örnekte, tek bir görüntü varlık &ndash; saydam arka plan beyaz Logonuzla &ndash; TINT çeşitlemeler oluşturmak için kullanılır:

![Saydam arka plan beyaz Xamarin logosu](lollipop-images/xamarin-logo-white.png)

Bu logo mavi döngüsel bir arka plan üzerinde aşağıdaki örneklerde gösterildiği gibi görüntülenir. Logo olmadan görünme görüntüdür soldaki bir `tint` ayarı. Merkezi görüntünün, logo, `tint` özniteliği koyu gri için ayarlanmış. Sağ taraftaki görüntüsündeki `tint` için açık bir gri ayarlayın:

![Yukarıdaki logosu farklı TINT ayarlarıyla örnekleri](lollipop-images/drawable-tinting.png)

Drawable Android 5.0 ile tonlamak hakkında daha fazla bilgi için bkz: [Drawable tonlamak](http://developer.android.com/training/material/drawables.html#DrawableTint).


### <a name="prominent-color-extraction"></a>Belirgin renk ayıklama

Yeni Android 5.0 `Palette` sınıfı sağlar, böylece bunları dinamik olarak bir özel renk paletini uygulayabilirsiniz renkleri görüntüden ayıklayın. `Palette` Sınıfı bir görüntüden altı renk ayıklar ve bu renklerin renk doygunluğunu ve parlaklığını göreli düzeylerine göre etiketler:

-   Vibrant

-   Canlı koyu

-   Canlı açık

-   Kapalı

-   Yumuşak koyu

-   Açık kapalı

Örneğin, aşağıdaki ekran görüntüleri bir fotoğraf uygulaması görüntüleme ekranındaki görüntüden belirgin renkleri ayıklar ve görüntünün eşleştirmek için renk düzenini uygulamanın uyarlamak için bu renkleri kullanır:

[![Yeşil, pembe ve mavi tema rengi ayıklamaları ekran görüntüleri](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

Yukarıdaki ekran görüntülerinde ayıklanan "Canlı ışık" Eylem çubuğu ayarlanır renk ve arka plan ayıklanan "Canlı koyu için" ayarlanmış rengi. Her yukarıdaki örnekte, bir satır küçük renk kareler görüntüden ayıklanmış palet renklerini göstermek için dahil edilmiştir.

Renk ayıklama Android 5.0 hakkında daha fazla bilgi için bkz: [ayıklanıyor belirgin renkleri bir görüntüden](http://developer.android.com/training/material/drawables.html#ColorExtract).


## <a name="new-ui-widgets"></a>Yeni kullanıcı Arabirimi pencere öğeleri

Android 5.0 iki yeni UI pencere öğeleri sunar:

-   `RecyclerView` &ndash; Kaydırılabilir öğelerinin bir listesini görüntüler Görünüm Grup.

-   `CardView` &ndash; Yuvarlak köşeli temel düzeni.

Her iki pencere öğeleri desteklenmiş malzeme tema özellikleri için destek içerir. Örneğin, `RecyclerView` animasyon ekleme ve kaldırma görünümleri için kullanır ve `CardView` kullandığı her kartı arka plan üstüne float görünür hale getirmek için gölge görüntüleyin. Bu yeni pencere öğeleri örnekleri aşağıdaki ekran görüntülerinde gösterilmektedir:

[![RecyclerView ile oluşturulmuş uygulamalara ekran görüntüleri](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Sol taraftaki ekran örneğidir `RecyclerView` olarak bir e-posta uygulaması ve ekran görüntüsü üzerinde kullanılan sağa örneğidir `CardView` seyahat ayırma uygulamada kullanılan.


### <a name="recyclerview"></a>RecyclerView

`RecyclerView` benzer `ListView,` fakat görünümleri veya dinamik olarak değişen öğeleri listeleriyle büyük kümeleri için daha uygundur. Gibi `ListView,` temel alınan veri kümesine erişmek için bir bağdaştırıcı belirtin. Ancak, farklı `ListView,` kullandığınız bir *Düzen Yöneticisi* konumlandırmak için öğelerini içinde `RecyclerView`. Düzen Yöneticisi ayrıca görünümü geri dönüştürme mvc'deki; artık kullanıcı tarafından görülebilir öğesi görünümleri kullanılmasını yönetir.

Kullandığınızda, bir `RecyclerView` pencere, belirtmelisiniz. bir `LayoutManager` ve bir bağdaştırıcı. Bu çizimde gösterildiği gibi `LayoutManager` bağdaştırıcısı arasında aracı olan ve `RecyclerView`:

![LayoutManager, bağdaştırıcı ve veri kümesi destekleme ile RecyclerView diyagramı](lollipop-images/recyclerview-diagram.png)

Aşağıdaki ekran görüntüleri gösteren bir `RecyclerView` 100 öğeleri içeren (her bir öğeyi oluşan bir `ImageView` ve `TextView`):

[![Resimler arasında kaydırma RecyclerView uygulama ekran görüntüleri](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` Bu büyük bir veri kümesinin kolayca işler &ndash; sona erdirmek için listenin başından kaydırma bu örnekteki listesinin uygulama yalnızca birkaç saniye sürer. `RecyclerView` Ayrıca animasyonları destekler; Aslında, animasyonları öğeler ekleme ve kaldırma için varsayılan olarak etkinleştirilir. İçin bir öğe eklendiğinde bir `RecyclerView`, ekran görüntüleri bu sırayla gösterildiği gibi buna belirerek:

[![Çerçevesi tarafından fotoğraf öğesi Soluklaşan Çerçeve ekran görüntüsü](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

Hakkında daha fazla bilgi için `RecyclerView`, bkz: [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).


### <a name="cardview"></a>Kart görünümü

`CardView` Kayan bir kart yuvarlak köşeli benzetim basit bir görünümdür. Çünkü `CardView` yerleşik görünüm gölgeleri sahipse, visual derinliği uygulamanıza eklemek kolay bir yol sağlar. Aşağıdaki ekran görüntüleri üç metin odaklı örnekleri Göster `CardView`:

[![Kart görünümü tabanlı öğeleriyle RecyclerView kullanarak uygulamaları örnek ekran görüntüleri](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Yukarıdaki örnekte kartların her birine içeren bir `TextView`; üzerinden arka plan rengini ayarlama `cardBackgroundColor` özniteliği.

Hakkında daha fazla bilgi için `CardView`, bkz: [kart görünümü](~/android/user-interface/controls/card-view.md).


## <a name="enhanced-notifications"></a>Gelişmiş bildirimleri

Android 5.0 bildirim sisteminde önemli ölçüde yeni bir görsel biçimi ve yeni özelliklerle güncelleştirilmiştir. Bildirimleri Android 5.0 ile yeni bir görünüme sahiptir. Örneğin, Android 5.0 Bildirimlerde artık açık bir arka plan üzerinde koyu metni kullanın:

![Beklemediğiniz genişletilmiş bir Android 5.0 bildirim örneği](lollipop-images/expanded-notification-contracted.png)

Büyük simge (yukarıdaki örnekte gösterildiği gibi) bir bildirim görüntülenir, Android 5.0 küçük simgesi büyük simge bir gösterge gösterir. 

Android 5.0 ile bildirimler ayrıca cihaz kilit ekranı üzerinde yer alabilir.
Örneğin, bir örnek ekran görüntüsü tek bir bildirim ile kilit ekranı şöyledir:

[![Kilit ekranında görünen bildirim ekran görüntüsü](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

Kullanıcılar çift dokunmayla cihazın kilidini açmak ve bu bildirim kaynaklanan uygulama atlamak için kilit ekranı bildirim veya sağdan bildirim yok sayın. Bildirimleri sahip yeni bir *görünürlük* ayarı belirler ne kadar içerik kilit ekranı üzerinde görüntülenebilir. Kullanıcılar kilit ekranı bildirimler gösterilecek hassas içerikleri izin verilip verilmeyeceğini seçebilirsiniz.

Android 5.0 tanıtır adlı yeni bir yüksek öncelikli bildirim sunu biçimi *ekran göstergesi*. Ekran göstergesi bildirimleri aşağı birkaç saniye ekranın üst kısmından kaydırın ve geri bildirim gölge ekranın üstünde için geri Çekilme. Ekran göstergesi bildirimleri sistem şu anda çalışan etkinlik kesintiye uğratmadan kullanıcı önünde önemli bilgileri koymak için kullanıcı Arabirimi olun. Aşağıdaki örnek bir uygulama üzerinde görüntüleyen basit bir ekran göstergesi bildirim gösterilmektedir:

[![Heads-up bildirim örneği](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

Ekran göstergesi bildirimleri genellikle aşağıdaki olaylar için kullanılır:

-   Yeni bir sonraki ileti

-   Gelen bir telefon araması

-   Düşük pil göstergesi

-   Bir uyarı

Yalnızca yüksek veya en fazla öncelik ayarı olduğunda android 5.0 ekran göstergesi biçiminde bir bildirim görüntüler.

Android 5.0 ile sıralamak ve bildirimleri daha akıllıca görüntülemek Android yardımcı olmak için bildirim meta verileri sağlayabilir. Android 5.0 bildirimleri önceliği, görünürlük ve kategoriye göre düzenler.
Bildirim kategorileri filtrelemek için kullanılan cihaz olduğunda hangi bildirimleri'nin sunulabilir *rahatsız* modu.

Oluşturma ve en son Android 5.0 özellikleri ile bildirimleri başlatma hakkında ayrıntılı bilgi için bkz: [yerel bildirimler](~/android/app-fundamentals/notifications/local-notifications.md).


## <a name="new-apis"></a>Yeni API'ler

Yukarıda açıklanan yeni görünüm ve kullanımında özelliklerine ek olarak, Android 5.0 varolan multimedya özelliklerini, depolama ve kablosuz/bağlantı işlevselliğini genişleten yeni API'leri ekler. Ayrıca, Android 5.0 desteklemek için yeni bir iş Zamanlayıcı özellik yeni API'ları içerir.

### <a name="camera"></a>Kamera

Android 5.0 birkaç yeni API için geliştirilmiş kamera yetenekleri sağlar. Yeni `Android.Hardware.Camera2` ad alanı, bir Android cihazına bağlı tek tek kamera aygıtları erişmek için işlevselliği içerir. Ayrıca, `Android.Hardware.Camera2` her kamera cihaz ardışık olarak modeller: Yakalama isteği kabul eder, görüntüyü yakalar ve sonucu çıkarır. Bu yaklaşım, kamera aygıt birden fazla Yakalama isteği kuyruğuna uygulamalar için mümkün kılar.

Aşağıdaki API, bu yeni özellikleri mümkün kılar:

-   `CameraManager.GetCameraIdList` &ndash; Kamera aygıtları programlı olarak erişmek için yardımcı olur; kullandığınız `CameraManager.OpenCamera` belirli kamera aygıta bağlanmayı.

-   `CameraCaptureSession` &ndash; Yakalar veya kamera aygıt görüntülerden akışları. Uygulamanız bir `CameraCaptureSession.CaptureListener` yeni görüntü yakalama olayları işlemek için arabirim.

-   `CaptureRequest` &ndash; Yakalama parametrelerini tanımlar.

-   `CaptureResult` &ndash; Görüntü yakalama işlemi sonuçlarını sağlar.

Yeni kamera Android 5.0 API'leri hakkında daha fazla bilgi için bkz: [medya](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="audio-playback"></a>Ses çalma

Android 5.0 güncelleştirmeleri `AudioTrack` sınıfı daha iyi ses çalma için:

-   `ENCODING_PCM_FLOAT` &ndash; Yapılandırır `AudioTrack` kayan nokta biçiminde daha iyi dinamik aralık, daha fazla boş alan ve (sayesinde yüksek duyarlık) daha yüksek kaliteli ses verileri kabul etmek için. Ayrıca, kayan nokta biçimi ses kırpma kaçınmaya yardımcı olur.

-   `ByteBuffer` &ndash; Ses verilere artık sağlayabilir `AudioTrack` bir bayt dizisi olarak.

-   `WRITE_NON_BLOCKING` &ndash; Bu seçenek arabelleğe alma basitleştirir ve bazı uygulamalar için çoklu iş parçacığı kullanımı.

Hakkında daha fazla bilgi için `AudioTrack` Android 5.0, yenilikleri görmek [medya](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="media-playback-control"></a>Ortam kayıttan yürütme denetimi

Android 5.0 tanıtır yeni `Android.Media.MediaController` sınıfı, hangi değiştirir `RemoteControlClient`. `Android.Media.MediaController` Basitleştirilmiş Aktarım Denetimi API'ler sağlar ve UI bağlamı dışında kayıttan yürütme iş parçacığı denetim sunar. Aşağıdaki yeni API taşıma kontrolün:

-   `Android.Media.Session.MediaSession` &ndash; Birden çok denetleyicisi işleyen bir medya denetim oturumu. Çağırmanız `MediaSession.GetSessionToken` uygulamanızı oturum ile etkileşim kurmak için kullandığı bir belirteç istemek için.

-   `MediaController.TransportControls` &ndash; Tanıtıcıları aktarım komutları gibi **Yürüt**, **durdurmak**, ve **atla**.

Ayrıca, yeni kullanabilirsiniz `Android.App.Notification.MediaStyle` (örneğin, ayıklama ve albüm resmini gösteren) zengin bildirim içerik medya oturumu ilişkilendirmek için sınıf.

Android 5.0 içinde yeni medya kayıttan yürütme denetim özellikleri hakkında daha fazla bilgi için bkz: [medya](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="storage"></a>Depolama

Android 5.0 dizinler ve belgeler çalışmak uygulamaları kolaylaştırmak için depolama erişim Framework güncelleştirir:

-   Bir dizin alt ağacı seçmek için derleme göndermek ve bir `Android.Intent.Action.OPEN_DOCUMENT_TREE` hedefi. Bu amacı alt ağacı seçimi destekler tüm sağlayıcısı örneklerini görüntülemek sistemin neden olur; Kullanıcı gözatar ve bir dizin seçer.

-   Oluşturun ve yeni belgeleri ya da bir alt ağacı altında herhangi bir yere dizinleri yönetmek için yeni kullanmanız `CreateDocument`, `RenameDocument`, ve `DeleteDocument` yöntemlerini `DocumentsContract`.

-   Tüm paylaşılan depolama cihazlarında medya dizinleri yollara almak için yeni çağrı `Android.Content.Context.GetExternalMediaDirs` yöntemi.

Yeni depolama Android 5.0 API'leri hakkında daha fazla bilgi için bkz: [depolama](http://developer.android.com/preview/api-overview.html#Storage).

### <a name="wireless--connectivity"></a>Kablosuz & bağlantısı

Android 5.0 kablosuz ve bağlantı için aşağıdaki API geliştirmeleri ekler:

-   Yeni *birden çok ağ* uygulamaların bulmak ve bir bağlantı yapmadan önce belirli özellikleri ağlarla seçmek olanaklı kılan API'leri.

-   Düşük enerji Bluetooth Çevre birimi hareket edecek bir Android 5.0 aygıt sağlayan Bluetooth yayın işlevselliği.

-   Daha kolay hale NFC geliştirmeleri, diğer aygıtlar ile veri paylaşımı için yakın alan iletişimi işlevini kullanın.

Yeni kablosuz ve bağlantı Android 5.0 API'leri hakkında daha fazla bilgi için bkz: [kablosuz ve bağlantı](http://developer.android.com/preview/api-overview.html#Wireless).

### <a name="job-scheduling"></a>İş zamanlaması

Yeni bir Android 5.0 tanıtır `JobScheduler` kullanıcılara yardımcı olabilecek API yalnızca cihaz prize takılı olduğu durumlar çalıştırmak için belirli görevler zamanlama ve şarj pil boşaltma en aza. Bu iş Zamanlayıcı özellik koşullar aygıt tarifeli ağ yerine bir Wi-Fi ağ üzerinden bağlandığında büyük bir dosya indirme gibi bu görev için daha uygun olduğunda çalışması için bir görev zamanlama için de kullanılabilir.

Yeni iş Android 5.0 API'leri zamanlama hakkında daha fazla bilgi için bkz: [zamanlama işleri](http://developer.android.com/preview/api-overview.html#JobScheduler).

## <a name="summary"></a>Özet

Bu makalede, Android 5.0 önemli yeni özelliklere genel bakış Xamarin.Android uygulaması geliştiricileri için sağlanan:

-   Malzeme tema

-   Animasyon

-   Görünüm gölgeleri ve yükseltme

-   Renk ve özellik, drawable tonlamak gibi belirgin ayıklama renk

-   Yeni `RecyclerView` ve `CardView` pencere öğeleri

-   Bildirim geliştirmeleri

-   Kamera, ses çalma, ortam denetimi, depolama, kablosuz/bağlantı ve iş zamanlama için yeni API'leri

Xamarin Android geliştirme için yeniyseniz, okuma [Kurulum ve yükleme](~/android/get-started/installation/index.md) Xamarin.Android ile çalışmaya başlamanıza yardımcı olacak.
[Merhaba, Android](~/android/get-started/hello-android/index.md) Android proje oluşturmak öğrenme için mükemmel bir giriş değil.



## <a name="related-links"></a>İlgili bağlantılar

- [Android L Developer Preview](http://developer.android.com/preview/index.html)
- [Android SDK'sı Al](https://developer.android.com/sdk/index.html#Other)
- [Malzeme tasarım](http://developer.android.com/preview/material/index.html)
- [Malzeme tasarım ilkeleri](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
