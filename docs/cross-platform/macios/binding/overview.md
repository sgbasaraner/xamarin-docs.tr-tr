---
title: Genel Bakış
description: Bağlama işleminin nasıl çalıştığı ayrıntıları
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: d1a90934cf7a9a832172f32ed95cf3e254e04385
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="overview"></a>Genel Bakış

_Bağlama işleminin nasıl çalıştığı ayrıntıları_

Xamarin ile kullanmak için bir Objective-C Kitaplığı bağlama üç adımdan alır:

1. Bir C# "API tanımı" yazma açıklamak için nasıl yerel API gösterilir .NET ve nasıl temel Objective-c eşlemeleri Bu gerçekleştirilir gibi standart C# kullanarak yapıları `interface` ve çeşitli bağlama **öznitelikleri** (Bu bkz [basit bir örnek](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Bir kez "API tanımı" C# ' ta "bağlama" derlemesi üretmek derleme yazdığınız. Bu, üzerinde yapılabilir [ **komut satırı** ](#commandline) veya kullanarak bir [ **bağlama proje** ](#bindingproject) Mac veya Visual Studio için Visual Studio.

3. Tanımladığınız API'si kullanılarak yerel işlevselliği erişebilmesi için bu "bağlama" derleme daha sonra Xamarin uygulaması projenize eklenir.
  Uygulama projelerden tamamen ayrı bağlama projesidir.

**Not:** 1. adım otomatik hale getirilebilir Yardımı ile [ **hedefi Sharpie**](#objectivesharpie). Objective-C API inceler ve önerilen bir C# "API tanımı." oluşturur Hedefi Sharpie tarafından oluşturulan dosyalar özelleştirebilir ve bağlama projesinde (veya komut satırı) kullanmak, bağlama derlemenizi oluşturmak için. Amaç Sharpie tek başına bağlamaları oluşturmaz, yalnızca bir isteğe bağlı işleminin bir parçası daha büyük.

Ayrıca, daha fazla teknik ayrıntıları okuyabilirsiniz [nasıl çalışır](#howitworks), hangi yardımcı olur, bağlamaları yazmanızı sağlar.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Komut satırı bağlamaları

Kullanabileceğiniz `btouch-native` Xamarin.iOS için (veya `bmac-native` Xamarin.Mac kullanıyorsanız) bağlamaları doğrudan oluşturmak için. El ile oluşturduğunuz C# API tanımlarını geçirerek çalışır (veya hedefi Sharpie kullanarak) komut satırı aracını (`btouch-native` iOS için veya `bmac-native` Mac için).


Bu araçları çalıştırma genel sözdizimi şöyledir:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Yukarıdaki komut dosyası oluşturur `cocos2d.dll` geçerli dizinin ve projenizde kullanabileceğiniz tam olarak bağlanmış kitaplık içerecektir. Bu bağlama proje kullanırsanız, bağlamaları oluşturmak için Visual Studio Mac için kullandığı araçtır (açıklanan [aşağıda](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Binding Project

Bir bağlama proje Mac veya (Visual Studio iOS bağlamaları yalnızca destekler) Visual Studio için Visual Studio'da oluşturulabilir ve düzenleyebilir ve yapı (karşı komut satırını kullanarak) bağlama için API tanımlarını kolaylaştırır.

Bu izleyin [Başlangıç Kılavuzu](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) oluşturma ve bir bağlama üretmek için bir bağlama proje kullanma hakkında bilgi için.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Amaç Sharpie

Amaç Sharpie bağlama oluşturma ilk aşamaları yardımcı olan ayrı komut satırı aracıdır. Bu bağlama tek başına oluşturmaz; bunun yerine hedef yerel kitaplığı için bir API tanımı oluşturmanın ilk adımı otomatikleştirir.

Okuma [hedefi Sharpie belgeleri](~/cross-platform/macios/binding/objective-sharpie/index.md) bağlamaları yerleşik API defintions içine yerel kitaplıkları, yerel çerçeveler ve CocoaPods ayrıştırma öğrenmek için.

<a name="howitworks" />

## <a name="how-binding-works"></a>Bağlama nasıl çalışır

Kullanmak da mümkündür [[Kaydet]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) özniteliği [[verme]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) özniteliği ve [el ile Objective-C Seçici çağırma](~/ios/internals/objective-c-selectors.md) birlikte el ile yeni (daha önce bağlamak için İlişkisiz) Objective-C türleri.

İlk olarak bağlamak istediğiniz bir türünü bulun. Tartışma amacıyla (ve Basitlik), biz bağlamak [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) türü (hangi zaten bağımlı [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); aşağıdaki yalnızca örnek amaçlıdır uygulamasıdır).

İkinci olarak, biz C# türü oluşturmanız gerekir. Biz büyük olasılıkla bu ad alanına yerleştirmek istersiniz; Objective-C ad alanlarını desteklemez olduğundan, biz kullanmanız gerekecektir `[Register]` Xamarin.iOS Objective-C çalışma zamanı ile kaydedilecek tür adını değiştirmek için öznitelik. C# türü de öğesinden devralmalıdır [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Üçüncü Objective-C belgelerini gözden geçirin ve oluşturma [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) örnekleri için kullanmak istediğiniz her Seçici. Bu sınıf gövdesi içinde koyun:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Dördüncü türünüz oluşturucular sağlamanız gerekir. *Gerekir* temel sınıf oluşturucuya, oluşturucu çağrı zincirine bağlı. `[Export]` Öznitelikleri Objective-C kodunu belirtilen Seçici adla oluşturucular çağırmak için izin ver:

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

Beşinci, adım 3'te her Seçici yöntemleri bildirilen sağlar. Bunlar kullanacağı `objc_msgSend()` Seçici yerel nesne üzerinde çağırmak için. Kullanımına dikkat edin [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) dönüştürmek için bir `IntPtr` uygun şekilde yazılan içine `NSObject` (alt) türü. Yöntemin Objective-C kodunu, üye aranabilir olmasını istiyorsanız *gerekir* olması **sanal**.

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

Tüm bir araya getirilmesi:

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

