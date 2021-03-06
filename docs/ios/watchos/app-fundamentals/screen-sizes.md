---
title: WatchOS ekran boyutlarında Xamarin ile çalışma
description: Bu belge, çeşitli watchOS ekran boyutlarıyla çalışma açıklar. Arabirimi Tasarımcısı, Simulator, watchOS watchOS açıklanır ve görüntü kaynakları.
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 30866c70879950acd8f43fd5880b1b24ba127fa4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790717"
---
# <a name="working-with-watchos-screen-sizes-in-xamarin"></a>WatchOS ekran boyutlarında Xamarin ile çalışma

Apple Watch iki ekran boyutlarında kullanılabilir:

- **38mm**
  - 136 x 170 mantıksal piksel (272 x 340 fiziksel piksel cinsinden)

- **42mm**
  - 156 x 195 mantıksal piksel (312 x 390 fiziksel piksel cinsinden).

Ekran boyutu tasarlama ve uygulamalarınızı test ederken dikkate almanız gerekir.

## <a name="watchos-interface-designer"></a>watchOS arabirimi Tasarımcısı

Visual Studio Mac Tasarımcısı için görüntüler varsayılan olarak arabirimi denetleyicilerde izlemek **herhangi Apple Watch**.

![](screen-sizes-images/screen-any-sml.png "Tüm Apple Watch Tasarımcısı görüntüler izleme arabirim denetleyicilerini")

Düzenle ve şeridinizin kullanılabilir ekran boyutlarına birini adresindeki önizlemek için boyutu menüsünü kullanın: **38mm** veya **42mm**:

![](screen-sizes-images/screen-menu-sml.png "38 mm veya 42 mm boyutunu seçme")

Daha büyük ekran boyutu bazen daha küçük ekranda kesilmiş/gizli olacak içerik işlemez.
Her iki boyutlarına test emin olun.


### <a name="interface-design"></a>Arabirimi

Uygulamanızı ekranında, boyutu, bağımsız olarak aynı içeriği görüntülemek ve genişletin veya öğeleri uygun şekilde sözleşme. Mac Tasarımcısı'nda özniteliği denetçisi için Visual Studio'da kullanmalısınız **kapsayıcısı göreli** veya **boyutuna uyacak içerik** sabit boyutları yerine.

![](screen-sizes-images/sizeattributepanel-sml.png "Kapsayıcı için göreli veya boyuta sığması içerik yerine sabit boyutları kullanın")

Gözcü ekran siyah bir çerçeve tarafından çevrelenmiş olduğundan Arabiriminizin çevresindeki dolgunun sağlama önerilmez. Ekranın kenarına karşı tutun ve uygulama doğal kenarlık form çerçeve izin öğeleri sağlar.


## <a name="watchos-simulator"></a>watchOS Simulator

Ne zaman simulator'da test kolayca geçiş yapabilirsiniz kullanarak iki ekran boyutlarına arasında **donanım > cihaz** menüsü.

![](screen-sizes-images/simulator.png "Benzetici donanım aygıtı menüsünü kullanarak iki ekran boyutlarına arasında geçiş yapabilirsiniz")


## <a name="image-resources"></a>Görüntü kaynakları

Tek bir varlık farklı boyutlarda iyi görünmüyorsa, birden çok görüntü varlıklarının kullanmanız gerekir. Görüntü varlık kataloglar için ayrı bit eşlemler her boyutu belirtilmesine izin ver:

![](screen-sizes-images/images-xcassets.png "Görüntü varlık Kataloğu Düzenleyicisi")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

Alternatif olarak, farklı görüntüleri tamamen yük ekran boyutunu belirlemek için kod kullanın:

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

Kullanma hakkında daha fazla okuma [görüntü denetimi](~/ios/watchos/user-interface/image.md).



## <a name="related-links"></a>İlgili bağlantılar

- [watchOS 3’e Giriş](~/ios/watchos/platform/introduction-to-watchos3/index.md)
