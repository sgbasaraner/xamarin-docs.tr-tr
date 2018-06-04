---
title: Öykünücü kurulum sorunlarını giderme
description: Bu makalede, tanılama ve Android Aygıt Yöneticisi'ni kullanırken ortaya çıkabilecek sorunlar üzerinde çalışmayı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: 4cbd7d7626d114856d6c68fe7bc9feb7c2a5abc2
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734056"
---
# <a name="troubleshooting-emulator-setup-problems"></a>Öykünücü kurulum sorunlarını giderme

_Bu makalede, tanılama ve Android öykünücüsünde yapılandırmak için Android Aygıt Yöneticisi'ni kullanırken, ortaya çıkabilecek sorunlar üzerinde çalışmayı açıklanmaktadır. Android öykünücüsünde çalıştırılırken sorunlarını giderme hakkında daha fazla bilgi için bkz: [Google Android öykünücüsü sorun giderme](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md)._

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="android-sdk-in-non-standard-location"></a>Standart olmayan konumda, Android SDK'sı

Genellikle, Android SDK'sı şu konumda yüklenir:

**C:\\Program Files (x86)\\Android\\android-SDK'sı**

SDK bu konumda yüklü değilse, Android Aygıt Yöneticisi'ni başlattığınızda bu hatayı alabilirsiniz:

![Android SDK'sı örneği hata](troubleshooting-images/win/01-sdk-error.png)

Bu sorunu geçici olarak çözmek için aşağıdakileri yapın:

1. Windows masaüstünden gidin **C:\\kullanıcılar\\*kullanıcıadı*\\AppData\\gezici\\XamarinDeviceManager**:

    ![Android cihaz Yöneticisi günlük dosyası konumu](troubleshooting-images/win/02-log-files.png)

2. Günlük dosyalarından birini açın ve bulmak için çift **yapılandırma dosyası yolu**. Örneğin:

    [![Günlük dosyasında yapılandırma dosyası yolu](troubleshooting-images/win/03-config-file-path-sml.png)](troubleshooting-images/win/03-config-file-path.png#lightbox)

3. Bu konum ve çift **user.config** açın. 

4. İçinde **user.config**, bulun **&lt;ayarlarını&gt;** öğesi ekleyin bir **AndroidSdkPath** özniteliği için. Bu öznitelik, Android SDK'sı, bilgisayarınızda yüklü olduğu yola ayarlayın ve dosyayı kaydedin. Örneğin, **&lt;ayarlarını&gt;** Android SDK adresindeki yüklenmişse, aşağıdaki gibi görünür **C:\\programları\\Android\\SDK**:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Bu değişikliği yaptıktan sonra **user.config**, Android Aygıt Yöneticisi'ni başlatma gerekir.

## <a name="snapshot-disables-wifi-on-android-oreo"></a>Anlık görüntü WiFi Android Oreo üzerinde devre dışı bırakır.

Bir anlık görüntü Wi-Fi erişimi devre dışı duruma neden olabilecek sonra AVD yeniden başlatmak için Android Oreo benzetimli Wi-Fi erişim ile yapılandırılmış bir AVD varsa.

Bu sorunu geçici olarak çözmek için

1. AVD Android Aygıt Yöneticisi'nde seçin.

2. Ek seçenekler menüden **Explorer'da ortaya**.

3. Gidin **anlık görüntüleri > default_boot**.

4. Silme **snapshot.pb** dosyası:

    ![Snapshot.pb dosyasının konumu](troubleshooting-images/win/05-delete-snapshot.png)

5. AVD yeniden başlatın. 

Bu değişiklikler yapıldıktan sonra AVD yeniden çalışmak Wi-Fi izin veren bir durumda yeniden başlatılır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="snapshot-disables-wifi-on-android-oreo"></a>Anlık görüntü WiFi Android Oreo üzerinde devre dışı bırakır.

Bir anlık görüntü Wi-Fi erişimi devre dışı duruma neden olabilecek sonra AVD yeniden başlatmak için Android Oreo benzetimli Wi-Fi erişim ile yapılandırılmış bir AVD varsa.

Bu sorunu geçici olarak çözmek için

1. AVD Android Aygıt Yöneticisi'nde seçin.

2. Ek seçenekler menüden **Finder ortaya**.

3. Gidin **anlık görüntüleri > default_boot**.

4. Silme **snapshot.pb** dosyası:

    [![Snapshot.pb dosyasının konumu](troubleshooting-images/mac/02-delete-snapshot-sml.png)](troubleshooting-images/mac/02-delete-snapshot.png#lightbox)

5. AVD yeniden başlatın. 

Bu değişiklikler yapıldıktan sonra AVD yeniden çalışmak Wi-Fi izin veren bir durumda yeniden başlatılır.


-----

## <a name="generating-a-bug-report"></a>Bir hata raporu oluşturma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android cihaz yukarıdaki sorun giderme ipuçları kullanarak çözümlenemeyen Yöneticisi ile ilgili bir sorun bulursanız, Lütfen bir hata raporu başlık çubuğunu sağ tıklatıp seçerek dosya **hata raporu oluşturmak**:

[![Bir hata raporu dosyalama için menü öğesi konumu](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Android cihaz yukarıdaki sorun giderme ipuçları kullanarak çözümlenemeyen Yöneticisi ile ilgili bir sorun bulursanız, Lütfen bir hata raporu tıklayarak dosya **Yardım > hata raporu oluşturmak**:

[![Bir hata raporu dosyalama için menü öğesi konumu](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)

-----
