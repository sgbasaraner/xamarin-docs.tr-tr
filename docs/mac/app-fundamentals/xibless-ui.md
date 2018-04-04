---
title: .Storyboard/.xib-less kullanıcı arabirimi tasarımı
description: Bu makalede, C# kodu, .storyboard dosyaları, .xib dosyaları ya da arabirimi Oluşturucu olmadan doğrudan Xamarin.Mac uygulamanın kullanıcı arabiriminde oluşturulması ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 66725b02d3e351e74fa79ae5336a7db3a9f2b534
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="storyboardxib-less-user-interface-design"></a>.Storyboard/.xib-less kullanıcı arabirimi tasarımı

_Bu makalede, C# kodu, .storyboard dosyaları, .xib dosyaları ya da arabirimi Oluşturucu olmadan doğrudan Xamarin.Mac uygulamanın kullanıcı arabiriminde oluşturulması ele alınmaktadır._


## <a name="overview"></a>Genel Bakış

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı kullanıcı arabirimi öğeleri erişiminiz ve çalışan bir geliştirici araçları *Objective-C* ve *Xcode* yapar. Genellikle, bir Xamarin.Mac uygulaması oluştururken, Xcode'nın arabirimi Oluşturucu .storyboard veya .xib dosyalarıyla oluşturmak ve uygulamanın kullanıcı arabiriminde korumak için kullanırsınız.

Ayrıca, doğrudan C# kodunda bazılarını veya tümünü Xamarin.Mac uygulamanızın kullanıcı Arabirimi oluşturma seçeneğiniz de vardır. Bu makalede, C# kodunda kullanıcı arabirimleri ve kullanıcı Arabirimi öğeleri oluşturma temelleri şu konulara değineceğiz.

[![Visual Studio Mac Kod Düzenleyicisi için](xibless-ui-images/intro01.png "Visual Studio için Mac Kod Düzenleyicisi")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>Kodu kullanmak için bir pencere değiştirme

Yeni bir Xamarin.Mac Cocoa uygulaması oluşturduğunuzda, varsayılan olarak standart boş, bir pencere alın. Bu windows tanımlanmış bir **Main.storyboard** (veya geleneksel bir **MainWindow.xib**) otomatik olarak projeye dahil dosyası. Bu da içerir bir **ViewController.cs** uygulamanın ana görünüm yönetir dosya (veya yeniden geleneksel bir **MainWindow.cs** ve **MainWindowController.cs** dosyası).

Bir uygulama için bir Xibless penceresi geçmek için aşağıdakileri yapın:

1. Kullanarak durdurmak istediğiniz uygulamayı açmak `.stroyboard` veya Mac için Visual Studio kullanıcı arabirimi tanımlamak için .xib dosyaları
2. İçinde **çözüm paneli**, sağ tıklayın **Main.storyboard** veya **MainWindow.xib** dosya ve seçin **kaldırmak**: 

    ![Ana film şeridi veya pencere kaldırma](xibless-ui-images/switch01.png "ana film şeridi veya pencere kaldırma")
3. Gelen **Kaldır iletişim**, tıklatın **silmek** düğmesini .storyboard veya .xib projeden tamamen kaldırmak için: 

    ![Silme onayı](xibless-ui-images/switch02.png "silme onayı")

Şu şekilde değiştirmeniz gerekir artık **MainWindow.cs** pencerenin düzenini tanımlamak ve değiştirmek için dosya **ViewController.cs** veya **MainWindowController.cs** oluşturmak için dosya bir örneği bizim `MainWindow` .storyboard veya .xib dosyası artık kullanıyoruz beri sınıfı.

Film şeritleri kendi kullanıcı arabirimi otomatik olarak dahil için kullanan Modern Xamarin.Mac uygulamalar **MainWindow.cs**, **ViewController.cs** veya **MainWindowController.cs** dosyaları. Gerektiğinde, yalnızca yeni bir boş C# sınıf projeye ekleyin (**Ekle** > **yeni dosya …**   >  **Genel** > **boş sınıfı**) ve eksik dosya ile aynı adı. 


### <a name="defining-the-window-in-code"></a>Pencerenin kodda tanımlama

Ardından, düzenleme **MainWindow.cs** dosya ve şu şekilde görünür yapın:

```csharp
using System;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindow : NSWindow
    {
        #region Private Variables
        private int NumberOfTimesClicked = 0;
        #endregion

        #region Computed Properties
        public NSButton ClickMeButton { get; set;}
        public NSTextField ClickMeLabel { get ; set;}
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }

        public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
            // Define the user interface of the window here
            Title = "Window From Code";

            // Create the content view for the window and make it fill the window
            ContentView = new NSView (Frame);

            // Add UI elements to window
            ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
                AutoresizingMask = NSViewResizingMask.MinYMargin
            };
            ContentView.AddSubview (ClickMeButton);

            ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
                BackgroundColor = NSColor.Clear,
                TextColor = NSColor.Black,
                Editable = false,
                Bezeled = false,
                AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
                StringValue = "Button has not been clicked yet."
            };
            ContentView.AddSubview (ClickMeLabel);
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Wireup events
            ClickMeButton.Activated += (sender, e) => {
                // Update count
                ClickMeLabel.StringValue = (++NumberOfTimesClicked == 1) ? "Button clicked one time." : string.Format("Button clicked {0} times.",NumberOfTimesClicked);
            };
        }
        #endregion

    }
}
```

Şimdi anahtar öğeleri bazılarını tartışın.

Birkaç ilk olarak, eklediğimiz _hesaplanan özellikleri_ , davranacağı gibi çıkışlar (penceresi .storyboard veya .xib dosyasında gibi oluşturulduysa):

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

Bunlar bize penceresinde görüntülenecek kalacaklarını kullanıcı Arabirimi öğeleri erişmenizi sağlar. Pencerenin .storyboard veya .xib dosyasından şişirileceğini değil olduğundan, bu örneği için bir yol ihtiyacımız (daha sonra anlatıldığı gibi `MainWindowController` sınıfı). Bu yeni Oluşturucusu yöntemi yaptığı olmasıdır:

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

Burada biz pencerenin düzenini tasarlamak ve gerekli kullanıcı arabirimi oluşturmak için gereken herhangi bir kullanıcı Arabirimi öğeleri yerleştirmek budur. Tüm kullanıcı Arabirimi öğeleri için bir pencere eklemeden önce gereken bir _içerik görünümü_ öğeleri içerecek şekilde:

```csharp
ContentView = new NSView (Frame);
```

Bu pencereyi dolduracak bir içerik görünümü oluşturur. Bizim ilk kullanıcı Arabirimi öğesi eklediğimiz artık bir `NSButton`, penceresine:

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

Burada dikkat edilecek ilk şey iOS, macOS matematiksel gösterimini penceresinin koordinat sistemini tanımlamak için kullanmasıdır. Bu nedenle başlangıç noktasını sağ ve pencerenin üst sağ alt köşesindeki doğru değerleri artan penceresinin sol alt köşesinde kullanılıyor. Biz oluşturduğunuzda yeni `NSButton`, ekranda, konum ve boyut tanımlarız Biz bu dikkate alın.

`AutoresizingMask = NSViewResizingMask.MinYMargin` Özelliği düğmesi biz penceresi dikey olarak yeniden boyutlandırıldığında penceresinin üst ile aynı konumda kalmasını istediğinizi söyler. Yeniden, bu gereklidir çünkü (0,0) pencerenin altındaki sol tarafında değil.

Son olarak, `ContentView.AddSubview (ClickMeButton)` yöntemi ekler `NSButton` içerik uygulamayı çalıştırın ve görüntülenen pencere olduğunda, BT'nin ekranında görüntülenir şekilde görüntülemek için.

Bir etiket kaç kez görüntülenir penceresine sonraki eklenir, `NSButton` tıklatıldıktan: 

```csharp
ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
    BackgroundColor = NSColor.Clear,
    TextColor = NSColor.Black,
    Editable = false,
    Bezeled = false,
    AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
    StringValue = "Button has not been clicked yet."
};
ContentView.AddSubview (ClickMeLabel);
``` 

MacOS belirli bir sahip olmadığından _etiket_ UI öğesi ekledik özel stil, düzenlenemeyen `NSTextField` etiket olarak hareket edecek. Önce hesaba boyutunu ve konumunu alır düğmesi gibi yalnızca o (0,0) pencerenin altındaki sol tarafında olur. `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` Özelliğini kullanarak **veya** iki birleştirme işleci `NSViewResizingMask` özellikleri. Bu pencereyi dikey olarak yeniden boyutlandırıldığında penceresinin üst ile aynı konumda kalmasını ve küçültmek ve pencereyi yatay olarak yeniden boyutlandırılıp genişliği dağıttıkça etiketini hale getirir.

Yeniden `ContentView.AddSubview (ClickMeLabel)` yöntemi ekler `NSTextField` içerik onun ekranda uygulamayı çalıştırın ve penceresi açıldığında görüntülenir şekilde görüntülemek için.


### <a name="adjusting-the-window-controller"></a>Pencere denetleyicisi ayarlama

Tasarımını itibaren `MainWindow` artık yüklendiği bir .storyboard veya .xib dosyasından biz penceresi denetleyicisine bazı ayarlamalar yapmanız gerekir. Düzen **MainWindowController.cs** dosya ve şu şekilde görünür yapın:

```csharp
using System;

using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindowController : NSWindowController
    {
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindowController (NSCoder coder) : base (coder)
        {
        }

        public MainWindowController () : base ("MainWindow")
        {
            // Construct the window from code here
            CGRect contentRect = new CGRect (0, 0, 1000, 500);
            base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);

            // Simulate Awaking from Nib
            Window.AwakeFromNib ();
        }

        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }

        public new MainWindow Window {
            get { return (MainWindow)base.Window; }
        }
    }
}

```

Bu değişikliği temel öğeleri ele olanak tanır.

İlk olarak, yeni bir örneğini tanımlarız `MainWindow` sınıf ve taban penceresi denetleyicinin atayın `Window` özelliği:

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Ekran pencere konumunu tanımlarız bir `CGRect`. Yalnızca koordinat sistemi gibi penceresinin ekranın sol alt köşesindeki (0,0) tanımlar. Ardından, pencere stili kullanarak tanımlarız **veya** iki veya daha fazla birleştirmek için işleci `NSWindowStyle` özellikleri:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

Aşağıdaki `NSWindowStyle` özellikleri kullanılabilir duruma gelir:

- **Kenarlıksız** -penceresi hiçbir kenarlık sahip olur.
- **Başlıklı** -penceresinin başlık çubuğunu sahip olur.
- **Closable** -pencereyi kapat düğmesi bulunur ve kapatılabilir.
- **Miniaturizable** -penceresi Miniaturize düğmesi bulunur ve en aza indirgenebilir.
- **Yeniden boyutlandırılabilir** -penceresinde bir yeniden boyutlandırma düğmesi bulunur ve yeniden boyutlandırılabilir.
- **Yardımcı programı** -bir yardımcı stilini (Masası) penceresidir.
- **DocModal** -pencere bir Panel ise, belge kalıcı yerine sistem kalıcı olur. 
- **NonactivatingPanel** -pencere bir Panel ise, ana pencereyi yapılmayacak.
- **TexturedBackground** -penceresi doku arka planı olur.
- **Ölçeklendirilmemiş** -penceresi değil ölçeklendirilir.
- **UnifiedTitleAndToolbar** -penceresinin başlık ve araç çubuğunda alanları katılacak.
- **Hud** -penceresi bir ekran göstergesi olarak görüntüle panelinde görüntülenir.
- **FullScreenWindow** -penceresini tam ekran moduna girebilirsiniz.
- **FullSizeContentView** -pencerenin içerik başlık ve araç çubuğu alanı arkasındaki görünümüdür.

Son iki özellikleri tanımlama _arabelleğe alma türü_ penceresinin ve çizim penceresinin ertelendi. Daha fazla bilgi için `NSWindows`, lütfen bkz. Apple'nın [Windows Giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) belgeleri.

Pencerenin .storyboard veya .xib dosyasından şişirileceğini değil olduğundan, son olarak, içinde benzetimini yapmak ihtiyacımız bizim **MainWindowController.cs** windows çağırarak `AwakeFromNib` yöntemi:

```csharp
Window.AwakeFromNib ();
```

Bu, bir .storyboard veya .xib dosyasından yüklenen standart bir pencere yalnızca penceresi karşı kod yazmak için istediğiniz izin verir.


### <a name="displaying-the-window"></a>Pencerenin görüntüleme

.Storyboard ya da .xib dosyasıyla kaldırıldı ve **MainWindow.cs** ve **MainWindowController.cs** değiştiren, dosyaları olarak oluşturulmuş olan tüm normal penceresi gibi penceresini kullanarak Xcode'nın arabirimi Oluşturucu .xib dosyası ile.

Aşağıdaki pencere ve kendi denetleyicisi yeni bir örneğini oluşturur ve ekranda penceresini görüntüleyin:

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

Bu noktada, uygulamayı çalıştırın ve birkaç kez düğmesine tıklandığında, aşağıdaki görüntülenir:

![Çalıştıran bir örnek uygulama](xibless-ui-images/run01.png "bir örnek uygulamayı çalıştırma")


## <a name="adding-a-code-only-window"></a>Kod yalnızca penceresi ekleme

Biz yalnızca kodu, xibless pencere varolan Xamarin.Mac uygulama eklemek istiyorsanız, projeye sağ tıklayın **çözüm paneli** seçip **Ekle** > **yeni dosya...** . İçinde **yeni dosya** iletişim seçin **Xamarin.Mac** > **Cocoa penceresi denetleyicisiyle**aşağıda gösterildiği gibi:

![Yeni bir pencere denetleyicisi ekleme](xibless-ui-images/add01.png "yeni bir pencere denetleyicisi ekleme") 

Varsayılan .storyboard veya .xib dosyası projeden silersiniz önce olduğu gibi (Bu durumda **SecondWindow.xib**) ve adımları [kodu kullanmak için bir pencere değiştirme](#Switching_a_Window_to_use_Code) karşılamak için yukarıdaki bölümde Kod tanımı pencerenin.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>Kod penceresinde bir kullanıcı Arabirimi öğesi ekleme

Bir pencere kodda oluşturulmuş veya .storyboard veya .xib dosyasından yüklenen olup olmadığını zamanlar olabilir ki UI öğesi için bir pencere koddan eklemek istediğiniz yere. Örneğin:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Yukarıdaki kod yeni bir oluşturur `NSButton` ve ona ekler `MyWindow` görüntüleme penceresi örneği. Temel olarak Xcode'nın arabirimi Oluşturucu .storyboard veya .xib dosyasında tanımlanan herhangi bir kullanıcı Arabirimi öğe kodda oluşturulabilir ve bir pencerede görüntülenir.


## <a name="defining-the-menu-bar-in-code"></a>Menü çubuğu kodda tanımlama

Xamarin.Mac içinde geçerli sınırlamalar nedeniyle, Xamarin.Mac uygulamanızın menü çubuğu – oluşturmanız önerilir değil`NSMenuBar`– içindeki kod ancak kullanmaya devam **Main.storyboard** veya **MainMenu.xib** dosyasını tanımlayın. Bu, ekleme ve C# kodunda menüler ve menü öğeleri kaldırmak da belirtti.

Örneğin, düzenleme **AppDelegate.cs** dosya ve olun `DidFinishLaunching` yöntemi görünüm aşağıdaki gibi:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    // Create a Status Bar Menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Phrases";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Phrases");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        Console.WriteLine("Address Selected");
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        Console.WriteLine("Date Selected");
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        Console.WriteLine("Greetings Selected");
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        Console.WriteLine("Signature Selected");
    };
    item.Menu.AddItem (signature);
}
```

Yukarıdaki koddan bir durum çubuğu menü oluşturur ve uygulama başlatıldığında görüntüler. Menülerle çalışma hakkında daha fazla bilgi için lütfen bkz bizim [menüleri](~/mac/user-interface/menu.md) belgeleri.


## <a name="summary"></a>Özet

Bu makalede, Xcode'nın arabirimi oluşturucusuyla .storyboard veya .xib dosyalarıyla aksine C# kod Xamarin.Mac uygulamanın kullanıcı arabirimi oluşturma sırasında ayrıntılı bir bakış sürdü.



## <a name="related-links"></a>İlgili bağlantılar

- [MacXibless (örnek)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [Menüler](~/mac/user-interface/menu.md)
- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Windows giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
