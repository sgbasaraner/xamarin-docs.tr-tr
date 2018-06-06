---
title: İOS ve macOS için yerel türler
description: Bu belgede nasıl Xamarin'ın birleşik API .NET türleri 32-bit ve 64-bit yerel türler, gerektiği gibi eşlendiği derleme hedef mimarisine bağlı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fc2b91a9265fcf09e4f58d5de27a1fdef9350b2d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781110"
---
# <a name="native-types-for-ios-and-macos"></a>İOS ve macOS için yerel türler

Mac ve iOS API'leri her zaman 32 bit 32-bit platformlarda ve 64 bit platformlarda 64 bit mimari özgü veri türlerini kullanın.

Örneğin, Objective-C eşlemeleri `NSInteger` veri türü için `int32_t` 32 bit sistemler ve çok `int64_t` 64 bit sistemler üzerinde.

Bu davranış, birleştirilmiş API'mize eşleşecek şekilde biz önceki kullanımlarını değiştirdiğiniz `int` (.NET içinde tanımlanan her zaman olduğu `System.Int32`) için yeni bir veri türü: `System.nint`. Yerel tamsayı platformunun türü için "n" / anlamı "yerel" düşünebilirsiniz.

Bu yeni veri türleri ile aynı kaynak kodu derleme bayrakları bağlı olarak 32-bit ve 64 bit mimari derlenir.

## <a name="new-data-types"></a>Yeni veri türleri

Aşağıdaki tabloda, bu yeni bir 64/32-bit dünya eşleşecek şekilde bizim veri türleri değişiklikleri gösterir:

|Yerel tür|32-bit yedekleme türü|64-bit yedekleme türü|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

Fazla veya az bugün görünür şekilde aramak, C# kodu izin vermek için bu adları seçtik.

### <a name="implicit-and-explicit-conversions"></a>Örtük ve Açık Dönüştürmeler

Yeni veri türlerinin tasarım 32 veya 64 bit depolama ana bilgisayar platformu ve derleme ayarları bağlı olarak doğal olarak kullanılacak tek bir C# kaynak dosyası sağlamak için tasarlanmıştır.

Bu, bize .NET Entegral ve kayan nokta veri türleri için örtük ve açık dönüştürmeler için ve platforma özgü veri türlerini kümesi tasarlamak gerekli.

Örtük dönüşümler işleçleri, olası veri kaybını (32 bit değerleri bir 64 bit alanında depolanmakta) olduğunda sağlanır.

Açık dönüşümler işleçleri bir olası veri kaybını olduğunda verilmiştir (64 bit değeri depolanıyor 32 veya potansiyel olarak 32 bir depolama konumunda).

 `int`, `uint` ve `float` tümü için örtük olarak dönüştürülebilir `nint`, `nuint` ve `nfloat` 32 bit 32 veya 64 bit cinsinden her zaman uygun şekilde.

 `nint`, `nuint` ve `nfloat` tümü için örtük olarak dönüştürülebilir `long`, `ulong` ve `double` 32 veya 64 bit değerleri her zaman 64 bit depolama alanına sığabilecek.

Açık dönüşümler gelen kullanmalısınız `nint`, `nuint` ve `nfloat` içine `int`, `uint` ve `float` yerel türleri depolama 64 bit tutabilir beri.

Açık dönüşümler gelen kullanmalısınız `long`, `ulong` ve `double` içine `nint`, `nuint` ve `nfloat` yerel türler yalnızca 32 bit depolama tutabilecek özellikte olabileceğinden.

## <a name="coregraphics-types"></a>CoreGraphics türleri

CoreGraphics ile kullanılan noktası, boyut ve dikdörtgen veri türleri 32 veya 64 bit bunlar çalıştıran aygıtları bağlı olarak kullanın.  Biz ilk olarak iOS ve Mac API'leri bağlı olduğunda ana bilgisayar platformu boyutlarını eşleşecek şekilde oldu mevcut veri yapılarını kullandık (veri türleri `System.Drawing`).

İçin taşırken **Unified**, örneklerini değiştirmeniz gerekecektir `System.Drawing` ile bunların `CoreGraphics` aşağıdaki tabloda gösterildiği gibi ortaklarınıza:

|System.Drawing eski türü|Yeni veri türü CoreGraphics|Açıklama|
|--- |--- |--- |
|`RectangleF`|`CGRect`|Kayan ayrı tutma dikdörtgen bilgi noktası.|
|`SizeF`|`CGSize`|Kayan ayrı tutma noktasının boyutu bilgileri (genişlik, yükseklik)|
|`PointF`|`CGPoint`|Kayan nokta (X, Y) bilgi noktası tutar|

Eski veri türleri kullanılır float veri yapılarını öğelerini depolamak için yeni bir kullanan while `System.nfloat`.

## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar Arası Uygulamalarda Yerel Türlerle Çalışma](~/cross-platform/macios/native-types-cross-platform.md)
- [Klasik vs Unified API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
