---
title: "Yıpranması aygıtta hata ayıklama"
description: "Bu makalede, bir Xamarin.Android yıpranması uygulaması yıpranması aygıtta hata ayıklama açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 5ec627fad1695bab8d05d75a5089fe849ea2fd75
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="debug-on-a-wear-device"></a>Yıpranması aygıtta hata ayıklama

_Bu makalede, bir Xamarin.Android yıpranması uygulaması yıpranması aygıtta hata ayıklama açıklanmaktadır._


## <a name="overview"></a>Genel Bakış

Android takmak Smartwatch gibi bir Android takmak cihazınız varsa, bir öykünücü kullanmak yerine cihazda uygulamayı çalıştırın. (Çalıştığınız henüz Android takmak uygulamaları dağıtma ve çalıştırma işlemi bilmiyorsanız bkz [Merhaba, takmak](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>Yıpranması cihaz hazırlayın:

Android takmak cihaz üzerinde hata ayıklamayı etkinleştirmek için aşağıdaki adımları kullanın:

1.  Açık **ayarları** Android takmak aygıt menüsünde.

2.  Kaydırma dokunun ve menü altına **hakkında**.

3.  Yapı numarası 7 kez dokunun.

4.  Üzerinde **ayarları** menü öğesine dokunun **Geliştirici seçenekleri**.

5.  Onaylayın **ADB hata ayıklama** etkinleştirilir.


## <a name="debugging-over-usb"></a>USB üzerinden hata ayıklama

Bir USB bağlantı noktasına yıpranması cihazınız varsa, yıpranması aygıt bilgisayarınıza bağlanabilir, dağıtma ve Android telefonla kullanır gibi uygulama Çalıştır/debug (daha fazla bilgi için bkz: [Debug bir cihazda](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Bluetooth üzerinden hata ayıklama

Yıpranması Cihazınızı bir USB bağlantı noktasına sahip değilse, uygulama yıpranması cihaza Bluetooth üzerinden bilgisayarınıza bağlı bir Android telefonla uygulamanın hata ayıklama çıktısı yönlendirerek dağıtabilirsiniz. 

### <a name="prepare-your-phone"></a>Telefonunuz hazırlama

Bluetooth bağlantıları yıpranması aygıta yapmak için telefonunuz hazırlamak için aşağıdaki adımları kullanın: 

1.  Zaten yapmadıysanız, telefonunuzun Xamarin.Android geliştirme açıklandığı şekilde ayarlama [ayarlamak yukarı geliştirme için cihazı](~/android/get-started/installation/set-up-device-for-development.md).

2.  Karşıdan yükleyip ücretsiz [Android takmak](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) Google Play Mağazası'ndan.

### <a name="connect-the-device"></a>Cihaza bağlanın

Telefonunuza yıpranması Cihazınızı bağlamak için aşağıdaki adımları kullanın:

1.  Telefonda, Bluetooth Ara (yukarıda yapılandırılan) olarak davranan, Android takmak uygulamayı başlatın. 

2.  Dokunun **ayarları** simgesi.

3.  Etkinleştirme **Bluetooth üzerinden hata ayıklama**. Android takmak uygulama ekranda görüntülenen aşağıdaki durum görmeniz gerekir:

        Host: disconnected
        Target: connected

4.  Telefon USB üzerinden bilgisayarınıza bağlayın. Bilgisayarınızda aşağıdaki komutları girin:

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    Bağlantı noktası 4444 kullanılabilir durumda değilse, erişiminiz olan diğer herhangi bir kullanılabilir bağlantı kullanabilirsiniz. 

    **Not**: Mac için Visual Studio veya Visual Studio yeniden başlatırsanız, bu komutları yıpranması aygıtına bağlantı Kurulum'u yeniden çalıştırmanız gerekir.

5.  Yıpranması aygıt istediğinde, size izin onaylayın **ADB hata ayıklama**. Android takmak uygulamada değiştirmek durumu görmeniz gerekir:

        Host: connected
        Target: connected

6.  Yukarıdaki adımları tamamladıktan sonra çalışan `adb devices` phone ve Android takmak cihaz durumunu gösterir:

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

Bu noktada, uygulamanız yıpranması cihaza dağıtabilirsiniz.

<a name="screenshots"/>

### <a name="taking-screenshots"></a>Ekran görüntüleri alma

Aşağıdaki komutu girerek bir ekran görüntüsünü yıpranması aygıt alabilir: 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

Ekran görüntüsü, aşağıdaki komutu girerek bilgisayarınıza kopyalayın:

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

Ekran cihazdaki aşağıdaki komutu girerek Sil:

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>Bir uygulamayı kaldırmasını

Aşağıdaki komutu girerek bir uygulama yıpranması aygıttan kaldırabilirsiniz:

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

Örneğin, paket adı ile uygulama kaldırmak için `com.xamarin.weartest`, aşağıdaki komutu girin:

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Android takmak cihazların Bluetooth üzerinden hata ayıklama hakkında daha fazla bilgi için bkz: [Bluetooth üzerinden hata ayıklama](https://developer.android.com/training/wearables/apps/bt-debugging.html).


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>Bir yardımcı telefon uygulaması yıpranması uygulamayla hata ayıklama

Android yıpranması uygulamaları Google play'de dağıtım için bir yardımcı Android telefon uygulaması ile paketlenmiştir (daha fazla bilgi için bkz: [paketle birlikte çalışma](~/android/wear/deploy-test/packaging.md)). Ancak, yine yıpranması uygulama ve onun yardımcı uygulama ayrı olarak geliştirin. Uygulamanızı Google Play mağazası aracılığıyla bıraktığınızda yıpranması uygulama yardımcı uygulamayla paketlenir ve mümkünse otomatik olarak yüklenir.

Bir yardımcı uygulamayla yıpranması uygulama hata ayıklamak için: 

1.  Derleme ve telefon yardımcı uygulama dağıtma.

2.  Yıpranması projesine sağ tıklayın ve varsayılan başlangıç projesi olarak ayarlayın.

3.  Yıpranması proje wearable cihaza dağıtabilirsiniz.

4.  Cihaza yıpranması uygulamanın hata ayıklama ve çalıştırın.

 
## <a name="summary"></a>Özet

Bu makalede, bir Android takmak aygıt Bluetooth üzerinden Visual Studio'dan yıpranması hata ayıklama için yapılandırma ve yardımcı telefon uygulaması yıpranması uygulamayla hata ayıklamak nasıl açıklanmıştır. Bluetooth aracılığıyla yıpranması uygulamayı hata ayıklama için ortak hata ayıklama ipuçları de sağlanır.
