---
title: Objective-C desteği
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 74d62121e9c99061c118f3ab85c27328ca950b9d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="objective-c-support"></a>Objective-C desteği

## <a name="specific-features"></a>Belirli özellikler

ObjC nesil belirtmeye değer olan birkaç, özel bir özelliğe sahiptir.

### <a name="automatic-reference-counting"></a>Otomatik başvuru sayımı

Otomatik başvuru sayım (ARC) kullanılmasıdır **gerekli** oluşturulan bağlamaları çağırmak için. Embeddinator tabanlı kitaplığı kullanarak proje derlenmiş, ile `-fobjc-arc`.

### <a name="nsstring-support"></a>NSString desteği

Kullanıma API'leri `System.String` türleri uygulamasına dönüştürülür `NSString`. Bu bellek yönetimi postalarla daha kolay hale getirir `char*`.

### <a name="protocols-support"></a>Protokolleri destekler

Yönetilen arabirimleri, tüm üyeleri nerede ObjC protokollere dönüştürülür `@required`.

### <a name="nsobject-protocol-support"></a>NSObject protokol desteği

Varsayılan olarak varsayılan karma olan varsayıyoruz ve eşitlik hem .net hem de ObjC çalışma zamanı, ince ve birbirinin yerine çok benzer bir semantik paylaştıkları gibi.

Ne zaman bir yönetilen türü geçersiz kılmaları `Equals(Object)` veya `GetHashCode` genellikle defaut (.NET) davranışa en iyisi değil demektir. Varsayılan Objective-C davranışa ya da olacağı kabul edilebilir.

Böyle durumda Oluşturucu geçersiz kılmaları [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) yöntemi ve [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) tanımlanan özelliği [ `NSObject` Protokolü](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Bu ObjC koddan saydam olarak kullanılmak üzere özel yönetilen uygulama sağlar.

### <a name="comparison"></a>Karşılaştırma

Yönetilen uygulama türleri `IComparable` veya genel sürümü `IComparable<T>` döndürür ObjC kolay yöntemleri oluşturacak bir `NSComparisonResult` ve kabul bir `nil` bağımsız değişkeni. Bu API ObjC geliştiriciler, daha kolay örneğin sağlar

```csharp
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Kategoriler

Yönetilen Uzantılar yöntemleri kategoriye dönüştürülür. Örneğin aşağıdaki genişletme yöntemleri üzerinde `Collection`:

```csharp
    public static class SomeExtensions {

        public static int CountNonNull (this Collection collection) { ... }

        public static int CountNull (this Collection collection) { ... }
    }
```

Bunun gibi bir Objective-C kategorisi oluşturacak:

```csharp
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;
@end
```

Tek bir yönetilen türü birden çok Objective-C kategori oluşturulan sonra çeşitli türleri genişletir.

### <a name="subscripting"></a>Alt Simge Oluşturma

Yönetilen Dizinli Özellikler nesne alt simge oluşturma uygulamasına dönüştürülür. Örneğin:

```csharp
    public bool this[int index] {
        get { return c[index]; }
        set { c[index] = value; }
    }
```

Objective-C benzer oluşturacak:

```csharp
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

hangi Objective-C subscripting sözdizimi kullanılabilir:

```csharp
    if ([intCollection [0] isEqual:@42])
        intCollection[0] = @13;
```

Dizin türüne bağlı olarak, uygun olan yerlerde dizinlenmiş veya anahtarlı alt simge oluşturma oluşturulur.

Bu [makale](http://nshipster.com/object-subscripting/) alt simge oluşturma için harika bir giriş değil.

## <a name="main-differences-with-net"></a>.NET ile temel farklar

### <a name="constructors-vs-initializers"></a>Oluşturucular vs başlatıcıları

Kullanılamaz (NS_UNAVAILABLE) işaretlenmemişse Objective-C, başlatıcı hiçbirini herhangi bir üst sınıf devralma zincirinde prototipleri çağırabilirsiniz.

C# ' ta, açıkça Oluşturucusu üyesi bir sınıf içinde bildirmeniz gerekir, bu oluşturucular devralınmaz anlamına gelir.

C# API Objective-C için doğru gösterimini kullanıma sunmak için eklediğimiz `NS_UNAVAILABLE` üst sınıfı'ndan alt sınıfında mevcut değil Başlatıcısı için.

C# API:

```csharp
public class Unique {
    public Unique () : this (1)
    {
    }

    public Unique (int id)
    {
    }
}

public class SuperUnique : Unique {
    public SuperUnique () : base (911)
    {
    }
}
```

Objective-C API ortaya:

```objectivec
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Burada görebiliriz `initWithId:` kullanılamaz olarak işaretlenmiş.

### <a name="operator"></a>İşleç

ObjC işleci desteklemediği C# yaptığı gibi işleçleri sınıf seçici dönüştürülür için aşırı yükleme:

```csharp
    public static AllOperators operator + (AllOperators c1, AllOperators c2)
    {
        return new AllOperators (c1.Value + c2.Value);
    }
```

to

```csharp
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

De dahil yaygın şekilde ancak, bazı .NET dillerini İşleç aşırı yüklemesi, desteklemeyen bir ["kolay"](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx) yöntemi işleci aşırı yanı sıra adlı.

İşleç sürümü ve "kolay" sürümü bulunursa, yalnızca kolay sürüm oluşturulmayacak, aynı objective-c adına oluşturacak şekilde.

```csharp
    public static AllOperatorsWithFriendly operator + (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
    {
        return new AllOperatorsWithFriendly (c1.Value + c2.Value);
    }

    public static AllOperatorsWithFriendly Add (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
    {
        return new AllOperatorsWithFriendly (c1.Value + c2.Value);
    }
```

Olur:

```csharp
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Eşitlik işleci

İçinde genel işlecinde == C# bir genel operatör olarak belirtilenlerle yukarıdaki işlenir.

Ancak, "kolay" eşittir işleci bulunması durumunda, her iki işleç == ve işleci! = oluşturma atlandı.

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

Gelen [NSDate'nın](https://developer.apple.com/reference/foundation/nsdate?language=objc) belgeleri:

> NSDate nesneleri tek bir nokta, zaman içindeki herhangi belirli calendrical sistem veya saat dilimi bağımsız kapsüller. Date nesneleri temsil eden bir sabit zaman aralığı bir mutlak başvuru tarihi göre değişmez (00:1 Ocak 2001 00:00 UTC).

Nedeniyle `NSDate` başvuru tarih, onu arasındaki tüm dönüştürmeler ve `DateTime` UTC olarak yapılmalıdır.

#### <a name="datetime-to-nsdate"></a>DateTime NSDate için

Dönüştürme zaman `DateTime` için `NSDate` DateTime's `Kind` özelliğini dikkate alınır.

|türü|Sonuçları                                                                                            |
|---|---|
|UTC|Dönüştürme, sağlanan kullanarak gerçekleştirilir `DateTime` nesnesi olarak.|
|Yerel|Arama sonucu `ToUniversalTime()` sağlanan içinde `DateTime` nesnesi, dönüştürme için kullanılır.|
|Belirtilmemiş|Sağlanan `DateTime` nesne olduğu varsayılır UTC, böylece aynı davranışı türü Utc ==.|

Dönüştürme, aşağıdaki formül kullanılarak yapılır:

> [!NOTE]
> **TimeInterval** DateTimeObjectTicks - NSDateReferenceDateTicks [dt] = / [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx)

Biz TimeInterval sonra NSDate'nın kullanırız [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) Seçici oluşturun.

#### <a name="nsdate-to-datetime"></a>DateTime değerine NSDate

Bir NSDate alma biz varsayıyoruz DateTime NSDate giderek başvuru tarihi olan örneğidir **1 Ocak 2001 00:00:00 UTC** ve aşağıdaki formülü kullanın:

> [!NOTE]
> **DateTimeTicks** = NSDateTimeIntervalSinceReferenceDate * [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx) + NSDateReferenceDateTicks[dt]

Biz hesaplanan sonra **DateTimeTicks** aşağıdaki DateTime kullanırız [Oluşturucusu](https://msdn.microsoft.com/en-us/library/w0d47c9c(v=vs.110).aspx) ayarı kendi `kind` için `DateTimeKind.Utc`.

Farkında olması gereken bazı noktalar vardır, NSDate olabilir `nil` ancak bir DateTime .NET yapıda ve tanım gereği, olamaz `null`. Size, bir `nil` NSDate biz Çevir onu eşleştiren varsayılan DateTime değerine `DateTime.MinValue`.

MinValue ve MaxValue ayrıca farklı, büyük veya daha düşük bir değer verdiğinizde, DateTime türüne ait ayarlarız şekilde NSDate DateTime'nın daha büyük ve alt sınırları destekleyebilmesi [MaxValue](https://msdn.microsoft.com/en-us/library/system.datetime.maxvalue(v=vs.110).aspx) veya [MinValue](https://msdn.microsoft.com/en-us/library/system.datetime.minvalue(v=vs.110).aspx) sırasıyla.

**dt**: NSDate başvuru tarihi **1 Ocak 2001 00:00:00 UTC** => `new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;`
