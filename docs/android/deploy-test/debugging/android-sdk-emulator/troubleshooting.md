---
title: "Android SDK öykünücüsü sorunlarını giderme"
description: "Tanımlamak ve Android SDK öykünücüsü sorunları gidermek nasıl"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 51cc7a4700e8cb3ece556b0ada841d70d5f2bb8b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="android-sdk-emulator-troubleshooting"></a>Android SDK öykünücüsü sorunlarını giderme

Bu makalede, en yaygın uyarı iletilerini ve Android SDK öykünücüsü (ve bunların çözümleri) ile ilgili sorunlar açıklanmıştır.
 
<a name="perfwarn" />

## <a name="performance-warnings"></a>Performans Uyarıları

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uygulamanızı Android SDK öykünücüsü ilk kez dağıttığınızda Visual Studio 2017 sürüm 15.4 başlayarak, bir performans Uyarısı iletişim kutusu görüntülenebilir. Bu uyarı iletişim kutuları, aşağıda açıklanmıştır.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Bilgisayar bir Intel Procesor içermiyor

![Bilgisayar bir Intel işlemci içermiyor](troubleshooting-images/01-no-intel-processor.png)

Bu iletişim kutusu görüntülendiğinde, bilgisayarınızı Android SDK öykünücüsü hızlandırması için gerekli olduğu bir Intel işlemci yok. Bilgisayarınızı bir Intel işlemci yoksa, fiziksel bir Android cihaz geliştirme için kullanmanızı öneririz.

### <a name="hyper-v-is-installed-or-active"></a>Hyper-V yüklü veya etkin değil

![Hyper-V yüklü veya etkin değil](troubleshooting-images/02-hyper-v-active.png)

Bu iletişim kutusu görüntülendiğinde, Hyper-V yüklü veya etkin ve devre dışı bırakılması gerekir. [Hyper-V devre dışı bırakma](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv) bu sorunun nasıl çözüleceği açıklanmaktadır. 

### <a name="haxm-is-not-installed"></a>HAXM yüklü değil

![HAXM yüklü değil](troubleshooting-images/03-haxm-not-installed.png)

Bilgisayarınızda Intel işlemci varsa, Hyper-V devre dışı ancak HAXM yüklenmemiş bu iletişim kutusunu gösterir.
[HAXM yükleme](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) HAXM yüklemek için gereken adımları açıklar.

### <a name="haxm-process-not-running"></a>HAXM işlemi çalışmıyor

![HAXM işlemi çalışmıyor](troubleshooting-images/04-haxm-process-not-running.png)

Bilgisayarınızda Intel işlemci varsa, Hyper-V devre dışıdır, Intel HAXM yüklü, ancak HAXM işlemi çalışmıyor bu iletişim kutusu görüntülenir. Bu sorunu çözmek için bir komut istemi penceresi açın ve aşağıdaki komutu girin:

```cmd
sc query intelhaxm
```

HAXM işlem çalışıyorsa, aşağıdakine benzer bir çıktı görmeniz gerekir:

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```


Varsa **durumu** ayarlanmazsa **çalıştıran**, bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmaya nasıl](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) sorunu gidermek için.


### <a name="other-failures"></a>Diğer hatalar

![Diğer hatalar](troubleshooting-images/05-other-failure.png)

Bilgisayarınızda Intel işlemci varsa, Hyper-V devre dışıdır, Intel HAXM yüklü, HAXM işlemin çalıştığı ancak bilinmeyen bir nedenden dolayı başlatmak öykünücü başarısız olursa bu iletişim kutusu görüntülenir.
Bu hatayı gidermek için bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmaya nasıl](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Performans uyarıları devre dışı bırakma

Bunun yerine performans uyarıları görmek istemiyorsanız, bunları devre dışı bırakabilirsiniz. Visual Studio'da'ı tıklatın **Araçlar > Seçenekler > Xamarin > Android ayarları** ve devre dışı **AVD hızlandırma değilse uyar (HAXM) desteklenen** seçeneği:

[![AVD hızlandırma uyarılarını devre dışı bırakma](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Uygulamanızı Android SDK öykünücüsü ilk kez dağıttığınızda Mac yapı (559 yapı) 7.2 Visual Studio ile başlayarak, bir performans Uyarısı iletişim kutusu görüntülenebilir. Bu uyarı iletişim kutuları, aşağıda açıklanmıştır.

### <a name="haxm-is-not-installed"></a>HAXM yüklü değil

![HAXM yüklü değil](troubleshooting-images/03-haxm-not-installed.png)

Bu iletişim kutusunu HAXM yüklü olmadığını gösterir.
[HAXM yükleme](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) HAXM yüklemek için gereken adımları açıklar.

### <a name="haxm-process-not-running"></a>HAXM işlemi çalışmıyor

![HAXM işlemi çalışmıyor](troubleshooting-images/04-haxm-process-not-running.png)

HAXM işlem çalışmıyorsa, bu iletişim kutusu görüntülenir. Bu sorunu gidermenize yardımcı olmak ayrıntılı bilgi için bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmaya nasıl](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) sorunu gidermek için.

### <a name="other-failures"></a>Diğer hatalar

![Diğer hatalar](troubleshooting-images/05-other-failure.png)

Öykünücü bilinmeyen bir nedenden dolayı başlayamazsa bu iletişim kutusu görüntülenir. Bu hatayı gidermek için bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmaya nasıl](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) sorunu gidermek için.

-----

<a name="solutions" />

## <a name="solutions-to-common-problems"></a>Sık karşılaşılan sorunların çözümleri

Birçok ortak Android SDK öykünücüsü sorunları, bilgisayarınızdaki yapılandırma değişikliklerini yaparak veya ek yazılım yükleyerek çözülebilir. Aşağıdaki bölümlerde, bu sorunları tanımlamak ve çözümleri sunar.

<a name="deployment" />

### <a name="deployment-issues"></a>Dağıtım sorunları

APK öykünücüsü üzerinde yükleme hatası veya Android hata ayıklama köprüsü çalıştırma hatası hakkında bir hata alırsanız (**adb**), Android SDK'sı, öykünücüsünü bağlanabildiğini doğrulayın. Bunu yapmak için aşağıdaki adımları kullanın:

1. Öykünücüsünden başlatma **Android sanal cihazı (AVD) Yöneticisi'ni** (sanal Cihazınızı seçin ve tıklatın **Başlat**).

2. Bir komut istemi açın ve klasöre gidin nerede **adb** yüklenir. Örneğin, Windows, bu en olabilir: **C:\\Program Files (x86)\\Android\\android sdk\\platformu Araçları\\adb.exe**.

3. Şu komutu yazın:

   ```shell
   adb devices
   ```

4. Android SDK öykünücüsü erişilebilen, öykünücü bağlı aygıtlar listesinde görünmesi gerekir. Örneğin:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Öykünücü bu listede görünmüyorsa, başlangıç **Android SDK Manager**, tüm güncelleştirmeleri uygulamanızı ve ardından öykünücü yeniden başlatmayı deneyin.


<a name="haxm-issues" />

### <a name="haxm-issues"></a>HAXM sorunları

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android SDK öykünücüsü düzgün başlamazsa, bu genellikle HAXM sorunlardan kaynaklanır. HAXM sorunları genellikle diğer sanallaştırma teknolojilerini, ayarları yanlış veya eski bir HAXM sürücü çakışıyor sonucudur.

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>HAXM sanallaştırma çakışmaları

Hyper-V, Windows cihaz koruyucusu ve bazı virüsten koruma yazılımı gibi sanallaştırma kullanan diğer teknolojilerle HAXM çakışabilir:

- **Hyper-V** &ndash; Windows Hyper-V etkin ile kullanıyorsanız, adımları [devre dışı bırakma Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv).

- **Cihaz koruyucusu** &ndash; Device Guard ve kimlik bilgisi koruma engelleyebilir Hyper-V Windows makinelerde devre dışı. Cihaz koruyucusu ve kimlik bilgisi koruma devre dışı bırakmak için bkz: [devre dışı bırakma Device Guard](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-devguard).

- **Virüsten koruma yazılımı** &ndash; donanım destekli sanallaştırma (örneğin, Avast) kullanan virüsten koruma yazılımı çalıştırıyorsanız, devre dışı bırakın veya bu yazılım, yeniden başlatma ve yeniden deneme Android SDK öykünücüsü kaldırın.

<a name="bios" />

#### <a name="incorrect-bios-settings"></a>Yanlış BIOS ayarları

Bir Windows Bilgisayarına HAXM kullanıyorsanız, sanallaştırma teknolojisi (Intel VT-x) BIOS'ta etkin değilse HAXM çalışmaz. VT-x devre dışıysa, Android SDK öykünücüsü başlatmaya çalıştığınızda, hata aşağıdakine benzer alırsınız:

**Bu bilgisayar HAXM gereksinimlerini karşılayan ancak Intel Sanallaştırma Teknolojisi'ni (VT-x) açık değil.**

Bu hatayı düzeltmek için bilgisayarın BIOS içinde önyükleme, VT-x ve SLAT (ikinci düzey adres çevirisi) etkinleştirmek ve geri Windows bilgisayarı yeniden başlatın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Android SDK öykünücüsü düzgün başlamazsa, bu genellikle HAXM sorunlardan kaynaklanır. HAXM sorunları genellikle diğer sanallaştırma teknolojilerini, ayarları yanlış veya eski bir HAXM sürücü çakışıyor sonucudur. Ayrıntılı adımları kullanarak HAXM sürücüsünü yeniden yüklemeyi deneyin [yükleme HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----
