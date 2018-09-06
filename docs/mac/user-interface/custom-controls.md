---
title: Xamarin.Mac içinde özel denetimler oluşturma
description: Bu belge, özel denetimlerinde Xamarin.Mac nasıl oluşturulduğu açıklanır. Bu özel denetimi oluşturun, durumunu izlemek, arayüz çizme, kullanıcı girişine yanıt vermek ve bir denetim kullanmak nasıl gösterir.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7180dd80c80515adc7312e2fe30d69852bf04eaa
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780595"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Xamarin.Mac içinde özel denetimler oluşturma

Bir Xamarin.Mac uygulamasında çalışırken, C# ve .NET ile aynı erişiminiz kullanıcı denetimleri, içinde çalışan bir geliştirici *Objective-C*, *Swift* ve *Xcode* yok . Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Xcode'un kullanabileceğiniz _arabirim Oluşturucu_ oluşturmak ve korumak, kullanıcı denetimleri (veya isteğe bağlı olarak bunları doğrudan C# kodu oluşturmak için).

MacOS çok sayıda yerleşik kullanıcı denetimleri sağlarken işlevselliğini kullanıma hazır sağlanmayan sağlamak ya da özel bir kullanıcı Arabirimi tema (örneğin, bir oyun arabirimi) eşleştirmek için özel bir denetim oluşturmak için ihtiyacınız zamanlar olabilir.

[![](custom-controls-images/intro01.png "Özel kullanıcı Arabirimi denetim örneği")](custom-controls-images/intro01.png#lightbox)

Bu makalede, biz bir Xamarin.Mac uygulamada yeniden kullanılabilir bir özel kullanıcı arabirimi denetim oluşturma hakkındaki temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Özel denetimler giriş

Yukarıda belirtildiği gibi yeniden kullanılabilir, özel kullanıcı arabirimi denetimi Xamarin.Mac uygulamanızın kullanıcı Arabirimi için benzersiz işlevselliğini sunmak veya özel kullanıcı Arabirimi tema (örneğin, bir oyun arabirimi) oluşturma oluşturmanız gerektiğinde zamanlar olabilir.

Bu gibi durumlarda, kolayca öğesinden devralabilir `NSControl` ve uygulamanızın kullanıcı arabirimi aracılığıyla Xcode'un arabirim oluşturucu veya C# kodu aracılığıyla eklenebilir ya da özel bir araç oluşturabilir. Öğesinden devralan tarafından `NSControl` özel denetiminizi otomatik olarak yerleşik bir kullanıcı arabirimi denetimi olan standart özelliklerin tümünü olacaktır (gibi `NSButton`).

Özel kullanıcı arabirimi denetim bilgileri (örneğin, bir özel grafiği ve grafik aracı) yalnızca görüntüler, devralınan isteyebilirsiniz `NSView` yerine `NSControl`.

Hangi temel sınıf kullanılan ne olursa olsun, özel bir denetim oluşturmak için temel adımlar aynıdır.

Bu makalede işlem içinde benzersiz bir kullanıcı arabirimi teması ve tam işlevsel bir özel kullanıcı arabirimi denetimi oluşturma örneği sağlar özel bir anahtara çevirme bileşen oluşturun.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Özel denetim oluşturma

Biz oluşturmakta olduğunuz özel denetimin kullanıcı girişine (sol fare düğmesine tıklama) yanıt vermesi olduğundan, devralınan kullanacağız `NSControl`. Bu şekilde, özel denetimimiz otomatik olarak tüm standart özelliklerin yerleşik bir kullanıcı arabirimi denetimi vardır ve standart macOS denetimi gibi yanıt.

Mac için Visual Studio'da bir özel kullanıcı arabirimi denetimi (veya yeni bir tane oluşturun) istediğiniz Xamarin.Mac projeyi açın. Yeni bir sınıf ekleyin ve onu çağırmak `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "Yeni sınıf ekleme")](custom-controls-images/custom01.png#lightbox)

Ardından, Düzenle `NSFlipSwitch.cs` sınıfı ve aşağıdaki gibi görünmesi:

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

Biz öğesinden devralan, bizim Özel sınıfı hakkında fark edilecek ilk şey `NSControl` ve kullanarak **kaydetme** Objective-C ve Xcode'un arabirim Oluşturucu bu sınıfa kullanıma sunmak için komut:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

Aşağıdaki bölümlerde, size ayrıntılı Yukarıdaki kod, bekleyen göz atacağız.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Denetimin durumunu izleme

Özel Denetimimiz anahtar olduğundan, anahtarın açık/kapalı durumu izlemek için bir yol ihtiyacımız var. Aşağıdaki kod ile işleme `NSFlipSwitch`:

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

Anahtarın durumu değiştiğinde, güncelleştirilmiş kullanıcı arabirimini bir şekilde ihtiyacımız var. Denetimin, kullanıcı Arabirimiyle yeniden çizmek için zorlayarak bunu `NeedsDisplay = true`.

Denetimimiz daha fazlası tek bir açık/kapalı durumu (örneğin bir çok durumlu anahtar ile 3 konumlar) gerektiriyorsa, biz kullanabilirdik bir **Enum** durumunu izlemek için. Basit bir Örneğimiz için **bool** yapar.

Açma ve kapatma arasında geçiş durumunu değiştirmek için bir yardımcı yöntem de ekledik:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Daha sonra biz çağırana zaman anahtarlar durumu değiştirildiğinde bildirmek için bu yardımcı sınıf genişletin.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>Denetimin arabirimi çizme

Çalışma zamanında kullanıcı arabirimi, özel denetimin çizmek için temel grafik çizim yordamları kullanmak için kullanacağız. Biz bunu yapmak için önce denetimimiz katmanları etkinleştirmek ihtiyacımız var. Aşağıdaki gizli yöntemi ile bunu:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Bu yöntem, her denetimi düzgün yapılandırıldığından emin olmak için denetimin oluşturucular çağrılır. Örneğin:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Ardından, geçersiz kılmak ihtiyacımız `DrawRect` yöntemi ve bir denetim çizmek için temel grafik yordamlar ekleyin:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Durumunu değiştirdiğinde biz denetimin görsel bir temsili ayarlama (gitmek gibi **üzerinde** için **kapalı**). Durum değişiklikleri için istediğiniz zaman, kullanabiliriz `NeedsDisplay = true` yeni durumu yeniden çizmek için Denetim zorlamak için komutu.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Kullanıcı girişine yanıt

Bizim özel denetimin kullanıcı girişine ekleyebiliriz iki temel yolu vardır: **geçersiz kılma fare işleme rutinleri** veya **hareket tanıyıcılar**. Kullandığımız hangi yöntemin göre oluşturulacak denetimimiz tarafından gerekli işlevselliği.

> [!IMPORTANT]
> Oluşturduğunuz tüm özel denetim için ya da kullanmalısınız **geçersiz kılma yöntemleri** _veya_ **hareket tanıyıcılar**, ancak ikisini birden aynı saat olarak birbiriyle çakışabilir.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Geçersiz kılma yöntemleri ile kullanıcı girişini işleme

Devralınan nesnelerin `NSControl` (veya `NSView`) birkaç işleme fare yöntemleri geçersiz kılmak veya klavye girişi. Arasında geçiş durumu ters çevirmek istediğimiz örnek denetimimiz **üzerinde** ve **kapalı** kullanıcı tıkladığında farenin sol düğmesi denetimi. Aşağıdaki yöntemleri geçersiz kılmak ekleyebiliriz `NSFliwSwitch` sınıfı bu durumu çözmek için:

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

Yukarıdaki kodda diyoruz `FlipSwitchState` yöntemi (yukarıda tanımlanan) açık çevirmek / anahtarı durumu kapalı `MouseDown` yöntemi. Bu, geçerli durumu yansıtacak şekilde çizilmesini denetimi de zorlar.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Hareket tanıyıcılar ile kullanıcı girişini işleme

İsteğe bağlı olarak, denetimle etkileşime geçen kullanıcının işlemek için hareket tanıyıcılar kullanabilirsiniz. Yukarıda eklediğiniz geçersiz kılmaları kaldırmak için Düzenle `Initialize` yöntemi ve aşağıdaki gibi görünmesi:

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

Burada, yeni bir oluşturuyoruz `NSClickGestureRecognizer` ve çağırma bizim `FlipSwitchState` kullanıcı üzerinde ile farenin sol düğmesine tıkladığında, anahtarın durumu değiştirmek için yöntemi. `AddGestureRecognizer (click)` Yöntemi hareket tanıyıcı denetimine ekler.

Yeniden kullandığımız hangi yöntemin ne özel denetimimiz gerçekleştirmek çalışıyoruz bağlıdır. Düşük düzey erişim ihtiyacımız olursa kullanıcı etkileşimi için geçersiz kılma yöntemleri kullanın. Fare tıklama gibi önceden tanımlanmış işlevselliği ihtiyacımız hareket tanıyıcılar kullanın.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>Durum değişikliği olayları için yanıt verme

Kullanıcı özel denetimimiz durumunu değiştirdiğinde, bir şekilde kod durumu değişikliği yanıt ihtiyacımız (bir şey yapmak gibi bir özel düğmesine tıklar). 

Bu işlevselliği sağlayacak şekilde düzenleme `NSFlipSwitch` sınıfı ve aşağıdaki kodu ekleyin:

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

Ardından, Düzenle `FlipSwitchState` yöntemi ve aşağıdaki gibi görünmesi:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

İlk olarak sağladığımız bir `ValueChanged` olay kullanıcı anahtarın durumu değiştiğinde bir eylem gerçekleştirebiliriz böylece biz bir işleyici C# kodu ekleyebilirsiniz.

İkinci özel denetimimiz öğesinden devralındığından `NSControl`, otomatik olarak sahip bir **eylem** Xcode'un arabirimi Oluşturucu'da atanabilir. Bu çağrı için **eylem** durumu değiştiğinde, biz aşağıdaki kodu kullanın:

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

İlk olarak biz olup olmadığını denetleriz bir **eylem** denetime atanmış. Ardından, diyoruz **eylem** tanımlanmadan durumunda.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>Özel denetim kullanarak

Tam olarak tanımlanan özel denetimimiz ya da C# kodunu kullanarak Xamarin.Mac uygulama kullanıcı Arabirimi veya Xcode'un arabirim Oluşturucu ekleyebiliriz.

Arabirim Oluşturucu kullanarak denetimi eklemek için ilk Xamarin.Mac proje temiz bir derlemesini, ardından çift `Main.storyboard` dosyayı düzenleme için arabirim Oluşturucu'da açın:

[![](custom-controls-images/custom02.png "Xcode içinde bir film şeridini düzenleme")](custom-controls-images/custom02.png#lightbox)

Sonraki adımda bir `Custom View` kullanıcı arabirimi tasarımı olarak:

[![](custom-controls-images/custom03.png "Kitaplığından özel bir görünüm seçme")](custom-controls-images/custom03.png#lightbox)

Seçili durumdayken özel görünüm ile geçiş **kimlik denetçisi** görünümün değiştirip **sınıfı** için `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "Ayar görünümün sınıfı")](custom-controls-images/custom04.png#lightbox)

Geçiş **Yardımcısı Düzenleyicisi** oluşturup bir **çıkışı** özel denetim için (içinde bağlamak emin `ViewControler.h` dosya ve `.m` dosya):

[![](custom-controls-images/custom05.png "Yeni çıkış yapılandırma")](custom-controls-images/custom05.png#lightbox)

Mac için Visual Studio geri dönüp yaptığınız değişiklikleri kaydetmek ve değişiklikleri eşitlemeye izin verin. Düzen `ViewController.cs` dosya ve olun `ViewDidLoad` yöntemi görünüm aşağıdaki gibi:

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

Burada, için yanıtlarız `ValueChanged` tanımladığımız yukarıda üzerinde olay `NSFlipSwitch` sınıfı ve geçerli yazma **değer** kullanıcı denetimi tıklattığında.

İsteğe bağlı olarak, biz için arabirim Oluşturucu dönün ve tanımlayan bir **eylem** denetimi:

[![](custom-controls-images/custom06.png "Yeni bir eylem yapılandırma")](custom-controls-images/custom06.png#lightbox)

Yeniden düzenleme `ViewController.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Ya da kullanmanız gerekir **olay** veya tanımlayan bir **eylem** arabirimi Oluşturucu, ancak aynı anda iki yöntem de kullanmamalıdır veya birbiriyle çakışabilir.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, yeniden kullanılabilir bir özel kullanıcı arabirimi denetimi bir Xamarin.Mac uygulamasında oluşturmaya ayrıntılı bakış duruma getirdi. Xcode'un arabirim Oluşturucu eylemleri yeni denetime nasıl sunacağınızı öğrenin ve fare ve kullanıcı girişi için yanıt vermek için özel denetimler kullanıcı Arabirimi, başlıca iki yolu çizmek nasıl gördük.

## <a name="related-links"></a>İlgili bağlantılar

- [MacCustomControl (örnek)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Veri Bağlama ve Anahtar-Değer Kodlaması](~/mac/app-fundamentals/databinding.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Fare olayları işleme](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
