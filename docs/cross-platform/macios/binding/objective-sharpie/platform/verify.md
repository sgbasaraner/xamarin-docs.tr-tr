---
title: Amaç Sharpie öznitelikleri doğrulayın
description: Bu belgenin amacı Sharpie tarafından oluşturulan [doğrulayın] özniteliğini açıklar. [Doğrulayın] özniteliği burada bunlar el ile hedefi Sharpie'nın çıkış doğrulamalısınız geliştiricilerine vurgular.
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 96e5bafc14c2d3aba03ccc137151a83ee8afeef9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780717"
---
# <a name="objective-sharpie-verify-attributes"></a>Amaç Sharpie öznitelikleri doğrulayın

Hedefi Sharpie tarafından üretilen bağlamaları ile Açıklama, genellikle bulacaksınız `[Verify]` özniteliği. Bu öznitelikler, gerektiğini belirtmek _doğrulayın_ hedefi Sharpie (, ilişkili bildiriminin üstüne bir yorum sağlanacak) özgün C/Objective-C bildirimine bağlamayla karşılaştırarak doğru olanı vermedi.

Doğrulama için önerilir _tüm_ bağlı bildirimleri, ancak büyük olasılıkla _gerekli_ ile bildirimleri açıklama için `[Verify]` özniteliği. Çoğu durumda olmadığı için yeterli meta verileri en iyi bir bağlama üretmek nasıl anlamak için özgün yerel kaynak kodunda budur. Belge veya kod açıklamaları en iyi bağlama kararı vermeniz başlık dosyalarının içindeki başvuru gerekebilir.

Bağlama olduğunu doğruladıktan sonra düzeltin veya bu doğru olması için sabit _kaldırmak_ `[Verify]` bağlama özniteliği.

> [!IMPORTANT]
> `[Verify]` böylece bağlama doğrulamak için zorlanır öznitelikleri kasıtlı olarak C# derleme hatalara neden olabilir. Kaldırmanız gerekir `[Verify]` özniteliği olan gözden (ve muhtemelen düzeltildi,) kodu.

## <a name="verify-hints-reference"></a>İpuçları başvurusu

Öznitelik sağlanan ipucu bağımsız değişkeni aşağıdaki belgeleriyle başvurulan çapraz olabilir. Belge herhangi üretilen `[Verify]` öznitelikleri bağlama tamamlandıktan sonra konsolda sağlanacaktır.

|`[Verify]` İpucu|Açıklama|
|---|---|
|InferredFromPreceedingTypedef|Bu bildirim adını ortak convention öğesinden tarafından çıkarımı yapılan hemen önce gelen `typedef` özgün yerel kaynak kodunda. Çıkarsanan adı bu kuralı belirsiz olduğu gibi doğru olduğundan emin olun.|
|ConstantsInterfaceAssociation|Hangi Objective-C arabirimiyle extern değişken bildirimi ilişkilendirilebilir belirlemek için kanıtı yolu yoktur. Bu örnekleri bağlı olarak `[Field]` büyük olasılıkla 'Sabitleri' ortadan daha sezgisel bir API üretmek için somut arabirimi yakın-tarafından kısmi bir arabirimine özelliklerinde arabirimi tamamen.|
|MethodToProperty|Objective-C yöntemi, hiçbir parametre alan ve değer (void olmayan iade) döndürme gibi kuralı nedeniyle C# özelliği olarak bağlıydı. Genellikle yöntemleri bu gibi özellikleri olarak daha Hoş görünmesi bir API ortaya çıkarma bağlanmalıdır, ancak bağlama gerçekte bir yöntemi olması gerekir ve yanlış pozitif sonuç bazen oluşabilir.|
|StronglyTypedNSArray|Yerel `NSArray*` olarak bağlı `NSObject[]`. Daha güçlü API belgelerine (örn. üstbilgi dosyası açıklamaları) aracılığıyla ayarlanabilir beklentilerini göre bağlamadaki dizi türü mümkün olabilir ya da test aracılığıyla dizi içeriklerini İnceleme. Örneğin, bir NSArray içeren yalnızca NSNumber * * instancescan bağlı olarak `NSNumber[]` yerine `NSObject[]`.|

İpucunu kullanarak belgelerine de hızlı bir şekilde alabilir `sharpie verify-docs` aracı, örneğin:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

