---
title: macOS Xamarin.Mac geliştiriciler için API'leri
description: Bu belge, Objective-C seçicileri okuma ve karşılık gelen kendi C# yöntemlerini bir Xamarin.Mac uygulamasını bulmak nasıl açıklar.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: 6dfaa3c7bf988228bfbacefe7c8e7268edc8117a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994318"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>macOS Xamarin.Mac geliştiriciler için API'leri

## <a name="overview"></a>Genel Bakış

Xamarin.Mac ile geliştirirken zamanınızın çoğunu için düşünme, okuma ve temel alınan Objective-C API'leri ile kadar kaygısı olmadan C# dilinde yazın. Ancak, bazen gerektiğinde Apple'dan API belgeleri okumak için sorununuz için Stack Overflow gelen yanıt bir çözüme Çevir veya var olan bir örnek için karşılaştırın.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Tehlikeli olacak şekilde yeterli Objective C okuma

Bazen bu Objective-C tanımı okumak gerekli olacaktır veya yöntemi çağırma ve, eşdeğer C# yöntemine çevir. Şimdi bir Objective-C işlev tanımı göz atın ve parçalara bölün. Bu yöntem (bir *Seçici* Objective-c) bulunabilir `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Bildirimi soldan sağa okuyabilirsiniz:

- `-` Önek anlamına gelir (statik olmayan) örnek yöntemi olduğundan. + sınıfı (statik) yöntemdir anlamına gelir
- `(BOOL)` dönüş türü (C#, Boole)
- `canDragRowsWithIndexes` ilk adının parçasıdır.
- `(NSIndexSet *)rowIndexes` İlk parametre olan ve onunla türüne sahip. İlk parametresi şu biçimdedir: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` İkinci parametre ve onun türü değildir. Birinciden sonraki her parametre biçimi şu şekildedir: `selectorPart:(Type) pararmName`
- Bu ileti Seçici tam adıdır: `canDragRowsWithIndexes:atPoint:`. Not `:` sonunda - önemlidir.
- Gerçek Xamarin.Mac C# bağlama aşağıdaki gibidir: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Aynı şekilde bu Seçici çağırma okuyabilirsiniz:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Örnek `v` yaşıyor kendi `canDragRowsWithIndexes:atPoint` iki parametrelerle çağrıldı Seçici `set` ve `point`, geçirilen.
- C# ' ta yöntem çağırma şöyle görünür: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>C# üyesi için belirtilen Seçici bulma

Çağırmak için gereken Objective-C Seçici buldunuz, sonraki adım, eşdeğer C# üyesine eşliyor. Dört yaklaşımlar deneyebilirsiniz vardır (devam etmeden `NSTableView CanDragRows` örnek):

1. Hızlıca bir şeyler aynı ada taramak için Otomatik Tamamlama listesini kullanın. Biz bildiğiniz örneğidir `NSTableView` yazabilirsiniz:

    - `NSTableView x;`
    - `x.` [ctrl + boşluk listede görünmüyorsa).
    - `CanDrag` [girin]
    - Yönteme sağ tıklayın, burada karşılaştırabilirsiniz bütünleştirilmiş kod tarayıcı açmak için bildirimine gidin `Export` öznitelik söz konusu Seçici

2. Sınıfın tamamı bağlama arayın. Biz bildiğiniz örneğidir `NSTableView` yazabilirsiniz:

    - `NSTableView x;`
    - Sağ `NSTableView`bütünleştirilmiş kod tarayıcı bildirimine gidin
    - Söz konusu Seçici arayın

3. Kullanabileceğiniz [Xamarin.Mac API çevrimiçi belgeleri](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0) .

4. Miguel Xamarin.Mac API'leri "Rosetta taş" görünümünü sağlar [burada](http://tirania.org/tmp/rosetta.html) , belirli bir API için aracılığıyla arayabilirsiniz. API'nizi AppKit veya Mac OS x özel değilse, burada bulabilirsiniz.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
