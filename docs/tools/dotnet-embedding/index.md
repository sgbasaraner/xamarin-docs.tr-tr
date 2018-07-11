---
title: .NET Embedding
description: '.NET katıştırma (C#, F # ve diğerleri) diğer programlama dilinde yazılmış kod tarafından tüketilmesi mevcut .NET kod sağlar.'
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 16f59498a49d10a43e04989136d8835bf78bd89d
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830406"
---
# <a name="net-embedding"></a>.NET Embedding

![Önizleme](~/media/shared/preview.png)

.NET katıştırma (C#, F # ve diğerleri) diğer programlama dilleri ve çeşitli farklı ortamlarda kullanılacak mevcut .NET kod sağlar.

Bu, var olan bir iOS uygulamasından kullanmak istediğiniz bir .NET kitaplığı varsa, bunu yapabilirsiniz, anlamına gelir.   Veya yerel bir C++ kitaplığı ile bağlantı istiyorsanız, bunu da yapabilirsiniz.   Veya Java .NET kodundan kullanamıyor.

.NET ekleme temel [Embeddinator 4000](https://github.com/mono/Embeddinator-4000) açık kaynak bir projedir.

## <a name="environments-and-languages"></a>Ortamlar ve diller

Her ikisi de bu ortam, hem de bunu kullanacaktır dil kullanan bir araçtır.   .NET ekleme statik olarak .NET kodunuzu yerel koda iOS kullanılabilir derleyeceği şekilde gibi iOS platform just-ın-time (JIT) derleme izin vermez.  Diğer ortamlarda JIT derlemesi izin ve bu ortamlarda, biz JIT derleme için iyileştirilmiş.

Hedef dilde kullanılan deyimsel kod olarak .NET kodu yüzeyler için çeşitli dil tüketicileri destekler.   Bu, şu anda desteklenen dillerin listesi verilmiştir:

- [**Objective-C** ](objective-c/index.md) – .NET deyimsel Objective-C API'ler için eşleme
- [**Java** ](android/index.md) – deyimsel Java API'ları için .NET eşleme
- [**C** ](get-started/c.md) – C API'leri gibi nesne yönelimli .NET eşleme

Daha fazla dil daha sonra gelecektir.

## <a name="getting-started"></a>Başlarken

Başlamak için her biri şu anda desteklenen diller için Kılavuzlarımızı birini denetleyin:

- [**Objective-C** ](get-started/objective-c/index.md) – macOS ve iOS kapsar
- [**Java** ](get-started/java/index.md) – macOS ve Android kapsar
- [**C** ](get-started/c.md) – C dili Masaüstü platformlarda kapsar

## <a name="related-links"></a>İlgili bağlantılar

- [Github'da Embeddinator 4000](https://github.com/mono/Embeddinator-4000)
