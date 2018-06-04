---
title: Google Android öykünücüsü sorunlarını giderme
description: Tanımlamak ve Google Android öykünücüsü sorunları gidermek nasıl
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: 73d0e578a0cf8ea6c0a62d8e21809cdab4b20910
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732368"
---
# <a name="google-android-emulator-troubleshooting"></a>Google Android öykünücüsü sorunlarını giderme

_Bu makalede, en yaygın uyarı iletilerini ve Google Android öykünücüsü çalıştırılırken oluşan sorunları, geçici çözümler ve ipuçları ile birlikte açıklanmaktadır. Öykünücü Kurulum sırasında sorunları giderme hakkında daha fazla bilgi için bkz: [öykünücüsü kurulum sorunlarını giderme](~/android/get-started/installation/android-emulator/troubleshooting.md)._

<a name="perfwarn" />

## <a name="performance-warnings"></a>Performans Uyarıları

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Google Android öykünücüsü ilk kez uygulama dağıttığınızda Visual Studio 2017 sürüm 15.4 başlayarak, bir performans Uyarısı iletişim kutusu görüntülenebilir. Bu uyarı iletişim kutuları, aşağıda açıklanmıştır.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Bilgisayar bir Intel Procesor içermiyor

![Bilgisayar bir Intel işlemci içermiyor](troubleshooting-images/01-no-intel-processor.png)

Bu iletişim kutusu görüntülendiğinde, bilgisayarınızı Android SDK öykünücüsü hızlandırması için gerekli olduğu bir Intel işlemci yok. Bilgisayarınızı bir Intel işlemci yoksa, fiziksel bir Android cihaz geliştirme için kullanmanızı öneririz.

### <a name="hyper-v-is-installed-or-active"></a>Hyper-V yüklü veya etkin değil

![Hyper-V yüklü veya etkin değil](troubleshooting-images/02-hyper-v-active.png)

Bu iletişim kutusu görüntülendiğinde, Hyper-V yüklü veya etkin ve devre dışı bırakılması gerekir. [Hyper-V devre dışı bırakma](#disable-hyperv) bu sorunun nasıl çözüleceği açıklanmaktadır.

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


Varsa `STATE` ayarlanmazsa `RUNNING`, bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmaya nasıl](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) sorunu gidermek için.


### <a name="other-failures"></a>Diğer hatalar

![Diğer hatalar](troubleshooting-images/05-other-failure.png)

Bilgisayarınızda Intel işlemci varsa, Hyper-V devre dışıdır, Intel HAXM yüklü, HAXM işlemin çalıştığı ancak bilinmeyen bir nedenden dolayı başlatmak öykünücü başarısız olursa bu iletişim kutusu görüntülenir.
Bu hatayı gidermek için bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmaya nasıl](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Performans uyarıları devre dışı bırakma

Bunun yerine performans uyarıları görmek istemiyorsanız, bunları devre dışı bırakabilirsiniz. Visual Studio'da'ı tıklatın **Araçlar > Seçenekler > Xamarin > Android ayarları** ve devre dışı **AVD hızlandırma değilse uyar (HAXM) desteklenen** seçeneği:

[![AVD hızlandırma uyarılarını devre dışı bırakma](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)



# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Google Android öykünücüsü ilk kez uygulama dağıttığınızda Mac yapı (559 yapı) 7.2 Visual Studio ile başlayarak, bir performans Uyarısı iletişim kutusu görüntülenebilir. Bu uyarı iletişim kutuları, aşağıda açıklanmıştır.

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


## <a name="deployment-issues"></a>Dağıtım sorunları

APK öykünücüsü üzerinde yükleme hatası veya Android hata ayıklama köprüsü çalıştırma hatası hakkında bir hata alırsanız (**adb**), Android SDK'sı, öykünücüsünü bağlanabildiğini doğrulayın. Bunu yapmak için aşağıdaki adımları kullanın:

1. Öykünücüsünden başlatma **Android Aygıt Yöneticisi'ni** (sanal Cihazınızı seçin ve tıklatın **Başlat**).

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


## <a name="haxm-issues"></a>HAXM sorunları

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Google Android öykünücüsü düzgün başlamazsa, bu genellikle HAXM sorunlara neden olur. HAXM sorunları genellikle diğer sanallaştırma teknolojilerini, ayarları yanlış veya eski bir HAXM sürücü çakışıyor sonucudur.

<a name="virt-conflicts" />

### <a name="haxm-virtualization-conflicts"></a>HAXM sanallaştırma çakışmaları

Hyper-V, Windows cihaz koruyucusu ve bazı virüsten koruma yazılımı gibi sanallaştırma kullanan diğer teknolojilerle HAXM çakışabilir:

- **Hyper-V** &ndash; önce Windows sürümünü kullanıyorsanız, **Windows 10 Nisan 2018 Güncelleştirmesi (yapı 1803)** ve Hyper-V etkin olduğunda, adımları [devre dışı bırakma Hyper-V](#disable-hyperv).

- **Cihaz koruyucusu** &ndash; Device Guard ve kimlik bilgisi koruma engelleyebilir Hyper-V Windows makinelerde devre dışı. Cihaz koruyucusu ve kimlik bilgisi koruma devre dışı bırakmak için bkz: [devre dışı bırakma Device Guard](#disable-devguard).

- **Virüsten koruma yazılımı** &ndash; donanım destekli sanallaştırma (örneğin, Avast) kullanan virüsten koruma yazılımı çalıştırıyorsanız, devre dışı bırakın veya bu yazılım, yeniden başlatma ve yeniden deneme Android SDK öykünücüsü kaldırın.


### <a name="incorrect-bios-settings"></a>Yanlış BIOS ayarları

Bir Windows Bilgisayarına HAXM kullanıyorsanız, sanallaştırma teknolojisi (Intel VT-x) BIOS'ta etkin değilse HAXM çalışmaz. VT-x devre dışıysa, Google Android öykünücüsü başlatmaya çalıştığınızda, hata aşağıdakine benzer alırsınız:

**Bu bilgisayar HAXM gereksinimlerini karşılayan ancak Intel Sanallaştırma Teknolojisi'ni (VT-x) açık değil.**

Bu hatayı düzeltmek için bilgisayarın BIOS içinde önyükleme, VT-x ve SLAT (ikinci düzey adres çevirisi) etkinleştirmek ve geri Windows bilgisayarı yeniden başlatın.

<a name="disable-hyperv" />

### <a name="disabling-hyper-v"></a>Hyper-V devre dışı bırakma

Daha önce Windows sürümünü kullanıyorsanız, **Windows 10 Nisan 2018 güncelleştirme (yapı 1803)** ve Hyper-V etkinleştirildiğinde, Hyper-V devre dışı bırakın ve yükleyip HAXM kullanabilmeniz için bilgisayarınızı yeniden başlatın. Kullanıyorsanız **Windows 10 Nisan 2018 güncelleştirme (yapı 1803)** ya da daha sonra Google Android öykünücüsü sürüm 27.2.7 veya daha sonra Hyper-V devre dışı bırakmak gerekli değildir (yerine HAXM) Hyper-V donanım hızlandırmasını kullanabilirsiniz.

Aşağıdaki adımları izleyerek Hyper-V Denetim Masası'ndan devre dışı bırakabilirsiniz:

1. Windows Arama kutusuna **programları ve** ardından **programlar ve Özellikler** arama sonucu.

2. Denetim Masası'nda **programlar ve Özellikler** iletişim kutusunda, tıklatın **kapatma Windows özelliklerini aç veya Kapat**:

    ![Windows özelliklerini aç veya kapat.](troubleshooting-images/win/07-turn-windows-features.png)

3. İşaretini **Hyper-V** ve bilgisayarı yeniden başlatın:

    ![Hyper-V Windows özellikleri iletişim kutusunda devre dışı bırakma](troubleshooting-images/win/08-uncheck-hyper-v.png)

Alternatif olarak, Hyper-V: devre dışı bırakmak için aşağıdaki Powershell cmdlet'ini kullanabilirsiniz

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM ve Microsoft Hyper-V her ikisi de aynı anda etkin olamaz. Ne yazık ki, arasında Hyper-V arasında HAXM, bilgisayarı yeniden başlatmadan geçiş yapmak için bir yolu yoktur. Visual Studio 2015 kullanmak istiyorsanız, [Android için Visual Studio öykünücüsü](~/android/deploy-test/debugging/visual-studio-android-emulator.md) (bağlı olan Hyper-V), Google Android öykünücüsü başlatmadan kullanmak mümkün olmayacaktır. Windows yükseltme etmektir bu sorunu gidermek için tek yönlü **Windows 10 Nisan 2018 güncelleştirme (yapı 1803)** veya üstü ve Hyper-V için her iki Öykünücüler kullanma (bkz [öykünücüsü performans donanım hızlandırmasını](~/android/get-started/installation/android-emulator/hardware-acceleration.md)).
Hyper-V ve HAXM açıklandığı gibi bir çok önyükleme Kurulum oluşturarak kullanmak için başka bir yoldur [herhangi bir hiper yönetici önyükleme girişi oluşturma](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/).

Bazı durumlarda, yukarıdaki adımları kullanarak Hyper-V Device Guard ve kimlik bilgisi koruma etkinse, devre dışı bırakılırken başarısız olur. Hyper-V devre dışı bırakmak oluşturulamıyor (veya devre dışı görünüyor ancak HAXM yükleme yine başarısız olursa), Device Guard ve kimlik bilgisi koruma devre dışı bırakmak için sonraki bölümde adımları kullanın.

<a name="disable-devguard" />

### <a name="disabling-device-guard"></a>Cihaz koruyucusu devre dışı bırakma

Cihaz koruyucusu ve kimlik bilgisi koruma Hyper-V Windows makinelerde devre dışı engelleyebilirsiniz. Bu genellikle bir yapılandırılan ve sahibi olan bir kuruluş tarafından denetlenen etki alanına katılan makineler için sorunudur.
Windows 10'olmadığını görmek için aşağıdaki adımları kullanın. **Device Guard** çalışıyor:

1. İçinde **Windows Search**, türü **sistem bilgisi** başlatmak için **sistem bilgisi** uygulama.

2. İçinde **Sistem Özeti**, olmadığını görmek için Görünüm **cihaz koruyucusu sanallaştırma tabanlı güvenlik** bulunduğundan ve yer **çalıştıran** durumu:

   [![Cihaz koruyucusu mevcut ve çalışıyor](troubleshooting-images/win/09-device-guard-sml.png)](troubleshooting-images/win/09-device-guard.png#lightbox)

Cihaz koruyucusu etkinse, devre dışı bırakmak için aşağıdaki adımları kullanın:

1. Emin **Hyper-V** devre dışı bırakıldı (altında **Windows özelliklerini aç veya Kapat**) önceki bölümde açıklandığı gibi.

2. Windows Arama kutusuna yazın **gpedit** seçip **Grup İlkesi düzenleme** arama sonucu. Bu başlatır **yerel Grup İlkesi Düzenleyicisi'ni**.

3. İçinde **yerel Grup İlkesi Düzenleyicisi**, gitmek **bilgisayar yapılandırması > Yönetim Şablonları > Sistem > Device Guard**:

   [![Cihaz koruyucusu yerel Grup İlkesi Düzenleyicisi'nde](troubleshooting-images/win/10-group-policy-editor-sml.png)](troubleshooting-images/win/10-group-policy-editor.png#lightbox)

4. Değişiklik **kapatma üzerinde sanallaştırma tabanlı güvenlik** için **devre dışı** (yukarıda gösterildiği gibi) ve çıkış **yerel Grup İlkesi Düzenleyicisi'ni**.

5. Windows Arama kutusuna yazın **cmd**. Zaman **komut istemi** görünür arama sonuçlarında sağ **komut istemi** seçip **yönetici olarak çalıştır**.

6. Aşağıdaki komutlar bir komut istemi penceresine yapıştırın (durumunda sürücü **Z:** olduğu kullanın, bunun yerine kullanmak için kullanılmayan bir sürücü harfi seçin):

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. Bilgisayarınızı yeniden başlatın. Önyükleme ekranında, aşağıdakine benzer bir istem görmeniz gerekir:

   **Kimlik bilgisi koruma devre dışı bırakmak istiyor musunuz?**

   Kimlik bilgisi koruma istendiğinde devre dışı bırakmak için belirtilen tuşuna basın.

8. Bilgisayar yeniden başlatıldıktan sonra Hyper-V (önceki adımlarda açıklandığı gibi) devre dışı bırakıldığından emin olmak için yeniden denetleyin.

Hyper-V hala devre dışı değil ise, etki alanına katılmış bilgisayarınızın ilkeleri cihaz koruyucusu veya kimlik bilgisi koruma devre dışı engelleyebilir. Bu durumda, etki alanı yöneticiniz, kimlik bilgisi koruma dışında opt olanak sağlamak için bir muafiyet isteyebilir. Alternatif olarak, olmayan HAXM kullanmak için etki alanına katılmış bir bilgisayarı kullanabilirsiniz.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Google Android öykünücüsü düzgün başlamazsa, bu genellikle HAXM sorunlara neden olur. HAXM sorunları genellikle diğer sanallaştırma teknolojilerini, ayarları yanlış veya eski bir HAXM sürücü çakışıyor sonucudur. Ayrıntılı adımları kullanarak HAXM sürücüsünü yeniden yüklemeyi deneyin [yükleme HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----

