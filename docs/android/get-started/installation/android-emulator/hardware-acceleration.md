---
title: "Android öykünücüsünde donanım hızlandırma"
description: "Bilgisayarınızı maksimum Android SDK öykünücüsü performans için hazırlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/22/2017
ms.openlocfilehash: 7560900ace62a737ac765bcfe93f759f8985aca2
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Android öykünücüsünde donanım hızlandırma

Android SDK öykünücüsü donanım hızlandırmasını, Intel's şekilde basımı karşılamayacak kadar yavaş olduğu için HAXM (donanım hızlandırılmış yürütme Yöneticisi) Android SDK öykünücüsü performansını önemli ölçüde artırmak için önerilen yöntem olduğu.


## <a name="haxm-overview"></a>HAXM genel bakış

HAXM bir konak makinesi üzerinde Android uygulaması öykünme hızlandırmak için Intel Sanallaştırma Teknolojisi (VT) kullanan bir donanım destekli sanallaştırma (hiper yönetici) altyapısıdır. Android x86 birlikte Intel ve resmi Android SDK Yöneticisi tarafından HAXM VT etkin sistemlerde daha hızlı Android öykünmesi için izin veren koşuluyla öykünücü görüntüler. VT yetenekleri olan bir Intel CPU bir makinesinde geliştiriyorsanız, büyük ölçüde Android SDK öykünücüsü hızlandırmak için HAXM yararlanabilir (, CPU VT destekleyip desteklemediğini emin değilseniz bkz [belirlemek, bilgisayarınızı işlemci destekleyen Intel Sanallaştırma Teknolojisi](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

Android SDK öykünücüsü otomatik olarak kullanılabilir olduğunda HAXM kullanır. Seçtiğinizde, bir **x86**-sanal aygıt dayalı (açıklandığı gibi [yapılandırma ve kullanım](~/android/deploy-test/debugging/android-sdk-emulator/index.md)), sanal cihazın donanım hızlandırmasını HAXM kullanır. Android SDK öykünücüsü ilk kez kullanmadan önce HAXM yüklenir ve Android SDK öykünücüsü kullanılabilir olduğunu doğrulamak için iyi bir fikirdir.

> [!NOTE]
> Bir sanal makinede HAXM çalıştırılamıyor.


## <a name="verifying-haxm-installation"></a>HAXM yüklemesini doğrulama

HAXM görüntüleyerek kullanılabilir olup olmadığını kontrol edebilirsiniz **başlangıç Android öykünücüsü** öykünücü başlatılırken penceresi. Android SDK öykünücüsü başlatmak için aşağıdakileri yapın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Tıklayarak Android öykünücüsü yöneticisini başlatmak **Araçlar > Android > Android Emulator Manager**:

    [![Android Emulator Manager menü öğesi konumu](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Görürseniz bir **Performans Uyarısı** iletişim HAXM henüz yüklenmemiş veya bilgisayarınızda düzgün yapılandırılmamış sonra aşağıdakine benzer:

    ![Performans Uyarısı iletişim kutusu HAXM hazır değil](hardware-acceleration-images/win/11-perf-warn.png)

   Varsa bir **Performans Uyarısı** iletişim şöyle gösterilir, bkz: [performans uyarıları](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) sorunun kaynağını belirlemek ve arka plandaki sorunu çözmek için.

3. Seçin bir **x86** görüntü (örneğin, **Visual Studio\_android 23\_x86\_telefon**), tıklatın **Başlat**, 'ıtıklatın **Başlatma**:

    ![Bir varsayılan sanal aygıt görüntüsüyle Android SDK öykünücüsü başlatılıyor](hardware-acceleration-images/win/02-start-default-avd.png)

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

    [![Bir varsayılan sanal aygıt görüntüsüyle Android SDK öykünücüsü başlatılıyor](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. İzlemesi **başlangıç Android öykünücüsü** öykünücü başlatılırken iletişim penceresi. HAXM yüklediyseniz iletisini görür **HAX çalıştığından ve öykünücüsü hızlı Sanal Küp Str modunda çalıştırır** bu ekran görüntüsünde gösterildiği gibi:

    ![HAXM başlangıç Android öykünücüsü iletişim kutusunda kullanılabilir olarak gösterilir](hardware-acceleration-images/mac/03-haxm-detected.png)

   HAXM bilgisayarınızda kullanılabilir değilse (örneğin, benzer bir hata iletisi görürseniz _Lütfen Intel HAXM propertly yüklü ve kullanılabilir olduğundan emin olun_), HAXM yüklemek için sonraki bölümde adımları kullanın.


-----

<a name="install-haxm" />

## <a name="installing-haxm"></a>HAXM yükleme

Öykünücü başlamazsa HAXM el ile yüklenmesi gerekebilir. Hem Windows hem de macOS edinilebilir için HAXM yükleme paketleri [Intel donanım hızlandırılmış yürütme Yöneticisi](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) sayfası. İndirip HAXM el ile yüklemek için aşağıdaki adımları kullanın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Intel Web sitesinden en son karşıdan yüklemek [HAXM sanallaştırma altyapısı](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Windows Installer. Doğrudan Intel Web sitesinden HAXM yükleyicisi indiriliyor avantajı, en son sürümünü kullanarak da gösterilmeyeceği olmasıdır.

   Alternatif olarak, HAXM yükleyici indirmek için SDK Yöneticisi'ni kullanabilirsiniz (SDK Yöneticisi'nde **Araçlar > Ek Özellikler > Intel x86 öykünücüsü Hızlandırıcı (HAXM Yükleyici)**). Android SDK HAXM yükleyici normalde aşağıdaki konuma yükler:

   **C:\\Program Files (x86)\\Android\\android sdk\\ek özellikler\\Intel\\donanım\_hızlandırılmış\_yürütme\_Yöneticisi**

   SDK Yöneticisi HAXM yüklemez, yalnızca yukarıdaki konuma HAXM yükleyici yükler unutmayın; el ile başlatma çözümlenmedi.

2. Çalıştırma **intelhaxm android.exe** HAXM yükleyiciyi başlatmak için. Yükleyici iletişim kutularını varsayılan değerleri kabul edin:

   ![Intel donanım hızlandırılmış yürütme Yöneticisi Kurulum penceresi](hardware-acceleration-images/win/05-haxm-installer.png)

Aşağıdaki hata iletişim kutusu görürseniz (_bu bilgisayar Intel Sanallaştırma Teknolojisi'ni (VT-x) desteklemez veya özel olarak Hyper-V tarafından kullanılan_), HAXM yükleyebilmek için önce Hyper-V devre dışı gerekir:

![Hyper-V çakışma nedeniyle HAXM yüklenemez.](hardware-acceleration-images/win/06-cant-install-haxm.png)

Sonraki bölümde, Hyper-V devre dışı bırakılmasını açıklar.

<a name="disable-hyperv" />

## <a name="disabling-hyper-v"></a>Hyper-V devre dışı bırakma

Windows Hyper-V etkin ile kullanıyorsanız, devre dışı bırakın ve yükleyip HAXM kullanabilmeniz için bilgisayarınızı yeniden başlatın. Aşağıdaki adımları izleyerek Hyper-V Denetim Masası'ndan devre dışı bırakabilirsiniz:

1. Windows Arama kutusuna **programları ve** ardından **programlar ve Özellikler** arama sonucu.

2. Denetim Masası'nda **programlar ve Özellikler** iletişim kutusunda, tıklatın **kapatma Windows özelliklerini aç veya Kapat**:

    ![Windows özelliklerini aç veya kapat.](hardware-acceleration-images/win/07-turn-windows-features.png)

3. İşaretini **Hyper-V** ve bilgisayarı yeniden başlatın:

    ![Hyper-V Windows özellikleri iletişim kutusunda devre dışı bırakma](hardware-acceleration-images/win/08-uncheck-hyper-v.png)

Alternatif olarak, Hyper-V: devre dışı bırakmak için aşağıdaki Powershell cmdlet'ini kullanabilirsiniz

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM ve Microsoft Hyper-V her ikisi de aynı anda etkin olamaz. Ne yazık ki, şu anda arasında Hyper-V arasında HAXM, bilgisayarı yeniden başlatmadan geçiş yapmak için bir yolu yoktur. Kullanmak istiyorsanız, [Android için Visual Studio öykünücüsü](~/android/deploy-test/debugging/visual-studio-android-emulator.md) (bağlı olan Hyper-V), Android SDK öykünücüsü başlatmadan kullanmak mümkün olmayacaktır. Hyper-V ve HAXM kullanmanın tek yolu olan bir çoklu önyükleme kurulumu açıklandığı gibi oluşturmak için [herhangi bir hiper yönetici önyükleme girişi oluşturma](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/).

Bazı durumlarda, yukarıdaki adımları kullanarak Hyper-V Device Guard ve kimlik bilgisi koruma etkinse, devre dışı bırakılırken başarısız olur. Hyper-V devre dışı bırakmak oluşturulamıyor (veya devre dışı görünüyor ancak HAXM yükleme yine başarısız olursa), Device Guard ve kimlik bilgisi koruma devre dışı bırakmak için sonraki bölümde adımları kullanın.

<a name="disable-devguard" />

## <a name="disabling-device-guard"></a>Cihaz koruyucusu devre dışı bırakma

Cihaz koruyucusu ve kimlik bilgisi koruma Hyper-V Windows makinelerde devre dışı engelleyebilirsiniz. Bu genellikle bir yapılandırılan ve sahibi olan bir kuruluş tarafından denetlenen etki alanına katılan makineler için sorunudur.
Windows 10'olmadığını görmek için aşağıdaki adımları kullanın. **Device Guard** çalışıyor:

1. İçinde **Windows Search**, türü **sistem bilgisi** başlatmak için **sistem bilgisi** uygulama.

2. İçinde **Sistem Özeti**, olmadığını görmek için Görünüm **cihaz koruyucusu sanallaştırma tabanlı güvenlik** bulunduğundan ve yer **çalıştıran** durumu:

   [![Cihaz koruyucusu mevcut ve çalışıyor](hardware-acceleration-images/win/09-device-guard-sml.png)](hardware-acceleration-images/win/09-device-guard.png#lightbox)

Cihaz koruyucusu etkinse, devre dışı bırakmak için aşağıdaki adımları kullanın:

1. Emin **Hyper-V** devre dışı bırakıldı (altında **Windows özelliklerini aç veya Kapat**) önceki bölümde açıklandığı gibi.

2. Windows Arama kutusuna yazın **gpedit** seçip **Grup İlkesi düzenleme** arama sonucu. Bu başlatır **yerel Grup İlkesi Düzenleyicisi'ni**.

3. İçinde **yerel Grup İlkesi Düzenleyicisi**, gitmek **bilgisayar yapılandırması > Yönetim Şablonları > Sistem > Device Guard**:

   [![Cihaz koruyucusu yerel Grup İlkesi Düzenleyicisi'nde](hardware-acceleration-images/win/10-group-policy-editor-sml.png)](hardware-acceleration-images/win/10-group-policy-editor.png#lightbox)

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

1. Intel Web sitesinden en son karşıdan yüklemek [HAXM sanallaştırma altyapısı](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) macOS yükleyici.

2. HAXM yükleyiciyi çalıştırın. Yükleyici iletişim kutularını varsayılan değerleri kabul edin:

   [![Intel donanım hızlandırılmış yürütme Yöneticisi Kurulum penceresi](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----
