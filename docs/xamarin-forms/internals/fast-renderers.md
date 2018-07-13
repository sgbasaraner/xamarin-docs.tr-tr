---
title: Xamarin.Forms hızlı Oluşturucu
description: Bu makalede, sonuçta elde edilen yerel denetim hiyerarşisi düzleştirme tarafından Enflasyon ve bir Xamarin.Forms denetimi android'de işleme maliyetlerini azaltmak hızlı oluşturucular tanıtılmaktadır.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: e4b060c703077e140e0f0d2f8c4c2b824c890e8d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997127"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms hızlı Oluşturucu

_Bu makalede, sonuçta elde edilen yerel denetim hiyerarşisi düzleştirme tarafından Enflasyon ve bir Xamarin.Forms denetimi android'de işleme maliyetlerini azaltmak hızlı oluşturucular tanıtılmaktadır._

Geleneksel olarak, android'de özgün denetim Oluşturucu çoğunu iki görünüm oluşur:

- Gibi bir yerel denetleyen bir `Button` veya `TextView`.
- Bir kapsayıcı `ViewGroup` , bazı Düzen iş, hareket işleme ve diğer görevleri işler.

Ancak, iki görünüm, daha fazla bellek ve daha fazlasını ekranda işlenecek işleme gerektiren daha karmaşık bir görsel ağaç sonuçları her mantıksal denetim için oluşturulur, bu yaklaşım bir performans olduğu çıkarımında sahiptir.

Hızlı oluşturucular tek bir görünümde Enflasyon ve Xamarin.Forms Denetim işleme maliyetlerini azaltın. Bu nedenle, iki görünüm oluşturma ve bunları görünümü ağacına ekleyerek yerine yalnızca bir tane oluşturulur. Sırayla daha az karmaşık bir görünüm ağaç anlamına gelir, daha az nesne oluşturarak ve (Bu da daha az çöp toplama duraklamaları sonuçlanır) bellek kullanımını daha az performansını artırır.

Hızlı oluşturucular Xamarin.Forms 2.4 aşağıdaki denetimleri android'de kullanılabilir:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)

İşlevsel olarak, bu hızlı oluşturucular özgün oluşturuculara farklı. Ancak, bunlar şu anda Deneysel ve yalnızca aşağıdaki kod satırını ekleyerek kullanılabilir, `MainActivity` sınıfı çağırmadan önce `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Hızlı Oluşturucular, yalnızca öncesi uygulama compat etkinliklerde bu ayarı göz ardı edilir şekilde uygulama compat Android arka için geçerlidir.

Performans iyileştirmeleri düzenini karmaşıklığına olarak, her bir uygulama için farklılık gösterir. Örneğin, performansı geliştirmelerinin x2 aracılığıyla kaydırdığınızda olası bir [ `ListView` ](xref:Xamarin.Forms.ListView) gösteren binlerce satır verileri, burada her bir satırdaki hücreleri hızlı oluşturucular kullanan denetimleri yapılır, içeren, sonuçlanıyor görünüşte daha yumuşak kaydırma.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
