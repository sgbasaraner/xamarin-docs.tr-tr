---
title: Sanal cihaz Android cihaz Yöneticisi ile yönetme
description: Bu makalede Android Aygıt Yöneticisi'ni oluşturmak ve Android sanal fiziksel Android cihazları öykünmek cihazlar (AVDs) yapılandırmak için nasıl kullanılacağı açıklanmaktadır. Bu sanal cihazlar ve fiziksel cihaz üzerindeki kullanan gerek kalmadan uygulamanızı test çalıştırmak için kullanabilirsiniz.
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 888f126d3e58b0300ba7ce3ad1cb5a8001fc545a
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734029"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Sanal cihaz Android cihaz Yöneticisi ile yönetme

_Bu makalede Android Aygıt Yöneticisi'ni oluşturmak ve Android sanal fiziksel Android cihazları öykünmek cihazlar (AVDs) yapılandırmak için nasıl kullanılacağı açıklanmaktadır. Bu sanal cihazlar ve fiziksel cihaz üzerindeki kullanan gerek kalmadan uygulamanızı test çalıştırmak için kullanabilirsiniz._

## <a name="overview"></a>Genel Bakış

Donanım hızlandırma etkinleştirildiğini doğruladıktan sonra (açıklandığı gibi [öykünücüsü performans donanım hızlandırmasını](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), sonraki adıma kullanmaktır _Android Aygıt Yöneticisi'ni_ (olarak da adlandırılır _Xamarin Android Aygıt Yöneticisi'ni_) sınamak ve uygulamanızın hatalarını ayıklamak için kullanabileceğiniz sanal cihaz oluşturmak için.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu makalede Android Aygıt Yöneticisi'ni oluşturma, çoğaltma, özelleştirme ve Android sanal cihaz başlatmak için nasıl kullanılacağı açıklanmaktadır.

[![Aygıtlar sekmesindeki Android Aygıt Yöneticisi'nin ekran görüntüsü](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Android Aygıt Yöneticisi'ni oluşturmak ve yapılandırmak için kullandığınız _Android sanal cihaz_ (AVDs) çalıştırın [Google Android öykünücüsü](~/android/deploy-test/debugging/android-sdk-emulator/index.md).
Her AVD, fiziksel bir Android cihazı taklit eden bir öykünücü yapılandırmadır. Çalıştırın ve farklı fiziksel Android cihazları benzetimini yapılandırmaları çeşitli uygulamanızı test etmek mümkün kılar.

## <a name="requirements"></a>Gereksinimler

Android Aygıt Yöneticisi'ni kullanmak için aşağıdakiler gerekir:

-   Visual Studio 2017 15.7 veya sonraki bir sürümü gereklidir. Visual Studio Community, Professional ve Enterprise sürümleri desteklenir.

-   Xamarin 4.9 veya sonraki bir sürümü için Visual Studio Araçları.

-   Android SDK yüklü olması gerekir (bkz [Xamarin.Android için Android SDK'sı ayarı](~/android/get-started/installation/android-sdk.md)) ve SDK Araçları sürüm 26.1.1 veya daha sonra sonraki bölümde açıklandığı gibi yüklü olmalıdır. Android SDK'sı şu konumda (zaten yüklü değilse) yüklediğinizden emin olun: **C:\\Program Files (x86)\\Android\\android sdk**.


## <a name="launching-the-device-manager"></a>Aygıt Yöneticisi'ni başlatma

Android Aygıt Yöneticisi'nden başlatın **Araçları** menüsünde tıklayarak **Araçlar > Android > Android Aygıt Yöneticisi'ni**:

[![Araçlar menüsünden başlatma](device-manager-images/win/04-tools-menu-sml.png)](device-manager-images/win/04-tools-menu.png#lightbox)

Başlatılırken şu hata iletişim kutusu görürseniz, bkz: [öykünücüsü kurulum sorunlarını giderme](~/android/get-started/installation/android-emulator/troubleshooting.md) yönergeleri geçici çözüm:

![Android SDK'sı örneği hata](device-manager-images/win/32-sdk-error.png)

Android Aygıt Yöneticisi'ni kullanmadan önce Android SDK Araçları sürüm 26.1.1 yüklemelisiniz veya sonraki bir sürümü. Android SDK 26.1.1 araçları veya sonrası yüklü değil, başlatılırken bu hata iletişim kutusu görebilirsiniz:

![Android SDK'sı örneği hata iletişim kutusu](device-manager-images/win/02-sdk-instance-error.png)

Bu hata iletişim kutusu görürseniz tıklatın **açma SDK Manager** Android SDK Yöneticisi'ni açın. Android SDK Yöneticisi'nde **Araçları** sekmesinde ve aşağıdaki yükleyin:

-   **Android SDK Araçları 26.1.1** veya daha yenisi 
-   **Android SDK platformunuzun Araçlar 27.0.1** veya daha yenisi  
-   **Android SDK derleme-araçları 27.0.3** veya daha yenisi

Bu paketleri ile gösterilmesi gereken **yüklü** aşağıdaki ekran görüntüsünde görülen durumu:

[![Android SDK Araçları 27.0 yükleme](device-manager-images/win/03-sdk-tools-sml.png)](device-manager-images/win/03-sdk-tools.png#lightbox)

Bu paketleri yüklendikten sonra SDK Yöneticisi'ni kapatın ve Android Aygıt Yöneticisi'ni yeniden başlatın.

## <a name="main-screen"></a>Ana Ekran

Android Aygıt Yöneticisi'ni ilk başlattığınızda, şu anda yapılandırılmış tüm sanal cihazlar görüntüleyen bir ekran gösterir. Her cihaz için **adı**, **işletim sistemi** (Android API düzey), **CPU**, **bellek** boyutu ve ekran çözünürlüğü görüntülenir:

[![Yüklü aygıtların listesi ve bunların parametrelerini](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

Listedeki bir aygıtı tıklattığınızda **Başlat** düğmesi, sağ tarafta görünür. Tıklayabilirsiniz **Başlat** düğmesi bu sanal cihazla öykünücü başlatmak için:

[![Cihaz görüntüsü için Başlat düğmesi](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

Seçili sanal cihazla öykünücü başladıktan sonra **Başlat** düğmesi değişiklikleri bir **durdurmak** öykünücü durdurmak için kullanabileceğiniz düğmesi:

[![Çalışan cihazın düğmesi Durdur](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>Yeni cihaz

Yeni bir cihaz oluşturmak için tıklatın **yeni** düğmesini (ekranın sağ üst bölümünde bulunur):

[![Yeni bir cihaz oluşturmak için yeni düğmesi](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

Tıklatarak **yeni** başlatır **yeni cihaz** ekran:

[![Yeni cihaz ekranı Aygıt Yöneticisi'nin](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

Yeni bir cihaz yapılandırmak için **yeni cihaz** ekranında, aşağıdaki adımları kullanın:

1. Tıklayarak benzetmek için fiziksel bir cihaz seçin **aygıt** açılır menü:

    [![Cihaz aşağı açılır menüsü](device-manager-images/win/10-device-menu-sml.png)](device-manager-images/win/10-device-menu.png#lightbox)

2. Tıklayarak bu sanal cihazla kullanmak için Sistem Görüntüsü Seç **sistem görüntüsü** açılır menü. Bu menü altında yüklü sistem Aygıt Yöneticisi'ni görüntüleri listeler **yüklü**. **Karşıdan** bölüm geliştirme bilgisayarınızda şu anda kullanılabilir olan, ancak otomatik olarak yüklenen sistem cihaz Yöneticisi görüntüleri listeler:

    [![Sistem görüntüsü aşağı açılır menüsü](device-manager-images/win/11-system-image-menu-sml.png)](device-manager-images/win/11-system-image-menu.png#lightbox)

3. Cihazı yeni bir ad verin. Aşağıdaki örnekte, yeni cihaz adlı **Nexus 5 API 25**:

    [![Yeni cihaz adlandırma](device-manager-images/win/12-device-name-sml.png)](device-manager-images/win/12-device-name.png#lightbox)

4. Değişiklik yapmanız özelliklerini düzenleyin. Özelliklerde değişiklik için bkz: [düzenleme Android sanal cihaz özellikleri](~/android/get-started/installation/android-emulator/device-properties.md).

5. Açıkça ayarlamak için gereken ek özellikleri ekleyin. **Yeni cihaz** ekran yalnızca en sık değiştirilen özellikleri listeler, ancak tıklayabilirsiniz **Özellik Ekle** aşağı açılır menüde (alt sol köşe) ek özellikler eklemek için. Aşağıdaki örnekte, `hw.lcd.backlight` özellik eklenir:

    [![Özellik aşağı açılır menü ekleme](device-manager-images/win/13-add-property-menu-sml.png)](device-manager-images/win/13-add-property-menu.png#lightbox)

6. Tıklatın **oluşturma** yeni cihaz oluşturmak için düğmeyi (alt sağ köşesinde):

    ![Oluştur düğmesi](device-manager-images/win/14-create-button.png)

7. Alma bir **lisans kabulünü** ekran. Tıklatın **kabul** lisans koşullarını kabul ediyorsanız:

    ![Lisans kabulünü ekranı](device-manager-images/win/15-license-acceptance.png)

8. Android Aygıt Yöneticisi'ni yeni cihaz görüntülenirken yüklü sanal aygıtların listesine ekler bir **oluşturma** İlerleme göstergesi cihaz oluşturma sırasında:

    [![Oluşturma İlerleme göstergesi](device-manager-images/win/16-creating-the-device-sml.png)](device-manager-images/win/16-creating-the-device.png#lightbox)

9. Oluşturma işlemi tamamlandığında, yeni cihaz ile yüklü sanal aygıtların listesi gösterilen bir **Başlat** düğmesi, başlatmak hazır:

   [![Yeni oluşturulan aygıt başlatmak hazır](device-manager-images/win/17-created-device-sml.png)](device-manager-images/win/17-created-device.png#lightbox)


### <a name="edit-device"></a>Cihaz Düzenle

Varolan bir sanal cihazın düzenlemek için cihazı seçin ve **Düzenle** düğmesini (ekranın sağ üst köşesinde bulunan):

[![Düzenle düğmesi, yeni bir cihaz değiştirme](device-manager-images/win/19-edit-button-sml.png)](device-manager-images/win/19-edit-button.png#lightbox)

Tıklatarak **Düzenle** seçili sanal cihaz için cihaz Düzenleyici başlatılır:

[![Cihaz Düzenleyicisi ekranı](device-manager-images/win/20-device-editor-sml.png)](device-manager-images/win/20-device-editor.png#lightbox)

**Aygıt Düzenleyicisi** ekran ilk sütununda, sanal cihaz özelliklerini ikinci sütundaki her bir özellik karşılık gelen değerleri listeler. Bir özellik seçtiğinizde, bu özellik için ayrıntılı bir açıklama sağ tarafta görüntülenir.

Örneğin, aşağıdaki ekran görüntüsünde `hw.lcd.density` özelliği değiştirildiğinde **420** için **240**:

[![Cihaz düzenleme örneği](device-manager-images/win/21-device-editing-sml.png)](device-manager-images/win/21-device-editing.png#lightbox)

Gerekli yapılandırma değişikliklerini yaptıktan sonra tıklatın **kaydetmek** düğmesi.
Sanal cihaz özelliklerini değiştirme hakkında daha fazla bilgi için bkz: [düzenleme Android sanal cihaz özellikleri](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Ek Seçenekler

Cihazları ile çalışmak için ek seçenekler kullanılabilir &hellip; sağ üst köşede menüde:

[![Ek Seçenekler menüsünü konumu](device-manager-images/win/22-overflow-menu-sml.png)](device-manager-images/win/22-overflow-menu.png#lightbox)

Ek Seçenekler menüsünü aşağıdaki öğeleri içerir:

-   **Yinelenen ve düzenleme** &ndash; şu anda seçili cihaz yineler ve bunun içinde açılacak **yeni cihaz** ekran farklı benzersiz bir ad ile. Örneğin, seçme **VisualStudio_android 23_x86_phone** tıklatıp **yinelenen ve düzenleme** bir sayaç adına ekler:

    [![Yinelenen ve düzenleme ekranı](device-manager-images/win/23-dupe-and-edit-sml.png)](device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **Explorer'da ortaya** &ndash; sanal aygıt için dosyalarını tutan klasöründe bir Windows Explorer penceresi açar. Örneğin, seçme **Nexus 5 X API 25** tıklatıp **Explorer'da ortaya** aşağıdaki gibi bir pencere açılır:

    [![Tıklama sonuçları Gezgininde ortaya](device-manager-images/win/24-reveal-in-explorer-sml.png)](device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Fabrika sıfırlaması** &ndash; Seçilen aygıt çalıştırılırken cihaz iç durumuna kullanıcı değişiklikleri silme kendi varsayılan ayarlara sıfırlar (Bu da geçerli sildiği [hızlı önyükleme](~/android/deploy-test/debugging/android-sdk-emulator/running-the-emulator.md#quick-boot) anlık görüntü Eğer varsa). Bu değişiklik, sanal cihaz oluşturma ve düzenleme sırasında yaptığınız değişiklikler değiştirmez. Bu sıfırlama alınamaz anımsatıcı ile bir iletişim kutusu görünür. Tıklatın **kullanıcı verilerini temizleme** sıfırlama onaylamak için.

-   **Silme** &ndash; seçili sanal aygıt kalıcı olarak siler.
    Bir iletişim kutusu, bir cihazın silinmesi geri alınamaz anımsatıcısı görüntülenir. Tıklatın **silmek** cihazı silmek istediğiniz emin olması durumunda.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu makalede Android Aygıt Yöneticisi'ni oluşturma, çoğaltma, özelleştirme ve Android sanal cihaz başlatmak için nasıl kullanılacağı açıklanmaktadır.

[![Aygıtlar sekmesindeki Android Aygıt Yöneticisi'nin ekran görüntüsü](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Bu kılavuz yalnızca Visual Studio Mac için geçerlidir.
Xamarin Studio ile Android Aygıt Yöneticisi'ni uyumlu değil.

Android Aygıt Yöneticisi'ni oluşturmak ve yapılandırmak için kullandığınız *Android sanal cihaz* (AVDs) çalıştırın [Google Android öykünücüsü](~/android/deploy-test/debugging/android-sdk-emulator/index.md).
Her AVD, fiziksel bir Android cihazı taklit eden bir öykünücü yapılandırmadır. Çalıştırın ve farklı fiziksel Android cihazları benzetimini yapılandırmaları çeşitli uygulamanızı test etmek mümkün kılar.

## <a name="requirements"></a>Gereksinimler

- Visual Studio Mac 7.5 veya üstü.

- Android SDK 8.0 (API 26) veya sonrası Android SDK Yöneticisi aracılığıyla yüklü olmalıdır.


## <a name="launching-the-device-manager"></a>Aygıt Yöneticisi'ni başlatma

Android Aygıt Yöneticisi'ni tıklatarak başlatma **Araçlar > Aygıt Yöneticisi'ni**:

[![Araçlar menüsünden başlatma](device-manager-images/mac/16-tools-menu-sml.png)](device-manager-images/mac/16-tools-menu.png#lightbox)

Android Aygıt Yöneticisi'ni kullanmadan önce Android SDK Araçları sürüm 26.0.2 yüklemelisiniz veya sonraki bir sürümü. Android SDK 26.0.2 araçları veya sonrası yüklü değil, başlatılırken bu hata iletişim kutusu görürsünüz:

![Android SDK'sı örneği hata iletişim kutusu](device-manager-images/mac/02-sdk-instance-error.png)

Bu hata iletişim kutusu görürseniz tıklatın **Tamam** Android SDK Yöneticisi'ni açın. Android SDK Yöneticisi'nde **Araçları** sekmesinde ve aşağıdaki yükleyin:

-   **Android SDK Araçları 26.0.2** veya daha yenisi 
-   **Android SDK platformunuzun Araçlar 26.0.0** veya daha yenisi 
-   **Android SDK derleme-araçları 26.0.0** veya daha yenisi

Bu paketleri ile gösterilmesi gereken **yüklü** aşağıdaki ekran görüntüsünde görülen durumu:

[![Android SDK Araçları 26.0 yükleme](device-manager-images/mac/03-sdk-tools-sml.png)](device-manager-images/mac/03-sdk-tools.png#lightbox)

## <a name="main-screen"></a>Ana Ekran

Android Aygıt Yöneticisi'ni ilk başlattığınızda, şu anda yapılandırılmış tüm sanal cihazlar görüntüleyen bir ekran gösterir. Her cihaz için **adı**, **sistem görüntüsü** (Android API düzey), **CPU**, **bellek** boyutu ve ekran çözünürlüğü görüntülenir:

[![Yüklü aygıtların listesi ve bunların parametrelerini](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

Tıklatın bir **Yürüt** düğmesi öykünücü tercih ettiğiniz sanal cihazla başlatmak için:

[![Cihaz görüntüsü için Başlat düğmesi](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

Seçili sanal cihazla öykünücü başladıktan sonra **Yürüt** düğmesi değişiklikleri bir **durdurmak** öykünücü durdurmak için kullanabileceğiniz düğmesi:

[![Çalışan cihazın düğmesi Durdur](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

### <a name="new-device"></a>Yeni cihaz

Yeni bir cihaz oluşturmak için tıklatın **yeni cihaz** düğmesini (ekranın sağ üst bölümünde bulunur):

[![Yeni bir cihaz oluşturmak için yeni düğmesi](device-manager-images/mac/08-new-button-sml.png)](device-manager-images/mac/08-new-button.png#lightbox)

Tıklatarak **yeni cihaz** başlatır **yeni cihaz** ekran:

[![Yeni cihaz ekranı Aygıt Yöneticisi'nin](device-manager-images/mac/09-new-device-editor-sml.png)](device-manager-images/mac/09-new-device-editor.png#lightbox)

Yeni bir cihaz yapılandırmak için aşağıdaki adımları kullanın **yeni cihaz** ekran:

1. Tıklayarak benzetmek için fiziksel bir cihaz seçin **aygıt** açılır menü:

    [![Cihaz aşağı açılır menüsü](device-manager-images/mac/10-device-menu-sml.png)](device-manager-images/mac/10-device-menu.png#lightbox)

2. Tıklayarak bu sanal cihazla kullanmak için Sistem Görüntüsü Seç **sistem görüntüsü** açılır menü. Bu menü altında yüklü sistem Aygıt Yöneticisi'ni görüntüleri listeler **yüklü**. **karşıdan** (gösterilen varsa) bölümü geliştirme bilgisayarınızda şu anda kullanılabilir olan, ancak otomatik olarak yüklenen sistem cihaz Yöneticisi görüntüleri listeler:

    [![Sistem görüntüsü aşağı açılır menüsü](device-manager-images/mac/11-system-image-menu-sml.png)](device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Cihazı yeni bir ad verin. Aşağıdaki örnekte, yeni cihaz adlı **Nexus 5 X API 25**:

    [![Yeni cihaz adlandırma](device-manager-images/mac/12-device-name-sml.png)](device-manager-images/mac/12-device-name.png#lightbox)

4. Değişiklik yapmanız özelliklerini düzenleyin. Özelliklerde değişiklik için bkz: [düzenleme Android sanal cihaz özellikleri](~/android/get-started/installation/android-emulator/device-properties.md).

5. Açıkça ayarlamak için gereken ek özellikleri ekleyin. **Yeni cihaz** ekran yalnızca en sık değiştirilen özellikleri listeler, ancak tıklayabilirsiniz **Özellik Ekle** aşağı açılır menüde (alt sol köşe) ek özellikler eklemek için:

    [![Özellik aşağı açılır menü ekleme](device-manager-images/mac/13-add-property-menu-sml.png)](device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Tıklatarak **özel** aygıt için yeni bir özellik tanımlamak için:

    ![Oluştur düğmesi](device-manager-images/mac/14-custom-button.png)

7. Tıklatın **oluşturma** yeni cihaz oluşturmak için düğmeyi (alt sağ köşesinde):

    ![Oluştur düğmesi](device-manager-images/mac/15-create-button.png)

8. Alma bir **lisans kabulünü** ekran. Tıklatın **kabul** lisans koşullarını kabul etmesi durumunda.

9. Android Aygıt Yöneticisi'ni yeni cihaz görüntülenirken yüklü sanal aygıtların listesine ekler bir **oluşturma** İlerleme göstergesi cihaz oluşturma sırasında:

    [![Oluşturma İlerleme göstergesi](device-manager-images/mac/17-creating-the-device-sml.png)](device-manager-images/mac/17-creating-the-device.png#lightbox)

10. Oluşturma işlemi tamamlandığında, yeni cihaz aygıt listesinde gösterilen bir **Yürüt** düğmesi, başlatmak hazır:

   [![Yeni oluşturulan aygıt başlatmak hazır](device-manager-images/mac/18-created-device-sml.png)](device-manager-images/mac/18-created-device.png#lightbox)


### <a name="edit-device"></a>Cihaz Düzenle

Varolan bir sanal cihazın düzenlemek için seçin **ek seçenekler** açılır menü (dişli simgesi) ve select **Düzenle**:
 
[![Yeni bir cihaz değiştirme menü seçimi Düzenle](device-manager-images/mac/19-edit-button-sml.png)](device-manager-images/mac/19-edit-button.png#lightbox)

Tıklatarak **Düzenle** seçili sanal cihaz için cihaz Düzenleyici başlatılır:
 
[![Cihaz Düzenleyicisi ekranı](device-manager-images/mac/20-device-editor-sml.png)](device-manager-images/mac/20-device-editor.png#lightbox)

**Aygıt Düzenleyicisi** ekran ilk sütununda, sanal cihaz özelliklerini ikinci sütundaki her bir özellik karşılık gelen değerleri listeler. Bir özellik seçtiğinizde, bu özellik için ayrıntılı bir açıklama sağ tarafta görüntülenir.

Örneğin, aşağıdaki ekran görüntüsünde `hw.lcd.density` özelliği değiştirildiğinde **320** için **240** ve `hw.ramSize` özelliği değiştirildiğinde **768**:
 
[![Cihaz düzenleme örneği](device-manager-images/mac/21-device-editing-sml.png)](device-manager-images/mac/21-device-editing.png#lightbox)

Gerekli yapılandırma değişikliklerini yaptıktan sonra tıklatın **kaydetmek** düğmesi.
Sanal cihaz özelliklerini değiştirme hakkında daha fazla bilgi için bkz: [düzenleme Android sanal cihaz özellikleri](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Ek Seçenekler

Bir aygıt ile çalışmak için ek seçenekler solunda bulunan aşağı açılır menüden kullanılabilir **Yürüt** düğmesi:

[![Ek Seçenekler menüsünü konumu](device-manager-images/mac/22-overflow-menu-sml.png)](device-manager-images/mac/22-overflow-menu.png#lightbox)

Ek Seçenekler menüsünü aşağıdaki öğeleri içerir:

-   **Düzen** &ndash; şu anda seçili aygıt daha önce açıklandığı gibi cihaz Düzenleyicisi'nde açar.

-   **Yinelenen ve düzenleme** &ndash; şu anda seçili cihaz yineler ve bunun içinde açılacak **yeni cihaz** ekran farklı benzersiz bir ad ile.
    Örneğin, seçme **Nexus 5 X API 25** tıklatıp **yinelenen ve düzenleme** bir sayaç adına ekler:

    [![Yinelenen ve düzenleme ekranı](device-manager-images/mac/23-dupe-and-edit-sml.png)](device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Finder ortaya** &ndash; sanal aygıt için dosyalarını tutan klasöründe macOS Bulucu penceresi açılır. Örneğin, seçme **Nexus 5 X API 25** tıklatıp **Finder ortaya** aşağıdaki gibi bir pencere açılır:

    [![Tıklama sonuçları Gezgininde ortaya](device-manager-images/mac/24-reveal-in-finder-sml.png)](device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Fabrika sıfırlaması** &ndash; Seçilen aygıt çalıştırılırken cihaz iç durumuna kullanıcı değişiklikleri silme kendi varsayılan ayarlara sıfırlar. Bu değişiklik, sanal cihaz oluşturma ve düzenleme sırasında yaptığınız değişiklikler değiştirmez. Bu sıfırlama alınamaz anımsatıcı ile bir iletişim kutusu görünür. Tıklatın **kullanıcı verilerini temizleme** sıfırlama onaylamak için.

-   **Silme** &ndash; seçili sanal aygıt kalıcı olarak siler.
    Bir iletişim kutusu, bir cihazın silinmesi geri alınamaz anımsatıcısı görüntülenir. Tıklatın **silmek** cihazı silmek istediğiniz emin olması durumunda.

-----

## <a name="summary"></a>Özet

Bu kılavuz Visual Studio'da kullanılabilir Android cihaz Yöneticisi için Xamarin Mac ve Visual Studio Araçları için kullanıma sunuldu. Başlatma ve durdurma Android öykünücüsünde çalıştırmak için bir Android sanal cihazı (AVD) seçme gibi temel özellikleri yeni sanal cihazlar ve sanal cihazı düzenleme oluşturma açıklanmıştır. Ayrıca, daha fazla özelleştirme profili donanım özelliklerini düzenlemek nasıl açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android SDK Aracı Üzerindeki Değişiklikler](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android SDK öykünücüsü ile hata ayıklama](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [Sürüm Notları (Google) SDK Araçları](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
