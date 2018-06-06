---
title: Bir görüntü Xamarin.iOS içinde görüntüleme
description: Bu makalede, bir Xamarin.iOS uygulaması ve C# kodu kullanarak veya iOS Tasarımcısı denetiminde atama, görüntü görüntüleme bir görüntü varlığı dahil olmak üzere yer almaktadır.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/24/2018
ms.openlocfilehash: 3ae63bb30c7759a1915939a2199d5ffc7dc75d15
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784279"
---
# <a name="displaying-an-image-in-xamarinios"></a>Bir görüntü Xamarin.iOS içinde görüntüleme

_Bu makalede, bir Xamarin.iOS uygulaması ve C# kodu kullanarak veya iOS Tasarımcısı denetiminde atama, görüntü görüntüleme bir görüntü varlığı dahil olmak üzere yer almaktadır._

Bu makalede ayrıntılı aşağıdaki konuları içerir:

- [Ekleme ve düzenleme görüntüleri bir Xamarin.iOS uygulaması](#adding-assets) - kapsayan görüntü varlıklarının ve dahil edilebilir nasıl düzenlenir ve Xamarin.iOS projesi içinde yönetilir.
- [Varlık katalog görüntüleri ekleme](#asset-catalogs) -varlık kataloglar görüntülerle yönetme.
    - [Varlık kataloglarında vektör görüntüleri kullanarak](#Using-Vector-Images-in-Asset-Catalogs) -tüm görüntü boyutları ile tek bir vektör sağlama.
- [Şablon görüntülerle çalışma](#Working-with-Template-Images) -şablon görüntüsü gibi bir görüntü varlığı ayarlayarak, geliştirici kolayca görüntünün ayarlayarak bir uygulamanın UI Tema değişiklikleri eşleşecek şekilde renklendirme `Tint` özelliği.
- [Denetimler ile görüntüleri kullanma](#controls) - gibi UI denetimleri ile bir Xamarin.iOS projesi dahil görüntü varlıklarının kullanarak kapsar `UIButton` ve `UIImageView` ve C# kullanarak görüntülerle çalışma konusunda `UIImage` nesnenin [FromBundle](#frombundle) yöntemi.
- [Film şeritleri içinde bir görüntü görüntüleme](#Displaying-an-Image-in-a-Storyboards) -film şeridi kullanarak görüntü görüntüleme ilişkin bir örnek verilmektedir.
- [Kodda bir görüntü görüntüleme](#Displaying-an-Image-in-Code) -C# kod kullanarak bir görüntü görüntüleme ilişkin bir örnek verilmektedir.

<a name="adding-assets" />

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Ekleme ve bir Xamarin.iOS uygulaması görüntüleri düzenleme

Bir Xamarin.iOS uygulaması kullanmak için görüntü ekleme, geliştirici için kullanacağı bir _varlık Kataloğu_ her iOS cihaz ve uygulama tarafından gerekli çözümleme desteklemek için.

İOS 7, eklenen **varlık kataloglar görüntü kümeleri** tüm sürümleri veya bir uygulama için Etkenler ölçekleme ve çeşitli aygıtları desteklemek gerekli olan bir görüntü gösterimlerini içerir. Görüntü varlıklar dosya kalmak yerine (bkz [çözümleme bağımsız görüntüler ve görüntü terminolojisi](~/ios/app-fundamentals/images-icons/displaying-an-image.md)), **görüntü kümeleri** hangi görüntü hangi cihaz ve/veya çözümleme ait belirtmek için bir Json dosyası kullanın . Bu, yönetmek ve görüntüleri iOS (Başlangıç iOS 9 veya üzeri) desteklemek için tercih edilen yoludur.

<a name="asset-catalogs" />

## <a name="adding-images-to-an-asset-catalog-image-set"></a>Bir varlık Kataloğu görüntüsüne ekleme görüntüleri ayarlama

Yukarıda belirtildiği gibi bir **varlık kataloglar görüntü kümeleri** tüm sürümleri veya bir uygulama için Etkenler ölçekleme ve çeşitli aygıtları desteklemek gerekli olan bir görüntü gösterimlerini içerir. Görüntü varlıklar dosya kalmak yerine **görüntü kümeleri** hangi görüntü hangi cihaz ve/veya çözümleme ait belirtmek için bir Json dosyası kullanın.

Yeni bir görüntü kümesi oluşturmak ve görüntüleri eklemek için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. İçinde **Çözüm Gezgini**, çift `Assets.xcassets` dosyayı düzenlemek için açın:

    ![](displaying-an-image-images/imageset01.png "Çözüm Gezgini'nde Assets.xcassets")
2. Sağ **varlıklar listesi** seçip **yeni görüntü kümesi**:

    ![](displaying-an-image-images/imageset02.png "Yeni bir görüntü kümesi ekleme")
3. Yeni bir görüntü kümesi seçin ve düzenleyici görüntülenir:

    ![](displaying-an-image-images/imageset03.png "Görüntü küme Düzenleyicisi")
4. Buradan, görüntüleri her farklı aygıtlar için sürükleyin ve ve gerekli çözümler. (Not: Bu çözümler belirtilen çözünürlük eşleşen [görüntü boyutları ve dosya adları](~/ios/app-fundamentals/images-icons/displaying-an-image.md) belge.)
5. Yeni görüntü kümenin çift **adı** içinde **varlıklar listesi** düzenlemek için: ![ ] (displaying-an-image-images/imageset04.png "yeni görüntü kümenin adını düzenleme")

Kullanırken bir **resmi ayarlama** iOS Tasarımcısı ', kümenin adını aşağı açılan listeden özelliği Düzenleyicisi'nde seçmeniz yeterlidir:

![](displaying-an-image-images/imageset06.png "Açılır listeden bir görüntü kümenin adını seçin")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Varlık Kataloğu'ndan açmak **Çözüm Gezgini**, sol üst köşede tıklatıp **artı** düğmesi:

    ![](displaying-an-image-images/asset5.png "Artı düğmesi")

2. Seçin **görüntü kümesi Ekle** ve resmi ayarlama Düzenleyicisi'ni yeni bir görüntü kümesi için görüntülenir. Buradan, görüntüleri her farklı aygıtlar için sürükleyin ve ve gerekli çözümler. (Not: Bu çözümler belirtilen çözünürlük eşleşen [görüntü boyutları ve dosya adları](~/ios/app-fundamentals/images-icons/displaying-an-image.md) belge):

    ![](displaying-an-image-images/asset7.png "Görüntü Düzenleyicisi'ni ayarlayın")

### <a name="renaming-an-image-set"></a>Bir görüntü kümesi yeniden adlandırma

Bir resmi ayarlama yeniden adlandırmak için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift **varlık Kataloğu** dosyayı düzenlemek için açın:

    ![](displaying-an-image-images/rename01.png "Çözüm Gezgini'nde varlık Kataloğu")
2. Seçin **resmi ayarlama** yeniden adlandırmak için:

    ![](displaying-an-image-images/rename02.png "Yeniden adlandırmak için görüntü kümesi seçin")
3. İçinde **özellikleri Explorer**, alt kısmına kaydırın ve seçin **adı**(altında **çeşitli** bölümü):

    ![](displaying-an-image-images/rename03.png "Çeşitli bölümü altında adı seçin")
4. Yeni bir girin **adı** için **resmi ayarlama** ve değişiklikleri kaydedin.

-----

Kullanırken bir **resmi ayarlama** kodda, ada göre arayarak başvuru `FromBundle` yöntemi `UIImage` sınıfı. Örneğin:

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> Bir görüntü kümesine atanan görüntüleri doğru görünmüyorsa, doğru dosya adı ile kullanıldığından emin olun `FromBundle` yöntemi ( **resmi ayarlama** ve üst **varlık Kataloğu** adı). PNG görüntüleri için `.png` uzantısı etmeyebilirsiniz. Diğer resim biçimleri için uzantısı (ör. gereklidir `PurpleMonkey.jpg`).

<a name="Using-Vector-Images-in-Asset-Catalogs" />

### <a name="using-vector-images-in-asset-catalogs"></a>Varlık kataloglarında vektör görüntüleri kullanma

İtibariyle iOS 8, özel **vektör** sınıfı için eklenene gibi **görüntü kümeleri** dahil etmek Geliştirici sağlayan bir **PDF** bunun yerine birlikte Kaset vektör görüntüde biçimlendirilmiş tek tek bit eşlem dosyaları farklı çözünürlüklerde. Bu yöntemi kullanarak sağlamak için bir tek vektör dosya `@1x` (vektör PDF dosyası olarak biçimlendirilmiş) çözümleme ve `@2x` ve `@3x` dosya sürümleri derleme zamanında oluşturulan ve uygulamanın pakete eklenen.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "Varlık kataloglar düzenleyicisinde vektör görüntüleri")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "Varlık kataloglar düzenleyicisinde vektör görüntüleri")

-----

Geliştirici içeriyorsa, örneğin, bir `MonkeyIcon.pdf` dosyası olarak bir varlık katalog vektör 150px x 150px, varlıklar dahil edilebilir son uygulama paketine derlenmesinden olduğunda aşağıdaki bit eşlem'i, çözünürlük:

- `MonkeyIcon@1x.png` -150px x 150px çözümleme.
- `MonkeyIcon@2x.png` -300px x 300px çözümleme.
- `MonkeyIcon@3x.png` -450px x 450px çözümleme.

Aşağıdakileri göz önünde PDF vektör görüntüleri varlık kataloglarında kullanırken yapılması gerekir:

- PDF bir bit eşlem için derleme zamanı ve son uygulamada sevk bit eşlemler rasterleştirilecek gibi bu tam vektör destek değil.
- Varlık kataloğunda ayarladıktan sonra görüntünün boyutu ayarlanamaz. Geliştirici görüntü (veya kod otomatik düzeni ve boyutu sınıflarını kullanarak) yeniden boyutlandırmak çalışırsa resmin diğer bitmap gibi bozuk.
- Varlık kataloglar yalnızca bir uygulamanın iOS 6 desteklemek için ihtiyacınız veya düşük varlık kataloglar kullanamazsınız büyük ve iOS 7 ile uyumlu olur.

<a name="Working-with-Template-Images" />

## <a name="working-with-template-images"></a>Şablon görüntüleri ile çalışma

Bir iOS uygulaması tasarımını bağlı olarak, simge veya görüntü renk düzenini (örneğin, kullanıcı tercihlerine göre) bir değişiklik eşleştirmek için kullanıcı arabirimi içinde özelleştirmek için geliştirici gerektiği zaman zamanlar olabilir.

Bu etkiyi kolayca elde etmek için geçiş _işleme modunu_ için görüntü varlığın **şablon görüntüsü**:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "Şablon görüntüsü için işleme modunu ayarlama")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "Şablon için işleme modunu ayarlama")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

İOS Tasarımcısı, gelen bir kullanıcı Arabirimi denetim görüntü varlığı atayın ve sonra ayarlamak **TINT** görüntü renklendirme için:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "TINT görüntü renklendirme ayarlayın")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "TINT görüntü renklendirme ayarlayın")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

İsteğe bağlı olarak, görüntü varlığı ve ton doğrudan kod içinde ayarlanabilir:

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

Şablon görüntüsü tamamen kodundan kullanmak için aşağıdakileri yapın:

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

Çünkü `RenderMode` özelliği bir `UIImage` salt okunur ise kullanmak `ImageWithRenderingMode` İstenen işleme modunu ayarla görüntüyü yeni bir örneğini oluşturmak için yöntemi.

Üç büyük olasılıkla ayarlarını `UIImage.RenderMode` aracılığıyla `UIImageRenderingMode` enum:

* `AlwaysOriginal` -Herhangi bir değişiklik yapılmadan özgün kaynak görüntü dosyasını olarak işlenecek resim zorlar.
* `AlwaysTemplate` -Belirtilen piksel renklendirme tarafından bir şablon görüntüsü olarak işlenecek resim zorlar `Tint` rengi.
* `Automatic` -Ya da işler görüntüsünü bir şablonu veya özgün olarak kullanılan ortam dayalı olarak. Örneğin, görüntünün kullanılıyorsa bir `UIToolBar`, `UINavigationBar`, `UITabBar` veya `UISegmentControl` şablon olarak kabul edilir.

<a name="Adding-new-Assets-Collections" />

## <a name="adding-new-assets-collections"></a>Yeni varlıklar koleksiyonu ekleme

Varlıklar kataloglar görüntülerle çalışırken zaman yeni bir koleksiyon olacaktır, tüm uygulamanın görüntüleri eklemek yerine, gerekli zamanlar olabilir `Assets.xcassets` koleksiyonu. Örneğin, isteğe bağlı kaynakları tasarlarken.

Yeni bir varlık Kataloğu projeye eklemek için:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Sağ **proje adı** içinde **Çözüm Gezgini** seçip **Ekle** > **yeni dosya...**
2. Seçin **iOS** > **varlık Kataloğu**, girin bir **adı** tıklatın ve koleksiyon için **yeni** düğmesi:

    ![](displaying-an-image-images/asset01.png "Yeni bir varlık katalog oluşturma")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çözüm Gezgini'nde sağ tıklayın **varlık kataloglar** klasörü ve select **Ekle > Yeni varlık Kataloğu**.
2. Bir ad verin ve tıklatın **Ekle**:

    ![](displaying-an-image-images/asset1.png "Yeni bir varlık katalog oluşturma")

-----


Buradan, koleksiyon ile aynı şekilde varsayılan olarak çalışılabilecek `Assets.xcassets` otomatik olarak projeye dahil koleksiyonu.

<a name="controls" />

## <a name="using-images-with-controls"></a>Denetimler ile görüntüleri kullanma

Bir uygulamayı desteklemek için görüntüleri kullanmanın yanı sıra iOS sekmesini çubukları, araç çubukları, gezinti çubukları, tablolar ve düğmeleri gibi uygulama denetim türleriyle görüntüleri de kullanır. Bir denetimde görünen bir görüntü oluşturmak için basit bir yol atamaktır bir `UIImage` denetimin örneğine `Image` özelliği.

<a name="frombundle" />

### <a name="frombundle"></a>FromBundle

`FromBundle` Yöntem çağrısı çağrıdır çok sayıda görüntü yükleme ve yönetim özelliği desteği ve görüntü dosyaları çeşitli çözümler için otomatik işlenmesini önbelleğe alma gibi yerleşik olan zaman uyumlu (engelleme).  

Aşağıdaki örnekte görüntüsü ayarlanacağı gösterilmiştir bir `UITabBarItem` üzerinde bir `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Varsayılarak `MyImage` bir varlık Kataloğu yukarıdaki eklenen bir görüntü varlığı adıdır. Varlık Kataloğu resimleri çalışırken, yalnızca görüntü kümesinde adını belirtin `FromBundle` yöntemi **PNG** görüntüleri biçimlendirilmiş:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Diğer görüntü biçimi için adı uzantısıyla içerir. Örneğin:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

Simgeler ve görüntüleri hakkında daha fazla bilgi için Apple belgelerine bakın [özel simge ve görüntü oluşturma yönergeleri](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html).

<a name="Displaying-an-Image-in-a-Storyboards" />

## <a name="displaying-an-image-in-a-storyboards"></a>Film şeritleri içinde bir görüntü görüntüleme

Görüntüyü bir varlık kataloglar kullanarak bir Xamarin.iOS projesi eklendikten sonra kolayca olabilir bir film şeridi kullanarak görüntülenen bir `UIImageView` Tasarımcısı iOS içinde. Örneğin, aşağıdaki görüntü varlığı eklediyseniz:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](displaying-an-image-images/display01.png "Örnek bir görüntü varlığı eklendi")

Bir şeridinde görüntülemek için aşağıdakileri yapın:

1. Çift `Main.storyboard` dosyasını **Çözüm Gezgini** iOS Tasarımcısı düzenlemek için açın.
2. Seçin bir **görüntü Görünüm** gelen **araç**:

     ![](displaying-an-image-images/display02.png "Araç Kutusu'ndan bir görüntü görünüm seçin")
3. Görüntü görünüm konumu ve tasarım yüzeyi sürükleyin ve gerektiğinde boyutu:

    ![](displaying-an-image-images/display03.png "Tasarım yüzeyine yeni bir resim görünümü")
4. İçinde **pencere öğesi** bölümünü **özelliği Explorer** istenen seçin **görüntü** görüntülenecek varlık:

    ![](displaying-an-image-images/display04.png "Görüntülenecek istenen görüntü varlığı seçin")
5. İçinde **Görünüm** bölümünde, kullanmak **modu** nasıl görüntü denetlemenizi ne zaman yeniden boyutlandırılabilir **resim görünümü** boyutlandırılır.
6. İle **resim görünümü** seçili, onu yeniden eklemek için tıklatın **kısıtlamaları**:

    ![](displaying-an-image-images/display05.png "Kısıtlamaları ekleme")
7. Her kenarında "şeklinde T" tutamacını sürükleyin **resim görünümü** "yanları görüntüye sabitlemek için" Ekran karşılık gelen tarafına. Bu şekilde **resim görünümü** küçültmek ve ekran boyutlandırıldığında büyütün.
8. Film şeridi için değişiklikleri kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "Örnek bir görüntü varlığı eklendi")

Bir şeridinde görüntülemek için aşağıdakileri yapın:

1. Çift `Main.storyboard` dosyasını **Çözüm Gezgini** iOS Tasarımcısı düzenlemek için açın.
2. Seçin bir **görüntü Görünüm** gelen **araç**:

     ![](displaying-an-image-images/display02vs.png "Araç Kutusu'ndan bir görüntü görünüm seçin")
3. Görüntü görünüm konumu ve tasarım yüzeyi sürükleyin ve gerektiğinde boyutu:

    ![](displaying-an-image-images/display03vs.png "Tasarım yüzeyine yeni bir resim görünümü")
4. İçinde **pencere öğesi** bölümünü **özelliği Explorer** istenen seçin **görüntü** görüntülenecek varlık:

    ![](displaying-an-image-images/display04vs.png "Görüntülenecek istenen görüntü varlığı seçin")
5. İçinde **Görünüm** bölümünde, kullanmak **modu** nasıl görüntü denetlemenizi ne zaman yeniden boyutlandırılabilir **resim görünümü** boyutlandırılır.
6. İle **resim görünümü** seçili, onu yeniden eklemek için tıklatın **kısıtlamaları**:

    ![](displaying-an-image-images/display05vs.png "Kısıtlamaları ekleme")
7. Her kenarında "şeklinde T" tutamacını sürükleyin **resim görünümü** "yanları görüntüye sabitlemek için" Ekran karşılık gelen tarafına. Bu şekilde **resim görünümü** küçültmek ve ekran boyutlandırıldığında büyütün.
8. Film şeridi için değişiklikleri kaydedin.

-----


<a name="Displaying-an-Image-in-Code" />

## <a name="displaying-an-image-in-code"></a>Kodda bir görüntü görüntüleme

Görüntüyü Xamarin.iOS bir varlık kataloglar kullanarak bir projeye eklendiğinde görüntünün bir film şeridi yalnızca görüntüleme gibi bunu kolayca C# kod kullanarak görüntülenebilir.

Aşağıdaki örnek alın:

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

Bu kod yeni bir oluşturur `UIImageView` ve bir başlangıç boyutunu ve konumunu sağlar. Bu projeye eklenen bir görüntü varlığı görüntüyü yükler ve ekler `UIImageView` üst `UIView` görüntülemek için.

## <a name="related-links"></a>İlgili bağlantılar

- [Görüntüleri (örnek) ile çalışma](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Merhaba, iPhone](~/ios/get-started/hello-ios/index.md)
- [Özel bir simge ve görüntü oluşturma yönergeleri](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
