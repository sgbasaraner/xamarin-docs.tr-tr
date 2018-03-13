---
title: NSString
ms.topic: article
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 87cc1a95f250069310941e051dabe9ea588b4709
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="nsstring"></a>NSString

Xamarin.iOS ve Xamarin.Mac tasarımına yerel .NET dize türünde kullanıma sunmak için kullanım API çağrılarının `string`, dize düzenlemesi C# ve diğer .NET programlama dilleri için ve dize yerineAPI'sitarafındankullanımasunulanveritürüolarakkullanımasunmakiçin`NSString` veri türü.


Bu geliştiriciler Xamarin.iOS & Xamarin.Mac API (Birleşik) içine çağırmak için kullanılması hedeflenen dizeleri tutmak olmamalıdır özel bir tür anlamına gelir (`Foundation.NSString`), bunlar Mono'nın kullanmaya devam edebileceğiniz `System.String` tüm işlemler için ve her bir dizeyi bir API Xamarin.iOS veya Xamarin.Mac gerektirir, bizim API bağlama mvc'deki bilgileri hazırlama.

Örneğin, Objective-C "metin" özelliği bir `UILabel` türü `NSString`, şöyle bildirilmiş:

```csharp
@property(nonatomic, copy) NSString *text
```

Bu Xamarin.iOS sunulur:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

Arka planda, bu özellik uyarlamasını C# dizeye sıralar bir `NSString` ve çağırır `objc_msgSend` Objective-C misiniz aynı şekilde yöntemi.

Değil tüketen üçüncü taraf Objective-C API'lerini sayıda vardır bir `NSString`, ancak bunun yerine bir C dize tüketebilir (bir "*char*"). Bu gibi durumlarda, C# dize veri türü kullanmaya devam edebilirsiniz, ancak kullanmalısınız [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) özniteliği bu dize olarak sıralanmalıdır değil bağlama Oluşturucu bildirmek üzere bir `NSString`, ancak bunun yerine C dize olarak.

 <a name="Exceptions_to_the_Rule" />


## <a name="exceptions-to-the-rule"></a>Kural için özel durumlar

Xamarin.iOS hem Xamarin.Mac Biz bu kural için bir özel yapıldı. Ne zaman biz kullanıma arasında karar `string`s, ve ne zaman vermiyoruz bir dışındaki ve kullanıma `NSString`s, varsa yapılan `NSString` yöntemi bir işaretçi karşılaştırması içerik karşılaştırma yerine yapılması.


Objective C API'lerini genel kullandığında oluşabilir `NSString` sabit dize gerçek içeriğini karşılaştırma yerine bazı eylemleri temsil eden bir belirteci olarak.


Bu durumlarda, `NSString` API'leri sunulur ve bu API'ları nadiren vardır. Ayrıca bazı sınıflarda NSString özellikleri sunulan fark edeceksiniz. Bu `NSString` özellikleri bildirimleri gibi öğeleri için sunulur. Bu özellikleri genellikle şuna şunlardır:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

Bildirimler için kullanılan anahtarları olan `NSNotification` sınıfı çalışma zamanı tarafından yayınlandığı belirli bir olay kaydetmek istediğinizde.

Anahtarlar genellikle aşağıdaki gibi görünmelidir:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Başka bir yere nerede `NSString`s sunulur iOS belirli API'leri veya OS X ele parametre olarak kullanılan belirteçleri API gibidir `NSDictionary` parametre olarak nesneleri. Sözlük genellikle içeriyorsa `NSString` anahtarları. Xamarin.iOS, kurala göre adları statik olanlar `NSString` "Anahtar" adı ekleyerek özellikleri.
