---
title: Windows için Uzak iOS Simülatörü
description: Düğümlerde iOS simülatörü Windows için Windows Visual Studio 2017 yanında görüntülenen bir iOS simülatörü, uygulamaları test etmenizi sağlar.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: topgenorth
ms.author: toopge
ms.date: 05/11/2018
ms.openlocfilehash: b07cc24e63f4aa3ce4451e3bdb5819f1df1058c6
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34149813"
---
# <a name="remoted-ios-simulator-for-windows"></a>Windows için Uzak iOS Simülatörü

Düğümlerde iOS simülatörü Windows için Windows Visual Studio 2017 yanında görüntülenen bir iOS simülatörü, uygulamaları test etmenizi sağlar.

[![](ios-simulator-images/hero-sml.png "iOS simülatörü Windows üzerinde çalışan")](ios-simulator-images/hero.png#lightbox)

## <a name="getting-started"></a>Başlarken

Düğümlerde iOS simülatörü Windows için Visual Studio 2017 Xamarin bir parçası olarak otomatik olarak yüklenir. Kullanmak için şu adımları izleyin:

1. [Bir Mac yapı konağı Visual 2017 eşleştirin](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Visual Studio 2017 içinde iOS veya tvOS projesinde hata ayıklamayı Başlat. Windows için düğümlerde iOS simülatörü Windows makinenizde görünür.

## <a name="simulator-window"></a>Simulator penceresi

Benzetici 's pencerenin üstündeki araç çubuğunda yararlı düğmelerini içerir:

- **Ev** – iOS cihazında giriş düğmesini taklit eder
- **Kilit** – simulator (kilidini açmak için sağdan) kilitler
- **Ekran** – bir ekran görüntüsünü simulator kaydeder
- [**Ayarları** ](#settings) – klavye, konum ve diğer ayarları görüntüler
- [**Diğer Seçenekler** ](#other-options) – döndürme ve sallama hareketleri gibi çeşitli simulator seçeneklerini getirir

    [![](ios-simulator-images/maps-app-sml.png "Örnek iOS simülatörü eşlemeleri")](ios-simulator-images/maps-app.png#lightbox)

## <a name="settings"></a>Ayarlar

Araç çubuğunun dişli simgesine tıklayarak açılır **ayarları** penceresi:

[![](ios-simulator-images/settings-sml.png "iOS simülatörü ayarları")](ios-simulator-images/settings.png#lightbox)

Bu ayarlar, donanım klavye etkinleştirmek için aygıt gereken bir konum seçin sağlar (statik ve taşıma konumlar desteklenir) rapor Touch ID'yi etkinleştirmek ve içerik ve ayarların simulator için sıfırlayın.

## <a name="other-options"></a>Diğer seçenekleri

Araç çubuğunun üç nokta düğmesini döndürme, sallama hareketleri ve yeniden başlatma gibi diğer seçenekleri ortaya çıkarır. Bu aynı seçenekler benzetici 's penceresinde herhangi bir yere sağ tıklayarak bir liste olarak görüntülenebilir:

[![](ios-simulator-images/more-sml.png "iOS simülatörü ek ayarlar")](ios-simulator-images/more.png#lightbox)

## <a name="touchscreen-support"></a>Dokunmatik desteği

Çoğu modern Windows bilgisayarları dokunmatik ekranlar vardır. Düğümlerde iOS simülatörü for Windows Dokunma etkileşimleri desteklediğinden, aynı tutarak, geçirme ve fiziksel iOS cihazlarla kullanım çok parmak dokunma hareketleri ile uygulamanızı test edebilirsiniz.

Benzer şekilde, düğümlerde iOS simülatörü Windows için Windows kalem giriş Apple kalem girdi olarak işler.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Düğümlerde iOS simülatörü Windows için devre dışı bırakma

Düğümlerde iOS simülatörü Windows için devre dışı bırakmak için gidin **Araçlar > Seçenekler > Xamarin > iOS ayarları** ve onay kutusunu temizleyin **Windows Uzak Simulator**.

[![](ios-simulator-images/options-sml.png "Simulator kullanmak için onay kutusu")](ios-simulator-images/options.png#lightbox)

Açılır hata ayıklama devre dışı bırakıldıysa, bu seçenek ile ana bilgisayar bağlı Mac Simulator'da iOS derleyin.
