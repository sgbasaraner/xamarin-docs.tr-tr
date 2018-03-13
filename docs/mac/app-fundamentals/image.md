---
title: "Görüntüler"
description: "Bu makalede, görüntüler ve Xamarin.Mac uygulama simgeleri ile çalışma yer almaktadır. Oluşturma ve uygulamanızın simgesi oluşturmak için gereken ve C# kodu ve Xcode'nın arabirimi Oluşturucu'da görüntü kullanarak görüntüleri koruma açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: d8098afea87765166db8318b76adf250818a0a6f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="images"></a>Görüntüler

_Bu makalede, görüntüler ve Xamarin.Mac uygulama simgeleri ile çalışma yer almaktadır. Oluşturma ve uygulamanızın simgesi oluşturmak için gereken ve C# kodu ve Xcode'nın arabirimi Oluşturucu'da görüntü kullanarak görüntüleri koruma açıklar._


## <a name="overview"></a>Genel Bakış

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı görüntüsüne erişimi ve simge, çalışan bir geliştirici araçları *Objective-C* ve *Xcode* yapar.

Varlıklar (eski adıyla Mac OS X) macOS uygulaması içinde kullanılan, görüntü birkaç yolu vardır. Bir görüntü gibi bir araç çubuğu veya kaynak liste öğesi, bir UI denetimi için simgeler, sağlamayı atayarak uygulamanızın kullanıcı Arabirimi için bir parçası olarak görüntülemede Xamarin.Mac harika resmi aşağıdaki yollarla macOS uygulamalarınıza eklemek kolaylaştırır : 

- **Kullanıcı Arabirimi öğeleri** -arka plan veya bir görüntü görünüm uygulamanızda bir parçası olarak resimler görüntülenebilir (`NSImageView`).
- **Düğme** -resimler düğmeleri görüntülenebilir (`NSButton`).
- **Görüntü hücre** - temel tablo denetim bir parçası olarak (`NSTableView` veya `NSOutlineView`), görüntüleri görüntü hücrede kullanılabilir (`NSImageCell`).
- **Araç çubuğu öğesi** -araç çubuğuna görüntülerini eklenebilir (`NSToolbar`) görüntüsü araç çubuğu öğesi olarak (`NSToolbarItem`).
- **Kaynak listesi simgesi** - kaynak listesinin bir parçası olarak (özel olarak biçimlendirilmiş `NSOutlineView`).
- **Uygulama simgesi** -bir dizi görüntü birlikte toplanabilir bir `.icns` ayarlamak ve uygulamanızın simgesi olarak kullanılır. Bkz: bizim [uygulama simgesi](~/mac/deploy-test/app-icon.md) daha fazla bilgi için.

Ayrıca, macOS uygulamanızın genelinde kullanılan önceden tanımlanmış görüntüler kümesi sağlar.

[![Bir örneği çalıştırmak uygulamasının](image-images/intro01.png "örnek uygulamasının çalıştırın")](image-images/intro01-large.png#lightbox)

Bu makalede, sizi bir Xamarin.Mac uygulamasında görüntüler ve simgeler ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.


## <a name="adding-images-to-a-xamarinmac-project"></a>Xamarin.Mac projeye görüntüler ekleme

Görüntüyü kullanmak için bir Xamarin.Mac uygulamasında eklerken, çeşitli yerlerde ve geliştirici projenin kaynak görüntü dosyasını içerebilir yolları vardır:

- **[Kullanım dışı] ana proje ağacı** -doğrudan projeleri ağacına görüntülerini eklenebilir. Kod ana proje ağacından depolanan görüntüleri çağrılırken hiçbir klasör konumu belirtildi. Örneğin: `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **[Kullanım dışı] Kaynaklar klasörünü** -özel **kaynakları** klasördür kullanıcının uygulamanın parçası olacak herhangi bir dosyayı paketini için simge, başlatma ekranı veya genel gibi görüntüleri (veya başka görüntü veya dosya Geliştirici Eklenecek istediği). Depolanan görüntüleri çağrılırken **kaynakları** kod klasöründen, görüntüleri gibi yalnızca ana proje ağacında depolanan, hiçbir klasör konumu belirtilmiş. Örneğin: `NSImage.ImageNamed("tags.png")`.
- **Özel klasör veya alt klasör [kullanım dışı]** -Geliştirici projeleri kaynak ağacına özel bir klasör ekleyin ve vardır görüntüleri depolamak. Dosyanın nereye eklenir konumu daha fazla proje düzenlenmesine yardımcı olmak için bir alt klasöre iç içe. Örneğin, geliştirici eklediyseniz bir `Card` proje ve bir alt klasörü klasörüne `Hearts` bu klasöre sonra görüntüyü depolamak **Jack.png** içinde `Hearts` klasörünü `NSImage.ImageNamed("Card/Hearts/Jack.png")` yansımayı yüklemek çalışma zamanı.
- **Varlık Kataloğu görüntü [tercih edilen] kümeleri** - OS X El Capitan, eklenen **varlık kataloglar görüntü kümeleri** tüm sürümleri veya için Etkenler ölçekleme ve çeşitli aygıtları desteklemek gerekli olan bir görüntü gösterimlerini içeren, uygulama. Görüntü varlıklar dosya kalmak yerine (**@1x**,  **@2x** ).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Varlık Kataloğu görüntü için görüntü ekleme ayarlayın

Yukarıda belirtildiği gibi bir **varlık kataloglar görüntü kümeleri** tüm sürümleri veya uygulamanız için Etkenler ölçekleme ve çeşitli aygıtları desteklemek gerekli olan bir görüntü gösterimlerini içerir. Görüntü varlıklar dosya kalmak yerine (Çözümleme bağımsız görüntüler ve görüntü terminolojisi yukarıdaki bakın), **görüntü kümeleri** hangi görüntü hangi cihaz ve/veya çözümleme ait belirtmek için varlık Düzenleyicisi'ni kullanın.

1. İçinde **çözüm paneli**, çift **Assets.xcassets** dosyayı düzenlemek için açın: 

    ![Assets.xcassets seçme](image-images/imageset01.png "Assets.xcassets seçme")
2. Sağ **varlıklar listesi** seçip **yeni görüntü kümesi**: 

    [![Yeni bir görüntü kümesi ekleme](image-images/imageset02.png "yeni bir görüntü kümesi ekleme")](image-images/imageset02-large.png#lightbox)
3. Yeni bir görüntü kümesi seçin ve düzenleyici görüntülenir: 

    [![Yeni bir görüntü kümesi seçme](image-images/imageset03.png "yeni bir görüntü kümesi seçme")](image-images/imageset03-large.png#lightbox)
4. Buradan biz görüntülerinde her biri farklı cihaz ve gerekli çözümleri sürükleyebilirsiniz. 
5. Yeni görüntü kümenin çift **adı** içinde **varlıklar listesi** düzenlemek için: 

    [![Görüntüyü düzenleme kümesi adı](image-images/imageset04.png "kümesi adı görüntüyü düzenleme")](image-images/imageset04-large.png#lightbox)
    
Özel bir **vektör** sınıfı için eklenene gibi **görüntü kümeleri** bize eklemeyi sağlayan bir _PDF_ yerine tek tek bit eşlem dosyaları dahil olmak üzere casset vektör görüntüde biçimlendirilmiş farklı çözümler. Bu yöntemi kullanarak, bir tek vektör dosya için sağladığınız  **@1x**  (vektör PDF dosyası olarak biçimlendirilmiş) çözümleme ve  **@2x**  ve  **@3x**  dosya sürümleri derleme zamanında oluşturulan ve uygulamanın pakete eklenen.

[![Düzenleyici arabirimi resmi ayarlama](image-images/imageset05.png "Düzenleyici arabirimi resmi ayarlama")](image-images/imageset05-large.png#lightbox)

Örneğin, dahil ederseniz bir `MonkeyIcon.pdf` dosyası olarak bir varlık katalog vektör 150px x 150px, varlıklar dahil edilebilir son uygulama paketine derlenmesinden olduğunda aşağıdaki bit eşlem'i, çözünürlük:

1. **MonkeyIcon@1x.png** -150px x 150px çözümleme.
2. **MonkeyIcon@2x.png** -300px x 300px çözümleme.
3. **MonkeyIcon@3x.png** -450px x 450px çözümleme.

Aşağıdakileri göz önünde PDF vektör görüntüleri varlık kataloglarında kullanırken yapılması gerekir:

- PDF bir bit eşlem için derleme zamanı ve son uygulamada sevk bit eşlemler rasterleştirilecek gibi bu tam vektör destek değil.
- Varlık kataloğunda ayarladıktan sonra görüntünün boyutu ayarlayamazsınız. Görüntü (veya kod otomatik düzeni ve boyutu sınıflarını kullanarak) yeniden boyutlandırma çalışırsanız, resmin diğer bitmap gibi bozuk.

Kullanırken bir **resmi ayarlama** Xcode'nın arabirimi Oluşturucusu'nda, yalnızca kümesinin adı açılan listede seçebilirsiniz **özniteliği denetçisi**: **

![Bir görüntüyü seçerek ayarlamak Xcode'nın arabirimi Oluşturucusu'nda](image-images/imageset06.png "bir görüntüyü seçerek Xcode'nın arabirimi Oluşturucusu'nda ayarlayın")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Yeni varlıklar koleksiyonu ekleme

Varlıklar kataloglar görüntülerle çalışırken görüntülerinizin tümünün eklemek yerine yeni bir koleksiyon oluşturmak istediğiniz zaman zamanlar olabilir **Assets.xcassets** koleksiyonu. Örneğin, isteğe bağlı kaynakları tasarlarken.

Yeni bir varlık Kataloğu projenize eklemek için:

1. Projeye sağ tıklayın **çözüm paneli** seçip **Ekle** > **yeni dosya...**
2. Seçin **Mac** > **varlık Kataloğu**, girin bir **adı** tıklatın ve koleksiyon için **yeni** düğmesi: 

    ![Yeni bir varlık katalog ekleme](image-images/asset01.png "yeni bir varlık katalog ekleme")

Buradan aynı şekilde varsayılan olarak koleksiyon ile çalışabilirsiniz **Assets.xcassets** otomatik olarak projeye dahil koleksiyonu.


### <a name="adding-images-to-resources"></a>Kaynaklara görüntüler ekleme

> [!IMPORTANT]
> Bu yöntem, bir macOS uygulamasında görüntülerle çalışma Apple'nın kullanım dışıdır. Kullanmanız gereken [varlık Kataloğu görüntü kümeleri](#asset-catalogs) Yöneticisi için bunun yerine, uygulamanızın görüntüler.

Uygulamanızdaki Xamarin.Mac (C# kodunda ya da arabirimi Oluşturucu gelen) bir görüntü dosyası kullanmadan önce projesinin eklenmesi gerekir **kaynakları** klasörü olarak bir **paket kaynak**. Bir dosya için bir proje eklemek için aşağıdakileri yapın:

1. Sağ tıklayın **kaynakları** projenizde klasöründe **çözüm paneli** seçip **Ekle** > **dosyaları Ekle...** : 

    ![Bir dosya ekleme](image-images/add01.png "bir dosya ekleme")
2. Gelen **dosyaları Ekle** görüntüleri dosyaları projeye eklemek için Seç iletişim kutusu, select `BundleResource` için **geçersiz kılma yapı eylemi** tıklatıp **açık** düğmesi:

    [![Eklemek için dosyaları seçerek](image-images/add02.png "eklemek için dosya seçme")](image-images/add02-large.png#lightbox)
3. Dosyalar zaten içinde değilse **kaynakları** klasörü, istenir istiyorsanız **kopya**, **taşıma** veya **bağlantı** dosyaları. Hangi her Setleri çekme genellikle, olacak gereksinimlerinizi **kopyalama**:

    ![Ekle eylemini seçerek](image-images/add04.png "ekleme eylemi seçme")
4. Yeni dosyalar projeye dahil ve kullanım için okuyun: 

    ![Çözüm defterine eklenen yeni resim dosyaları](image-images/add03.png "çözüm defterine eklenen yeni resim dosyaları")
5. Gerekli olan tüm resim dosyaları için bu işlemi yineleyin.

Bir kaynak görüntüsü olarak Xamarin.Mac uygulamanızda herhangi bir png, jpg veya pdf dosyasını kullanabilirsiniz. Sonraki bölümde biz bizim görüntüleri yüksek çözünürlüklü sürümlerini ekleme adresindeki göreceğiz ve Mac bilgisayarları Retina desteklemek için simgeler temel.

> [!IMPORTANT]
> Görüntüleri ekliyorsanız **kaynakları** klasörünü bırakabilir **geçersiz kılma yapı eylemi** kümesine **varsayılan**. Bu klasör için yapı eylemi varsayılandır `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Tüm uygulama grafik kaynakların yüksek çözünürlüklü versiyona sağlayın

Bir Xamarin.Mac uygulaması (simgeler, özel denetimler, özel imleçler, özel resim, vb.) eklediğiniz herhangi bir grafik varlığını standart çözümleme sürümlerine ek olarak yüksek çözünürlüklü versiyona olması gerekir. Bu, böylece Mac bilgisayar Retina görüntü çalıştırılmasında donatılmış uygulamanızı en iyi görüneceğini gereklidir.


### <a name="adopt-the-2x-naming-convention"></a>Benimsemeyi @2x adlandırma

> [!IMPORTANT]
> Bu yöntem, bir macOS uygulamasında görüntülerle çalışma Apple'nın kullanım dışıdır. Kullanmanız gereken [varlık Kataloğu görüntü kümeleri](#asset-catalogs) Yöneticisi için bunun yerine, uygulamanızın görüntüler.

Bir görüntü standart ve yüksek çözünürlüklü sürümleri oluşturduğunuzda, görüntü çifti için bu adlandırma kuralını Xamarin.Mac projenize ekleme yaparken izleyin:

- **Standart çözümleme**  - **ImageName.filename uzantısı** (örnek: **tags.png**)
- **Yüksek çözünürlüklü**   -   **ImageName@2x.filename-extension**  (örnek:  **tags@2x.png** )

Bir projeye eklendiğinde, bunlar şu şekilde görünür:

![Çözüm panelinde görüntü dosyaları](image-images/add03.png "çözüm panelinde resim dosyaları")

Bir kullanıcı Arabirimi öğesi arabirimi Oluşturucu görüntü atandığı yalnızca dosyasında öğrenmiş olacaksınız _görüntü adı_**.** _dosya adı uzantısı_ biçimi (örnek: **tags.png**). Aynı görüntüyü C# kod içinde kullanma için dosyada öğrenmiş olacaksınız _görüntü adı_**.** _dosya adı uzantısı_ biçimi.

Xamarin.Mac uygulama bir Mac üzerinde çalıştırdığınızda _görüntü adı_**.** _dosya adı uzantısı_ biçim görüntüsü standart çözümleme ekranlarda kullanılacak  **ImageName@2x.filename-extension**  görüntü otomatik olarak çekilen Retina görüntü taban Mac'ler.


## <a name="using-images-in-interface-builder"></a>Arabirim Oluşturucusu'nda görüntüleri kullanma

Hizmetine eklediğiniz herhangi bir görüntü kaynağı **kaynakları** , Xamarin.Mac klasöründe proje ve yapı eylem olarak ayarlanmış **BundleResource** arabirimi Oluşturucusu'nda otomatik olarak gösterilir ve olabilir (resimleri işleme durumunda) bir kullanıcı Arabirimi öğesi bir parçası olarak seçilmiş.

Bir görüntüyü arabirimi Oluşturucusu'nda kullanmak için aşağıdakileri yapın:

1. Görüntüye ekleme **kaynakları** klasörüyle bir **yapı eylemi** , `BundleResource`: 

     ![Bir görüntü kaynağı çözüm panelinde](image-images/ib00.png "çözüm panelinde bir görüntü kaynağı")
2. Çift **Main.storyboard** dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın: 

     [![Ana film şeridi düzenleme](image-images/ib01.png "ana film şeridi düzenleme")](image-images/ib01-large.png#lightbox)
3. Tasarım yüzeyine görüntüleri alan UI öğesi sürükleme (örneğin, bir **görüntü araç çubuğu öğesi**): 

     ![Araç çubuğu öğesi düzenleme](image-images/ib02.png "araç çubuğu öğesi düzenleme")
4. Eklediğiniz görüntüyü seçin **kaynakları** klasöründe **görüntü adı** açılır: 

     [![Araç çubuğu öğesi için bir görüntüyü seçerek](image-images/ib03.png "araç çubuğu öğesi için görüntü seçme")](image-images/ib03-large.png#lightbox)
5. Seçilen görüntü tasarım yüzeyine görüntülenir: 

     ![Araç çubuğu Düzenleyicisi'nde görüntülenen görüntünün](image-images/ib04.png "araç çubuğu Düzenleyicisi'nde görüntülenen resmi")
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Kendi görüntü özelliği ayarlanmış olması veren herhangi bir kullanıcı Arabirimi öğe için yukarıdaki adımlarını **özniteliği denetçisi**. Yeniden eklediyseniz, bir  **@2x**  Görüntü dosyanızın sürümü, onu otomatik olarak kullanılacak üzerinde Retina görüntü tabanlı Mac'ler.

> [!IMPORTANT]
> Görüntü kullanılabilir durumda olmazsa **görüntü adı** açılan listesinde, Xcode .storyboard projenizde kapatıp Visual Studio'dan Mac için Görüntü hala kullanılabilir durumda değilse, emin olun, **yapı eylemi** olan `BundleResource` ve görüntü için eklenen **kaynakları** klasör.

## <a name="using-images-in-c-code"></a>Görüntüleri C# kod içinde kullanma

Bir görüntü Xamarin.Mac uygulamanızda kullanarak C# kodu belleğe yüklerken, görüntü depolanır bir `NSImage` nesnesi. Görüntü dosyası (kaynaklarında dahil) Xamarin.Mac uygulama paketteki dahil edilmişse, resmi yüklemek için aşağıdaki kodu kullanın:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Yukarıdaki kod statik kullanır `ImageNamed("...")` yöntemi `NSImage` verilen görüntü bellekten yüklemek için sınıf **kaynakları** görüntü bulunamazsa klasörü `null` döndürülür. İsteyip arabirimi Oluşturucusu'nda, atanan görüntüleri dahil ettiğiniz bir  **@2x**  Görüntü dosyanızın sürümü, onu otomatik olarak kullanılacak üzerinde Retina görüntü tabanlı Mac'ler.

Uygulama paketi (Mac dosya sisteminden) dışında yansımaları yüklemek için aşağıdaki kodu kullanın:

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Şablon görüntüleri ile çalışma

MacOS uygulamanızı tasarımını bağlı olarak, simge veya görüntü renk düzenini (örneğin, kullanıcı tercihlerine göre) bir değişiklik eşleştirmek için kullanıcı arabirimi içinde özelleştirme gerektiğinde zamanlar olabilir.

Bu etkiyi elde etmek için geçiş _işleme modunu_ görüntü varlığınız, **şablon görüntüsü**:

[![Şablon görüntüsü ayarı](image-images/templateimage01.png "şablon görüntüsü ayarlama")](image-images/templateimage01-large.png#lightbox)

Xcode's arabirimi Oluşturucusu'ndan Görüntü varlığı UI denetimi ata:

![Xcode'nın arabirimi Oluşturucusu'nda bir görüntüyü seçerek](image-images/templateimage02.png "Xcode'nın arabirimi Oluşturucu'da görüntü seçme")

Veya isteğe bağlı olarak kod görüntü kaynağı ayarlayın:

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Aşağıdaki genel işlevi, görünüm denetleyiciye ekleyin:

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

Son olarak, bir şablon görüntüsü renk tonu için renklendirme görüntüye karşı bu işlevini çağırın:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>Tablo görünümleri ile görüntüleri kullanma

Hücrenin bir parçası olarak bir resim eklemek için bir `NSTableView`, tablo görünümün tarafından döndürülen veriler nasıl değiştirmeniz gerekir `NSTableViewDelegate's` `GetViewForItem` yöntemini kullanmak üzere bir `NSTableCellView` tipik yerine `NSTextField`. Örneğin:

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

Burada ilginizi çeken birkaç satır vardır. İlk olarak, biz görüntüyü dahil etmek istediğiniz sütun için yeni bir oluşturuyoruz `NSImageView` gerekli boyutunu ve konumunu ayrıca yeni oluşturduğumuz `NSTextField` ve varsayılan konumuna desteklemediğini görüntüyü kullanıyoruz üzerinde temel yerleştirin:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

İkincisi, en yeni resim görünümü ve metin alanı üst eklemek ihtiyacımız `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Son olarak, metin alanı, daraltma ve böylelikle Tablo görünümü hücresi büyümesine bildirmek ihtiyacımız var:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Örnek çıktı:

[![Bir uygulamada bir görüntü görüntüleme örneği](image-images/tables01.png "bir uygulamada bir görüntü görüntüleme örneği")](image-images/tables01-large.png#lightbox)

Tablo görünümler ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [tablosu görünümleri](~/mac/user-interface/table-view.md) belgeleri.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>Anahat görünümlerle görüntüleri kullanma

Hücrenin bir parçası olarak bir resim eklemek için bir `NSOutlineView`, anahat görünümün tarafından döndürülen veriler nasıl değiştirmeniz gerekir `NSTableViewDelegate's` `GetView` yöntemini kullanmak üzere bir `NSTableCellView` tipik yerine `NSTextField`. Örneğin:

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

Burada ilginizi çeken birkaç satır vardır. İlk olarak, biz görüntüyü dahil etmek istediğiniz sütun için yeni bir oluşturuyoruz `NSImageView` gerekli boyutunu ve konumunu ayrıca yeni oluşturduğumuz `NSTextField` ve varsayılan konumuna desteklemediğini görüntüyü kullanıyoruz üzerinde temel yerleştirin:

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

İkincisi, en yeni resim görünümü ve metin alanı üst eklemek ihtiyacımız `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Son olarak, metin alanı, daraltma ve böylelikle Tablo görünümü hücresi büyümesine bildirmek ihtiyacımız var:

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Örnek çıktı:

[![Örnek bir anahat görünümünde görüntülenen görüntünün](image-images/outline01.png "anahat görünümünde görüntülenen görüntünün örneği")](image-images/outline01-large.png#lightbox)

Anahat görünümler ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [anahat görünümleri](~/mac/user-interface/outline-view.md) belgeleri.


## <a name="summary"></a>Özet

Bu makalede, görüntüler ve simgeler Xamarin.Mac uygulamada çalışma ayrıntılı bir bakış sürdü. Biz farklı türleri gördünüz ve görüntüleri, görüntüler ve simgeler Xcode'nın arabirimi Oluşturucusu'nda nasıl kullanılacağı ve C# kodunda görüntüler ve simgeler ile çalışmak nasıl kullanır.



## <a name="related-links"></a>İlgili bağlantılar

- [MacImages (örnek)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Tablo görünümleri](~/mac/user-interface/table-view.md)
- [Anahat görünümleri](~/mac/user-interface/outline-view.md)
- [macOS X İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [OS X için yüksek çözünürlüğü hakkında](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
