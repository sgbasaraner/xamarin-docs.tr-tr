---
title: Xamarin.Mac'te yapıştırın
description: Bu makale, kopyalama sağlamak ve bir Xamarin.Mac uygulamasında yapıştırmak için çalışma alanı ile çalışmayı kapsar. Nasıl çalışacağınızı göstermektedir standart veri türleriyle birden fazla uygulama ve belirli bir uygulamanın içinde özel veri desteklemek nasıl arasında paylaşılabilir.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 728e0264f7da8f3adfef360dd473772dd7e28a11
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780587"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Xamarin.Mac'te yapıştırın

_Bu makale, kopyalama sağlamak ve bir Xamarin.Mac uygulamasında yapıştırmak için çalışma alanı ile çalışmayı kapsar. Nasıl çalışacağınızı göstermektedir standart veri türleriyle birden fazla uygulama ve belirli bir uygulamanın içinde özel veri desteklemek nasıl arasında paylaşılabilir._

## <a name="overview"></a>Genel Bakış

C# ve .NET ile bir Xamarin.Mac uygulamasında çalışırken, Objective-C içinde çalışan bir geliştirici olan aynı çalışma alanı (Kopyala ve Yapıştır) destek erişiminiz vardır.

Bu makalede şu çalışma alanına bir Xamarin.Mac uygulamasını kullanmak için iki ana yolu kapsayan:

1. **Standart veri türleri** -çalışma alanı işlemleri genellikle iki ilişkisiz uygulamalar arasında gerçekleştirilen bu yana hiçbir uygulama destekler diğer veri türlerini bilir. Paylaşım için olası en üst düzeye çıkarmak için çalışma alanına (standart ortak veri türleri kümesi kullanarak), belirli bir öğe birden çok temsillerini tutabilir, bu izin alıcı uygulamanın kendi gereksinimleriniz için en uygun sürümünü seçin.
2. **Özel veri** - kopyalama ve yapıştırma karmaşık veri çalışma alanı tarafından işlenecek bir özel veri türü tanımlayabilirsiniz, Xamarin.Mac içinde desteklemek için. Örneğin, bir vektör çizim uygulama, kullanıcının birden çok veri türleri ve noktaları oluşan karmaşık şekiller kopyalayıp izin verir.

[![Çalışan bir uygulama örneği](copy-paste-images/intro01.png "çalıştıran uygulama örneği")](copy-paste-images/intro01-large.png#lightbox)

Bu makalede, biz çalışma alanında destek kopyalama ve yapıştırma işlemleri için bir Xamarin.Mac uygulamasını ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve UI öğeleri, C# sınıfları kablo kullanılır.

## <a name="getting-started-with-the-pasteboard"></a>Çalışma alanı ile çalışmaya başlama

Çalışma alanı, belirli bir uygulamayı veya uygulamaları arasında veri değişimi için standartlaştırılmış bir mekanizma sunar. Diğer işlemlerin sayısı (Sürükle & bırak ve uygulama hizmetleri gibi) da desteklenir ancak bir çalışma alanında bir Xamarin.Mac uygulamasını için genel kullanım işlemek kopyalama ve yapıştırma işlemleriyle, sağlamaktır.

Hızlı bir şekilde buluta taşıyın için pasteboards bir Xamarin.Mac uygulamasını kullanarak bir basit, pratik giriş başlatmak için kullanacağız. Daha sonra çalışma alanına nasıl çalıştığını ve kullanılan yöntemleri ayrıntılı bir açıklama sağlarız.

Bu örnekte, biz içeren bir görüntü görünümü bir pencere yöneten bir basit bir belge tabanlı uygulama oluşturma. Kullanıcı, resim uygulamasında ya da diğer uygulamalar veya aynı uygulama içinde birden çok windows belgeler arasında kopyalama ve yapıştırma mümkün olacaktır.

### <a name="creating-the-xamarin-project"></a>Xamarin projesi oluşturma

Olması kopyalama ekleme ve ederiz yapıştırma desteği için yeni bir alan belgesi Xamarin.Mac uygulamasını oluşturmak için ilk olarak kullanacağız.

Aşağıdakileri yapın:

1. Mac ve tıklatın için Visual Studio'yu başlatın **yeni proje...**  bağlantı.
2. Seçin **Mac** > **uygulama** > **Cocoa uygulaması**, ardından **sonraki** düğmesi: 

    [![Yeni bir Cocoa uygulaması projesi oluşturma](copy-paste-images/sample01.png "yeni Cocoa uygulaması projesi oluşturma")](copy-paste-images/sample01-large.png#lightbox)
3. Girin `MacCopyPaste` için **proje adı** ve varsayılan her şey tutun. İleri'ye tıklayın: 

    [![Proje adını ayarlama](copy-paste-images/sample01a.png "projesinin adını ayarlama")](copy-paste-images/sample01a-large.png#lightbox)

4. Tıklayın **Oluştur** düğmesi: 

    [![Yeni Proje ayarları onaylanıyor](copy-paste-images/sample02.png "yeni proje ayarları doğrulanıyor")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Bir NSDocument Ekle

Özel sonraki ekleyeceğiz `NSDocument` uygulamanın kullanıcı arabirimini arka plan depolama görecek sınıfı. Bu, tek bir görüntü görünüm içerir ve görüntü varsayılan çalışma alanına görünümüne kopyalayın ve varsayılan çalışma alanına bir görüntüden alıp görüntü görünümünde görüntülemek bilmeniz.

Xamarin.Mac projeye sağ tıklayarak **çözüm bölmesi** seçip **Ekle** > **yeni dosya...** :

![Bir NSDocument projeye eklenirken](copy-paste-images/sample03.png "bir NSDocument projeye ekleniyor")

Girin `ImageDocument` için **adı** tıklatıp **yeni** düğmesi. Düzen **ImageDocument.cs** sınıfı ve aşağıdaki gibi görünmesi:

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

Bir kod bazıları aşağıda ayrıntılı olarak bakalım.

Aşağıdaki kod bir görüntü varsa görüntü verileri varsayılan çalışma alanı üzerinde varlığını test etmek için bir özellik sağlar `true` başka döndürülür `false`:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

Aşağıdaki kod, varsayılan çalışma alanına eklenen resmin görünüme bir görüntü kopyalar:

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

Ve aşağıdaki kodu bir görüntüden varsayılan çalışma alanına yapıştırabilir ve (çalışma alanı geçerli bir görüntü varsa) eklenen resmin görünümünde görüntüler:

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

Xamarin.Mac uygulama için kullanıcı arabirimi bu belge yerinde oluşturacağız.

### <a name="building-the-user-interface"></a>Kullanıcı arabirimi oluşturma

Çift **Main.storyboard** dosyasını Xcode'da açın. Ardından, bir araç çubuğu ve görüntü de ekleyin ve bunları aşağıdaki gibi yapılandırın:

[![Araç düzenleme](copy-paste-images/sample04.png "düzenleme araç çubuğu")](copy-paste-images/sample04-large.png#lightbox)

Bir kopya ekleyin ve yapıştırın **görüntü araç çubuğu öğesi** sol tarafına araç. Bu kısayollar olarak Düzenle Menüsü'nden kopyalayıp kullanacağız. Ardından, dört ekleyin **görüntü araç çubuğu öğelerini** araç çubuğunun sağ tarafına. Bu görüntünün bazı varsayılan görüntüleri ile doldurmak için kullanacağız.

Araç çubukları ile çalışma hakkında daha fazla bilgi için lütfen bkz. bizim [araç çubukları](~/mac/user-interface/toolbar.md) belgeleri.

Ardından, aşağıdaki çıkışlar ve Eylemler için sunduğumuz araç çubuğu öğeleri ve görüntü şimdi de kullanıma:

[![Çıkışlar veya Eylemler oluşturma](copy-paste-images/sample05.png "çıkışlar veya Eylemler oluşturma")](copy-paste-images/sample05-large.png#lightbox)

Çıkışlar ve Eylemler ile çalışma hakkında daha fazla bilgi için lütfen bkz [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) bölümünü bizim [Merhaba, Mac](~/mac/get-started/hello-mac.md) belgeleri.

### <a name="enabling-the-user-interface"></a>Kullanıcı arabirimini etkinleştirme

Xcode ve çıkışlar ve eylemleri kullanıma sunduğumuz UI öğesi içinde oluşturulan kullanıcı arabirimimizi ile kullanıcı arabirimini etkinleştirmek üzere kod eklemek ihtiyacımız var. Çift **ImageWindow.cs** dosyası **çözüm bölmesi** ve aşağıdaki gibi görünmesi:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

Aşağıda ayrıntılı olarak bu kodu göz atalım.

İlk olarak biz örneği kullanıma `ImageDocument` yukarıda oluşturduğumuz sınıfı:

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

Kullanarak `Export`, `WillChangeValue` ve `DidChangeValue`, Kurulum sahibiz `Document` izin vermek için anahtar-değer kodlaması ve veri bağlama xcode'da özelliği.

Ayrıca kullanıma sunuyoruz görüntünün görüntüden iyi Arabirimimizi xcode'da aşağıdaki özellik ekledik:

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

Ana pencerenin yüklendi ve görüntülenen bir örneğini oluşturacağız. bizim `ImageDocument` sınıfı ve kullanıcı Arabiriminin görüntü da aşağıdaki kod ile ekleyebilirsiniz:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

Son olarak, kopyalama ve yapıştırma araç çubuğu öğeleri tıklayarak kullanıcı yanıt olarak, örneğini diyoruz `ImageDocument` asıl işi yapmak için sınıfı:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Dosya ve düzenleme menüleri etkinleştirme

Yapmamız gereken son etkinleştirme ise **yeni** menü öğesi **dosya** menü (bizim ana pencerenin yeni örnekleri oluşturmak için) ve etkinleştirmek için **Kes**, **kopyalama**  ve **Yapıştır** menü öğelerini gelen **Düzenle** menüsü.

Etkinleştirmek için **yeni** Düzenle menüsü öğesi, **AppDelegate.cs** dosyasını açıp aşağıdaki kodu ekleyin:

```csharp
public int UntitledWindowCount { get; set;} =1;
...

[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Daha fazla bilgi için lütfen bkz [birden çok Windows ile çalışan](~/mac/user-interface/window.md) bölümünü bizim [Windows](~/mac/user-interface/window.md) belgeleri.

Etkinleştirmek için **Kes**, **kopyalama** ve **Yapıştır** menü öğelerini düzenle **AppDelegate.cs** dosyasını açıp aşağıdaki kodu ekleyin:

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

Her bir menü öğesi için biz geçerli, en üstte, anahtar penceresi alın ve bunu bizim `ImageWindow` sınıfı:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

Buradan diyoruz `ImageDocument` işlemek kopyalama ve yapıştırma penceresinin sınıf örneği. Örneğin: 

```csharp
window.Document.CopyImage (sender);
```

Yalnızca istediğimiz **Kes**, **kopyalama** ve **Yapıştır** menü öğeleri varsa erişilebilir olması için varsayılan çalışma alanına veya geçerli etkin pencereyi görüntüsü de görüntü.

Ekleyelim bir **EditMenuDelegate.cs** Xamarin.Mac projeye dosya ve aşağıdaki gibi görünmesi:

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

Yeniden geçerli, üstteki pencere elde etmek ve kullanırız kendi `ImageDocument` gerekli resim verileri olup olmadığını görmek için sınıf örneği. Sonra da kullandığımız `MenuWillHighlightItem` etkinleştirmek veya devre dışı her öğe için yöntem tabanlı bu durumu.

Düzen **AppDelegate.cs** dosya ve olun `DidFinishLaunching` yöntemi görünüm aşağıdaki gibi:
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

İlk olarak, Otomatik etkinleştirme ve Düzen menüsündeki menü öğelerinin devre dışı bırakma devre dışı bırakırız. Ardından, biz bir örneğini ekleme `EditMenuDelegate` yukarıda oluşturduğumuz sınıfı.

Daha fazla bilgi için lütfen bkz. bizim [menüleri](~/mac/user-interface/menu.md) belgeleri.

### <a name="testing-the-app"></a>Uygulamayı test etme

Olan her şeyi yerinde, uygulamayı test etmek hazırız. Derleme ve uygulamayı çalıştırın ve ana arabirimi görüntülenir:

![Uygulamayı çalıştıran](copy-paste-images/run01.png "uygulamayı çalıştırma")

Düzenleme menüsünü açın, dikkat **Kes**, **kopyalama** ve **Yapıştır** olmadığından hiçbir resim görüntüde de veya varsayılan çalışma alanı devre dışı bırakılır:

![Düzenleme menüsünü açarak](copy-paste-images/run02.png "için düzenleme menüsünü açma")

Resim görüntüsüne da ekleme ve düzenleme menüsünü açın, öğeler artık etkinleştirilecek:

![Düzen menüsü öğeleri gösteren etkin](copy-paste-images/run03.png "düzenleme menü öğelerini gösteren etkin")

Resmi kopyalayıp seçin **yeni** Dosya menüsünden Yeni bir pencerede o yansıma yapıştırabilirsiniz:

![Yeni bir pencerede bir görüntü yapıştırma](copy-paste-images/run04.png "yeni bir pencerede bir görüntü yapıştırma")

Aşağıdaki bölümlerde, biz çalışma alanında bir Xamarin.Mac uygulamasını çalışma sırasında ayrıntılı göz atacağız.

## <a name="about-the-pasteboard"></a>Çalışma alanı hakkında

MacOS (eski adıyla OS X da bilinir), çalışma alanına (`NSPasteboard`) kopyalayın ve yapıştırın, sürükle & bırak ve gibi uygulama hizmetleri, birkaç sunucu süreçleri için destek sağlar. Aşağıdaki bölümlerde, biz birkaç temel çalışma alanı kavramları daha yakından göz atacağız.

### <a name="what-is-a-pasteboard"></a>Bir çalışma alanı nedir?

`NSPasteboard` Sınıfı içinde belirli bir uygulamanın veya uygulamaların arasında bilgi alışverişi için standartlaştırılmış bir mekanizma sağlar. Kopyalama ve yapıştırma işlemleri işlemek için bir çalışma alanı ana işlevini şöyledir:

1. Kullanıcı bir uygulamada bir öğe seçer ve kullanır **Kes** veya **kopyalama** menü öğesi, çalışma alanındaki bir veya daha fazla seçili öğe temsillerini yerleştirilir.
2. Kullanıcı kullandığında **Yapıştır** menü öğesi (aynı uygulama ya da farklı bir içinde) sürümü, işleyebileceği veri çalışma kopyalanır ve uygulamaya eklenir.

İşlem uygulaması hizmetleri ve daha az belirgin bir çalışma alanı kullanır Bul sürükleyin, sürükle ve bırak içerir:

- Kullanıcı, bir sürükleme işlemi başlattığında, çalışma alanındaki Sürükle veri kopyalanır. Sürükleme işlemi başka bir uygulamanın üzerine bir bırakma ile bitiyorsa, bu uygulama, veri çalışma kopyalar.
- Çeviri hizmetleri için çalışma alanındaki isteyen uygulama tarafından çevrilmesi için verileri kopyalanır. Uygulama hizmeti, çalışma alanı ' verileri alır, yapıştırır çalışma alanındaki veri geri çevirme, yapar.

Kendi basit haliyle, belirli bir uygulamanın içinde veya uygulamalar arasında veri taşıma ve uygulamanın işlem dışında bir özel genel bellek alanı therefor mevcut pasteboards kullanılır. Pasteboards kavramlarını kolayca çalışırken grasps, dikkate alınması gereken birkaç daha karmaşık ayrıntıları vardır. Bunlar aşağıda ayrıntılı olarak ele alınacaktır.

### <a name="named-pasteboards"></a>Adlandırılmış pasteboards

Bir çalışma alanı, genel veya özel olabilir ve çeşitli amaçlarla bir uygulama içinde veya birden fazla uygulama arasında kullanılabilir. macOS birkaç standart pasteboards her biri belirli ve iyi tanımlanmış bir kullanım sağlar:

- `NSGeneralPboard` -Varsayılan çalışma alanı için **Kes**, **kopyalama** ve **Yapıştır** operations.
- `NSRulerPboard` -Destekleyen **Kes**, **kopyalama** ve **Yapıştır** işlemleri **Cetveller**.
- `NSFontPboard` -Destekleyen **Kes**, **kopyalama** ve **Yapıştır** işlemleri `NSFont` nesneleri.
- `NSFindPboard` -Destekler uygulamaya özgü arama metni paylaşabilirsiniz panelleri bulun.
- `NSDragPboard` -Destekleyen **sürükleyip bırakma** operations.

Çoğu durumlarda, sistem tarafından tanımlanan pasteboards birini kullanacaksınız. Ancak, kendi pasteboards oluşturmanızı gerektiren durumlar olabilir. Bu durumda, kullandığınız `FromName (string name)` yöntemi `NSPasteboard` verilen ada sahip bir özel çalışma alanı oluşturmak için sınıf.

İsteğe bağlı olarak çağırabilirsiniz `CreateWithUniqueName` yöntemi `NSPasteboard` benzersiz olarak adlandırılmış bir çalışma alanı oluşturmak için sınıf.

### <a name="pasteboard-items"></a>Çalışma alanı öğeleri

Her bir uygulama için bir çalışma alanı Yazar veri parçası olarak kabul edilir bir _çalışma öğesi_ ve bir çalışma alanı, aynı anda birden çok öğe içerebilir. Bu şekilde, bir çalışma alanı (örneğin, düz metin ve biçimlendirilmiş metin) ve alma uygulama kopyalanan verileri birden çok sürümünü (yalnızca düz metin gibi) işleyebilir yalnızca devre dışı verileri okuyabilir bir uygulama yazabilirsiniz.

### <a name="data-representations-and-uniform-type-identifiers"></a>Veri gösterimler ve Tekdüzen tür tanımlayıcıları

Çalışma alanı işlemleri genellikle olması birbirleriyle veya veri türleri hakkında bilgiye sahip iki (veya daha fazlası) arasında bir yerde uygulamalar her işleyebilir. Bir çalışma alanı, bilgi paylaşımı için olası en üst düzeye çıkarmak için yukarıdaki bölümde belirtildiği gibi birden fazla kopyalanan ve yapıştırılan verilerin temsillerini basılı tutabilirsiniz.

Aracılığıyla bir Tekdüzen türü tanımlayıcısı (sunulmasını tarih türü benzersiz olarak tanımlayan basit bir dize fazlası olan UTI), her gösterimi tanımlanır (daha fazla bilgi için Apple'nın bkz [Tekdüzen tür tanımlayıcıları genel bakış ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) belgeleri). 

Bir özel veri türü (örneğin, bir vektör çizim uygulama çizim nesnesine) oluşturuyorsanız, benzersiz olarak kopyalama tanımlamak ve yapıştırma işlemleri için kendi UTI oluşturabilirsiniz.

Uygulama, bir çalışma kopyalanan verileri yapıştırmak hazırlanırken (varsa), yeteneklerini gereksinimlerine temsili bulmanız gerekir. Genellikle bu fikirlerini kullanılabilen türü (örneğin biçimlendirilmiş metin bir sözcük işleme uygulaması için), olacaktır kullanılabilir gerekli (düz metin olarak basit bir metin düzenleyicisi için) basit form dönülüyor.

<a name="Promised_Data" />

### <a name="promised-data"></a>Taahhüt edilen veri

Genel olarak bakıldığında, uygulamalar arasında paylaşma en üst düzeye çıkarmak için mümkün olduğunca kopyalanan verileri çok temsillerini sağlamanız gerekir. Ancak, saati veya bellek kısıtlamaları nedeniyle her veri türü ile çalışma alanına gerçekten yazmak için pratik olmayabilir.

Bu durumda, çalışma alanındaki ilk veri temsilini yerleştirebilirsiniz ve alıcı uygulamanın oluşturulan üzerinde halindeyken yapıştırma işlemi hemen önce olabilir farklı bir temsili, talep edebilir.

İlk öğeyi çalışma alanında yerleştirdiğinizde, bir veya daha fazla kullanılabilir diğer gösterimleri uyan bir nesne tarafından sağlanan belirteceği `NSPasteboardItemDataProvider` arabirimi. Bu nesneler alıcı uygulama tarafından istenen şekilde isteğe bağlı olarak ek gösterimleri sağlayacaktır.

### <a name="change-count"></a>Değiştirme sayısı

Her çalışma alanı tutan bir _değiştirme sayısı_ artışlarla her yeni bir sahip zaman bildirilir. Bir uygulama çalışma alanı'nın içeriği, bunu, değiştirme sayısı değerini denetleyerek incelenirken en son ne zaman beri değiştirdiyseniz belirleyebilirsiniz.

Kullanım `ChangeCount` ve `ClearContents` yöntemlerinin `NSPasteboard` verilen çalışma alanında 's değişiklik sayısını değiştirmek için sınıf.

## <a name="copying-data-to-a-pasteboard"></a>Bir çalışma alanındaki veri kopyalama

İlk çalışma alanında erişerek, var olan tüm içeriği temizlemek ve yazma çalışma alanındaki gerekli olduğu gibi veri gibi çok sayıda temsillerini kopyalama işlemini gerçekleştirin.

Örneğin:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

Yukarıdaki örnekte uyguladığımız gibi genellikle yalnızca genel çalışma alanı için yazdığınız. İçin gönderdiğiniz herhangi bir nesne `WriteObjects` yöntemi *gerekir* uygun `INSPasteboardWriting` arabirimi. Birkaç, yerleşik sınıfları (gibi `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, ve `NSPasteboardItem`) Bu arabirim için otomatik olarak uygun.

Çalışma alanındaki özel bir veri sınıfı yazıyorsanız uymak zorundadır `INSPasteboardWriting` arabirim veya örneğinde sarmalamak `NSPasteboardItem` sınıfı (bkz [özel veri türleri](#Custom_Data_Types) bölümüne bakın).

## <a name="reading-data-from-a-pasteboard"></a>Bir çalışma alanından verileri okuma

Yukarıda belirtildiği gibi uygulamalar arasında veri paylaşımı için olası en üst düzeye çıkarmak için birden fazla kopyalanan verilerin temsillerini çalışma alanındaki yazılabilir. En fazla (varsa) yeteneklerini için olası en zengin sürümü seçin alıcı uygulamasıdır.

### <a name="simple-paste-operation"></a>Basit yapıştırma işlemi

Çalışma alanı kullanarak verileri okumak `ReadObjectsForClasses` yöntemi. Bu iki parametre gerektirir:

1. Bir dizi `NSObject` çalışma okumak istediğiniz sınıf türleri tabanlı. Bu en çok istenen veri türüne sahip ilk olarak, tercih azaltılması, kalan tüm türleri ile sipariş.
2. (Örneğin, belirli URL içerik türleri için sınırlama) ek kısıtlamalar içeren bir sözlük veya başka hiçbir kısıtlama gerekiyorsa boş bir sözlük.

Yöntem, geçirilen şu ölçütlere uyan öğeleri bir dizi döndürür ve bu nedenle en çok talep edilen veri türleri aynı sayıda içerir. İstenen türlerden hiçbiri mevcut olduğundan ve boş bir dizi döndürülür mümkündür.

Örneğin, aşağıdaki denetler kod bir `NSImage` genel çalışma alanında var ve varsa bir görüntü iyi görüntüler:

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>Birden çok veri türü isteme

Oluşturulan Xamarin.Mac uygulama türüne bağlı olarak, birden çok yapıştırılmakta olan verilerin temsillerini işlemek mümkün olabilir. Bu durumda, çalışma alanı ' veri almak için iki senaryo vardır:

1. Tek bir çağrı yapmak `ReadObjectsForClasses` yöntem ve (tercih edilen sırayla) bağlamasına gösterimleri bir dizi sağlama.
2. Birden çok çağrı yapmak `ReadObjectsForClasses` soran farklı bir dizi için bir yöntem her zaman türleri.

Bkz: **basit yapıştırma işlemi** bölümünde yukarıda bir çalışma veri alma hakkında daha fazla bilgi.

### <a name="checking-for-existing-data-types"></a>Var olan veri türleri için denetleniyor

Çalışma gerçekten veri okumadan çalışma alanında belirtilen veri temsilini içerip içermediğini denetlemek için isteyebileceğiniz durumlar (etkinleştirme gibi **Yapıştır** yalnızca geçerli veri mevcut olduğunda menü öğesi).

Çağrı `CanReadObjectForClasses` verilen tür içerip içermediğini görmek için çalışma alanına yöntemi.

Örneğin, aşağıdaki kod genel çalışma alanı içerip içermediğini belirler. bir `NSImage` örneği:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>Çalışma alanı URL'lerden okuma

Bunlar belirli ölçütleri (örneğin, dosyaları ya da belirli bir veri türüne URL'lerini işaret eden) birtakım karşılıyorsanız belirli bir Xamarin.Mac uygulamasını işleticini bağlı olarak, bir çalışma URL'leri okumak için gerekli, ancak yalnızca olabilir. Bu durumda, ikinci parametresi kullanarak ek ölçüt belirtebilirsiniz `CanReadObjectForClasses` veya `ReadObjectsForClasses` yöntemleri.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Özel veri türleri

Ne zaman kendi özel türlerinizde çalışma alanındaki bir Xamarin.Mac uygulamasını kaydetme gerekecektir zamanlar vardır. Örneğin, kopyalama ve yapıştırma çizim nesneleri kullanıcıya izin veren bir uygulama çizim vektör.

Bu durumda, veri özel sınıfınıza öğesinden devralacak şekilde tasarlayın gerekecektir `NSObject` ve birkaç arabirimlere uygun (`INSCoding`, `INSPasteboardWriting` ve `INSPasteboardReading`). İsteğe bağlı olarak kullanabileceğiniz bir `NSPasteboardItem` kopyalanabilir veya yapıştırılan verilerin yalıtılacak.

Bu iki seçenek aşağıda ayrıntılı olarak ele alınacaktır.

### <a name="using-a-custom-class"></a>Özel bir sınıf kullanma

Bu bölümde Biz bu belgenin başlangıcında oluşturduğumuz basit örnek uygulamasında genişletme ve windows arasında biz kopyalama ve yapıştırma görüntü ile ilgili bilgileri izlemek için özel sınıf ekleme.

Projeye yeni bir sınıf ekleyin ve onu çağırmak **ImageInfo.cs**. Dosyasını düzenleyin ve aşağıdaki gibi görünmesi:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

Aşağıdaki bölümlerde Biz bu sınıf, ayrıntılı göz atacağız.

#### <a name="inheritance-and-interfaces"></a>Devralma ve arabirimleri

Özel bir veri sınıfı yazılan veya bir çalışma okuma önce uygun olmalıdır `INSPastebaordWriting` ve `INSPasteboardReading` arabirimleri. Ayrıca, öğesinden devralmalıdır `NSObject` ve ayrıca için uymak `INSCoding` arabirimi:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Sınıfı, Objective-C sunulmalıdır kullanarak `Register` yönergesi ve gerekir kullanıma tüm gerekli özellikleri veya yöntemleri kullanarak `Export`. Örneğin:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Biz bu sınıfın içereceği - veri görüntünün adını ve türünü (jpg, png, vb.) iki alan bırakıyorsunuz. 

Daha fazla bilgi için [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) belgeleri, açıklar `Register` ve `Export` öznitelikleri Objective-C nesneleri ve UI öğeleri, C# sınıfları kablo kullanılır.

#### <a name="constructors"></a>Oluşturucular

Bir çalışma okuyun, böylece (düzgün bir şekilde kullanıma sunulan Objective-C) iki Oluşturucu, özel veri sınıfı için gerekli olacaktır:

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

İlk olarak, size kullanıma _boş_ Oluşturucusu altında varsayılan Objective-C yöntemi `init`.

Kullanıma sunuyoruz ardından, bir `NSCoding` verilen adının altında yapıştırırken çalışma nesnesinin yeni bir örneğini oluşturmak için kullanılacak uyumlu Oluşturucusu `initWithCoder`.

Bu oluşturucu alır bir `NSCoder` (tarafından oluşturulan bir `NSKeyedArchiver` çalışma alanındaki yazılırken), anahtar/değer eşleştirilmiş verileri ayıklayan ve veri sınıfın özellik alanları kaydeder.

#### <a name="writing-to-the-pasteboard"></a>Çalışma alanına yazma

Uyumludur tarafından `INSPasteboardWriting` arabirimi ihtiyacımız iki yöntem ve isteğe bağlı üçüncü bir yöntem kullanıma sunmak ve böylece sınıf çalışma alanındaki yazılabilir.

İlk olarak biz çalışma alanı özel bir sınıf için yazılabilir gösterimleri hangi verilerin yazın söylemeniz gerekir:

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Aracılığıyla bir Tekdüzen türü tanımlayıcısı (sunulan veri türü benzersiz olarak tanımlayan basit bir dize fazlası olan UTI), her gösterimi tanımlanır (daha fazla bilgi için Apple'nın bkz [Tekdüzen tür tanımlayıcıları genel bakış ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) belgeleri).

Bizim özel biçim için kendi UTI oluşturuyoruz: "com.xamarin.image-bilgi" (olduğu gibi bir uygulama tanımlayıcısı geriye doğru gösterimde olduğuna dikkat edin). Bizim sınıfı ayrıca çalışma alanındaki standart bir dize yazmak yeteneğine sahiptir (`public.text`). 

Ardından, biz, çalışma alanındaki gerçekte yazılan istenen biçimde nesnesi oluşturmanız gerekir:

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

İçin `public.text` türü, biz döndürüyor basit, biçimlendirilmiş `NSString` nesne. Özel `com.xamarin.image-info` kullanıyoruz türü, bir `NSKeyedArchiver` ve `NSCoder` özel veri sınıfı için bir anahtar/değer eşleştirilmiş arşiv kodlamak için arabirim. Aslında kodlama işlemek için aşağıdaki yöntemi uygulamak gerekecektir:

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Tek tek anahtar/değer çiftleri kodlayıcıya yazılır ve yukarıda eklediğimiz ikinci oluşturucu kullanılarak kodu çözülen.

İsteğe bağlı olarak, şu seçeneklerden çalışma alanındaki veri yazarken tanımlamak için aşağıdaki yöntemi şunları içerebilir:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Şu anda yalnızca `WritingPromised` seçenek kullanılabilir ve belirli bir tür yalnızca taahhüt ve çalışma alanındaki gerçekten yazılan olduğunda kullanılmalıdır. Daha fazla bilgi için lütfen bkz [taahhüt veri](#Promised_Data) yukarıdaki bölümde.

Bu yöntemlerle yerinde bizim özel bir sınıf için çalışma alanına yazmak için aşağıdaki kodu kullanılabilir:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Çalışma alanı veri okuma

Uyumludur tarafından `INSPasteboardReading` arabirimi ihtiyacımız özel veri sınıfı çalışma okuyabilirsiniz üç yöntem kullanıma sunmak.

İlk olarak biz çalışma alanı özel bir sınıf panodan okuyabilirsiniz gösterimleri hangi verilerin yazın söylemeniz gerekir:

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Yine, bunlar basit Utı'leri tanımlanır ve tanımladığımız aynı türleridir **yazma çalışma alanındaki** yukarıdaki bölümde.

Ardından, çalışma alanı bildirmek ihtiyacımız _nasıl_ UTI türlerinin her biri aşağıdaki yöntemi kullanılarak okunur:

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

İçin `com.xamarin.image-info` türü, biz belirten çalışma alanı ile oluşturduğumuz anahtar/değer çifti çözülecek `NSKeyedArchiver` zaman yazma sınıfı çalışma alanındaki çağırarak `initWithCoder:` sınıfa ekledik Oluşturucusu.

Son olarak, biz çalışma bir UTI veri gösterimleri okumak için aşağıdaki yöntemi eklemeniz gerekir:

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

Tüm bu yöntemleri ile yerinde, özel veri sınıfı aşağıdaki kodu kullanarak çalışma alanındaki okuyabilirsiniz:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>Bir NSPasteboardItem kullanma

Ne zaman özel öğeler için özel bir sınıf oluşturulmasını garanti değil çalışma alanı için yazmanız gereken veya verileri yalnızca gerektiği yaygın bir biçimde sağlamak istediğiniz zamanlar olabilir. Bu durumlarda, kullandığınız bir `NSPasteboardItem`.

A `NSPasteboardItem` ayrıntılı denetim sağlar çalışma alanındaki yazıldıktan sonra çalışma alanındaki yazılır ve geçici erişim için - tasarlanan veriler üzerinde bu, atılmalıdır.

#### <a name="writing-data"></a>Veri yazma

Özel verilerinizi yazmak için bir `NSPasteboardItem` özel sağlamak ihtiyacınız olacak `NSPasteboardItemDataProvider`. Projeye yeni bir sınıf ekleyin ve onu çağırmak **ImageInfoDataProvider.cs**. Dosyasını düzenleyin ve aşağıdaki gibi görünmesi:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

Özel veri sınıfıyla yaptığımız gibi kullanılacak ihtiyacımız `Register` ve `Export` Objective-c göstermek için yönergeleri Sınıf devralmalıdır `NSPasteboardItemDataProvider` ve uygulamalıdır `FinishedWithDataProvider` ve `ProvideDataForType` yöntemleri.

Kullanım `ProvideDataForType` yöntemi içinde sarmalanmış verilerini sağlamak için `NSPasteboardItem` gibi:

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

Bu durumda, biz görüntümüzü (adı ve ImageType) hakkında bilgilerin iki parça depolamak ve bu basit bir dizeye yazma (`public.text`).

Tür veri yazma çalışma alanı için aşağıdaki kodu kullanın:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>Verileri okuma

Yeniden çalışma verileri okumak için aşağıdaki kodu kullanın:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>Özet

Bu makalede, çalışma alanında bir Xamarin.Mac uygulamasını destekleyen kopyalama ve yapıştırma işlemleri için çalışan bir ayrıntılı bakış duruma getirdi. İlk olarak, standart pasteboards işlemleriyle tanımak için basit bir örnek sunulmaktadır. Ardından, çalışma alanı ve okuma ve ondan veri yazma konusunda ayrıntılı bilgi işlem. Son olarak, kopyalama ve yapıştırma bir uygulama içinde karmaşık veri türlerini desteklemek için bir özel veri türü kullanarak görünüyordu.



## <a name="related-links"></a>İlgili bağlantılar

- [MacCopyPaste (örnek)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Çalışma alanı Programlama Kılavuzu](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
