---
title: Emulator performansını için donanım hızlandırma
description: Bu makalede, Android Emulator performansını en üst düzeye çıkarmak için bilgisayarınızın donanım hızlandırma özelliklerinden kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: c2bef2c614d4cc0655deb9732ccefec223a8318a
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848473"
---
# <a name="hardware-acceleration-for-emulator-performance"></a>Emulator performansını için donanım hızlandırma

_Bu makalede, Android Emulator performansını en üst düzeye çıkarmak için bilgisayarınızın donanım hızlandırma özelliklerinden kullanmayı açıklar._

## <a name="overview"></a>Genel Bakış

Visual Studio Android öykünücüsü Android cihaz kullanılamıyor veya pratik olduğu durumlarda kullanarak kendi Xamarin.Android uygulamalarında hata ayıklama ve test geliştiricilere kolaylaştırır.
Donanım hızlandırma çalıştığı bilgisayarda kullanılabilir değilse, ancak çok yavaş Android öykünücüsünde çalışır. Ayrıca, iki sanallaştırma teknolojilerini biriyle birlikte hedef x86 donanım özel sanal cihaz görüntüleri kullanarak Android emulator performansını önemli ölçüde artırabilirsiniz:

1. **Microsoft'un Hyper-V ve hiper yönetici platformuna**. Hyper-V sanallaştırılmış bilgisayar sistemleri bir fiziksel ana bilgisayarda çalışması mümkün kılar Windows sanallaştırma özelliğidir. Bu Android Emulator'da daha hızlı sunabilirsiniz kullanmak için önerilen bir sanallaştırma teknolojisidir. Hyper-V hakkında daha fazla bilgi için bkz: [Hyper-V Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).

2. **Intel donanım hızlandırılmış yürütme Yöneticisi (HAXM)**. 
   HAXM, Intel CPU'yu çalıştıran bilgisayarlar için bir sanallaştırma altyapısıdır.
   Hyper-V şekilde çalıştıramaması bilgisayarlar için önerilen bir sanallaştırma altyapısı budur.

Android öykünücüsü otomatik olarak oluşturacak aşağıdaki ölçütler karşılandığında donanım hızlandırma kullanın:

-   Donanım hızlandırma etkin geliştirme bilgisayarında ve kullanılabilir.

-   Öykünücü için özel olarak oluşturulmuş bir öykünücü görüntüsü çalıştıran bir **x86**-tabanlı sanal cihaz.

Android öykünücüsü ile hata ayıklama ve başlatma hakkında daha fazla bilgi için bkz: [Android öykünücü üzerinde hata ayıklama](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="hyper-v"></a>Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Hyper-V desteği şu anda Önizleme aşamasındadır.

Windows 10 kullanan geliştiriciler (Nisan 2018 güncelleştirmesi veya sonrası) için Android öykünücüsü hızlandırmak için Microsoft'un Hyper-V kullanmanız kesinlikle önerilir. Hyper-V: ile Android öykünücü kullanmak için

1. **Güncelleştirme Windows 10 Nisan 2018 Güncelleştirmesi (derleme 1803) veya üzeri**.
   Çalışan Windows sürümünü doğrulamak için türü ve Cortana Arama çubuğuna tıklayın **hakkında**. Seçin **bilgisayarınız hakkında** arama sonuçlarında. Aşağı kaydırın **hakkında** iletişim kutusuna **Windows belirtimleri** bölümü. **Sürüm** 1803'en az olmalıdır:

    [![Windows özellikleri](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Windows hiper yönetici platformu**.
   Cortana Arama çubuğuna yazın **kapatma Windows özelliklerini aç veya Kapat**.
   Aşağı kaydırın **Windows özellikleri** iletişim olduğundan emin olun **Windows hiper yönetici platformu** etkinleştirilir:

    [![Windows hiper yönetici platformu, etkin](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

   Etkinleştirme **Windows hiper yönetici platformu** otomatik olarak Hyper-V sağlar. Bu değişikliği yaptıktan sonra Windows yeniden başlatmak için iyi bir fikirdir.

3. **Yükleme [Visual Studio 15,8 önizleme 1 veya sonraki sürümler](https://visualstudio.microsoft.com/vs/preview/)**.
   Visual Studio'nun bu sürümü, Hyper-V ile Android öykünücüsünde çalıştırmak için IDE desteği sağlar.
 
4. **Android öykünücüsü Paketi 27.2.7 yükleyin veya üzeri**. Bu paketi yüklemek için gidin **Araçlar > Android > Android SDK Yöneticisi** Visual Studio'da. Seçin **Araçları** sekmesini ve Android öykünücüsü sürümünün en az 27.2.7 olduğundan emin olun. Ayrıca Android SDK Tools sürümü 26.1.1 olduğundan emin olun veya daha sonra:

    [![Android SDK ve araçları iletişim](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Öykünücü sürümü en az 27.2.7 27.3.1 küçüktür ama ise, aşağıdaki geçici çözümü Hyper-V: kullanmak için gereklidir

    1.  İçinde **C:\\kullanıcılar\\_kullanıcı adı_\\.android** klasöründe adlı bir dosya oluşturun **advancedFeatures.ini** (bunu değil ise zaten var).

    2.  Aşağıdaki satırı ekleyin **advancedFeatures.ini**:
        ```
        WindowsHypervisorPlatform = on
        ```


### <a name="known-issues"></a>Bilinen Sorunlar

-   27.2.7 öykünücüsü sürümüne veya daha sonra bir Visual Studio Önizleme parametresini güncelleştirdikten sonra güncelleştirme bulamıyorsanız, doğrudan yüklemeniz gerekebilir [yükleyici Önizleme](http://aka.ms/hyperv-emulator-dl) yeni öykünücü sürümleri etkinleştirmek için.

-   Belirli Intel ve AMD tabanlı işlemciler kullanırken performans azalabilir.

-   Android uygulaması olağan dışı bir dağıtım yüklemek için zaman miktarı alabilir.

-   OLMASI erişim hatası aralıklı olarak bir önyükleme Android öykünücüsü engelliyor olabilir. Öykünücü yeniden başlatılması bu çözümlenmelidir.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Windows 10 Hyper-V desteği gerektirir. Lütfen [Hyper-V gereksinimleri](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) daha fazla ayrıntı için.

-----

## <a name="haxm"></a>HAXM

HAXM, bir konak makinesi üzerinde Android uygulaması öykünmesinin hızlandırmak için Intel Sanallaştırma Teknolojisi (VT) kullanan bir donanım Yardımlı sanallaştırma (hiper yönetici) altyapısıdır. HAXM Android x86 ile birlikte kullanarak Intel tarafından sağlanan öykünücü görüntüleri sağlar Android öykünme VT özellikli sistemler üzerinde daha hızlı.

VT yeteneklere sahip bir Intel CPU'yu olan bir makinede geliştiriyorsanız, Android öykünücüsü'nü önemli ölçüde hızlandırmak için HAXM yararlanabilirsiniz (CPU VT destekleyip desteklemediğini emin değilseniz bkz [mu My işlemci destekler Intel Sanallaştırma Teknolojisi nedir? ](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Bir VM hızlandırılmış öykünücü VirtualBox, VMWare ya da Docker tarafından barındırılan bir sanal makine gibi başka bir VM içinde çalıştırılamaz. Android öykünücüsü çalıştırmalısınız [doğrudan sistem donanımınıza](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

İlk kez Android öykünücüsü'nü kullanmadan önce yüklü ve mevcut Android öykünücüsü HAXM olduğunu doğrulamak için iyi bir fikirdir.

### <a name="verifying-haxm-installation"></a>HAXM yüklemesini doğrulama

HAXM görüntüleyerek kullanılabilir olup olmadığını kontrol edebilirsiniz **Android öykünücüsü başlatma** öykünücüsü başlatılırken penceresi. Android öykünücüsünde başlatmak için aşağıdakileri yapın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Android cihaz Yöneticisi tıklanması **Araçlar > Android > Android cihaz Yöneticisi**:

    [![Android cihaz Yöneticisi'ni menü öğesi konumu](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Görürseniz bir **Performans Uyarısı** iletişim HAXM henüz yüklü veya bilgisayarınızda düzgün yapılandırılmış sonra aşağıdakine benzer:

    ![Performans Uyarısı iletişim kutusu HAXM hazır değil](hardware-acceleration-images/win/11-perf-warn.png)

   Varsa bir **Performans Uyarısı** şöyle iletişim gösterilir, bkz [Performans uyarılarını](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) sorunun kaynağını belirlemek ve arka plandaki sorunu çözün.

3. Seçin bir **x86** görüntü (örneğin, **VisualStudio\_android-23\_x86\_telefon**) tıklayıp **Başlat**:

    ![Varsayılan sanal cihaz görüntüsü ile Android öykünücüsü başlatılıyor](hardware-acceleration-images/win/02-start-default-avd.png)

4. İzlemesi **Android öykünücüsü başlatma** öykünücüsü başlatılırken iletişim penceresi. HAXM yüklü değilse, iletisini görür **HAX çalıştığından ve öykünücü hızlı vırt modunda çalıştırır** bu ekran görüntüsünde gösterildiği gibi:

    ![HAXM Android öykünücüsü başlatma iletişim kutusunda kullanılabilir olarak gösterilir](hardware-acceleration-images/win/03-haxm-detected.png)

   Bu iletiyi görmüyorsanız, HAXM yüklü olmayabilir. Örneğin, bir ekran görüntüsü HAXM kullanılamıyorsa görebileceğiniz bir ileti şu şekildedir:

    ![HAXM, bu makinede mevcut değil.](hardware-acceleration-images/win/04-haxm-error.png)

   HAXM bilgisayarınızda mevcut değilse, HAXM yüklemek için sonraki bölümde adımları kullanın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Android cihaz Yöneticisi tıklanması **Araçlar > cihaz Yöneticisi**:

    [![Android cihaz Yöneticisi'ni menü öğesi konumu](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Görürseniz bir **Performans Uyarısı** iletişim HAXM henüz yüklü veya bilgisayarınızda düzgün yapılandırılmış sonra aşağıdakine benzer:

    ![Performans Uyarısı iletişim kutusu HAXM hazır değil](hardware-acceleration-images/mac/04-avd-warning.png)

   Varsa bir **Performans Uyarısı** şöyle iletişim gösterilir, bkz [Performans uyarılarını](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) sorunun kaynağını belirlemek ve arka plandaki sorunu çözün.

3. Seçin **x86** görüntü (örneğin, **Android\_hızlandırılmış\_x86**) tıklayıp **Play**:

    [![Varsayılan sanal cihaz görüntüsü ile Android öykünücüsü başlatılıyor](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. İzlemesi **Android öykünücüsü başlatma** öykünücüsü başlatılırken iletişim penceresi. HAXM yüklü değilse, iletisini görür **HAX çalıştığından ve öykünücü hızlı vırt modunda çalıştırır** bu ekran görüntüsünde gösterildiği gibi:

    ![HAXM Android öykünücüsü başlatma iletişim kutusunda kullanılabilir olarak gösterilir](hardware-acceleration-images/mac/03-haxm-detected.png)

   HAXM bilgisayarınızda mevcut değilse (örneğin, benzer bir hata iletisi görürseniz _Intel HAXM propertly yüklü ve kullanılabilir olduğundan emin olun_), HAXM yüklemek için sonraki bölümde adımları kullanın.

-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>HAXM yükleniyor

Öykünücü başlatılmazsa, HAXM el ile yüklenmesi gerekebilir. HAXM'yi yükle paketleri hem Windows hem de macOS için [Intel donanım hızlandırılmış yürütme Yöneticisi](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) sayfası. İndirmek ve HAXM el ile yüklemek için aşağıdaki adımları kullanın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. En son Intel Web sitesinden indirme [HAXM bir sanallaştırma altyapısı](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) için Windows Yükleyici. HAXM yükleyicisi doğrudan Intel Web sitesinden indirerek avantajı, size en son sürümünü kullanarak olabilirsiniz, olmasıdır.

   Alternatif olarak, HAXM yükleyiciyi indirmek için SDK Yöneticisi'ni kullanabilirsiniz (SDK Yöneticisi'nde **Araçlar > Ek Özellikler > Intel x86 öykünücü Hızlandırıcı (HAXM Yükleyicisi)**). Android SDK'sı, normalde HAXM yükleyicisi şu konuma yükler:

   **C:\\Program dosyaları (x86)\\Android\\android sdk\\ek özellikler\\Intel\\donanım\_hızlandırılmış\_yürütme\_Yöneticisi**

   SDK Yöneticisi'ni HAXM yüklemez, yalnızca yukarıdaki konumuna HAXM yükleyicisi indirir dikkat edin. yine de el ile başlatma gerekir.

2. Çalıştırma **intelhaxm android.exe** HAXM yükleyicisi başlatılamadı. Yükleyici kutularındaki varsayılan değerleri kabul edin:

   ![Intel donanım hızlandırılmış yürütme Yöneticisi kurulumunu penceresi](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>Donanım hızlandırma ve AMD işlemciler

Android öykünücüsü şu anda AMD donanım hızlandırma desteklediğinden [yalnızca Linux üzerinde](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), donanım hızlandırma Windows çalıştıran AMD tabanlı bilgisayarlar için kullanılabilir değil.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. En son Intel Web sitesinden indirme [HAXM bir sanallaştırma altyapısı](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) macOS için yükleyici.

2. HAXM Yükleyicisi'ni çalıştırın. Yükleyici kutularındaki varsayılan değerleri kabul edin:

   [![Intel donanım hızlandırılmış yürütme Yöneticisi kurulumunu penceresi](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>İlgili bağlantılar

* [Uygulamaları üzerinde Android Emulator uygulamasını çalıştırma](https://developer.android.com/studio/run/emulator)
