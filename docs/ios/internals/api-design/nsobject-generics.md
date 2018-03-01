---
title: "NSObject Genel alt sınıfları"
ms.topic: article
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 76c35cfb993bade324b25f86e75db7eb028a7399
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="generic-subclasses-of-nsobject"></a>NSObject Genel alt sınıfları

## <a name="using-generics-with-nsobjects"></a>Genel türler NSObjects ile kullanma

Xamarin.iOS 7.2.1 ile başlayan kullanabilir genel türler içinde alt sınıflarının `NSObject` (örneğin [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)).

Şimdi bunun gibi genel sınıfları oluşturabilirsiniz:

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Bu alt'beri nesneleri `NSObject` kayıtlı Objective-C çalışma zamanı ile genel alt sınıflarının ile mümkündür ilişkin bazı sınırlamalar vardır `NSObject` türleri.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Genel alt sınıfların NSObject ilişkin konular

Bu belge Genel alt sınıflarının için sınırlı destek sınırlamaları ayrıntıları `NSObjects` 7.2.1 Xamarin.iOS ile sunulmuştur.
    
### <a name="generic-type-arguments-in-member-signatures"></a>Üye imzalarında genel tür bağımsız değişkenleri

Tüm genel tür bağımsız değişkenleri Objective-C için kullanıma sunulan bir üye imzada olmalıdır bir `NSObject` kısıtlaması.

**İyi**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Neden**: genel tür parametresi bir `NSObject`, bu nedenle Seçici imzası `myMethod:` Objective-C için güvenli bir şekilde kullanıma sunulabilecek (her zaman olacaktır `NSObject` veya bunun bir alt sınıfı).

**Hatalı**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Neden**: İmza genel tür tam türüne bağlı olarak farklı bu yana Objective-C kodunu çağırabilirsiniz dışarı aktarılan üyeler bir Objective-C imzası oluşturmak mümkün değil `T`.

**İyi**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**Neden**: verilen üye imza parçası almayan sürece genel tür bağımsız değişkenleri Kısıtlanmamış mümkündür.

**İyi**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**Neden**: `T` Objective-C parametresinde verilen `MyMethod` olacak şekilde kısıtlanmış bir `NSObject`, Kısıtlanmamış türü `U` imza parçası değil.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Genel türler Objective-c örneklemesi

Objective-C genel türlerinden eşlemesinin izin verilmiyor. Bu durum, normalde bir yönetilen türü bir xib kullanıldığında oluşur.

Alan bir oluşturucu sunan bu sınıf tanımını göz önünde bulundurun bir `IntPtr` (yerel Objective-C örneğinden C# nesnesi oluşturma Xamarin.iOS yolu):
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Yukarıdaki yapı çalışma zamanında ince olsa da, bir örneğini oluşturmak Objective-C çalışırsa, bu, bir özel durum oluşturur.

Bu Objective-C genel türler kavramına sahiptir ve oluşturmak için tam genel türü belirtilemez çünkü olur.

Bu sorunu çözmek genel tür öğesinin özelleştirilmiş bir alt kümesi oluşturarak çalışılabilecek.   Örneğin:
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

Belirsizlik olmaz, sınıf artık `GenericUIView` xibs içinde kullanılabilir.

## <a name="no-support-for-generic-methods"></a>Genel yöntemler için destek yok

### <a name="generic-methods-are-not-allowed"></a>Genel yöntemler izin verilmiyor.

Aşağıdaki kod derlenmez:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Neden**: Xamarin.iOS tür bağımsız değişkeni için kullanılacak türün bilmediğinden buna izin verilmiyor `T` zaman yöntemi çağrıldığında Objective-C.

Özel bir yöntem oluşturma ve bunun yerine, dışarı aktarmak için kullanılan bir alternatiftir:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>İzin verilen hiçbir statik üyeler

Genel öğesinin bir alt kümesi içinde barındırılıyorsa, Objective-C statik üyelerine getirebilir değil `NSObject`.

Desteklenmeyen bir senaryodur örneği:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**Neden:** genel yöntemler, genel tür bağımsız değişkeni için T. kullanmak için ne tür biliyor olması Xamarin.iOS çalışma zamanı gereksinimlerini'olduğu gibi

Örneği için örnek kullanılan üyeler (hiçbir zaman olacağından örneğini genel<T>, her zaman genel olacaktır<SomeSpecificClass>), ancak statik üyeleri için bu bilgileri yok.

Söz konusu üyeyi herhangi bir yolla tür bağımsız değişkeni T kullanmıyorsa bile bu geçerlidir.

Bu durumda alternatif özelleştirilmiş bir alt oluşturmaktır:

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIVIewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

### <a name="requires-new-static-registrar"></a>Yeni statik kayıt gerektirir

Genel türler destek yeni gerektiren [kayıt sistemi](~/ios/internals/registrar.md).

Eski kullanmaya çalışırsa, eski kayıt sistem genel türleri (tanımsız davranışını elde edilen doğru kodu oluşturmak değil ek olarak) ile karşılaştığında uyarılar gösterir.
    
## <a name="performance"></a>Performans

Genellikle yaptığı gibi statik kayıt genel bir tür dışarı aktarılan bir üye derleme zamanında çözümlenemiyor, çalışma zamanında Aranan gerekir. Bu, Objective-C böyle bir yöntemi çağırma'üyeleri genel olmayan sınıflardan çağırma değerinden biraz daha yavaş olduğu anlamına gelir.

