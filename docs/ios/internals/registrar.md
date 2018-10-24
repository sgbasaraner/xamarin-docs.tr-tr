---
title: Xamarin.iOS için türü kayıt şirketi
description: Getiren Xamarin.iOS türü kayıt şirketi bu belgede açıklanır C# sınıfları Objective-C çalışma zamanı için kullanılabilir.
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 8/29/2018
ms.openlocfilehash: cdd57095b03c24472abec5646ee3a70350770d7c
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "34786180"
---
# <a name="type-registrar-for-xamarinios"></a>Xamarin.iOS için türü kayıt şirketi

Bu belgede, Xamarin.iOS tarafından kullanılan türü kayıt sistemi açıklanmaktadır.

## <a name="registration-of-managed-classes-and-methods"></a>Yönetilen sınıflar ve yöntemler kaydı

Başlatma sırasında Xamarin.iOS kaydeder:

- İle sınıfları bir [[kaydetme]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Objective-C sınıflar olarak özniteliği.
- İle sınıfları bir [[Category]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) Objective-C kategorisi olarak özniteliği.
- Arabirimleri ile bir [[Protocol]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) Objective-C protokoller olarak özniteliği.
- Üyelerle bir [[dışarı]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/), Objective-bunlara erişmek C için mümkün hale getirme.

Örneğin, yönetilen düşünün `Main` Xamarin.iOS uygulamalarında yaygın yöntemi:

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

Objective-C çalışma zamanı adı türü kullanmak için bu kodu söyler `AppDelegate` uygulamanın temsilci sınıfı. Objective-C çalışma zamanı örneğini oluşturabilmek için C# `AppDelegate` sınıfının, sınıf kayıtlı olması gerekir.

Xamarin.iOS kayıt zamanında (dinamik kayıt) veya (statik kayıt) derleme zamanında otomatik olarak gerçekleştirir.

Dinamik kayıt yansıma Başlangıçta tüm sınıflar ve yöntemler kaydetmek için bulmak için bunları Objective-C çalışma zamanı için geçirme kullanır. Simülatör yapıları için dinamik kayıt varsayılan olarak kullanılır.

Statik kayıt derleme zamanında uygulama tarafından kullanılan derlemeleri inceler. Objective-C ile kaydetmeye yönelik yöntemler ve sınıflar belirler ve ikili dosyanıza katıştırılır bir harita oluşturur.
Ardından, başlangıçta bu harita Objective-C çalışma zamanı ile kaydeder. Statik kayıt cihaz derlemeleri için kullanılır.

### <a name="categories"></a>Kategoriler

Xamarin.iOS 8.10 ile başlayarak, Objective-C Kategoriler kullanarak oluşturmak mümkündür C# söz dizimi.

Bir kategori oluşturmak için `[Category]` özniteliği ve genişletmek için türü belirtin. Örneğin, aşağıdaki kod genişletir `NSString`:

```csharp
[Category (typeof (NSString))]
```

Her bir kategorinin yöntemlerin bir `[Export]` özniteliği, Objective-C çalışma zamanı kullanılabilir hale getirir:

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

Tüm yönetilen uzantı yöntemleri statik olmalıdır, ancak standart kullanarak Objective-C örnek yöntemler oluşturmak mümkündür C# genişletme yöntemleri için söz dizimi:

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

Genişletme yöntemi için ilk bağımsız değişken yöntemi çağrıldı örneğidir:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
 }
 ```

Bu örnek yerel ekleyeceksiniz `toUpper` örnek yöntemi için `NSString` sınıfı. Bu yöntem, Objective-C: çağrılabilir

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAutoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

### <a name="protocols"></a>Protokolleri

İle Xamarin.iOS 8.10 ile başlayarak, arabirimleri `[Protocol]` öznitelik verilen protokoller olarak Objective-C:

```csharp
[Protocol ("MyProtocol")]
interface IMyProtocol
{
    [Export ("method")]
    void Method ();
}

class MyClass : IMyProtocol
{
    void Method ()
    {
    }
}
```

Bu kod aktarır `IMyProtocol` Objective-C olarak bir protokol için çağrılan `MyProtocol` ve bir sınıfa `MyClass` protokolü uygulayan.

## <a name="new-registration-system"></a>Yeni bir kayıt sistemi

Kararlı 6.2.6 ile başlangıç sürümü ve 6.3.4 beta sürümü, yeni bir statik Kaydedici ekledik. 7.2.1 içinde sürüm yaptık yeni kayıt şirketi varsayılan.

Bu yeni bir kayıt sistemi aşağıdaki yeni özellikleri sunar:

- Derleme zamanı algılama Programcı hataları:
    - Aynı ada sahip Kaydedilmekte iki sınıf.
    - Aynı Seçici için yanıt verilmesini birden fazla yöntemi
- Kullanılmayan yerel kod temizleme:
    - Yeni bir kayıt sistemi güçlü statik kitaplıklarda kullanılan kod yerel bağlayıcıya kullanılmayan yerel koddan elde edilen ikili çıkarmanız izin vererek başvurular ekler. Xamarin'in örnek bağlamalarını üzerinde uygulamaların çoğu en az 300 k daha küçük olur.

- Genel alt sınıfları için destek `NSObject`; bkz [NSObject genel türler](~/ios/internals/api-design/nsobject-generics.md) daha fazla bilgi için. Ayrıca yeni bir kayıt sistemi, daha önce çalışma zamanı rastgele davranışına neden desteklenmeyen genel yapıları yakalar.

### <a name="errors-caught-by-the-new-registrar"></a>Yeni kayıt şirketi tarafından yakalanan hataları

Yeni kayıt şirketi tarafından yakalanan hataları bazı örnekler aşağıdadır.

- Aynı Seçici aynı sınıf içinde birden çok kez dışarı aktarma:

    ```csharp
    [Register]
    class MyDemo : NSObject 
    {
        [Export ("foo:")]
        void Foo (NSString str);
        [Export ("foo:")]
        void Foo (string str)
    }
    ```

- Objective-C aynı ada sahip birden fazla yönetilen sınıf dışarı aktarma:

    ```csharp
    [Register ("Class")]
    class MyClass : NSObject {}

    [Register ("Class")]
    class YourClass : NSObject {}
    ```

- Genel yöntemler dışarı aktarma:

    ```csharp
    [Register]
    class MyDemo : NSObject
    {
        [Export ("foo")]
        void Foo<T> () {}
    }
    ```

### <a name="limitations-of-the-new-registrar"></a>Yeni kayıt şirketi sınırlamaları

Yeni kayıt şirketi hakkında göz önünde bulundurmanız gereken bazı hususlar:

- Bazı üçüncü taraf kitaplıklar, yeni bir kayıt sistemi ile çalışacak şekilde güncelleştirilmesi gerekir. Bkz: [gerekli değişiklikleri](#required_modifications) aşağıda daha fazla ayrıntı için.

- Kısa vadeli bir dezavantajı, ayrıca hesapları framework kullanılıyorsa, Clang kullanılmalıdır, (Bunun nedeni, Apple'nın **accounts.h** üstbilgisi yalnızca Clang tarafından derlenmesi). Ekleme `--compiler:clang` Xcode 4.6 veya önceki bir sürümü kullanıyorsanız, Clang kullanılacak diğer mtouch bağımsız değişkenleri için (Xamarin.iOS otomatik olarak seçer Clang Xcode 5.0 veya sonraki sürümlerde.)

- Xcode 4.6 (veya öncesi) ise kullanıldığında, GCC / G ++ türü dışarı aktardıysanız seçilmelidir adları (Bu, Xcode 4.6 ile birlikte gelen Clang sürümünü Objective-C kodu ASCII olmayan karakterler tanımlayıcılar içinde desteklemediğinden) ASCII olmayan karakterler içeremez. Ekleme `--compiler:gcc` GCC kullanılacak diğer mtouch bağımsız değişkenleri için.

## <a name="selecting-a-registrar"></a>Bir kaydedici seçme

Aşağıdaki seçeneklerden birini diğer mtouch bağımsız değişkenleri projenin ekleyerek farklı bir kayıt şirketi seçebilirsiniz **iOS derleme** ayarları:

- `--registrar:static` – cihaz derlemeleri için varsayılan
- `--registrar:dynamic` – simülatör yapıları için varsayılan

> [!NOTE]
> Xamarin'in Klasik API desteklenen diğer seçenekleri gibi `--registrar:legacystatic` ve `--registrar:legacydynamic`. Ancak, bu seçenekler birleşik API tarafından desteklenmez.

## <a name="shortcomings-in-the-old-registration-system"></a>Eski kayıt sistemdeki eksiklikleri

Eski kayıt sistemi aşağıdaki dezavantajları vardır:

- Objective-C sınıflar ve yöntemler (her şeyi kaldırılacağını nedeniyle), gerçekten kullanılan değildi üçüncü taraf yerel kod kaldırmak için yerel bağlayıcı isteriz alınamadı, amacı, üçüncü taraf yerel kitaplıklarında (yerel) statik başvuru vardı. Bunun sebebi `-force_load libNative.a` , her üçüncü taraf bağlama yapmak zorunda (veya eşdeğer `ForceLoad=true` içinde `[LinkWith]` özniteliği).
- Objective-C aynı ada sahip iki yönetilen türler hiçbir uyarı ile dışarı aktarılamadı. Nadir bir senaryo iki düştüğünden oluşturmaktı `AppDelegate` farklı ad alanlarında sınıflar. Çalışma zamanında bu hangisinin (aslında bu çok kafa ve can sıkıcı bir hata ayıklama deneyimi için sunulan bile yeniden değildi - bir uygulama çalıştırma arasında değiştirilen) çekilmiş tamamen rastgele olacaktır.
- Objective-C aynı imzaya sahip iki yöntem dışarı aktarılamadı. Henüz yeniden rastgele hangisinin Objective-C çağrılır (ancak çoğunlukla tek yolu aslında bu hatayı deneyimi unlucky Yönetilen yöntemi geçersiz kılmak için olduğundan bu sorunu önceki kümeyle, yaygın olarak değildi).
- Dışarı aktarılan yöntemleri kümesini dinamik ve statik derlemeler arasında biraz farklılık gösterir.
- Yapılandırma düzgün genel sınıfları dışarı aktarılırken çalışmaz (çalışma zamanında hangi tam bir genel uygulama yürütülen etkili bir şekilde belirlenmemiş davranışlara neden olur, rastgele olacaktır).

## <a name="new-registrar-required-changes-to-bindings"></a>Yeni kayıt şirketi: gerekli bağlamaları için değişiklikler

Bu bölümde, yeni kayıt şirketi ile çalışmak için yapılması gereken bağlamaları değişiklikler açıklanmaktadır.

### <a name="protocols-must-have-the-protocol-attribute"></a>Protokolleri [Protocol] özniteliği olmalıdır

Protokolleri artık olmalıdır `[Protocol]` özniteliği. Bunu yaparsanız, bir yerel bir bağlayıcı hatası gibi olur:

```console
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>Seçiciler parametreleri için geçerli bir sayı olmalıdır

Tüm seçiciler doğru parametre sayısı belirtmelidir. Daha önce bu hataları yoksayıldı ve çalışma zamanı sorunlarına neden olabilir.

Kısacası, iki nokta üst üste, parametrelerin sayısıyla eşleşmelidir:

- Hiçbir parametre: `foo`
- Bir parametre: `foo:`
- İki parametre: `foo:parameterName2:`

Yanlış kullanımları şunlardır:

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>İçinde dışarı aktarmayı IsVariadic parametresini kullanın

Değişen sayıda bağımsız değişken işlevleri kullanmalıdır `IsVariadic` bağımsız değişkeni `[Export]` özniteliği:

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>Mevcut sembollere bağlamanız gerekir

Yerel Kitaplığı'nda mevcut olmayan sınıflar bağlamak mümkün değildir.
Bir sınıf kaldırıldı veya yeniden adlandırılmış yerel bir Kitaplığı'nda, eşleştirilecek bağlamalarını güncelleştirin emin olun.
