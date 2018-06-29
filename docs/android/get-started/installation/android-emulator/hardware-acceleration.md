---
title: Donanım hızlandırma öykünücüsü performansı
description: Bu makalede, bilgisayarınızın donanım hızlandırma özelliklerinden Android öykünücüsü performansını en üst düzeye çıkarmak için nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: c2bef2c614d4cc0655deb9732ccefec223a8318a
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066500"
---
# <a name="hardware-acceleration-for-emulator-performance"></a>Donanım hızlandırma öykünücüsü performansı

_Bu makalede, bilgisayarınızın donanım hızlandırma özelliklerinden Android öykünücüsü performansını en üst düzeye çıkarmak için nasıl kullanılacağı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Visual Studio test ve Xamarin.Android uygulamalarına bir Android cihaz kullanılamıyor veya pratik olduğu durumlarda Android öykünücüsü kullanarak hata ayıklama geliştiricilere kolaylaştırır.
Donanım hızlandırma çalıştırılan bilgisayarda kullanılabilir değilse, ancak çok yavaş Android öykünücüsünde çalışır. İki sanallaştırma teknolojilerini biri ile birlikte bu hedef x86 donanım özel sanal cihaz görüntüleri kullanarak Android öykünücüsünde performansını önemli ölçüde artırabilir:

1. **Microsoft'un Hyper-V ve hiper yönetici platformuna**. Hyper-V sanallaştırılmış bilgisayar sistemleri bir fiziksel ana bilgisayarda çalıştırmak mümkün kılan Windows sanallaştırma özelliğidir. Android öykünücüsünde hızlandırmaya için kullanılacak önerilen sanallaştırma teknolojisi budur. Hyper-V hakkında daha fazla bilgi için bkz: [Hyper-V Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).

2. **Intel'in donanım hızlandırılmış yürütme Yöneticisi'ni (HAXM)**. 
   HAXM Intel CPU çalıştıran bilgisayarlar için sanallaştırma altyapısıdır.
   Bu işlev, Hyper-V çalıştırılamıyor olan bilgisayarlar için önerilen sanallaştırma altyapısıdır.

Android öykünücüsünde otomatik hale getirir donanım hızlandırmasını aşağıdaki ölçütler karşılanıyorsa kullanın:

-   Donanım hızlandırma kullanılabilir ve geliştirme bilgisayarındaki etkin.

-   Öykünücü için özel olarak oluşturulmuş bir öykünücü görüntünüzün çalıştıran bir **x86**-sanal aygıt bağlı.

Başlatma ve Android öykünücü ile hata ayıklama hakkında daha fazla bilgi için bkz: [Android öykünücüsünde hata ayıklama](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="hyper-v"></a>Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Hyper-V desteği şu anda önizlemede değil.

Windows 10 kullanan geliştiriciler (Nisan 2018 güncelleştirmek veya sonrası) Android öykünücüsünde hızlandırmak için Microsoft'un Hyper-V kullanmayı önemle önerilir. Android öykünücüsünde Hyper-V: ile kullanmak için

1. **Windows 10 Nisan 2018 güncelleştirmeye güncelleştirme (yapı 1803) veya daha sonra**.
   Hangi Windows sürümünü çalıştıran doğrulamak için Cortana arama çubuğu ve türünü tıklatın **hakkında**. Seçin **bilgisayarınız hakkında** arama sonuçlarında. İçinde aşağı kaydırarak **hakkında** iletişim kutusuna **Windows belirtimleri** bölümü. **Sürüm** en az 1803 olmalıdır:

    [![Windows özellikleri](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Windows hiper yönetici platformunu etkinleştirin**.
   Cortana Arama çubuğuna **kapatma Windows özelliklerini aç veya Kapat**.
   İçinde aşağı kaydırarak **Windows özelliklerini** iletişim ve emin olun **Windows hiper yönetici platformu** etkinleştirilir:

    [![Windows hiper yönetici platformu etkin](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

   Etkinleştirme **Windows hiper yönetici platformu** otomatik olarak Hyper-V sağlar. Bu değişikliği yaptıktan sonra Windows'u yeniden başlatmak için iyi bir fikirdir.

3. **Yükleme [Visual Studio 15,8 Preview 1 veya sonrası](https://visualstudio.microsoft.com/vs/preview/)**.
   Visual Studio'nun bu sürümü, Hyper-V ile Android öykünücüsünde çalıştırmak için IDE desteği sağlar.
 
4. **Android öykünücüsünde Paketi 27.2.7 yüklemek ya da daha sonra**. Bu paketi yüklemek için gidin **Araçlar > Android > Android SDK Manager** Visual Studio. Seçin **Araçları** sekmesinde ve Android öykünücüsü sürümünün en az 27.2.7 olduğundan emin olun. Ayrıca Android SDK Araçları sürüm 26.1.1 olduğundan emin olun ya da daha sonra:

    [![Android SDK'lar ve Araçlar iletişim](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Hyper-V: kullanmak için aşağıdaki geçici çözüm öykünücüsü sürümü en az 27.2.7 değerinden 27.3.1 ancak ise gereklidir

    1.  İçinde **C:\\kullanıcılar\\_kullanıcıadı_\\.android** klasörünü adlı bir dosya oluşturun **advancedFeatures.ini** (yoktur, zaten var).

    2.  Aşağıdaki satırı ekleyin **advancedFeatures.ini**:
        ```
        WindowsHypervisorPlatform = on
        ```


### <a name="known-issues"></a>Bilinen Sorunlar

-   27.2.7 öykünücüsü sürümüne veya daha sonra Visual Studio Önizleme sürümüne güncelleştirdikten sonra güncelleştiremiyor varsa, doğrudan yüklemek zorunda kalabilirsiniz [yükleyici Önizleme](http://aka.ms/hyperv-emulator-dl) yeni öykünücü sürümleri etkinleştirmek için.

-   Performans, belirli Intel ve AMD tabanlı işlemciler kullanırken azaltılabilir.

-   Android uygulaması olağan dışı bir dağıtımda yüklemek için zaman miktarı sürebilir.

-   OLMASI erişim hatası, zaman zaman Android öykünücüsünü önyükleme engel olabilir. Öykünücü yeniden bu çözümlenmelidir.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Hyper-V desteği, Windows 10 gerektirir. Lütfen bakın [Hyper-V gereksinimleri](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) daha fazla ayrıntı için.

-----

## <a name="haxm"></a>HAXM

HAXM bir konak makinesi üzerinde Android uygulaması öykünme hızlandırmak için Intel Sanallaştırma Teknolojisi (VT) kullanan bir donanım destekli sanallaştırma (hiper yönetici) altyapısıdır. Android x86 birlikte HAXM kullanarak Intel tarafından sağlanan öykünücüsü görüntüleri sağlar VT etkin sistemlerde daha hızlı Android öykünmesi için.

VT yetenekleri olan bir Intel CPU bir makinesinde geliştiriyorsanız, büyük ölçüde Android öykünücüsü hızlandırmak için HAXM yararlanabilir (, CPU VT destekleyip desteklemediğini emin değilseniz bkz [mu My işlemci destekleyen Intel Sanallaştırma Teknolojisi nedir? ](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Bir VM hızlandırılmış öykünücüsü VirtualBox, VMWare veya Docker tarafından barındırılan bir VM'nin gibi başka bir VM içinde çalıştırılamaz. Android öykünücüsünde çalıştırmak [sistem donanımınız üzerinde doğrudan](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Android öykünücüsünde ilk kez kullanmadan önce HAXM yüklenir ve Android öykünücüsü kullanılabilir olduğunu doğrulamak için bir fikirdir.

### <a name="verifying-haxm-installation"></a>HAXM yüklemesini doğrulama

HAXM görüntüleyerek kullanılabilir olup olmadığını kontrol edebilirsiniz **başlangıç Android öykünücüsü** öykünücü başlatılırken penceresi. Android öykünücüsünde başlatmak için aşağıdakileri yapın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Android Aygıt Yöneticisi'ni tıklatarak başlatma **Araçlar > Android > Android Aygıt Yöneticisi'ni**:

    [![Android cihaz Yöneticisi menü öğesi konumu](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Görürseniz bir **Performans Uyarısı** iletişim HAXM henüz yüklenmemiş veya bilgisayarınızda düzgün yapılandırılmamış sonra aşağıdakine benzer:

    ![Performans Uyarısı iletişim kutusu HAXM hazır değil](hardware-acceleration-images/win/11-perf-warn.png)

   Varsa bir **Performans Uyarısı** iletişim şöyle gösterilir, bkz: [performans uyarıları](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) sorunun kaynağını belirlemek ve arka plandaki sorunu çözmek için.

3. Seçin bir **x86** görüntü (örneğin, **Visual Studio\_android 23\_x86\_telefon**) tıklatıp **Başlat**:

    ![Bir varsayılan sanal aygıt görüntüsüyle Android öykünücüsünde başlatılıyor](hardware-acceleration-images/win/02-start-default-avd.png)

4. İzlemesi **başlangıç Android öykünücüsü** öykünücü başlatılırken iletişim penceresi. HAXM yüklediyseniz iletisini görür **HAX çalıştığından ve öykünücüsü hızlı Sanal Küp Str modunda çalıştırır** bu ekran görüntüsünde gösterildiği gibi:

    ![HAXM başlangıç Android öykünücüsü iletişim kutusunda kullanılabilir olarak gösterilir](hardware-acceleration-images/win/03-haxm-detected.png)

   Bu iletiyi görmüyorsanız HAXM yüklü olmayabilir. Örneğin, bir ekran görüntüsünü HAXM kullanılabilir olup olmadığını görebilirsiniz bir ileti şöyledir:

    ![HAXM bu makinede kullanılabilir değil](hardware-acceleration-images/win/04-haxm-error.png)

   HAXM bilgisayarınızda kullanılabilir durumda değilse, HAXM yüklemek için sonraki bölümde adımları kullanın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Android Aygıt Yöneticisi'ni tıklatarak başlatma **Araçlar > Aygıt Yöneticisi'ni**:

    [![Android cihaz Yöneticisi menü öğesi konumu](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Görürseniz bir **Performans Uyarısı** iletişim HAXM henüz yüklenmemiş veya bilgisayarınızda düzgün yapılandırılmamış sonra aşağıdakine benzer:

    ![Performans Uyarısı iletişim kutusu HAXM hazır değil](hardware-acceleration-images/mac/04-avd-warning.png)

   Varsa bir **Performans Uyarısı** iletişim şöyle gösterilir, bkz: [performans uyarıları](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) sorunun kaynağını belirlemek ve arka plandaki sorunu çözmek için.

3. Seçin **x86** görüntü (örneğin, **Android\_hızlandırılmış\_x86**) tıklatıp **Yürüt**:

    [![Bir varsayılan sanal aygıt görüntüsüyle Android öykünücüsünde başlatılıyor](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. İzlemesi **başlangıç Android öykünücüsü** öykünücü başlatılırken iletişim penceresi. HAXM yüklediyseniz iletisini görür **HAX çalıştığından ve öykünücüsü hızlı Sanal Küp Str modunda çalıştırır** bu ekran görüntüsünde gösterildiği gibi:

    ![HAXM başlangıç Android öykünücüsü iletişim kutusunda kullanılabilir olarak gösterilir](hardware-acceleration-images/mac/03-haxm-detected.png)

   HAXM bilgisayarınızda kullanılabilir değilse (örneğin, benzer bir hata iletisi görürseniz _Lütfen Intel HAXM propertly yüklü ve kullanılabilir olduğundan emin olun_), HAXM yüklemek için sonraki bölümde adımları kullanın.

-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>HAXM yükleme

Öykünücü başlamazsa HAXM el ile yüklenmesi gerekebilir. Hem Windows hem de macOS edinilebilir için HAXM yükleme paketleri [Intel donanım hızlandırılmış yürütme Yöneticisi](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) sayfası. İndirip HAXM el ile yüklemek için aşağıdaki adımları kullanın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Intel Web sitesinden en son karşıdan yüklemek [HAXM sanallaştırma altyapısı](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Windows Installer. Doğrudan Intel Web sitesinden HAXM yükleyicisi indiriliyor avantajı, en son sürümünü kullanarak da gösterilmeyeceği olmasıdır.

   Alternatif olarak, HAXM yükleyici indirmek için SDK Yöneticisi'ni kullanabilirsiniz (SDK Yöneticisi'nde **Araçlar > Ek Özellikler > Intel x86 öykünücüsü Hızlandırıcı (HAXM Yükleyici)**). Android SDK HAXM yükleyici normalde aşağıdaki konuma yükler:

   **C:\\Program Files (x86)\\Android\\android sdk\\ek özellikler\\Intel\\donanım\_hızlandırılmış\_yürütme\_Yöneticisi**

   SDK Yöneticisi HAXM yüklemez, yalnızca yukarıdaki konuma HAXM yükleyici yükler unutmayın; el ile başlatma çözümlenmedi.

2. Çalıştırma **intelhaxm android.exe** HAXM yükleyiciyi başlatmak için. Yükleyici iletişim kutularını varsayılan değerleri kabul edin:

   ![Intel donanım hızlandırılmış yürütme Yöneticisi Kurulum penceresi](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>Donanım hızlandırma ve AMD CPU'lar

Android öykünücüsü şu anda AMD donanım hızlandırmasını desteklediğinden [yalnızca Linux üzerinde](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), donanım hızlandırmasını Windows çalıştıran AMD tabanlı bilgisayarlar için kullanılabilir değil.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Intel Web sitesinden en son karşıdan yüklemek [HAXM sanallaştırma altyapısı](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) macOS yükleyici.

2. HAXM yükleyiciyi çalıştırın. Yükleyici iletişim kutularını varsayılan değerleri kabul edin:

   [![Intel donanım hızlandırılmış yürütme Yöneticisi Kurulum penceresi](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>İlgili bağlantılar

* [Uygulamaların Android öykünücüsünde çalıştırın](https://developer.android.com/studio/run/emulator)
