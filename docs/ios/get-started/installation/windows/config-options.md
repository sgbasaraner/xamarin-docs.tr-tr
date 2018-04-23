---
title: Visual Studio 2017 yapılandırma
description: Bu makalede, Visual Studio 2017 için çeşitli Xamarin.iOS yapılandırma seçenekleri açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 22D82244-890D-4325-B3CC-C0AC49130BCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: ee08cf7d68bd9d10026f1c15d4438077349fe367
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="configuring-visual-studio-2017"></a>Visual Studio 2017 yapılandırma

_Bu makalede, Visual Studio için çeşitli Xamarin.iOS yapılandırma seçenekleri açıklanmaktadır._

## <a name="using-matching-xamarinios-versions"></a>Eşleşen Xamarin.iOS sürümleri kullanma

Visual Studio 2017 Mac yapı konakta yüklü Xamarin.iOS aynı sürümünü kullanmanız gerekir. Emin olmak için bu geçerlidir:

 - Visual Studio 2017 kullanıyorsanız seçin **kararlı** Visual Studio for Mac güncelleştirmelerini kanal

 - Visual Studio 2017 Preview kullanıyorsanız seçin **alfa** Visual Studio for Mac güncelleştirmelerini kanal

> [!NOTE]
> İle başlayarak [Visual Studio 2017 sürüm 15,6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Visual Studio 2017 otomatik olarak algılar Mac yapı konak Windows Xamarin.iOS aynı sürümünü kullanıyorsa. Sürüm uyuşmazlığı varsa, uzaktan Mac yapı ana bilgisayarda doğru sürümünü yüklemek Visual Studio 2017 sunar. Daha fazla bilgi için bir göz atalım [otomatik Mac sağlama](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning) bölümünü [Mac çiftine](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Kılavuzu.

## <a name="ios-toolbar"></a>iOS araç çubuğu

Bir iOS projesi Visual Studio 2017 açık olduğunda, iOS araç görünmesi gerekir.  Varsayılan olarak, bu Xamarin.iOS geliştirme için yararlı olan dört düğmelerini içerir:

![Visual Studio 2017'ın iOS araç](config-options-images/ios-toolbar.png "Visual Studio 2017'ın iOS araç çubuğu")

- **Mac çiftine** – Mac iletişim çiftine açar. Bir iOS projesi Visual Studio 2017 açık olduğunda etkin.
- **İOS simülatörü Göster** – yapı konak üzerinde Mac, iOS simülatörü'nü öne getirir. Bir iOS projesi Visual Studio 2017 açık olduğunda etkin.
- **Cihaz günlük** – cihaz günlüklerini inceleyin olanak tanıyan bir pencereyi getirir. Bir iOS projesi Visual Studio 2017 açık olduğunda etkin.
- **Derleme sunucusundaki IPA dosyasını Göster** – uygulaması .ipa dosyası konumunu gösteren Mac yapı ana bilgisayardaki bir pencere açılır. Bir .ipa oluşturulduğu bir yapı tamamladıktan sonra etkin.

Bu araç çubuğu görünmüyorsa açmak **Görünüm** Visual Studio 2017 menüde ve **Araç Çubukları > iOS**:

![İOS araç etkinleştirme](config-options-images/ios-toolbar-enable.png "iOS araç etkinleştirme")

## <a name="solution-platforms-drop-down-menu"></a>Çözüm platformları açılır menü

**Çözüm platformları** açılır menü sonraki yapınızın bir fiziksel aygıt ya da bir simulator hedeflediğini seçmenize olanak sağlar.

Emin olmak için standart araç çubuğundaki bu açılır menü görülebilir:

- Visual Studio 2017 standart araç çubuğunun sağ ucundaki aşağı oka tıklayın.
- Seçin **ekleme veya kaldırma düğmeleri** 
- Emin olun **çözüm platformları** madde işaretli:

![Çözüm platformları açılır menü etkinleştirme](config-options-images/solution-platforms-enable.png "çözüm platformları açılır menü etkinleştirme")

Açık bir iOS projesi ile **standart** ve **iOS** araç çubukları aşağıdaki ekran görüntüsünde şimdi benzer:

![Standart ve iOS araç çubukları](config-options-images/toolbars.png "standart ve iOS araç çubukları")


