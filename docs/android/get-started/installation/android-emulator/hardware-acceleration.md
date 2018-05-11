---
title: Android öykünücüsünde donanım hızlandırma
description: Bilgisayarınızı maksimum Google Android öykünücüsü performans için hazırlama
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/07/2018
ms.openlocfilehash: 2d903df97da2e8d6ae0c5df3b1ba09dd3015e404
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Android öykünücüsünde donanım hızlandırma

Google Android öykünücüsü donanım hızlandırmasını şekilde basımı karşılamayacak kadar yavaş olur. Google Android öykünücüsü'ı özel öykünücüsü lunlar görüntüleri, hedef x86 donanım ve iki sanallaştırma teknolojilerini birini kullanarak performansını önemli ölçüde artırmak mümkündür:

1. **Microsoft Hyper-V ve hiper yönetici platformuna kullanıcının** &ndash; Hyper-V fiziksel ana bilgisayar üzerinde çalışan sanallaştırılmış bilgisayar sistemleri sağlayan Windows 10 kullanılabilir olan bir bileşenidir sanallaştırma. Hızlandırılmış Google Android öykünücüsü görüntüleri için önerilen sanallaştırma teknolojisi budur. Hyper-V hakkında daha fazla bilgi için lütfen bakın [Windows 10 kılavuzu üzerinde Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).
2. **Intel'in donanım hızlandırılmış yürütme Yöneticisi'ni (HAXM)** &ndash; Bu, Intel CPU çalıştıran bilgisayarlar için sanallaştırma altyapısıdır. Bu işlev, Hyper-V kullanamıyorsunuz geliştiriciler için önerilen sanallaştırma altyapısıdır.

Android SDK Yöneticisi'ni otomatik olarak yapmak kullanılabilir olduğunda donanım hızlandırma kullanımı için özellikle öykünücüsü görüntü çalıştığı bir **x86**-sanal aygıt tabanlı (açıklandığı gibi [yapılandırma ve kullanma ](~/android/deploy-test/debugging/android-sdk-emulator/index.md)).

## <a name="hyper-v-overview"></a>Hyper-V'ye Genel Bakış

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Hyper-V desteği şu anda önizlemede değil.

Windows 10 kullanan geliştiriciler (Nisan 2018 güncelleştirmesi) Microsoft'un Hyper-V kullanmanız önemle önerilir. Xamarin için Visual Studio Araçları sınamak ve bir Android cihaz kullanılamıyor veya pratik olduğu durumlar Xamarin.Android uygulamalarında hata ayıklama geliştiriciler için kolay hale getirir.

Hyper-V ve Google Android öykünücüsü kullanmaya başlamak için:

1. **Windows 10 Nisan 2018 güncelleştirmeye güncelleştirme (yapı 1803)** &ndash; hangi Windows sürümünü çalıştıran doğrulamak için Cortana arama çubuğu türü içinde tıklatın ve **hakkında**. Seçin **bilgisayarınız hakkında** arama sonuçlarında. İçinde aşağı kaydırarak **hakkında** iletişim, **Windows belirtimleri** bölümü. **Sürüm** en az 1803 olmalıdır:

    [![Windows özellikleri](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

1. **Hyper-V ve Windows hiper yönetici platformunu etkinleştir** &ndash; Cortana arama çubuğunda, türü **kapatma Windows özelliklerini aç veya Kapat**. İçinde aşağı kaydırın **Windows özelliklerini** iletişim kutusunda ve emin **Windows hiper yönetici platformu** etkinleştirilir.

    [![Hyper-V ve Windows hiper yönetici platformu etkin](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

    Windows Hyper-V ve Windows hiper yönetici platformu etkinleştirdikten sonra bilgisayarınızı yeniden başlatmanız gerekebilir.

1. **Yükleme [Visual Studio 15,8 Preview 1](https://aka.ms/hyperv-emulator-dl)**  &ndash; Visual Studio'nun bu sürümü, Google Android öykünücüsü Hyper-V desteği ile başlatmak için IDE desteği sağlar.

1. **Google Android öykünücüsü Paketi 27.2.7 yüklemek ya da daha yüksek** &ndash; bu paketi yüklemek için gidin **Araçlar > Android > Android SDK Manager** Visual Studio. Seçin **Araçları** sekmesini tıklatın ve Android öykünücüsü bileşen en az olduğundan emin olun 27.2.7 sürümü.

    [![Android SDK'lar ve Araçlar iletişim](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

### <a name="known-issues"></a>Bilinen Sorunlar

* Performans, belirli Intel ve AMD tabanlı işlemciler kullanırken azaltılabilir.
* Android uygulaması olağan dışı bir dağıtımda yüklemek için zaman miktarı sürebilir.
* OLMASI erişim hatası, zaman zaman Android öykünücüsünü önyükleme engel olabilir. Öykünücü yeniden bu çözümlenmelidir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Hyper-V desteği, Windows 10 gerektirir. Lütfen bakın [Hyper-V gereksinimleri](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) daha fazla ayrıntı için.

-----

## <a name="haxm-overview"></a>HAXM genel bakış

HAXM bir konak makinesi üzerinde Android uygulaması öykünme hızlandırmak için Intel Sanallaştırma Teknolojisi (VT) kullanan bir donanım destekli sanallaştırma (hiper yönetici) altyapısıdır. Android x86 birlikte Intel ve resmi Android SDK Yöneticisi tarafından HAXM VT etkin sistemlerde daha hızlı Android öykünmesi için izin veren koşuluyla öykünücü görüntüler. 

VT yetenekleri olan bir Intel CPU bir makinesinde geliştiriyorsanız, büyük ölçüde Google Android öykünücüsü hızlandırmak için HAXM yararlanabilir (, CPU VT destekleyip desteklemediğini emin değilseniz bkz [belirlemek, bilgisayarınızı işlemci destekleyen Intel Sanallaştırma Teknolojisi](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Bir VM hızlandırılmış öykünücüsü VirtualBox, VMWare veya Docker tarafından barındırılan bir VM'nin gibi başka bir VM içinde çalıştırılamaz. Google Android öykünücüsü çalıştırmalısınız [sistem donanımınız üzerinde doğrudan](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Google Android öykünücüsü ilk kez kullanmadan önce HAXM yüklenir ve Google Android öykünücüsü kullanılabilir olduğunu doğrulamak için iyi bir fikirdir.

### <a name="verifying-haxm-installation"></a>HAXM yüklemesini doğrulama

HAXM görüntüleyerek kullanılabilir olup olmadığını kontrol edebilirsiniz **başlangıç Android öykünücüsü** öykünücü başlatılırken penceresi. Google Android öykünücüsü başlatmak için aşağıdakileri yapın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Tıklayarak Android öykünücüsü yöneticisini başlatmak **Araçlar > Android > Android Emulator Manager**:

    [![Android Emulator Manager menü öğesi konumu](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Görürseniz bir **Performans Uyarısı** iletişim HAXM henüz yüklenmemiş veya bilgisayarınızda düzgün yapılandırılmamış sonra aşağıdakine benzer:

    ![Performans Uyarısı iletişim kutusu HAXM hazır değil](hardware-acceleration-images/win/11-perf-warn.png)

   Varsa bir **Performans Uyarısı** iletişim şöyle gösterilir, bkz: [performans uyarıları](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) sorunun kaynağını belirlemek ve arka plandaki sorunu çözmek için.

3. Seçin bir **x86** görüntü (örneğin, **Visual Studio\_android 23\_x86\_telefon**), tıklatın **Başlat**, 'ıtıklatın **Başlatma**:

    ![Google Android öykünücüsü bir varsayılan sanal aygıt görüntüsüyle başlatılıyor](hardware-acceleration-images/win/02-start-default-avd.png)

4. İzlemesi **başlangıç Android öykünücüsü** öykünücü başlatılırken iletişim penceresi. HAXM yüklediyseniz iletisini görür **HAX çalıştığından ve öykünücüsü hızlı Sanal Küp Str modunda çalıştırır** bu ekran görüntüsünde gösterildiği gibi:

    ![HAXM başlangıç Android öykünücüsü iletişim kutusunda kullanılabilir olarak gösterilir](hardware-acceleration-images/win/03-haxm-detected.png)

   Bu iletiyi görmüyorsanız HAXM yüklü olmayabilir. Örneğin, bir ekran görüntüsünü HAXM kullanılabilir olup olmadığını görebilirsiniz bir ileti şöyledir:

    ![HAXM bu makinede kullanılabilir değil](hardware-acceleration-images/win/04-haxm-error.png)

   HAXM bilgisayarınızda kullanılabilir durumda değilse, HAXM yüklemek için sonraki bölümde adımları kullanın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Tıklayarak Android öykünücüsü yöneticisini başlatmak **Araçlar > Google öykünücü yöneticisini**:

    [![Android Emulator Manager menü öğesi konumu](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Görürseniz bir **Performans Uyarısı** iletişim HAXM henüz yüklenmemiş veya bilgisayarınızda düzgün yapılandırılmamış sonra aşağıdakine benzer:

    ![Performans Uyarısı iletişim kutusu HAXM hazır değil](hardware-acceleration-images/mac/04-avd-warning.png)

   Varsa bir **Performans Uyarısı** iletişim şöyle gösterilir, bkz: [performans uyarıları](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) sorunun kaynağını belirlemek ve arka plandaki sorunu çözmek için.

3. Seçin **x86** görüntü (örneğin, **Android\_hızlandırılmış\_x86**), tıklatın **Başlat**, ardından **başlatma**:

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