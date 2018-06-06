---
title: Xamarin.Mac içinde özel denetimler oluşturma
description: Bu belge, Xamarin.Mac özel denetimlerinde yapı açıklar. Özel denetim oluşturmak, durumunu izlemek, onun arabirim çizin, kullanıcı girişine yanıt verir ve bir uygulamada denetimi kullanmak nasıl gösterir.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: e4c2b2c9ee7bae3d6489fec6b22881653ec53043
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792684"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Xamarin.Mac içinde özel denetimler oluşturma

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı erişiminiz kullanıcı denetimleri, içinde çalışan bir geliştirici *Objective-C*, *Swift* ve *Xcode* yapar . Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve kullanıcı denetimleri korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

MacOS yerleşik kullanıcı denetimleri çeşitlilikte sağlarken işlevselliği Giden kutusu sağlanmayan sağlamak ya da özel bir kullanıcı Arabirimi tema (örneğin, bir oyun arabirimi) eşleştirmek için özel bir denetim oluşturmak gerekir zamanlar olabilir.

[![](custom-controls-images/intro01.png "Özel bir kullanıcı Arabirimi denetim örneği")](custom-controls-images/intro01.png#lightbox)

Bu makalede, biz Xamarin.Mac uygulamasında yeniden kullanılabilir bir özel kullanıcı arabirimi denetim oluşturma temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Özel denetimler giriş

Yukarıda belirtildiği gibi yeniden kullanılabilir, özel kullanıcı arabirimi denetim Xamarin.Mac uygulamanızın kullanıcı Arabirimi benzersiz işlevselliği sağlamak ya da özel bir kullanıcı Arabirimi tema (örneğin, bir oyun arabirimi) oluşturmak için Oluştur gerektiğinde zamanlar olabilir.

Bu durumlarda, kolayca gelen devralabilirsiniz `NSControl` ve uygulamanızın kullanıcı Arabirimi aracılığıyla C# kodu veya Xcode'nın arabirimi oluşturucu aracılığıyla ya da eklenebilir özel bir araç oluşturun. İçinden devralma tarafından `NSControl` özel denetim otomatik olarak yerleşik bir kullanıcı arabirimi denetim sahip tüm standart özelliklere sahip olacaktır (gibi `NSButton`).

Özel kullanıcı arabirimi denetim bilgilerini (örneğin, bir özel grafik ve grafik aracı) yalnızca görüntüler, devralınan isteyebilirsiniz `NSView` yerine `NSControl`.

Hangi temel sınıf kullanılan olsun, özel bir denetim oluşturmak için temel adımlar aynıdır.

Bu makale işlem benzersiz bir kullanıcı arabirimi temasını ve tam olarak işlevsel bir özel kullanıcı arabirimi denetim oluşturmanın bir örnek sağlar özel bir anahtar Çevir bileşen oluşturun.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Özel denetim oluşturma

Biz oluşturmakta özel denetim (sol fare düğmesini tıklama) kullanıcı girişine yanıt vermesi olduğundan, biz devralınmalıdır olacak `NSControl`. Bu şekilde, bizim özel denetim otomatik olarak yerleşik bir kullanıcı arabirimi denetim sahip tüm standart özelliklere sahip ve standart macOS denetimi gibi yanıt.

Mac için Visual Studio'da bir özel kullanıcı arabirimi denetim için (veya yeni bir tane oluşturmak için) istediğiniz Xamarin.Mac projeyi açın. Yeni bir sınıf ekleyin ve bunu `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "Yeni bir sınıf ekleme")](custom-controls-images/custom01.png#lightbox)

Ardından, düzenleme `NSFlipSwitch.cs` sınıfı ve şu şekilde görünür yapın:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using AppKit;
using CoreGraphics;

namespace MacCustomControl
{
    [Register("NSFlipSwitch")]
    public class NSFlipSwitch : NSControl
    {
        #region Private Variables
        private bool _value = false;
        #endregion

        #region Computed Properties
        public bool Value {
            get { return _value; }
            set {
                // Save value and force a redraw
                _value = value;
                NeedsDisplay = true;
            }
        }
        #endregion

        #region Constructors
        public NSFlipSwitch ()
        {
            // Init
            Initialize();
        }

        public NSFlipSwitch (IntPtr handle) : base (handle)
        {
            // Init
            Initialize();
        }

        [Export ("initWithFrame:")]
        public NSFlipSwitch (CGRect frameRect) : base(frameRect) {
            // Init
            Initialize();
        }

        private void Initialize() {
            this.WantsLayer = true;
            this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
        }
        #endregion

        #region Draw Methods
        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Use Core Graphic routines to draw our UI
            ...

        }
        #endregion

        #region Private Methods
        private void FlipSwitchState() {
            // Update state
            Value = !Value;
        }
        #endregion

    }
}
```

Biz içinden devralma, bizim Özel sınıfı hakkında dikkat edilecek ilk şey `NSControl` ve kullanarak **kaydetmek** Objective-C ve Xcode'nın arabirimi Oluşturucu bu sınıfa kullanıma sunmak için komutu:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

Aşağıdaki bölümlerde, biz Yukarıdaki kod ayrıntılı rest göz atın.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Denetimin durumunu izleme

Bizim özel denetimi bir anahtar olmadığından, kimliğinizi anahtarının açık/kapalı durumunu izlemek için bir yol gerekiyor. Aşağıdaki kod ile işleme `NSFlipSwitch`:

```csharp
private bool _value = false;
...

public bool Value {
    get { return _value; }
    set {
        // Save value and force a redraw
        _value = value;
        NeedsDisplay = true;
    }
}
```

Anahtarın durumu değiştiğinde, güncelleştirilen kullanıcı arabirimini bir şekilde ihtiyacımız var. Kullanıcı Arabiriminde ile yeniden çizmek için Denetim zorlayarak bunu `NeedsDisplay = true`.

Bizim denetim, daha fazla tek bir açık/kapalı durumu (örneğin bir çok durumlu anahtarıyla 3 konumlar) gerekirse, biz kullanabilirdik bir **Enum** durumunu izlemek için. Bizim örneğimizde, basit bir için **bool** yapar.

Açma ve kapatma arasında geçiş durumunu değiştirmek için yardımcı bir yöntem de eklediğimiz:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Daha sonra biz zaman anahtarları durumu değişti çağıran bildirmek için bu yardımcı sınıf genişletin.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>Denetimin arabirimi çizme

Çalışma zamanında bizim özel denetimin kullanıcı arabirimi çizmek için çekirdek grafik çizim yordamları kullanma olacak. Biz bunu yapabilmeniz için önce biz bizim denetimi için katmanlardaki etkinleştirmeniz gerekir. Biz bunu, aşağıdaki özel yöntemiyle yapın:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Bu yöntem her denetim'ın düzgün yapılandırıldığından emin olmak için denetimin oluşturucular çağrılır. Örneğin:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Ardından, geçersiz kılmak ihtiyacımız `DrawRect` yöntemi ve denetimi çizmek için çekirdek grafik yordamları ekleyin:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Durumu değiştiğinde biz görsel denetimi için ayarlama (gelen giderek gibi **üzerinde** için **kapalı**). Herhangi bir durum değiştiğinde, biz kullanabilirsiniz `NeedsDisplay = true` denetimi için yeni durum yeniden boyutlandırmaya zorlamak için komutu.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Kullanıcı girişine yanıt

Bizim özel denetim için kullanıcı girişi ekleyebiliriz iki temel yolu vardır: **geçersiz kılma fare işleme yordamları** veya **hareketi tanıyıcıları**. Kullanıyoruz, hangi yöntemi temel alınacak bizim denetimi tarafından gerekli işlevselliği.

> [!IMPORTANT]
> Oluşturduğunuz tüm özel denetim için ya da kullanmalısınız **geçersiz kılma yöntemleri** _veya_ **hareketi tanıyıcıları**, ancak ikisini aynı anda zaman birbiriyle çelişen gibi.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Geçersiz kılma yöntemleri ile kullanıcı girişini işleme

Öğesinden devralan nesneler `NSControl` (veya `NSView`) birkaç işleme fare yöntemlerini geçersiz kılabilir veya klavye girişi. Bizim örnek denetimi için arasında geçiş durumu Çevir istiyoruz **üzerinde** ve **devre dışı** kullanıcı tıkladığında sol fare düğmesini denetimiyle üzerinde. Aşağıdaki yöntemleri geçersiz kılmak ekleyebiliriz `NSFliwSwitch` sınıfı bu durumu çözmek için:

```csharp
#region Mouse Handling Methods
// --------------------------------------------------------------------------------
// Handle mouse with Override Methods.
// NOTE: Use either this method or Gesture Recognizers, NOT both!
// --------------------------------------------------------------------------------
public override void MouseDown (NSEvent theEvent)
{
    base.MouseDown (theEvent);

    FlipSwitchState ();
}

public override void MouseDragged (NSEvent theEvent)
{
    base.MouseDragged (theEvent);
}

public override void MouseUp (NSEvent theEvent)
{
    base.MouseUp (theEvent);
}

public override void MouseMoved (NSEvent theEvent)
{
    base.MouseMoved (theEvent);
}
## endregion
```

Yukarıdaki kodda diyoruz `FlipSwitchState` yöntemi (yukarıda tanımlanan) sayfasında Çevir / anahtarda durumunu Kapalı `MouseDown` yöntemi. Bu ayrıca denetimi geçerli durumunu yansıtacak şekilde yeniden başlamaya zorlayacak.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Hareketi tanıyıcılarla kullanıcı girişini işleme

İsteğe bağlı olarak, denetimi ile etkileşim kullanıcı işlemek için hareketi tanıyıcıları kullanabilirsiniz. Yukarıda eklediğiniz geçersiz kılmaları kaldırmak, düzenleme `Initialize` yöntemi ve şu şekilde görünür yapın:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;

    // --------------------------------------------------------------------------------
    // Handle mouse with Gesture Recognizers.
    // NOTE: Use either this method or the Override Methods, NOT both!
    // --------------------------------------------------------------------------------
    var click = new NSClickGestureRecognizer (() => {
        FlipSwitchState();
    });
    AddGestureRecognizer (click);
}
```

Burada, yeni bir oluşturuyoruz `NSClickGestureRecognizer` ve çağırma bizim `FlipSwitchState` kullanıcı üzerinde sol fare düğmesini tıklattığında anahtarın durumu değiştirmek için yöntem. `AddGestureRecognizer (click)` Yöntemi denetime hareketi tanıyıcı ekler.

Yeniden ne biz bizim sahip özel bir denetim elde etmeye çalıştığınız kullanıyoruz hangi yöntemi bağlıdır. Biz düşük düzey erişmesi gerekiyorsa geçersiz kılma yöntemleri kullanıcı etkileşimi için kullanın. Fare tıklamaları gibi önceden tanımlanmış işlevselliği ihtiyacımız hareketi tanıyıcıları kullanın.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>Durum değişikliği olayları için yanıt verme

Kullanıcı, özel bir denetim durumu değiştiğinde, kodda durumu değişikliği için yanıt için bir yol ihtiyacımız (bir şey yapmak gibi özel bir düğmesine tıklar). 

Bu işlevselliği sağlayacak şekilde Düzenle `NSFlipSwitch` sınıfı ve aşağıdaki kodu ekleyin:

```csharp
#region Events
public event EventHandler ValueChanged;

internal void RaiseValueChanged() {
    if (this.ValueChanged != null)
        this.ValueChanged (this, EventArgs.Empty);

    // Perform any action bound to the control from Interface Builder
    // via an Action.
    if (this.Action !=null) 
        NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
}
## endregion
```

Ardından, düzenleme `FlipSwitchState` yöntemi ve şu şekilde görünür yapın:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

İlk olarak, size sağladığımız bir `ValueChanged` olay böylece kullanıcı anahtarının durumunu değiştirdiğinde bir eylemi gerçekleştirmek size bir işleyiciye C# kodunda ekleyebilirsiniz.

Bizim özel denetim sınıfından devraldığı için ikinci `NSControl`, otomatik olarak sahip bir **eylem** Xcode'nın arabirimi Oluşturucusu'nda atanabilir. Bu çağrı için **eylem** durumu değiştiğinde, aşağıdaki kodu kullanın:

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

İlk olarak, biz olup olmadığını denetleyin bir **eylem** denetime atanmış. Ardından, diyoruz **eylem** onu tanımlanmışsa.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>Özel denetim kullanma

Tam olarak tanımlanan bizim sahip özel bir denetim, biz ya da bu bizim Xamarin.Mac uygulamanın kullanıcı arabirimini kullanarak C# kodu veya Xcode'nın arabirimi Oluşturucu ekleyebilirsiniz.

Arabirimi Oluşturucusu'nu kullanarak denetim eklemek için önce Xamarin.Mac projesinin temiz bir yapı yapın ve ardından `Main.storyboard` dosyayı düzenleme için arabirimi Oluşturucusu'nda açın:

[![](custom-controls-images/custom02.png "Xcode'da film şeridi düzenleme")](custom-controls-images/custom02.png#lightbox)

Ardından, sürükleyin bir `Custom View` kullanıcı arabirimi tasarımı içine:

[![](custom-controls-images/custom03.png "Özel bir görünüm Kitaplığı'ndan seçme")](custom-controls-images/custom03.png#lightbox)

Halen seçili olan özel görünüm ile geçiş **kimlik denetçisi** görünümün değiştirip **sınıfı** için `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "Ayar görünümün sınıfı")](custom-controls-images/custom04.png#lightbox)

Geçiş **Yardımcısı Düzenleyicisi** ve oluşturma bir **çıkışı** özel denetim için (içinde bağlamak emin `ViewControler.h` dosya ve `.m` dosya):

[![](custom-controls-images/custom05.png "Yeni bir çıkış yapılandırma")](custom-controls-images/custom05.png#lightbox)

Mac için Visual Studio dönün yaptığınız değişiklikleri kaydedin ve eşitlemek değişikliklere izin verecek. Düzen `ViewController.cs` dosya ve olun `ViewDidLoad` yöntemi görünüm aşağıdaki gibi:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Do any additional setup after loading the view.
    OptionTwo.ValueChanged += (sender, e) => {
        // Display the state of the option switch
        Console.WriteLine("Option Two: {0}", OptionTwo.Value);
    };
}
``` 

Burada, biz yanıt `ValueChanged` biz yukarıda üzerinde tanımlanan olay `NSFlipSwitch` sınıfı ve geçerli yazma **değeri** kullanıcı denetimi tıklattığında.

İsteğe bağlı olarak, biz arabirimi oluşturucuya dönün ve tanımlayan bir **eylem** denetimi:

[![](custom-controls-images/custom06.png "Yeni bir eylem yapılandırma")](custom-controls-images/custom06.png#lightbox)

Yeniden düzenleme `ViewController.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Ya da kullanması gereken **olay** veya tanımlayan bir **eylem** arabirimi Oluşturucu, ancak aynı anda iki yöntem kullanmamalıdır veya birbiriyle çelişen.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, yeniden kullanılabilir bir özel kullanıcı arabirimi denetim Xamarin.Mac uygulamasında oluşturma sırasında ayrıntılı bir bakış sürdü. Özel denetimler UI, başlıca iki yolu fare ve kullanıcı girişine yanıt çizin ve yeni denetim eylemlerine Xcode'nın arabirimi Oluşturucusu'nda kullanıma gördük.

## <a name="related-links"></a>İlgili bağlantılar

- [MacCustomControl (örnek)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Veri Bağlama ve Anahtar-Değer Kodlaması](~/mac/app-fundamentals/databinding.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Fare olayları işleme](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
