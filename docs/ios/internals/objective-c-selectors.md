---
title: Xamarin.iOS, objective-C seçicileri
description: Bu belge, Objective-C seçicileri C# ile etkileşim kurmak anlatılmaktadır. Bu Seçici ve bunu yaparken dikkate alınması gereken teknik konular çağırmayı açıklar.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: d8708e518e967083bcfb95c88f8c20fddbb44b53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998800"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Xamarin.iOS, objective-C seçicileri

Objective-C dilinin temel aldığı *Seçici*. Seçici, bir nesneye gönderilen bir iletidir veya *sınıfı*. [Xamarin.iOS](~/ios/internals/api-design/index.md) haritalar örnek Seçici örnek yöntemleri için ve sınıfının statik yöntemleri Seçici.

Normal C işlevlerinin aksine (ve C++ üye işlevleri gibi), doğrudan kullanarak bir seçici çağrılamıyor [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
(*Kenara*: teorik olarak, P/Invoke sanal olmayan C++ üye işlevleri için kullanabilirsiniz, ancak daha iyi göz ardı sorunlu dünyasına olduğu derleyici başına ad değiştirmeyi hakkında endişe etmeniz gerekir.) Bunun yerine, seçici bir Objective-C sınıfına gönderilir ya da kullanarak [ `objc_msgSend` işlevi](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

İlginizi çekebilecek [Objective-C ileti üzerinde bu yardımcı kılavuz](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) yararlıdır.

<a name="Example" />

## <a name="example"></a>Örnek

Çağrılacak istediğinizi düşünelim [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) Seçici.
(Apple'nın belgelerindeki) bildirimi şu şekildedir:

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  Dönüş türü *CGSize* birleşik API'si için.
-  *Yazı tipi* parametresi bir [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (ve türetilen (dolaylı olarak) bir tür [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ) ve bu nedenle eşlenmiş [System.IntPtr](xref:System.IntPtr) .
-  *Genişliği* parametresi bir *CGFloat* , eşlenmiş *nfloat*.
-  *LineBreakMode* parametresi bir *UILineBreakMode* , zaten Xamarin.iOS, bağlı [UILineBreakMode numaralandırma](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Hepsini bir araya getirin ve eşleşen bir objc_msgSend bildirimi istiyoruz:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

Bunu bildirmeniz gerekir:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Size uygun parametreleri oluşturduktan sonra bildirilen sonra biz çağırabilirsiniz:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode);
```

Döndürülen değer olan boyutu 8 bayttan küçük olan bir yapı vardı (eski ister `SizeF` için birleşik API'lerini geçmeden önce kullanılan) Yukarıdaki kod cihazda çöken ancak simülatör üzerinde çalıştırdığınız. Bu nedenle değerinden 8 bit boyutu, dolayısıyla biz bir değer döndüren bir seçici çağırmak için *ayrıca* bildirmenize gerek `objc_msgSend_stret()` işlevi:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Çağırma hale gelir:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode);
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode);
```


<a name="Invoking_a_Selector" />

## <a name="invoking-a-selector"></a>Bir seçici çağırma

Bir seçici çağırma üç adım vardır:

1.  Seçici hedef alın.
1.  Seçici adını alın.
1.  Uygun bağımsız değişkenlerle objc_msgSend() çağırın.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>Seçici hedefleri

Bir seçici veya bir nesne örneği, hem de bir Objective-C sınıfı hedeftir. Hedef bir örneğidir ve ilişkili bir Xamarin.iOS türünden gelen, kullanmanız [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) özelliği.

Hedef sınıf ise kullanın [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) sınıf örneği için bir başvuru almak için ardından kullanmak [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) özelliği.


<a name="Selector_Names" />

### <a name="selector-names"></a>Seçici adları

Seçici adları içinde Apple'nın belgeleri listelenmiştir. Örneğin, [Uıkit NSString genişletme yöntemleri](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) dahil [sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) ve [sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). Katıştırılmış ve sondaki iki nokta üst üste önemlidir ve Seçici adın bir parçasıdır.

Seçici adına sahip olduğunuzda, oluşturabileceğiniz bir [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) de örneği.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Objc_msgSend() çağırma

 `objc_msgSend()` bir nesneye (Seçici) ileti göndermek için kullanılır. Bu işlev ailesini en az iki gerekli bağımsız değişkeni alır: Seçici hedef (örneği veya tanıtıcı sınıfı), Seçici ve ardından belirli Seçici için gerekli bağımsız değişkenler. Örnek ve Seçici bağımsız değişkenler olmalıdır `System.IntPtr`, ve kalan tüm bağımsız değişkenleri Seçici bekliyor, örneğin türüyle eşleşmelidir bir `nint` için bir `int`, veya bir `System.IntPtr` tüm `NSObject`-türetilmiş türler. Kullanım [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) özelliği almak için bir `IntPtr` için Objective-C tür örneği.

Ne yazık ki yok birden fazla `objc_msgSend()` işlevi.

Kullanım [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) bir yapı döndüren seçicileri için.
"İlginç" ARM üzerinde Bu anlatım gözetildiği için olan tüm dönüş türlerini içeren *değil* (char, short, int, long, kayan, çift) C yerleşik türlerinden herhangi birini ya da bir sabit listesi. X86 (simülatörü), bu boyutu 8 bayt daha büyük olan tüm yapıları için kullanılması gerekir. (CGSize----yukarıdaki örnekte kullanılan tam olarak 8 bayt olan ve bu nedenle kullanmaz `objc_msgSend_stret()` benzeticide.)

Kullanım [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) kayan döndürmesi seçiciler değeri x86 yalnızca noktası. Bu işlev üzerinde ARM kullanılması gerekmez; Bunun yerine, `objc_msgSend()`.

Ana [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) işlevi, diğer tüm seçiciler için kullanılır.

Hangi verdikten `objc_msgSend()` işlevlere çağırmanız gerekir (ve birden çok örneğin simülatör ve cihaz olabilir), normal kullanabileceğiniz [ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute) sonraki çağrı işlevi bildirmek için yöntemi.

Bir dizi önceden yapılmış `objc_msgSend()` bildirimleri bulunabilir [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## <a name="the-ugly"></a>Çirkin

Objective-C sahip üç tür, `objc_msgSend` yöntemleri: normal çağrılarını, kayan nokta değerleri (yalnızca x86) döndüren etkinleştirmeleri için diğeri için yapı değerleri döndüren etkinleştirmeleri için bir tane. İkinci soneki içerir `_stret` içinde `ObjCRuntime.Messaging`.

(Kurallar aşağıda açıklanmıştır) belirli yapılar döndüren bir yöntemi çağırdığınız, şunun gibi bir çıkış değeri, birinci parametre olarak dönüş değeri ile yöntemini çağırmanız gerekir:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

İşlerin hallolmasını çirkin burada ve kuralı ne zaman kullanmalısınız _stret_ farklı bağlamalarınızı simülatör hem de cihaz üzerinde çalışmak için isterseniz X86 ve ARM üzerinde şuna benzeyen bir kod eklemeniz gerekir:

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>Objc kullanarak\_msgSend\_stret yöntemi

Kural ne zaman kullanılacağı için [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) bu gibidir **ARM**:

-  Herhangi bir değer türü bir numaralandırmanın veya numaralandırma için temel türlerinden herhangi birini değil (int, long, double, byte, short, float).


Kural ne zaman kullanılacağı için [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) bu gibidir **X86**:

-  Herhangi bir değer türü bir numaralandırmanın veya numaralandırma için temel türlerinden herhangi birini değil (int, long, double, byte, short, float) ve yerel boyutu 8 bayttan büyük.


### <a name="creating-your-own-signatures"></a>Kendi imzaları oluşturuluyor.

Aşağıdaki [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) kendi imzaları oluşturmak için gereken kullanılabilir.



## <a name="related-links"></a>İlgili bağlantılar

- [Seçiciler örneği](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
