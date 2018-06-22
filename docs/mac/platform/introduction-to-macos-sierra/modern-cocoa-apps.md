---
title: Modern macOS uygulamaları oluşturma
description: Bu makalede, bazı ipuçları, özellik ve teknikleri Geliştirici Xamarin.Mac modern macOS uygulamada oluşturmak için kullanabileceğiniz kapsar.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4eb4ff4a9e4784d816e2cbe8734e0422573cad92
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30787785"
---
# <a name="building-modern-macos-apps"></a>Modern macOS uygulamaları oluşturma

_Bu makalede, bazı ipuçları, özellik ve teknikleri Geliştirici Xamarin.Mac modern macOS uygulamada oluşturmak için kullanabileceğiniz kapsar._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>Modern görünümlerle modern görünümler oluşturma

Modern bir görünüme aşağıda gösterilen örnek uygulaması gibi modern bir pencere ve araç çubuğu görünümünü içerir:

[![](modern-cocoa-apps-images/content08.png "Modern bir Mac uygulaması UI örneği")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>İçerik görünümleri boyutta tam etkinleştirme

Bu görünümler Xamarin.Mac uygulamasında elde etmek için geliştirici kullanmak istersiniz bir _tam boyutu içerik görünümü_, içerik genişletir altındaki aracı ve başlık çubuğu alanlar anlamına gelir ve macOS tarafından bulanık otomatik olarak.

Kodda bu özelliği etkinleştirmek için özel bir sınıf için oluşturma `NSWindowController` ve şu şekilde görünür yapın:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Set window to use Full Size Content View
            Window.StyleMask = NSWindowStyle.FullSizeContentView;
        }
        #endregion
    }
}
```

Bu özellik ayrıca Xcode'nın arabirimi Oluşturucusu'nda penceresinin seçilmesi ve denetleme etkinleştirilebilir **tam boyutlu içerik görünümü**:

[![](modern-cocoa-apps-images/content01.png "Xcode'nın arabirimi Oluşturucu ana film şeridi düzenleme")](modern-cocoa-apps-images/content01.png#lightbox)

Tam boyutu içerik görünümü kullanırken, geliştirici başlık ve araç çubuğunda alanları altındaki içeriği belirli içerik (örneğin, etiketleri) altında slayt değil böylece uzaklığı gerekebilir.

Bu sorunu karmaşık hale için başlık ve araç çubuğunda alanları kullanıcı şu anda gerçekleştiren, eylemini göre dinamik bir yükseklik sağlayabilirsiniz kullanıcı yüklü macOS ve/veya uygulamayı çalıştıran Mac donanım sürümü.

Sonuç olarak, kullanıcı arabirimi yerleştirirken uzaklık yalnızca sabit kodlama çalışmaz. Geliştirici dinamik bir yaklaşım uygular gerekecektir.

Apple eklendi [anahtar-değer Observable](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` özelliği `NSWindow` kodu geçerli içerik alanında almak için sınıf. Geliştirici, içerik alanının değiştiğinde gerekli öğeleri el ile konumlandırmak için bu değeri kullanabilirsiniz.

Kod veya arabirim Builder kullanıcı Arabirimi öğeleri yerleştirmek için otomatik düzeni ve boyutu sınıflarını kullanmak için en iyi çözümdür bakın.

Kod aşağıdaki örnekteki gibi uygulamanın View Controller otomatik düzenleme ve boyutu sınıflarını kullanarak kullanıcı Arabirimi öğeleri yerleştirmek için kullanılabilir:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        #region Computed Properties
        public NSLayoutConstraint topConstraint { get; set; }
        #endregion

        ...

        #region Override Methods
        public override void UpdateViewConstraints ()
        {
            // Has the constraint already been set?
            if (topConstraint == null) {
                // Get the top anchor point
                var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
                var topAnchor = contentLayoutGuide.TopAnchor;

                // Found?
                if (topAnchor != null) {
                    // Assemble constraint and activate it
                    topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
                    topConstraint.Active = true;
                }
            }

            base.UpdateViewConstraints ();
        }
        #endregion
    }
}
```

Bu kod bir etiket için uygulanacak bir üst kısıtlaması için depolama oluşturur (`ItemTitle`) başlık ve araç çubuğu alanı altında irsaliyesi değil emin olmak için:

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

Görünüm denetleyicinin kılarak `UpdateViewConstraints` yöntemi, geliştirici gerekli kısıtlaması zaten oluşturulmuş olmadığını görmek için test edebilir ve gerekirse oluşturun.

Yeni bir kısıtlama oluşturulması gerekip gerekmediğini `ContentLayoutGuide` özelliği kısıtlı gereken denetimi erişilen ve içine cast penceresinin bir `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

TopAnchor özelliği `NSLayoutGuide` erişilir ve varsa, istenen kaydırma miktarı ile yeni bir kısıtlama oluşturmak için kullanılan ve new kısıtlaması uygulayan etkinleştirilir:

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>Etkinleştirme kolaylaştırılmış araç çubukları

Normal macOS penceresinin başlık çubuğuna pencerenin üst kenarına çalıştıran bir standart içerir. Pencerenin bir araç çubuğu da içeriyorsa, bu başlık çubuğu alanı altında görüntülenir:

[![](modern-cocoa-apps-images/content02.png "Standart Mac araç çubuğu")](modern-cocoa-apps-images/content02.png#lightbox)

Kolaylaştırılmış bir araç kullanarak, başlık alanı kaybolur ve araç çubuğunun başlık çubuğunun konumda yukarı taşır zaman penceresini kapatın, simge durumuna küçült ve Ekranı Kapla düğmesi satır içi:

[![](modern-cocoa-apps-images/content03.png "Kolaylaştırılmış Mac araç çubuğu")](modern-cocoa-apps-images/content03.png#lightbox)

Geçersiz kılarak kolaylaştırılmış araç etkin `ViewWillAppear` yöntemi `NSViewController` ve aşağıdaki gibi görünmesi yapma:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

Bu etkiyi tipik olarak kullanılan _Shoebox uygulamaları_ (bir pencere uygulamalar) eşlemeleri, takvim, notlar ve sistem tercihleri gibi. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>Aksesuar görünüm denetleyicileri kullanma

Uygulama tasarımı bağlı olarak, geliştirici de içeriğe duyarlı denetimleri etkinliği tabanlı kullanıcıya bunlar sağlamak üzere Title/araç çubuğu alanının altında sağ görünür bir donatıyı View Controller ile alan başlık çubuğu tamamlamak isteyebilirsiniz şu anda gerçekleştiriliyor:

[![](modern-cocoa-apps-images/content04.png "Bir örnek donatıyı View Controller")](modern-cocoa-apps-images/content04.png#lightbox)

Aksesuar görünüm denetleyicisini otomatik olarak bulanık ve olması Geliştirici müdahalesi olmadan sistem tarafından yeniden boyutlandırılabilir.

Bir donatıyı View Controller eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosyayı düzenlemek için açın.
2. Sürükleme bir **özel görünüm denetleyicisini** pencerenin hiyerarşiye: 

    [![](modern-cocoa-apps-images/content05.png "Yeni bir özel görünüm denetleyicisi ekleme")](modern-cocoa-apps-images/content05.png#lightbox)
3. Düzen aksesuar görünüme ait kullanıcı Arabirimi: 

    [![](modern-cocoa-apps-images/content06.png "Yeni görünümü tasarlama")](modern-cocoa-apps-images/content06.png#lightbox)
4. Aksesuar görünüm olarak kullanıma bir **çıkışı** ve diğer **Eylemler** veya **çıkışlar** için kullanıcı Arabiriminde: 

    [![](modern-cocoa-apps-images/content07.png "Gerekli çıkışı ekleme")](modern-cocoa-apps-images/content07.png#lightbox)
5. Değişiklikleri kaydedin.
6. Visual Studio değişiklikleri eşitlemek Mac için geri dönün.

Düzenleme `NSWindowController` ve şu şekilde görünür yapın:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }
        #endregion
    }
}
```

Bu kodu önemli noktaları arabirimi Oluşturucusu'nda tanımlanan ve olarak gösterilen özel görünüm için Görünüm ayarlandığı olan bir **çıkışı**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

Ve `LayoutAttribute` tanımlayan _burada_ donatıyı görüntülenir:

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

MacOS artık tam olarak yerelleştirilmiştir çünkü `Left` ve `Right` `NSLayoutAttribute` özellikleri kullanım dışı bırakıldı ve ile değiştirilmelidir `Leading` ve `Trailing`.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>Sekmeli Windows kullanma

Ayrıca, macOS sistem uygulamanın pencereye donatıyı görünüm denetleyicileri ekleyebilirsiniz. Örneğin, burada birkaç uygulamanın Windows bir sanal penceresine birleştirilir Windows sekmeli oluşturmak için şunu yazın:

[![](modern-cocoa-apps-images/content08.png "Sekmeli Mac penceresi örneği")](modern-cocoa-apps-images/content08.png#lightbox)

Genellikle, geliştirici sınırlı eylem kullanmak Xamarin.Mac uygulamalarını sekmeli Windows'da gerekir, sistem onları şekilde otomatik olarak işleyecek:

- Windows otomatik olarak olacaktır ne zaman sekmeli `OrderFront` yöntemi çağrılır.
- Windows otomatik olarak olacaktır ne zaman Untabbed `OrderOut` yöntemi çağrılır.
- Tüm sekmeli windows hala "görünür" olarak kabul edilir kodda, ancak herhangi bir öndeki olmayan sekme CoreGraphics kullanarak sistem tarafından gizli.
- Kullanım `TabbingIdentifier` özelliği `NSWindow` Grup Windows sekmeleri birleştirerek.
- Eğer öyleyse bir `NSDocument` birkaç bu özelliklerin tabanlı uygulama etkin olacaktır (örneğin, sekme çubuğuna eklenmekte olan otomatik olarak artı düğmesini) herhangi bir geliştirici eylem.
- Olmayan`NSDocument` tabanlı uygulamalar, yeni bir belge geçersiz kılarak eklemek için sekme grubu "artı" düğmesini etkinleştirebilir `GetNewWindowForTab` yöntemi `NSWindowsController`.

Tüm parçaları birlikte getiren `AppDelegate` kullanmak istediğinizi bir uygulama sistem tabanlı sekmeli Windows aşağıdaki gibi görünebilir:

```csharp
using AppKit;
using Foundation;

namespace MacModern
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewDocumentNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom Actions
        [Export ("newDocument:")]
        public void NewDocument (NSObject sender)
        {
            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow (this);
        } 
        #endregion
    }
}
```

Burada `NewDocumentNumber` özelliği oluşturulan yeni belgeler sayısını izler ve `NewDocument` yöntemi yeni bir belge oluşturur ve görüntüler.

`NSWindowController` Gibi görünebilir:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Application Access
        /// <summary>
        /// A helper shortcut to the app delegate.
        /// </summary>
        /// <value>The app.</value>
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void SetDefaultDocumentTitle ()
        {
            // Is this the first document?
            if (App.NewDocumentNumber == 0) {
                // Yes, set title and increment
                Window.Title = "Untitled";
                ++App.NewDocumentNumber;
            } else {
                // No, show title and count
                Window.Title = $"Untitled {App.NewDocumentNumber++}";
            }
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Prefer Tabbed Windows
            Window.TabbingMode = NSWindowTabbingMode.Preferred;
            Window.TabbingIdentifier = "Main";

            // Set default window title
            SetDefaultDocumentTitle ();

            // Set window to use Full Size Content View
            // Window.StyleMask = NSWindowStyle.FullSizeContentView;

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }

        public override void GetNewWindowForTab (NSObject sender)
        {
            // Ask app to open a new document window
            App.NewDocument (this);
        }
        #endregion
    }
}
```

Burada statik `App` özelliği sağlar almak için bir kısayol `AppDelegate`. `SetDefaultDocumentTitle` Yöntemi oluşturulan yeni belgeler sayısına göre yeni bir belge başlık ayarlar.

Aşağıdaki kod, uygulamanın sekmeleri kullanmayı tercih eder macOS bildirir ve uygulamanın Windows sekme halinde gruplandırılır izin veren bir dize sağlar:

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

Ve aşağıdaki yöntemi aşırı yükleme kullanıcı tarafından tıklatıldığında yeni bir belge oluşturacak sekme çubuğunu artı düğmesini ekler:

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>Çekirdek animasyon kullanma

Çekirdek animasyon macOS yerleşik bir yüksek güç beslemeli grafik işleme altyapısıdır. Çekirdek animasyon grafik işlemleri makineyi yavaşlatabilir CPU üzerinde çalışan aksine modern macOS donanımda kullanılabilir GPU (grafik işlem birimi) yararlanmak için optimize edilmiştir.

`CALayer`, Çekirdek animasyon tarafından sağlanan, hızlı ve sıvı kaydırma ve animasyonları gibi görevleri için kullanılabilir. Bir uygulamanın kullanıcı arabirimi, birden çok subviews ve çekirdek animasyon tam olarak yararlanmak için katmanları oluşan.

A `CALayer` nesnesi ne sunulan denetlemek Geliştirici gibi kullanıcıya ekranda izin çeşitli özellikleri sağlar:

- `Content` -Olabilir bir `NSImage` veya `CGImage` katmanın içeriğini sağlar.
- `BackgroundColor` -Katman olarak arka plan rengini ayarlar bir `CGColor`
- `BorderWidth` -Kenarlık genişliğini ayarlar.
- `BorderColor` -Kenarlık rengini belirler.

Uygulamanın kullanıcı arabiriminde çekirdek grafikleri kullanmak için bunu kullanarak gerekir _katman yedeklenen_ Apple Geliştirici pencerenin içerik görünümünde her zaman etkinleştirmelisiniz öneren görünümleri. Bu şekilde, tüm alt görünümler otomatik olarak katman yedekleme de devralır.

Ayrıca, yeni bir ekleme aksine katman yedeklenen görünümlerini kullanarak Apple öneren `CALayer` bir alt katman olarak sistem birkaç gerekli ayarları (örneğin Retina görüntü tarafından gerekli olanlar) otomatik olarak işleyecek olduğundan.

Yedekleme katman ayarlayarak etkinleştirilebilir `WantsLayer` , bir `NSView` için `true` veya Xcode'nın arabirimi Oluşturucu altında içinde **görünüm etkileri denetçisi** denetleyerek **çekirdek animasyon katmanı**:

[![](modern-cocoa-apps-images/content09.png "Görünüm etkileri denetçisi")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>Katmanlar görünümlerle yeniden çizim

Katman yedeklenen görünümleri Xamarin.Mac kullanmayla ayarlanırken olduğu başka bir önemli adım `LayerContentsRedrawPolicy` , `NSView` için `OnSetNeedsDisplay` içinde `NSViewController`. Örneğin:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Ve çerçeve kaynağına değiştiğinde, geliştirici bu özelliği ayarlı değil, görünüm çizilir hangi değil istenen performans nedenleriyle. Bu özelliği ile `OnSetNeedsDisplay` Geliştirici el ile ayarlamanız gerekir `NeedsDisplay` için `true` yeniden boyutlandırmaya, ancak içerik zorlamak için.

Bir görünüm kirli olarak işaretli olduğunda sistem denetler `WantsUpdateLayer` görünümünün özelliği. Döndürürse `true` sonra `UpdateLayer` yöntemi çağrıldığında, aksi takdirde `DrawRect` yöntemi görünümün Görünüm'ün içeriğini güncellemek için çağrılır.

Apple gerektiğinde görünümleri içeriği güncelleştirmek için aşağıdaki önerileri vardır:

- Apple tercih kullanarak `UpdateLater` üzerinden `DrawRect` zaman, olabildiğince önemli ölçüde performans artışı sağlar.
- Aynı `layer.Contents` şuna benzer kullanıcı Arabirimi öğeleri için.
- Apple Geliştirici Standart görünümler gibi kullanarak, kullanıcı oluşturmak için de tercih `NSTextField`, yeniden mümkün olduğunda.

Kullanılacak `UpdateLayer`, için özel bir sınıf oluşturun `NSView` ve aşağıdaki gibi kod yapın:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView
    {
        #region Computed Properties
        public override bool WantsLayer {
            get { return true; }
        }

        public override bool WantsUpdateLayer {
            get { return true; }
        }
        #endregion

        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void DrawRect (CoreGraphics.CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

        }

        public override void UpdateLayer ()
        {
            base.UpdateLayer ();

            // Draw view 
            Layer.BackgroundColor = NSColor.Red.CGColor;
        }
        #endregion
    }
}
```

<a name="Using-Modern-Drag-and-Drop" />

## <a name="using-modern-drag-and-drop"></a>Modern sürükle ve bırak kullanma

Kullanıcı için modern bir Sürükle ve bırak deneyimi sunmak için geliştirici benimsemeyi _sürükleme Flocking_ kendi uygulamasının sürükle ve bırak işlemleri. Sürükleme Flocking, her bir dosya veya başlangıçta sürüklenen öğesi, kullanıcı sürükleme işlemi devam ediyor olarak (Grup öğeleri sayısını imleç altında birlikte) flocks tek bir öğe göründüğü ' dir.

Kullanıcı sürükleme işlemi sona ererse, ayrı ayrı öğeler Unflock ve özgün konumlarına geri dönün.

Aşağıdaki kod örneği, sürükle Flocking özel bir görünüm üzerinde sağlar:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView, INSDraggingSource, INSDraggingDestination
    {
        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void MouseDragged (NSEvent theEvent)
        {
            // Create group of string to be dragged
            var string1 = new NSDraggingItem ((NSString)"Item 1");
            var string2 = new NSDraggingItem ((NSString)"Item 2");
            var string3 = new NSDraggingItem ((NSString)"Item 3");

            // Drag a cluster of items
            BeginDraggingSession (new [] { string1, string2, string3 }, theEvent, this);
        }
        #endregion
    }
}
```

Her öğe için sürüklenen göndererek flocking etkisi ulaşılmıştı `BeginDraggingSession` yöntemi `NSView` dizideki ayrı bir öğe olarak.

İle çalışırken bir `NSTableView` veya `NSOutlineView`, kullanın `PastboardWriterForRow` yöntemi `NSTableViewDataSource` sınıfı sürükleme işlemi başlatmak için:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDataSource: NSTableViewDataSource
    {
        #region Constructors
        public ContentsTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override INSPasteboardWriting GetPasteboardWriterForRow (NSTableView tableView, nint row)
        {
            // Return required pasteboard writer
            ...
            
            // Pasteboard writer failed
            return null;
        }
        #endregion
    }
}
```

Tek bir sağlamak Geliştirici böylece `NSDraggingItem` aksine eski yöntemi sürüklenen tablosundaki her öğe için `WriteRowsWith` , yazma tüm satırlar tek bir grup için çalışma alanı.

İle çalışırken `NSCollectionViews`, yeniden kullanmak `PasteboardWriterForItemAt` yöntemi tersine `WriteItemsAt` sürüklendiğinde yöntemi başlar.

Geliştirici, her zaman büyük dosyalar çalışma alanındaki koyma kaçınmanız gerekir. MacOS Sierra, yeni _dosya söz_ başvuruları yerleştirmek Geliştirici izin verilen kullanıcı yeni kullanarak bırakma işlemi tamamlandığında, daha sonra uyması gereken çalışma alanındaki dosyaları `NSFilePromiseProvider` ve `NSFilePromiseReceiver` sınıflar.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>Modern olay izleme kullanma

Bir kullanıcı arabirimi öğesi için (gibi bir `NSButton`) eklenmiş bir başlık veya araç çubuğu alanı için kullanıcı öğesini tıklatın ve sahip (örneğin, bir açılır pencere görüntüleme) normal olarak bir olay tetikleyin. Ancak, öğe ayrıca başlık veya araç çubuğu alanında olduğundan, kullanıcının pencereyi de taşımak için öğeyi sürükleyip mümkün olması gerekir.

Bu kodda gerçekleştirmek için öğesi için özel bir sınıf oluşturun (gibi `NSButton`) ve geçersiz kılma `MouseDown` şekilde olay:

```csharp
public override void MouseDown (NSEvent theEvent)
{
    var shouldCallSuper = false;

    Window.TrackEventsMatching (NSEventMask.LeftMouseUp, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Handle event as normal
        stop = true;
        shouldCallSuper = true;
    });

    Window.TrackEventsMatching(NSEventMask.LeftMouseDragged, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Pass drag event to window
        stop = true;
        Window.PerformWindowDrag (evt);
    });

    // Call super to handle mousedown
    if (shouldCallSuper) {
        base.MouseDown (theEvent);
    }
}
```

Bu kodu kullanır `TrackEventsMatching` yöntemi `NSWindow` UI öğesi müdahale bağlı `LeftMouseUp` ve `LeftMouseDragged` olaylar. İçin bir `LeftMouseUp` olayı, kullanıcı Arabirimi öğesi yanıtlar normal olarak. İçin `LeftMouseDragged` olayı, olay iletilir `NSWindow`'s `PerformWindowDrag` pencereyi ekranda taşımak için yöntem.

Çağırma `PerformWindowDrag` yöntemi `NSWindow` sınıfı, aşağıdaki avantajları sağlar:

- Uygulama askıda olsa bile pencere taşımak sağlar (ne zaman gibi derin bir döngü işleme).
- Alan geçiş beklendiği gibi çalışmaz.
- Boşluk çubuğu normal olarak görüntülenir.
- Tutturma penceresi ve hizalama normal olarak çalışır.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>Modern kapsayıcı görünümü denetimlerini kullanma

macOS Sierra varolan kapsayıcı görünümü denetimleri önceki işletim sistemi sürümünde kullanılabilir birçok modern geliştirmeleri sağlar.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>Tablo görünümü iyileştirmeleri

Geliştirici her zaman yeni kullanmalısınız `NSView` kapsayıcı görünümü denetimleri sürümünü gibi temel `NSTableView`. Örneğin:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }
        #endregion
    }
}
```

Bu özel tablo satır (örneğin, şu satırı silmek için geçirmeyi) tablosundaki satırları verilen eklenmesi gereken eylemler sağlar. Bu davranışı etkinleştirmek için geçersiz kılma `RowActions` yöntemi `NSTableViewDelegate`:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }

        public override NSTableViewRowAction [] RowActions (NSTableView tableView, nint row, NSTableRowActionEdge edge)
        {
            // Take action based on the edge
            if (edge == NSTableRowActionEdge.Trailing) {
                // Create row actions
                var editAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Regular, "Edit", (action, rowNum) => {
                    // Handle row being edited
                    ...
                });

                var deleteAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Destructive, "Delete", (action, rowNum) => {
                    // Handle row being deleted
                    ...
                });

                // Return actions
                return new [] { editAction, deleteAction };
            } else {
                // No matching actions
                return null;
            }
        }
        #endregion
    }
}
```

Statik `NSTableViewRowAction.FromStyle` aşağıdaki stiller yeni bir tablo satır eylemi oluşturmak için kullanılır:

- `Regular` -Sıranın içeriği Düzenle gibi standart bozucu olmayan bir eylemi gerçekleştirir.
- `Destructive` -Tablosundan satır silme gibi zararlı bir eylemi gerçekleştirir. Bu eylemler kırmızı bir arka plan ile işlenir.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>Kaydırma görünüm geliştirmeleri 

Bir kaydırma görünümünü kullanırken (`NSScrollView`) doğrudan veya başka bir denetim bir parçası olarak (gibi `NSTableView`), kaydırma görünümünün içeriğini Modern görünümlü ve görünümleri kullanarak bir Xamarin.Mac uygulaması başlık ve araç çubuğundaki alanlarında altında kaydırın.

Sonuç olarak, listedeki ilk öğe kaydırma görünümü içerik alanının kısmen başlık ve araç çubuğundaki alan tarafından görünmeyebilir.

Bu sorunu gidermek için Apple için iki yeni özellik eklemiştir `NSScrollView` sınıfı:

- `ContentInsets` -Sağlamak Geliştirici sağlayan bir `NSEdgeInsets` kaydırma görünümüne dön uygulanacak uzaklık tanımlama nesnesi.
- `AutomaticallyAdjustsContentInsets` -İf `true` kaydırma Görünüm otomatik olarak işleyecek `ContentInsets` geliştirici için.

Kullanarak `ContentInsets` Geliştirici Donatılar eklenmesi için gibi izin vermek için kaydırma görünümünü başlangıcı ayarlayabilirsiniz:

- Sıralama göstergesi Mail uygulamasında gösterilene ister.
- Bir arama alanı.
- Yenileme veya güncelleştirme düğmesi.

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>Otomatik düzeni ve Modern uygulamalar yerelleştirme

Apple Geliştirici kolayca bir Uluslararası macOS uygulama oluşturmak izin Xcode'da çeşitli teknolojiler eklemiştir. Xcode şimdi geliştirici, kullanıcıya açık metin uygulamanın kullanıcı arabirimi film şeridi dosyalarından tasarımında ayırmak ve Bu ayrım UI değişirse korumak için araçlar sağlar.

Daha fazla bilgi için lütfen Apple'nın bkz [uluslararası duruma getirme ve yerelleştirme Kılavuzu](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html).

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>Uygulama temel uluslararası hale getirme 

Geliştirici, temel uluslararası uygulayarak, uygulamanın kullanıcı arabirimini temsil eder ve tüm kullanıcı dönük dizeleri ayırmak için tek bir film şeridi dosya sağlayabilir. 

Geliştirici oluştururken ilk film şeridi dosya (veya dosyaları), temel uluslararası (Geliştirici konuşur dili) oluşturulacak uygulamanın kullanıcı arabirimi tanımlayın.

Ardından, geliştirici yerelleştirmeler ve birden çok dillere çevrilebilir temel uluslararası dizeleri (film şeridi UI tasarımında) dışarı aktarabilirsiniz.

Daha sonra bu yerelleştirmeler alınabilir ve Xcode film şeridi dile özgü dize dosyaları oluşturur.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>Yerelleştirme desteklemek için otomatik düzeni uygulama

Yerelleştirilmiş sürümleri çünkü dizesi değerleri çok farklı boyutlarda olabilir ve/veya okuma yönü, geliştirici otomatik yerleşim konumu ve uygulamanın kullanıcı arabiriminde bir film şeridi dosya boyutu için kullanmanız gerekir.

Apple önermek için aşağıdakileri yapın:

- **Sabit Genişlik kısıtlamaları kaldırmak** -tüm metin tabanlı görünümleri yeniden boyutlandırmak için içeriğine göre izin verilmelidir. Sabit Genişlik görünüm içeriklerini belirli diller kırpma.
- **İç içerik boyutunu kullan** - varsayılan metin tabanlı görünümleri otomatik-içeriklerini uyacak şekilde boyutuna göre. Doğru boyutlandırma değil metin tabanlı görünümü, Xcode'nın arabirimi Oluşturucusu'nda seçin sonra seçin **Düzenle** > **boyutu için uygun içerik**.
- **Başında ve sonunda öznitelikleri uygulamak** - metnin yönünü değişebildiğinden kullanıcının diline dayalı, yeni kullanmak `Leading` ve `Trailing` varolan aksine kısıtlaması özniteliklerini `Right` ve `Left` öznitelikler. `Leading` ve `Trailing` otomatik olarak dilleri yönüne göre ayarlar.
- **PIN görünümleri bitişik görünümlere** -bu yeniden konumlandırma ve yanıt olarak seçilen dil etrafında görünümleri değiştirmek gibi yeniden boyutlandırmak görünümleri sağlar.
- **Bir Windows Minimum ve/veya en büyük boyutlar ayarlamazsanız** -seçilen dil kendi alanlara göre yeniden boyutlandırır gibi boyutunu değiştirmek için Windows izin.
- **Test düzeni değişiklikleri sürekli** - sırasında uygulama geliştirme test sürekli farklı dillerde. Apple'nın bkz [bilgisayarınızı Uluslararası yapılan sınama uygulama](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) daha fazla ayrıntı için belgeleri.
- **NSStackViews için PIN görünümleri birlikte kullanmak**  -  `NSStackViews` içeriklerini kaydırma ve tahmin edilebilir şekilde büyümesine ve içeriği seçilen dile göre boyutu Değiştir sağlar.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Xcode'nın arabirimi Oluşturucusu'nda yerelleştirme

Apple Geliştirici tasarlarken veya bir uygulamanın kullanıcı arabirimini düzenleme yerelleştirme desteklemek için kullanabileceğiniz çeşitli özellikler Xcode'nın arabirimi Oluşturucu sağlamıştır. **Metin yönü** bölümünü **özniteliği denetçisi** Geliştirici ipuçları nasıl yönü kullanılır ve bir select metin tabanlı görünümü güncelleştirilmiş sağlamasına olanak verir (gibi `NSTextField`):

[![](modern-cocoa-apps-images/content10.png "Metin yönü seçenekleri")](modern-cocoa-apps-images/content10.png#lightbox)

Üç olası değerler için **metin yönü**:

- **Doğal** -düzeni denetime atanmış dize temel alır.
- **Soldan sağa** -düzeni her zaman soldan sağa zorlanır.
- **Sağdan sola** -düzeni her zaman sağdan sola zorlanır.

İki olası değerler için **düzeni**:

- **Soldan sağa** -düzeni her zaman soldan sağa olur.
- **Sağdan sola** -düzeni her zaman sağdan sola olur.

Belirli bir uyum gerekli olmadığı sürece genellikle bu değiştirilmemelidir.

**Yansıtma** özelliği (hücre resmi konumu gibi) belirli denetim özellikleri ters çevirmek için sistem söyler. Üç olası değerler vardır:

- **Otomatik olarak** -konumu otomatik olarak seçilen dil yönüne göre değişir.
- **Sağ için sol arabiriminde** -konum yalnızca sağdan sola yazılan tabanlı diller olarak değiştirilecek.
- **Hiçbir zaman** -konumu hiçbir zaman değiştirir.

Geliştirici belirtilmişse **Center**, **blokla** veya **tam** metin tabanlı bir görünüm içerikteki hizalama, bunlar hiç olacaktır seçili çevrilen göre dil.

MacOS Sierra önce kodda oluşturulan denetimleri otomatik olarak yansıtılması değil. Geliştirici yansıtma işlemek için aşağıdaki gibi kod kullanmanız gerekiyordu:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Setting a button's mirroring based on the layout direction
    var button = new NSButton ();
    if (button.UserInterfaceLayoutDirection == NSUserInterfaceLayoutDirection.LeftToRight) {
        button.Alignment = NSTextAlignment.Right;
        button.ImagePosition = NSCellImagePosition.ImageLeft;
    } else {
        button.Alignment = NSTextAlignment.Left;
        button.ImagePosition = NSCellImagePosition.ImageRight;
    }
}
```

Burada `Alignment` ve `ImagePosition` temel alınarak ayarlanan `UserInterfaceLayoutDirection` denetimi.

macOS Sierra ekler birkaç yeni kolaylık Oluşturucusu (statik aracılığıyla `CreateButton` yöntemi) (örneğin, başlık, görüntü ve eylem) birkaç parametre alır ve otomatik olarak doğru şekilde yansıtma. Örneğin:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>Sistem görünümlerini kullanma

Modern macOS uygulamalar da görüntü oluşturma, düzenleme veya sunu uygulamalar için çalışır bir yeni koyu arabirimi görünümü benimseyebilirsiniz:

[![](modern-cocoa-apps-images/content11.png "Koyu Mac penceresi UI örneği")](modern-cocoa-apps-images/content11.png#lightbox)

Bu pencere görüntülenmeden önce bir kod satırı ekleyerek yapılabilir. Örneğin:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        ...
    
        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Apply the Dark Interface Appearance
            View.Window.Appearance = NSAppearance.GetAppearance (NSAppearance.NameVibrantDark);

            ...
        }
        #endregion
    }
}
```

Statik `GetAppearance` yöntemi `NSAppearance` sınıfı sistemden adlandırılmış bir görünüm için kullanılır (Bu durumda `NSAppearance.NameVibrantDark`).

Apple sistem görünümleri kullanmak için aşağıdaki önerileri vardır:

- Adlandırılmış renkleri sabit kodlanmış değerler tercih (gibi `LabelColor` ve `SelectedControlColor`).
- Sistem standart denetim stili, mümkün olduğunda kullanın.

Sistem görünümleri kullanan bir macOS uygulama otomatik olarak sistem tercihleri uygulamadan erişilebilirlik özellikleri etkinleştirdiyseniz kullanıcılar için düzgün çalışmaz. Sonuç olarak, Apple içinde macOS uygulamalarını sistem görünümleri, geliştirici her zaman kullanmalısınız önerir.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>Film şeritleri ile Uı'lar tasarlama

Film şeritleri yalnızca bir uygulamanın kullanıcı arabirimi, Yukarı ancak görselleştirmek ve kullanıcı arabirimini tasarlamak için ayrı ayrı öğeler akış tasarım ve de belirli öğeleri hiyerarşisini geliştiriciye izin verir.

Denetleyicileri öğeleri oluşturma ve Segues soyut bir birimde toplamak ve tipik "izleme kodu" görünümü hiyerarşinin tamamında taşımak için gereken kaldırmak Geliştirici izin ver:

[![](modern-cocoa-apps-images/content12.png "Xcode'nın arabirimi Oluşturucu kullanıcı Arabiriminde düzenleme")](modern-cocoa-apps-images/content12.png#lightbox)

Daha fazla bilgi için lütfen bkz bizim [film şeritleri giriş](~/mac/platform/storyboards/index.md) belgeleri.

Bir film şeridi tanımlanan belirli bir Sahne önceki Sahne görünüm hiyerarşideki verileri nerede gerektirecek birçok örneği vardır. Apple planda arasında bilgi geçirme için aşağıdaki önerileri vardır:

- Her zaman, veri bağımlılıklar aşağı doğru hareket hiyerarşide basamaklı.
- Bu UI esneklik sınırlar olarak cmdlet'e kod UI yapısal bağımlılıklar, kaçının.
- Genel veri bağımlılıklar sağlamak için C# arabirimleri kullanın.

Segue kaynağı olarak hareket görünüm denetleyicisini kılabilirsiniz `PrepareForSegue` yöntemi ve sıfırlamaları gerekli (veri geçirme gibi) önce Segue View Controller hedef görüntülemek için yürütüldüğünde. Örneğin:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

Daha fazla bilgi için lütfen bkz bizim [Segues](~/mac/platform/storyboards/indepth.md#Segues) belgeleri.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>Eylemler yayma

MacOS uygulama tasarımını bağlı olarak, ne zaman bir eylem UI denetimi için en iyi işleyicisi başka bir yerde UI hiyerarşisinde olabilir zamanlar olabilir. Bu, genellikle menüleri ve menü öğeleri, uygulamanın kullanıcı Arabirimi geri kalanından ayrı kendi Sahne Canlı geçerlidir.

Bu durumu yönetmek için geliştirici özel eylem oluşturabilir ve Yanıtlayıcı zincir oluşturan eylem geçirin. Daha fazla bilgi için lütfen bkz bizim [özel pencere eylemlerle çalışma](~/mac/user-interface/menu.md) belgeleri.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>Modern Mac özellikleri

Apple Geliştirici gibi Mac platformun en iyi şekilde sağlayan macOS içinde Sierra birkaç kullanıcı dönük özellikleri eklemiştir:

- **NSUserActivity** -bu uygulamanın kullanıcı de dahil etkinliğini tanımlayan izin verir. `NSUserActivity` Burada bir kullanıcının cihaz başlatılan bir etkinlik toplanmış ve başka bir cihazda devam iletimi desteklemek için başlangıçta oluşturuldu. `NSUserActivity` macOS şekliyle aynı mu iOS çalışır, bu nedenle Lütfen bakın bizim [iletimi giriş](~/ios/platform/handoff.md) ayrıntılı bilgi için iOS belgelerine.
- **Mac üzerinde Siri** -Siri geçerli etkinlik kullanır (`NSUserActivity`) kullanıcı verebilir komutları bağlamına sağlamak için.
- **Durum geri yükleme** - kullanıcı bir uygulama macOS ve daha sonra relaunches, uygulama otomatik olarak sonlandırılıyor önceki durumuna geri döndürülen zaman. Geliştirici durumu geri yükleme API kodlamak ve kullanıcı için kullanıcı arabirimini görüntülemeden önce geçici UI durumları geri yüklemek için kullanabilirsiniz. Uygulama ise `NSDocument` bağlı olarak, durumu geri yüklemesinin otomatik olarak gerçekleştirilir. Durumu geri yüklemesinin için etkinleştirmek için olmayan`NSDocument` tabanlı uygulamalarınızı ayarlamak `Restorable` , `NSWindow` sınıfının `true`.
- **Bulut belgelerde** -macOS Sierra önce bir uygulama açıkça kullanıcının iCloud sürücü belgelerle çalışma katılımı gerekiyordu. MacOS Sierra kullanıcı'nın içinde **Masaüstü** ve **belgeleri** klasörleri işlemleri senkronize edilir kullanıcıların iCloud sürücü otomatik olarak sistem tarafından. Sonuç olarak, kullanıcının makinesinde alan boşaltmak için belgeleri yerel kopyalarını silinebilir. `NSDocument` Bu değişiklik tabanlı uygulamalar otomatik olarak işler. Diğer tüm uygulama türleri kullanmanız gerekecektir bir `NSFileCoordinator` okuma ve yazma belgelerin eşitlenecek.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, bazı ipuçları, özellik ve teknikleri Geliştirici Xamarin.Mac modern macOS uygulamada oluşturmak için kullanabileceğiniz ele.



## <a name="related-links"></a>İlgili bağlantılar

- [macOS örnekleri](https://developer.xamarin.com/samples/mac/)
