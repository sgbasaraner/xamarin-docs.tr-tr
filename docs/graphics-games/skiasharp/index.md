---
title: "2B çizim"
description: "Çapraz Platform SkiaSharp ile 2B çizim"
ms.topic: article
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: ee0625f22062fef3c27a697ce33488274abc24d9
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="2d-drawing"></a>2B çizim

SkiaSharp 2B grafik yapmak için güçlü bir C# API sağlar. Tarafından sağlanmıştır [Google Skia Kitaplığı](http://skia.org), Google Chrome, Firefox ve Android'ın grafik yığınları'nın temelini oluşturan aynı kitaplığı.

[![](images/ide-sml.png "2B grafik yapmak için güçlü bir C# API SkiaSharp sağlar")](images/ide.png#lightbox)

SkiaSharp taşınabilir bir kitaplığıdır ve rahat olarak gelir bir [platformlar arası NuGet paketi](https://www.nuget.org/packages/SkiaSharp)ve kutunun dışında aşağıdaki platformları destekler: macOS, Xamarin.Android, Xamarin.iOS ve Windows Masaüstü.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[SkiaSharp giriş](~/graphics-games/skiasharp/introduction.md)

Temel kavramlar SkiaSharp ve örnek genel bakış grafikler, metin, bit eşlemler işlemek için kod ve resim filtreleri kullanın.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Xamarin.Forms SkiaSharp öğreticileri](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Xamarin.Forms içinde işlemek platformu grafik çapraz çalışmak öğrenin:

- [Çizim temelleri](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Basit bir daire çizme](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Xamarin.Forms ile Tümleştirme](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Piksel ve CİHAZDAN bağımsız birimler](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Temel animasyon](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Metin ve grafik tümleştirme](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Bit eşlem temelleri](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Satırları ve yolları](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Satırları ve vuruş büyük harfler](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Yol temelleri](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Yolun dolgu türleri](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Kullansa ve parametrik denklemini](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Nokta ve tire](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Parmak Boyama](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Çevir Dönüştür](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Ölçek dönüştürme](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Döndürme dönüşümü](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Eğme dönüştürmesi](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Matris dönüşümleri](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Dokunma işlemeleri](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Olmayan afin dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3B Döndürme](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Eğriler ve yolları](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Yay Çizmenin Üç Yolu](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Üç Tür Bézier Eğrisi](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [SVG Yol Verileri](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Yollar ve Bölgeler ile Kırpma](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Yol Etkileri](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Yollar ve Metin](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Yol Bilgileri ve Numaralandırması](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Platforma Özgü Notlar](~/graphics-games/skiasharp/platform.md)

Bu sayfa, iOS, Android, macOS ve Windows dahil farklı platformlarda SkiaSharp kurulum yönergelerini açıklar.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[API belgeleri](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Tarayabileceğiniz [API belgelerine](https://developer.xamarin.com/api/namespace/SkiaSharp/) SkiaSharp web sitemizde için.

## <a name="work-in-progress"></a>İş sürüyor

SkiaSharp bir iş biz bizim toplulukla paylaştığı sürdürülüyor. Biz Skia API önemli kısımlarını bağlı olsa da, kadar iş tamamlandı olarak kalır. Tarafından Skia ortaya kararlı C API kullanıyoruz ve katkıda bulunan tam kapsamı API'lerine sağlamak için Skia C bağlamalarını bizim çalışmaya devam etmek için bizim planınızdır.

Bizim bağlama çaba Kılavuzu, lütfen ve önerileriniz GitHub deposunu sorunları bırakın yardımcı olmak için [http://github.com/mono/SkiaSharp](http://github.com/mono/SkiaSharp).
