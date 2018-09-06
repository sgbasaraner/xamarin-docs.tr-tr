---
title: Xamarin.Mac görüntüleri
description: Bu makale, görüntüler ve simgeler Xamarin.Mac uygulamada çalışmaya kapsar. Bu, oluşturulması ve bakımının yapılması uygulamanızın simgesi oluşturmak için gereken ve C# kodu hem de Xcode'un arabirim Oluşturucu görüntüleri kullanarak görüntüleri açıklar.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 3bc3c731729f92ae3c5ad6126166892a236e2eab
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780574"
---
# <a name="images-in-xamarinmac"></a>Xamarin.Mac görüntüleri

_Bu makale, görüntüler ve simgeler Xamarin.Mac uygulamada çalışmaya kapsar. Bu, oluşturulması ve bakımının yapılması uygulamanızın simgesi oluşturmak için gereken ve C# kodu hem de Xcode'un arabirim Oluşturucu görüntüleri kullanarak görüntüleri açıklar._

## <a name="overview"></a>Genel Bakış

Bir Xamarin.Mac uygulamasında çalışırken, C# ve .NET ile aynı görüntü erişiminiz ve simge, içinde çalışmakta olan bir geliştirici araçları *Objective-C* ve *Xcode* yapar.

Bir macOS (Mac OS X eski adıyla) uygulaması içinde kullanılan varlıklar o yansıma birkaç yolu vardır. Araç çubuğu veya kaynak liste öğesi gibi bir UI denetimi için simgeler, sağlamayı atayarak bir görüntü için uygulamanızın kullanıcı arabiriminin bir parçası olarak görüntülenmesini Xamarin.Mac, aşağıdaki yollarla macOS uygulamalarınız için büyük resim eklemek kolaylaştırır : 

- **Kullanıcı Arabirimi öğeleri** -arka plan olarak veya bir görüntü görünümde, uygulamanızın bir parçası olarak, görüntüleri görüntülenebilir (`NSImageView`).
- **Düğme** -düğmeleri görüntüleri görüntülenebilir (`NSButton`).
- **Görüntü hücre** - göre tablo denetimi bir parçası olarak (`NSTableView` veya `NSOutlineView`), görüntüleri görüntü hücrede kullanılabilir (`NSImageCell`).
- **Araç çubuğu öğesi** -araç çubuğu görüntülerini eklenebilir (`NSToolbar`) görüntü araç çubuğu öğesi olarak (`NSToolbarItem`).
- **Kaynak liste simgesini** - kaynak listesinin bir parçası olarak (özel olarak biçimlendirilmiş `NSOutlineView`).
- **Uygulama simgesi** -görüntüleri bir dizi içinde birlikte gruplandırılabilir bir `.icns` ayarlayın ve uygulamanızın simgesi olarak kullanılır. Bkz. bizim [uygulama simgesi](~/mac/deploy-test/app-icon.md) daha fazla bilgi için belgelere bakın.

Ayrıca, macOS, uygulamanızın genelinde kullanılan önceden tanımlanmış bir görüntü kümesi sağlar.

[![Bir örnek bir uygulamayı çalıştırmanızı](image-images/intro01.png "bir örnek uygulamanın çalıştırın")](image-images/intro01-large.png#lightbox)

Bu makalede, biz bir Xamarin.Mac uygulamasında görüntüler ve simgeler ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.


## <a name="adding-images-to-a-xamarinmac-project"></a>Xamarin.Mac projeye görüntüleri ekleme

Bir görüntü kullanmak için bir Xamarin.Mac uygulamasında eklerken, çeşitli yerlerde ve geliştirici projenin kaynak görüntü dosyasını içerebilir yolları vardır:

- **Ana proje ağacı [kullanım dışı]** -görüntülerini doğrudan projeleri ağacına eklenebilir. Ana proje ağacında koddan depolanan görüntüler çağırırken hiçbir klasör konumu belirtildi. Örneğin: `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **[Kullanım dışı] Kaynaklar klasörünü** -özel **kaynakları** klasör, kullanıcının, uygulamanın bir parçası olacak tüm dosya paket için simge, ekran başlatma veya genel gibi görüntüler (veya başka görüntü veya dosya Geliştirici eklemek isteyen). Depolanan görüntüler çağrılırken **kaynakları** kod klasöründen, görüntüleri gibi yalnızca ana proje ağacında depolanan, hiçbir klasör konumu belirtilmedi. Örneğin: `NSImage.ImageNamed("tags.png")`.
- **Özel klasör veya alt [kullanım dışı]** -Geliştirici özel bir klasör projeler kaynak ağacına ekleyebilir ve orada görüntüleri depolayın. Daha fazla proje düzenlemenize yardımcı olmak için bir alt klasöre dosya nerede eklendiğinde konumu yuvalanabilir. Örneğin, geliştirici eklediyseniz bir `Card` klasör proje ile bir alt klasörü `Hearts` bu klasöre ardından görüntüyü depolamak **Jack.png** içinde `Hearts` klasöründe `NSImage.ImageNamed("Card/Hearts/Jack.png")` görüntü yüklenir çalışma zamanı.
- **Varlık Kataloğu görüntü [tercih edilen] ayarlar** - OS X El Capitan, eklenen **varlık katalogları görüntü kümeleri** tüm sürümleri veya çeşitli cihazları desteklemek ve ölçek için gerekli olan bir görüntü gösterimlerini içeren, uygulama. Görüntü varlıkları dosya kalmak yerine (**@1x**, **@2x**).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Varlık Kataloğu görüntüye görüntüleri ekleme ayarlayın

Yukarıda belirtildiği gibi bir **varlık katalogları görüntü kümeleri** tüm sürümleri veya uygulamanız için ölçek ve çeşitli cihazları desteklemek için gerekli olan bir görüntü temsillerini içerir. Görüntü varlıkları dosya kalmak yerine (bkz. Yukarıdaki görüntü terminolojiyi ve çözümleme bağımsız görüntüleri), **görüntü kümeleri** hangi görüntü hangi cihaz ve/veya çözümleme ait belirlemek için varlık Düzenleyicisi'ni kullanın.

1. İçinde **çözüm bölmesi**, çift **Assets.xcassets** dosyayı düzenlemek için açın: 

    ![Assets.xcassets seçerek](image-images/imageset01.png "Assets.xcassets seçme")
2. Sağ **Assets listesini** seçip **yeni görüntü kümesi**: 

    [![Yeni bir görüntü kümesi ekleme](image-images/imageset02.png "yeni bir görüntü kümesi ekleme")](image-images/imageset02-large.png#lightbox)
3. Yeni görüntü kümesi seçin ve düzenleyici görüntülenir: 

    [![Yeni görüntü kümesi seçme](image-images/imageset03.png "yeni görüntü kümesi seçme")](image-images/imageset03-large.png#lightbox)
4. Buradan her biri farklı cihaz ve gereken çözünürlükleri için görüntüleri sürükleyebilirsiniz. 
5. Yeni görüntü kümenin çift **adı** içinde **Assets listesini** düzenlemek için: 

    [![Görüntüyü düzenleme kümesi adı](image-images/imageset04.png "görüntüyü düzenleme kümesi adı")](image-images/imageset04-large.png#lightbox)
    
Özel bir **vektör** sınıfı eklenmiş olarak **görüntü kümeleri** dahil etmemizi sağlayan bir _PDF_ bunun yerine tek bir bit eşlem dosyaları dahil olmak üzere casset vektör görüntüde biçimlendirilmiş farklı çözümler. Bu yöntemi kullanarak, bir tek vektör dosyası için tedarik **@1x** (vektör PDF dosyası olarak biçimlendirilmiş) çözüm ve **@2x** ve **@3x** derleme zamanında oluşturulan ve uygulamanın pakete eklenen dosyanın sürümü.

[![Düzenleyici arabirimini resmi ayarlama](image-images/imageset05.png "Düzenleyici arabirimini resmi ayarlama")](image-images/imageset05-large.png#lightbox)

Örneğin, eklerseniz bir `MonkeyIcon.pdf` dosyası bir varlık Kataloğu vektörü 150px x 150px, aşağıdaki bit eşlem varlıklar eklenmesi son uygulama paketi grubuna, derlendiğinde, bir çözüm olarak:

1. **MonkeyIcon@1x.png** -150px x 150px çözümleme.
2. **MonkeyIcon@2x.png** -300 piksel x 300px çözümleme.
3. **MonkeyIcon@3x.png** -450px x 450px çözümleme.

Aşağıdakiler dikkate PDF vektör görüntüleri varlık katalogları kullanırken dikkat edilmelidir:

- PDF taranmış bir bit eşlemi derleme zaman ve son uygulamada sevk bit eşlemler olarak değil tam vektör destek budur.
- Varlık Kataloğu'nda ayarlandıktan sonra görüntünün boyutu ayarlayamazsınız. Görüntü (veya kodda Otomatik Yerleşim ve boyut sınıflarını kullanarak) yeniden boyutlandırmak çalışırsanız resmin diğer bitmap gibi bozuk.

Kullanırken bir **resmi ayarlama** Xcode'un arabirimi Oluşturucu'da, yalnızca kümesinin adı açılan listeden seçebilirsiniz **özniteliği denetçisi**: **

![Bir görüntüyü seçerek Xcode'un arabirimi Oluşturucusu'nda ayarlayın](image-images/imageset06.png "ayarlamak Xcode'un arabirimi Oluşturucu'da görüntü seçme")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Yeni varlıklar koleksiyonu ekleme

Varlık katalogları görüntüleri ile çalışırken tüm görüntülerinizin eklemek yerine yeni bir koleksiyon oluşturmak istediğiniz zaman zamanlar olabilir **Assets.xcassets** koleksiyonu. Örneğin, isteğe bağlı kaynakları tasarlarken.

Yeni bir varlık Kataloğu projenize eklemek için:

1. Projeye sağ tıklayarak **çözüm bölmesi** seçip **Ekle** > **yeni dosya...**
2. Seçin **Mac** > **varlık Kataloğu**, girin bir **adı** tıklatın ve koleksiyon için **yeni** düğmesi: 

    ![Yeni bir varlık Kataloğu ekleme](image-images/asset01.png "yeni bir varlık Kataloğu ekleme")

Buradan koleksiyon ile aynı şekilde varsayılan olarak çalışabilir **Assets.xcassets** otomatik olarak projeye dahil koleksiyonu.


### <a name="adding-images-to-resources"></a>Görüntü kaynakları ekleme

> [!IMPORTANT]
> Bu yöntem, görüntüleri bir macOS uygulaması ile çalışma, Apple tarafından onaylanmaz. Kullanmanız gereken [varlık Kataloğu görüntü kümeleri](#asset-catalogs) Yöneticisi için bunun yerine, uygulamanızın görüntüler.

Xamarin.Mac uygulamanızda (ya da C# kodu veya arabirim Oluşturucu) bir resim dosyası kullanmadan önce projesinin eklenmesi gerektiğini **kaynakları** klasörü olarak bir **paket kaynak**. Projeye bir dosya eklemek için aşağıdakileri yapın:

1. Sağ **kaynakları** projenizde klasöründe **çözüm bölmesi** seçip **Ekle** > **Dosya Ekle...** : 

    ![Dosya ekleme](image-images/add01.png "dosya ekleme")
2. Gelen **Add Files** görüntüleri dosyaları projeye Ekle iletişim kutusunda, seçmek `BundleResource` için **geçersiz kılmayı derle eylem** tıklatıp **açık** düğmesi:

    [![Eklenecek dosyaları seçerek](image-images/add02.png "eklemek üzere dosyaları seçme")](image-images/add02-large.png#lightbox)
3. Dosyalar zaten kullanımda değilse **kaynakları** klasöründe sorulur istiyorsanız **kopyalama**, **taşıma** veya **bağlantı** dosyaları. Hangi her cins çekme genellikle, olur gereksinimleriniz **kopyalama**:

    ![Ekle eylemini seçerek](image-images/add04.png "Ekle eylemini seçerek")
4. Yeni dosyalar projeye dahil ve okumak için kullanın: 

    ![Çözüm bölmesi için eklenen yeni resim dosyalarını](image-images/add03.png "çözüm bölmesi için eklenen yeni görüntü dosyaları")
5. Gerekli olan tüm resim dosyaları için işlemi tekrarlayın.

Xamarin.Mac uygulamanıza bir kaynak görüntüsü olarak herhangi bir png, jpg veya pdf dosyasını kullanabilirsiniz. Sonraki bölümde, yüksek çözünürlüklü Görüntülerimizi sürümlerini ekleme sırasında inceleyeceğiz ve Mac bilgisayarları tabanlı Retina desteklemek için simgeler.

> [!IMPORTANT]
> Görüntüleri ekliyorsanız **kaynakları** klasöründe bırakabilirsiniz **geçersiz kılmayı derle eylem** kümesine **varsayılan**. Bu klasör için derleme eylemi varsayılandır `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Tüm uygulama grafik kaynakların yüksek çözünürlüklü versiyona sağlayın

Bir Xamarin.Mac uygulamasını (simge, özel denetimler, özel imleçler, özel resimler, vb.) eklediğiniz herhangi bir grafik varlığı Standart çözünürlükte sürümlerine ek olarak yüksek çözünürlüklü versiyona olması gerekir. Mac bilgisayar Retina görünen çalıştırmada donatılmış bittiğinde, uygulamanızı en iyi şekilde görünür, böylece bu gereklidir.


### <a name="adopt-the-2x-naming-convention"></a>Benimseme @2x adlandırma kuralı

> [!IMPORTANT]
> Bu yöntem, görüntüleri bir macOS uygulaması ile çalışma, Apple tarafından onaylanmaz. Kullanmanız gereken [varlık Kataloğu görüntü kümeleri](#asset-catalogs) Yöneticisi için bunun yerine, uygulamanızın görüntüler.

Standart ve yüksek çözünürlüklü görüntüyü sürümleri oluşturduğunuzda, görüntü çifti için şu adlandırma kuralını Xamarin.Mac projenize dahil olmak üzere zaman izleyin:

- **Standart çözünürlükte**  - **ImageName.filename uzantısı** (örnek: **tags.png**)
- **Yüksek çözünürlüklü**   -  **ImageName@2x.filename-extension** (örnek: **tags@2x.png**)

Bir projeye eklendiğinde, şu şekilde görünür:

![Görüntü dosyaları çözüm panelinde](image-images/add03.png "çözüm panelinde görüntü dosyaları")

Bir görüntü arabirimi Oluşturucu UI öğesi atandığında dosyasında yalnızca öğrenmiş olacaksınız _IMAGENAME_**.** _dosya adı uzantısı_ biçiminde (örnek: **tags.png**). Aynı görüntüyü kullanarak C# kodu için dosyada öğrenmiş olacaksınız _IMAGENAME_**.** _dosya adı uzantısı_ biçimi.

Xamarin.Mac uygulama, Mac bilgisayarlarda, çalıştırdığınızda _IMAGENAME_**.** _dosya adı uzantısı_ görüntü biçimlendirme Standart çözünürlük ekranlarda kullanılacak **ImageName@2x.filename-extension** görüntü otomatik olarak çekilen Retina görünen tabanları Mac.


## <a name="using-images-in-interface-builder"></a>Arabirimi Oluşturucu'da görüntülerini kullanma

Eklediğiniz herhangi bir görüntü kaynağı **kaynakları** , Xamarin.Mac klasöründe proje ve derleme eylemi olarak ayarlanmış **BundleResource** olabilir ve arabirim Oluşturucu'da otomatik olarak gösterilir (görüntüleri işliyorsa) UI öğesi bir parçası olarak seçildi.

Arabirimi Oluşturucu'da bir görüntüyü kullanmak için aşağıdakileri yapın:

1. Görüntüye ekleme **kaynakları** klasörüyle bir **derleme eylemi** , `BundleResource`: 

     ![Çözüm panelinde bir görüntü kaynağı](image-images/ib00.png "çözüm panelinde bir görüntü kaynağı")
2. Çift **Main.storyboard** dosyayı arabirimi Oluşturucusu'nda düzenleme için açın: 

     [![Ana görsel taslak düzenleme](image-images/ib01.png "ana görsel taslak düzenleme")](image-images/ib01-large.png#lightbox)
3. Tasarım yüzeyine görüntüleri alan UI öğesi sürükleyin (örneğin, bir **görüntü araç çubuğu öğesi**): 

     ![Araç çubuğu öğesi düzenleme](image-images/ib02.png "düzenleme araç çubuğu öğesi")
4. Eklediğiniz görüntüyü seçin **kaynakları** klasöründe **görüntü adı** açılır: 

     [![Araç çubuğu öğesi için bir görüntü seçerek](image-images/ib03.png "araç çubuğu öğesi için bir görüntü seçme")](image-images/ib03-large.png#lightbox)
5. Tasarım yüzeyinde, seçilen görüntü görüntülenir: 

     ![Araç çubuğu Düzenleyicisi'nde görüntülenen görüntünün](image-images/ib04.png "araç çubuğu Düzenleyicisi'nde görüntülenen resmi")
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Kendi görüntü özelliğinin ayarlanması izin veren herhangi bir UI öğe için yukarıdaki adımlarını **özniteliği denetçisi**. Yeniden eklediyseniz, bir **@2x** , görüntü dosyasının sürümü, bu otomatik olarak kullanılacak üzerinde Mac Retina görüntü tabanlı.

> [!IMPORTANT]
> Resim kullanılamaz durumdaysa **görüntü adı** açılır listesinde, .storyboard projenizi xcode'da kapatıp Mac için Visual Studio'da yeniden açın Görüntünün kullanılabilir değilse, yine de emin olun, **derleme eylemi** olan `BundleResource` ve görüntü için eklenmiş **kaynakları** klasör.

## <a name="using-images-in-c-code"></a>C# kodunda görüntülerini kullanma

Görüntü, C# kodunu kullanarak Xamarin.Mac uygulamanızda belleğe yüklenirken, görüntünün barındırılacaktır bir `NSImage` nesne. Görüntü dosyası (kaynakları dahil) Xamarin.Mac uygulama paketteki dahil, görüntüyü yüklemek için aşağıdaki kodu kullanın:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Yukarıdaki kodu statik kullanır `ImageNamed("...")` yöntemi `NSImage` belleğe belirtilen görüntü yüklemek için sınıf **kaynakları** klasörü, görüntüyü bulunamazsa `null` döndürülür. Eklediyseniz, atanan arabirimi Oluşturucu'da görüntü gibi bir **@2x** , görüntü dosyasının sürümü, bu otomatik olarak kullanılacak üzerinde Mac Retina görüntü tabanlı.

Uygulama paketinden (Mac dosya sistemi) dışındaki görüntülerin yüklemek için aşağıdaki kodu kullanın:

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Şablon görüntüleri ile çalışma

MacOS uygulamanızın tasarımına bağlı, simge veya resim (örneğin, kullanıcı tercihleri temelinde) renk düzeninde bir değişiklik eşleştirmek için kullanıcı arabirimi içinde özelleştirme gerektiğinde zamanlar olabilir.

Bu etkiyi elde etmek için geçiş _işleme modu_ , görüntü varlığınız **şablon görüntüsü**:

[![Bir şablon görüntüsü ayarlama](image-images/templateimage01.png "bir şablon görüntüsü ayarlama")](image-images/templateimage01-large.png#lightbox)

Xcode'un arabirimi Oluşturucusu'ndan bir UI denetimine görüntü varlık atayın:

![Xcode'un arabirimi Oluşturucu'da bir görüntü seçerek](image-images/templateimage02.png "Xcode'un arabirimi Oluşturucu'da görüntü seçme")

Veya resim kaynağını kodu isteğe bağlı olarak ayarlayın:

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Görünüm denetleyicinizdeki genel aşağıdaki işlevi ekleyin:

```csharp
public NSImage ImageTintedWithColor (NSImage image, NSColor tint)
{
    var tintedImage = image.Copy () as NSImage;
    var frame = new CGRect (0, 0, image.Size.Width, image.Size.Height);

    // Apply tint
    tintedImage.LockFocus ();
    tint.Set ();
    NSGraphics.RectFill (frame, NSCompositingOperation.SourceAtop);
    tintedImage.UnlockFocus ();
    tintedImage.Template = false;

    // Return tinted image
    return tintedImage;
}
```

Son olarak, bir şablon görüntüsü renk tonu için renklendirmeye görüntüye karşı bu işlevi çağırın:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>Tablo görünümleri ile görüntülerini kullanma

Hücresinde bir parçası olarak bir resim eklemek için bir `NSTableView`, tablo görünümün tarafından döndürülen veriler nasıl değiştirileceği ihtiyacınız olacak `NSTableViewDelegate's` `GetViewForItem` yönteminin kullanılacağını bir `NSTableCellView` tipik yerine `NSTextField`. Örneğin:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Burada birkaç satır vardır. İlk olarak, bir resim eklemek istediğimiz sütunları için yeni bir oluştururuz `NSImageView` gereken boyut ve konum, biz de yeni bir oluşturma `NSTextField` ve olup olmadığını biz görüntü kullanmanıza bağlı varsayılan konumuna getirin:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

İkincisi, en yeni resim görünümü ve metin alanının üst eklemek ihtiyacımız `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Son olarak, biz, küçültme ve böylelikle Tablo görünümü hücresi ile büyütün metin alanına söylemeniz gerekir:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Örnek çıktı:

[![Bir uygulamada bir görüntüyü görüntüleme örneği](image-images/tables01.png "bir örneği bir uygulamada bir görüntüyü görüntüleme")](image-images/tables01-large.png#lightbox)

Tablo görünümleri ile çalışma hakkında daha fazla bilgi için lütfen bkz. bizim [tablo görünümleri](~/mac/user-interface/table-view.md) belgeleri.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>Anahat görünümleri ile görüntülerini kullanma

Hücresinde bir parçası olarak bir resim eklemek için bir `NSOutlineView`, anahat görünümün tarafından döndürülen veriler nasıl değiştirileceği ihtiyacınız olacak `NSTableViewDelegate's` `GetView` yönteminin kullanılacağını bir `NSTableCellView` tipik yerine `NSTextField`. Örneğin:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Burada birkaç satır vardır. İlk olarak, bir resim eklemek istediğimiz sütunları için yeni bir oluştururuz `NSImageView` gereken boyut ve konum, biz de yeni bir oluşturma `NSTextField` ve olup olmadığını biz görüntü kullanmanıza bağlı varsayılan konumuna getirin:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

İkincisi, en yeni resim görünümü ve metin alanının üst eklemek ihtiyacımız `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Son olarak, biz, küçültme ve böylelikle Tablo görünümü hücresi ile büyütün metin alanına söylemeniz gerekir:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Örnek çıktı:

[![Örnek bir ana görünümünde görüntülenen görüntünün](image-images/outline01.png "bir ana görünümünde görüntülenen görüntünün örneği")](image-images/outline01-large.png#lightbox)

Anahat görünümleri ile çalışma hakkında daha fazla bilgi için lütfen bkz. bizim [anahat görünümleri](~/mac/user-interface/outline-view.md) belgeleri.


## <a name="summary"></a>Özet

Bu makalede ayrıntılı bir Xamarin.Mac uygulama içinde görüntüler ve simgeler ile çalışmak göz duruma getirdi. Biz farklı türleri gördünüz ve görüntüler, görüntüler ve simgeler Xcode'un arabirimi Oluşturucu'da kullanma ve C# kodunda görüntüler ve simgeler ile çalışma konusunda kullanır.



## <a name="related-links"></a>İlgili bağlantılar

- [MacImages (örnek)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Tablo görünümleri](~/mac/user-interface/table-view.md)
- [Anahat görünümleri](~/mac/user-interface/outline-view.md)
- [macOS X İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [OS X için yüksek çözünürlüklü hakkında](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
