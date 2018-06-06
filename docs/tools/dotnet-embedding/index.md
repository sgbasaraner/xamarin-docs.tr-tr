---
title: .NET Embedding
description: '.NET katıştırma varolan .NET kodunuzun (C#, F # ve diğerleri) diğer programlama dilinde yazılan kod tarafından tüketilmesi sağlar.'
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 16f59498a49d10a43e04989136d8835bf78bd89d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793127"
---
# <a name="net-embedding"></a>.NET Embedding

![Önizleme](~/media/shared/preview.png)

.NET katıştırma (C#, F # ve diğerleri) diğer programlama dilleri ve çeşitli farklı ortamlarda kullanılacak, varolan .NET kodu sağlar.

Bu, var olan iOS uygulamanızdan kullanmak istediğiniz bir .NET kitaplığı varsa, bunu yapabilirsiniz, anlamına gelir.   Veya bir yerel C++ kitaplıkla bağlanmak istiyorsanız, bunu da yapabilirsiniz.   Veya Java .NET kodundan kullanabilir.

.NET katıştırma temel [Embeddinator 4000](https://github.com/mono/Embeddinator-4000) açık kaynaklı proje.

## <a name="environments-and-languages"></a>Ortamları ve diller

Her ikisi de kullanacağı ortamı, hem de tüketiyor mu dilini kullanan bir araçtır.   .NET katıştırma statik olarak .NET kodunuzu iOS kullanılabilir yerel kod içine derleyin şekilde Örneğin, iOS platformu tam zamanında (JIT) derleme izin vermiyor.  Bu ortamlarda, biz JIT derleme iptal et ve diğer ortamlara JIT derleme izin verin.

Çeşitli dil tüketicileri destekler, böylece bu .NET kodu hedef dilde kullanılan deyimsel kod olarak ortaya çıkarır.   Bu, şu anda desteklenen dillerin listesi verilmiştir:

- [**Objective-C** ](objective-c/index.md) – kullanılan deyimsel Objective-C API'ları için .NET eşleme
- [**Java** ](android/index.md) – kullanılan deyimsel Java API'ları için .NET eşleme
- [**C** ](get-started/c.md) – nesne yönelimli C API'leri gibi .NET eşleme

Daha fazla dil daha sonra gelecektir.

## <a name="getting-started"></a>Başlarken

Başlamak için kılavuzlarımız şu anda desteklenen dillerin her birini denetleyin:

- [**Objective-C** ](get-started/objective-c/index.md) – macOS ve iOS kapsar.
- [**Java** ](get-started/java/index.md) – macOS ve Android kapsar.
- [**C** ](get-started/c.md) – C dil Masaüstü platformlarda kapsar

## <a name="related-links"></a>İlgili bağlantılar

- [Github'da Embeddinator 4000](https://github.com/mono/Embeddinator-4000)
