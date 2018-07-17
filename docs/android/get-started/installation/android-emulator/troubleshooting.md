---
title: Android öykünücüsü sorunlarını giderme
description: Bu makalede, tanılayın ve Android öykünücüsü'nü kullanırken oluşabilecek sorunları çalışma şekli açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 1d13a3dae509fea4a2e955c4ad206a81a57e75ed
ms.sourcegitcommit: 6433b424410a850f504e0f934bbb5baf8f093e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067340"
---
# <a name="android-emulator-troubleshooting"></a>Android öykünücüsü sorunlarını giderme

_Bu makalede, Android öykünücüsü'nü çalıştıran ve yapılandırma sırasında oluşabilecek sorunları ve en yaygın uyarı iletilerini, geçici çözümlere ve ipuçları ile birlikte açıklanmıştır._

<a name="perfwarn" />

## <a name="performance-warnings"></a>Performans Uyarıları

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android öykünücüsü'nü ilk kez uygulama dağıttığınızda Visual Studio 2017 sürüm 15.4 ile başlayarak, bir performans Uyarısı iletişim kutusu görüntülenebilir. Bu uyarı iletişim kutuları, aşağıda açıklanmıştır.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Bilgisayar bir Intel Procesor içermiyor

![Bilgisayar bir Intel işlemci içermiyor](troubleshooting-images/01-no-intel-processor.png)

Bu iletişim kutusu görüntülendiğinde, bilgisayarınızı Android SDK öykünücüsü hızlandırması için gerekli olduğu bir Intel işlemci yok. Bilgisayarınızı bir Intel işlemci yoksa, geliştirme için fiziksel bir Android cihazı kullanmanızı öneririz.

### <a name="hyper-v-is-installed-or-active"></a>Hyper-V yüklü veya etkin

![Hyper-V yüklü veya etkin](troubleshooting-images/02-hyper-v-active.png)

Bu iletişim kutusu görüntülendiğinde, Hyper-V yüklü ya da etkin ve devre dışı bırakılmalıdır. [Hyper-V'yi devre dışı](#disable-hyperv) bu sorunun nasıl çözüleceği açıklanmaktadır.

### <a name="haxm-is-not-installed"></a>HAXM yüklü olduğu

![HAXM yüklü değil](troubleshooting-images/03-haxm-not-installed.png)

Bilgisayarınızda Intel işlemcisi varsa, Hyper-V devre dışı bırakıldı ancak HAXM yüklü değil, bu iletişim kutusunu gösterir.
[HAXM Yükleniyor](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) HAXM yüklemek için gereken adımlar açıklanmaktadır.

### <a name="haxm-process-not-running"></a>HAXM işlemi çalışmıyor

![HAXM işlemi çalışmıyor](troubleshooting-images/04-haxm-process-not-running.png)

Bilgisayarınızda Intel işlemcisi varsa, Hyper-V devre dışı bırakıldı, Intel HAXM yüklü ancak HAXM işlemi çalışmıyor bu iletişim kutusu görüntülenir. Bu sorunu çözmek için bir komut istemi penceresi açın ve aşağıdaki komutu girin:

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

Varsa `STATE` ayarlı değil `RUNNING`, bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmayı](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) sorunu çözmek için.


### <a name="other-failures"></a>Diğer hatalar

![Diğer hatalar](troubleshooting-images/05-other-failure.png)

Bilgisayarınızda Intel işlemcisi varsa, Hyper-V devre dışı bırakıldı, Intel HAXM yüklü, HAXM işlem çalışıyor ancak bilinmeyen bir nedenden dolayı başlatmak öykünücü başarısız olursa bu iletişim kutusu görüntülenir.
Bu hatayı gidermek için bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmayı](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Performans uyarıları devre dışı bırakma

Bunun yerine performans uyarıları görmek istemiyorsanız, bunları devre dışı bırakabilirsiniz. Visual Studio'da **Araçlar > Seçenekler > Xamarin > Android ayarları** ve devre dışı bırakma **AVD hızlandırma değilse uyar (HAXM) desteklenen** seçeneği:

[![AVD hızlandırma uyarıları devre dışı bırakma](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Android öykünücüsü'nü ilk kez uygulama dağıttığınızda sürüm 7.2 (559 yapı) Mac için Visual Studio ile başlayarak, bir performans Uyarısı iletişim kutusu görüntülenebilir. Bu uyarı iletişim kutuları, aşağıda açıklanmıştır.

### <a name="haxm-is-not-installed"></a>HAXM yüklü olduğu

![HAXM yüklü değil](troubleshooting-images/03-haxm-not-installed.png)

HAXM yüklü değil, bu iletişim kutusunu gösterir.
[HAXM Yükleniyor](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) HAXM yüklemek için gereken adımlar açıklanmaktadır.

### <a name="haxm-process-not-running"></a>HAXM işlemi çalışmıyor

![HAXM işlemi çalışmıyor](troubleshooting-images/04-haxm-process-not-running.png)

HAXM işlem çalışır durumda değilse, bu iletişim kutusu görüntülenir. Bu sorunu gidermenize yardımcı olmak ayrıntılı bilgi için bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmayı](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) sorunu çözmek için.

### <a name="other-failures"></a>Diğer hatalar

![Diğer hatalar](troubleshooting-images/05-other-failure.png)

Öykünücü bilinmeyen bir nedenden dolayı başlatılamıyorsa bu iletişim kutusu görüntülenir. Bu hatayı gidermek için bkz: [Intel donanım hızlandırılmış yürütme Yöneticisi'ni kullanmayı](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) sorunu çözmek için.

-----

## <a name="deployment-issues"></a>Dağıtım sorunları

Emulator'da apk'yı yükleme hatası veya Android hata ayıklama köprüsü çalıştırma hatası hakkında bir hata alırsanız (**adb**), Android SDK'sı, öykünücüsü için bağlantı kurabildiğini doğrulayın. Bunu yapmak için aşağıdaki adımları kullanın:

1. Öykünücüsünden başlatma **Android cihaz Yöneticisi** (sanal Cihazınızı seçin ve tıklayın **Başlat**).

2. Bir komut istemi açın ve klasöre gidin. burada **adb** yüklenir. Örneğin, Windows üzerinde bu en olabilir: **C:\\Program dosyaları (x86)\\Android\\android sdk\\platform Araçları\\adb.exe**.

3. Şu komutu yazın:

   ```shell
   adb devices
   ```

4. Android SDK'sından öykünücü erişilebiliyorsa, öykünücü takılı cihazlar listesinde görünmelidir. Örneğin:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Öykünücü bu listede görünmüyorsa, başlangıç **Android SDK Yöneticisi**, tüm güncelleştirmeleri uygulayın ve ardından öykünücüyü yeniden başlatmayı deneyin.


## <a name="haxm-issues"></a>HAXM sorunları

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android öykünücüsü düzgün başlatılmazsa, bu genellikle HAXM ile ilgili sorunlar nedeniyle oluşur. HAXM sorunları genellikle diğer sanallaştırma teknolojilerini, ayarları yanlış veya süresi dolmuş bir HAXM sürücüsü ile çakışıyor sonucudur.

<a name="virt-conflicts" />

### <a name="haxm-virtualization-conflicts"></a>HAXM sanallaştırma çakışmaları

HAXM kullanan sanallaştırma, Hyper-V, Windows Device Guard ve bazı virüsten koruma yazılımı gibi diğer teknolojileri ile çakışabilir:

- **Hyper-V** &ndash; önce Windows sürümünü kullanıyorsanız **Windows 10 Nisan 2018 Güncelleştirmesi (derleme 1803)** ve Hyper-V etkin, adımları [Hyper-V devre dışı bırakma](#disable-hyperv).

- **Cihaz koruma** &ndash; Device Guard ve Credential Guard engelleyebilir Hyper-V Windows makinelerde devre dışı. Device Guard ve Credential Guard devre dışı bırakmak için bkz: [devre dışı bırakma Device Guard](#disable-devguard).

- **Virüsten koruma yazılımı** &ndash; donanım Yardımlı sanallaştırma (örneğin, Avast) kullanan virüsten koruma yazılımı çalıştırıyorsanız, devre dışı bırakın veya bu yazılım yeniden başlatma ve yeniden deneme Android SDK öykünücüsü'nü kaldırın.


### <a name="incorrect-bios-settings"></a>Yanlış BIOS ayarları

Bir Windows PC'de HAXM kullanıyorsanız, BIOS'ta sanallaştırma teknolojisi (Intel VT-x) etkinleştirilmediği HAXM çalışmıyor. VT-x devre dışıysa, Android öykünücüsünde başlatmak denediğinizde bir hata aşağıdakine benzer erişmenizi sağlayacak:

**Bu bilgisayar için HAXM gereksinimlerini karşılıyor ancak Intel Sanallaştırma Teknolojisi'ni (VT-x) açık değil.**

Bu hatayı düzeltmek için bilgisayarın BIOS içinde önyükleme, birlikte VT-x hem SLAT (ikinci düzey adres çevirisi) etkinleştirin ve ardından geri ile Windows bilgisayarı yeniden başlatın.

<a name="disable-hyperv" />

### <a name="disabling-hyper-v"></a>Hyper-V'yi devre dışı

Önce Windows sürümünü kullanıyorsanız **Windows 10 Nisan 2018 Güncelleştirmesi (derleme 1803)** ve Hyper-V etkin olduğunda, Hyper-V devre dışı bırakın ve HAXM yüklenir ve bilgisayarınızı yeniden başlatmanız gerekir. Kullanıyorsanız **Windows 10 Nisan 2018 Güncelleştirmesi (derleme 1803)** veya üzeri, Android öykünücüsü sürüm 27.2.7 veya Hyper-V devre dışı bırakmak gerekli değildir (HAXM) yerine Hyper-V için donanım hızlandırma, daha sonra kullanabilirsiniz.

Aşağıdaki adımları izleyerek Denetim Masası'ndan Hyper-V devre dışı bırakabilirsiniz:

1. Windows Arama kutusuna **programları ve** ardından **programlar ve Özellikler** arama sonucu.

2. Denetim Masası'nda **programlar ve Özellikler** iletişim kutusunda, tıklayın **kapatma Windows özelliklerini aç veya Kapat**:

    ![Windows özelliklerini aç veya kapat](troubleshooting-images/win/07-turn-windows-features.png)

3. Onay kutusunu temizleyin **Hyper-V** ve bilgisayarı yeniden başlatın:

    ![Windows özellikleri iletişim kutusunda Hyper-V devre dışı bırakma](troubleshooting-images/win/08-uncheck-hyper-v.png)

Alternatif olarak, Hyper-V'yi devre dışı bırakmak için aşağıdaki Powershell cmdlet'ini kullanabilirsiniz

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM ve Microsoft Hyper-V her ikisi de aynı anda etkin olamaz. Ne yazık ki arasında Hyper-V arasında HAXM, bilgisayarı yeniden başlatmadan geçiş yapmak için hiçbir yolu yoktur. 

Bazı durumlarda, yukarıdaki adımları kullanarak Hyper-V Device Guard ve Credential Guard etkinse devre dışı bırakılırken başarılı olmaz. Hyper-V devre dışı bırakmak oluşturulamıyor (veya devre dışı bırakılmış gibi görünüyor ancak HAXM yükleme yine başarısız olursa), Device Guard ve Credential Guard devre dışı bırakmak için sonraki bölümde adımları kullanın.

<a name="disable-devguard" />

### <a name="disabling-device-guard"></a>Cihaz koruma devre dışı bırakma

Device Guard ve Credential Guard Hyper-V Windows makinelerde devre dışı engelleyebilirsiniz. Bu genellikle yapılandırılan ve sahip olan bir kuruluş tarafından denetlenen etki alanına katılan makineler için bir sorundur.
Windows 10'da, olmadığını görmek için aşağıdaki adımları kullanın. **Device Guard** çalışıyor:

1. İçinde **Windows Search**, türü **sistem bilgisi** başlatmak için **sistem bilgileri** uygulama.

2. İçinde **Sistem Özeti**, olmadığını görmek için Görünüm **cihaz koruyucusu sanallaştırma tabanlı güvenlik** varsa ve içinde **çalıştıran** durumu:

   [![Cihaz koruma mevcut ve çalışıyor](troubleshooting-images/win/09-device-guard-sml.png)](troubleshooting-images/win/09-device-guard.png#lightbox)

Device Guard etkinse devre dışı bırakmak için aşağıdaki adımları kullanın:

1. Emin **Hyper-V** devre dışı bırakıldı (altında **Windows özelliklerini aç veya Kapat**) önceki bölümde açıklandığı gibi.

2. Windows Search kutusuna **gpedit** seçip **Grup İlkesi düzenleme** arama sonucu. Böylece **yerel Grup İlkesi Düzenleyicisi**.

3. İçinde **yerel Grup İlkesi Düzenleyicisi**, gitmek **bilgisayar yapılandırması > Yönetim Şablonları > Sistem > Device Guard**:

   [![Yerel Grup İlkesi Düzenleyicisi'nde cihaz koruma](troubleshooting-images/win/10-group-policy-editor-sml.png)](troubleshooting-images/win/10-group-policy-editor.png#lightbox)

4. Değişiklik **kapatma üzerinde sanallaştırma tabanlı güvenlik** için **devre dışı bırakılmış** (yukarıda gösterildiği gibi) ve çıkış **yerel Grup İlkesi Düzenleyicisi**.

5. Windows Search kutusuna **cmd**. Zaman **komut istemi** görünür sağ tıklayın arama sonuçlarına **komut istemi** seçip **yönetici olarak çalıştır**.

6. Aşağıdaki komutlar, komut istemi penceresine yapıştırın (durumunda sürücü **Z:** olan kullanın, bunun yerine kullanmak için kullanılmayan bir sürücü harfi seçin):

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. Bilgisayarınızı yeniden başlatın. Önyükleme ekranda bir komut istemi aşağıdaki gibi görmeniz gerekir:

   **Credential Guard ' ı devre dışı bırakmak istiyor musunuz?**

   Credential Guard istendiği gibi devre dışı bırakmak için belirtilen tuşuna basın.

8. Bilgisayar yeniden başlatıldıktan sonra Hyper-V (önceki adımlarda açıklandığı gibi) devre dışı emin olmak için tekrar denetleyin.

Hyper-V yine de devre dışı değil ise, etki alanına katılmış bilgisayar ilkeleri Device Guard veya Credential Guard devre dışı engelleyebilir. Bu durumda, etki alanı yöneticiniz, Credential Guard dışında iyileştirilmiş olanak sağlamak için bir muafiyet isteyebilir. Alternatif olarak, etki alanı ile birleşik HAXM kullanmak için olmayan bir bilgisayarda kullanabilirsiniz.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Android öykünücüsü düzgün başlatılmazsa, bu genellikle HAXM ile ilgili sorunlar nedeniyle oluşur. HAXM sorunları genellikle diğer sanallaştırma teknolojilerini, ayarları yanlış veya süresi dolmuş bir HAXM sürücüsü ile çakışıyor sonucudur. Ayrıntılı adımları kullanarak HAXM sürücüsü yeniden yüklemeyi deneyin [yükleme HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----

