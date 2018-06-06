---
title: Xamarin Canlı oynatıcı uygulaması
description: Bu belgede Xamarin Canlı Önizleme kod değişiklikleri için kullanılan uygulama Canlı aygıtta Player açıklanmaktadır. Bu kurulum, örnekleri, günlükleri, aygıtları ve daha fazla yönetme ayarları açıklanır.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 05/14/2017
ms.openlocfilehash: 88f7f62650484007c221aa7baaa684f872e0a8e9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794156"
---
# <a name="xamarin-live-player-app"></a>Xamarin Canlı oynatıcı uygulaması

![Önizleme özelliği](~/media/shared/preview.png)

Telefonunuzda uygulama yüklendiğinde izleyin [kurulum yönergeleri](~/tools/live-player/install.md) bilgisayarınıza bağlanmak için. Aşağıdakilerden birini deneyin [örnek uygulamaları](~/tools/live-player/samples.md) bunu çalıştırmak için.

Başlangıçta, Xamarin Canlı oynatıcı uygulaması şu şekilde görünür (iOS ve Android sırasıyla):

![Canlı Player iOS uygulama ekran görüntüsü](player-images/app-iphone-sml.png) ![Canlı Player Android uygulama ekran görüntüsü](player-images/app-android-sml.png)

Bastığınızda **Visual Studio çiftine**, bilgisayarınızda gösteren barkod taramak için kameranızı kullanın:

![İOS barkod tarayıcısı ekran görüntüsü](player-images/scan-iphone-sml.png) ![Android barkod tarayıcısı ekran görüntüsü](player-images/scan-android-sml.png)

Bağlantı başarılı olursa, cihazda hemen kod çalıştırmanız gerekir (gibi [hesaplayıcı örnek](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![Cihazda çalışan örnek hesaplayıcı uygulama](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Seçenekler

Bilgi düğmesine basın **(i)** ortaya çıkarmak üzere uygulama altındaki **seçenekleri** menüsü:

[![Seçenekler menüsünün ekran görüntüsü](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Günlükleri

Sorunlara tanı koymak için günlüklere bakın.

### <a name="settings"></a>Ayarlar

- Derleme ve çalışma zamanı hataları görüntüsünü değiştir.
- Sürüm bilgileri.
- Geri bildirim gönderin.

[![Ayarlarının ekran görüntüsü](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Cihazları yönetme

Bir cihaz ilk kez bağlanmak için'ndaki yönergeleri izleyin [gereksinimleri & Kurulum](~/tools/live-player/install.md). Birden çok cihazı (örneğin bir iOS ve bir Android) eşleştirin ve bunları IDE yönetebilirsiniz.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio'da, **Araçlar > Xamarin Canlı Player > cihazları yönetme...**

![Aygıtları penceresini yönetme](player-images/manage-tools-menu-vs.png)

Bu pencere şunları yapmanızı sağlar:

- Kod tarayarak yeni bir cihaz çifti
- Alternatif olarak, ekranda görüntülenen kodu yazarak bir aygıtı eşleştirin.
- Listeden var olan cihazları kaldırın

Bu pencere aygıt listesinden de erişebilirsiniz.

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

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
- [Xamarin Canlı Player örnekleri](samples.md)
