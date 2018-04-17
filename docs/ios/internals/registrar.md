---
title: Kayıt türü
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 9d4d2e5f3e1c2dc1233b466c00dd60340552fed0
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="type-registrar"></a>Kayıt türü

Bu belgede Xamarin.iOS tarafından kullanılan tür kayıt sistemi açıklanmaktadır.

## <a name="registration-of-managed-classes-and-methods"></a>Yönetilen sınıflar ve yöntemler kaydı

Başlatma sırasında Xamarin.iOS kaydeder:

  - İle sınıfları bir [[Kaydet]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Objective-C sınıflar olarak özniteliği.
  - İle sınıfları bir [[Kategori]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) Objective-C kategoriler olarak özniteliği.
  - İle arabirimleri bir [[Protocol]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) Objective-C protokoller olarak özniteliği.

ve her durumda üyeleriyle bir [[verme]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) özniteliği Objective-c verilir Bu olmasını yönetilen sınıflar sağlar oluşturulan ve yönetilen Objective-C çağrılacak yöntemleri ve yöntemleri ve özellikleri Objective-C bir C# world arasında bağlı olduğunu yoludur.

Çok basit bir örnek her uygulama olan AppDelegate sınıftır. Yönetilen ana yöntemi bunun gibi bir satır olduğundan geri çağırma:

    UIApplication.Main (args, null, "AppDelegate");

Bu uygulamanın temsilci sınıf olarak "AppDelegate" olarak adlandırılan türü oluşturmak için Objective-C çalışma zamanı bildirir.  C# ile yazılmış "AppDelegate" sınıfının bir örneğini oluşturmak nasıl bilmeniz Objective-C çalışma zamanı için bu sınıfı kaydedilecek sahiptir.

Xamarin.iOS çalışma zamanı kaydını sizin için dikkatli ve dahili olarak, bu kayıt çalışma zamanında tamamen (dinamik kayıt) yapılabilir veya derleme zamanında (statik kayıt) yapılabilir.  Tüm sınıflar ve yöntemler kaydetmek ve onlara Objective-C çalışma zamanı geçirmek için bulmak için başlangıçta yansıma kullanarak dinamik yaklaşım içerir.  Statik yaklaşım derleme zamanında uygulama tarafından kullanılan derlemeler olup olmadığını denetler.  Sınıflar ve yöntemler Objective-C ile kaydetmek için belirler ve ikili dosyanıza katıştırılmış bir harita oluşturur.  Ardından, başlangıçta, biz harita Objective-C çalışma zamanı ile kaydedin.

### <a name="categories"></a>Kategoriler

Xamarin.iOS 8.10 ile başlatmanızı C# sözdizimi kullanarak Objective-C kategorileri oluşturmak mümkün olacaktır.

Bu yapılır öznitelik bağımsız değişken olarak genişletmek için türünü belirtme [Kategori] özniteliğini kullanarak.
Aşağıdaki örnek NSString örneği için genişletebilirsiniz:

    [Category (typeof (NSString))]

Her kategori yöntemi Objective-[verme] özniteliğini kullanarak C için yöntemleri dışa aktarmak için normal mekanizması kullanıyor:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Tüm yönetilen genişletme yöntemleri statik olmalıdır, ancak C# genişletme yöntemleri için standart söz dizimini kullanarak Objective-C örnek yöntemleri oluşturmak mümkündür:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

ve ilk bağımsız değişken genişletme yöntemi üzerinde yöntemi çağrıldı örneği olacaktır.

Tam örnek:

    [Category (typeof (NSString))]
    public static class MyStringCategory
    {
        [Export ("toUpper")]
        static string ToUpper (this NSString self)
        {
            return self.ToString ().ToUpper ();
        }
    }

Bu örnek bir yerel toUpper örnek yöntemi hedefi C'den çağrılabilir NSString sınıfı ekler

    [Category (typeof (UIViewController))]
    public static class MyViewControllerCategory
    {
        [Export ("shouldAutoRotate")]
        static bool GlobalRotate ()
        {
            return true;
        }
    }

### <a name="protocols"></a>protokolleri

Xamarin.iOS 8.10 ile başlayan arabirimleri [Protocol] özniteliğiyle Objective-C protokoller olarak dışarı aktarılır.

Örnek:

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

Bu Objective-C için bir protokol (İletişimKuralım) ve protokolü uygulayan bir sınıf (MyClass) dışarı aktarılır.

 **Dinamik kayıt**

derleme/hata ayıklama döngüsü hızlandırır olarak simulator derlemeler için kullanılır.  Bu sınıf eşleme ve bu harita tablo derleme uygulama Başlatıcısı içine uygulamanızı başlatın ve bunun yerine, genel amaçlı Başlatıcısı her zaman kullanılır her zaman oluşturan adımları ortadan kaldırmaya sonucudur.  Masaüstü bilgisayarınızda çalışma zamanı gerçekleştirmek için güç Eskinin olan performans hiçbir zaman söz konusu şekilde sınıfları hızlı tarama.

 **Statik kayıt**

cihaz derlemeler için mobil cihazları masaüstü bilgisayarlar yavaştır ve çalışma zamanı gerçekleştirme tarama yavaş yöneliktir.  Cihaz derlemeler bu yana her zaman yeni bir ikili oluşturmanız gerekir, yapı/hata ayıklama döngüsü kayıt eşleme oluşturma işlemi etkilenmez.

## <a name="new-registration-system"></a>Yeni kayıt sistemi

Kararlı 6.2.6 ile başlayan sürümü ve eklediğimiz yeni bir statik kayıt 6.3.4 beta sürümü. 7.2.1 içinde sürüm biz yapılan yeni kayıt varsayılan.

Bu yeni kayıt sistemi aşağıdaki yeni özellikleri sunar:

- Programcı hataların zaman algılama derleyin:
    - Aynı ada sahip Kaydedilmekte iki sınıf.
    - Dışa aktarılan aynı Seçici yanıt için birden fazla yöntemi



- Kullanılmayan yerel kod kaldırabilirsiniz
    - Yeni kayıt sistemi statik kitaplıklarda kullanılan kod elde edilen ikili kullanılmayan yerel kod çıkışı çıkarabilmesi yerel bağlayıcı izin için güçlü başvurular ekler.
      Xamarin'ın örnek bağlamaları üzerinde çoğu uygulamalar en az 300 k daha küçük hale gelir.

- NSObject Genel alt sınıflarının desteği. Bkz: [NSObject genel türler](~/ios/internals/api-design/nsobject-generics.md) daha fazla bilgi için. Ayrıca yeni kayıt sistemi daha önce çalışma zamanında rastgele davranışa neden desteklenmeyen genel yapıları yakalar.

Bu yeni registar tarafından yakalanan hataları bazı örnekleri şunlardır:

Aynı Seçici aynı sınıfta birden çok kez dışarı aktarma.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo:")]
    void Foo (NSString str);
    [Export ("foo:")]
    void Foo (string str)
}
```

Objective-C adında birden fazla yönetilen sınıf veriliyor.

```csharp
[Register ("Class")]
class MyClass : NSObject {}

[Register ("Class")]
class YourClass : NSObject {}
```

Genel yöntemler veriliyor.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo")]
    void Foo<T> () {}
}
```



Yeni kayıt ilgili göz önünde bulundurmanız gereken bazı şeyler:
- Kitaplıkları yeni kayıt sistemiyle çalışacak şekilde güncelleştirilmesi gereken bazı üçüncü taraf bölümüne bakın [gerekli değişiklikleri aşağıdaki](#required_modifications) daha fazla ayrıntı için.
- Kısa vadeli dezavantajı, aynı zamanda hesapları framework kullanılıyorsa Clang kullanılmalıdır olan (Apple'nın accounts.h üstbilgisi tarafından Clang yalnızca derlenebilir olmasıdır). Ekleme <code>--compiler:clang</code> Clang durumlarda kullanmak üzere ek mtouch bağımsız değişkenler için Xcode 4.6 veya önceki bir sürümünü kullanıyorsanız (Xamarin.iOS otomatik olarak seçebilir Clang Xcode 5.0 veya daha yenisi.)

    <li>Xcode 4.6 (veya öncesi) ise kullanılan, GCC / G ++ türü veriliyorsa seçilmelidir adları (Bu, Xcode 4.6 ile gönderilen Clang sürümü Objective-C kodunu ASCII olmayan karakterler tanımlayıcıları içinde desteklemediğinden) ASCII olmayan karakterler içeriyor. Ekleme <code>--compiler:gcc</code> GCC kullanmak için ek mtouch bağımsız.


## <a name="selecting-a-registrar"></a>Bir kayıt seçme

Aşağıdaki seçeneklerden birini projenin iOS derleme seçenekleri ek mtouch değişkenlerinde ekleyerek farklı Kaydedici seçebilirsiniz:

-  `--registrar:static` : cihaz derlemeler için varsayılan
-  `--registrar:dynamic` : simulator derlemeler için varsayılan
-  `--registrar:legacystatic` : 7.2.1 Xamarin.iOS kadar aygıt derlemeler için varsayılan
-  `--registrar:legacydynamic` : 7.2.1 Xamarin.iOS kadar simulator derlemeler için varsayılan


## <a name="shortcomings-in-the-old-registration-system"></a>Eski kayıt sistemindeki eksik

Eski kayıt sistem aşağıdaki dezavantajları vardır:

-  Objective-C sınıflar ve yöntemler biz (her şeyi kaldırılırlar nedeniyle), aslında kullanılan değildi üçüncü taraf yerel kod kaldırmak için yerel bağlayıcı isteyin uygulanamadı amacı üçüncü taraf yerel kitaplıklarında (yerel) hiçbir statik başvuru vardı. Bunun sebebi "-force_load libNative.a" her üçüncü taraf bağlama yapmak zorunda (veya eşdeğer ForceLoad = true [LinkWith] özniteliğinde).
-  İki yönetilen türleriyle aynı adla Objective-C, hiçbir uyarı dışarı aktarılamadı. Nadir bir senaryo iki AppDelegate sınıfları (farklı ad alanları) şunun için oluştu. Çalışma zamanında, hangisinin (aslında çok puzzling ve can sıkıcı hata ayıklama deneyimini hale bile yeniden değildi - bir uygulama çalıştırılan arasında farklılık) çekilmiş tamamen rastgele olacaktır.
-  Aynı Objective-C imzaya sahip iki yöntem dışarı aktarılamadı. Henüz yeniden rastgele hangisinin Objective-C adlı (ancak genellikle bu hata gerçekte yaşamaya tek yolu unlucky Yönetilen yöntemi geçersiz kılmak için olduğundan bu sorun öncekinin olarak ortak değildi).
-  Dışarı aktarılan yöntemleri kümesini dinamik ve statik yapı arasında biraz farklı.
-  Öğe düzgün genel sınıfları dışarı aktarılırken çalışmaz (tam hangi genel uygulama çalışma zamanında yürütülen etkili bir şekilde belirlenmemiş davranışını elde edilen rastgele olacaktır).


 <a name="required_modifications" />


## <a name="new-registrar-required-changes-to-bindings"></a>Yeni kayıt şirketi: Gerekli değişiklikleri bağlamaları


Varolan Objective-C bağlamaları yeni registar ile çalışmak üzere güncellenmelidir gerekebilir.

Burada, yapılmasına gerek yapılan değişikliklerin bir listesi verilmiştir.

### <a name="protocols-must-have-the-protocol-attribute"></a>Protokolleri [Protocol] özniteliğe sahip olması gerekir

Protokolleri uygulamaları artık uygulanmış [Protocol] özniteliği olmalıdır.  Bunu yaparsanız, bunun gibi bir yerel bağlayıcı hatası olur:

```csharp
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>Seçici parametrelerinin geçerli bir sayı olmalıdır

Tüm seçiciler doğru parametre sayısı belirtmelidir.  Daha önce bu hataları yoksayıldı ve çalışma zamanı sorunlarına neden olabilir.

Kısacası, iki nokta üst üste sayısı parametre sayısı eşleşmelidir.

Örneğin:

-  Hiçbir parametre: 'foo'
-  One parameter: `foo:'
-  Two parameters: `foo:parameterName2:'


Yanlış kullanımları şunlardır:

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>Dışa aktarma IsVariadic parametresini kullanın

Variadic işlevleri gerekir söyleyin bunu kendi verme özniteliği (Seçici bağımsız değişken sayısı hakkında yanlış olduğundan bu, yani 2 noktası. Üstten ihlal):

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>Varolan simgeleri bağlamanız gerekir

Yerel kitaplığında mevcut sınıfları bağlamak mümkün değildir.

Var olmayan sınıfları bağlamak çalışırsanız yerel bağlayıcı hata iletisi alır.

Bu genellikle bir bağlama süre için var ve belirli bir yerel sınıf ya da kaldırılmıştır veya bağlama güncelleştirilmez karşın, yeniden adlandırılmış, yerel kod bu işlem sırasında değiştirilmiş olur.

