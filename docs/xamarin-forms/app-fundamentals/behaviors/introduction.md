---
title: Davranışları giriş
description: Davranışları alt kümesi için bunlara gerek kalmadan kullanıcı arabirimi denetimlerini işlevselliğini eklemenizi sağlar. Bunun yerine, işlevi bir davranış sınıfta uygulanır ve denetim parçası boşmuş gibi denetime bağlı. Bu makalede davranışları tanıtılmaktadır.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: dc6d8396c2908d251290e4540dbb3cec3344542f
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732794"
---
# <a name="introduction-to-behaviors"></a>Davranışları giriş

_Davranışları alt kümesi için bunlara gerek kalmadan kullanıcı arabirimi denetimlerini işlevselliğini eklemenizi sağlar. Bunun yerine, işlevi bir davranış sınıfta uygulanır ve denetim parçası boşmuş gibi denetime bağlı. Bu makalede davranışları tanıtılmaktadır._

Davranışları doğrudan yönelik olarak kısaca denetime bağlı ve olması yeniden kullanım için birden çok uygulama arasında paketlenmiş olduğunu şekilde denetiminde API ile etkileşim olduğundan, normal olarak arka plan kod, yazmanız gerekir kodu uygulamak etkinleştirin. Geniş denetimlerine, işlevselliğini sağlamak üzere kullanılabilir:

- Bir e-posta Doğrulayıcı ekleme bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/).
- Dokunun hareketi tanıyıcıyı derecelendirme denetimi oluşturuluyor.
- Bir animasyon denetleme.
- Efekt bir denetimine ekleme.

Davranışları daha Gelişmiş senaryolar da etkinleştirin. Bağlamında *kumanda*, davranışları olan denetim komut bağlamak için yararlı bir yaklaşım. Buna ek olarak, bunlar komutları ile etkileşim kurmak için tasarlanmamıştır denetimleri komutlarını ilişkilendirmek için kullanılabilir. Örneğin, bunlar tetikleme bir olaya yanıt olarak bir komut çağırmak için kullanılabilir.

Xamarin.Forms davranışları iki farklı stillerini destekler:

- **Xamarin.Forms davranışları** – öğesinden türetilen sınıflar [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) veya [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) sınıfı, burada `T` denetimine türü davranışı uygulamanız gerekir. Xamarin.Forms davranışları hakkında daha fazla bilgi için bkz: [Xamarin.Forms davranışları](~/xamarin-forms/app-fundamentals/behaviors/creating.md) ve [yeniden kullanılabilir davranışları](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Davranışları bağlı** – `static` bir veya daha fazla ekli özellikler sınıflarıyla. Ekli davranışları hakkında daha fazla bilgi için bkz: [bağlı davranışları](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Davranış yapımı için tercih edilen yaklaşım oldukları için bu kılavuzu Xamarin.Forms davranışlarını odaklanır.



## <a name="related-links"></a>İlgili bağlantılar

- [Davranışı](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Davranış&lt;T&gt;](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
