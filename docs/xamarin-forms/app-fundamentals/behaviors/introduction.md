---
title: Davranışlar giriş
description: Davranışlar alt sınıfı için bunlara gerek kalmadan işlevsellik için kullanıcı arabirimi denetimleri eklemenize olanak sağlar. Bunun yerine, işlevselliği bir davranış sınıfta uygulanır ve bu denetimi bir parçası olarak ise denetimine bağlı. Bu makalede, davranışları tanıtılmaktadır.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 176f41d4b7349af2cf7cc49de8ba0789ad2f8c11
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995820"
---
# <a name="introduction-to-behaviors"></a>Davranışlar giriş

_Davranışlar alt sınıfı için bunlara gerek kalmadan işlevsellik için kullanıcı arabirimi denetimleri eklemenize olanak sağlar. Bunun yerine, işlevselliği bir davranış sınıfta uygulanır ve bu denetimi bir parçası olarak ise denetimine bağlı. Bu makalede, davranışları tanıtılmaktadır._

Davranışlar doğrudan API denetimi kısaca denetimine bağlı ve olması için yeniden kullanılması birden fazla uygulama arasında paketlendi, bu şekilde etkileşimde bulunduğundan genellikle arka plan, kod yazmanız gerekir kodu uygulamalarına olanak sağlar. Gibi çeşitli denetimleri işlevsellik sağlamak için kullanılabilir:

- Bir e-posta doğrulayıcıya ekleyerek bir [ `Entry` ](xref:Xamarin.Forms.Entry).
- Bir dokunun hareket tanıyıcı kullanarak derecelendirme denetimi oluşturma.
- Bir animasyon denetleme.
- Bir denetime efekt ekleme.

Davranışlar, daha gelişmiş senaryoları da etkinleştirin. Bağlamında *komut vermeye genel*, davranışları olan komut bir denetimi bağlama için kullanışlı bir yaklaşım. Ayrıca, bunlar komutları ile etkileşim kurmak için tasarlanmamıştır denetimleri komutlarını ilişkilendirmek için kullanılabilir. Örneğin, bunlar tetikleme bir olaya yanıt olarak bir komut çağırmak için kullanılabilir.

Xamarin.Forms davranışları iki farklı türlerde destekler:

- **Xamarin.Forms davranışları** – öğesinden türetilen sınıfları [ `Behavior` ](xref:Xamarin.Forms.Behavior) veya [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) sınıfı burada `T` denetimine türü davranışı uygulamanız gerekir. Xamarin.Forms davranışları hakkında daha fazla bilgi için bkz. [Xamarin.Forms davranışları](~/xamarin-forms/app-fundamentals/behaviors/creating.md) ve [yeniden kullanılabilir davranışlar](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Ekli davranışlar** – `static` sınıflarıyla bir veya daha fazla ekli özellikler. Ekli Davranışlar hakkında daha fazla bilgi için bkz: [ekli davranışlar](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Bu kılavuz, çünkü bunlar davranışı oluşturma için tercih edilen yaklaşım Xamarin.Forms davranışları odaklanır.



## <a name="related-links"></a>İlgili bağlantılar

- [Davranışı](xref:Xamarin.Forms.Behavior)
- [Davranış&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
