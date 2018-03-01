---
title: "Yerel türler"
ms.topic: article
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: b78ade19efed92ab3b2d8ba790f2d7334472bab4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="native-types"></a>Yerel türler

Fark özünde Mac ve iOS API'leri her zaman 32 bit platformlarda ve 64 bit 64 bit platformlarda 32 bit bir mimariye özgü veri türlerini kullanın.

Örneğin, Objective-C eşlemeleri `NSInteger` veri türü için `int32_t` 32 bit sistemler ve çok `int64_t` 64 bit sistemler üzerinde.

Bu davranış, birleştirilmiş API'mize eşleşecek şekilde biz önceki kullanımlarını değiştirdiğiniz `int` (.NET içinde tanımlanan her zaman olduğu `System.Int32`) için yeni bir veri türü: `System.nint`.  Yerel tamsayı platformunun türü için "n" / anlamı "yerel" düşünebilirsiniz.

Bu yeni veri türleri ile aynı kaynak kodu, 32 bit, 32 bit ve 64 bit veya 64 bit, Derleme bayrakları bağlı olarak derlenir.

## <a name="new-data-types"></a>Yeni veri türleri

Aşağıdaki tabloda, bu yeni 32/64 bit world eşleşecek şekilde bizim veri türleri değişiklikleri gösterir:

<table>
        <tr>
            <th>Yerel tür</th>
            <th>32-bit yedekleme türü</th> 
            <th>64-bit yedekleme türü</th>
        </tr>
        <tr>
            <td><code>System.nint</code></td>
        <td><code>System.Int32</code> (<code>int</code>)</td>
        <td><code>System.Int64</code> (<code>long</code>)</td>
        </tr>
        <tr>
            <td><code>System.nuint</code></td>
        <td><code>System.UInt32</code> (<code>uint</code>)</td>
        <td><code>System.UInt64</code> (<code>ulong</code>)</td>
        </tr>
        <tr>
            <td><code>System.nfloat</code></td>
        <td><code>System.Single</code> (<code>float</code>)</td>
        <td><code>System.Double</code> (<code>double</code>)</td>
        </tr>
    </table>

Fazla veya az bugün görünür şekilde aramak, C# kodu izin vermek için bu adları seçtik.

### <a name="implicit-and-explicit-conversions"></a>Örtük ve Açık Dönüştürmeler

Yeni veri türlerinin tasarım 32 veya 64 bit depolama konak platorm ve derleme ayarları bağlı olarak doğal olarak kullanılacak tek bir C# kaynak dosyası sağlamak için tasarlanmıştır.

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

<table>
        <tr>
            <th>System.Drawing eski türü</th>
            <th>Yeni veri türü CoreGraphics</th> 
            <th>Açıklama</th>
        </tr>
        <tr>
        <td><code>RectangleF</code></td>
        <td><code>CGRect</code></td>
        <td>Kayan ayrı tutma dikdörtgen bilgi noktası.  </td>
        </tr>
        <tr>
        <td><code>SizeF</code></td>
        <td><code>CGSize</code></td>
        <td>Kayan ayrı tutma noktasının boyutu bilgileri (genişlik, yükseklik)</td>
        </tr>
        <tr>
        <td><code>PointF</code></td>
        <td><code>CGPoint</code></td>
        <td>Kayan nokta (X, Y) bilgi noktası tutar</td>
        </tr>
    </table>

Eski veri türleri kullanılır float veri yapılarını öğelerini depolamak için yeni bir kullanan while `System.nfloat`.

## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası uygulamalar içindeki yerel türler ile çalışma](~/cross-platform/macios/native-types-cross-platform.md)
- [Klasik vs Unified API farklılıkları](http://developer.xamarin.comhttps://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
