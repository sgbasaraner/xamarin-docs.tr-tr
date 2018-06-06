---
title: Objective-C için en iyi uygulamaları .NET katıştırma
description: Bu belgede .NET katıştırma Objective-c ile kullanmak için çeşitli en iyi uygulamalar açıklanmaktadır Yönetilen kod kümesini gösterme, chunkier API gösterme, adlandırma ve daha fazla açıklanır.
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: b4b0df6f1c7c1d5931c0c18a1508747a7c570bea
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793503"
---
# <a name="net-embedding-best-practices-for-objective-c"></a>Objective-C için en iyi uygulamaları .NET katıştırma

Bu bir taslak ve içinde eşitleme özellikleriyle aracı tarafından şu anda desteklenmiyor olabilir. Umuyoruz bu belgeyi ayrı olarak gelişmesi ve sonunda son aracı eşleşmesi, uzun vadeli en iyi yaklaşımın - değil hemen geçici çözümler yani önerdiğimiz.

Bu belgenin büyük bir bölümü, desteklenen diğer diller için de geçerlidir. Ancak tüm örnekler C# ve Objective-c sağlanır

## <a name="exposing-a-subset-of-the-managed-code"></a>Yönetilen kod kümesini gösterme

Oluşturulan yerel kitaplığı/framework her sunulan yönetilen API'leri çağırmak için Objective-C kodunu içerir. Daha fazla API, yüzey (ortak olun) sonra büyük yerel _Birleştirici_ kitaplığı hale gelir.

Yalnızca yerel geliştiriciye gerekli API'leri için farklı, daha küçük derleme oluşturmak için iyi bir fikir olabilir. Bu cephesi da adlandırma, oluşturulan kodunu... denetlenirken hata görünürlük hakkında daha fazla denetime izin verir.

## <a name="exposing-a-chunkier-api"></a>Chunkier API gösterme

Yönetilen (ve geri) geçiş gelen yerel ödeme yapmak için bir fiyat yoktur. Bu nedenle, kullanıma sunmak daha iyi _chatty yerine chunky_ API'leri yerel geliştiricilere örn.

**Chatty**

```csharp
public class Person {
  public string FirstName { get; set; }
  public string LastName { get; set; }
}
```

```objc
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Chunky**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

Geçişlerin sayısı daha küçük olduğundan performansı daha iyi olur. Bu da daha küçük bir yerel kitaplık oluşturacak şekilde ayrıca oluşturulması için daha az kod gerektirir.

## <a name="naming"></a>Adlandırma

Nesneleri adlandırma iki en zor sorunlar bilgisayar bilimi diğer önbellek geçersiz kılma ve devre dışı-göre-1 hataları olan biridir. Neyse .NET katıştırma dışındaki tüm adlandırma kalkanı.

### <a name="types"></a>Türler

Objective-C ad alanlarını desteklemez. Genel olarak, türlerinden 2 ile (için Apple) önek veya 3 (için 3. taraflar) karakter öneki gibi `UIView` Uıkit'ın görünümü için hangi gösterir framework.

Yinelenen ya da kafa karıştırıcı, adları getirebilir gibi .NET türleri için ad alanı atlanıyor mümkün değildir. Bu, örneğin mevcut .NET türleri çok uzun sağlar

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

gibi kullanılır:

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

Ancak türü olarak yeniden getirebilir:

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

Daha fazla Objective-C kolay hale getirme, örneğin kullanılacak:

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>Yöntemler

Daha iyi .NET adları Objective-C API için ideal olmayabilir.

Objective C'de adlandırma kuralları .NET (pascal durumda, daha ayrıntılı yerine ortası büyük harf) farklıdır.
Lütfen okuyun [kodlama yönergeleri Cocoa için](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF).

Açısından bir Objective-C Geliştirici, bir yöntemle bir `Get` öneki gelir sahibi örneği, yani [kuralını Al](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

Bu adlandırma kuralı .NET GC dünyada eşleşme vardır; .NET yöntemi ile bir `Create` öneki aynı şekilde davranır .NET içinde. Ancak, Objective-C geliştiriciler için normalde size ait döndürülen örneği, yani geldiğini [kuralını](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029).

## <a name="exceptions"></a>Özel Durumlar

.NET özel durumlar hatalarını raporlamak için kapsamlı bir şekilde kullanmak için bu durum oldukça yaygındır. Ancak, bunlar yavaş ve Objective C'deki oldukça aynı Mümkün olduğunda Objective-C geliştiriciden gizle.

Örneğin, .NET `Try` düzeni Objective-C kodunu kullanmak çok daha kolay olacaktır:

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

karşılaştırması

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>İçinde özel durumlar `init*`

.NET içinde bir oluşturucu ya da başarılı dönün ve gereken bir (_umarız_) geçerli örneği veya throw bir özel durum.

Buna karşılık, Objective-C verir `init*` döndürülecek `nil` ne zaman bir örneği oluşturulamıyor. Apple'nın çerçeveler birçoğu kullanılan ortak ancak değil genel bir desen budur. Diğer bazı durumlarda bir `assert` durum (ve geçerli işlemi sonlandırılamadı).

Oluşturucunun izleyin aynı `return nil` oluşturulan için desen `init*` yöntemleri. Yönetilen bir özel durum oluşturulduktan sonra onu basılır (kullanarak `NSLog`) ve `nil` çağırana döndürülür.

## <a name="operators"></a>İşleçler

Objective-C C# yaptığı gibi bunlar için sınıf seçici dönüştürülür şekilde aşırı yüklenmiş işleçler izin vermiyor.

["Kolay"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) adlandırılmış yöntemleri işlecin yerine oluşturulan zaman bulundu ve kolay bir API kullanmak üretebilir.

İşleçler geçersiz kılma sınıfları `==` ve/veya `!=` de standart eşittir (nesne) yöntemin üzerine yazması gerekir.
