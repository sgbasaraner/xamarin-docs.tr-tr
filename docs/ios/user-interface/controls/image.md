---
title: "Görüntüleri görüntüleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 716189fbf1518e9100a78cc5ae64e9e63a24c949
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="displaying-images"></a>Görüntüleri görüntüleme

Uygulamanıza görüntüler ekleme, iki adımı gerektirir: önce görüntüler; projenize ekleyin. Ardından, denetimleri ve ekranda görüntülemek için kodu ekleyin. Başvurmak [görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) kapsamı içinde Xamarin.iOS işleme resminin ayrıntılı için makalesi.

## <a name="adding-images-to-your-app"></a>Uygulamanıza görüntüler ekleme

Herhangi bir klasörde Mac çözüm için Visual Studio için görüntülerini eklenebilir ve **yapı eylemi** ayarlanır **içerik** dosya, uygulamanızı dahil edilir ve görüntülenebilir.

Mac için Visual Studio görüntü dosyaları de içerebilir kaynakları adlı özel bir dizin de destekler. Kaynakları klasöründeki dosyaları olmalıdır **yapı eylemi** kümesine **BundleResource**.

Bu ekran gösterir **yapı eylemi** bir dosya açıldığında görüntülenen seçenekleri sağ:

 [![](image-images/image30a.png "Eylem menüsü oluşturma")](image-images/image30a.png#lightbox)

Mac için Visual Studio genellikle seçin doğru **yapı eylemi** otomatik olarak, ancak özellikle dosyalarını projenizde taşırsanız, bu ayarlar dikkat etmeniz gerekir.

### <a name="adding-an-image-file"></a>Bir görüntü dosyası ekleme

Bir görüntü dosyası projenize eklemek için önce projeyi sağ tıklayın ve seçin **dosyaları Ekle...**

 [![](image-images/image31a.png "Dosyaları Ekle... menüsü")](image-images/image31a.png#lightbox)

Görüntü (veya görüntü) seçin standart dosya iletişim kutusunda eklemek istediğiniz. Varsayılan yapı eylemi görüntüler için **BundleResource** – belirli bir nedeniniz yoksa bu değeri geçersiz kılmaz.

 [![](image-images/image32a.png "Dosyaları iletişim ekleyin")](image-images/image32a.png#lightbox)

Görüntü projenize ve yüklenen ve kodda görüntülenecek kullanılabilir eklenir. Bu ekran görüntüsü, bir iOS uygulaması projesine eklenen bir resim gösterir:

 [![](image-images/image33a.png "Proje görüntüde")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Kaynak dizini nedir?

Kaynakları dizine yerleştirilen dosyalar farklı normal dosyalarından davranılır – kaynakları klasörünün içeriğini uygulama kök dizinine kopyalanır ve kodunuzda buradan başvurulabilir. Bu nedenlerle yararlı olabilir:

-  Uygulama simgeleri ve varsayılan başlangıç görüntüleri gibi uygulamanın özelliklerinde yapılandırılan görüntüleri saklamak.
-  Diğer görüntüleri ve kod dosyalarından ayrı olarak depolanması, bu nedenle bunlar daha kolay (alt dizinleri kaynakları dizin içeriği kopyalandığında korunur).


Kod bu görüntüleri gerektiren görüntü, ses, video, XML veya diğer paylaşılan kodu kitaplıkları yazma kolaylaştırma kullanıcı uygulama kök kopyalanacak kabul edilebilir olduğundan kaynaklar dizin kitaplığı projesinde, özellikle yararlı olur dosyalar.



Bu nedenle kaynaklar dizin adlı ve tüm dosyalar kümesine yapı eylemi sahip olmalıdır **BundleResource**

## <a name="displaying-the-image"></a>Görüntü görüntüleme

Tasarımcı kullanarak bir görüntü görüntülemek için bir resim görünümü bir kapsayıcı olarak kullanılmalıdır ve tek bir görüntü veya animasyonun görüntülerinin görüntüleyebilirsiniz. **Resim görünümü** araç simgesinden aşağıda gösterilmektedir:

 [![](image-images/image35a.png "Araç çubuğundaki ImageView")](image-images/image35.png#lightbox)

Sürükleme **görüntü Görünüm** gelen **Toobox** görünüm denetleyicisine. Altında ** görüntü Görünüm > Görüntü ** aşağı açılan listeden projenizdeki tüm kullanılabilir görüntü dosyaların bir listesini sağlar. Bu görüntü görünümünüze eklemek için seçin.

 [![](image-images/image36a.png "Araç çubuğundaki ImageView")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>Görüntü programlı olarak görüntüleme

Blocks.jpg kaynakları dizin kökünde bulunan olduğundan çalışma zamanında uygulama paketin kök kullanılabilir olacaktır. Denetim bir ImageView bu görüntüyü görüntülemek için aşağıdaki kodu kullanın:

```csharp
imageview1.Image = UIImage.FromBundle ("SF Monkey.png");
```

Görüntüde yerleştirilmiş durumunda `/Resources/Pics/blocks.jpg` sonra kod yolunda PICS klasör içerir:

```csharp
imageview1.Image = UIImage.FromBundle ("Pics/SF Monkey.png");
```

Kaynak dosyası başvuran hiçbir zaman eklemenize gerek `Resources` klasör.


## <a name="related-links"></a>İlgili bağlantılar

- [Denetimleri (örnek)](https://developer.xamarin.com/samples/Controls/)
