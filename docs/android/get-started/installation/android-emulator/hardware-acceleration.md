---
title: Donanım hızlandırma öykünücüsü performansı
description: Bu makalede, bilgisayarınızın donanım hızlandırma özelliklerinden Google Android öykünücüsü performansını en üst düzeye çıkarmak için nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: cf75d070dfff97f48bab2210f72af747bc53099c
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732950"
---
# <a name="hardware-acceleration-for-emulator-performance"></a>Donanım hızlandırma öykünücüsü performansı

_Bu makalede, bilgisayarınızın donanım hızlandırma özelliklerinden Google Android öykünücüsü performansını en üst düzeye çıkarmak için nasıl kullanılacağı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Visual Studio test ve Xamarin.Android uygulamalarına bir Android cihaz kullanılamıyor veya pratik olduğu durumlarda Google Android öykünücüsü kullanarak hata ayıklama geliştiricilere kolaylaştırır.
Donanım hızlandırma çalıştırılan bilgisayarda kullanılabilir değilse, ancak çok yavaş Android öykünücüsünde çalışır. İki sanallaştırma teknolojilerini biri ile birlikte bu hedef x86 donanım özel sanal cihaz görüntüleri kullanarak Android öykünücüsünde performansını önemli ölçüde artırabilir:

1. **Microsoft'un Hyper-V ve hiper yönetici platformuna**. Hyper-V sanallaştırılmış bilgisayar sistemleri bir fiziksel ana bilgisayarda çalıştırmak mümkün kılan Windows sanallaştırma özelliğidir. Google Android öykünücüsü hızlandırmaya için kullanılacak önerilen sanallaştırma teknolojisi budur. Hyper-V hakkında daha fazla bilgi için bkz: [Hyper-V Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).

2. **Intel'in donanım hızlandırılmış yürütme Yöneticisi'ni (HAXM)**. 
   HAXM Intel CPU çalıştıran bilgisayarlar için sanallaştırma altyapısıdır.
   Bu işlev, Hyper-V çalıştırılamıyor olan bilgisayarlar için önerilen sanallaştırma altyapısıdır.

Google Android öykünücüsü otomatik hale getirir, aşağıdaki ölçütler karşılanıyorsa donanım hızlandırmasını kullanın:

-   Donanım hızlandırma kullanılabilir ve geliştirme bilgisayarındaki etkin.

-   Öykünücü için özel olarak oluşturulmuş bir öykünücü görüntünüzün çalıştıran bir **x86**-sanal aygıt bağlı.

Başlatma ve Android öykünücü ile hata ayıklama hakkında daha fazla bilgi için bkz: [Google Android öykünücüsü ile hata ayıklama](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

## <a name="hyper-v"></a>Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Hyper-V desteği şu anda önizlemede değil.

Windows 10 kullanan geliştiriciler (Nisan 2018 güncelleştirmek veya sonrası) Google Android öykünücüsü hızlandırmak için Microsoft'un Hyper-V kullanmayı önemle önerilir. Google Android öykünücüsü Hyper-V: ile kullanmak için

1. **Windows 10 Nisan 2018 güncelleştirmeye güncelleştirme (yapı 1803) veya daha sonra**.
   Hangi Windows sürümünü çalıştıran doğrulamak için Cortana arama çubuğu ve türünü tıklatın **hakkında**. Seçin **bilgisayarınız hakkında** arama sonuçlarında. İçinde aşağı kaydırarak **hakkında** iletişim kutusuna **Windows belirtimleri** bölümü. **Sürüm** en az 1803 olmalıdır:

    [![Windows özellikleri](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Windows hiper yönetici platformunu etkinleştirin**.
   Cortana Arama çubuğuna **kapatma Windows özelliklerini aç veya Kapat**.
   İçinde aşağı kaydırarak **Windows özelliklerini** iletişim ve emin olun **Windows hiper yönetici platformu** etkinleştirilir:

    [![Windows hiper yönetici platformu etkin](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

   Etkinleştirme **Windows hiper yönetici platformu** otomatik olarak Hyper-V sağlar. Bu değişikliği yaptıktan sonra Windows'u yeniden başlatmak için iyi bir fikirdir.

3. **Yükleme [Visual Studio 15,8 Preview 1 veya sonrası](https://www.visualstudio.com/vs/preview/)**.
   Visual Studio'nun bu sürümü, Google Android öykünücüsü Hyper-V ile çalıştırmak için IDE desteği sağlar.
 
4. **Google Android öykünücüsü Paketi 27.2.7 yüklemek ya da daha sonra**. Bu paketi yüklemek için gidin **Araçlar > Android > Android SDK Manager** Visual Studio. Seçin **Araçları** sekmesinde ve Android öykünücüsü sürümünün en az 27.2.7 olduğundan emin olun. Ayrıca Android SDK Araçları sürüm 26.1.1 olduğundan emin olun ya da daha sonra:

    [![Android SDK'lar ve Araçlar iletişim](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Hyper-V: kullanmak için aşağıdaki geçici çözüm öykünücüsü sürümü en az 27.2.7 değerinden 27.3.1 ancak ise gereklidir

    1.  İçinde **C:\\kullanıcılar\\_kullanıcıadı_\\.android** klasörünü adlı bir dosya oluşturun **advancedFeatures.ini** (yoktur, zaten var).

    2.  Aşağıdaki satırı ekleyin **advancedFeatures.ini**:
        ```
        WindowsHypervisorPlatform = on
        ```


### <a name="known-issues"></a>Bilinen Sorunlar

-   Performans, belirli Intel ve AMD tabanlı işlemciler kullanırken azaltılabilir.

-   Android uygulaması olağan dışı bir dağıtımda yüklemek için zaman miktarı sürebilir.

-   OLMASI erişim hatası, zaman zaman Android öykünücüsünü önyükleme engel olabilir. Öykünücü yeniden bu çözümlenmelidir.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Hyper-V desteği, Windows 10 gerektirir. Lütfen bakın [Hyper-V gereksinimleri](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) daha fazla ayrıntı için.

-----

## <a name="haxm"></a>HAXM

HAXM bir konak makinesi üzerinde Android uygulaması öykünme hızlandırmak için Intel Sanallaştırma Teknolojisi (VT) kullanan bir donanım destekli sanallaştırma (hiper yönetici) altyapısıdır. Android x86 birlikte HAXM kullanarak Intel tarafından sağlanan öykünücüsü görüntüleri sağlar VT etkin sistemlerde daha hızlı Android öykünmesi için.

VT yetenekleri olan bir Intel CPU bir makinesinde geliştiriyorsanız, büyük ölçüde Google Android öykünücüsü hızlandırmak için HAXM yararlanabilir (, CPU VT destekleyip desteklemediğini emin değilseniz bkz [My işlemci destekleyen Intel mu Sanallaştırma teknolojisi nedir? ](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Bir VM hızlandırılmış öykünücüsü VirtualBox, VMWare veya Docker tarafından barındırılan bir VM'nin gibi başka bir VM içinde çalıştırılamaz. Google Android öykünücüsü çalıştırmalısınız [sistem donanımınız üzerinde doğrudan](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Google Android öykünücüsü ilk kez kullanmadan önce HAXM yüklenir ve kullanmak Google Android öykünücüsü kullanılabilir olduğunu doğrulamak için bir fikirdir.

### <a name="verifying-haxm-installation"></a>HAXM yüklemesini doğrulama

HAXM görüntüleyerek kullanılabilir olup olmadığını kontrol edebilirsiniz **başlangıç Android öykünücüsü** öykünücü başlatılırken penceresi. Google Android öykünücüsü başlatmak için aşağıdakileri yapın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Android Aygıt Yöneticisi'ni tıklatarak başlatma **Araçlar > Android > Android Aygıt Yöneticisi'ni**:

    [![Android cihaz Yöneticisi menü öğesi konumu](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Görürseniz bir **Performans Uyarısı** iletişim HAXM henüz yüklenmemiş veya bilgisayarınızda düzgün yapılandırılmamış sonra aşağıdakine benzer:

    ![Performans Uyarısı iletişim kutusu HAXM hazır değil](hardware-acceleration-images/win/11-perf-warn.png)

   Varsa bir **Performans Uyarısı** iletişim şöyle gösterilir, bkz: [performans uyarıları](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) sorunun kaynağını belirlemek ve arka plandaki sorunu çözmek için.

3. Seçin bir **x86** görüntü (örneğin, **Visual Studio\_android 23\_x86\_telefon**) tıklatıp **Başlat**:

    ![Google Android öykünücüsü bir varsayılan sanal aygıt görüntüsüyle başlatılıyor](hardware-acceleration-images/win/02-start-default-avd.png)

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

   Varsa bir **Performans Uyarısı** iletişim şöyle gösterilir, bkz: [performans uyarıları](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) sorunun kaynağını belirlemek ve arka plandaki sorunu çözmek için.

3. Seçin **x86** görüntü (örneğin, **Android\_hızlandırılmış\_x86**) tıklatıp **Yürüt**:

    [![Google Android öykünücüsü bir varsayılan sanal aygıt görüntüsüyle başlatılıyor](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

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

Google Android öykünücüsü şu anda AMD donanım hızlandırmasını desteklediğinden [yalnızca Linux üzerinde](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), donanım hızlandırmasını Windows çalıştıran AMD tabanlı bilgisayarlar için kullanılabilir değil.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Intel Web sitesinden en son karşıdan yüklemek [HAXM sanallaştırma altyapısı](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) macOS yükleyici.

2. HAXM yükleyiciyi çalıştırın. Yükleyici iletişim kutularını varsayılan değerleri kabul edin:

   [![Intel donanım hızlandırılmış yürütme Yöneticisi Kurulum penceresi](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>İlgili bağlantılar

* [Uygulamaların Android öykünücüsünde çalıştırın](https://developer.android.com/studio/run/emulator)
