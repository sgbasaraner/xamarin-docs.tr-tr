---
title: Objective-C desteği
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 66ba31b1054559516fdbfbeb0421a82f4a0e9fa5
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="objective-c-support"></a>Objective-C desteği

## <a name="specific-features"></a>Belirli özellikler

Objective-C nesil belirtmeye değer olan birkaç özelliklere sahiptir.

### <a name="automatic-reference-counting"></a>Otomatik başvuru sayımı

Otomatik başvuru sayım (ARC) kullanılmasıdır **gerekli** oluşturulan bağlamaları çağırmak için. Embeddinator tabanlı kitaplığı kullanarak proje derlenmiş, ile `-fobjc-arc`.

### <a name="nsstring-support"></a>NSString desteği

Kullanıma API'leri `System.String` türleri uygulamasına dönüştürülür `NSString`. Bu bellek yönetimi ile zaman ilgilenme daha kolay hale getirir `char*`.

### <a name="protocols-support"></a>Protokolleri destekler

Yönetilen arabirimleri, tüm üyeleri nerede Objective-C protokollere dönüştürülür `@required`.

### <a name="nsobject-protocol-support"></a>NSObject protokol desteği

Varsayılan olarak, varsayılan karma ve eşitlik hem .NET hem de Objective-C çalışma zamanı benzer bir semantik paylaşımı şeklinde birbirinin yerine, olarak kabul edilir.

Ne zaman bir yönetilen türü geçersiz kılar `Equals(Object)` veya `GetHashCode`, genellikle varsayılan (.NET) davranış yeterli değil demektir; bu varsayılan Objective-C davranışı olasıdır gelir yeterli ya da.

Böyle durumlarda, oluşturucu geçersiz kılmaları [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) yöntemi ve [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) tanımlanan özelliği [ `NSObject` Protokolü](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Bu, Objective-C kodunu saydam olarak kullanılmak üzere özel yönetilen uygulama sağlar.

### <a name="exceptions-support"></a>Özel durumlar desteği

Geçirme `--nativeexception` bağımsız değişken olarak `objcgen` yönetilen özel durumlar yakalanan ve işlenen Objective-C özel durumlar dönüştürür. 

### <a name="comparison"></a>Karşılaştırma

Yönetilen uygulama türleri `IComparable` (veya genel sürümünün `IComparable<T>`) döndüren Objective-C kolay yöntemler oluşturacak bir `NSComparisonResult` ve kabul bir `nil` bağımsız değişkeni. Bu API Objective-C geliştiriciler için daha kolay hale getirir. Örneğin:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Kategoriler

Yönetilen Uzantılar yöntemleri kategoriye dönüştürülür. Örneğin, aşağıdaki genişletme yöntemleri üzerinde `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

Bunun gibi bir Objective-C kategorisi oluşturacak:

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

Tek bir yönetilen türü çeşitli türleri genişletir, birden çok Objective-C kategori üretilir.

### <a name="subscripting"></a>Alt Simge Oluşturma

Yönetilen Dizinli Özellikler nesne alt simge oluşturma uygulamasına dönüştürülür. Örneğin:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

Objective-C benzer oluşturacak:

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

hangi Objective-C subscripting sözdizimi kullanılabilir:

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

Dizin türüne bağlı olarak, uygun olan yerlerde dizinlenmiş veya anahtarlı alt simge oluşturma oluşturulur.

Bu [makale](http://nshipster.com/object-subscripting/) alt simge oluşturma için harika bir giriş değil.

## <a name="main-differences-with-net"></a>.NET ile temel farklar

### <a name="constructors-vs-initializers"></a>Oluşturucular vs başlatıcıları

Kullanılamaz olarak işaretlendiğinden sürece Objective-C, başlatıcı hiçbirini herhangi bir üst sınıf devralma zincirinde prototipleri çağırabilirsiniz (`NS_UNAVAILABLE`).

C# ' ta Oluşturucusu üyesi oluşturucular devralınmaz anlamına gelir bir sınıf içinde açıkça bildirilmelidir.

Objective-C, C# API'si sağ gösterimini kullanıma sunmak için `NS_UNAVAILABLE` üst sınıfı'ndan alt sınıfında mevcut değil Başlatıcı eklenir.

C# API'Sİ:

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

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Burada, `initWithId:` kullanılamaz olarak işaretlenmiş.

### <a name="operator"></a>İşleç

Objective-C işleci desteklemediği C# yaptığı gibi işleçleri sınıf seçici dönüştürülür için aşırı yükleme:

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

to

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

De dahil yaygın şekilde ancak, bazı .NET dillerini İşleç aşırı yüklemesi, desteklemeyen bir ["kolay"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) yöntemi işleci aşırı yanı sıra adlı.

İşleç sürümü ve "kolay" sürümü bulunursa, yalnızca kolay sürüm oluşturulmayacak, aynı Objective-C adına oluşturacak şekilde.

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

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Eşitlik işleci

Genel işlecinde `==` C# ' ta yukarıda da belirtildiği gibi işleci genel işlenir.

Ancak, "kolay" eşittir işleci bulunması durumunda, her iki işleç `==` and işleci `!=` oluşturma atlanacak.

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

Gelen [ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc) belgeleri:

> `NSDate` nesnelerin tek bir nokta, zaman içindeki herhangi belirli calendrical sistem veya saat dilimi bağımsız kapsüller. Date nesneleri temsil eden bir sabit zaman aralığı bir mutlak başvuru tarihi göre değişmez (00:1 Ocak 2001 00:00 UTC).

Nedeniyle `NSDate` başvuru tarih, onu arasındaki tüm dönüştürmeler ve `DateTime` UTC olarak yapılmalıdır.

#### <a name="datetime-to-nsdate"></a>DateTime NSDate için

Dönüştürme zaman `DateTime` için `NSDate`, `Kind` özellikte `DateTime` dikkate alınır:

|türü|Sonuçları|
|---|---|
|`Utc`|Dönüştürme, sağlanan kullanarak gerçekleştirilir `DateTime` nesnesi olarak.|
|`Local`|Arama sonucu `ToUniversalTime()` sağlanan içinde `DateTime` nesnesi, dönüştürme için kullanılır.|
|`Unspecified`|Sağlanan `DateTime` nesne olduğu varsayılır, böylece aynı davranışı UTC zaman `Kind` olan `Utc`.|

Dönüştürme, aşağıdaki formülü kullanır:

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

Bu formülde: 

- `NSDateReferenceDateTicks` temel alınarak hesaplanır `NSDate` başvuru 00:00:00 UTC tarihi 1 Ocak 2001: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) üzerinde tanımlanan [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Oluşturmak için `NSDate` nesnesi `TimeInterval` ile kullanılan `NSDate` [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) Seçici.

#### <a name="nsdate-to-datetime"></a>DateTime değerine NSDate

Dönüştürme işlemi `NSDate` için `DateTime` aşağıdaki formülü kullanır:

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

Bu formülde: 

- `NSDateReferenceDateTicks` temel alınarak hesaplanır `NSDate` başvuru 00:00:00 UTC tarihi 1 Ocak 2001: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) üzerinde tanımlanan [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Hesaplandıktan sonra `DateTimeTicks`, `DateTime` [Oluşturucusu](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) , ayarı çağrılır kendi `kind` için `DateTimeKind.Utc`.

> [!NOTE]
> `NSDate` olabilir `nil`, ancak bir `DateTime` okunamayan tanımı tarafından .NET içindeki bir yapı `null`. Size, bir `nil` `NSDate`, varsayılan çevrilir `DateTime` eşlendiği değeri `DateTime.MinValue`.

`NSDate` daha yüksek bir maksimum ve daha düşük bir minimum değer destekleyen `DateTime`. Dönüştürme zaman `NSDate` için `DateTime`, bu daha yüksek ve düşük değerler dönüştürülen `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) veya [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)sırasıyla.
