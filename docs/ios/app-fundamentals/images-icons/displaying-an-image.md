---
title: Bir görüntüyü Xamarin.ios'ta görüntüleme
description: Bu makale, bir görüntü varlığı bir Xamarin.iOS uygulaması ve C# kodu kullanarak veya bir denetime iOS Designer'daki atayarak o yansıma görüntüleme dahil olmak üzere kapsar.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/24/2018
ms.openlocfilehash: 4b2bddeb6b04b5c5288f501fce0d6bb03e0b6584
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780627"
---
# <a name="displaying-an-image-in-xamarinios"></a>Bir görüntüyü Xamarin.ios'ta görüntüleme

_Bu makale, bir görüntü varlığı bir Xamarin.iOS uygulaması ve C# kodu kullanarak veya bir denetime iOS Designer'daki atayarak o yansıma görüntüleme dahil olmak üzere kapsar._

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Ekleme ve bir Xamarin.iOS uygulaması görüntüleri düzenleme

Geliştirici bir görüntü kullanmak için bir Xamarin.iOS uygulaması eklerken kullanacağı bir _varlık Kataloğu_ her iOS cihazını ve bir uygulama tarafından gereken çözüm desteklemek için.

İOS 7, eklenen **varlık katalogları görüntü kümeleri** tüm sürümleri veya bir uygulama için ölçek ve çeşitli cihazları desteklemek için gerekli olan bir görüntü temsillerini içerir. Görüntü varlıkları dosya kalmak yerine (bkz [çözümleme bağımsız görüntüleri ve görüntü terminolojisi](~/ios/app-fundamentals/images-icons/displaying-an-image.md)), **görüntü kümesi** hangi görüntü hangi cihaz ve/veya çözümleme ait belirtmek için bir Json dosyası kullanma . Bu, yönetmek ve iOS (iOS 9 veya üzeri) başlangıç görüntülerini desteklemek için tercih edilen yoludur.

## <a name="adding-images-to-an-asset-catalog-image-set"></a>Varlık Kataloğu görüntüye görüntüleri ekleme ayarlayın

Yukarıda belirtildiği gibi bir **varlık katalogları görüntü kümeleri** tüm sürümleri veya bir uygulama için ölçek ve çeşitli cihazları desteklemek için gerekli olan bir görüntü temsillerini içerir. Görüntü varlıkları dosya kalmak yerine **görüntü kümeleri** hangi görüntü hangi cihaz ve/veya çözümleme ait belirtmek için bir Json dosyası kullanın.

Yeni bir görüntü oluşturmak ve görüntüleri eklemek için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. İçinde **Çözüm Gezgini**, çift `Assets.xcassets` dosyayı düzenlemek için açın:

    ![](displaying-an-image-images/imageset01.png "Çözüm Gezgini'nde Assets.xcassets")
2. Sağ **Assets listesini** seçip **yeni görüntü kümesi**:

    ![](displaying-an-image-images/imageset02.png "Yeni bir görüntü kümesi ekleme")
3. Yeni görüntü kümesi seçin ve düzenleyici görüntülenir:

    ![](displaying-an-image-images/imageset03.png "Görüntü kümesi Düzenleyicisi")
4. Buradan, görüntüleri her biri farklı cihazlar için sürükleyin ve ve gereken çözünürlükleri. 
5. Yeni görüntü kümenin çift **adı** içinde **Assets listesini** düzenlemek için: ![](displaying-an-image-images/imageset04.png "yeni görüntü kümesinin adı düzenleme")

Kullanırken bir **resmi ayarlama** iOS Designer yalnızca aşağı açılan listeden özellik Düzenleyicisi'nde kümenin adını seçin:

![](displaying-an-image-images/imageset06.png "Açılır listeden bir görüntü kümenin adını seçin")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Varlık Kataloğu'ndan açmak **Çözüm Gezgini**, sol üst köşede tıklatıp **artı** düğmesi:

    ![](displaying-an-image-images/asset5.png "Artı düğmesi")

2. Seçin **resim kümesi Ekle** ve görüntü kümesi Düzenleyicisi için yeni görüntü kümesi görüntülenir. Buradan, görüntüleri her biri farklı cihazlar için sürükleyin ve ve gereken çözünürlükleri. 

    ![](displaying-an-image-images/asset7.png "Görüntü kümesi Düzenleyicisi")

### <a name="renaming-an-image-set"></a>Bir görüntü kümesi yeniden adlandırma

Bir görüntü kümesi yeniden adlandırmak için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift **varlık Kataloğu** dosyayı düzenlemek için açın:

    ![](displaying-an-image-images/rename01.png "Çözüm Gezgini'nde varlık Kataloğu")
2. Seçin **resmi ayarlama** yeniden adlandırmak için:

    ![](displaying-an-image-images/rename02.png "Yeniden adlandırmak için görüntü kümesi seçin")
3. İçinde **özellikleri Gezgini**, alt kısma kaydırın ve **adı**(altında **çeşitli** bölümü):

    ![](displaying-an-image-images/rename03.png "Çeşitli bölümü altında adını seçin")
4. Yeni bir girin **adı** için **resmi ayarlama** ve değişiklikleri kaydedin.

-----

Kullanırken bir **resmi ayarlama** kodda adıyla çağırarak başvurmanız `FromBundle` yöntemi `UIImage` sınıfı. Örneğin:

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> Bir görüntü kümesine atanan görüntü doğru görünmüyorsa, doğru dosya adı ile kullanıldığından emin olun `FromBundle` yöntemi ( **resmi ayarlama** ve üst **varlık Kataloğu** adı). PNG görüntülerini `.png` uzantısı atlanabilir. Diğer resim biçimleri için uzantısı (örn. gereklidir `PurpleMonkey.jpg`).

### <a name="using-vector-images-in-asset-catalogs"></a>Varlık katalogları vektör görüntüleri kullanma

İOS 8, özel'den itibaren **vektör** sınıfı eklenmiş olarak **görüntü kümeleri** içerecek şekilde Geliştirici izin veren bir **PDF** vektör görüntüsü yerine dahil olmak üzere kasette biçimlendirilmiş tek bir bit eşlem dosyaları farklı çözünürlükte. Bu yöntemi kullanarak sağlamak için bir tek vektör dosyası `@1x` (vektör PDF dosyası olarak biçimlendirilmiş) çözüm ve `@2x` ve `@3x` derleme zamanında oluşturulan ve uygulamanın pakete eklenen dosyanın sürümü.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "Varlık katalogları düzenleyicisinde vektör görüntüleri")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "Varlık katalogları düzenleyicisinde vektör görüntüleri")

-----

Geliştirici içeriyorsa, örneğin, bir `MonkeyIcon.pdf` dosyası bir varlık Kataloğu vektörü 150px x 150px, aşağıdaki bit eşlem varlıklar eklenmesi son uygulama paketi grubuna, derlendiğinde, bir çözüm olarak:

- `MonkeyIcon@1x.png` -150px x 150px çözümleme.
- `MonkeyIcon@2x.png` -300 piksel x 300px çözümleme.
- `MonkeyIcon@3x.png` -450px x 450px çözümleme.

Aşağıdakiler dikkate PDF vektör görüntüleri varlık katalogları kullanırken dikkat edilmelidir:

- PDF taranmış bir bit eşlemi derleme zaman ve son uygulamada sevk bit eşlemler olarak değil tam vektör destek budur.
- Resmin boyutu varlık Kataloğu'nda ayarlandıktan sonra değiştirilemez. Görüntü (veya kodda Otomatik Yerleşim ve boyut sınıflarını kullanarak) yeniden boyutlandırmak Geliştirici çalışırsa resmin diğer bitmap gibi bozuk.
- Varlık katalogları yalnızca bir uygulamaya ihtiyacınız iOS 6 desteklemek veya düşük varlık katalogları kullanamazsınız uyumlu iOS 7 ve üzeri, vardır.

## <a name="working-with-template-images"></a>Şablon görüntüleri ile çalışma

Bir iOS uygulamasının tasarımına bağlı, geliştirici simge veya resim (örneğin, kullanıcı tercihleri temelinde) renk düzeninde bir değişiklik eşleştirmek için kullanıcı arabirimi içinde özelleştirme gerektiğinde zamanlar olabilir.

Kolayca Bu etkiyi elde etmek için geçiş _işleme modu_ için görüntü varlığın **şablon görüntüsü**:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "Şablon görüntüsü işleme modunu ayarlama")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "Şablon için işleme modunu ayarlama")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

İOS Tasarımcısı bir UI denetimine görüntü varlık atayın ve ardından ayarlayın **renk tonu** görüntü renklendirmek için:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "Bir görüntüyü renklendirmek için renk tonu ayarlayın")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "Bir görüntüyü renklendirmek için renk tonu ayarlayın")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

İsteğe bağlı olarak, görüntü varlık ve renk tonu doğrudan kod içinde ayarlanabilir:

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

Bir şablon görüntüsü tamamen kodundan kullanmak için aşağıdakileri yapın:

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

Çünkü `RenderMode` özelliği bir `UIImage` salt okunur, kullanın `ImageWithRenderingMode` İstenen işleme modunu ayarla görüntünün yeni bir örneğini oluşturmak için yöntemi.

Üç büyük olasılıkla ayarlarını `UIImage.RenderMode` aracılığıyla `UIImageRenderingMode` sabit listesi:

* `AlwaysOriginal` -Herhangi bir değişiklik yapmadan özgün kaynak görüntü dosyası olarak işlenecek resim zorlar.
* `AlwaysTemplate` -Belirtilen piksel renklendirme ile bir şablon görüntüsü işlenecek resim zorlar `Tint` rengi.
* `Automatic` -Ya da işler görüntüsünü kullanılır bu ortamda bir şablon veya özgün dayalı olarak. Örneğin görüntüsü kullanılırsa, bir `UIToolBar`, `UINavigationBar`, `UITabBar` veya `UISegmentControl` şablon olarak kabul edilir.

## <a name="adding-new-assets-collections"></a>Yeni varlıklar koleksiyonu ekleme

Varlık katalogları görüntüleri ile çalışırken zaman yeni bir koleksiyon olması, tüm uygulamanın görüntüleri ekleme yerine, gerekli zamanlar olabilir `Assets.xcassets` koleksiyonu. Örneğin, isteğe bağlı kaynakları tasarlarken.

Yeni bir varlık Kataloğu projeye eklemek için:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Sağ **proje adı** içinde **Çözüm Gezgini** seçip **Ekle** > **yeni dosya...**
2. Seçin **iOS** > **varlık Kataloğu**, girin bir **adı** tıklatın ve koleksiyon için **yeni** düğmesi:

    ![](displaying-an-image-images/asset01.png "Yeni bir varlık Kataloğu oluşturma")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çözüm Gezgini'nde sağ **varlık katalogları** klasörü ve select **Ekle > Yeni varlık Kataloğu**.
2. Bir ad verin ve tıklayın **Ekle**:

    ![](displaying-an-image-images/asset1.png "Yeni bir varlık Kataloğu oluşturma")

-----

Burada, koleksiyon ile aynı şekilde varsayılan olarak çalışılabilmesi `Assets.xcassets` otomatik olarak projeye dahil koleksiyonu.

## <a name="using-images-with-controls"></a>Görüntü denetimleri ile kullanma

Bir uygulamayı desteklemek için görüntüleri kullanmanın yanı sıra iOS sekme çubukları, araç çubukları, gezinti çubukları, tablolar ve düğmeler gibi uygulama denetimi türleri ile görüntüleri de kullanır. Bir denetimde görünen bir görüntü oluşturmak için basit bir yol atamaktır bir `UIImage` denetimin örneğine `Image` özelliği.

### <a name="frombundle"></a>FromBundle

`FromBundle` Yöntem çağrısı, çok sayıda görüntü yükleme ve yönetim özelliği desteği ve görüntü dosyaları çeşitli çözünürlükleri için otomatik işlenmesini önbelleğe alma gibi yerleşik olan zaman uyumlu (engelleme) çağrısı ise.  

Aşağıdaki örnek görüntüsü ayarlama işlemini gösterir. bir `UITabBarItem` üzerinde bir `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Varsayarak `MyImage` bir varlık Kataloğu yukarıdaki eklenmiş bir resim varlığı adıdır. Varlık Kataloğu görüntüleri çalışırken, yalnızca görüntü kümesi'nde adını `FromBundle` yöntemi **PNG** biçimlendirilmiş görüntüleri:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Diğer resim biçimi için adın uzantıyla içerir. Örneğin:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

Simgelerle ve görüntülerle hakkında daha fazla bilgi için Apple belgelerine bakın [özel simgesini ve görüntü oluşturma yönergeleri](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html).

## <a name="displaying-an-image-in-a-storyboards"></a>Bir film şeritleri bir görüntüyü görüntüleme

Bir görüntü kullanarak bir varlık katalogları bir Xamarin.iOS projesi eklendikten sonra kolayca olabilir bir görsel taslak kullanarak görüntülenen bir `UIImageView` iOS Designer'daki. Örneğin, aşağıdaki resim varlığı eklediyseniz:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](displaying-an-image-images/display01.png "Bir örnek görüntü varlığı eklendi.")

Bir film şeridinde görüntülemek için aşağıdakileri yapın:

1. Çift `Main.storyboard` dosyası **Çözüm Gezgini** iOS Designer'daki düzenleme için açın.
2. Seçin bir **görüntü Görünüm** gelen **araç kutusu**:

     ![](displaying-an-image-images/display02.png "Araç kutusundan bir görüntü görünüm seçin")
3. Görüntü görünüm konumu ve tasarım yüzeyi sürükleyin ve gerektiği şekilde boyutu:

    ![](displaying-an-image-images/display03.png "Tasarım yüzeyinde yeni bir resim görünümü")
4. İçinde **pencere öğesi** bölümünü **özellik Gezgini** istenen seçin **görüntü** görüntülenecek varlık:

    ![](displaying-an-image-images/display04.png "Görüntülenecek istenen görüntü varlığı seçin")
5. İçinde **görünümü** bölümündeki **modu** nasıl görüntü denetlemenizi ne zaman yeniden boyutlandırılmış **resim görünümü** yeniden boyutlandırılır.
6. İle **resim görünümü** seçili yeniden eklemek için tıklatın **kısıtlamaları**:

    ![](displaying-an-image-images/display05.png "Kısıtlamaları ekleme")
7. Her kenarında "T" şeklinde tutamacı sürükleyin **resim görünümü** karşılık gelen ekranın "yanlara resim sabitlemek için" için. Bu şekilde **resim görünümü** daralabilmesi ve ekranı yeniden boyutlandırıldığından büyütün.
8. Görsel taslak için değişiklikleri kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "Bir örnek görüntü varlığı eklendi.")

Bir film şeridinde görüntülemek için aşağıdakileri yapın:

1. Çift `Main.storyboard` dosyası **Çözüm Gezgini** iOS Designer'daki düzenleme için açın.
2. Seçin bir **görüntü Görünüm** gelen **araç kutusu**:

     ![](displaying-an-image-images/display02vs.png "Araç kutusundan bir görüntü görünüm seçin")
3. Görüntü görünüm konumu ve tasarım yüzeyi sürükleyin ve gerektiği şekilde boyutu:

    ![](displaying-an-image-images/display03vs.png "Tasarım yüzeyinde yeni bir resim görünümü")
4. İçinde **pencere öğesi** bölümünü **özellik Gezgini** istenen seçin **görüntü** görüntülenecek varlık:

    ![](displaying-an-image-images/display04vs.png "Görüntülenecek istenen görüntü varlığı seçin")
5. İçinde **görünümü** bölümündeki **modu** nasıl görüntü denetlemenizi ne zaman yeniden boyutlandırılmış **resim görünümü** yeniden boyutlandırılır.
6. İle **resim görünümü** seçili yeniden eklemek için tıklatın **kısıtlamaları**:

    ![](displaying-an-image-images/display05vs.png "Kısıtlamaları ekleme")
7. Her kenarında "T" şeklinde tutamacı sürükleyin **resim görünümü** karşılık gelen ekranın "yanlara resim sabitlemek için" için. Bu şekilde **resim görünümü** daralabilmesi ve ekranı yeniden boyutlandırıldığından büyütün.
8. Görsel taslak için değişiklikleri kaydedin.

-----

## <a name="displaying-an-image-in-code"></a>Kodda bir görüntüyü görüntüleme

Bir görüntü kullanarak bir varlık katalogları bir Xamarin.iOS projesi eklendiğinde bir görüntü içinde bir film şeridini yalnızca görüntüleme gibi bunu kolayca C# kodunu kullanarak görüntülenebilir.

Aşağıdaki örnek uygulayın:

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

Bu kod yeni bir oluşturur `UIImageView` ve bir başlangıç boyutunu ve konumunu sağlar. Projeye eklenen bir resim varlığı görüntüyü yükler ve ekler `UIImageView` üst `UIView` görüntülenecek.

## <a name="related-links"></a>İlgili bağlantılar

- [(Örnek) görüntülerle çalışma](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Merhaba, iPhone](~/ios/get-started/hello-ios/index.md)
- [Görüntü boyutu ve çözünürlük (Apple)](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
