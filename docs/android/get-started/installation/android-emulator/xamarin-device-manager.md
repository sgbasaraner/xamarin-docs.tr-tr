---
title: "Xamarin Android cihaz Yöneticisi"
description: "Xamarin Android Aygıt Yöneticisi, şu anda önizlemede Google Eski Aygıt Yöneticisi'ni değiştirir. Bu kılavuz, Xamarin Android Aygıt Yöneticisi'ni oluşturmak ve Android sanal Android cihazları öykünmek cihazlar (AVDs) yapılandırmak için nasıl kullanılacağını açıklar. Bu sanal cihazlar ve fiziksel cihaz üzerindeki kullanan gerek kalmadan uygulamanızı test çalıştırmak için kullanabilirsiniz."
ms.topic: article
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: 01fb21729e919872935fd63af28a13642a11fa4b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="xamarin-android-device-manager"></a>Xamarin Android cihaz Yöneticisi

_Xamarin Android Aygıt Yöneticisi, şu anda önizlemede Google Eski Aygıt Yöneticisi'ni değiştirir. Bu kılavuz, Xamarin Android Aygıt Yöneticisi'ni oluşturmak ve Android sanal Android cihazları öykünmek cihazlar (AVDs) yapılandırmak için nasıl kullanılacağını açıklar. Bu sanal cihazlar ve fiziksel cihaz üzerindeki kullanan gerek kalmadan uygulamanızı test çalıştırmak için kullanabilirsiniz._

![Şu anda önizlemede](~/media/shared/preview.png)

 
## <a name="overview"></a>Genel Bakış

Donanım hızlandırma etkinleştirildiğini doğruladıktan sonra (açıklandığı gibi [donanım hızlandırmasını](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), test ve uygulamanızı hata ayıklama için kullanılacak sanal cihaz oluşturmak için sonraki adımdır. Android SDK öykünücüsü tarafından kullanılacak sanal cihaz oluşturmak için Xamarin Android Aygıt Yöneticisi'ni kullanabilirsiniz.

Neden yerine Xamarin Android Aygıt Yöneticisi'ni kullanmak istediğiniz [Google Aygıt Yöneticisi'ni](~/android/get-started/installation/android-emulator/google-emulator-manager.md)?
Android SDK Araçları sürüm 26.0.1 itibariyle Google UI tabanlı AVD ve SDK yöneticilerinin lehinde kaldıran yeni (komut satırı arabirimi) CLI araçlarını desteği kaldırdı. Bu değişikliği nedeniyle kullanmalısınız [Xamarin SDK Manager](~/android/get-started/installation/android-sdk.md) ve Xamarin Android cihaz Android SDK Araçları 26.0.1 güncelleştirdiğinizde Yöneticisi ve daha sonra (olmayan Android 8.0 Oreo geliştirme için gerekli).


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu kılavuz yüklemek ve Windows'da Visual Studio için Xamarin Android Aygıt Yöneticisi'ni kullanmak nasıl açıklar (veya [Mac için](?tabs=vsmac)):

[![Xamarin Android Aygıt Yöneticisi'nin aygıtlar sekmesinden ekran görüntüsü](xamarin-device-manager-images/win/01-devices-dialog-sml.png)](xamarin-device-manager-images/win/01-devices-dialog.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu kılavuz yüklemek ve Mac için Visual Studio için Xamarin Android Aygıt Yöneticisi'ni kullanmak nasıl açıklar (veya [Windows için](?tabs=vswin)):

[![Xamarin Android Aygıt Yöneticisi'nin aygıtlar sekmesinden ekran görüntüsü](xamarin-device-manager-images/mac/01-devices-dialog-sml.png)](xamarin-device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Bu kılavuz yalnızca Visual Studio Mac için geçerlidir.
Xamarin Studio Xamarin Android cihaz Yöneticisi ile uyumlu değil.

-----

Xamarin Android Aygıt Yöneticisi'ni oluşturmak ve yapılandırmak için kullandığınız *Android sanal cihaz* (AVDs) çalışan [Android SDK öykünücüsü](~/android/deploy-test/debugging/android-sdk-emulator/index.md).
Her AVD, fiziksel bir Android cihazı taklit eden bir öykünücü yapılandırmadır. Çalıştırın ve farklı fiziksel Android cihazları benzetimini yapılandırmaları çeşitli uygulamanızı test etmek mümkün kılar. Xamarin Android Aygıt Yöneticisi'ni Google'nın tek başına AVD (kullanım dışı bırakıldı) Yöneticisi değiştirir.

Bu kılavuzda, yüklemek ve Android Aygıt Yöneticisi'ni başlatmak öğreneceksiniz. Oluşturma, çoğaltma, özelleştirme ve sanal aygıtların başlatma öğreneceksiniz. Bu kılavuz ayrıca accelerometer, GPS, Yönlendirme ve açık algılayıcı gibi benzetimli algılayıcılar etkinleştir/devre dışı bırakılır ve donanım türünü yapılandırmak için (örneğin, API düzeyi, CPU, bellek ve çözümleme), her sanal cihaz özelliklerini yapılandırmak nasıl açıklar Bu sanal aygıt tarafından kullanılan hızlandırma.

## <a name="requirements"></a>Gereksinimler

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android Aygıt Yöneticisi'ni kullanmak için aşağıdakiler gerekir:

-   Visual Studio 2017 15,5 veya sonraki bir sürümü gereklidir. Visual Studio Community sürümü ve üzeri desteklenir.

-   Visual Studio 4.8 veya sonraki bir sürümü için Xamarin. Xamarin güncelleştirme hakkında daha fazla bilgi için bkz: [güncelleştirmeleri kanalı değiştirmek](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).

-   En son sürümünü [Xamarin Aygıt Yöneticisi'ni yükleyici](https://go.microsoft.com/fwlink/?linkid=865528) Windows için.

-   **Android SDK** &ndash; Android SDK yüklü olması gerekir (bkz [Android SDK Kurulum](~/android/get-started/installation/android-sdk.md)), ve sonraki bölümde açıklandığı gibi SDK Araçları sürüm 26.0'in yüklenmesi gerekir. Android SDK'sı şu konumda (zaten yüklü değilse) yüklediğinizden emin olun: **C:\\Program Files (x86)\\Android\\android sdk**.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   Visual Studio Mac 7.4 veya daha sonra.

-   En son sürümünü [Xamarin Aygıt Yöneticisi'ni yükleyici](https://go.microsoft.com/fwlink/?linkid=865527) macOS için.

-   **Android SDK** &ndash; Android SDK 8.0 (API 26) veya daha sonra SDK Yöneticisi aracılığıyla yüklenmesi gerekir.

-----

 
## <a name="installing-the-device-manager"></a>Aygıt Yöneticisi'ni yükleme

Xamarin Android Aygıt Yöneticisi'ni yüklemek için aşağıdaki adımları kullanın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Karşıdan [Xamarin Aygıt Yöneticisi'ni yükleyici](https://go.microsoft.com/fwlink/?linkid=865528) Windows için.

2. Çift **Xamarin.DeviceManager.msi** ve yükleme yönergelerini izleyin: 

    ![Xamarin Android Aygıt Yöneticisi'ni Kurulum Sihirbazı](xamarin-device-manager-images/win/30-installer.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Karşıdan [Xamarin Aygıt Yöneticisi'ni yükleyici](https://go.microsoft.com/fwlink/?linkid=865527) macOS için.

2. Çift **AndroidDevices.pkg** ve yükleme yönergelerini izleyin: 

    [![Xamarin Android Aygıt Yöneticisi'ni Kurulum Sihirbazı](xamarin-device-manager-images/mac/30-installer-sml.png)](xamarin-device-manager-images/mac/30-installer.png#lightbox)

-----

 
## <a name="launching-the-device-manager"></a>Aygıt Yöneticisi'ni başlatma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 15,6 Preview 3 ve sonraki sürümlerinde, Xamarin Android Aygıt Yöneticisi'nden başlatabilirsiniz **Araçları** menüsü. Daha sonra Aygıt Yöneticisi'ni tıklatarak başlatmak veya Visual Studio 15,6 Preview 3 kullanıyorsanız **Araçlar > Android Emulator Manager**:

[![Araçlar menüsünden başlatma](xamarin-device-manager-images/win/04-tools-menu-sml.png)](xamarin-device-manager-images/win/04-tools-menu.png#lightbox)

Visual Studio'nun önceki bir sürümünü kullanıyorsanız, Xamarin Android Aygıt Yöneticisi'ni Windows başlatılması **Başlat** menüsü.

![Xamarin Android Aygıt Yöneticisi'nde Başlat menüsü](xamarin-device-manager-images/win/31-start-menu.png)

Sağ **Xamarin Android Aygıt Yöneticisi'ni** seçip **daha > yönetici olarak çalıştır**. Başlatılırken şu hata iletişim kutusu görürseniz, bkz: [sorun giderme](#troubleshooting) bölümü geçici çözüm yönergeleri için:

![Android SDK'sı örneği hata](xamarin-device-manager-images/win/32-sdk-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Visual Studio'da Mac 7.6 Preview 3 (şu anda'alfa kanal) veya sonrası, Xamarin Android Aygıt Yöneticisi'ni seçerek başlatabilirsiniz **Araçlar > öykünücü yöneticisini**:

[![Araçlar menüsünden başlatma](xamarin-device-manager-images/mac/16-tools-menu-sml.png)](xamarin-device-manager-images/mac/16-tools-menu.png#lightbox)

Mac için Visual Studio'nun önceki bir sürümünü kullanıyorsanız, Xamarin Android Aygıt Yöneticisi'ni bağımsız olarak başlatılması gerekir. Bulun **Android cihazları** içinde **uygulamaları** klasörü ve başlatmak için çift tıklatın:

[![Xamarin Android Aygıt Yöneticisi'ni konumda Bulucu](xamarin-device-manager-images/mac/31-location-in-finder-sml.png)](xamarin-device-manager-images/mac/31-location-in-finder.png#lightbox)


-----

Android Aygıt Yöneticisi'ni kullanmadan önce Android SDK Araçları sürüm 26.0.0 yüklemelisiniz veya sonraki bir sürümü. Android SDK 26.0.0 araçları veya sonrası yüklü değil, başlatılırken bu hata iletişim kutusu görürsünüz:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android SDK'sı örneği hata iletişim kutusu](xamarin-device-manager-images/win/02-sdk-instance-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Android SDK'sı örneği hata iletişim kutusu](xamarin-device-manager-images/mac/02-sdk-instance-error.png)

-----

Bu hata iletişim kutusu görürseniz tıklatın **Tamam** Android SDK Yöneticisi'ni açın. Android SDK Yöneticisi'nde **Araçları** sekmesinde ve yükleme **Android SDK Araçları 26.0.2** veya sonraki sürümlerde, **Android SDK platformunuzun Araçlar 26.0.0** veya sonraki bir sürümü ve  **Android SDK derleme-araçları 26.0.0** (veya üstü):


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android SDK Araçları 26.0 yükleme](xamarin-device-manager-images/win/03-sdk-tools-sml.png)](xamarin-device-manager-images/win/03-sdk-tools.png#lightbox)

Bu paketleri yüklendikten sonra SDK Yöneticisi'ni kapatın ve Android Aygıt Yöneticisi'ni yeniden başlatın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Android SDK Araçları 26.0 yükleme](xamarin-device-manager-images/mac/03-sdk-tools-sml.png)](xamarin-device-manager-images/mac/03-sdk-tools.png#lightbox)

-----

 
## <a name="main-screen"></a>Ana Ekran

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android Aygıt Yöneticisi'ni ilk başlattığınızda, şu anda yapılandırılmış tüm sanal cihazlar görüntüleyen bir ekran gösterir. Her cihaz için **adı**, **işletim sistemi** (Android API düzey), **CPU**, **bellek** boyutu ve ekran çözünürlüğü görüntülenir:

[![Yüklü aygıtların listesi ve bunların parametrelerini](xamarin-device-manager-images/win/05-installed-list-sml.png)](xamarin-device-manager-images/win/05-installed-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Android Aygıt Yöneticisi'ni ilk başlattığınızda, şu anda yapılandırılmış tüm sanal cihazlar görüntüleyen bir ekran gösterir. Her cihaz için **adı**, **sistem görüntüsü** (Android API düzey), **CPU**, **bellek** boyutu ve ekran çözünürlüğü görüntülenir:

[![Yüklü aygıtların listesi ve bunların parametrelerini](xamarin-device-manager-images/mac/05-devices-list-sml.png)](xamarin-device-manager-images/mac/05-devices-list.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Listedeki bir aygıtı tıklattığınızda **Başlat** düğmesi, sağ tarafta görünür. Tıklayabilirsiniz **Başlat** düğmesi bu sanal cihazla öykünücü başlatmak için:

[![Cihaz görüntüsü için Başlat düğmesi](xamarin-device-manager-images/win/06-start-button-sml.png)](xamarin-device-manager-images/win/06-start-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Tıklatın bir **Yürüt** düğmesi öykünücü tercih ettiğiniz sanal cihazla başlatmak için:
 
[![Cihaz görüntüsü için Başlat düğmesi](xamarin-device-manager-images/mac/06-start-button-sml.png)](xamarin-device-manager-images/mac/06-start-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Seçili sanal cihazla öykünücü başladıktan sonra **Başlat** düğmesi değişiklikleri bir **durdurmak** öykünücü durdurmak için kullanabileceğiniz düğmesi:

[![Çalışan cihazın düğmesi Durdur](xamarin-device-manager-images/win/07-stop-button-sml.png)](xamarin-device-manager-images/win/07-stop-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Seçili sanal cihazla öykünücü başladıktan sonra **Yürüt** düğmesi değişiklikleri bir **durdurmak** öykünücü durdurmak için kullanabileceğiniz düğmesi:
 
[![Çalışan cihazın düğmesi Durdur](xamarin-device-manager-images/mac/07-stop-button-sml.png)](xamarin-device-manager-images/mac/07-stop-button.png#lightbox)
 
-----

 
### <a name="new-device"></a>Yeni cihaz

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Yeni bir cihaz oluşturmak için tıklatın **yeni** düğmesini (ekranın sağ üst bölümünde bulunur):

[![Yeni bir cihaz oluşturmak için yeni düğmesi](xamarin-device-manager-images/win/08-new-button-sml.png)](xamarin-device-manager-images/win/08-new-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Yeni bir cihaz oluşturmak için tıklatın **yeni cihaz** düğmesini (ekranın sağ üst bölümünde bulunur):
 
[![Yeni bir cihaz oluşturmak için yeni düğmesi](xamarin-device-manager-images/mac/08-new-button-sml.png)](xamarin-device-manager-images/mac/08-new-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tıklatarak **yeni** başlatır **yeni cihaz** ekran:

[![Yeni cihaz ekranı Aygıt Yöneticisi'nin](xamarin-device-manager-images/win/09-new-device-editor-sml.png)](xamarin-device-manager-images/win/09-new-device-editor.png#lightbox)

Yeni bir cihaz yapılandırmak için **yeni cihaz** ekranında, aşağıdaki adımları kullanın:

1. Tıklayarak benzetmek için fiziksel bir cihaz seçin **aygıt** açılır menü:

    [![Cihaz aşağı açılır menüsü](xamarin-device-manager-images/win/10-device-menu-sml.png)](xamarin-device-manager-images/win/10-device-menu.png#lightbox)

2. Tıklayarak bu sanal cihazla kullanmak için Sistem Görüntüsü Seç **sistem görüntüsü** açılır menü. Bu menü altında yüklü sistem görüntüleri listeler **yüklü**. **Karşıdan** bölüm geliştirme bilgisayarınızda şu anda kullanılabilir olan, ancak otomatik olarak yüklenen sistem görüntüleri listeler:

    [![Sistem görüntüsü aşağı açılır menüsü](xamarin-device-manager-images/win/11-system-image-menu-sml.png)](xamarin-device-manager-images/win/11-system-image-menu.png#lightbox)

3. Cihazı yeni bir ad verin. Aşağıdaki örnekte, yeni cihaz adlı **Nexus 5 API 25**:

    [![Yeni cihaz adlandırma](xamarin-device-manager-images/win/12-device-name-sml.png)](xamarin-device-manager-images/win/12-device-name.png#lightbox)

4. Değişiklik yapmanız özelliklerini düzenleyin. Özelliklerde değişiklik için bkz: [profil özellikleri](#properties) bu kılavuzda daha sonra.

5. Açıkça ayarlamak için gereken ek özellikleri ekleyin. **Yeni cihaz** ekran yalnızca en sık değiştirilen özellikleri listeler, ancak tıklayabilirsiniz **Özellik Ekle** aşağı açılır menüde (alt sol köşe) ek özellikler eklemek için. Aşağıdaki örnekte, `hw.lcd.backlight` özellik eklenir:

    [![Özellik aşağı açılır menü ekleme](xamarin-device-manager-images/win/13-add-property-menu-sml.png)](xamarin-device-manager-images/win/13-add-property-menu.png#lightbox)

6. Tıklatın **oluşturma** yeni cihaz oluşturmak için düğmeyi (alt sağ köşesinde):

    ![Oluştur düğmesi](xamarin-device-manager-images/win/14-create-button.png)

7. Alma bir **lisans kabulünü** ekran. Tıklatın **kabul** lisans koşullarını kabul ediyorsanız:

    ![Lisans kabulünü ekranı](xamarin-device-manager-images/win/15-license-acceptance.png)

8. Android Aygıt Yöneticisi'ni ile yüklü sanal aygıtların listesi yeni cihaz ekleyen bir **oluşturma** cihaz oluştururken İlerleme göstergesi:

    [![Oluşturma İlerleme göstergesi](xamarin-device-manager-images/win/16-creating-the-device-sml.png)](xamarin-device-manager-images/win/16-creating-the-device.png#lightbox)

9. Oluşturma işlemi tamamlandığında, yeni cihaz ile yüklü sanal aygıtların listesi gösterilen bir **Başlat** düğmesi, başlatmak hazır:

   [![Yeni oluşturulan aygıt başlatmak hazır](xamarin-device-manager-images/win/17-created-device-sml.png)](xamarin-device-manager-images/win/17-created-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Tıklatarak **yeni cihaz** başlatır **yeni cihaz** ekran:

[![Yeni cihaz ekranı Aygıt Yöneticisi'nin](xamarin-device-manager-images/mac/09-new-device-editor-sml.png)](xamarin-device-manager-images/mac/09-new-device-editor.png#lightbox)

Yeni bir cihaz yapılandırmak için aşağıdaki adımları kullanın **yeni cihaz** ekran:

1. Tıklayarak benzetmek için fiziksel bir cihaz seçin **aygıt** açılır menü:

    [![Cihaz aşağı açılır menüsü](xamarin-device-manager-images/mac/10-device-menu-sml.png)](xamarin-device-manager-images/mac/10-device-menu.png#lightbox)

2. Tıklayarak bu sanal cihazla kullanmak için Sistem Görüntüsü Seç **sistem görüntüsü** açılır menü. Bu menü altında yüklü sistem görüntüleri listeler **yüklü**. **karşıdan** (gösterilen varsa) bölümü geliştirme bilgisayarınızda şu anda kullanılabilir olan, ancak otomatik olarak yüklenen sistem görüntüleri listeler:

    [![Sistem görüntüsü aşağı açılır menüsü](xamarin-device-manager-images/mac/11-system-image-menu-sml.png)](xamarin-device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Cihazı yeni bir ad verin. Aşağıdaki örnekte, yeni cihaz adlı **Nexus 5 X API 25**:

    [![Yeni cihaz adlandırma](xamarin-device-manager-images/mac/12-device-name-sml.png)](xamarin-device-manager-images/mac/12-device-name.png#lightbox)

4. Değişiklik yapmanız özelliklerini düzenleyin. Özelliklerde değişiklik için bkz: [profil özellikleri](#properties) bu kılavuzda daha sonra.

5. Açıkça ayarlamak için gereken ek özellikleri ekleyin. **Yeni cihaz** ekran yalnızca en sık değiştirilen özellikleri listeler, ancak tıklayabilirsiniz **Özellik Ekle** aşağı açılır menüde (alt sol köşe) ek özellikler eklemek için:

    [![Özellik aşağı açılır menü ekleme](xamarin-device-manager-images/mac/13-add-property-menu-sml.png)](xamarin-device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Tıklatarak **özel** aygıt için yeni bir özellik tanımlamak için:

    ![Oluştur düğmesi](xamarin-device-manager-images/mac/14-custom-button.png)

7. Tıklatın **oluşturma** yeni cihaz oluşturmak için düğmeyi (alt sağ köşesinde):

    ![Oluştur düğmesi](xamarin-device-manager-images/mac/15-create-button.png)

8. Alma bir **lisans kabulünü** ekran. Tıklatın **kabul** lisans koşullarını kabul etmesi durumunda.

9. Cihaz oluşturduğu sırada Android Aygıt Yöneticisi'ni yeni cihaz ile cihaz listesine ekler. bir **oluşturma** İlerleme göstergesi:

    [![Oluşturma İlerleme göstergesi](xamarin-device-manager-images/mac/17-creating-the-device-sml.png)](xamarin-device-manager-images/mac/17-creating-the-device.png#lightbox)

10. Oluşturma işlemi tamamlandığında, yeni cihaz aygıt listesinde gösterilen bir **Yürüt** düğmesi, başlatmak hazır:

   [![Yeni oluşturulan aygıt başlatmak hazır](xamarin-device-manager-images/mac/18-created-device-sml.png)](xamarin-device-manager-images/mac/18-created-device.png#lightbox)

-----


<a name="device-edit" />


### <a name="edit-device"></a>Cihaz Düzenle

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Varolan bir sanal cihazın düzenlemek için cihazı seçin ve **Düzenle** düğmesini (ekranın sağ üst köşesinde bulunan):

[![Düzenle düğmesi, yeni bir cihaz değiştirme](xamarin-device-manager-images/win/19-edit-button-sml.png)](xamarin-device-manager-images/win/19-edit-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Varolan bir sanal cihazın düzenlemek için seçin **ek seçenekler** açılır menü (dişli simgesi) ve select **Düzenle**:
 
[![Yeni bir cihaz değiştirme menü seçimi Düzenle](xamarin-device-manager-images/mac/19-edit-button-sml.png)](xamarin-device-manager-images/mac/19-edit-button.png#lightbox)
 
-----

Tıklatarak **Düzenle** seçili sanal cihaz için cihaz Düzenleyici başlatılır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cihaz Düzenleyicisi ekranı](xamarin-device-manager-images/win/20-device-editor-sml.png)](xamarin-device-manager-images/win/20-device-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
 
[![Cihaz Düzenleyicisi ekranı](xamarin-device-manager-images/mac/20-device-editor-sml.png)](xamarin-device-manager-images/mac/20-device-editor.png#lightbox)
 
-----

**Aygıt Düzenleyicisi** ekran ilk sütununda, sanal cihaz özelliklerini ikinci sütundaki her bir özellik karşılık gelen değerleri listeler. Bir özellik seçtiğinizde, bu özellik için ayrıntılı bir açıklama sağ tarafta görüntülenir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Örneğin, aşağıdaki ekran görüntüsünde `hw.lcd.density` özelliği değiştirildiğinde **420** için **240**:

[![Cihaz düzenleme örneği](xamarin-device-manager-images/win/21-device-editing-sml.png)](xamarin-device-manager-images/win/21-device-editing.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Örneğin, aşağıdaki ekran görüntüsünde `hw.lcd.density` özelliği değiştirildiğinde **320** için **240** ve `hw.ramSize` özelliği değiştirildiğinde **768**:
 
[![Cihaz düzenleme örneği](xamarin-device-manager-images/mac/21-device-editing-sml.png)](xamarin-device-manager-images/mac/21-device-editing.png#lightbox)
 
-----

Gerekli yapılandırma değişikliklerini yaptıktan sonra tıklatın **kaydetmek** düğmesi.
Sanal cihaz özelliklerini değiştirme hakkında daha fazla bilgi için bkz: [profil özellikleri](#properties) bu kılavuzda daha sonra.


 
### <a name="additional-options"></a>Ek Seçenekler

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Cihazları ile çalışmak için ek seçenekler kullanılabilir &hellip; sağ üst köşede menüde:

[![Ek Seçenekler menüsünü konumu](xamarin-device-manager-images/win/22-overflow-menu-sml.png)](xamarin-device-manager-images/win/22-overflow-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bir aygıt ile çalışmak için ek seçenekler solunda bulunan aşağı açılır menüden kullanılabilir **Yürüt** düğmesi:

[![Ek Seçenekler menüsünü konumu](xamarin-device-manager-images/mac/22-overflow-menu-sml.png)](xamarin-device-manager-images/mac/22-overflow-menu.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ek Seçenekler menüsünü aşağıdaki öğeleri içerir:

-   **Yinelenen ve düzenleme** &ndash; şu anda seçili cihaz yineler ve bunun içinde açılacak **yeni cihaz** ekran farklı benzersiz bir ad ile. Örneğin, seçme **VisualStudio_android 23_x86_phone** tıklatıp **yinelenen ve düzenleme** bir sayaç adına ekler:

    [![Yinelenen ve düzenleme ekranı](xamarin-device-manager-images/win/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **Explorer'da ortaya** &ndash; sanal aygıt için dosyalarını tutan klasöründe bir Windows Explorer penceresi açar. Örneğin, seçme **Nexus 5 X API 25** tıklatıp **Explorer'da ortaya** aşağıdaki gibi bir pencere açılır:

    [![Tıklama sonuçları Gezgininde ortaya](xamarin-device-manager-images/win/24-reveal-in-explorer-sml.png)](xamarin-device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Fabrika sıfırlaması** &ndash; Seçilen aygıt çalıştırılırken cihaz iç durumuna kullanıcı değişiklikleri silme kendi varsayılan ayarlara sıfırlar. Bu değişiklik, sanal cihaz oluşturma ve düzenleme sırasında yaptığınız değişiklikler değiştirmez. Bu sıfırlama alınamaz anımsatıcı ile bir iletişim kutusu görünür. Tıklatın **kullanıcı verilerini temizleme** sıfırlama onaylamak için.

-   **Silme** &ndash; seçili sanal aygıt kalıcı olarak siler.
    Bir iletişim kutusu, bir cihazın silinmesi geri alınamaz anımsatıcısı görüntülenir. Tıklatın **silmek** cihazı silmek istediğiniz emin olması durumunda.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Ek Seçenekler menüsünü aşağıdaki öğeleri içerir:

-   **Düzen** &ndash; şu anda seçili aygıt daha önce açıklandığı gibi cihaz Düzenleyicisi'nde açar [Düzenle aygıt](#device-edit).

-   **Yinelenen ve düzenleme** &ndash; şu anda seçili cihaz yineler ve bunun içinde açılacak **yeni cihaz** ekran farklı benzersiz bir ad ile.
    Örneğin, seçme **Nexus 5 X API 25** tıklatıp **yinelenen ve düzenleme** bir sayaç adına ekler:

    [![Yinelenen ve düzenleme ekranı](xamarin-device-manager-images/mac/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Finder ortaya** &ndash; sanal aygıt için dosyalarını tutan klasöründe macOS Bulucu penceresi açılır. Örneğin, seçme **Nexus 5 X API 25** tıklatıp **Finder ortaya** aşağıdaki gibi bir pencere açılır:

    [![Tıklama sonuçları Gezgininde ortaya](xamarin-device-manager-images/mac/24-reveal-in-finder-sml.png)](xamarin-device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Fabrika sıfırlaması** &ndash; Seçilen aygıt çalıştırılırken cihaz iç durumuna kullanıcı değişiklikleri silme kendi varsayılan ayarlara sıfırlar. Bu değişiklik, sanal cihaz oluşturma ve düzenleme sırasında yaptığınız değişiklikler değiştirmez. Bu sıfırlama alınamaz anımsatıcı ile bir iletişim kutusu görünür. Tıklatın **kullanıcı verilerini temizleme** sıfırlama onaylamak için.

-   **Silme** &ndash; seçili sanal aygıt kalıcı olarak siler.
    Bir iletişim kutusu, bir cihazın silinmesi geri alınamaz anımsatıcısı görüntülenir." Tıklatın **silmek** cihazı silmek istediğiniz emin olması durumunda.

-----

<a name="properties" />
 

## <a name="profile-properties"></a>Profil özellikleri

**Yeni cihaz** ve **aygıt Düzenle** ekranlar ilk sütununda, sanal cihaz özelliklerini ikinci sütundaki her bir özellik karşılık gelen değerleri listesi. Bir özellik seçtiğinizde, bu özellik için ayrıntılı bir açıklama sağ tarafta görüntülenir. Değiştirebileceğiniz kendi *donanım profilinin özelliklerini* ve kendi *AVD özellikleri*.
Donanım profilinin özelliklerini (gibi `hw.ramSize` ve `hw.accelerometer`) benzetilmiş aygıt fiziksel özelliklerini açıklar. Bir accelerometer mevcut olup olmadığını ekran boyutu, kullanılabilir RAM miktarını bu özellikleri içerir. Çalıştırıldığında AVD işlemi AVD özelliklerini belirtin. Örneğin, AVD özellikleri AVD geliştirme bilgisayarınızın ekran kartı işleme için nasıl kullandığını belirlemek için yapılandırılabilir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aşağıdaki yönergeleri kullanarak özelliklerini değiştirebilirsiniz:

-   Bir boolean özelliği değiştirmek için boolean özelliği sağındaki onay işaretine tıklayın:

    ![Bir boolean özelliği değiştirme](xamarin-device-manager-images/win/25-boolean-value.png)

-   Değiştirmek için bir *enum* (numaralandırılmış) özelliği, özelliğin sağındaki aşağı oka tıklayın ve yeni bir değer seçin.

    ![Bir liste özelliğini değiştirme](xamarin-device-manager-images/win/27-enum-value.png)

-   Bir string veya tamsayı özelliğini değiştirmek için değer sütununda geçerli string veya tamsayı ayarını çift tıklatın ve yeni bir değer girin.

    ![Bir tamsayı özelliği değiştirme](xamarin-device-manager-images/win/26-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Aşağıdaki yönergeleri kullanarak özelliklerini değiştirebilirsiniz:

-   Bir boolean özelliği değiştirmek için boolean özelliği sağındaki onay işaretine tıklayın:

    ![Bir boolean özelliği değiştirme](xamarin-device-manager-images/mac/25-boolean-value.png)

-   Değiştirmek için bir *enum* (numaralandırılmış) özelliği, özellik sağındaki açılan menüsüne tıklayın ve yeni bir değer seçin.

    ![Bir liste özelliğini değiştirme](xamarin-device-manager-images/mac/27-enum-value.png)

-   Bir string veya tamsayı özelliğini değiştirmek için değer sütununda geçerli string veya tamsayı ayarını çift tıklatın ve yeni bir değer girin.

    ![Bir tamsayı özelliği değiştirme](xamarin-device-manager-images/mac/26-integer-value.png)


-----

Aşağıdaki tabloda listelenen özellikleri ayrıntılı bir açıklamasını sağlar **yeni cihaz** ve **aygıt Düzenleyicisi** ekranlar:

[!include[](~/android/includes/emulator-properties.md)]

Bu özellikler hakkında daha fazla bilgi için bkz: [donanım profilinin özelliklerini](https://developer.android.com/studio/run/managing-avds.html#hpproperties).


<a name="troubleshooting" />

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki Xamarin Android cihaz Yöneticisi ile ilgili genel sorunları ve geçici çözümleri açıklar:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-in-non-standard-location"></a>Standart olmayan konumda, Android SDK'sı

Genellikle, Android SDK'sı şu konumda yüklenir:

**C:\\Program Files (x86)\\Android\\android-SDK'sı**

SDK bu konumda yüklü değilse başlatılırken bu hatayı alabilirsiniz:

![Android SDK'sı örneği hata](xamarin-device-manager-images/win/32-sdk-error.png)

Geçici çözüm için bu sorun aşağıdakileri yapın:

1. Windows masaüstünden gidin **C:\\kullanıcılar\\*kullanıcıadı*\\AppData\\gezici\\XamarinDeviceManager**:

    ![Xamarin Aygıt Yöneticisi'ni günlük dosyası konumu](xamarin-device-manager-images/win/33-log-files.png)

2. Günlük dosyalarından birini açın ve bulmak için çift **yapılandırma dosyası yolu**. Örneğin:

    [![Günlük dosyasında yapılandırma dosyası yolu](xamarin-device-manager-images/win/34-config-file-path-sml.png)](xamarin-device-manager-images/win/34-config-file-path.png#lightbox)

3. Bu konum ve çift **user.config** açın. 

4. İçinde **user.config**, bulun  **&lt;ayarlarını&gt;**  öğesi ekleyin bir **AndroidSdkPath** özniteliği için. Bu öznitelik, Android SDK'sı, bilgisayarınızda yüklü olduğu yola ayarlayın ve dosyayı kaydedin. Örneğin,  **&lt;ayarlarını&gt;**  Android SDK adresindeki yüklenmişse, aşağıdaki gibi görünür **C:\\programları\\Android\\SDK**:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Bu değişikliği yaptıktan sonra **user.config**, Xamarin Android Aygıt Yöneticisi'ni başlatma gerekir.

### <a name="snapshot-disables-wifi-on-android-oreo"></a>Anlık görüntü WiFi Android Oreo üzerinde devre dışı bırakır.

Bir anlık görüntü Wi-Fi erişimi devre dışı duruma neden olabilecek sonra AVD yeniden başlatmak için Android Oreo benzetimli Wi-Fi erişim ile yapılandırılmış bir AVD varsa.

Bu sorunu geçici olarak çözmek için

1. AVD Xamarin Aygıt Yöneticisi'nde seçin.

2. Ek seçenekler menüden **Explorer'da ortaya**.

3. Gidin **anlık görüntüleri > default_boot**.

4. Silme **snapshot.pb** dosyası:

    [![Snapshot.pb dosyasının konumu](xamarin-device-manager-images/win/36-delete-snapshot-sml.png)](xamarin-device-manager-images/win/36-delete-snapshot.png#lightbox)

5. AVD yeniden başlatın. 

Bu değişiklikler yapıldıktan sonra AVD yeniden çalışmak Wi-Fi izin veren bir durumda yeniden başlatılır.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

### <a name="snapshot-disables-wifi-on-android-oreo"></a>Anlık görüntü WiFi Android Oreo üzerinde devre dışı bırakır.

Bir anlık görüntü Wi-Fi erişimi devre dışı duruma neden olabilecek sonra AVD yeniden başlatmak için Android Oreo benzetimli Wi-Fi erişim ile yapılandırılmış bir AVD varsa.

Bu sorunu geçici olarak çözmek için

1. AVD Xamarin Aygıt Yöneticisi'nde seçin.

2. Ek seçenekler menüden **Finder ortaya**.

3. Gidin **anlık görüntüleri > default_boot**.

4. Silme **snapshot.pb** dosyası:

    [![Snapshot.pb dosyasının konumu](xamarin-device-manager-images/mac/36-delete-snapshot-sml.png)](xamarin-device-manager-images/mac/36-delete-snapshot.png#lightbox)

5. AVD yeniden başlatın. 

Bu değişiklikler yapıldıktan sonra AVD yeniden çalışmak Wi-Fi izin veren bir durumda yeniden başlatılır.

-----


### <a name="generating-a-bug-report"></a>Bir hata raporu oluşturma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android cihaz yukarıdaki sorun giderme ipuçları kullanarak çözümlenemeyen Yöneticisi ile ilgili bir sorun bulursanız, Lütfen bir hata raporu başlık çubuğunu sağ tıklatıp seçerek dosya **hata raporu oluşturmak**:

![Bir hata raporu dosyalama için menü öğesi konumu](xamarin-device-manager-images/win/35-bug-report.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Xamarin Android cihaz yukarıdaki sorun giderme ipuçları kullanarak çözümlenemeyen Yöneticisi ile ilgili bir sorun bulursanız, Lütfen bir hata raporu tıklayarak dosya **Yardım > hata raporu oluşturmak**:

![Bir hata raporu dosyalama için menü öğesi konumu](xamarin-device-manager-images/mac/35-bug-report.png)

-----

 
 
## <a name="summary"></a>Özet

Bu kılavuz, Visual Studio için Xamarin Android Aygıt Yöneticisi'ni Visual Studio'da kullanılabilir Mac ve Xamarin için sunmuştur. Başlatma ve durdurma Android öykünücüsünde çalıştırmak için bir Android sanal cihazı (AVD) seçme gibi temel özellikleri yeni sanal cihazlar ve sanal cihazı düzenleme oluşturma açıklanmıştır. Ayrıca, daha fazla özelleştirme profili donanım özelliklerini düzenlemek nasıl açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android SDK Aracı Üzerindeki Değişiklikler](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android SDK öykünücüsü ile hata ayıklama](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [Sürüm Notları (Google) SDK Araçları](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
