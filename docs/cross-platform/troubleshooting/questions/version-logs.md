---
title: My sürüm bilgileri ve günlükleri nereden bulabilirim?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 9234ee0553b2cc9376c0e4e39ffc0700deaacda1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>My sürüm bilgileri ve günlükleri nereden bulabilirim?

## <a name="outline"></a>Anahat

- [Sürüm bilgileri](#version-information)
    - Windows sürüm bilgileri
    - Mac sürüm bilgileri
    - Android SDK'sı, platformunuzun Araçlar, yapı-Araçlar
- [IDE ve yükleyici günlükleri](#ide-and-installer-logs)
    - [Windows Günlükleri](#windows-logs)
        - Xamarin Studio
        - Visual Studio için Xamarin
        - Xamarin Evrensel yükleyici
        - Tek tek `.msi` yükleyiciler, ayrıntılı günlükleri
        - Visual Studio Başlangıç, ayrıntılı günlükleri
    - [Mac günlükleri](#mac-logs)
        - Ana derleme
    - Mac için Visual Studio
        - Xamarin Studio
        - Xamarin yükleyici
- [Ayrıntılı yapı çıktısı](#verbose-build-output-logs)
- [Hata ayıklama günlüklerini Xamarin.Android ve Xamarin.iOS uygulamaları için](#debug-logs-for-xamarin-apps)
    - Android `adb` logcat günlükleri
    - iOS simülatörü (Mac) günlüğe yazar.
    - iOS cihazı (Mac) günlüğe yazar.

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />Sürüm bilgileri

Çoğunlukla tüm bilgileri geri göndermek için en iyi **kopyalama bilgi** düğmeler. Aksi durumda biz genellikle ek bilgi istemek gerekir. Örneğin, işletim sistemi sürümleri, Xcode sürüm Android API düzeylerini yüklü ve .NET sürüm tüm bir sorunu gidermeye yardımcı olacak önemli olabilir.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Windows sürüm bilgileri

#### <a name="xamarin-studio"></a>Xamarin Studio

**Yardım > hakkında > Ayrıntıları Göster > kopyalama bilgileri [düğmesi]**

#### <a name="visual-studio"></a>Visual Studio

**Yardım > Microsoft Visual Studio hakkında > [düğmesi] Bilgi Kopyala**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac sürüm bilgileri

#### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

**Visual Studio > hakkında Visual Studio > Ayrıntıları Göster > kopyalama bilgileri [düğmesi]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK'sı, platformunuzun Araçlar, yapı-Araçlar

Android SDK Yöneticisi'ni açın ve bir ekran görüntüsünü üst ele **Araçları** bölümü.

![](https://kb.xamarin.com/customer/portal/attachments/337323 "Android SDK Yöneticisi'nin ekran görüntüsü > Araçlar klasörü")

#### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

**Araçlar > Android SDK Manager'ı açın**

#### <a name="visual-studio"></a>Visual Studio

SDK Manager araç simgesini seçin:

![](https://kb.xamarin.com/customer/portal/attachments/420238 "Visual Studio Başlangıç Android SDK Manager araç simgesi")

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE ve yükleyici günlükleri

Her günlük konumu için ZIP ve tüm günlük klasörü eklemek emin olun.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Windows Günlükleri

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Xamarin için Visual Studio Araçları

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Visual Studio yükleme günlüklerini alma](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Xamarin "Evrensel" Yükleyici

`%LOCALAPPDATA%\Xamarin\Universal`

Günlükleri bunlar `XamarinInstaller.exe` yükleyici.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Tek tek `.msi` yükleyiciler, ayrıntılı günlükleri

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Başvuru: [komut satırı seçenekleri](http://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Visual Studio Başlangıç, ayrıntılı günlükleri

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Başvuru:  [ /log (devenv.exe)](http://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac günlükleri

Seçebileceğiniz **Git > klasörüne gidin** menü öğesi Finder ve ardından kopyalayın ve bu yolların iletişim kutusuna yapıştırın.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Mac için Visual Studio

`~/Library/Logs/VisualStudio/7.0` (Bu sayı, kullanmakta olduğunuz sürümüne bağlı olarak değişebilir)

Bu klasör ayrıca aracılığıyla "Yardım açık günlük dizini ->" açılabilir.

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (Bu sayı, kullanmakta olduğunuz sürümüne bağlı olarak değişebilir)

Bu klasör ayrıca aracılığıyla "Yardım açık günlük dizini ->" açılabilir.

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Xamarin "Evrensel" Yükleyici

`~/Library/Logs/XamarinInstaller/Universal`

Günlükleri bunlar `XamarinInstaller.dmg` yükleyici.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Xamarin derleme ana bilgisayar

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Ayrıntılı yapı çıktısı

1.  Etkinleştirme [tanılama MSBuild çıktı](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2.  İOS uygulamaları için de etkinleştirmeniz **ayrıntılı mtouch çıkış** ekleyerek `-v -v -v -v` altında **proje özellikleri > iOS Yapı > Genel [tab] > Ek Seçenekler > ek mtouch bağımsız değişkenleri**.

    **Visual Studio (Windows)**

    [![](https://kb.xamarin.com/customer/portal/attachments/337319 "Dört tire v ek mtouch bağımsız değişkenleri girdi alanına girin")](https://kb.xamarin.com/customer/portal/attachments/337319)

    **Mac için Visual Studio**

    [![](https://kb.xamarin.com/customer/portal/attachments/337316 "Dört tire v ek mtouch bağımsız değişkenleri girdi alanına girin")](https://kb.xamarin.com/customer/portal/attachments/337316)

3.  Temizleyin ve projeyi yeniden derleyin.

4.  Kopyalama ve IDE yapı çıktısını bir metin dosyasına yapıştırın.
     - Visual Studio (Windows): **Görünüm > Çıktı > Göster çıktı: derleme**
     - Mac için Visual Studio: **Görünüm > klavye takımı > hataları > yapı çıktı [tab]**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Hata ayıklama günlüklerini Xamarin.Android ve Xamarin.iOS uygulamaları için

### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

**Görünüm > klavye takımı > Uygulama çıktısı**

![](https://kb.xamarin.com/customer/portal/attachments/337290 "Mac için Visual Studio'da uygulama çıkış Paneli simgesi")


(Uygulama başlatıldıktan sonra bu menü öğesini yalnızca görünür unutmayın.)

### <a name="visual-studio"></a>Visual Studio

**Görünüm > Çıktı > Göster çıktı: hata ayıklama**

![](https://kb.xamarin.com/customer/portal/attachments/337292 "Visual Studio'da hata ayıklama seçeneği gösteren çıkış paneli")

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](http://developer.android.com/tools/help/adb.html) logcat günlükleri

Çalıştırdıktan sonra `adb` komutu, geri attach **android_logcat.txt** masaüstünüzden dosya. Bu yönergeler yalnızca bir aygıt bağlı olduğunu varsayın.

Ayrıca bkz. [Android hata ayıklama günlüğünü](~/android/deploy-test/debugging/android-debug-log.md) sayfası.

#### <a name="visual-studio"></a>Visual Studio

1.  **Araçlar > Android > Android Adb komut istemi açın**
2.  Günlük temizleme: `adb logcat -c`
3.  Sorunu yeniden oluşturun.
4.  Çıkış günlüğü: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

1.  **Araçlar > Android SDK komut istemi açın**
2.  Günlük temizleme: `adb logcat -c`
3.  Sorunu yeniden oluşturun.
4.  Çıkış günlüğü: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />iOS simülatörü (Mac) günlüğe yazar.

*   Sistem günlüğüne erişmek için seçin **hata ayıklama > Sistem günlüğü Aç...**  iOS simülatörü uygulama.

    ![](https://kb.xamarin.com/customer/portal/attachments/382617 "Sistem günlüğü Aç seçeneğini gösteren menü hata ayıklama")

*   Simulator kilitlenme raporlarını görüntüleme için Console.app açın ve gidin `~/Library/Logs > DiagnosticReports`.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />iOS cihazı (Mac) günlüğe yazar.

#### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

**Görünüm > klavye takımı > iOS aygıtı günlük**

#### <a name="xcode"></a>Xcode 

**Pencere > cihazlar > ${DeviceName}**

Kilitlenme raporları altında kullanılabilir **cihaz günlüklerini görüntüle** düğmesi. Açıklama ok altında pencerenin altındaki cihaz için sistem günlüğünü görünür <img alt="Disclosure arrow" src="https://kb.xamarin.com/customer/portal/attachments/382618" style="width: 15px; height: 12px;" />biçimindeki telefon numarasıdır.

#### <a name="xcode-5"></a>Xcode 5

**Pencere > Düzenleyici > aygıtları [tab] > ${DeviceName}**
