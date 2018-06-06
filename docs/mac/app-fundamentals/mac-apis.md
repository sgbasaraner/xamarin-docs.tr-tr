---
title: macOS Xamarin.Mac geliştiriciler için API'leri
description: Bu belge, Objective-C seçiciler okuyun ve Xamarin.Mac uygulamada kendi ilgili C# yöntemleri bulmak açıklar.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: cceaa2f6e89b712be5929f7e978663d8c47f18c5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791556"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>macOS Xamarin.Mac geliştiriciler için API'leri

## <a name="overview"></a>Genel Bakış

Xamarin.Mac ile geliştirme zaman çoğunu için düşünün, okuyabilir ve C# ' ta temel Objective-C API'leri ile kadar sorun olmadan yazma. Ancak, bazen gerektiğinde API belgelerine Apple'dan okumak için sorun için çözüm yığın taşması gelen yanıt Çevir veya varolan bir örnek karşılaştırın.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Tehlikeli olması için yeterli Objective C okuma

Bazen bir Objective-C tanımı okumak gerekli verilir veya yöntemini çağırın ve eşdeğer C# yöntemine çevir. Şimdi bir Objective-C işlev tanımı göz atın ve parçaları bölün. Bu yöntem (bir *Seçici* Objective C'de) bulunabilir `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Bildirim soldan sağa okuyabilirsiniz:

- `-` Öneki anlamına gelir, bir örnek (statik olmayan) yöntemi. + sınıfı (statik) yöntem olduğu anlamına gelir
- `(BOOL)` dönüş türü (Boole C#)
- `canDragRowsWithIndexes` adının ilk bölümüdür.
- `(NSIndexSet *)rowIndexes` İlk parametre olan ve onunla sahip yazın. İlk parametresi şu biçimdedir: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` İkinci parametre türü ise. Her parametre ilk sonra biçimi şöyledir: `selectorPart:(Type) pararmName`
- Bu ileti Seçici tam adıdır: `canDragRowsWithIndexes:atPoint:`. Not `:` sonunda - önemlidir.
- Gerçek Xamarin.Mac C# bağlama aşağıdaki gibidir: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Aynı şekilde bu Seçici çağırma okuyabilirsiniz:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Örnek `v` yaşıyor kendi `canDragRowsWithIndexes:atPoint` iki parametrelerle adlı Seçici `set` ve `point`, geçirilen.
- C# ' ta yöntemi çağırma şöyle görünür: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>C# üye için sağlanan Seçici bulma

Çağırma gerek Objective-C Seçici buldunuz, sonraki adım, eşdeğer C# üyesine eşlemektir. Deneyebilirsiniz dört yaklaşım vardır (devam etmeden `NSTableView CanDragRows` örnek):

1. Hızla bir şey aynı adı taramak için Otomatik Tamamlama listesini kullanın. Bunu biliyoruz örneği olduğu `NSTableView` yazabilirsiniz:

    - `NSTableView x;`
    - `x.` [ctrl + listede görünmüyorsa alanı).
    - `CanDrag` [girin]
    - Yönteme sağ tıklayın, burada karşılaştırabilirsiniz derleme tarayıcı açmak için bildirimine Git `Export` özniteliği söz konusu Seçici

2. Tüm sınıf bağlama arayın. Bunu biliyoruz örneği olduğu `NSTableView` yazabilirsiniz:

    - `NSTableView x;`
    - Sağ `NSTableView`derleme tarayıcı bildirimine gidin
    - Söz konusu Seçici arayın

3. Kullanabileceğiniz [Xamarin.Mac API çevrimiçi belgeleri](https://developer.xamarin.com/api/root/monomac-lib/) .

4. Miguel Xamarin.Mac API'leri "Rosetta taş" görünümünü sağlar [burada](http://tirania.org/tmp/rosetta.html) için belirli bir API aracılığıyla arama. API'nizi AppKit veya macOS belirli değilse var. bulabilirsiniz.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
