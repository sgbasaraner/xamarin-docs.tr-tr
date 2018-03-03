---
title: "Hızlı Oluşturucu"
description: "Bu makalede, sonuçta elde edilen yerel denetim hiyerarşisi düzleştirme tarafından Enflasyon ve Android Xamarin.Forms denetiminde işleme maliyetlerini azaltmak hızlı Oluşturucu tanıtılır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 92a11ebe983840270d3679fd11f5faa0b8222cfe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="fast-renderers"></a>Hızlı Oluşturucu

_Bu makalede, sonuçta elde edilen yerel denetim hiyerarşisi düzleştirme tarafından Enflasyon ve Android Xamarin.Forms denetiminde işleme maliyetlerini azaltmak hızlı Oluşturucu tanıtılır._

Geleneksel olarak, özgün denetim Oluşturucu android'de çoğunu iki görünümde oluşur:

- Yerel bir denetim, gibi bir `Button` veya `TextView`.
- Bir kapsayıcı `ViewGroup` bazı Düzen iş, hareketi işleme ve diğer görevleri gerçekleştiren.

Ancak, daha fazla bellek ve daha fazlasını ekranda işlemek için işleme gerektiren daha karmaşık bir görsel ağaç sonuçları her mantıksal denetim için iki görünüm oluşturulur, bu yaklaşımın bir performans uygulanır vardır.

Hızlı Oluşturucu, tek bir görünümde Enflasyon ve Xamarin.Forms Denetim işleme maliyetlerini azaltabilirsiniz. Bu nedenle, iki görünümü oluşturma ve bunları görünüm ağacına ekleyerek yerine tek bir oluşturulur. Bu sırayla daha az karmaşık bir görünüm ağacı anlamına gelir, daha az nesne oluşturarak ve (ve ayrıca daha az atık toplama duraklatır sonuçlanır) bellek kullanımını daha az performansı artırır.

Hızlı Oluşturucu aşağıdaki denetimleri Xamarin.Forms 2.4 için Android üzerinde kullanılabilir:

- [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)
- [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)
- [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)

İşlevsel olarak, bu hızlı Oluşturucu özgün oluşturuculara hiç farklı değildir. Ancak, bunlar şu anda Deneysel ve kod, aşağıdaki satırı ekleyerek yalnızca kullanılabilir, `MainActivity` çağırmadan önce sınıfın `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> **Not**: hızlı Oluşturucu yalnızca uygulama uyumluluğu Android arka ucuna geçerli nedenle bu ayarı öncesi uygulama uyumluluğu etkinliklerini sayılacak.

Performans iyileştirmeleri düzeni karmaşıklığına bağlı olarak, her bir uygulama için değişir. Örneğin, x2 performans geliştirmeleri ile kaydırma sırasında olası bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) burada her satır hücrelerde yapılan hızlı Oluşturucu kullanan denetimlerin, veri satırı binlerce içeren hangi sonuçlanıyor görünür şekilde daha yumuşak kaydırma.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
