---
title: "Aygıtı geliştirme için ayarlama"
description: "Bu makalede, bir Android cihaz kurulumu ve böylece cihaz çalıştırın ve Xamarin.Android uygulamalarında hata ayıklamak için kullanılabilir bir bilgisayara bağlamak nasıl ele alınacaktır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9116A3AA-EA00-56AF-AE70-BAEEC045EF11
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 64036af82ea49ad4d758a89767ff0da02eef094f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="set-up-device-for-development"></a>Aygıtı geliştirme için ayarlama

_Bu makalede, bir Android cihaz kurulumu ve böylece cihaz çalıştırın ve Xamarin.Android uygulamalarında hata ayıklamak için kullanılabilir bir bilgisayara bağlamak nasıl ele alınacaktır._

Şimdi tarafından Android öykünücüsünde çalışır harika yeni uygulamanızı büyük olasılıkla gördünüz ve parlak Android cihazında çalışmasını görmek istediğinizi açıklayın. Hata ayıklama için bir bilgisayara bir aygıt bağlanmayı ile ilgili adımlar şunlardır:

1.  **Hata ayıklama cihazda etkinleştirmek** -varsayılan olarak, Android cihazda uygulamalarda hata ayıklama mümkün olmayacaktır.

2.  **USB sürücüleri yüklemek** -bu adımın OS X bilgisayarlar için gerekli değildir. Windows bilgisayarları USB sürücülerinin yüklenmesini gerektirebilir.

3.  **Aygıt bilgisayara bağlanın** -cihazın USB veya WiFi bilgisayara bağlanmasını son adım içerir.

Bu adımların her biri aşağıdaki bölümlerde daha ayrıntılı olarak ele alınacaktır.


## <a name="enable-debugging-on-the-device"></a>Cihaz üzerinde hata ayıklamayı etkinleştir

Android uygulama test etmek için herhangi bir Android cihaz kullanmak da mümkündür. Ancak, aygıt hata ayıklama önce düzgün şekilde yapılandırılmalıdır oluşabilir. Adımlarını Android cihazda çalışan sürümüne bağlı olarak biraz farklılık gösterir.


### <a name="android-40-to-android-41"></a>Android 4.0 Android 4.1 için

Android 4.1.x için Android 4.0.x için hata ayıklama, aşağıdaki adımları izleyerek etkinleştirilir:

1.  Git **ayarları** ekran.
2.  Seçin **Geliştirici seçenekleri** .
3.  Onay kutusunu temizleyin **USB hata ayıklama** seçeneği.

Bu ekran gösterir **Geliştirici seçenekleri** Android 4.0.3 çalıştıran bir cihazda ekran:

[![Geliştirici seçenekleri](set-up-device-for-development-images/developer-options-sml.png)](set-up-device-for-development-images/developer-options.png#lightbox)


### <a name="android-42-and-higher"></a>Android 4.2 ve üzeri

Android 4.2 ve üzeri, başlangıç **Geliştirici seçenekleri** varsayılan olarak gizlidir. Kullanılabilir hale getirmek için şu adrese gidin **ayarlar > telefon hakkında**ve dokunun **yapı numarası** öğesi ortaya çıkarmak üzere yedi defa **Geliştirici seçenekleri** sekmesi:

[![Yapı numarası öğesi](set-up-device-for-development-images/about-phone-sml.png)](set-up-device-for-development-images/about-phone.png#lightbox)

Bir kez **Geliştirici seçenekleri** sekmesi altında kullanılabilir **ayarlar > Sistem**, geliştirici ayarları ortaya çıkarmak üzere açın:

[![Geliştirici ayarları ekranı](set-up-device-for-development-images/developer3.png)](set-up-device-for-development-images/developer3.png#lightbox)

Bu USB hata ayıklama gibi geliştirici seçenekleri etkinleştirip uyanık modu kalmak yerdir.


## <a name="install-usb-drivers"></a>USB sürücüleri yükleyin

Bu adım, OS X için gerekli değildir. Bağlamanız yeterlidir cihaza bir USB kablosu ile mac'e.

Bir Windows bilgisayarda bir Android cihazını USB ile bağlı algılar önce bazı ek sürücüler yüklemeniz gerekebilir.

> [!NOTE]
> Bunlar Google Nexus aygıtı ayarlama adımlarını ve bir başvuru olarak sağlanır. Belirli cihazınıza adımları farklılık gösterebilir, ancak benzer bir desen izler. Konusunda sorun yaşıyorsanız, cihazınız için Internet arayın.

Çalıştırma **android.bat** uygulama **[Android SDK yükleme yolu] \tools** dizin. Varsayılan olarak, Xamarin.Android yükleyici, Android SDK'sı konumu bir Windows bilgisayarda aşağıdaki koyun:

    C:\Users\[username]\AppData\Local\Android\android-sdk


### <a name="download-the-usb-drivers"></a>USB sürücüleri yükleyin

Google Nexus (Galaxy Nexus) hariç cihazları Google USB sürücüsü gerektirir. Galaxy Nexus sürücü [Samsung tarafından dağıtılan](http://www.samsung.com/us/support/downloads/).
Tüm Android cihazları kullanması gereken [ilgili üreticiye USB sürücüsünden](http://developer.android.com/tools/extras/oem-usb.html#Drivers).

Yükleme **Google USB sürücüsü** Android SDK Manager'ı başlatıp genişleterek tarafından paket **ek özellikler** izleyin ekran görüntüsünde görüldüğü gibi klasör:

[![Seçili Google USB sürücü paketi](set-up-device-for-development-images/usbdriverpackage.png)](set-up-device-for-development-images/usbdriverpackage.png#lightbox)

Denetleme **Google USB sürücüsü** ve'ı tıklatın **yükleme** düğmesi.
Sürücü dosyaları şu konuma yüklenir:

    [Android SDK install path]\extras\google\usb\_driver

Xamarin.Android yükleme için varsayılan yolu şöyledir:

    C:\Users\[username]\AppData\Local\Android\android-sdk\extras\google\usb_driver



### <a name="installing-the-usb-driver"></a>USB sürücüsü yükleme

USB sürücüleri yüklendikten sonra bunları yüklemeniz gereklidir.
Windows 7'de sürücüleri yüklemek için:

1.  Aygıtınızı bir USB kablosuyla bilgisayarınıza bağlayın.

2.  Masaüstü bilgisayardan veya Windows Gezgini'nde sağ tıklatın ve seçin **Yönet** .

3.  Seçin **aygıtları** sol bölmede.

4.  Bulup genişletin **diğer aygıtlar** sağ bölmede.

5.  Cihaz adını sağ tıklatıp **sürücü yazılımı güncelleştirme** .
    Bu Donanım Güncelleştirme Sihirbazı'nı başlatır.

6.  Seçin **sürücü yazılımı için bilgisayarıma Gözat** tıklatıp **sonraki** .

7.  Tıklatın **Gözat** ve USB sürücü klasörünü bulun (Google USB sürücüsü bulunan **[Android SDK yükleme yolu] \extras\google\usb_driver**.

8.  Tıklatın **sonraki** sürücüyü yüklemek için.


### <a name="installing-unverified-drivers-in-windows-8"></a>Windows 8 doğrulanmamış sürücüleri yükleme

Windows'da doğrulanmamış bir sürücü yüklemek için ek adımlar gerekli
8. Aşağıdaki adımları sürücülerin Galaxy Nexus için nasıl yükleneceği açıklanmaktadır:

1.  **Windows 8 Gelişmiş Önyükleme Seçenekleri erişim** -Gelişmiş Önyükleme Seçenekleri erişmek için bilgisayar yeniden başlatıldığından bu adımı içerir. Komut satırı komut istemi başlatın ve aşağıdaki komutu kullanarak bilgisayarı yeniden başlatın:

        shutdown.exe /r /o

2.  **Cihaza bağlanın** -cihazı bilgisayara bağlanın

3.  **Aygıt Yöneticisi'ni başlatın** - çalışma **devmgmt.msc**; Cihazınızı ile sarı bir üçgen üzerine listede görmeniz gerekir.

4.  **Aygıt sürücülerini yüklemek** -yukarıda açıklandığı gibi aygıt sürücülerini yükleyin.



## <a name="connect-the-device-to-the-computer"></a>Aygıt bilgisayara bağlanın

Aygıt bilgisayara bağlanmak için son adımdır bakın. Bunu yapmanın iki yolu vardır:

-   **USB kablosu** -kolay ve en yaygın yolu budur. Yalnızca USB kablosu cihaz ve sonra da bilgisayarda takın.

-   **WiFi** -Android cihazı bir USB kablosu WiFi kullanmadan bir bilgisayara bağlanmak mümkündür. Bu teknik biraz daha fazla çaba gerektiriyor, ancak hiçbir USB kablosu yok ya da cihaz için uzakta için USB kablosu olduğunda yararlı olabilir. WiFi bağlanma sonraki bölümde ele alınacaktır.


### <a name="connecting-over-wifi"></a>WiFi bağlanma

Varsayılan olarak, [Android hata ayıklama köprüsü](http://developer.android.com/tools/help/adb.html) (*ADB*) ile bir Android cihazını USB ile iletişim kuracak şekilde yapılandırıldı. Yerine USB TCP/IP'yi kullanacak şekilde yeniden yapılandırmanız için mümkündür. Bunu yapmak için aygıt ve bilgisayar aynı WiFi ağ üzerinde olmalıdır. Komut satırından aşağıdaki adımları WIF sorunu hata ayıklamak için ortamınızın kurulumu için:

1.  Android Cihazınızı IP adresini belirler. IP adresidir altında aramak için çıkış bulmak için tek yönlü **ayarlar > Wi-Fi** ve ardından cihazın bağlandığı WiFi ağ seçeneğine dokunun. Bu ekran görüntüsünde görülen için benzer ağ bağlantısı hakkında bilgi gösteren ayarları ekranı getirir:

    ![IP adresi](set-up-device-for-development-images/ip-settings.png)

    Android bazı sürümlerinde, IP adresi var. listelenmez ancak bunun yerine altında bulunabilir **ayarlar > telefon hakkında > Durum**.

2.  Android Cihazınızı USB aracılığıyla bilgisayarınıza bağlayın.

3.  Ardından, ADB şekilde yeniden TCP bağlantı noktası 5555 kullanarak bu BT. Bir komut isteminden aşağıdaki komutu yazın:

        adb tcpip 5555

    Bu komutu verildikten sonra bilgisayarınız USB üzerinden bağlanan cihazları dinlemek mümkün olmaz.

4.  Cihazınızı bilgisayarınıza bağlanma USB kablosunun bağlantısını kesin.

5.  ADB 1. adımda belirtilen bağlantı noktası, Android Cihazınızda bağlanacak şekilde yapılandırın:

        adb connect 192.168.1.28:5555

    Bu komut işlemi tamamladıktan sonra Android cihaz WiFi üzerinden bilgisayara bağlı.

Bitirdiğinizde WiFi hata ayıklama, USB modu aşağıdaki komutla ADB başa olası sıfırlama değildir:

    adb usb

Bilgisayara bağlı aygıtları listelemek için ADB istemeniz mümkündür. Cihazları nasıl bağlandığını bağımsız olarak ne bağlandığını görmek için komut istemine aşağıdaki komutu gönderebilirsiniz:

    adb devices


## <a name="summary"></a>Özet

Bu makalede ele alınan cihazda hata ayıklama sağlayarak bir Android cihaz geliştirme için yapılandırma. Ayrıca, cihazın USB veya WiFi kullanarak bir bilgisayara bağlanmak nasıl ele.


## <a name="related-links"></a>İlgili bağlantılar

- [Android hata ayıklama köprüsü](http://developer.android.com/tools/help/adb.html)
- [Donanım aygıtlarını kullanma](http://developer.android.com/tools/device.html)
- [Samsung sürücü yüklemeleri](http://www.samsung.com/us/support/downloads/)
- [OEM USB sürücüleri](http://developer.android.com/tools/extras/oem-usb.html#Drivers)
- [Google USB Driver](http://developer.android.com/sdk/win-usb.html)
- [XDA geliştiricileri: Windows 8 - ADB/karşılayamamasına sürücü sorun çözüldü](http://forum.xda-developers.com/showthread.php?t=1583801)
