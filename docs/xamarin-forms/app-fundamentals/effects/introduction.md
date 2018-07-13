---
title: Etkileri giriş
description: Etkileri izin özelleştirilmek üzere her platformda yerel denetimleri ve genellikle küçük stil değişiklikler için kullanılır. Bu makalede etkileri tanıtılmaktadır, etkiler ve özel Oluşturucu arasındaki sınırı özetler ve PlatformEffect sınıfı tanımlar.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d3fa958e999a10832d5fa15e4190077955b0e6df
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997391"
---
# <a name="introduction-to-effects"></a>Etkileri giriş

_Etkileri izin özelleştirilmek üzere her platformda yerel denetimleri ve genellikle küçük stil değişiklikler için kullanılır. Bu makalede etkileri tanıtılmaktadır, etkiler ve özel Oluşturucu arasındaki sınırı özetler ve PlatformEffect sınıfı tanımlar._

Xamarin.Forms [sayfaları, düzenler ve denetimleri](~/xamarin-forms/user-interface/controls/index.md) platformlar arası mobil kullanıcı arabirimlerini tanımlamak için ortak bir API sunar. Her sayfa, Düzen ve denetimi farklı kullanarak her platformunda işlenen bir `Renderer` sırayla (Xamarin.Forms gösterimi için karşılık gelen), yerel bir denetim oluşturan sınıf ekranda düzenler ve paylaşılan içinde belirtilen davranışı ekler kodu.

Geliştiriciler, kendi özel uygulayabilirsiniz `Renderer` sınıflar görünümünü ve davranışını özelleştirin. Ancak, bir basit denetimi özelleştirme gerçekleştirmek için bir özel Oluşturucu sınıf uygulama genellikle ağır yanıt olur. Etkileri, yerel denetimleri daha kolay özelleştirilmek üzere her platformda izin vererek, bu işlemi basitleştireceksiniz.

Etkileri platforma özgü projelerinde sınıflara tarafından oluşturulan `PlatformEffect` denetimi ve etkileri tüketilen uygun bir denetime bir Xamarin.Forms .NET Standard kitaplığı veya paylaşılan kitaplık projesi ekleyerek.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Neden bir efekti özel Oluşturucu kullanmalıyım?

Etkileri denetimi özelleştirme basitleştirin, yeniden kullanılabilir ve daha fazla yeniden artırmak için parametreli olabilir.

Bir efekti ile sağlanabilir herhangi bir şey de özel Oluşturucu ile gerçekleştirilebilir. Özel oluşturucular, ancak daha fazla esneklik ve özelleştirme etkileri daha sunar. Aşağıdaki yönergeler özel bir oluşturucu efekt seçebileceğiniz durumlarda listeleyin:

- Ne zaman bir platforma özel denetimin özelliklerini değiştirmek istediğiniz sonucu elde edeceğini efekt önerilir.
- Özel oluşturucu, platforma özel denetimin yöntemleri geçersiz kılmak için ihtiyaç olduğunda gereklidir.
- Özel oluşturucu, bir Xamarin.Forms denetimi uygular platforma özel denetimi değiştirmek üzere ihtiyaç olduğunda gereklidir.

## <a name="subclassing-the-platformeffect-class"></a>Sınıflara PlatformEffect sınıfı

Ad alanı için aşağıdaki tabloda `PlatformEffect` her platform ve türlerini özelliklerini sınıfı:

|Platform|Ad Alanı|Kapsayıcı|Denetim|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|Uıview|Uıview|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|Görüntüle|
|Evrensel Windows Platformu (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

Her platforma özgü `PlatformEffect` sınıfı aşağıdaki özellikleri sunar:

- `Container` – düzenini uygulamak için kullanılan platforma özgü denetimine başvurur.
- `Control` – Xamarin.Forms denetimi uygulamak için kullanılan platforma özgü denetimine başvurur.
- `Element` – işlenen Xamarin.Forms denetimine başvurur.

Etkileri kapsayıcı, denetimi veya herhangi bir öğeye iliştirilmiş olduğundan bağlandıkları öğesi hakkında bilgi türü yok. Bu nedenle, efekt desteği olmayan bir öğe eklendiğinde, düzgün bir şekilde veya bir özel durum oluşturması gerekir. Ancak, `Container`, `Control`, ve `Element` özellikleri, uygulama türüne dönüştürülür. Bunlar hakkında daha fazla bilgi Bkz türleri için [oluşturucu temel sınıfları ve yerel denetimleri](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Her platforma özgü `PlatformEffect` sınıfı geçersiz kılınmalıdır bir efekti uygulamak için aşağıdaki yöntemleri sunar:

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) – Efekt Xamarin.Forms denetime eklendiğinde çağırılır. Her platform özel efekt sınıfındaki bu yöntemi geçersiz kılınan bir sürümünü özelleştirme etkisi belirtilen Xamarin.Forms denetime uygulanamaz durumunda özel durum işleme ile birlikte denetiminin gerçekleştirmeyi yerdir.
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) – bir Xamarin.Forms denetiminden efekt ayrıldığında çağrılır. Her platforma özel efekt sınıfındaki bu yöntemi geçersiz kılınan bir sürümünü, bir olay işleyicisi XML'deki kaydetme gibi herhangi bir etkisi temizleme işlemini gerçekleştirmek için yerdir.

Ayrıca, `PlatformEffect` sunan [ `OnElementPropertyChanged` ](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs)) yöntemi de geçersiz kılınabilir. Bu yöntem, öğenin bir özellik değiştiğinde çağrılır. Her platforma özel efekt sınıfındaki bu yöntemi geçersiz kılınan bir sürümünü Xamarin.Forms denetimi bağlanılabilir özellik değişikliklerine yanıt verme yerdir. Bu geçersiz kılma birden çok kez çağrılabilir olarak değişen bir özellik için bir onay her zaman yapılmalıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
