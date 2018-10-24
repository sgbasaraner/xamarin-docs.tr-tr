---
title: Xamarin.iOS, objective-C seçicileri
description: Bu belge, Objective-C seçicileri C# ile etkileşim kurmak anlatılmaktadır. Bu Seçici ve bunu yaparken dikkate alınması gereken teknik konular çağırmayı açıklar.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/12/2017
ms.openlocfilehash: b51ee6b547cc53761f23379e7233bb710090a61b
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39351736"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Xamarin.iOS, objective-C seçicileri

Objective-C dilinin temel aldığı *Seçici*. Seçici, bir nesneye gönderilen bir iletidir veya *sınıfı*. [Xamarin.iOS](~/ios/internals/api-design/index.md) haritalar örnek Seçici örnek yöntemleri için ve sınıfının statik yöntemleri Seçici.

Normal C işlevlerinin aksine (ve C++ üye işlevleri gibi), doğrudan kullanarak bir seçici çağrılamıyor [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/) bunun yerine, seçici bir Objective-C sınıfına gönderilir veya kullanma örneği [`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
işlev.

Objective-C iletiler hakkında daha fazla bilgi için Apple'nın göz atın [nesneleriyle çalışma](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2) Kılavuzu.

## <a name="example"></a>Örnek

Çağrılacak istediğinizi düşünelim [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
seçici üzerinde [ `NSString` ](https://developer.apple.com/documentation/foundation/nsstring).
(Apple'nın belgelerindeki) bildirimi şu şekildedir:

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

Bu API, aşağıdaki özelliklere sahiptir:

- Dönüş türü `CGSize` birleşik API'si için.
- `font` Parametresi bir [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (ve türetilen (dolaylı olarak) bir tür [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)) ve eşlendiği [System.IntPtr](xref:System.IntPtr).
- `width` Parametresi bir `CGFloat`, eşlenmiş `nfloat`.
- `lineBreakMode` Parametresi bir [ `UILineBreakMode` ](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc), Xamarin.iOS, önceden bağlandı [`UILineBreakMode`](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/)
Sabit listesi.

Hepsi bir araya getirildiğinde `objc_msgSend` bildirimi eşleşmelidir:

```csharp
CGSize objc_msgSend(
    IntPtr target, 
    IntPtr selector, 
    IntPtr font, 
    nfloat width, 
    UILineBreakMode mode
);
```

Şu şekilde bildirin:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Bu yöntemi çağırmak için kod aşağıdaki gibi kullanın:

```csharp
NSString target = ...
Selector selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont font = ...
nfloat width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, 
    selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode
);
```

Döndürülen değer olan boyutu 8 bayttan küçük olan bir yapı vardı (eski ister `SizeF` için birleşik API'lerini geçmeden önce kullanılan), yukarıdaki kod cihazda çöken ancak simülatör üzerinde çalıştırdığınız. Değerinden 8 bit boyutunda bir değer döndüren bir seçici çağırmak için `objc_msgSend_stret` işlevi:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Bu yöntemi çağırmak için kod aşağıdaki gibi kullanın:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, 
        selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode
    );
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode
    );
```

## <a name="invoking-a-selector"></a>Bir seçici çağırma

Bir seçici çağırma üç adım vardır:

1. Seçici hedef alın.
2. Seçici adını alın.
3. Çağrı `objc_msgSend` uygun bağımsız değişkenlerle.

### <a name="selector-targets"></a>Seçici hedefleri

Bir seçici veya bir nesne örneği, hem de bir Objective-C sınıfı hedeftir. Hedef bir örneğidir ve ilişkili bir Xamarin.iOS türünden gelen, kullanmanız [ `ObjCRuntime.INativeObject.Handle` ](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) özelliği.

Hedef sınıf ise kullanın [ `ObjCRuntime.Class` ](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) sınıf örneği için bir başvuru almak için ardından kullanmak [ `Class.Handle` ](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) özelliği.

### <a name="selector-names"></a>Seçici adları

Apple belgelerinde Seçici adları listelenmektedir. Örneğin, [ `NSString` ](https://developer.apple.com/documentation/foundation/nsstring?language=objc) içerir [ `sizeWithFont:` ](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc) ve [ `sizeWithFont:forWidth:lineBreakMode:` ](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc) seçicileri. Katıştırılmış ve sondaki iki nokta üst üste Seçici adın bir parçasıdır ve atlanamaz.

Seçici adına sahip olduğunuzda, oluşturabileceğiniz bir [ `ObjCRuntime.Selector` ](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) de örneği.

### <a name="calling-objcmsgsend"></a>Objc_msgSend çağırma

`objc_msgSend` bir nesneye (Seçici) bir ileti gönderir. Bu işlev ailesini en az iki gerekli bağımsız değişkeni alır: Seçici hedef (örneği veya tanıtıcı sınıfı), Seçici ve Seçici için gerekli herhangi bir bağımsız değişken. Örnek ve Seçici bağımsız değişkenler olmalıdır `System.IntPtr`, ve kalan tüm bağımsız değişkenleri Seçici bekliyor, örneğin türüyle eşleşmelidir bir `nint` için bir `int`, veya bir `System.IntPtr` tüm `NSObject`-türetilmiş türler. Kullanın [`NSObject.Handle`](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/)
özelliği almak için bir `IntPtr` için Objective-C tür örneği.

Birden fazla yoktur `objc_msgSend` işlevi:

- Kullanım [ `objc_msgSend_stret` ](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) yapı döndüren seçicileri için. ARM üzerinde bu numaralandırma olmayan tüm dönüş türleri veya C yerleşik türlerinden herhangi birini içerir (`char`, `short`, `int`, `long`, `float`, `double`). X86 (simülatörü), bu yöntem boyutu 8 bayt daha büyük olan tüm yapıları için kullanılmalıdır (`CGSize` 8 bayt ve kullanmayan `objc_msgSend_stret` benzeticide). 
- Kullanım [ `objc_msgSend_fpret` ](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc) kayan döndüren seçiciler değeri x86 yalnızca noktası. Bu işlev üzerinde ARM kullanılması gerekmez; Bunun yerine, `objc_msgSend`. 
- Ana [objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) işlevi, diğer tüm seçiciler için kullanılır.

Hangi verdikten `objc_msgSend` çağırmak için size gereken işlevlere (simülatör ve cihaz her gerektirebilir farklı bir yöntem), normal kullanabileceğiniz [ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute) sonraki çağrı işlevi bildirmek için yöntemi.

Bir dizi önceden yapılmış `objc_msgSend` bildirimleri bulunabilir `ObjCRuntime.Messaging`.

## <a name="different-invocations-on-simulator-and-device"></a>Simülatör ve cihaz farklı çağrıları

Yukarıda açıklandığı gibi üç tür Objective-C sahip, `objc_msgSend` yöntemleri: normal çağrılarını, kayan nokta değerleri (yalnızca x86) döndüren etkinleştirmeleri için diğeri için yapı değerleri döndüren etkinleştirmeleri için bir tane. İkinci soneki içerir `_stret` içinde `ObjCRuntime.Messaging`.

Belirli yapılar (aşağıda açıklanan kurallar) döndüren bir yöntemi çağırdığınız, birinci parametre olarak dönüş değeri ile yöntemini çağırmanız gerekir bir `out` değeri:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Kural ne zaman kullanılacağı için `_stret_` yöntemi x86 ve ARM farklıdır.
Simülatör hem de cihaz üzerinde çalışmaya bağlamalarınızı istiyorsanız, aşağıdaki gibi bir kod ekleyin:

```csharp
if (Runtime.Arch == Arch.DEVICE)
{
    PointF ret;
    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);
    return ret;
} 
else
{
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
}
```

### <a name="using-the-objcmsgsendstret-method"></a>Objc_msgSend_stret yöntemi kullanarak

ARM için oluşturma sırasında kullanın [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
bir numaralandırmanın veya numaralandırma için temel türlerinden herhangi birini değil herhangi bir değer türü için (`int`, `byte`, `short`, `long`, `double`, `float`).

İçin x86 oluştururken kullanın [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
bir numaralandırmanın veya numaralandırma için temel türlerinden herhangi birini değil herhangi bir değer türü için (`int`, `byte`, `short`, `long`, `double`, `float`) ve yerel boyutu 8 bayttan büyük.

### <a name="creating-your-own-signatures"></a>Kendi imzaları oluşturma

Aşağıdaki [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) kendi imzaları oluşturmak için gereken kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Objective-C seçicileri](https://developer.xamarin.com/samples/mac-ios/Objective-C/) örnek
