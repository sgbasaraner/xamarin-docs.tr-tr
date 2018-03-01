---
title: "Sorun giderme ipuçları"
ms.topic: article
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ce62e844a9ec76217947c0f0f5ed5e9a81336c7e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

<a name="Getting_Diagnostic_Information" />

## <a name="getting-diagnostic-information"></a>Tanılama bilgileri alınıyor

Xamarin.Android çeşitli hatalar izlerken aramak için birkaç yerler vardır.
Bu güncelleştirmeler şunlardır:

1.  Tanılama MSBuild çıktı.
2.  Cihaz dağıtım günlükleri.
3.  Android hata ayıklama günlük çıktısı.


 <a name="Diagnostic_MSBuild_Output" />


## <a name="diagnostic-msbuild-output"></a>Tanılama MSBuild çıktı

Tanılama MSBuild paket oluşturma ile ilgili ek bilgiler içerebilir ve bazı paket dağıtım bilgileri içerebilir.

Visual Studio tanılama MSBuild çıktısı etkinleştirmek için:

1.  Tıklatın **Araçlar > Seçenekler...**
2.  Soldaki ağaç görünümünde seçin **projeler ve çözümler > derleme ve çalıştırma**
3.  Sağ taraftaki panelinde MSBuild yapı çıktı ayrıntı açılır tanı ayarlayın.
4.  **Tamam**’a tıklayın.
5.  Temizleyin ve paketinizi yeniden derleyin.
6.  Tanılama çıktıları çıkış paneli içinde görünür olur.


Visual Studio tanılama MSBuild çıktısı için Mac/OS X: etkinleştirmek için

1.  Tıklatın **Mac için Visual Studio > tercihleri...**
2.  Soldaki ağaç görünümünde seçin **projeleri > derleme**
3.  Sağ taraftaki panelinde günlük ayrıntı açılan tanı ayarlayın
4.  **Tamam**’a tıklayın.
5.  Mac için Visual Studio'yu yeniden başlatın
6.  Temizleyin ve paketinizi yeniden derleyin.
7.  Tanılama çıktıları hataları paneli içinde görünür olduğundan (**Görünüm > klavye takımı > hataları** ), yapı çıktı düğmesini tıklatarak.


 <a name="Device_Deployment_Logs" />


## <a name="device-deployment-logs"></a>Cihaz dağıtım günlükleri

Visual Studio içinde cihaz dağıtım günlüğü etkinleştirmek için:

1.  **Araçlar > Seçenekler...**>
2.  Soldaki ağaç görünümünde seçin **Xamarin > Android ayarları**
3.  [X] sağ panelinde etkinleştirmek **(monodroid.log yazma masaüstünüze) uzantısı hata ayıklama günlüğünü** onay kutusu.
4.  Günlük iletilerini masaüstünüzde monodroid.log dosyasına yazılır.


Mac için Visual Studio her zaman cihaz dağıtım günlükleri yazar. Bunları bulma biraz daha zordur; bir *AndroidUtils* her gün + oluşan bir dağıtım, örneğin saat için günlük dosyası oluşturulur: **AndroidTools-2012-10-24_12-35-45.log**.

-  Günlük dosyalarına yazılır, Windows'da `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`.
-  Günlük dosyalarına yazılır OS x'te `$HOME/Library/Logs/XamarinStudio-{VERSION}`.


 <a name="Android_Debug_Log_Output" />


## <a name="android-debug-log-output"></a>Android hata ayıklama günlük çıktısı

Android birçok iletiler yazma [Android hata ayıklama günlüğünü](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin.Android Android hata ayıklama günlüğüne ek iletiler nesil denetlemek için Android sistem özelliklerini kullanır. Android Sistem özellikleri ayarlanabilen *setprop* komutunu çalıştırmamanızı [Android hata ayıklama Köprüsü (adb)](http://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Sistem özellikleri işlem başlangıcı sırasında okuma ve böylece ayarlayın ya da uygulama başlatıldığında veya Sistem özellikleri değiştikten sonra uygulama yeniden başlatılmalıdır önce olması gerekir.

<a name="Xamarin.Android_System_Properties" />


### <a name="xamarinandroid-system-properties"></a>Xamarin.Android System Properties

Xamarin.Android aşağıdaki sistem özellikleri destekler:

-   *Debug.Mono.Debug*: boş olmayan bir dize ise, bu eşdeğer olan `*mono-debug*`.

-   *Debug.Mono.env*: kanal ayrılmış ('*|*') uygulama başlatılırken dışarı aktarmak için ortam değişkenleri listesi *önce* mono başlatıldı. Bu ortam değişkenlerini ayarlama mono oturum denetleme sağlar.

    - *Not*: değer olduğundan '*|*'-ayrılmış, değeri tırnak içine almak, bir ek düzeyi olmalıdır olarak \` *adb Kabuk* \` komutu kaldırılır bir teklifleri kümesi.

    - *Not*: Android sistemi özellik değerleri 92 karakterden uzun olamaz.

    - Örnek:

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *Debug.Mono.log*: virgülle ayrılmış ('*,*') Android hata ayıklama günlüğüne ek iletiler yazdırması gereken bileşenlerin listesi. Varsayılan olarak, hiçbir şey ayarlanır. Bileşenleri şunları içerir:

    -   *tüm*: tüm iletileri yazdırma
    -   *GC*: yazdırma GC ilgili iletileri.
    -   *gref*: yazdırma (zayıf, genel) başvurusu ayırma ve ayırmayı kaldırma iletileri.
    -   *lref*: yerel bir referans ayırma ve ayırmayı kaldırma iletilerini yazdırma.

    *Not*: Bunlar *son derece* ayrıntılı. İçin gerçekten gerekli olmadıkça etkinleştirmeyin.

-   *Debug.Mono.Trace*: ayarının izin verdiğinden [mono--izleme](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` ayarı.



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android cannot resolve System.ValueTuple

Visual Studio ile bir uyumsuzluk nedeniyle bu hata oluşur.

- **Visual Studio 2017 güncelleştirme 1** (sürüm 15.1 veya daha eski) ile uyumlu yalnızca **System.ValueTuple NuGet 4.3.0** (veya daha eski).

- **Visual Studio 2017 güncelleştirme 2** (15.2 ya da daha yeni sürümü) uyumlu yalnızca **System.ValueTuple NuGet 4.3.1** (veya daha yeni).

Lütfen Visual Studio 2017 yüklemenizle birlikte karşılık gelen doğru System.ValueTuple NuGet seçin.

<a name="GC_Messages" />

## <a name="gc-messages"></a>GC iletileri

GC bileşen iletileri gc içeren bir değer debug.mono.log sistem özelliği ayarlanarak görüntülenebilir.

GC yürütür ve çalışma hakkında ne kadar GC vermedi bilgileri sağlayan her GC iletileri oluşturulur:

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Zamanlama bilgileri gibi ek GC bilgileri ayarlayarak oluşturulabilir `MONO_LOG_LEVEL` ortam değişkenine `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Bu, bu üç sonucu dahil olmak üzere (çok sayıda) ek Mono iletilerinde, neden olur:

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

İçinde `GC_BRIDGE` ileti `num-objects` köprüsü nesneleri bu geçiş düşünüldüğünde, sayısı ve `num_hash_entries` bu köprüsü kodu çağırma sırasında işlenen nesne sayısı.

İçinde `GC_MINOR` ve `GC_MAJOR` iletileri, `total` world duraklatıldığında zaman miktarıdır (iş parçacığı Yürütülüyor), ancak `bridge` (Java VM ile ilgilenir) kod işleme köprüsü geçen süre miktarı. Dünya *değil* köprüsü işleme gerçekleşirken duraklatıldı.

 *Genel*, büyük değeri `num_hash_entries`, daha fazla zaman `bridge` koleksiyonları alır ve büyük `total` harcanan süre toplama olacaktır.

 <a name="Global_Reference_Messages" />


## <a name="global-reference-messages"></a>Genel başvuru iletileri

Genel başvuru loggig günlüğü'nü (GREF) etkinleştirmek için *debug.mono.log* sistem özelliği içermelidir *gref*, örneğin:

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android Android genel başvurular bir Java örneği Java için sağlanması gereken bir Java yöntemi çağrılırken olarak Java örnekleri ile ilişkili yönetilen Örnekler arasındaki eşlemeleri sağlamak için kullanır.

Ne yazık ki, Android öykünücüsünü yalnızca aynı anda mevcut 2000 Genel başvurular izin verir. Donanım çok daha yüksek bir sınır 52000 genel başvuruları sahiptir. Alt sınırı uygulamaları şekilde bilinmesiyle öykünücüsünde çalıştırırken sorunlu olabilir *burada* gelen örnek gelen çok kullanışlı olabilir.

 *Not*: Genel başvuru sayısı Xamarin.Android için dahili kullanım içindir ve işlem içine yüklenmiş diğer yerel kitaplıkları tarafından gerçekleştirilen genel başvuruları desteklemez ve olamaz içerir. Genel başvuru sayısı tahmini olarak kullanın.

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

Sonuç dört iletileri şunlardır:

-  Genel Başvuru oluşturma: ile başlayan satırlar bunlar *+ g +* , yığın izlemesi oluşturma kodu yolunu sağlar.
-  Genel başvuru yok etme: ile başlayan satırlar bunlar *- g-* , genel başvurusu atma kod yolu için yığın izlemesi sağlayabilir. GC gref siliniyorsa, hiçbir yığın izlemesi sağlanacaktır.
-  Zayıf genel başvuru oluşturma: ile başlayan satırlar bunlar *+ w +* .
-  Zayıf genel başvuru yok etme: ile başlayan satırları bunlar *-w-* .


Tüm iletilerindeki *grefc* değerdir Xamarin.Android oluşturdu, genel başvuru sayısı sırada *grefwc* değeri Xamarin.Android oluşturdu zayıf genel başvurular sayısıdır. *İşlemek* veya *obj tanıtıcı* değerdir JNI tanıtıcı değeri ve sonrasında karakter '  */* ' tanıtıcı değeri türü: */L* yerel bir referans için */G* genel başvuruları için ve */W* zayıf genel başvurular için.

GC işleminin bir parçası olarak genel başvurular (+ g +) zayıf genel başvurular dönüştürülür (neden + w + ve - g-), bir Java tarafı GC başlayacağı zamana ve toplanan olmadığını görmek için zayıf genel başvuru sonra denetlenir. Hala kullanımda değilse yeni bir gref zayıf ref oluşturulur (+ g +, -w-), aksi takdirde zayıf ref yok (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java örneği oluşturulur ve bir MCW tarafından Sarmalanan

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>Bir GC gerçekleştiriliyor...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>Nesne tanıtıcı hala etkin! = null
## <a name="wref-turned-back-into-a-gref"></a>wref gref geri açık

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>Nesnesidir işleyici ölü null ==
## <a name="wref-is-freed-no-new-gref-created"></a>oluşturulan boşaltılmış, yeni gref wref olduğu

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

Burada bir "ilginç" İpucu yoktur: Android 4.0 önce çalışan hedeflerde gref değeri Android zamanının bellekte Java nesne adresine eşittir. (Diğer bir deyişle, GC bir taşıma olmayan, zorlayacağı, Toplayıcı, ve bu nesnelere doğrudan başvuruları gönderdikten.) Bu nedenle sonra bir g +, + w +, - g-, + g +, - w-sırası, sonuçta elde edilen gref özgün gref değeri ile aynı değere sahip olur. Bu günlükler aracılığıyla grepping oldukça basit hale getirir.

Ancak, Android 4.0 taşıma Toplayıcı var ve artık Android çalışma zamanı doğrudan başvuruları VM nesneleri aktarır. Sonuç olarak, sonra bir g +, + w +, - g-, + g +, - w sırası, gref değer *farklı olacaktır*. Nesne birden çok GC'ler devam eder, burada bir örnek gerçekte gelen ayrıldı belirlemek daha zor hale getirme birkaç gref değerlere göre geçer.

### <a name="querying-programmatically"></a>Program aracılığıyla sorgulama

Sorgulayarak GREF ve WREF sayısını sorgulayabilirsiniz `JniRuntime` nesnesi.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` -Genel başvuru sayısı

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -Zayıf başvuru sayısı

 <a name="Offline_Activation" />


## <a name="offline-activation"></a>Çevrimdışı etkinleştirme

Xamarin.Android Windows etkinleştirilemiyor veya Mac OS X üzerinde Xamarin.Android'ın tam sürümünü yüklemek için, bkz: Lütfen [çevrimdışı etkinleştirme](~/android/get-started/installation/index.md) sayfası.

 <a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Indie/iş deneme hesabından yükseltemiyor.

Yakın zamanda Xamarin.Android satın alınan ve daha önce bir Xamarin.Android deneme başlatıldı, Mac veya Visual Studio için Visual Studio tarafından toplanma bu lisans değişiklik almak için aşağıdaki adımları tamamlayın gerekebilir.

-  Mac/Visual Studio için Visual Studio'yu kapatın
-  Tüm dosyaları Mac üzerinde ~/Library/MonoAndroid veya %PROGRAMDATA%\Mono Android\License\ Windows için kaldırma
-  Mac/Visual Studio için Visual Studio'yu yeniden açın ve bir Xamarin.Android projesi oluşturma


Bu çalışır almalısınız. Sorunları yaşamaya devam ederseniz, denemek isteyebilirsiniz bir [çevrimdışı etkinleştirme](~/android/get-started/installation/index.md) istasyonunuzu etkinleştirmeyi tamamlamak için.

 <a name="Receiving_'Activation_Incomplete'_Error_Message" />


## <a name="receiving-activation-incomplete-error-message"></a>Alma ' etkinleştirme tamamlanmamış hata iletisi

Xamarin.Android Visual Studio için kullanırken bu sorun ortaya çıkabilir. Bu sorunu çözmek için lütfen günlükleri şu konuma gönderin  *contact@xamarin.com* .

-  Günlük konumu: **LocalAppData %\\Xamarin\\günlükleri**


 <a name="Receiving_'Error_Retrieving_Update_Information'_Error_Message" />


## <a name="receiving-error-retrieving-update-information-error-message"></a>'Güncelleştirme bilgilerini alma hatası' hata iletisini almaya

Zaman zaman, bir güncelleştirme güncelleştirmeler denetlenirken sıklıkla meydana gelir bu şu hatayla başarısız olur:

Çoğu zaman, bu hata yalnızca Xamarin hesabınıza açarak çözülebilir ve sonra günlük geri.

Bunu başarmak için tercih platformunuz bulmak ve adımları izleyin:

**On Mac:**
1. Mac için açık Visual Studio
2. Mac için Visual Studio seçin > Hesap...
3. Oturumu kapatma tıklatın
4. Oturum Aç tıklatın
5. Kimlik bilgilerinizi girin
6. Güncelleştirmeleri denetle

**Bilgisayarınızda Visual Studo kullanma:**
1. Açık Visual Studio
2. Araçlar > Xamarin hesabı
3. Oturumu kapatma tıklatın
4. Oturum Aç tıklatın
5. Kimlik bilgilerinizi girin
6. Güncelleştirmeleri denetle

Bu hata iletisi görünmeye devam ederse, lütfen e-posta  **contact@xamarin.com** .


 <a name="Android_Debug_Logs" />


## <a name="android-debug-logs"></a>Android hata ayıklama günlükleri

[Android hata ayıklama günlüklerini](~/android/deploy-test/debugging/android-debug-log.md) görmesini çalışma zamanı hataları ile ilgili ek bağlam sağlayabilir.

 <a name="Floating-Point_performance_is_terrible!" />


## <a name="floating-point-performance-is-terrible"></a>Kayan nokta performans korkunç!

Alternatif olarak, "Uygulamam daha hızlı 10 x ile hata ayıklama yapıyla yayın çalıştırır!"

Xamarin.Android destekleyen birden çok aygıt ABIs: *pushservice*, *pushservice-v7a*, ve *x86*. Aygıt ABIs içinde belirtilebilir **proje özellikleri > Uygulama sekmesi > desteklenen mimariler**.

Hata ayıklama derlemelerinde, tüm ABIs sağlar ve böylece hızlı ABI hedef cihaz için kullanacağı bir Android paketini kullanın.

Yayın derlemeleri yalnızca proje özellikleri sekmede seçilen ABIs dahil edilir. Birden fazla seçilebilir.

*pushservice* abı varsayılandır ve geniş cihaz desteğe sahiptir. *Ancak*, pushservice çok CPU aygıtlar ve donanım kayan nokta, amont başka şeyler desteklemiyor. Sonuç olarak, pushservice sürüm çalışma zamanı kullanarak uygulamaları tek bir çekirdek bağlanır ve bir yumuşak float uygulamasını kullanarak. Bunların her ikisi de uygulamanız için önemli ölçüde performans katkıda bulunabilirsiniz.

Uygulamanızı makul kayan nokta performans (örneğin oyunlar) gerektiriyorsa, etkinleştirmelisiniz *pushservice-v7a* ABI. Yalnızca desteklemek istediğiniz *pushservice-v7a* çalışma zamanı, buna karşın yalnızca destekleyen eski aygıtları *pushservice* uygulamanızı çalıştırma mümkün olmayacaktır.

 <a name="Could_not_locate_Android_SDK" />


## <a name="could-not-locate-android-sdk"></a>Android SDK bulunamadı.

2 yüklemeleri Google Android SDK Windows için kullanılabilir.
.Exe yükleyici seçerseniz, yüklendiği Xamarin.Android söyleyin kayıt defteri anahtarlarını yazacaksınız. .Zip dosyasını seçin ve kendiniz sıkıştırmasını açın, Xamarin.Android için SDK'sı nereye bilmez. SDK Visual Studio giderek olduğu Xamarin.Android anlayabilirsiniz **Araçlar > Seçenekler > Xamarin > Android ayarları**:

[![Xamarin Android ayarları Android SDK'sı konumu](troubleshooting-images/01a.png)]()

<a name="IDE_does_not_display_target_device" />


## <a name="ide-does-not-display-target-device"></a>IDE hedef aygıt görüntülenmez

Bazen bir aygıt, ancak değil gösterilen aygıtı Seç iletişim kutusunda dağıtmak istediğiniz aygıt uygulamanızı dağıtmak dener. Bu durum Android hata ayıklama köprüsü tatile karar ortaya çıkar.

Bu sorunu tanılamak için Bul [adb program](~/android/deploy-test/debugging/android-debug-log.md), ardından çalıştırın:

```shell
adb devices
```

Cihazınızı yoksa, Cihazınızı bulunabilir böylece Android hata ayıklama köprüsü sunucuyu yeniden başlatmanız gerekir:

```shell
adb kill-server
adb start-server
```

HTC eşitleme yazılım engelleyebilir **adb start-server** düzgün çalışmasını. Varsa **adb start-server** komutu başlatılıyor üzerindeki hangi bağlantı noktasının yazdırmanız değil, lütfen HTC eşitleme yazılımını çıkın ve adb sunucuyu yeniden başlatmayı deneyin.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>Belirtilen görev yürütülebilir "keytool" çalıştırılamadı

Başka bir deyişle, YOLUNUZU Java SDK'ın bin dizinine bulunduğu dizin içermiyor. Bu adımları izlediğiniz denetleyin [yükleme](~/android/get-started/installation/index.md) Kılavuzu.


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe veya aresgen.exe 1 koduyla çıktı

Bu sorunu hata ayıklama, Visual Studio uygulamasına gidin ve bunu yapmak için MSBuild ayrıntı düzeyi, değiştirmenize yardımcı olmak için seçin: **Araçlar > Seçenekler > Proje** ve **çözümleri > Yapı** ve **çalıştırmak > MSBuild proje derleme çıktı ayrıntı** ve bu değer ayarlanırsa **Normal**.

Yeniden oluşturun ve tam hata içermelidir Visual Studio'nun çıkış bölmesi denetleyin.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>Paketi dağıtmak için cihaz üzerinde yeterli depolama alanı yok

Bu durum, Visual Studio içinde öykünücüsünden başlatma oluşur. Visual Studio dışında öykünücüsü başlatırken geçirmeniz gereken `-partition-size 512` seçenekleri, örn.

```shell
emulator -partition-size 512 -avd MonoDroid
```

Kullandığınız doğru simulator adı, yani olun [simulator yapılandırırken kullandığınız adı](~/android/get-started/installation/windows.md#device).

<a name="INSTALL_FAILED_INVALID_APK_when_installing_a_package" />

## <a name="installfailedinvalidapk-when-installing-a-package"></a>Yükleme\_başarısız\_geçersiz\_bir paket yüklerken APK

Android paketini adları *gerekir* içeren bir nokta ('*.*'). Paket adı, bir süre içerecek şekilde düzenleyin.

-   Visual Studio içinde:
    -   Projenize sağ tıklayın > Özellikler
    -   Sol taraftaki Android derleme bildirimi sekmesini tıklatın.
    -   Paket adı alanını güncelleştirin.
        -   İletiyi görürseniz &ldquo;Hayır AndroidManifest.xml bulundu. Bir eklemek için tıklatın. &rdquo;, bağlantıya tıklayın ve ardından paket ad alanını güncelleştirin.
-   Mac için Visual Studio içinde:
    -   Projenize sağ tıklayın > Seçenekler.
    -   Yapı gidin / Android uygulama bölümü.
    -   Paket adı alanı içerecek şekilde değiştirmeniz bir '.'.


<a name="INSTALL_FAILED_MISSING_SHARED_LIBRARY_when_installing_a_package" />


## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>Yükleme\_başarısız\_eksik\_paylaşılan\_bir paket yüklerken kitaplığı

Bu bağlamda "paylaşılan kitaplık" *değil* yerel paylaşılan bir kitaplık (*libfoo.so*) dosya; bunun yerine ayrı olarak Google haritalar gibi hedef aygıta yüklenmesi gereken bir kitaplık değil.

Android paketini ile hangi paylaşılan kitaplıklar gereklidir belirtir `<uses-library/>` öğesi. Varsa bir *gerekli* kitaplığı, hedef cihazda mevcut değil (örneğin `//uses-library/@android:required` olan *true*, varsayılan), paket yükleme başarısız olur *yükleme\_ BAŞARISIZ\_eksik\_paylaşılan\_Kitaplığı*.

Hangi paylaşılan kitaplıklar gerektiğini belirlemek için görüntüleme *oluşturulan*
**AndroidManifest.xml** dosyası (örneğin **obj\\hata ayıklama\\android \\AndroidManifest.xml**) ve Ara `<uses-library/>` öğeleri. `<uses-library/>` öğeleri el ile eklenebilir, projenizin içinde **özellikleri\\AndroidManifest.xml** dosya ve aracılığıyla [UsesLibraryAttribute özel öznitelik](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/).

Örneğin, bir derleme başvurusu ekleme *Mono.Android.GoogleMaps.dll* örtük olarak ekleyecek bir `<uses-library/>` Google haritalar paylaşılan kitaplık için.

<a name="INSTALL_FAILED_UPDATE_INCOMPATIBLE_when_installing_a_package" />


## <a name="installfailedupdateincompatible-when-installing-a-package"></a>Yükleme\_başarısız\_güncelleştirme\_bir paket yüklerken UYUMSUZ

Android paketleri üç gereksinimlere sahiptir:

-   İçermesi gereken bir '.' (önceki girişine bakın)
-   Benzersiz bir dize paket adı olması gerekir (Bu nedenle Android uygulaması adları, örneğin com.android.chrome Chrome uygulaması için görünen ters tld kuralı)
-   Paketleri yükseltirken, paketin aynı imzalama anahtarı olmalıdır.

Bu nedenle, bu senaryoyu düşünün:

1.  Yapı & uygulamanızı bir hata ayıklama uygulama olarak dağıtma
2.  İmzalama anahtarı örneğin için değiştirdiğiniz sürüm uygulama olarak kullanın (veya ister yoktur çünkü varsayılan tarafından sağlanan imzalama anahtarı hata ayıklama)
3.  İlk kaldırmadan uygulamanızı yüklemek, örneğin hata ayıklama > hata ayıklama olmadan Başlat Visual Studio içinde


Bu durumda, paket yükleme ile bir yükleme başarısız olur\_başarısız\_güncelleştirme\_UYUMSUZ hatası, paket adı imzalarken değişmedi çünkü anahtar vermedi. [Android hata ayıklama günlüğünü](~/android/deploy-test/debugging/android-debug-log.md) da benzer bir ileti içerir:

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Bu hatayı düzeltmek için tamamen uygulama aygıtınızdan yeniden yüklemeden önce kaldırın.

<a name="INSTALL_FAILED_UID_CHANGED_when_installing_a_package" />

## <a name="installfaileduidchanged-when-installing-a-package"></a>Yükleme\_başarısız\_UID\_bir paket yüklerken değiştirildi

Bir Android paketi yüklendiğinde, atanan bir *kullanıcı kimliği* (UID).
*Bazen*, önceden yüklenmiş bir uygulama yüklerken şu anda bilinmeyen nedeniyle, yükleme başarısız `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Bu sorunu çözmek için *tamamen kaldırın* Android paketini Android hedefin GUI uygulamayı yüklemeden veya kullanarak `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**KULLANMAYIN** `adb uninstall -k`bu gibi *korumak* uygulama verilerini ve bu nedenle hedef aygıttaki çakışan UID koruyabilirsiniz.


<a name="Release_apps_fail_to_launch_on_device" />

## <a name="release-apps-fail-to-launch-on-device"></a>Yayın uygulamaları aygıtta başlatılamadı

Android hata ayıklama günlüğünü çıkış içerecek benzer bir ileti:

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

Bu durumda, iki olası nedeni vardır:

1.  .apk hedef aygıt destekleyen bir ABI sağlamaz.
    Örneğin, .apk yalnızca pushservice-v7a ikili dosyaları içerir ve hedef aygıt yalnızca pushservice destekler.

2.  Bir [Android hata](http://code.google.com/p/android/issues/detail?id=21670). Bu durumda, uygulamaları kaldırmak, parmakları arası ve uygulamayı yeniden yükleyin.

(1) düzeltmek için Proje seçenekleri/özelliklerini düzenleyin ve [gerekli ABI desteği desteklenen ABIs listesine eklemek](~/android/app-fundamentals/cpu-architectures.md). Eklemenize gerek hangi ABI belirlemek için hedef Cihazınızı karşı aşağıdaki adb komutu çalıştırın:

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

Çıktı birincil içerir (ve isteğe bağlı ikincil) ABIs.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Proje için OutPath özelliği ayarlanmamış &ldquo;MyApp.csproj&rdquo;

Bu, genellikle bir HP bilgisayar ve ortam değişkeninin sahip anlamına &ldquo;Platform&rdquo; için bir şeyler MCD veya HPD gibi ayarlanmadı. Bu genellikle ayarlamak için MSBuild Platform özelliği ile çakışan &ldquo;herhangi bir CPU&rdquo; veya &ldquo;x86&rdquo;. MSBuild çalışabilmesi için önce bu ortam değişkenini makinenizden kaldırmanız gerekir:

-   Denetim Masası > Sistem > Gelişmiş > ortam değişkenleri

Visual Studio veya Visual Studio Mac için yeniden başlatın ve yeniden deneyin. Şeyler şimdi beklendiği gibi çalışması gerekir.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject için dönüştürülür olamaz...

Xamarin.Android 4.x değil düzgün sıralama iç içe geçmiş genel türler düzgün. Örneğin, aşağıdaki C göz önünde bulundurun\# kullanan kod [SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```


Xamarin.Android yanlış iç içe geçmiş genel türler sıralar sorunudur. `List<IDictionary<string, object>>` İçin sıralanmış bir [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/), ancak `ArrayList` içeren `mono.android.runtime.JavaObject` örnekleri (hangi başvuru `Dictionary<string, object>` örnekleri) yerine uygulayanbirşey[java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/), sonuçta elde edilen aşağıdaki özel durum:

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

Geçici çözüm sağlanan kullanmaktır [Java koleksiyon türleri](~/android/internals/api-design.md) yerine `System.Collections.Generic` için türleri &ldquo;iç&rdquo; türleri. Bu örnekleri hazırlama zaman uygun Java türlerinde sonuçlanır. (Aşağıdaki kod gref yaşam süresi azaltmak için gerekenden daha karmaşıktır. Özgün kodu aracılığıyla değiştirmeye basitleştirilebilir `s/List/JavaList/g` ve `s/Dictionary/JavaDictionary/g` gref yaşam süreleri bir endişelenmeyin değilseniz.)

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[Bu bir sonraki sürümde düzeltilecektir](https://bugzilla.xamarin.com/show_bug.cgi?id=5401).

<a name="Unexpected_NullReferenceExceptions" />

## <a name="unexpected-nullreferenceexceptions"></a>Beklenmeyen NullReferenceExceptions

Bazen [Android hata ayıklama günlüğünü](~/android/deploy-test/debugging/android-debug-log.md) NullReferenceExceptions Bahsediyor, &ldquo;durum olamaz&rdquo; veya uygulama bitmeden önce kısa bir süre içinde Android çalışma zamanı kodu için Mono gelebilir:

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

veya

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

Herhangi bir şey yaparken veya hedefin GREF sınırına ulaşması nedenlere sayıda oluşabilir işlemini iptal etmek Android çalışma zamanı karar bu ortaya çıkar &ldquo;yanlış&rdquo; JNI ile.

Bu durumda olup olmadığını görmek için iletiye benzer şekilde, işleminden Android hata ayıklama günlüğü denetleyin:

```shell
E/dalvikvm(  123): VM aborting
```

<a name="Abort_due_to_Global_Reference_Exhaustion" />

## <a name="abort-due-to-global-reference-exhaustion"></a>Genel başvuru Tükenme nedeniyle iptal

Android zamanının JNI katmanı yalnızca JNI nesne başvuruları zamanında belirli bir anda geçerli olması için sınırlı sayıda destekler. Bu sınırı aşıldığında şeyler bölün.

GREF (*genel başvuru*) sınır öykünücü 2000 başvurularında, ~ 52000 donanımda başvuruyor.

Bu gibi Android hata ayıklama günlüğü iletileri gördüğünüzde çok fazla GREFs oluşturmak başlatıyorsanız bildiğiniz:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

GREF sınırına ulaştığında, aşağıdaki gibi bir ileti yazdırılır:

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```


Yukarıdaki örnekte (; Bu arada, gelen [hata 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)) sorun örnekleri oluşturuluyor çok fazla Android.Graphics.Point olan; bkz [açıklama \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) düzeltmelerin listesi Bu belirli hatayı.

Genellikle, hangi tür çok fazla sayıda örneğe sahip bulmak için kullanışlı bir çözüm olan ayrılmış &ndash; yukarıdaki döküm Android.Graphics.Point &ndash; , kaynak kodu ve bunların dispose uygun şekilde oluşturuldukları Bul (böylece kendi Java-Object ömrü kısaltılmış). Bu her zaman uygun değildir (\#Önemsiz çözüm Dispose çağrısı önler şekilde 685215 birden çok iş parçacıklı,), ancak dikkate almanız gereken ilk şey değil.

Etkinleştirebilirsiniz [GREF günlüğü](~/android/troubleshooting/index.md) GREFs ne zaman oluşturulduğunu ve kaç tane var görmek için.

<a name="Abort_due_to_JNI_type_mismatch" />

## <a name="abort-due-to-jni-type-mismatch"></a>JNI tür uyuşmazlığı nedeniyle iptal

Elle-Top JNI kodu varsa, türleri doğru örneğin eşleşmeyecektir mümkündür çağrılacak çalışırsanız `java.lang.Runnable.run` uygulamaz türündeki `java.lang.Runnable`. Bu durumda olacaktır bir ileti Android hata ayıklama günlüğüne şuna benzer:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Dinamik kod desteği

### <a name="dynamic-code-does-not-compile"></a>Dinamik kodu derlenmiyor

C kullanılacak\# System.Core.dll, Microsoft.CSharp.dll ve Mono.CSharp.dll projenize eklemek zorunda dinamik uygulama ya da kitaplık.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>Yayın derleme MissingMethodException çalışma zamanında dinamik kodunu oluşur.

-   Uygulama projesi System.Core.dll, Microsoft.CSharp.dll veya Mono.CSharp.dll başvuruları yok olasıdır. Bu derlemeler başvurulduğundan emin olun.

    -   Bu dinamik kodu her zaman maliyetleri unutmayın. Verimli kod gerekiyorsa, dinamik kodu kullanmayan göz önünde bulundurun.

-   Her derlemesindeki türler açıkça uygulama kodu tarafından kullanılmadığı sürece ilk Önizleme'de, bu derlemeler dışlandı. Geçici bir çözüm için aşağıdakilere bakın: [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Uygulama Nesne AĞACI + LLVM kilitlenme ile x86 üzerinde oluşturulan projeler cihazları

İle oluşturulmuş bir uygulama dağıtırken [Uygulama Nesne AĞACI + LLVM](~/android/deploy-test/release-prep/index.md) x86 tabanlı cihazlarda, aşağıdakine benzer bir özel durum hata iletisi görebilirsiniz:

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

Bu bilinen bir sorun bildirilen aynıdır [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111). Geçici çözüm LLVM devre dışı bırakmaktır.
