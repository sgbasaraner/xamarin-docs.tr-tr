---
title: SkiaSharp giriş
description: Bu belge, temel SkiaSharp kavramları hakkında temel bilgiler sağlar. Özellikle, alma ve bir SKCanvas üzerinde çizim ele alır.
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: eb4a391c52c598c6d276b75028337bf54455e7b4
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615502"
---
# <a name="an-introduction-to-skiasharp"></a>SkiaSharp giriş

_Bu SkiaSharp kavramları hakkında temel bilgiler sağlar_

SkiaSharp zengin ve güçlü 2B grafik 2B arabellekler işlemek için kullanılan bir API sağlar.  Bu, özel kullanıcı arabirimi öğeleri ve uygulamanıza eklenebilir 2B grafikler uygulamak için kullanabilirsiniz.  SkiaSharp olan bir .NET bağlama [Skia](https://skia.org) kitaplığı ve bu kitaplık gücünü ve özellikleri devralır.

Platformlar arası şu anda kullanılabilir kitaplık [NuGet paketini](https://www.nuget.org/packages/SkiaSharp), NuGet başvuru ekleyerek bunu projenize ekleyebilirsiniz.

Çizmek için kodunuzu oluşturacak bir `SkCanvas` burada çizim işlemlerini gerçekleşecek surface açıklar.

## <a name="obtaining-an-skcanvas"></a>Bir SKCanvas edinme

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>SKCanvas üzerinde çizim

`SKCanvas` Kullanan başka bir çizim için Ruhu benzer bir çizim modeli modeller tanıyor olabileceği gibi bir isteğe bağlı şeffaflık kanalıyla renk kullanır ve çizgiler, yaylar, metin ve görüntüleri çizebilirsiniz.

SkiaSharp ile yapılabilir birçok farklı şey birkaçı aşağıda verilmiştir.  Değişkeni aşağıdaki örneklerde `canvas` SKCanvas türüdür.

### <a name="drawing-xamagon"></a>Xamagon çizme

Bu örnekte Xamarin'in logosu Xamagon çizer:

```csharp
// clear the canvas / fill with white
canvas.Clear (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x2c, 0x3e, 0x50);
  paint.StrokeCap = SKStrokeCap.Round;

  // create the Xamagon path
  using (var path = new SKPath ()) {
    path.MoveTo (71.4311121f, 56f);
    path.CubicTo (68.6763107f, 56.0058575f, 65.9796704f, 57.5737917f, 64.5928855f, 59.965729f);
    path.LineTo (43.0238921f, 97.5342563f);
    path.CubicTo (41.6587026f, 99.9325978f, 41.6587026f, 103.067402f, 43.0238921f, 105.465744f);
    path.LineTo (64.5928855f, 143.034271f);
    path.CubicTo (65.9798162f, 145.426228f, 68.6763107f, 146.994582f, 71.4311121f, 147f);
    path.LineTo (114.568946f, 147f);
    path.CubicTo (117.323748f, 146.994143f, 120.020241f, 145.426228f, 121.407172f, 143.034271f);
    path.LineTo (142.976161f, 105.465744f);
    path.CubicTo (144.34135f, 103.067402f, 144.341209f, 99.9325978f, 142.976161f, 97.5342563f);
    path.LineTo (121.407172f, 59.965729f);
    path.CubicTo (120.020241f, 57.5737917f, 117.323748f, 56.0054182f, 114.568946f, 56f);
    path.LineTo (71.4311121f, 56f);
    path.Close ();

    // draw the Xamagon path
    canvas.DrawPath (path, paint);
  }
}
```

### <a name="drawing-text"></a>Metin çizme

```csharp
// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.TextSize = 64.0f;
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x42, 0x81, 0xA4);
  paint.IsStroke = false;

  // draw the text
  canvas.DrawText ("Skia", 0.0f, 64.0f, paint);
}
```

### <a name="drawing-bitmaps"></a>Bit eşlemler çizme

```csharp
Stream fileStream = File.OpenRead ("MyImage.png");

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  canvas.DrawBitmap(bitmap, SKRect.Create(Width, Height), paint);
}
```

### <a name="drawing-with-image-filters"></a>Resim filtreleri ile çizme

```csharp
Stream fileStream = File.OpenRead ("MyImage.png"); // open a stream to an image file

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  // create the image filter
  using (var filter = SKImageFilter.CreateBlur(5, 5)) {
    paint.ImageFilter = filter;

    // draw the bitmap through the filter
    canvas.DrawBitmap(bitmap, SKRect.Create(width, height), paint);
  }
}
```

## <a name="more-information"></a>Daha fazla bilgi

SkiaSharp kullanma hakkında daha fazla bilgi bulunabilir [API belgelerini çevrimiçi](https://developer.xamarin.com/api/namespace/SkiaSharp/)


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp iOS çalışma kitabı](https://developer.xamarin.com/workbooks/graphics/skiasharp/logo/skialogo-ios.workbook)
