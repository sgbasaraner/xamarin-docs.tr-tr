---
title: Objective-C bağlamaları genel bakış
description: Bu belge, C# bağlamaları için komut satırı bağlamalar ve bağlama projeleri hedefi Sharpie dahil olmak üzere, Objective-C kodunu oluşturmak için farklı yollarına genel bakış sağlar. Bağlamanın nasıl çalıştığı açıklanır.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: asb3993
ms.author: amburns
ms.openlocfilehash: 97d0c5b9f61d4dafe144d2b2f22df6d465cbbccb
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855279"
---
# <a name="overview-of-objective-c-bindings"></a>Objective-C bağlamaları genel bakış

_Bağlama işleminin nasıl çalıştığına ilişkin ayrıntıları_

Xamarin ile kullanmak için bir Objective-C kitaplığını bağlama üç adımdan alır:

1. Bir C# "API tanımı" yazma açıklamak için nasıl yerel API sunulmuştur .NET ve temel alınan Objective-c eşlemelerini nasıl Bu işlem gerçekleştirilir gibi standart C# kullanarak oluşturur `interface` ve çeşitli bağlama **öznitelikleri** (bkz. Bu [basit örnek](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Bir kez "API tanımı" C# ' ta, derlemeniz "bağlama" derlemesi üretmek için yazdığınız. Bu, üzerinde yapılabilir [ **komut satırı** ](#commandline) veya kullanarak bir [ **bağlama projesi** ](#bindingproject) Visual Studio veya Mac için Visual Studio'da.

3. Tanımladığınız API'sini kullanarak yerel bir işlev erişebilmesi için bu "bağlama" derleme sonra Xamarin uygulaması projenize eklenir.
  Bağlama projesi uygulama projelerinizden tamamen ayrıdır.

**Not:** 1. adım otomatik hale getirilebilir Yardımı ile [ **hedefi Sharpie**](#objectivesharpie). Objective-C API inceler ve önerilen bir C# "API tanımı" oluşturur Hedefi Sharpie tarafından oluşturulan dosyaları özelleştirmenizi ve bağlama projesi (veya komut satırında) kullanın, bağlama derlemesi oluşturmak için. Amaç Sharpie kendisi tarafından bağlamaları oluşturmaz, daha büyük işlem yalnızca isteğe bağlı bir parçasıdır.

Daha fazla teknik ayrıntılarını ayrıca okuyabilirsiniz [nasıl çalıştığını](#howitworks), hangi yardımcı olacak bağlamalarınızı yazılacak.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Komut satırı bağlamaları

Kullanabileceğiniz `btouch-native` Xamarin.iOS için (veya `bmac-native` Xamarin.Mac kullanıyorsanız) doğrudan bağlamalarını derlemesine. El ile oluşturduğunuz bir C# API tanımlarını geçirerek çalışır (veya hedefi Sharpie kullanarak) komut satırı aracı (`btouch-native` iOS için veya `bmac-native` Mac için).


Bu araçlar çağırmak için genel sözdizimi aşağıdaki gibidir:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Yukarıdaki komut dosyası oluşturur `cocos2d.dll` geçerli dizindeki ve projenizde kullanabileceğiniz tam olarak ilişkili kitaplık içerecektir. Bu bağlama projesi kullanıyorsanız bağlamalarınızı oluşturmak için Mac için Visual Studio kullanan aracıdır (açıklanan [aşağıda](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Bağlama projesi

Bağlama projesi (yalnızca Visual Studio iOS bağlamaları'destekler) Visual Studio veya Mac için Visual Studio'da oluşturulabilir ve düzenleyin ve derleme bağlama (karşı komut satırını kullanarak) için API tanımları kolaylaştırır.

İzleyin [Başlangıç Kılavuzu](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) oluşturma ve bağlama oluşturmak için bir bağlama projesi kullanma hakkında bilgi için.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Amaç Sharpie

Amaç Sharpie bir bağlama oluşturma ile ilk aşamalarında yardımcı olan başka ve ayrı bir komut satırı aracıdır. Bu bağlama kendisi tarafından oluşturmaz, bunun yerine, hedef yerel kitaplık için bir API tanımı oluşturmanın ilk adımı otomatikleştirir.

Okuma [hedefi Sharpie docs](~/cross-platform/macios/binding/objective-sharpie/index.md) bağlamaları oluşturulan API tanımlarını içine yerel kitaplıklar, yerel çerçeveler ve CocoaPods ayrıştırılacak öğrenin.

<a name="howitworks" />

## <a name="how-binding-works"></a>Bağlama nasıl çalışır?

Kullanmak mümkün mü [[kaydetme]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) özniteliği [[dışarı]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) özniteliği ve [el ile Objective-C Seçici çağırma](~/ios/internals/objective-c-selectors.md) birlikte el ile yeni (daha önce bağlamak için Objective-C tür ilişkisiz).

İlk olarak bağlamak istediğiniz bir türü bulunamadı. Tartışma amacıyla (ve Basitlik için), biz bağlamak [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) türü (hangi zaten bağımlı [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); aşağıdaki uygulama yalnızca bir örneğin amacıyla kullanılır).

İkinci olarak, C# türünü oluşturmak ihtiyacımız var. Biz bu ad alanına yerleştirmek büyük olasılıkla istersiniz; Objective-C ad alanları desteklemediğinden, kullanılacak gerekir `[Register]` Xamarin.iOS Objective-C çalışma zamanı ile kaydolacak türü adını değiştirmek için özniteliği. C# türü de devralmalıdır [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Üçüncü olarak, Objective-C belgelerini gözden geçirin ve oluşturma [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) örnekleri için kullanmak istediğiniz her Seçici. Bu sınıfın gövdesi içinde yerleştirin:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Dördüncü türünüz oluşturucular sağlamanız gerekir. *Gerekir* temel sınıf oluşturucusuna, oluşturucu çağrı zinciri. `[Export]` Öznitelikleri izin Objective-C kodunu belirtilen Seçici adıyla oluşturucuları çağırmak için:

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

Beşinci, adım 3'te yöntemleri her Seçici için bildirilen sağlar. Bunlar kullanacağı `objc_msgSend()` Seçici yerel nesnesi üzerinde çağırmak için. Kullanımına dikkat edin [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) dönüştürmek için bir `IntPtr` uygun şekilde yazılmış halinde `NSObject` (sub) türü. Yöntemin Objective-C koddan üyesi çağrılabilir olmasını istiyorsanız *gerekir* olması **sanal**.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

Hepsi bir araya getirildiğinde:

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)