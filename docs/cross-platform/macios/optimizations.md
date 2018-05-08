---
title: En iyi duruma getirme derleme
description: Bu belgede Xamarin.iOS ve Xamarin.Mac uygulamalar için derleme zamanında uygulanan çeşitli iyileştirmeler açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
dateupdated: 04/16/2018
ms.openlocfilehash: 42d9903e75267a9578fb320b0bc532ad4f0fd71a
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="build-optimizations"></a>En iyi duruma getirme derleme

Bu belgede Xamarin.iOS ve Xamarin.Mac uygulamalar için derleme zamanında uygulanan çeşitli iyileştirmeler açıklanmaktadır.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>UIApplication.EnsureUIThread Kaldır / NSApplication.EnsureUIThread

Çağrı kaldırır [UIApplication.EnsureUIThread] [ 1] (için Xamarin.iOS) veya `NSApplication.EnsureUIThread` (için Xamarin.Mac).

Bu en iyi duruma getirme aşağıdaki kod türde değişir:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

Aşağıdaki:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

Varsayılan sürüm için etkin olarak oluşturur.

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=[+|-]remove-uithread-checks` mtouch/mmp için.

[1]: https://developer.xamarin.com/api/member/UIKit.UIApplication.EnsureUIThread/

## <a name="inline-intptrsize"></a>Satır içi IntPtr.Size

Inlines sabit değer olarak `IntPtr.Size` hedef platformlara göre.

Bu en iyi duruma getirme aşağıdaki kod türde değişir:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

(64-bit platformu için oluştururken) aşağıdaki noktaları:

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

Varsayılan olarak tek bir mimari hedefleme varsa etkinleştirildiğinden ya da platform derleme (**Xamarin.iOS.dll**, **Xamarin.TVOS.dll**, **Xamarin.WatchOS.dll** veya  **Xamarin.Mac.dll**).

Birden fazla mimari hedefleme, bu en iyi duruma getirme 32-bit sürümü ve uygulama 64-bit sürümü için farklı derlemeleri oluşturur ve her iki sürümünü de etkili bir şekilde azaltılması yerine son uygulama boyutunun artırılması, uygulama bulunması gerekir .

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=[+|-]inline-intptr-size` mtouch/mmp için.

## <a name="inline-nsobjectisdirectbinding"></a>Satır içi NSObject.IsDirectBinding

`NSObject.IsDirectBinding` belirli bir örneği bir sarmalayıcı türü olup olmadığını belirleyen bir örnek özelliği (bir sarmalayıcı türü bir yerel türüyle eşleyen bir yönetilen türü; örnek yönetilen `UIKit.UIView` yazın yerel eşlenir `UIView` türü - karşıtı olduğu bir kullanıcı türü Bu durumda `class MyUIView : UIKit.UIView` bir kullanıcı türü olacaktır).

Değerini bilmeniz gereken `IsDirectBinding` değeri hangi sürümünün belirlediğinden Objective-C çağrılırken `objc_msgSend` kullanmak için.

Yalnızca aşağıdaki kodu verilir:

```csharp
class UIView : NSObject {
    public virtual string SomeProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class NSUrl : NSObject {
    public virtual string SomeOtherProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class MyUIView : UIView {
}
```

Biz, belirleyebilirsiniz `UIView.SomeProperty` değerini `IsDirectBinding` bir sabit değil ve satır içi olamaz:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Ancak, tüm türleri uygulamasında bakın ve devralınmalıdır türü yok belirlemek mümkün olması `NSUrl`, ve bu nedenle satır içi daha güvenlidir `IsDirectBinding` bir sabit değere `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

Özellikle, bu en iyi duruma getirme kod aşağıdaki türde değişir (Bu bağlama kodudur `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Aşağıdaki içine (zaman onu belirlenebilir hiçbir alt sınıflarının olduğunu `NSUrl` uygulamasında):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

Bu her zaman Xamarin.iOS için varsayılan olarak etkindir ve her zaman Xamarin.Mac için varsayılan olarak devre dışı (dinamik olarak Xamarin.Mac derlemelerde yüklemek mümkün olduğundan, belirli bir sınıfın hiçbir zaman sınıflandırma belirlemek mümkün değildir).

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=[+|-]inline-isdirectbinding` mtouch/mmp için.

## <a name="inline-runtimearch"></a>Satır içi Runtime.Arch

Bu en iyi duruma getirme aşağıdaki kod türde değişir:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

(aygıt için oluştururken) aşağıdaki noktaları:

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

Xamarin.iOS (Xamarin.Mac için kullanılamaz) için varsayılan olarak her zaman etkindir.

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=[+|-]inline-runtime-arch` mtouch için.

## <a name="dead-code-elimination"></a>Ölü kod eleme

Bu en iyi duruma getirme aşağıdaki kod türde değişir:

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

içine:

```csharp
Console.WriteLine ("Doing this");
```

Ayrıca, bu gibi sabit karşılaştırmaları de değerlendirir:

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

ve belirleyen ifade `8 == 8` olan bir her zaman doğru ve azaltmak için:

```csharp
Console.WriteLine ("Doing this");
```

Aşağıdaki kod türünü dönüştürebilirsiniz satır içi kullanım iyileştirmeleri ile birlikte kullanıldığında güçlü bir iyileştirme olmasıdır (Bu bağlama kodudur `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

```csharp
NSRange ret;
if (IsDirectBinding) {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret (out ret, this.Handle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    }
} else {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret (out ret, this.SuperHandle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    }
}
return ret;
```

Bu içine (64-bit aygıt için oluştururken ve ayrıca mümkün olduğunda olmadığından emin olun hiçbir `NFCIso15693ReadMultipleBlocksConfiguration` uygulama sınıflarında):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Uygulama Nesne AĞACI derleyici zaten şöyle ölü kod ortadan kaldırmak mümkün olmakla birlikte, bu en iyi duruma getirme artık kullanılmaz ve bu nedenle (başka bir yerde kullanılan sürece) kaldırılabilir birden çok yöntem bulunmaktadır görmeye bağlayıcı anlamına bağlayıcı içinde yapılır :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

(Bağlayıcı etkinleştirildiğinde) her zaman varsayılan olarak etkindir.

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=[+|-]dead-code-elimination` mtouch/mmp için.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>BlockLiteral.SetupBlock çağrıları en iyi duruma getirme

Xamarin.iOS/Mac çalışma zamanı, temsilci için yönetilen bir Objective-C bloğu oluştururken blok imza bilmek ister. Bu oldukça pahalı bir işlem olabilir. Bu en iyi duruma getirme blok imza derleme zamanında hesaplamak ve çağırmak için IL değiştirme bir `SetupBlock` yönteminin imzasını bunun yerine bağımsız değişken olarak alan. Bunun yapılması, çalışma zamanında imza hesaplamak için gereken önler.

Bu bir blok 10-15 faktörüyle çağırma hızlandırır kıyaslamaları gösterir.

Aşağıdaki dönüştüren [kod](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

içine:

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

Statik kayıt (statik kayıt aygıt yapılar için varsayılan olarak etkindir ve statik kayıt sürüm için varsayılan olarak etkindir Xamarin.Mac derlemeler Xamarin.iOS içinde) kullanırken, varsayılan olarak etkindir.

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=[+|-]blockliteral-setupblock` mtouch/mmp için.

## <a name="optimize-support-for-protocols"></a>Destek protokoller için en iyi duruma getirme

Xamarin.iOS/Mac çalışma zamanı nasıl yönetilen türleri uygulayan Objective-C protokolleri hakkında bilgi gerekiyor. Bu bilgiler arabirimleri (ve bu arabirimleri özniteliklerinde), depolanan çok verimli bir biçimde değil veya bağlayıcı kolay değil.

Bir örnektir bu arabirimleri tüm Protokolü üyelerinde hakkındaki bilgileri depolamak bir `[ProtocolMember]` başvurular bu üyeler parametre türleri için başka şeylerin özniteliği. Bunun anlamı yalnızca bu tür arabirimi uygulama bu arabiriminde kullanılan tüm türleri korumak bağlayıcı yapar, isteğe bağlı üyeleri için bile uygulama hiçbir zaman çağıran veya uygular.

Bu iyileştirme gerekli bilgilerin kolay ve hızlı çalışma zamanında bulmak çok az bellek kullanır verimli bir biçimde depolamak statik kayıt hale getirir.

Bu mutlaka bu arabirimleri ya da ilgili özniteliklerin herhangi birini korumak gerekmez, bu bağlayıcı öğretir.

Bu iyileştirme bağlayıcı ve statik kayıt etkin olmasını gerektirir.

Bağlayıcı ve statik kayıt etkin olduğunda Xamarin.iOS üzerinde bu en iyi duruma getirme varsayılan olarak etkindir.

Xamarin.Mac üzerinde bu en iyi duruma getirme derlemeleri dinamik olarak yükleme Xamarin.Mac destekler ve bu derlemeler edilmemiş derleme zamanında bilinen (ve dolayısıyla en iyi duruma getirilmezse çünkü) varsayılan olarak hiçbir zaman etkindir.

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=-register-protocols` mtouch/mmp için.

## <a name="remove-the-dynamic-registrar"></a>Dinamik kayıt kaldırma

Xamarin.iOS ve Xamarin.Mac çalışma zamanı için destek içerir [yönetilen türlerini kaydetme](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) Objective-C çalışma zamanı. Derleme zamanında veya çalışma zamanında (veya kısmen derleme zamanında ve çalışma zamanında rest) ya da yapılabilir, ancak tamamen derleme zamanında yapılır, çalışma zamanında yapmak için destek kodunu kaldırılabilir demektir. Bu uygulama boyutu, önemli bir düşüş özellikle uzantıları gibi daha küçük uygulamaları veya watchOS uygulamalar için sonuçlanır.

Bu en iyi duruma getirme statik kayıt ve bağlayıcı etkin olmasını gerektirir.

Bağlayıcı dinamik kayıt ve kaldırmak deneyecek şekilde IF kaldırmak güvenli olup olmadığını belirlemek deneyecek.

Xamarin.Mac yükleme derlemeler (hangi derleme zamanında bilinen değil) çalışma zamanında dinamik olarak desteklediğinden, derleme zamanında bu güvenli iyileştirme olup olmadığını belirlemek mümkün değildir. Başka bir deyişle, bu en iyi duruma getirme Xamarin.Mac uygulamalar için varsayılan olarak hiçbir zaman etkindir.

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=[+|-]remove-dynamic-registrar` mtouch/mmp için.

Dinamik kayıt kaldırmak için varsayılan kılınırsa, algılarsa bağlayıcı uyarıları yayma güvenli değildir (ancak dinamik kayıt hala kaldırılacak).

## <a name="inline-runtimedynamicregistrationsupported"></a>Satır içi Runtime.DynamicRegistrationSupported

Inlines değeri, `Runtime.DynamicRegistrationSupported` derleme zamanında belirlenen.

Dinamik kayıt kaldırılırsa (bkz [dinamik kayıt kaldırma](#remove-the-dynamic-registrar) iyileştirme), bu bir sabit değer `false` değerini, aksi takdirde bir sabit değer `true` değeri.

Bu en iyi duruma getirme aşağıdaki kod türde değişir:

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

dinamik kayıt kaldırıldığında aşağıdaki noktaları:

```csharp
throw new Exception ("dynamic registration is not supported");
```

dinamik kayıt değil kaldırıldığında aşağıdaki noktaları:

```csharp
Console.WriteLine ("do something");
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

(Bağlayıcı etkinleştirildiğinde) her zaman varsayılan olarak etkindir.

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=[+|-]inline-dynamic-registration-supported` mtouch/mmp için.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Objective-C blokları yönetilen temsilciler oluşturmak için yöntem önceden hesaplar

Objective-C Seçici, çağırdığında bir blok bir parametre olarak alır ve ardından yönetilen kod kılınmadı Xamarin.iOS yöntemi sahip / Xamarin.Mac çalışma zamanı bloğu için bir temsilci oluşturmanız gerekiyor.

Bağlama Oluşturucu tarafından oluşturulan bağlama kodu içerecek bir `[BlockProxy]` türüyle belirten özniteliği bir `Create` bunu yapabilirsiniz yöntemi.

Aşağıdaki Objective-C kodunu verilen:

```objc
@interface ObjCBlockTester : NSObject {
}
-(void) classCallback: (void (^)())completionHandler;
-(void) callClassCallback;
@end

@implementation ObjCBlockTester
-(void) classCallback: (void (^)())completionHandler
{
}

-(void) callClassCallback
{
    [self classCallback: ^()
    {
        NSLog (@"called!");
    }];
}
@end
```

ve aşağıdaki bağlama kodu:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

Oluşturucunun üretir:

```csharp
[Register("ObjCBlockTester", true)]
public unsafe partial class ObjCBlockTester : NSObject {
    // unrelated code...

    [Export ("callClassCallback")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public virtual void CallClassCallback ()
    {
        if (IsDirectBinding) {
            ApiDefinition.Messaging.void_objc_msgSend (this.Handle, Selector.GetHandle ("callClassCallback"));
        } else {
            ApiDefinition.Messaging.void_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("callClassCallback"));
        }
    }
    
    [Export ("classCallback:")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public unsafe virtual void ClassCallback ([BlockProxy (typeof (Trampolines.NIDActionArity1V0))] System.Action completionHandler)
    {
        // ...
        
    }
}

static class Trampolines
{
    [UnmanagedFunctionPointerAttribute (CallingConvention.Cdecl)]
    [UserDelegateType (typeof (System.Action))]
    internal delegate void DActionArity1V0 (IntPtr block);

    static internal class SDActionArity1V0 {
        static internal readonly DActionArity1V0 Handler = Invoke;
        
        [MonoPInvokeCallback (typeof (DActionArity1V0))]
        static unsafe void Invoke (IntPtr block) {
            var descriptor = (BlockLiteral *) block;
            var del = (System.Action) (descriptor->Target);
            if (del != null)
                del (obj);
        }
    }
    
    internal class NIDActionArity1V0 {
        IntPtr blockPtr;
        DActionArity1V0 invoker;
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe NIDActionArity1V0 (BlockLiteral *block)
        {
            blockPtr = _Block_copy ((IntPtr) block);
            invoker = block->GetDelegateForBlock<DActionArity1V0> ();
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        ~NIDActionArity1V0 ()
        {
            _Block_release (blockPtr);
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe static System.Action Create (IntPtr block)
        {
            if (block == IntPtr.Zero)
                return null;
            if (BlockLiteral.IsManagedBlock (block)) {
                var existing_delegate = ((BlockLiteral *) block)->Target as System.Action;
                if (existing_delegate != null)
                    return existing_delegate;
            }
            return new NIDActionArity1V0 ((BlockLiteral *) block).Invoke;
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        unsafe void Invoke ()
        {
            invoker (blockPtr);
        }
    }
}
```

Objective-C çağırdığında `[ObjCBlockTester callClassCallback]`, Xamarin.iOS / Xamarin.Mac çalışma zamanı sırasında görünür `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` parametre özniteliği. Ardından görüneceğini `Create` bu türünün yöntemi ve temsilci oluşturmak için bu yöntemi çağırın.

Bu iyileştirme bulacaksınız `Create` yapı saat ve statik kayıt yöntemi, (Bu çok daha hızlıdır ve ayrıca bağlayıcı tanır yansıma ve öznitelik kullanmayı meta veri belirteçleri kullanarak çalışma zamanında yöntemi arar kodu üretir Uygulama küçülterek karşılık gelen çalışma zamanı kodu kaldırmak için).

Mmp/mtouch bulamıyor ise `Create` yöntemi, bir MT4174/MM4174 uyarı gösterilir ve arama çalışma zamanında yerine gerçekleştirilir.
En olası neden bağlama kodu gerekli olmadan el ile yazılmış `[BlockProxy]` öznitelikleri.

Bu iyileştirme etkinleştirilecek statik kayıt gerektirir.

(Statik kayıt etkin olduğu sürece) her zaman varsayılan olarak etkindir.

Geçirerek varsayılan davranışı geçersiz kılınabilir `--optimize=[+|-]static-delegate-to-block-lookup` mtouch/mmp için.
