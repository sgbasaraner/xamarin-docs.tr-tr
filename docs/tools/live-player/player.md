---
title: "Xamarin Canlı oynatıcı uygulaması"
description: "Düzenle ve iOS veya Android cihazında gerçek zamanlı uygulamaları test etme"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/10/2017
ms.openlocfilehash: 6b0d62a9026c1248a66166e75ed41bb0148547a6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="xamarin-live-player-app"></a>Xamarin Canlı oynatıcı uygulaması

![Önizleme özelliği](~/media/shared/preview.png)

## <a name="get-the-app"></a>Uygulamayı Al

### <a name="xamarin-live-player-for-android"></a>Android için Xamarin Canlı Player
Google Play'de Android için Xamarin Canlı Player kullanılabilir:

[ ![Google play'de kullanılabilir](images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Google Play gerek kalmadan Android cihazlar için Xamarin Canlı Player aracılığıyla kullanılabilir [HockeyApp](https://aka.ms/xlp-hockeyapp) dağıtım. Android için seçim tarafından doğrudan Google Play'den yüklenebilir için erken Önizleme ek olarak, derlemeler [açık beta programı](https://play.google.com/apps/testing/com.xamarin.live)

### <a name="xamarin-live-player-for-ios"></a>İOS için Xamarin Canlı Player
Katılmak için kullanıcıların öneririz [Xamarin Canlı Player uygulama _iOS Önizleme_ ](https://aka.ms/liveplayeralpha) keyfini TestFlight üzerinden yapılan en son geliştirmeleri hızlı erişim için.



## <a name="using-the-app"></a>Uygulamasını kullanarak

Telefonunuzda uygulama yüklendiğinde izleyin [kurulum yönergeleri](~/tools/live-player/install.md) bilgisayarınıza bağlanmak için. Aşağıdakilerden birini deneyin [örnek uygulamaları](~/tools/live-player/samples.md) bunu çalıştırmak için.

Başlangıçta, Xamarin Canlı oynatıcı uygulaması şu şekilde görünür (iOS ve Android sırasıyla):

![Canlı Player iOS uygulama ekran görüntüsü](player-images/app-iphone-sml.png) ![Canlı Player Android uygulama ekran görüntüsü](player-images/app-android-sml.png)

Bastığınızda **Visual Studio çiftine**, bilgisayarınızda gösteren barkod taramak için kameranızı kullanın:

![İOS barkod tarayıcısı ekran görüntüsü](player-images/scan-iphone-sml.png) ![Android barkod tarayıcısı ekran görüntüsü](player-images/scan-android-sml.png)

Bağlantı başarılı olursa, kod hemen (örneğin hesaplayıcı örnek) aygıtta çalıştırmanız gerekir:

![Cihazda çalışan örnek hesaplayıcı uygulama](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Seçenekler

Bilgi düğmesine basın **(i)** ortaya çıkarmak üzere uygulama altındaki **seçenekleri** menüsü:

![Seçenekler menüsünün ekran görüntüsü](player-images/options.png)

### <a name="logs"></a>Günlükleri

Sorunlara tanı koymak için günlüklere bakın.

### <a name="settings"></a>Ayarlar

* Derleme ve çalışma zamanı hataları görüntüsünü değiştir.
* Sürüm bilgileri.
* Geri bildirim gönderin.

![Ayarlarının ekran görüntüsü](player-images/settings.png)

## <a name="managing-devices"></a>Cihazları yönetme

Bir cihaz ilk kez bağlanmak için'ndaki yönergeleri izleyin [gereksinimleri & Kurulum](~/tools/live-player/install.md). Birden çok cihazı (örneğin bir iOS ve bir Android) eşleştirin ve bunları IDE yönetebilirsiniz.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da, **Araçlar > Xamarin Canlı Player > cihazları yönetme...**

![Aygıtları penceresini yönetme](player-images/manage-tools-menu-vs.png)

Bu pencere şunları yapmanızı sağlar:

- Kod tarayarak yeni bir cihaz çifti
- Alternatif olarak, ekranda görüntülenen kodu yazarak bir aygıtı eşleştirin.
- Listeden var olan cihazları kaldırın

Bu pencere aygıt listesinden de erişebilirsiniz.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da, **Araçlar > (Xamarin Canlı Player) aygıtları yönetme...**

![Aygıtları penceresini yönetme](player-images/manage-tools-menu.png)

Bu pencere şunları yapmanızı sağlar:

- Kod tarayarak yeni bir cihaz çifti
- Alternatif olarak, ekranda görüntülenen kodu yazarak bir aygıtı eşleştirin.
- Listeden var olan cihazları kaldırın

![Aygıtları penceresini yönetme](player-images/manage.png)

Bu pencere aygıt listesinden de erişebilir:

![Xamarin Canlı Player aygıtları aygıt listesinden seçin](player-images/manage-device-menu.png)

-----

Tüm sorunları bakın karşılaşırsanız [sınırlamalar ve sorun giderme](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Sınırlamalar](~/tools/live-player/limitations.md)
- [Sorun giderme](~/tools/live-player/troubleshooting.md)
- [Xamarin Canlı Player örnekleri](~/tools/livehttps://developer.xamarin.com/samples.md)
