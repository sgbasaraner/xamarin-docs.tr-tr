---
title: SkiaSharp ile 2B çizim
description: Bu belge ile SkiaSharp çizimi platformlar arası 2B genel bir bakış sağlar. Bu, SkiaSharp tanımlayan çeşitli kılavuzları ve çeşitli API'lerini için bağlar.
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 0c8cbc14308c8c4131e5aaa2bcc0ddfa798af610
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130926"
---
# <a name="2d-drawing-with-skiasharp"></a>SkiaSharp ile 2B çizim

SkiaSharp 2B grafikler yapmak için güçlü bir C# API sağlar. Tarafından desteklenen [Google'nın Skia Kitaplığı](http://skia.org), Google Chrome, Firefox ve Android grafik yığınları güç veren aynı kitaplığı.

[![](images/ide-sml.png "SkiaSharp 2B grafikler yapmak için güçlü bir C# API sağlar.")](images/ide.png#lightbox)

SkiaSharp taşınabilir kitaplık ve ücretsiz olarak bir kolayca gelir bir [platformlar arası NuGet paketini](https://www.nuget.org/packages/SkiaSharp)ve hazır aşağıdaki platformları destekler: macOS, Windows Masaüstü Xamarin.Android ve Xamarin.iOS.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[SkiaSharp giriş](~/graphics-games/skiasharp/introduction.md)

SkiaSharp örnek ve temel kavramlar hakkında genel bakış, grafik, metin, bit eşlemler işlemek için kod ve resmi filtreleri kullanın.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Xamarin.Forms için SkiaSharp öğreticiler](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Xamarin.Forms içinde işleme platformu grafik çapraz ile çalışmayı öğrenin:

- [Temel çizim](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Basit bir daire çizme](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Xamarin.Forms ile Tümleştirme](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Piksel ve CİHAZDAN bağımsız birimler](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Temel animasyon](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Metin ve grafikleri tümleştirme](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Temel bit eşlem bilgileri](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Satırları ve yolları](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Satırlar ve vuruş uçları](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Temel yol bilgileri](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Yol dolgusu türleri](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Çoklu çizgiler ve parametreli denklemler](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Noktalar ve tireler](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Parmak Boyama](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Çeviri dönüşümü](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Ölçekleme dönüşümü](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Döndürme dönüşümü](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Eğme dönüşümü](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Matris dönüşümleri](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Dokunma işlemeleri](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Olmayan afin dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3B Döndürme](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Eğrileri ve yolları](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Yay Çizmenin Üç Yolu](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Üç Tür Bézier Eğrisi](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [SVG Yol Verileri](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Yollar ve Bölgeler ile Kırpma](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Yol Etkileri](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Yollar ve Metin](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Yol Bilgileri ve Numaralandırması](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)
- [Bit eşlemler](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/index.md)
  * [Bit eşlemler görüntüleme](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/displaying.md)
  * [Bit eşlemler üzerinde çizim ve oluşturma](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/drawing.md)
  * [Bit eşlemler kırpma](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/cropping.md)
  * [Bit eşlem segmentli görüntüleme](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/segmented.md)
  * [Bit eşlemleri dosyaları kaydetme](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/saving.md)
  * [Bit eşlem piksel BITS erişme](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/pixel-bits.md)
  * [Bit eşlemler animasyon ekleme](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/animating.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Platforma Özgü Notlar](~/graphics-games/skiasharp/platform.md)

Bu sayfada, iOS, Android, macOS ve Windows dahil olmak üzere farklı platformlarda SkiaSharp kurulum yönergelerini açıklar.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[API belgeleri](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Göz atabilirsiniz [API belgeleri](https://developer.xamarin.com/api/namespace/SkiaSharp/) SkiaSharp web sitemizde için.

## <a name="work-in-progress"></a>Süren iş

SkiaSharp bir biz Topluluğumuzun paylaştığı aşamasındadır. Biz Skia API önemli bölümleri bağladıktan sırasında yapılacak kadar iş kaldı. Kararlı C Skia tarafından ortaya API'yi kullanıyoruz ve planımız işimizi C bağlamalarını Skia API'lerine tam kapsamı sağlamak için katkıda bulunan devam sağlamaktır.

Bağlama çalışmalarımızın Kılavuzu, lütfen yorum veya öneriniz GitHub deposuna sorunları bırakın yardımcı olmak için [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp).
