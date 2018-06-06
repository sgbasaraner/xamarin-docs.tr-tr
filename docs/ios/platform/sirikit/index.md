---
title: Xamarin.iOS SiriKit
description: Bu makalede SiriKit bir Xamarin.iOS uygulaması bir iOS cihazında Siri kullanarak kullanıcı tarafından erişilebilir hizmetleri sağlamak için nasıl kullanılacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 584b694c83d6c66d6e79e1030b2e682cefb64e6a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788058"
---
# <a name="sirikit-in-xamarinios"></a>Xamarin.iOS SiriKit

_Bu makalede SiriKit bir Xamarin.iOS uygulaması bir iOS cihazında Siri kullanarak kullanıcı tarafından erişilebilir hizmetleri sağlamak için nasıl kullanılacağı gösterilmektedir._

Yeni 10 iOS için uygulama uzantıları ve yeni kullanarak iOS cihazında Siri ve haritalar uygulama kullanarak kullanıcı tarafından erişilebilir hizmetleri sağlamak bir iOS uygulaması SiriKit sağlar **hedefleri** ve **hedefleri UI** çerçeveleri.

Siri çalışır kavramıyla **etki alanları**, grupları bilmeniz ilgili görevleri için Eylemler. Bir uygulama ile Siri sahip her etkileşim, bilinen bir hizmet etki alanlarından biri aşağıdaki gibi olmalıdır:

- Ses veya video çağırma.
- Bir kılma kayıt.
- Egzersizleriniz yönetme.
- İleti.
- Fotoğraf aranıyor.
- Gönderme veya ödemeler alma.

Kullanıcı, bir uygulama uzantının hizmetlerinden birini içeren Siri isteği yaptığında, SiriKit uzantısı gönderir bir **hedefi** kullanıcının isteği destekleyici verilerin tanımlayan nesne. Uygulama Uzantısı sonra uygun oluşturur **yanıt** için nesne verilen **hedefi**, uzantısı isteği nasıl işleyebilir ayrıntılı.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[SiriKit Kavramlarını Anlama](~/ios/platform/sirikit/understanding-sirikit.md)

Bu makalede, bir Xamarin.iOS uygulaması SiriKit ile çalışmak için gerekli olacak temel kavramları kapsar. Yeni kapsayan hedefleri ve hedefleri UI uzantı noktaları ve nasıl Siri uygulamayı açmak için uygulama ve kullanıcı sözlük ile çalışır.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[SiriKit Uygulama](~/ios/platform/sirikit/implementing-sirikit.md)

Bu makalede bir Xamarin.iOS uygulamaları SiriKit destek uygulamak için gerekli adımlar kapsanmaktadır. Geliştirici, başarılı bir şekilde gerçekleştirilebilmesi için gerekli olacak kavramlarını ele anahtarı olarak bir uygulama için SiriKit desteği eklemeyi denemeden önce yukarıda anlama SiriKit Kavramları kılavuzu okumanız gerekir.





## <a name="related-links"></a>İlgili bağlantılar

- [ElizaChat örnek](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Hedefleri Framework başvurusu](https://developer.apple.com/reference/intents)
- [Hedefleri UI Framework başvurusu](https://developer.apple.com/reference/intentsui)
