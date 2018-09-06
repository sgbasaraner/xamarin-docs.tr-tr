---
title: Xamarin Live Player uygulaması
description: Bu belgede, Xamarin Live Player kod değişikliklerinin önizlemesini görüntülemek için kullanılan uygulama, Canlı cihaz üzerinde açıklanmıştır. Bu kurulum, örnekler, günlükleri, cihazları yönetebilir ve daha fazlasını ayarları açıklanır.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 08/08/2017
ms.openlocfilehash: b3166aa440cbe2981d597771b360373fadc6451b
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780665"
---
# <a name="xamarin-live-player-app"></a>Xamarin Live Player uygulaması

![Önizleme özelliği](~/media/shared/preview.png)

Uygulamasını telefonunuza yükledikten sonra takip [kurulum yönergeleri](~/tools/live-player/install.md) bilgisayarınıza bağlanmak için. Aşağıdakilerden birini deneyin [örnek uygulamalar](~/tools/live-player/samples.md) bunu çalıştırmak için.

Başlangıçta, Xamarin Live Player uygulamasını şu şekilde görünür:

![Live Player Android uygulama ekran görüntüsü](player-images/app-android-sml.png)

Bastığınızda **Visual Studio ile eşleştirme**, kamera, bilgisayarınızda gösteren barkodu taratmasının istendiği kullanın:

![Android barkod tarayıcı ekran görüntüsü](player-images/scan-android-sml.png)

Bağlantı başarılı olursa, cihazı hemen kodun çalışması gereken (gibi [hesaplayıcı örnek](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![Cihaz üzerinde çalışan örnek hesaplayıcısı uygulaması](player-images/basic-calculator-sml.png)

## <a name="options"></a>Seçenekler

Bilgi düğmesine basın **(i)** göstermek üzere uygulamanın alt kısmındaki **seçenekleri** menüsü:

[![Seçenekler menüsünün ekran görüntüsü](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Günlükleri

Sorunları tanılamak için günlükleri görüntüleyin.

### <a name="settings"></a>Ayarlar

- Derleme ve çalışma zamanı hataları görünümünü Aç/Kapat.
- Sürüm bilgisi.
- Geri bildirim gönderin.

[![Ekran görüntüsü ayarları](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Cihazları yönetme

Bir cihaz ilk kez bağlanmaya'ndaki yönergeleri izleyin. [gereksinimleri ve Kurulum](~/tools/live-player/install.md). Birden çok cihazı eşleştirin ve bunları IDE yönetin.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio'da **Araçlar > Xamarin Live Player > cihazları Yönet...**

![Cihazları penceresini yönetme](player-images/manage-tools-menu-vs.png)

Bu pencerede, aşağıdakileri sağlar:

- Kodu tarayarak yeni bir cihaz çifti
- Alternatif olarak, ekranda görüntülenen kodu yazarak bir cihaz eşlemeden
- Listeden var olan cihazları Kaldır

Bu gibi durumlarda, bu pencereyi ayrıca cihaz listesinden erişebilirsiniz.

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

Mac için Visual Studio'da **Araçlar > (Xamarin Live Player) cihazları Yönet...**

![Cihazları penceresini yönetme](player-images/manage-tools-menu.png)

Bu pencerede, aşağıdakileri sağlar:

- Kodu tarayarak yeni bir cihaz çifti
- Alternatif olarak, ekranda görüntülenen kodu yazarak bir cihaz eşlemeden
- Listeden var olan cihazları Kaldır

![Cihazları penceresini yönetme](player-images/manage.png)

Bu gibi durumlarda, bu pencereyi ayrıca cihaz listesinden erişebilirsiniz:

![Xamarin Live Player cihazları cihaz listesinden seçin.](player-images/manage-device-menu.png)

-----

Tüm sorunları bakın karşılaşırsanız [sınırlamalar ve sorun giderme](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Sınırlamalar](~/tools/live-player/limitations.md)
- [Sorun giderme](~/tools/live-player/troubleshooting.md)
- [Xamarin Live Player örnekleri](samples.md)
