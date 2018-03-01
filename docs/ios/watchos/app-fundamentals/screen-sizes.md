---
title: "Ekran boyutlarıyla çalışma"
ms.topic: article
ms.prod: xamarin
ms.assetid: 156D6D1C-83CA-4088-BA08-40B22312269C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c64e5821c1067b64caf3afc85c0e290a354e914d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-screen-sizes"></a>Ekran boyutlarıyla çalışma

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

- [WatchOS 3 giriş](~/ios/watchos/platform/introduction-to-watchos3/index.md)
