---
title: Kopyala ve Yapıştır
description: Bu makalede kopyalama sağlamak ve bir Xamarin.Mac uygulamasında yapıştırmak için çalışma alanı'nı çalışmak kapsar. Nasıl çalışacağınız gösterilmektedir standart veri türleriyle birden çok uygulama ve belirli bir uygulamanın içinden özel veri desteklemek nasıl arasında paylaşılabilir.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: cf81666403f687ce997e20f6f5f097dc9fcf1421
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="copy-and-paste"></a>Kopyala ve Yapıştır

_Bu makalede kopyalama sağlamak ve bir Xamarin.Mac uygulamasında yapıştırmak için çalışma alanı'nı çalışmak kapsar. Nasıl çalışacağınız gösterilmektedir standart veri türleriyle birden çok uygulama ve belirli bir uygulamanın içinden özel veri desteklemek nasıl arasında paylaşılabilir._

## <a name="overview"></a>Genel Bakış

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, Objective-C çalışan bir geliştirici sahip aynı çalışma alanı (Kopyala ve Yapıştır) destek erişiminiz vardır.

Bu makalede biz çalışma alanına Xamarin.Mac uygulamada kullanmak için iki ana yolu kapsayan:

1. **Standart veri türleri** -çalışma alanı işlemleri genellikle iki ilişkisiz uygulamalar arasında gerçekleştirilen olduğundan, diğer destekleyen veri türlerini hiçbiri uygulama bilir. Paylaşım olası en üst düzeye çıkarmak için çalışma alanına (ortak veri türleri standart bir dizi kullanarak) belirli bir öğe birden çok gösterimlerini tutabilir, kendi gereksinimleriniz için en uygun sürümünü seçmek alıcı uygulamanın bu imkan tanır.
2. **Özel veri** - kopyalama ve yapıştırma çalışma alanı'nı tarafından işlenecek bir özel veri türü tanımlayabilirsiniz, Xamarin.Mac içindeki karmaşık verilerin desteklemek için. Örneğin, bir vektör çizim uygulama, kullanıcının birden çok veri türleri ve noktaları oluşan karmaşık şekiller kopyalayıp izin verir.

[![Çalışan uygulama örneği](copy-paste-images/intro01.png "çalışan uygulama örneği")](copy-paste-images/intro01-large.png#lightbox)

Bu makalede, biz çalışma alanında desteklenmesi için kopyalama ve yapıştırma işlemlerini Xamarin.Mac uygulama ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve kullanıcı Arabirimi öğeleri, C# sınıfları wire için kullanılır.

## <a name="getting-started-with-the-pasteboard"></a>Çalışma alanı ile çalışmaya başlama

Çalışma alanındaki belirli bir uygulamada veya uygulamalar arasında veri değişimi için standartlaştırılmış bir mekanizma sunar. Diğer işlemlerin sayısı da (Sürükle & açılır ve uygulama hizmetleri gibi) desteklenir ancak tipik bir çalışma alanında bir Xamarin.Mac uygulaması için işler kopyalama ve yapıştırma işlemlerini, için kullanılır.

Zemin hızlı bir şekilde almak için biz pasteboards bir Xamarin.Mac uygulamasını kullanarak bir basit, pratik giriş başlatmaya adımıdır. Daha sonra çalışma alanına nasıl çalıştığını ve kullanılan yöntemleri ayrıntılı bir açıklaması sağlarız.

Bu örnekte, biz içeren bir görüntü görünümü bir pencere yöneten bir dayalı basit bir belge uygulaması oluşturuluyor. Kullanıcı resim uygulamasında ve için veya diğer uygulamalar ya da aynı uygulama içinde birden çok windows belgeler arasında kopyalama ve yapıştırma mümkün olacaktır.

### <a name="creating-the-xamarin-project"></a>Xamarin projesi oluşturma

İlk olarak, biz biz kopya ekleme ve olması için destek yapıştırın, yeni bir temel belgenin Xamarin.Mac uygulaması oluşturmak için adımıdır.

Aşağıdakileri yapın:

1. Mac ve tıklatın için Visual Studio'yu başlatın **yeni proje...**  bağlantı.
2. Seçin **Mac** > **uygulama** > **Cocoa uygulama**, ardından **sonraki** düğmesi: 

    [![Yeni bir Cocoa uygulama projesi oluşturma](copy-paste-images/sample01.png "yeni Cocoa uygulama projesi oluşturma")](copy-paste-images/sample01-large.png#lightbox)
3. Girin `MacCopyPaste` için **proje adı** ve varsayılan olarak bir şey tutun. İleri'yi tıklatın: 

    [![Proje adını ayarlama](copy-paste-images/sample01a.png "projesinin adı ayarlama")](copy-paste-images/sample01a-large.png#lightbox)

4. Tıklatın **oluşturma** düğmesi: 

    [![Yeni proje ayarlarını onaylama](copy-paste-images/sample02.png "yeni proje ayarlarını onaylama")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Bir NSDocument ekleyin

Özel sonraki ekleyeceğiz `NSDocument` uygulamanın kullanıcı arabirimi için arka plan depolama görecek sınıfı. Bu tek bir görüntü görünüm içerir ve varsayılan çalışma alanı görünüme görüntüyü kopyalamak nasıl ve varsayılan çalışma alanı görüntüden almak ve görüntü görünümünde görüntülemek nasıl biliyorsunuz.

Xamarin.Mac projeye sağ tıklayın **çözüm paneli** seçip **Ekle** > **yeni dosya...** :

![Projeye bir NSDocument ekleme](copy-paste-images/sample03.png "bir NSDocument projesine ekleniyor")

Girin `ImageDocument` için **adı** tıklatıp **yeni** düğmesi. Düzen **ImageDocument.cs** sınıfı ve şu şekilde görünür yapın:

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

Aşağıdaki kod varsayılan çalışma alanı görüntü verilerini varlığını görüntüyü olup olmadığını sınamak için bir özellik sağlar `true` else döndürülen `false`:

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

Aşağıdaki kod varsayılan çalışma alanı ekli görüntü görünüme görüntüyü kopyalar:

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

Ve aşağıdaki kodu varsayılan çalışma alanı görüntüden gönderebilir ve (çalışma alanına geçerli bir görüntüsü içeriyorsa) bağlı görüntü görünümünde görüntüler:

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

Yerinde bu belgeyle Xamarin.Mac uygulama için kullanıcı arabirimi oluşturacağız.

### <a name="building-the-user-interface"></a>Kullanıcı arabirimi oluşturma

Çift **Main.storyboard** dosyayı Xcode'da açın. Ardından, bir araç ve görüntünün iyi ekleyebilir ve bunları aşağıdaki gibi yapılandırabilirsiniz:

[![Düzenleme araç çubuğunu](copy-paste-images/sample04.png "araç düzenleme")](copy-paste-images/sample04-large.png#lightbox)

Bir kopyası ekleyin ve Yapıştır **görüntü araç çubuğu öğesi** araç çubuğunun sol tarafında için. Biz kısayolları olarak kopyalamak ve yapıştırmak Düzen menüsünden için kullanırsınız. Ardından, dört eklemek **görüntü araç çubuğu öğeleri** araç çubuğunun sağ tarafında için. Bu görüntü bazı varsayılan görüntüleri ile iyi doldurmak için kullanacağız.

Araç çubukları ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [araç çubukları](~/mac/user-interface/toolbar.md) belgeleri.

Ardından, şimdi aşağıdaki çıkışlar ve bizim araç çubuğu öğeleri ve görüntü için Eylemler de kullanıma:

[![Çıkışlar ve eylemleri oluşturma](copy-paste-images/sample05.png "çıkışlar ve eylemleri oluşturma")](copy-paste-images/sample05-large.png#lightbox)

Çıkışlar ve Eylemler ile çalışma hakkında daha fazla bilgi için lütfen bkz [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) bölümünü bizim [Hello, Mac](~/mac/get-started/hello-mac.md) belgeleri.

### <a name="enabling-the-user-interface"></a>Kullanıcı arabirimi etkinleştirme

Xcode ve çıkışlar ve Eylemler aracılığıyla kullanıma sunulan bizim UI öğesi içinde oluşturulan bizim kullanıcı arabirimiyle biz kullanıcı arabirimini etkinleştirmek için kodu eklemeniz gerekir. Çift **ImageWindow.cs** dosyasını **çözüm paneli** ve aşağıdaki gibi görünmesi:

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

Bir bu kodu aşağıda ayrıntılı olarak bakalım.

İlk olarak, biz örneği kullanıma `ImageDocument` yukarıda oluşturduğumuz sınıfı:

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

Kullanarak `Export`, `WillChangeValue` ve `DidChangeValue`, Kurulum sahibiz `Document` anahtar-değer kodlama ve veri bağlama xcode'da izin vermek için özellik.

Biz de iyi xcode'da bizim UI için aşağıdaki özellikler ile eklediğimiz görüntü görüntüden ortaya çıkarır:

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

Ana penceresi yüklenen ve görüntülenen bir örneğini oluşturmak bizim `ImageDocument` sınıfı ve kullanıcı Arabiriminin görüntüsünü de aşağıdaki kod ile bağlayın:

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

Son olarak, Kopyala ve Yapıştır araç öğelerde tıklatarak kullanıcıya yanıtta örneğini diyoruz `ImageDocument` sınıfı asıl işi yapmak için:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Dosya ve düzenleme menüleri etkinleştirme

İhtiyacımız yapmak için son etkinleştirme şeydir **yeni** menü öğesinden **dosya** menü (bizim ana penceresinin yeni örnekleri oluşturmak için) ve etkinleştirmek için **Kes**, **Kopyala**  ve **Yapıştır** menü öğelerini **Düzenle** menüsü.

Etkinleştirmek için **yeni** menü öğesi, düzenleme **AppDelegate.cs** dosya ve aşağıdaki kodu ekleyin:

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

Daha fazla bilgi için lütfen bkz [birden çok Windows ile çalışma](~/mac/user-interface/window.md) bölümünü bizim [Windows](~/mac/user-interface/window.md) belgeleri.

Etkinleştirmek için **Kes**, **kopya** ve **Yapıştır** menü öğelerini düzenle **AppDelegate.cs** dosya ve aşağıdaki kodu ekleyin:

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

Her bir menü öğesi için biz geçerli, üstteki, anahtar penceresi almak ve hangisine bizim `ImageWindow` sınıfı:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

Buradan diyoruz `ImageDocument` kopya işlemek ve Eylemler yapıştırmak için penceresinin sınıf örneği. Örneğin: 

```csharp
window.Document.CopyImage (sender);
```

Yalnızca istiyoruz **Kes**, **kopya** ve **Yapıştır** varsa erişilebilir olmasını menü öğeleri görüntü veri varsayılan çalışma alanı ya da geçerli etkin pencereyi görüntüsü de.

Ekleyelim bir **EditMenuDelegate.cs** dosya Xamarin.Mac projeye ve şu şekilde görünür yapın:

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

Yeniden, biz geçerli, üstteki pencere almak ve kullanmak, `ImageDocument` gerekli resim verileri var olup olmadığını görmek için sınıf örneği. Kullanırız sonra `MenuWillHighlightItem` yöntemi etkinleştirmek veya devre dışı her öğe için temel bu durumda.

Düzen **AppDelegate.cs** dosya ve olun `DidFinishLaunching` yöntemi görünüm aşağıdaki gibi:
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

İlk olarak, Otomatik etkinleştirme ve düzenleme menüde menü öğelerini devre dışı bırakma devre dışı bırakırız. Ardından, biz bir örneğini ekleme `EditMenuDelegate` yukarıda oluşturduğumuz sınıfı.

Daha fazla bilgi için lütfen bkz bizim [menüleri](~/mac/user-interface/menu.md) belgeleri.

### <a name="testing-the-app"></a>Uygulamayı test etme

Her yerde biz uygulamayı test etmek hazır olursunuz. Derleme ve uygulamayı çalıştırın ve ana arabirimi görüntülenir:

![Uygulamayı çalıştıran](copy-paste-images/run01.png "uygulamayı çalıştırma")

Düzen menüsü açarsanız, unutmayın **Kes**, **kopyalama** ve **Yapıştır** olduğu için hiçbir resim görüntüde iyi veya varsayılan çalışma alanı'nı devre dışı bırakılır:

![Düzen menüsü açma](copy-paste-images/run02.png "Düzen menüsünü açma")

İyi bir görüntü ekleyebileceğiniz ve Düzen menüsünü açın, öğeleri artık etkin:

![Düzen menüsü öğeleri gösteren etkin](copy-paste-images/run03.png "düzenleme menü öğeleri gösteren etkin")

Görüntü kopyalayıp seçin **yeni** Dosya menüsünden Yeni bir pencerede, görüntü yapıştırabilirsiniz:

![Yeni bir pencerede görüntüyü yapıştırma](copy-paste-images/run04.png "görüntüyü yeni bir pencerede yapıştırma")

Aşağıdaki bölümlerde, biz çalışma alanında bir Xamarin.Mac uygulaması ile çalışma ayrıntılı bir göz atalım.

## <a name="about-the-pasteboard"></a>Çalışma alanı hakkında

MacOS (önceki adıyla OS X olarak bilinir), çalışma alanına (`NSPasteboard`) birden fazla sunucu gibi kopyalayın ve yapıştırın, sürükleyip ve uygulama hizmetlerini işlemleri için destek sağlar. Aşağıdaki bölümlerde, biz birkaç anahtar çalışma alanı kavramları yakından sürer.

### <a name="what-is-a-pasteboard"></a>Bir çalışma alanı nedir?

`NSPasteboard` Sınıfı uygulamalar arasında veya belirli bir uygulamanın içinde bilgi alışverişi için standartlaştırılmış bir mekanizma sağlar. Kopyalama ve yapıştırma işlemlerini işlemek için bir çalışma alanı ana işlevi şöyledir:

1. Kullanıcı bir uygulamada bir öğe seçer ve kullanır **Kes** veya **kopyalama** menü öğesi seçili öğenin bir veya daha fazla gösterimlerini çalışma alanındaki yerleştirilir.
2. Kullanıcı kullandığında **Yapıştır** menü öğesi (aynı uygulama ya da farklı bir içinde) ele alabilir verileri çalışma alanına kopyalanır ve uygulamasına eklendi.

Bulma, sürükle, sürükle ve bırak kadar belirgin çalışma alanı kullanımlar içerir ve uygulama işlemleri Hizmetleri:

- Kullanıcı, bir sürükleme işlemi başlattığında Sürükle veriler için çalışma alanına kopyalanır. Başka bir uygulamanın üzerine açılan ile sürükleme işlemi sona ererse, bu uygulama veri çalışma kopyalar.
- Çeviri hizmetleri için istekte bulunan uygulama tarafından çalışma alanındaki çevrilecek veri kopyalanır. Uygulama hizmeti çalışma verileri alır, yapıştırır çalışma alanındaki verileri yedekleyin sonra çeviri yapar.

Kendi basit haliyle, pasteboards therefor uygulamanın işlem dışında özel genel bellek alanında var ve belirli bir uygulamanın içinde veya uygulamalar arasında veri taşımak için kullanılır. Pasteboards kavramlarını kolayca çalışırken grasps, dikkate alınması gereken birkaç daha karmaşık ayrıntıları vardır. Bunlar, aşağıda ayrıntılı olarak ele alınacaktır.

### <a name="named-pasteboards"></a>Adlandırılmış pasteboards

Bir çalışma alanı, genel veya özel olabilir ve çeşitli amaçlarla bir uygulama içinde veya arasında birden fazla uygulama için kullanılabilir. macOS her bir özel, iyi tanımlanmış kullanımı ile birkaç standart pasteboards sağlar:

- `NSGeneralPboard` -Varsayılan çalışma alanı için **Kes**, **kopya** ve **Yapıştır** işlemleri.
- `NSRulerPboard` -Destekleyen **Kes**, **kopya** ve **Yapıştır** işlemleri **Cetveller**.
- `NSFontPboard` -Destekleyen **Kes**, **kopya** ve **Yapıştır** işlemleri `NSFont` nesneleri.
- `NSFindPboard` -Desteklediğini uygulamaya özgü arama metni paylaşabilirsiniz paneller öğrenmek.
- `NSDragPboard` -Destekleyen **sürükleyip** işlemleri.

Çoğu durumlarda, sistem tarafından tanımlanan pasteboards birini kullanmanız. Ancak, kendi pasteboards oluşturmak gereken durumlar olabilir. Bu durumlarda, kullandığınız `FromName (string name)` yöntemi `NSPasteboard` verilen ada sahip özel bir çalışma alanı oluşturmak için sınıfı.

İsteğe bağlı olarak çağırabilirsiniz `CreateWithUniqueName` yöntemi `NSPasteboard` benzersiz olarak adlandırılmış bir çalışma alanı oluşturmak için sınıfı.

### <a name="pasteboard-items"></a>Çalışma alanı öğeleri

Her bir çalışma alanındaki uygulama Yazar veri parçası olarak kabul edilir bir _çalışma öğesi_ ve bir çalışma alanı aynı anda birden çok öğe içerebilir. Bu şekilde, bir uygulama, bir çalışma alanı (örneğin, düz metin ve biçimlendirilmiş metin) ve alma uygulama için kopyalanan verileri birden fazla sürümünü (yalnızca düz metin gibi) işleyebilir verileri yalnızca okuyabilir yazabilirsiniz.

### <a name="data-representations-and-uniform-type-identifiers"></a>Veri Beyanları ve Tekdüzen tür tanımlayıcıları

Çalışma alanı işlemleri genelde birbirleriyle veya veri türlerini hiçbir bilgiye sahip iki (veya daha fazla) arasında yer uygulamaları sürer her işleyebilir. Bir çalışma alanı hakkında bilgi paylaşımı için olası en üst düzeye çıkarmak için yukarıdaki bölümde belirtildiği gibi kopyalanan ve yapıştırılan verileri birden çok gösterimlerini basılı tutabilirsiniz.

Her gösterimi aracılığıyla bir Tekdüzen türü tanımlayıcısı (fazlası sunulmasını tarih türü benzersiz olarak tanımlayan basit bir dize olan UTI), tanımlanan (daha fazla bilgi için lütfen Apple'nın bakın [Tekdüzen türü tanımlayıcıları genel bakış ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) belgeleri). 

Bir özel veri türü (örneğin, bir çizim nesnesinde bir vektör uygulama çizim) oluşturuyorsanız, benzersiz olarak kopyasında tanımlamak ve yapıştırma işlemleri için kendi UTI oluşturabilirsiniz.

Bir uygulama, bir çalışma kopyalanan verileri yapıştırmak hazırlanırken (varsa), kendi yeteneklerini en uygun gösterimi bulmanız gerekir. Bu richest türü kullanılabilir (örneğin biçimlendirilmiş metin sözcük işleme uygulama için), genellikle olacaktır gerekli (düz metin için basit bir metin düzenleyici) olarak kullanılabilir basit form geri dönmeden.

<a name="Promised_Data" />

### <a name="promised-data"></a>Taahhüt edilen verileri

Genel olarak bakıldığında, uygulamalar arasında paylaşımı en üst düzeye çıkarmak için mümkün olduğunca kopyalanan verileri sayıda gösterimlerini sağlamalıdır. Ancak, saat ve bellek kısıtlamaları nedeniyle gerçekte her bir veri türü çalışma alanı yazmak için pratik olabilir.

Bu durumda, ilk veri temsili çalışma alanı'nı koyun ve alıcı uygulama oluşturulan üzerinde-çalışma sırasında yapıştırma işlemi hemen önce olabilir farklı bir gösterimi, isteyebilir.

Çalışma alanında ilk öğe yerleştirdiğinizde, bir veya daha fazla kullanılabilir diğer Beyanları uyan bir nesne tarafından sağlanan belirteceksiniz `NSPasteboardItemDataProvider` arabirimi. Bu nesneler fazladan Beyanları, isteğe bağlı alan uygulama tarafından istenen şekilde sağlar.

### <a name="change-count"></a>Değişiklik sayısı

Her çalışma alanındaki tutar bir _değişiklik sayısı_ artışlarla her bir yeni sahibi zaman bildirilir. Bir uygulama çalışma alanı'nın içeriği, bu değişiklik sayısı değerini denetleyerek incelenmesi en son ne zaman itibaren değişip değişmediğini.

Kullanım `ChangeCount` ve `ClearContents` yöntemlerinin `NSPasteboard` belirli çalışma alanında 's değişiklik sayısını değiştirmek için sınıf.

## <a name="copying-data-to-a-pasteboard"></a>Bir çalışma alanındaki veri kopyalama

İlk bir çalışma alanı erişme, var olan tüm içeriği temizleyerek ve çalışma alanındaki gerektiği şekilde veri sayıda gösterimlerini yazma bir kopyalama işlemi gerçekleştirin.

Örneğin:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

Yukarıdaki örnekte yapmış olmanız gibi tipik olarak, yalnızca genel çalışma alanındaki yazacaksınız. İçin gönderdiğiniz herhangi bir nesne `WriteObjects` yöntemi *gerekir* uygun `INSPasteboardWriting` arabirimi. Birkaç yerleşik sınıfları (gibi `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, ve `NSPasteboardItem`) otomatik olarak bu arabirime uygun.

Çalışma alanındaki özel bir veri sınıfı yazıyorsanız uygun olmalıdır `INSPasteboardWriting` arabirim veya bir örnekte sarmalamak `NSPasteboardItem` sınıfı (bkz [özel veri türleri](#Custom_Data_Types) bölümüne bakın).

## <a name="reading-data-from-a-pasteboard"></a>Bir çalışma alanı verileri okuma

Yukarıda belirtildiği gibi uygulamalar arasında veri paylaşımı için olası en üst düzeye çıkarmak için kopyalanan verilerin birden çok gösterimlerini çalışma alanındaki yazılabilir. En fazla (varsa) yeteneklerini için olası richest sürümü seçin alıcı uygulamasıdır.

### <a name="simple-paste-operation"></a>Basit yapıştırma işlemi

Çalışma alanı kullanarak veri okuma `ReadObjectsForClasses` yöntemi. İki parametre gerekir:

1. Bir dizi `NSObject` temel alarak çalışma okumak istediğiniz sınıfı tür. Bu en istenen veri türüne sahip ilk olarak, azalan tercihlere içinde kalan tüm türleri ile sipariş.
2. (Örneğin, belirli URL içerik türlerine sınırlama) ek kısıtlamalar içeren bir sözlük veya başka kısıtlamalar gerekirse boş bir sözlük.

Yöntemi, biz geçirilen ölçütlerini karşılayan öğeleri bir dizi döndürür ve bu nedenle istenen veri türleri aynı sayıda en fazla içerir. İstenen türlerden hiçbiri varsa ve boş bir dizi döndürdü mümkündür.

Örneğin, aşağıdaki denetler kod bir `NSImage` genel çalışma alanında var ve aşması durumunda bir görüntüsünü iyi görüntüler:

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

### <a name="requesting-multiple-data-types"></a>Birden çok veri türleri isteme

Oluşturulan Xamarin.Mac uygulama türüne bağlı olarak, birden çok gösterimlerini yapıştırılan verileri işlemek mümkün olabilir. Bu durumda, veri çalışma almak için iki senaryo vardır:

1. Tek çağırmaya `ReadObjectsForClasses` yöntemi ve (tercih sırasına göre) işlemleriniz temsili bir dizi sağlama.
2. Birden fazla çağrı yapmak `ReadObjectsForClasses` farklı bir dizi için isteyen yöntemi her zaman türleri.

Bkz: **basit yapıştırma işlemi** bölüm üzerinde bir çalışma veri alma hakkında daha fazla bilgi.

### <a name="checking-for-existing-data-types"></a>Var olan veri türleri için denetleniyor

Veri çalışma gerçekte okumadan bir çalışma alanı verilen veri gösterimi içerip içermediğini denetlemek isteyebilirsiniz zamanlar (etkinleştirme gibi **Yapıştır** yalnızca geçerli veri mevcut olduğunda menü öğesi).

Çağrı `CanReadObjectForClasses` belirli bir türde içerip içermediğini çalışma alanı'nı yöntemi.

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

### <a name="reading-urls-from-the-pasteboard"></a>Çalışma alanına URL'lerden okuma

Ölçüt (örneğin, dosya veya belirli bir veri türüne URL'lerini işaret eden) belirli bir dizi karşılıyorsa belirli bir Xamarin.Mac uygulamanın işlevi üzerinde bağlı olarak, bir çalışma URL'leri okumak için gerekli, ancak yalnızca kaynaklanıyor olabilir. Bu durumda, ikinci parametresinin kullanarak ek arama ölçütü belirtebilirsiniz `CanReadObjectForClasses` veya `ReadObjectsForClasses` yöntemleri.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Özel veri türleri

Ne zaman kendi özel türler çalışma alanındaki Xamarin.Mac uygulamadan kaydetmeniz gerekecek zamanlar vardır. Örneğin, Kopyala ve Yapıştır çizim nesneleri kullanıcıya sağlayan uygulama çizim vektör.

Bu durumda, böylece öğesinden devralınan veri özel sınıfınız tasarım gerekir `NSObject` ve birkaç arabirimlerine uyan (`INSCoding`, `INSPasteboardWriting` ve `INSPasteboardReading`). İsteğe bağlı olarak kullanabileceğiniz bir `NSPasteboardItem` kopyaladığınız veya yapıştırılan verileri yalıtılacak.

Bu seçeneklerin ikisi de aşağıda ayrıntılı olarak ele alınacaktır.

### <a name="using-a-custom-class"></a>Özel bir sınıf kullanma

Bu bölümde Biz bu belge listesinin başında oluşturduğumuz basit bir örnek uygulama genişletme ve pencereler arasında biz kopyalama ve yapıştırma görüntüyü hakkındaki bilgileri izlemek için özel bir sınıf ekleme.

Projeye yeni bir sınıf ekleyin ve bunu **ImageInfo.cs**. Dosyasını düzenleyin ve aşağıdaki gibi yapar:

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

Aşağıdaki bölümlerde Biz bu sınıf ayrıntılı bir göz atalım.

#### <a name="inheritance-and-interfaces"></a>Devralma ve arabirimleri

Özel bir veri sınıfı yazılan veya bir çalışma okumadan önce uygun olmalıdır `INSPastebaordWriting` ve `INSPasteboardReading` arabirimleri. Ayrıca, öğesinden devralmalıdır `NSObject` ve ayrıca uygun `INSCoding` arabirimi:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Sınıfı, Objective-C sunulmalıdır kullanarak `Register` yönergesi ve gerekir kullanıma tüm gerekli özellikleri veya yöntemlerini kullanarak `Export`. Örneğin:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Biz bu sınıfın içereceği - veri görüntünün adını ve türünü (jpg, png, vb.) iki alanlarını bırakıyorsunuz. 

Daha fazla bilgi için bkz: [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) belgeleri açıklar `Register` ve `Export` öznitelikleri Objective-C nesneleri ve kullanıcı Arabirimi öğeleri, C# sınıfları wire için kullanılır.

#### <a name="constructors"></a>Oluşturucular

Bir çalışma okuyabilmesini (düzgün bir şekilde kullanıma sunulan Objective-C) iki oluşturucular bizim Özel veri sınıfı için gerekli olacaktır:

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

İlk olarak, biz kullanıma _boş_ Oluşturucusu varsayılan Objective-C yöntemi altında `init`.

Ardından, biz kullanıma bir `NSCoding` verilen adının altında yapıştırılırken çalışma nesnesinin yeni bir örneğini oluşturmak için kullanılan uyumlu Oluşturucusu `initWithCoder`.

Bu oluşturucu alır bir `NSCoder` (tarafından oluşturulan bir `NSKeyedArchiver` çalışma alanındaki yazılırken), anahtar/değer eşleştirilmiş verileri ayıklar ve veri sınıfının özellik alanları kaydeder.

#### <a name="writing-to-the-pasteboard"></a>Çalışma alanına yazma

Uyumludur tarafından `INSPasteboardWriting` arabirimi, ihtiyacımız iki yöntem ve isteğe bağlı olarak üçüncü bir yöntem kullanıma sunmak böylece sınıfı çalışma alanındaki yazılabilir.

İlk olarak, çalışma alanı, özel bir sınıf için yazılabilir Beyanları hangi veri türü bildirmek ihtiyacımız var:

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Her gösterimi aracılığıyla bir Tekdüzen türü tanımlayıcısı (fazlası sunulmasını veri türü benzersiz olarak tanımlayan basit bir dize olan UTI), tanımlanan (daha fazla bilgi için lütfen Apple'nın bakın [Tekdüzen türü tanımlayıcıları genel bakış ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) belgeleri).

Bizim özel biçim için kendi UTI oluşturuyoruz: "com.xamarin.image-info" (bir uygulama tanımlayıcısı gibi ters gösteriminde olduğuna dikkat edin). Bizim sınıfı ayrıca çalışma alanındaki standart bir dize yazmak yeteneğine sahiptir (`public.text`). 

Ardından, çalışma alanındaki gerçekte yazılan istenen biçimde nesnesi oluşturmak ihtiyacımız var:

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

İçin `public.text` türü, biz döndürmesi basit, biçimlendirilmiş `NSString` nesnesi. Özel `com.xamarin.image-info` türü, biz kullandığınız bir `NSKeyedArchiver` ve `NSCoder` bir anahtar/değer eşleştirilmiş arşiv özel veri sınıfına kodlamak için arabirim. Gerçekte kodlama işlemek için aşağıdaki yöntemi uygulamak için yapmanız gerekir:

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Tek tek anahtar/değer çiftlerini kodlayıcıya yazılır ve yukarıdaki eklediğimiz ikinci oluşturucu kullanılarak kodlanmamış.

İsteğe bağlı olarak, size herhangi bir seçenek verileri çalışma alanındaki yazılırken tanımlamak için aşağıdaki yöntemi içerebilir:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Şu anda yalnızca `WritingPromised` seçeneği kullanılabilir ve belirli bir türde olduğunda ve yalnızca taahhüt gerçekten çalışma alanındaki yazılmış kullanılmalıdır. Daha fazla bilgi için lütfen bkz [taahhüt veri](#Promised_Data) yukarıdaki bölümde.

Yerinde bu yöntemlerle çalışma alanındaki bizim özel bir sınıf yazmak için aşağıdaki kodu kullanılabilir:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Çalışma alanına veri okuma

Uyumludur tarafından `INSPasteboardReading` arabirimi, ihtiyacımız özel veri sınıfı çalışma okuyabilmeniz üç yöntem kullanıma sunmak.

İlk olarak, çalışma alanı, özel bir sınıf panodan okuyabilir Beyanları hangi veri türü bildirmek ihtiyacımız var:

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Yeniden, bu basit UTIs tanımlanır ve biz tanımlanan aynı türleri **çalışma alanındaki yazma** yukarıdaki bölümde.

Ardından, çalışma alanına bildirmek ihtiyacımız _nasıl_ UTI türlerinin her biri aşağıdaki yöntemi kullanarak okunacak:

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

İçin `com.xamarin.image-info` türü, biz söyleyen ile oluşturduğumuz anahtar/değer çifti çözecek çalışma alanı'nı `NSKeyedArchiver` zaman yazma sınıfı çalışma alanındaki çağırarak `initWithCoder:` sınıfına eklediğimiz Oluşturucusu.

Son olarak, çalışma bir UTI veri Beyanları okumak için aşağıdaki yöntemi ekleyin ihtiyacımız var:

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

Tüm bu yöntemlerle yerinde, özel veri sınıfı aşağıdaki kodu kullanarak çalışma okuyabilirsiniz:

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

Ne zaman özel bir sınıf oluşturulmasını garanti değil çalışma alanındaki özel öğeler yazmanız gerekir ya da bir ortak biçiminde, yalnızca gerekli verileri sağlamak istediğiniz zamanlar olabilir. Bu durumlarda, kullanabileceğiniz bir `NSPasteboardItem`.

A `NSPasteboardItem` ayrıntılı denetim sağlar çalışma alanındaki yazıldıktan sonra çalışma alanındaki yazılır ve geçici erişim için - tasarlanmıştır veriler üzerinde elden.

#### <a name="writing-data"></a>Veri yazma

Özel verilerinizi yazmak için bir `NSPasteboardItem` özel sağlamanız gerekir `NSPasteboardItemDataProvider`. Projeye yeni bir sınıf ekleyin ve bunu **ImageInfoDataProvider.cs**. Dosyasını düzenleyin ve aşağıdaki gibi yapar:

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

Özel veri sınıfıyla yaptığımız gibi kullanmak ihtiyacımız `Register` ve `Export` Objective-c kullanıma sunmak için yönergeleri Sınıf öğesinden devralmalıdır `NSPasteboardItemDataProvider` ve uygulamalıdır `FinishedWithDataProvider` ve `ProvideDataForType` yöntemleri.

Kullanım `ProvideDataForType` içinde kaydırılan veri sağlamak için yöntemi `NSPasteboardItem` gibi:

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

Bu durumda, biz bizim görüntü (adı ve ImageType) hakkında bilgi iki parça depolamak ve bu basit bir dize yazma (`public.text`).

Tür veri yazma çalışma alanındaki şu kodu kullanın:

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

Bu makalede, çalışma alanı'nı desteklenmesi için kopyalama ve yapıştırma işlemlerini Xamarin.Mac uygulamasında çalışan bir ayrıntılı bakış duruma getirdi. İlk olarak, standart pasteboards işlemleriyle hakkında bilgi edinmek için basit bir örnek kullanıma sunuldu. Ardından, çalışma alanına ve okuma ve ondan veri yazmak nasıl ayrıntılı bir bakış sürdü. Son olarak, kopyalama ve yapıştırma bir uygulama içinde karmaşık veri türlerini desteklemek için bir özel veri türü kullanarak anlatılmaktadır.



## <a name="related-links"></a>İlgili bağlantılar

- [MacCopyPaste (örnek)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Çalışma alanı Programlama Kılavuzu](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
