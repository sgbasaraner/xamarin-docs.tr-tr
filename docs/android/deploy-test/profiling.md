---
title: Android uygulamaları profil oluşturma
description: Bu kılavuz, profil oluşturucu araçlar performans ve Android uygulaması bellek kullanımını incelemek için nasıl kullanılacağını açıklar.
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C823FEE-A6F6-4C31-9EB6-E51407A2FD8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/03/2018
ms.openlocfilehash: fd9ebc7922428d2779e6985379c3118274a46aff
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066669"
---
# <a name="profiling-android-apps"></a>Android uygulamaları profil oluşturma

Bir uygulama mağazasının Uygulamanızı dağıtmadan önce tanımlamak ve herhangi bir performans sorunları, aşırı bellek kullanım sorunları veya ağ kaynaklarına verimsiz kullanımı gidermek önemlidir. Bu amaca hizmet iki profil oluşturucu Araçlar kullanılabilir:

-  Xamarin Profiler 
-  Android Studio'da Android Profil Oluşturucu

Bu kılavuz, Xamarin profil oluşturucu tanıtır ve Android Profiler kullanarak ile çalışmaya başlama hakkında ayrıntılı bilgi sağlar.

 
## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin profil oluşturucu Visual Studio ve Visual Studio ile Mac için Xamarin uygulamaları IDE içinden profil oluşturma için tümleştirilmiş başına bir uygulamadır. Xamarin profil oluşturucu kullanma hakkında daha fazla bilgi için bkz: [Xamarin profil oluşturucu](~/tools/profiler/index.md).

> [!NOTE]
> Olması gereken bir [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) abone Mac için Xamarin profil oluşturucu özellik ya da Visual Studio Enterprise Windows ya da Visual Studio kilidini açmak için
 
## <a name="android-studio-profiler"></a>Android Studio Profil Oluşturucu

Android Studio 3.0 ve sonraki, Android Profil Oluşturucu aracı içerir. Visual Studio ile yerleşik bir Xamarin Android uygulaması performansını ölçmek için Android profil oluşturucu kullanabilirsiniz &ndash; Visual Studio Enterprise lisans gerek kalmadan. Ancak, Xamarin profil oluşturucu aksine Android profil oluşturucu Visual Studio ile tümleşik olmayan ve önceden oluşturulmuş ve Android profil oluşturucu alınan bir Android uygulama paketi (APK) profili için yalnızca kullanılabilir.

### <a name="launching-a-xamarin-android-app-in-android-profiler"></a>Android profil oluşturucu bir Xamarin Android uygulamasını başlatma

Aşağıdaki adımlar, bir Xamarin Android uygulamasını Android Studio'nun Android profil oluşturucu aracında başlatma açıklanmaktadır. Aşağıdaki örnek ekran görüntüleri, Xamarin Forms içinde [XamagonXuzzle](https://developer.xamarin.com/samples/mobile/LivePlayer/XamagonXuzzleLP/) uygulama oluşturulur ve Android Profiler kullanarak profili:

1.  Android projesinde derleme seçenekleri, devre dışı bırakma **kullanım çalışma zamanı paylaşılan**. Bu, Android uygulama paketi (APK) paylaşılan geliştirme zamanı Mono çalışma zamanı bağımlılık yerleşik sağlar.

    ![Çalışma zamanı paylaşılan kullanımı devre dışı bırakma](profiling-images/vswin/01-turn-off-shared-runtime.png)

2.  Uygulama için yapı **hata ayıklama** ve bir fiziksel cihaz veya öykünücü dağıtın. Bu bir imzalı neden **hata ayıklama** oluşturulacak APK sürümü.
    İçin **XamagonXuzzle** örnek, elde edilen APK adlandırılan **com.companyname.XamagonXuzzle Signed.apk**.

3.  Proje klasörünü açın ve gidin **bin/Debug**. Bu klasörde bulun **Signed.apk** uygulamasının sürümü ve (örneğin, Masaüstü) kolayca erişilebilir bir yere kopyalayın. Aşağıdaki ekran görüntüsü, APK **com.companyname.XamagonXuzzle Signed.apk** bulunan ve masaüstüne kopyalanır:

    [![Hata ayıklama konumunu APK dosya imzalanmış](profiling-images/vswin/02-locating-the-debug-apk-sml.png)](profiling-images/vswin/02-locating-the-debug-apk.png#lightbox)

4.  Android Studio'yu başlatın ve seçin **profili ya da hata ayıklama APK**:

    ![Profil Oluşturucu Android Studio Başlat ekranından başlatılıyor](profiling-images/vswin/03-android-studio.png)

5.  İçinde **APK Dosya Seç** iletişim kutusunda, yerleşik ve daha önce kopyaladığınız APK gidin. APK tıklatıp **Tamam**: 
    
    ![APK APK Dosya Seç iletişim kutusunda seçme](profiling-images/vswin/04-select-apk-dialog.png)

6.  Android Studio APK ve dissassembles yükler **classes.dex**:

    ![APK ayarlama](profiling-images/vswin/05-setting-up-the-apk.png)

7.  APK yüklendikten sonra Android Studio APK için aşağıdaki proje ekranı görüntüler. Uygulama adı seçin ve sol ağaç görünümünde sağ **açık modülü ayarları**:

    [![Açık modülü ayarları menü öğesi konumu](profiling-images/vswin/06-open-module-settings-sml.png)](profiling-images/vswin/06-open-module-settings.png#lightbox)

8.  Gidin **proje Ayarları > modülleri**seçin **-imzalı** uygulama düğümünün ardından  **&lt;Hayır SDK&gt;**:

    [![SDK ayarına gezinme](profiling-images/vswin/07-project-settings-modules-sml.png)](profiling-images/vswin/07-project-settings-modules.png#lightbox)

9.  İçinde **modülü SDK** aşağı açılır menüsünde, uygulamanızı oluşturmak için kullanılan Android SDK düzeyini seçin (Bu örnekte, API düzeyi 26 oluşturmak için kullanılan **XamagonXuzzle**):

    [![Proje SDK'sı düzeyini belirleme](profiling-images/vswin/08-project-sdk-level-sml.png)](profiling-images/vswin/08-project-sdk-level.png#lightbox)

    Tıklatın **Uygula** ve **Tamam** bu ayarı kaydetmek için.

10. Profil oluşturucu araç simgesinden başlatın:

    [![Profil oluşturucu araç simgenin konumunu](profiling-images/vswin/09-launch-profiler-sml.png)](profiling-images/vswin/09-launch-profiler.png#lightbox)

11. Çalışan/tıklatın ve uygulama profil oluşturma için dağıtım hedefini seçin **Tamam**. Dağıtım hedefi bir fiziksel aygıt ya da bir öykünücü çalıştıran sanal bir cihaz olabilir. Bu örnekte, Nexus 5 X cihaz kullanılır:

    ![Dağıtım hedef seçme](profiling-images/vswin/10-select-deployment-target.png)

12. Profil Oluşturucu başlatıldıktan sonra dağıtım cihaz ve uygulama işlemi bağlanmak için de birkaç saniye sürer. APK yükleme sırasında Android Profil Oluşturucusu rapor eder **hiçbir bağlı aygıtları** ve **debuggable hiçbir işlem**.

    [![Profil Oluşturucu APK yükler](profiling-images/vswin/11-no-connected-devices-sml.png)](profiling-images/vswin/11-no-connected-devices.png#lightbox)

13. Birkaç saniye sonra Android profil oluşturucu tamamlamak APK yükleme ve cihaz adını ve profili oluşturuluyor uygulama işlemin adını raporlama APK başlatma (Bu örnekte, **LGE Nexus 5 X** ve  **com.companyname.XamagonXuzzle**sırasıyla):

    [![Profil Oluşturucu penceresi sonra Başlat](profiling-images/vswin/12-profiler-starts-sml.png)](profiling-images/vswin/12-profiler-starts.png#lightbox)

14. Aygıt ve debuggable işlem tanımlanır sonra uygulama profil Android profil oluşturucu başlar:

    [![Çalışan uygulamaya profil oluşturucu görüntüler](profiling-images/vswin/13-profiler-running-sml.png)](profiling-images/vswin/13-profiler-running.png#lightbox)

15. Dokunduğunuzda, **rasgele** düğmesini **XamagonXuzzle** (neden olan kaydırma ve döşeme rasgele yap), uygulamanın rastgele aralığında artırmak CPU kullanımı görürsünüz:

    [![RASGELE düğmesi dokunduğunuz, CPU kullanımı](profiling-images/vswin/14-tap-randomize-sml.png)](profiling-images/vswin/14-tap-randomize.png#lightbox)


### <a name="using-the-android-profiler"></a>Android profil oluşturucu kullanma

Android profil oluşturucu kullanma hakkında ayrıntılı bilgi yer almaktadır [Android Studio belgeleri](https://developer.android.com/studio/profile/android-profiler.html).
Aşağıdaki konular, Xamarin Android geliştiricileri için ilgi olacaktır:

-   [CPU profil oluşturucu](https://developer.android.com/studio/profile/cpu-profiler.html) &ndash; gerçek zamanlı uygulamanın CPU kullanımı ve iş parçacığı etkinliği incelemek açıklanmaktadır.

-   [Bellek profil oluşturucu](https://developer.android.com/studio/profile/memory-profiler.html) &ndash; uygulamanın bellek kullanımı gerçek zamanlı bir grafiği görüntüler ve kayıt bellek ayırmaları çözümleme için bir düğmeye içerir.

-   [Profil Oluşturucu ağ](https://developer.android.com/studio/profile/network-profiler.html) &ndash; gönderilen ve uygulama tarafından alınan veriler gerçek zamanlı ağ etkinliğini görüntüler.
