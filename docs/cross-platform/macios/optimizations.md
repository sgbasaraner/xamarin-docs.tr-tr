---
title: Derleme iyileştirmeleri
description: Bu belgede, Xamarin.iOS ve Xamarin.Mac uygulamaları için derleme zamanında uygulanan çeşitli iyileştirmeler açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: 432511a35f9f285d2c0060b5521256f834bb35ea
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351645"
---
# <a name="build-optimizations"></a>Derleme iyileştirmeleri

Bu belgede, Xamarin.iOS ve Xamarin.Mac uygulamaları için derleme zamanında uygulanan çeşitli iyileştirmeler açıklanmaktadır.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>UIApplication.EnsureUIThread kaldırın / NSApplication.EnsureUIThread

Çağrıları kaldırır [UIApplication.EnsureUIThread] [ 1] (için Xamarin.iOS) veya `NSApplication.EnsureUIThread` (için Xamarin.Mac).

Bu iyileştirme, aşağıdaki kodun türünü değiştirir:

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

Varsayılan davranış, geçirerek kılınabilir `--optimize=[+|-]remove-uithread-checks` mtouch/mmp için.

[1]: https://developer.xamarin.com/api/member/UIKit.UIApplication.EnsureUIThread/

## <a name="inline-intptrsize"></a>Satır içi IntPtr.Size

Satır içleri sabit değer, `IntPtr.Size` göre hedef platformu.

Bu iyileştirme, aşağıdaki kodun türünü değiştirir:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

(64-bit platformu oluştururken) aşağıdaki içine:

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

Varsayılan olarak tek bir mimarisini hedeflemek, etkinleştirildiğinden veya platform derleme için (**Xamarin.iOS.dll**, **Xamarin.TVOS.dll**, **Xamarin.WatchOS.dll** veya  **Xamarin.Mac.dll**).

Birden çok mimarileri hedefleyen, bu en iyi duruma getirme 32 bit sürümü ve 64 bit sürümü uygulamanın farklı derlemelerde oluşturur ve iki sürümü de etkili bir şekilde azalan yerine son uygulama artırıldığında, bu uygulamayı dahil edilmesi gerekir .

Varsayılan davranış, geçirerek kılınabilir `--optimize=[+|-]inline-intptr-size` mtouch/mmp için.

## <a name="inline-nsobjectisdirectbinding"></a>Satır içi NSObject.IsDirectBinding

`NSObject.IsDirectBinding` belirli bir örneğine bir kapsayıcı türü olup olmadığını belirleyen bir örnek özelliği (bir sarmalayıcı türü bir yerel türüyle eşleyen bir yönetilen türü; örnek yönetilen `UIKit.UIView` yazın yerel eşlenir `UIView` türü - tersine, bir kullanıcı türü Bu durumda `class MyUIView : UIKit.UIView` bir kullanıcı türü olacaktır).

Değerini bilmeniz gereken `IsDirectBinding` değeri hangi sürümünün belirlediğinden Objective-C içinde çağrılırken `objc_msgSend` kullanılacak.

Yalnızca aşağıdaki kodu verilen:

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

Uygulamasında belirleyebiliriz `UIView.SomeProperty` değerini `IsDirectBinding` bir sabit değil ve satır içine alınmış olamaz:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Ancak, tüm türlerin uygulamasında bakmak ve devralma türü yok belirlemek mümkün `NSUrl`, ve bu nedenle satır içi daha güvenlidir `IsDirectBinding` bir sabit değere `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

Özellikle, bu en iyi duruma getirme aşağıdaki kodun türünü değiştirir (Bu, bağlama kodunu `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Aşağıdaki içine (ne zaman bunu belirlenebilir hiçbir alt sınıflarından birini olduğunu `NSUrl` uygulamasında):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

Bu her zaman Xamarin.iOS için varsayılan olarak etkindir ve her zaman Xamarin.Mac için varsayılan olarak devre dışı (dinamik olarak Xamarin.Mac derlemeleri yüklemek mümkün olduğundan, belirli bir sınıfın hiç sınıflandırma belirlemek mümkün değildir).

Varsayılan davranış, geçirerek kılınabilir `--optimize=[+|-]inline-isdirectbinding` mtouch/mmp için.

## <a name="inline-runtimearch"></a>Satır içi Runtime.Arch

Bu iyileştirme, aşağıdaki kodun türünü değiştirir:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

(cihaz için oluştururken) aşağıdaki içine:

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

Xamarin.iOS (Xamarin.Mac için kullanılamaz) için varsayılan olarak her zaman etkindir.

Varsayılan davranış, geçirerek kılınabilir `--optimize=[+|-]inline-runtime-arch` mtouch için.

## <a name="dead-code-elimination"></a>Ölü kod ortadan kaldırılması

Bu iyileştirme, aşağıdaki kodun türünü değiştirir:

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

Ayrıca, bu gibi sabit karşılaştırmalar da değerlendirir:

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

ve böylelikle ifade `8 == 8` olduğu bir her zaman true ve azaltmak için:

```csharp
Console.WriteLine ("Doing this");
```

Aşağıdaki kod türü dönüştürebilirsiniz inlining'i iyileştirmeleri ile birlikte kullanıldığında güçlü bir iyileştirme olmasıdır (Bu, bağlama kodunu `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

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

Bu oturum (64-bit cihaz için oluşturma sırasında ve ayrıca mümkün olduğunda olmadığından emin olmak hiçbir `NFCIso15693ReadMultipleBlocksConfiguration` uygulama sınıflarında):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

AOT derleyici zaten böyle ölü kod ortadan kaldırmak kullanabilirsiniz, ancak bu en iyi duruma getirme, artık kullanılmaz ve bu nedenle (başka bir yerde kullanılan sürece) kaldırılabilir birden çok yöntem olduğunu görmeye bağlayıcı anlamına bağlayıcı içinde gerçekleştirilir. :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

(Bağlayıcı etkin olduğunda) her zaman varsayılan olarak etkindir.

Varsayılan davranış, geçirerek kılınabilir `--optimize=[+|-]dead-code-elimination` mtouch/mmp için.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>BlockLiteral.SetupBlock çağrıları en iyi duruma getirme

Xamarin.iOS/Mac çalışma zamanı, temsilci bir Objective-C bloğu için bir yönetilen oluştururken blok imza bilmek ister. Bu çok pahalı bir işlem olabilir. Bu iyileştirme blok imza oluşturma zamanında hesaplamak ve çağırmak için IL değiştirme bir `SetupBlock` imza, bunun yerine bağımsız değişken olarak alan yöntemi. Bunun yapılması, çalışma zamanında imzayı hesaplama gereksinimini ortadan kaldırır.

Bu bir blok 10 ila 15 faktörüyle çağırma hızlandırır Kıyaslama gösterir.

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

Statik kayıt şirketi (statik kayıt şirketi, cihaz derlemeleri için varsayılan olarak etkindir ve statik kayıt şirketi, yayın için varsayılan olarak etkindir Xamarin.Mac kapsadığı Xamarin.ios'ta) kullanırken, varsayılan olarak etkindir.

Varsayılan davranış, geçirerek kılınabilir `--optimize=[+|-]blockliteral-setupblock` mtouch/mmp için.

## <a name="optimize-support-for-protocols"></a>Protokoller için desteği en iyi duruma getirme

Xamarin.iOS/Mac çalışma zamanı, yönetilen türleri uygulayan Objective-C protokolleri hakkında bilgi gerekiyor. Bu bilgiler arabirimleri (ve bu arabirimler özniteliklerinde), depolanan çok verimli bir biçimde değil veya bağlayıcı kullanıma uygun değil.

Bir örnek olduğundan bu arabirimlerin tüm Protokolü üyeleri hakkında bilgi depolamak bir `[ProtocolMember]` , parametre türleri bu üyelerin başvuruları içeren diğer özelliklerin yanı sıra özniteliği. Yani bu arabiriminde kullanılan tüm türler korumak bağlayıcı böyle bir arabirim yalnızca uygulama oluşturacak, isteğe bağlı üyeleri için bile uygulama hiçbir zaman çağırır veya uygular.

Bu iyileştirme, gerekli tüm bilgileri kolay ve hızlı çalışma zamanında bulmak çok az bellek kullanan verimli bir biçimde depolamak statik kayıt şirketi hale getirir.

Bu mutlaka bu arabirimler ve ilgili öznitelikleri korumak gerekmeyen, bağlayıcı kurabileceğinizi öğreneceksiniz.

Bu iyileştirme, bağlayıcı ve statik kayıt şirketi etkinleştirilmesini gerektirir.

Xamarin.iOS üzerinde bağlayıcı hem statik kaydedici etkin olduğunda bu en iyi duruma getirme varsayılan olarak etkindir.

Xamarin.Mac üzerinde bu en iyi duruma getirme derlemeler dinamik olarak yükleme Xamarin.Mac destekler ve bu derlemeler değil alınan derleme zamanında bilinen (ve bu nedenle en nedeniyle) varsayılan olarak, hiçbir zaman etkinleştirilir.

Varsayılan davranış, geçirerek kılınabilir `--optimize=-register-protocols` mtouch/mmp için.

## <a name="remove-the-dynamic-registrar"></a>Dinamik kayıt şirketi Kaldır

Hem Xamarin.iOS ve Xamarin.Mac çalışma zamanı desteği dahil [yönetilen türlerini kaydetme](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) Objective-C çalışma zamanı ile. Derleme zamanında veya çalışma zamanında (ya da kısmen derleme zamanı ve çalışma zamanında rest) ya da yapılabilir, ancak tamamen oluşturma zamanında gerçekleştirilir, çalışma zamanında yapmak için destek kodunu kaldırılabilir demektir. Uygulama boyutu, önemli bir azalma özellikle uzantıları gibi daha küçük uygulamalar veya watchOS uygulamalarının sonuçlanır.

Bu iyileştirme, statik kayıt şirketi ve bağlayıcı etkinleştirilmesini gerektirir.

Bağlayıcı dinamik kayıt şirketi ve kaldırılacağı denemesi için IF kaldırmak güvenli olup olmadığını belirlemeye çalışır.

Xamarin.Mac yükleme derlemeleri (hangi derleme zamanında bilinen değil) çalışma zamanında dinamik olarak desteklediğinden, oluşturma zamanında bu güvenli bir iyileştirme olup olmadığını belirlemek mümkün değildir. Başka bir deyişle, bu en iyi duruma getirme Xamarin.Mac uygulamaları için varsayılan olarak hiçbir zaman etkindir.

Varsayılan davranış, geçirerek kılınabilir `--optimize=[+|-]remove-dynamic-registrar` mtouch/mmp için.

Dinamik kayıt şirketi kaldırmak için varsayılan kılınırsa, algılarsa bağlayıcı uyarıları yayar güvenli değildir (ancak yine de dinamik kayıt şirketi kaldırılacak).

## <a name="inline-runtimedynamicregistrationsupported"></a>Satır içi Runtime.DynamicRegistrationSupported

Satır içleri değeri, `Runtime.DynamicRegistrationSupported` derleme sırasında belirlenen.

Dinamik kayıt şirketi kaldırılırsa (bkz [dinamik kayıt şirketi kaldırmak](#remove-the-dynamic-registrar) iyileştirme), bu bir sabittir `false` değerini, aksi takdirde bir sabit `true` değeri.

Bu iyileştirme, aşağıdaki kodun türünü değiştirir:

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

dinamik kayıt şirketi çıkarıldığında aşağıdaki içine:

```csharp
throw new Exception ("dynamic registration is not supported");
```

dinamik kayıt şirketi değil kaldırıldığında aşağıdaki içine:

```csharp
Console.WriteLine ("do something");
```

Bu iyileştirme bağlayıcı etkin olmasını gerektirir ve yalnızca yöntemleriyle uygulanır `[BindingImpl (BindingImplOptions.Optimizable)]` özniteliği.

(Bağlayıcı etkin olduğunda) her zaman varsayılan olarak etkindir.

Varsayılan davranış, geçirerek kılınabilir `--optimize=[+|-]inline-dynamic-registration-supported` mtouch/mmp için.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Objective-C blokları için yönetilen temsilci oluşturmak için yöntemleri önceden hesaplar

Objective-C Seçici, çağırdığında bir blok bir parametre olarak alır ve ardından yönetilen kod Xamarin.iOS yöntemi geçersiz kılınmış olan / Xamarin.Mac çalışma zamanı bloğu için bir temsilci oluşturmak gerekir.

Bağlama Oluşturucu tarafından oluşturulan bağlama kodu içerecek bir `[BlockProxy]` türüyle belirten özniteliği bir `Create` Bunu yapmak için yöntemi.

Aşağıdaki Objective-C kodu verilen:

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

ve aşağıdaki bağlama kodunu:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

Oluşturucu oluşturur:

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

Objective-C çağırdığında `[ObjCBlockTester callClassCallback]`, Xamarin.iOS / Xamarin.Mac çalışma zamanı sırasında görünür `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` parametre özniteliği. Bunun ardından görünür `Create` ilgili türdeki yöntem ve temsilci oluşturmak için bu yöntemi çağırın.

Bu iyileştirme bulabilirsiniz `Create` yöntemi derleme zaman ve statik kayıt şirketi (Bu çok daha hızlıdır ve bağlayıcı de tanır yansıma ve öznitelik kullanmayı meta veri belirteçleri kullanarak çalışma zamanında yöntemi arayan kod üretir Uygulama küçülterek karşılık gelen çalışma zamanı kod kaldırmak için).

Mmp/mtouch bulamıyor ise `Create` yöntemi MT4174/MM4174 uyarısı gösterilir ve arama çalışma zamanında yerine gerçekleştirilir.
En olası neden gerekli olmadan bağlama kodunu el ile yazılan `[BlockProxy]` öznitelikleri.

Bu en iyi duruma getirme statik kaydedici etkin olmasını gerektirir.

(Statik kaydedici etkin olduğu sürece) her zaman varsayılan olarak etkindir.

Varsayılan davranış, geçirerek kılınabilir `--optimize=[+|-]static-delegate-to-block-lookup` mtouch/mmp için.
