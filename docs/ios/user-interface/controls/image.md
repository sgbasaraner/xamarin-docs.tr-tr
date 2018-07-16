---
title: Xamarin.iOS ile görüntüleri görüntüleme
description: Bu belge, Xamarin.ios'ta görüntüler açıklar. Program aracılığıyla ya da iOS Designer üzerinden bir uygulamaya ekleme görüntüleri kapsar.
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/13/2018
ms.openlocfilehash: 9777b4abf6e7f370178bcff2cb40666612888a9f
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038384"
---
# <a name="displaying-images-with-xamarinios"></a>Xamarin.iOS ile görüntüleri görüntüleme

Görüntüleri uygulamanıza eklemek için iki adımı gerekir: ilk olarak, görüntüleri; projenize ekleyin Ardından, denetimleri ve bunları bir ekranda görüntülemek için kod ekleyin. Başvurmak [görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) kapsamı Xamarin.ios'ta işleme resminin ayrıntılı için makalesi.

## <a name="adding-images-to-your-app"></a>Görüntüleri uygulamanıza ekleme

Görüntüleri, çözüm Mac için Visual Studio içinde herhangi bir klasöre eklenebilir ve **derleme eylemi** ayarlanır **içerik** dosya uygulamanızla dahil edilir ve görüntülenebilir.

Mac için Visual Studio ayrıca adlı özel bir dizin destekler **kaynakları** , içerebilir görüntü dosyaları. Dosyalarını Resources klasöründeki olmalıdır **derleme eylemi** kümesine **BundleResource**.

Bu ekran görüntüsünde gösterilmiştir **derleme eylemi** bir dosya açıldığında görüntülenen seçenekleri görüntülemez:

 [![](image-images/image30a.png "Eylem menüsü oluşturma")](image-images/image30a.png#lightbox)

Mac için Visual Studio genellikle seçim doğru **derleme eylemi** otomatik olarak ancak özellikle, dosyaları projenize yerleri değiştirilirse, bu ayarları bilmeniz gerekir.

### <a name="adding-an-image-file"></a>Bir görüntü dosyası ekleme

Projenize bir görüntü dosyası eklemek için öncelikle projeye sağ tıklayıp seçin **Dosya Ekle...**

 [![](image-images/image31a.png "Dosya Ekle... menüsü")](image-images/image31a.png#lightbox)

Görüntü (veya görüntü) seçin, standart dosya iletişim kutusunda eklemek istediğiniz. Görüntüleri için varsayılan derleme eylemi **BundleResource** – belirli bir neden olmadığı sürece bu değeri geçersiz.

 [![](image-images/image32a.png "Dosya iletişim kutusu Ekle")](image-images/image32a.png#lightbox)

Görüntü, projenize ve yüklenmesi ve kodda görüntülenen kullanımına eklenecektir. Bu ekran görüntüsünde, bir iOS uygulaması projesine eklenen bir resim göstermektedir:

 [![](image-images/image33a.png "Projedeki görüntü")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Kaynak dizini nedir?

Dosyalar yerleştirilir **kaynakları** dizin farklı normal dosyalarından – içeriğini kabul **kaynakları** klasörü uygulamanın kök dizinine kopyalanır ve buradan içinde başvurulabilir kodunuzu. Bu nedenlerle yararlı olabilir:

-  Uygulama simgeleri ve varsayılan başlangıç görüntülerini gibi uygulamanın özellikleri yapılandırılmış görüntüleri depolama.
-  Diğer görüntüleri ve ayrı ayrı kod dosyaların depolanması, bu nedenle bunlar daha kolay (alt dizin kaynakları dizin içeriğini kopyalandığında korunur).


**Kaynakları** kodu, bu görüntüleri gerektiren yazma paylaşılan kod kitaplıkları kolaylaştıran, kullanıcı uygulama kökü içine kopyalanacak kabul edilebilir olduğundan dizin kitaplığı projesinde, özellikle kullanışlı Görüntü, ses, video, XML veya diğer dosyaları.

**Kaynakları** dizin gerekir böylece adı ve tüm dosyaları derleme eylemi kümesine sahip olmalıdır **BundleResource**.

## <a name="displaying-the-image"></a>Görüntüyü görüntüleme

İOS Tasarımcısı kullanma bir **resim görünümü** bir resim veya animasyonlu dizi görüntüsü görüntülenecek. **Resim görünümü** araç kutusundan simge aşağıda gösterilmektedir:

 [![](image-images/image35a.png "Araç çubuğunda ImageView")](image-images/image35.png#lightbox)

Sürükleme **resim görünümü** gelen **araç kutusu** görünüm denetleyicisine. Ardından, altında **görüntü Görünüm > Görüntü** açılır listede, projenizdeki tüm kullanılabilir görüntü dosyaları listesini sağlar. Bu, görüntü görünümüne eklemek için seçin.

 [![](image-images/image36a.png "Araç çubuğunda ImageView")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>Program aracılığıyla resim görüntüleme

Çünkü **SF Monkey.jpg** kök dizininde bulunan **kaynakları** kullanılabilir olacağı çalışma zamanında uygulama paketin kök dizini. Bu görüntüyü bir resim görünümü denetiminde görüntülemek için aşağıdaki kodu kullanın:

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

Biz görüntüde yerleştirdiyseniz **PICS/kaynak/SF Monkey.jpg**, kod eklemeniz **PICS** yolundaki klasör:

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

Kaynak dosyasına başvuran hiçbir zaman eklemenize gerek **kaynakları** klasör.

## <a name="related-links"></a>İlgili bağlantılar

- [Denetimler (örnek)](https://developer.xamarin.com/samples/Controls/)
