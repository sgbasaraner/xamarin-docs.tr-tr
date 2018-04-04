---
title: Düğümlerde iOS simülatörü (Windows)
description: Windows Visual Studio içinde tamamen test ve hata ayıklama iOS uygulamaları
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/07/2017
ms.openlocfilehash: bcf0aa2b1677af0b980c6ca48bb29c1cad32e52d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>Düğümlerde iOS simülatörü (Windows)

_Windows Visual Studio içinde tamamen test ve hata ayıklama iOS uygulamaları_

[![](ios-simulator-images/hero-sml.png "iOS simülatörü Windows üzerinde çalışan")](ios-simulator-images/hero.png#lightbox)

## <a name="download-and-install"></a>İndirme ve yükleme

Karşıdan [yükleyici](https://dl.xamarin.com/xamarin-simulator/Xamarin.Simulator.Installer.msi) ve Windows bilgisayarınıza yükleyin. Xamarin için Visual Studio Araçları önceden yüklenmiş olması gerekir.

> [!NOTE]
> Visual Studio uzaktan iOS simülatörü'nü kullanarak yüklü Xamarin ile ağa bağlı bir Mac gerektirir.

## <a name="getting-started"></a>Başlarken

Uzak iOS simülatörü kullanmak için:

1. Visual Studio uzaktan iOS simülatörü en az bir kez başlatmadan önce Mac bağlı emin olun.
2. Bir iOS veya tvOS uygulama olduğundan emin olun **başlangıç projesi** ve hata ayıklamayı Başlat.

Gelen uzak iOS simülatörü devre dışı bırakabilirsiniz **Araçlar > Seçenekler > Xamarin > iOS ayarları** kutusunu kaldırarak **Windows Uzak Simulator** burada gösterilen:

[![](ios-simulator-images/options-sml.png "Simulator kullanmak için onay kutusu")](ios-simulator-images/options.png#lightbox)

İOS simülatörü bağlı Mac bilgisayarda sonra açılır. Uzak iOS simülatörü yeniden açmak için bu seçeneği işaretleyin.

## <a name="features"></a>Özellikler

Uzak iOS simülatörü sınamak ve tamamen Visual Studio'dan Windows simulator iOS uygulamaları hatalarını ayıklamak için bir yol sağlar.

### <a name="simulator-window"></a>Simulator penceresi

Pencere araç çubuğu düğmeleri simulator ile etkileşim kurmak için çeşitli içerir:

- **Ev** – cihazın giriş düğmesini benzetimini yapar.
- **Kilit** – (sizin doğru çekin kilidini açmak için) simulator kilitler.
- **Ekran** – bir ekran görüntüsünü simulator diske kaydeder.
- [**Ayarları** ](#settings) – klavye ve yerini yapılandırın.
- Diğer [ **seçenekleri** ](#options) – çeşitli simulator seçenekleri döndürme, sallama gibi kullanılabilir veya benzetici diğer durumlarda çağırma. Bazı seçenekler yapılabileceği, araç çubuğunda veya penceresinde sağ tıklanarak görüntülenen üç nokta simgesinden erişilebilir.

    [![](ios-simulator-images/maps-app-sml.png "Örnek iOS simülatörü eşlemeleri")](ios-simulator-images/maps-app.png#lightbox)


### <a name="settings"></a>Ayarlar

"Dişli" simgesi açar **ayarları** penceresi:

[![](ios-simulator-images/settings-sml.png "iOS simülatörü ayarları")](ios-simulator-images/settings.png#lightbox)

Bu, simulator donanım klavyede etkinleştirmek ve hangi konumu (bir statik konumu ya da diğer taşıma konumu seçenekleri dahil) cihaza bildirilen seçin olanak sağlar.



### <a name="other-options"></a>Diğer seçenekleri

Sallama hareketi tetikleme ve simulator yeniden döndürme gibi simulator kullanılabilir tüm seçenekleri görüntülemek için simulator penceresinde herhangi bir yere sağ tıklatın:

[![](ios-simulator-images/more-sml.png "iOS simülatörü ek ayarlar")](ios-simulator-images/more.png#lightbox)

### <a name="touchscreen-support"></a>Dokunmatik desteği

Çoğu modern Windows bilgisayarları dokunmatik ekranlar vardır ve uzak iOS simülatörü kullanıcı etkileşimleri iOS uygulamanızı test etmek için simulator penceresi touch olanak tanır.

Bu çimdik, geçirme ve birden çok parmak dokunma hareketleri - daha önce yalnızca kolayca fiziksel cihazları test edilmesi şeyleri içerir.

Windows'ta kalem desteği, aynı zamanda Apple kalem giriş benzetici, çevrilir.

<!--
<a name="knownissues" />

# Known Issues

 - Apple Watch devices may show in the Visual Studio device list, but are not yet supported.
 - Launching in **Release** mode may also start Apple’s simulator on the networked Mac.
 - Closing the remote iOS Simulator on Windows will not immediately stop debugging in Visual Studio. Stop debugging manually from the menu or the red button.
 - Opening too many different simulators simultaneously will produce unexpected results.
 - Exception of type `Foundation.NSErrorException` may be thrown while launching Simulators. Workaround is to kill csproxy (server process) on the Mac host and re-deploy to the simulator.
 - Performance may be slower when using Xcode 8
-->
