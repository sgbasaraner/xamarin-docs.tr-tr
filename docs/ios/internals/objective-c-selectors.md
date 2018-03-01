---
title: "Objective-C seçiciler"
ms.topic: article
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 3fa01d8f28dc1c86f9d4a8ee4d9fc0a9cdb8ee9c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="objective-c-selectors"></a>Objective-C seçiciler

Objective-C dil temel aldığı *seçiciler*. Bir seçici bir nesneye gönderilen iletisidir veya *sınıfı*. [Xamarin.iOS](~/ios/internals/api-design/index.md) eşlemeleri örneği seçiciler örneği yöntemlere ve sınıf seçici statik yöntemler için.

Normal C işlevlerini aksine (ve C++ üye işlevleri gibi), doğrudan bir seçici kullanarak çağrılamıyor [P/Invoke](http://www.mono-project.com/Dllimport).
(*Kenara*: teorik olarak P/Invoke için sanal olmayan C++ üye işlevlerini kullanabilirsiniz, ancak daha iyi göz ardı sorun teşkil edecek bir dünyanın olduğu ad derleyici başına bozma hakkında endişelenmeye gerek gerekir.) Bunun yerine, seçici bir Objective-C sınıfı gönderilen veya kullanarak örnek [ `objc_msgSend` işlevi](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

Fark edebilirsiniz [Objective-C ileti üzerinde bu yardımcı kılavuz](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) yararlıdır.

<a name="Example" />

## <a name="example"></a>Örnek

Çağrılacak istediğinizi düşünelim [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) Seçici.
Bildiriminden (Apple belgeleri) aşağıdaki gibidir:

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  Dönüş türü *CGSize* birleştirilmiş API'si.
-  *Yazı tipi* parametresi bir [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (ve bir türü (dolaylı olarak) türetilmiş [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ) ve bu nedenle eşlenmiş [System.IntPtr](https://developer.xamarin.com/api/type/System.IntPtr/) .
-  *Genişliği* parametresi, bir *CGFloat* , eşlenmiş *nfloat*.
-  *LineBreakMode* parametresi, bir *UILineBreakMode* , zaten Xamarin.iOS içinde bağlı [UILineBreakMode numaralandırma](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Hepsini bir araya getirin ve eşleşen bir objc_msgSend bildirimi istiyoruz:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

Bunu bildirmek için yapmanız gerekir:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Uygun parametreleri sahibiz sonra bildirilen sonra biz çağırabilirsiniz:

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

Sahip döndürülen değer edilmiş boyutu 8 bayt'tan küçük olan bir yapı (eski ister `SizeF` birleşik API'lerine değiştirmeden önce kullanılan) Yukarıdaki kod ancak aygıttaki çöken simulator üzerinde çalışmaya başlaması gereken. Bu nedenle boyutu, bu nedenle biz 8 bitten az bir değer döndüren bir seçici çağırmak için *de* bildirmeniz gerekir `objc_msgSend_stret()` işlevi:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Çağırma sonra olacak:

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

1.  Seçici hedef alır.
1.  Seçici adını alın.
1.  Uygun bağımsız değişkenlerle objc_msgSend() çağırın.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>Seçici hedefleri

Bir seçici bir nesne örneği veya Objective-C sınıfı hedeftir. Hedef bir örneğidir ve ilişkili bir Xamarin.iOS türünden gelen kullanın [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) özelliği.

Hedef sınıf ise kullanmak [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) sınıf örneği için bir başvuru almak için daha sonra kullanmak [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) özelliği.


<a name="Selector_Names" />

### <a name="selector-names"></a>Seçici adları

Seçici adları Apple belgeleri içinde listelenir. Örneğin, [Uıkit NSString genişletme yöntemleri](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) dahil [sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) ve [sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). Katıştırılmış ve izleyen iki nokta üst üste önemlidir ve Seçici adı bir parçasıdır.

Bir seçici ada sahip bir kez oluşturabileceğiniz bir [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) için bu örneği.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Objc_msgSend() çağırma

 `objc_msgSend()` bir nesneye (Seçici) ileti göndermek için kullanılır. Bu ailesi işlevlerini en az iki gerekli bağımsız değişkeni alır: Seçici hedef (örneği veya işlemek sınıfı), Seçici ve belirli Seçici için gerekli sonra herhangi bir bağımsız değişken. Örnek ve Seçici bağımsız değişkenleri olmalıdır `System.IntPtr`, ve tüm kalan bağımsız değişkenleri Seçici bekler, örneğin türüyle eşleşmelidir bir `nint` için bir `int`, veya bir `System.IntPtr` tüm `NSObject`-türetilmiş tür. Kullanım [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) edinme özelliği bir `IntPtr` Objective-C türü örneği için.

Ne yazık ki, var olan birden fazla `objc_msgSend()` işlevi.

Kullanım [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) bir yapı döndürmesi seçiciler için.
"İlginç", ARM üzerinde bu öğeleri tutmak için olan tüm dönüş türleri içerir *değil* numaralandırma veya (char, short, int, uzun, float, çift) C yerleşik türlerinden herhangi birini. X86 (benzetici), bu 8 bayt boyutunda büyük tüm yapıları için kullanılmalıdır. (Yukarıdaki örnekte kullanılan CGSize--8 bayt tam olduğundan ve bu nedenle kullanmayan `objc_msgSend_stret()` simulator içinde.)

Kullanım [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) için kayan döndürmesi seçici değeri yalnızca x86 üzerinde noktası. Bu işlev, ARM üzerinde kullanılacak gerek yoktur; Bunun yerine, kullanın `objc_msgSend()`.

Ana [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) işlevi, diğer tüm seçiciler için kullanılır.

Hangi karar verdim sonra `objc_msgSend()` işlev vardı çağırmanız gerekir (ve birden çok örneğin simulator ve aygıt olabilir), normal bir kullanabilirsiniz [ `[DllImport]` ](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.DllImportAttribute/) işlevi sonraki çağrı için bildirmek için yöntem.

Bir dizi önceden yapılmış `objc_msgSend()` bildirimleri bulunabilir [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## <a name="the-ugly"></a>Çirkin

Objective-C sahip üç tür, `objc_msgSend` yöntemleri: normal çağrılarını, bir kayan nokta değerlerine (yalnızca x86) dönüş etkinleştirmeleri için diğeri için dönüş yapısı değerleri etkinleştirmeleri için. İkinci soneki içerir `_stret` içinde `ObjCRuntime.Messaging`.

(Kuralları aşağıda açıklanmıştır) belirli yapılarını döndürülecek bir yöntemi çağırma, böyle bir çıkış değeri ilk parametre olarak dönüş değeri yöntemiyle çağırmanız gerekir:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

İşlerinizi çirkin buraya ve çünkü kullandığınızda gerekir için kural _stret_ farklı hem benzetici hem de cihaz üzerinde çalışmak için bağlama istiyorsanız X86 ve ARM üzerinde şuna benzer bir kod eklemeniz gerekir:

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

-  Herhangi bir değer türü numaralandırma veya numaralandırma için temel türlerinden herhangi birini değil (int, bayt, kısa, uzun, double, float).


Kural ne zaman kullanılacağı için [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) bu gibidir **X86**:

-  Herhangi bir değer türü numaralandırma veya numaralandırma için temel türlerinden herhangi birini değil (int, bayt, kısa, uzun, double, float) ve yerel büyüklüğü 8 bayttan büyük.


### <a name="creating-your-own-signatures"></a>Kendi imzaları oluşturma.

Aşağıdaki [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) kendi imzaları oluşturmak için gerekli olursa kullanılabilir.



## <a name="related-links"></a>İlgili bağlantılar

- [Seçici örneği](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
