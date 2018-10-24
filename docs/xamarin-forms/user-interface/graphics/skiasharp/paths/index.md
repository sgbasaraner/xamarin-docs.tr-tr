---
title: SkiaSharp satırları ve yolları
description: Bu makalede SkiaSharp satırları ve grafik yolları Xamarin.Forms uygulamalarında çizmek için nasıl kullanılacağını açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 3f2597c67459e407ac066ee19d54d134d60f3076
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615141"
---
# <a name="skiasharp-lines-and-paths"></a>SkiaSharp satırları ve yolları

_SkiaSharp satırları ve grafik yolları çizmek için kullanın_

[Önceki bölümde](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) , gösterilen SkiaSharp `SKCanvas` sınıfı, daire, elips, dikdörtgenler, Yuvarlatılmış Dikdörtgen, metin ve bit eşlemler çizmek için çeşitli yöntemler içerir. Bu bölümde ve sonraki bölümlerde, çeşitli sınıfları oluşturma ve işleme bağlı kapsayan *grafik yolları*.

Grafik çizim çizgiler ve eğriler SkiaSharp en genelleştirilmiş yaklaşımı yoludur. Bu bölümde incelemektedir bir [ `SKPath` ](xref:SkiaSharp.SKPath) nesne düz çizgi çizmek için ve küçük düz çizgiler koleksiyonu kullanmak için (adlı bir *çoklu çizgi*) algorithmically tanımlayabilirsiniz eğriler çizmek için. Bir sonraki bölüm [ **SkiaSharp eğrileri ve yolları** ](../curves/index.md) eğrileri tarafından desteklenen çeşitli tür anlatılmaktadır `SKPath`.

Bu bölümdeki tüm örnek programlar başlığının altında görünür **satırları ve yolları** giriş sayfasında [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programı ve [ **Yolları** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) Bu çözüm klasörü.

## <a name="lines-and-stroke-capslinesmd"></a>[Satırlar ve Vuruş Uçları](lines.md)

SkiaSharp farklı vuruş uçlarıyla çizgi çizmek için kullanmayı öğrenin.

## <a name="path-basicspathsmd"></a>[Temel Yol Bilgileri](paths.md)

SkiaSharp keşfedin `SKPath` çizgiler ve eğrilerle birleştirme nesne.

## <a name="the-path-fill-typesfill-typesmd"></a>[Yol Dolgusu Türleri](fill-types.md)

SkiaSharp yol dolgusu türleri ile olası farklı etkileri keşfedin.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Çoklu Çizgiler ve Parametreli Denklemler](polylines.md)

SkiaSharp ile parametreli denklemler tanımlayabilirsiniz herhangi bir satır işlemek için kullanın.

## <a name="dots-and-dashesdotsmd"></a>[Noktalar ve Tireler](dots.md)

İçinde SkiaSharp noktalı ve kesikli çizgi çizme, ayrıntılı olarak incelenmektedir Yöneticisi.

## <a name="finger-paintingfinger-paintmd"></a>[Parmak Boyama](finger-paint.md)

Boyama tuval üzerinde için parmaklarınızın kullanın.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
