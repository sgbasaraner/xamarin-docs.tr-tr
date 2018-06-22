---
title: Visual Studio Android öykünücüsü
description: Bu kılavuz, yapılandırmak ve Visual Studio 2015'te Xamarin.Android uygulamaları geliştirmek için Visual Studio Android öykünücüsü kullanma açıklanmaktadır.
ms.prod: xamarin
ms.assetid: CD128CB9-499F-4558-B49F-77248824EFDF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/30/2018
ms.openlocfilehash: 29e35d0dee614d28eed08fbe8799fc74c5ad1eba
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436995"
---
# <a name="visual-studio-android-emulator"></a>Visual Studio Android öykünücüsü

_Bu kılavuz, yapılandırmak ve Visual Studio 2015'te Xamarin.Android uygulamaları geliştirmek için Visual Studio Android öykünücüsü kullanma açıklanmaktadır._

## <a name="visual-studio-android-emulator-overview"></a>Visual Studio Android öykünücüsü'ne genel bakış

Microsoft Visual Studio 2015 bir Xamarin.Android uygulaması hata ayıklama için hedef olarak kullanılabilir bir Android öykünücü içerir: *Android için Visual Studio öykünücüsü*. Bu öykünücüsü daha hızlı başlatma ve Android SDK'sı ile birlikte gelen varsayılan öykünücü daha yürütme sürelerinin sonuçta geliştirme bilgisayarınıza Hyper-V yeteneklerini kullanır. Android için Visual Studio öykünücüsü varsayılan Android SDK öykünücüsü alternatif olarak bir Xamarin.Android uygulaması geliştirilirken kullanılır.

> [!NOTE]
> Visual Studio Android öykünücüsü yalnızca Visual Studio 2015 ile uyumlu olan &ndash; Visual Studio 2017 ile çalışmaz.

Bu kılavuz, uygulamanızı test etmek için Visual Studio'dan Microsoft Android öykünücüsü başlatmak nasıl açıklar ve öykünücüsünde kullanılabilen çeşitli özellikleri açıklar. Seçilecek öğreneceksiniz *aygıt profilleri* (varsayılan Android SDK öykünücüsü aygıt tanımlarında benzer) Android cihazları farklı türlerde benzetimini yapmak için. Son olarak, bir sorun giderme bölümü, ortak Tuzaklar ve geçici çözümler açıklanmaktadır.

## <a name="requirements"></a>Gereksinimler

Öykünücü çalıştırmak için Hyper-V çalıştırmak için gereksinimleri karşılaması gerekir. Hyper-V, Windows 8, Windows 8.1, Windows 10 veya daha yüksek Pro sürümü 64-bit sürümünü gerektirir. Gereksinimleri hakkında daha fazla bilgi için bkz: [Android için Visual Studio öykünücüsü sistem gereksinimleri](https://msdn.microsoft.com/en-us/library/mt228280.aspx).

> [!NOTE]
> (Android SDK öykünücüsü tarafından kullanılan) HAXM kullanamazsınız Hyper-V etkinken. HAXM ile ilgili olası sorunları ve sınırlamaları hakkında daha fazla bilgi için bkz: [HAXM sanallaştırma çakışmaları](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#virt-conflicts).


## <a name="running-the-emulator"></a>Öykünücüsünün çalışır durumda

Visual Studio birkaç önceden yapılandırılmış hedef aygıt profilleri kullanılabilir yapar **hata ayıklama hedefi** açılır menü (olarak aşağıdaki ekran görülen). Microsoft Android öykünücüsü hedefleri ile başlayan **VS öykünücüsü**:

[![Önceden yapılandırılmış hedef aygıt profilleri](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs-sml.png)](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs.png#lightbox)

Visual Studio bir Xamarin.Android uygulaması başlatıldığında öykünücü ile seçilen aygıt hedef başlatılır ve öykünücüsünü uygulamanın dağıtıldığı. Öykünücü Başlatılmakta olan Visual Studio belirten, sol alt köşedeki içinde bir ileti görüntülenir:

[![VS öykünücü başlatma](visual-studio-android-emulator-images/02-emulator-starting-vs-sml.png)](visual-studio-android-emulator-images/02-emulator-starting-vs.png#lightbox)

Bir başlatma gecikmesi sonra solda gösterildiği gibi öykünücüsü ekranı görüntülenir. Kilit simgesi yukarı cihazın kilidini açmak için ekranda sürükleyin.
Xamarin.Android uygulaması sonra öykünücüsünde sağ tarafta gösterildiği gibi çalıştırması gerekir:

[![Öykünücü ekran görüntüleri](visual-studio-android-emulator-images/03-first-screen-vs-sml.png)](visual-studio-android-emulator-images/03-first-screen-vs.png#lightbox)

Varsayılan Android SDK öykünücüsü olduğu gibi ile kodunda kesme noktalarını ayarlayın, değişkenleri inceleyin ve çağrı yığınını görüntülemek mümkündür. Öykünücü sağındaki dikey araç öykünücüsü özelliklerine erişim sağlar:

[![Dikey araç çubuğundaki düğmeler](visual-studio-android-emulator-images/04-vertical-toolbar-vs-sml.png)](visual-studio-android-emulator-images/04-vertical-toolbar-vs.png#lightbox)

Aşağıdaki listede her dikey araç çubuğunda işlevi özetlenmektedir:

-   **Kapat** &ndash; öykünücüsü uygulama kapatır. Bu düğme genellikle kullanılmaz &ndash; genellikle öykünücü sol sonra ilk kez başlatıldığında (öykünücüsü yeniden başlatma gecikmesi önlemek için) çalıştıran ve yalnızca zaman artık gerekli kapalı.

-   **Simge Durumuna Küçült** &ndash; çalıştıran öykünücüsü bırakır, ancak görev çubuğuna en aza indirir.

-   **Güç** &ndash; aygıt açma ve kapatma Simulates. (Öykünücü çalışmaya devam eder.)

-   **Çok dokunma** &ndash; yer paylaşımları cihazda birkaç nokta görüntülemek noktalar çimdik ve yakınlaştırma için touch gibi bu eylemi.
    Bir nokta sürükleyerek iki parmak taşıma benzetimi ters yönde taşımak diğer nokta neden olur.

-   **Fare giriş noktası tek** &ndash; aygıt tek nokta girişine (çoklu dokunma girişi kullandıktan sonra) döndürür.

-   **Sol/Döndür sağa döndürün** &ndash; yardımcı test uygulama yönlendirme değişiklikleri nasıl yanıt vereceğini. Örneğin, ilk kez *Sola Döndür* düğmesine tıklandığında, öykünücü yatay moduna geçiş yapar. Zaman *Sağa Döndür* düğmesi dikey moduna geri dönmek için öykünücüsü basılı.

-   **Ekrana Sığdır** &ndash; böylece Masaüstü ekranda sığar öykünücüsü ekran boyutunu büyütür.

-   **Yakınlaştırma** &ndash; % 33, % 50, % 66 ', % 100 veya bazı özel yüzdeyle öykünücüsü ekran ölçeklendirir.


*Ek araçlar* düğmesi öykünücü ek özelliklerini görüntüler iletişim açılır görüntüler:

[![Ek araçlar iletişim](visual-studio-android-emulator-images/05-additional-tools-vs-sml.png)](visual-studio-android-emulator-images/05-additional-tools-vs.png#lightbox)


Her bir ek özellik iletişim kutusunun üstündeki sekmeler satırının kullanılabilir:

-   **Accelerometer** &ndash; aygıt taşıma 3B uzaydaki benzetimini yapar.

-   **Konum** &ndash; seçin ve bir GPS konum benzetimini yapmak için kullanılan bir harita gösterir. Bu harita üzerinde *eşleme noktaları* konum arasında taşıması benzetimi için oluşturulabilir.

-   **Pil** &ndash; ücret miktarını benzetimini yapmak için bir kaydırıcıyı sola pil sağlar.

-   **Ekran** &ndash; bu sekmede **yakalama** bir ekran görüntüsü alır ve anlık bir önizleme görüntüler düğmesi. *Kaydetmek* düğmesi ekran Kaydet.

-   **Kamera** &ndash; Simulates ana bilgisayarınızda sabit animasyonlu bir görüntü olan bir dosya veya eklenmiş bir Web kamerası resim aracılığıyla resim alma. Ön veya arka kameralar seçmek mümkündür.

-   **SD kart** &ndash; , ana bilgisayardaki bir klasöre öykünücü cihaz SD kartı olarak için kullanılabilir hale getirebilirsiniz.
    Uygulama okur ve dosyaları benzetimli SD kartına yazma olduğunda, bunlar doğrudan masaüstünden kullanmadan erişilip `adb` komutu.

-   **Ağ** &ndash; (öykünücü ana bilgisayarın ağ bağlantısı yeniden kullanır) öykünücüsü'nın ağ ayarlarını özetini görüntüler.

Bu özellikler kullanma hakkında daha fazla bilgi için bkz: [Android için giriş Visual Studio öykünücüsü](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/).



## <a name="configuring-device-profiles"></a>Cihaz profillerini yapılandırma

Microsoft Android öykünücüsü en popüler Android sürümleri, ekran boyutlarına ve pazara Android aygıtların donanım özelliklerini temsil eden aygıt profilleri kümesi içerir. Ayrıca, bu cihaz profillerini KitKat, Lolipop ve Marshmallow gibi çeşitli Android sürümleri için zaten yapılandırılmış.

*Öykünücü yöneticisini* yüklemek, kaldırmak ve cihaz profillerini başlatmak için kullanılır. Gelen **Araçları** menüsünde, select **Android için Visual Studio öykünücüsü...**  bu ekran görüntüsünde gösterildiği gibi:

[![Araçlar menüsünden öykünücü başlatma](visual-studio-android-emulator-images/06-launch-emulator-manager-vs-sml.png)](visual-studio-android-emulator-images/06-launch-emulator-manager-vs.png#lightbox)

Bu açılır **aygıt profilleri** iletişim. Yüklü profiller aygıt profili listesinin başında vurgulanır. Yüklü olmayan (ancak yükleme için kullanılabilir) profilleri gri:

[![Cihaz profillerinin simgeler](visual-studio-android-emulator-images/07-device-profiles-vs-sml.png)](visual-studio-android-emulator-images/07-device-profiles-vs.png#lightbox)

Yeni bir profil yüklemek için profili yükleme simgesi (bir aşağı işaret ok yukarıdaki ekran görüntüsü gösterildiği gibi) tıklayın. Örneğin, profili yükleme simgesini tıkladığınızda **5.7" Marshmallow (6.0.0) XHDPI telefon**, aşağıda gösterildiği gibi öykünücü yöneticisini profilini indirir:

[![Profilleri yükleme örneği](visual-studio-android-emulator-images/08-downloading-profile-vs-sml.png)](visual-studio-android-emulator-images/08-downloading-profile-vs.png#lightbox)

Cihaz profili yüklendikten sonra profilin başarıyla yüklendiğini belirtmek için vurgulanır. Tıklatarak *ayrıntıları göster* simgesine platform türü, cpu mimarisi, ekran boyutu/çözünürlüğü ve kullanılabilir bellek cihaza görüntülenir:

[![Cihaz profili ayrıntıları göster](visual-studio-android-emulator-images/09-show-details-vs-sml.png)](visual-studio-android-emulator-images/09-show-details-vs.png#lightbox)

Zaman Visual Studio **hata ayıklama hedefi** açılır menü açıldığında, yeni yüklenmiş aygıt profili hedef olarak kullanılabilir:

[![Hedef açılan menüsünde Yeni profili](visual-studio-android-emulator-images/10-debug-target-vs-sml.png)](visual-studio-android-emulator-images/10-debug-target-vs.png#lightbox)

Bu liste tıklayarak kısaltılmış **bu profili Kaldır** içinde *öykünücü yöneticisini* kullanılmayan cihaz profillerini kaldırmak için. Şu anda bu öykünücüsünde özelleştirilmiş cihaz profili oluşturmak için hiçbir yolu yoktur.


## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde bazı yaygın hatalar ve geçici çözümler kullanırken açıklanmaktadır *Android için Visual Studio öykünücüsü* Xamarin.Android ile.

<a name="cant_connect" />

### <a name="emulator-will-not-start"></a>Öykünücü başlatılamaz

Ana bilgisayar işlemci ve Hyper-V sanal makine arasında incompabilities varsa bazı durumlarda, öykünücü başlatılmaz. Bu sorunu çözmek için bir sanal makinenin sahip olabileceği işlemci özelliklerini sınırlayın için Hyper-V yapılandırma &ndash; bu sanal makinenin farklı ana bilgisayar işlemcilerle uyumluluğu geliştirir.
Bu değişikliği yapmak için aşağıdaki adımları kullanın:

1.  Tıklatın **Başlat** düğmesini tıklatın, yazın **MMC**ve basın **Enter**. Tıklatın **Hyper-V Yöneticisi'ni** aşağıda gösterildiği gibi:

    [![Hyper-V Yöneticisi](visual-studio-android-emulator-images/15-launch-hyperv-manager.png)](visual-studio-android-emulator-images/15-launch-hyperv-manager.png#lightbox)

2.  Hyper-V Yöneticisi'nde **sanal makineleri** öykünücü bölmesinde, sağ tıklatın, kullanma ve tıklatın düzenlemek için **ayarları...** ":

    [![Sanal makineler ayarları menü öğesi](visual-studio-android-emulator-images/16-vm-settings.png)](visual-studio-android-emulator-images/16-vm-settings.png#lightbox)

3.  Ayarları penceresinde bulun **Uyumluluk** bölümüne (altında **donanım > İşlemci**) ve etkinleştirme **farklı işlemci sürümüylefizikselbilgisayarageçiş**:

    [![İşaretli seçeneği geçirme](visual-studio-android-emulator-images/17-set-compatibility-vs-sml.png)](visual-studio-android-emulator-images/17-set-compatibility-vs.png#lightbox)

4.  Tıklatın **Tamam** ve Hyper-V Yöneticisi'ni penceresini kapatın.



### <a name="app-deploys-and-starts-but-fails-immediately"></a>Uygulama dağıtır ve başlar ancak hemen başarısız olur

Bu durumda, öykünücüyü başlatır, uygulamayı başarıyla öykünücüsünü dağıtır ve uygulamayı başlatır. Ancak, uygulama hemen başarısız olur.
Çoğu durumda, bu da ana bilgisayar işlemci ve Hyper-V sanal makine arasında incompabilities kaynaklanır. Bu hatayı gidermek için'ndaki yönergeleri izleyin [öykünücüsü başlatılmayacak](#cant_connect) (yukarıda).


### <a name="emulator-stops-with-the-diagnostic-message-libaot-mscorlibdllso-not-found"></a>Öykünücü tanılama iletiyle durdurur: **libaot mscorlib.dll.so bulunamadı**

Bu hatayı gidermek için hızlı dağıtım devre dışı bırakmak için aşağıdaki adımları kullanın:

1.  ' Ndaki yönergeleri izleyin [öykünücüsü başlatılmayacak](#cant_connect) (yukarıda).

2.  Proje çift **özellikleri**.

3.  Tıklatın **Android seçenekleri** ve işaretini **kullanım hızlı dağıtım (yalnızca hata ayıklama modu)**:

    [![Hızlı Dağıtım seçeneğini işaretli kullanın](visual-studio-android-emulator-images/18-fast-deployment-vs-sml.png)](visual-studio-android-emulator-images/18-fast-deployment-vs.png#lightbox)



### <a name="drag-and-drop-does-not-work"></a>Sürükle ve Bırak çalışmıyor

Varsa *Android için Visual Studio öykünücüsü* yönetici olarak başlatıldı (ya da Visual Studio Yönetici ayrıcalığıyla çalışırken, ondan Visual Studio'dan Başlat varsa), sürükleyip. APK veya. ZIP may iş dosyaları değil. Bu sorunu geçici olarak çözmek için çalıştırın *Android için Visual Studio öykünücüsü* yükseltilmiş izinler olmadan (yani, yönetici olarak değil).


### <a name="other-errors"></a>Diğer hatalar

Yukarıdaki sorun giderme ipuçları, Visual Studio Android öykünücüsü Xamarin.Android ile kullanırken en sık karşılaşılan kapsar. Visual Studio Android öykünücüsü sorun giderme için daha kapsamlı kılavuzu için bkz: [Android için Visual Studio öykünücüsü sorun giderme](https://msdn.microsoft.com/en-us/library/mt228282.aspx).



## <a name="summary"></a>Özet

Bu makalede sunulan *Android için Visual Studio öykünücüsü*; öykünücü Xamarin.Android uygulamaları Visual Studio'da hata ayıklama için nasıl kullanılacağı açıklanmıştır, dikey araç çubuğundaki düğmeler işlevleri açıklanan ve bunu sağlayan bir uygulamasında kullanılabilen özellikleri kısa genel bakış **ek araçlar** iletişim. Nasıl kullanılacağı açıklanmıştır *öykünücü yöneticisini* yüklemek için kaldırmak ve cihaz profillerini başlatın. A *sorun giderme* bölümü, ortak sorunları ve geçici çözümler öykünücü kullanırken açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android için Visual Studio Öykünücüsü](https://www.visualstudio.com/vs/msft-android-emulator/)
- [Android için Visual Studio öykünücüsü Tanıtımı](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/)
