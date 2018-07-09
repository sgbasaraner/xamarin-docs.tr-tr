---
title: Amaç Sharpie öznitelikleri doğrulayın
description: Bu belgenin amacı Sharpie tarafından oluşturulan [doğrulama] özniteliğini açıklar. [Doğrulama] özniteliği nerede bunlar el ile hedefi Sharpie'nın çıktı doğrulamalıdır geliştiricilerine vurgular.
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4bca896afb4dfc96fd6c1d7cdf489feb6a879e31
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855035"
---
# <a name="objective-sharpie-verify-attributes"></a>Amaç Sharpie öznitelikleri doğrulayın

Hedefi Sharpie tarafından üretilen bağlamaları ile açıklanan, genellikle bulabilirsiniz `[Verify]` özniteliği. Bu öznitelikler, gerektiğini belirtmek _doğrulayın_ hedefi Sharpie bağlama (Bu bir yorum ilişkili bildiriminin üstüne sağlanacaktır) özgün C/Objective-C bildirimi ile karşılaştırarak en doğru şey olmadı.

Doğrulama için önerilir _tüm_ bildirimleri bağlı, ancak büyük olasılıkla _gerekli_ bildirimleri ile ek açıklama için `[Verify]` özniteliği. Çoğu durumda olmadığı için yeterli sayıda meta veri en iyi bir bağlama oluşturmak nasıl çıkarsamak için özgün yerel kaynak kodunda budur. Belge veya kod açıklamaları en iyi bağlama kararı vermek için üst bilgi dosyaları içindeki başvurmanız gerekir.

Bağlama olduğunu doğruladıktan sonra düzeltin veya doğru olması için sabit _Kaldır_ `[Verify]` bağlama özniteliği.

> [!IMPORTANT]
> `[Verify]` böylece bağlama doğrulamak için zorunlu öznitelikler kasıtlı olarak C# derleme hatalarına neden. Kaldırmalısınız `[Verify]` özniteliği olan gözden (ve muhtemelen düzeltildi,) kod.

## <a name="verify-hints-reference"></a>İpuçları başvurusu

Aşağıdaki belgeleri ile başvurulan özniteliğe sağlanan ipucu bağımsız değişken çapraz olabilir. Belgeleri herhangi üretilen `[Verify]` öznitelikleri bağlama tamamlandıktan sonra konsolda sağlanacaktır.

|`[Verify]` İpucu|Açıklama|
|---|---|
|InferredFromPreceedingTypedef|Bu bildirim adını genel kuralı tarafından ortaya çıkan hemen önceki `typedef` özgün yerel kaynak kodunda. Çıkarsanan adı bu kuralı belirsiz olduğundan doğru olduğunu doğrulayın.|
|ConstantsInterfaceAssociation|Hangi Objective-C arabirimiyle extern değişken bildirimi ilişkilendirilebilir belirlemek için kanıtı yolu yoktur. Bu örnekleri bağlı olarak `[Field]` büyük olasılıkla 'Sabitleri' ortadan kaldırır, daha sezgisel bir API oluşturmak için somut arabirimi yakın-tarafından kısmi bir arabirimine özelliklerinde arabirimi tamamen.|
|MethodToProperty|Objective-C yöntemi gibi hiçbir parametre alan ve döndüren bir değer (void olmayan dönüş) kuralı nedeniyle bir C# özelliği olarak bağlı değildi. Genellikle bunlar gibi yöntemler, özellikler olarak daha Hoş görünmesi bir API yüzey bağlanmalıdır, ancak bazen yanlış pozitifleri ortaya çıkabilir ve bağlama aslında bir yöntemi olması gerekir.|
|StronglyTypedNSArray|Yerel `NSArray*` olarak bağlı `NSObject[]`. Daha güçlü API belgelerini (örn açıklamaları üstbilgi dosyasında) aracılığıyla ayarlanan beklentileri göre bağlamasında dizi türüne mümkün olabilir veya test yoluyla dizinin içeriklerini İnceleme. Örneğin, bir NSArray içeren yalnızca NSNumber * * instancescan bağlı olarak `NSNumber[]` yerine `NSObject[]`.|

Hızla bir ipucu kullanmaya yönelik belgeler de alabilir `sharpie verify-docs` aracı, örneğin:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
