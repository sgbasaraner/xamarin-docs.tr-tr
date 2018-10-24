---
title: SkiaSharp yolu temel bilgileri
description: Bu makalede, bağlı çizgiler ve eğrilerle birleştirme SkiaSharp SKPath nesnesini inceler ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 2980884c94bfa2cddbef89e8a2e5f6aaf778d033
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615541"
---
# <a name="path-basics-in-skiasharp"></a>SkiaSharp yolu temel bilgileri

_Bağlantılı çizgiler ve eğrilerdir birleştirme SkiaSharp SKPath nesne keşfedin_

Grafik yolun en önemli özelliklerden biri, birden fazla satır olduğunda bağlı ve ne zaman bağlı tanımlamak için olanağıdır. İki bu üçgen taraflarının göz atarak fark önemli olabilir:

![](paths-images/connectedlinesexample.png "Bağlı ve bağlantısı kesik çizgilerin arasındaki farkı gösteren iki üçgen")

Bir grafik yolu yalıtılan [ `SKPath` ](xref:SkiaSharp.SKPath) nesne. Bir koleksiyon bir veya daha fazla yoludur *dağılımlarını*. Her dağılımı oluşan bir koleksiyondur *bağlı* düz çizgiler ve eğrilerle. Dağılımlarını birbirine bağlı olmadığı, ancak görsel olarak örtüşüyor olabilir. Bazen tek bir dağılımı kendisini binebilir.

Bir dağılımı genellikle aşağıdaki yöntemi çağrısı ile başlar `SKPath`:

- [`MoveTo`](SkiaSharp.SKPath.MoveTo*) Yeni bir dağılımı başlamak için

Bu yönteme bağımsız değişken olarak express tek bir noktası olan bir `SKPoint` değer veya ayrı X ve Y koordinatları. `MoveTo` Çağrı başına bir noktada dağılımı ve bir ilk kurar *geçerli noktası*. Bir satır veya eğriye zamandaki geçerli noktadan sonra yeni geçerli noktası olur yöntemi içinde belirtilen bir nokta dağılımı devam etmek için aşağıdaki yöntemleri çağırabilirsiniz:

- [`LineTo`](SkiaSharp.SKPath.LineTo*) düz çizgi yolu eklemek için
- [`ArcTo`](SkiaSharp.SKPath.ArcTo*) bir daire veya elipsin çevresi satırda olduğu bir yay eklemek için
- [`CubicTo`](SkiaSharp.SKPath.CubicTo*) bir üçüncü dereceden Bezier eğrisi eklemek için
- [`QuadTo`](SkiaSharp.SKPath.QuadTo*) bir ikinci dereceden Bezier eğrisi eklemek için
- [`ConicTo`](SkiaSharp.SKPath.ConicTo*) conic bölümleri (üç nokta, paraboller ve hiperboller) doğru bir şekilde oluşturulabilen bir rasyonel ikinci dereceden Bezier eğrisi, eklemek için

Bu beş yöntemlerin hiçbiri satır ya da eğrisini açıklamak gereken tüm bilgileri içerir. Beş bu yöntemlerin her biri, yöntem çağrısının hemen önceki tarafından belirlenen geçerli noktasıyla birlikte çalışır. Örneğin, `LineTo` yöntemi ekler bir çizgide dağılımı dayalı geçerli bir noktasında, bu nedenle parametresi `LineTo` yalnızca tek bir nokta.

`SKPath` Sınıfı da tanımlar bu altı yöntem olarak, ancak ile aynı ada sahip yöntem bir `R` başında:

- [`RMoveTo`]((SkiaSharp.SKPath.RMoveTo*))
- [`RLineTo`](SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](SkiaSharp.SKPath.RConicTo*)

`R` Anlamına gelen *göreli*. Bu yöntemleri aynı söz dizimine karşılık gelen yöntemlere sahip `R` ancak göre geçerli noktasıdır. Bunlar, benzer bir yolda birden çok kez çağıran bir yöntem bölümlerini çizmek için kullanışlıdır.

Başka bir çağrıyı bir dağılımı biten `MoveTo` veya `RMoveTo`, yeni bir dağılımı veya çağrı başlar `Close`, dağılımı kapatır. `Close` Yöntemi otomatik olarak zamandaki geçerli noktadan dağılımı ilk noktasına düz bir çizgi ekler ve yolun kapalı olarak işaretler herhangi bir vuruş uçları işlenir anlamına gelir.

Açık ve kapalı dağılımlarını arasındaki farkı gösterilmiştir **iki üçgen dağılımlarını** sayfası, kullanan bir `SKPath` iki üçgen işlemek için iki dağılımlarını nesne. İlk dağılımı açık ve ikinci kapalı. İşte [ `TwoTriangleContoursPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) sınıfı:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

Bir çağrı ilk dağılımı oluşan [ `MoveTo` ](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) X ve Y koordinatları kullanarak yerine `SKPoint` değeri, üç çağrı ardından [ `LineTo` ](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) üç tarafının çizmek için üçgeni temsil eder. İkinci dağılımı yalnızca iki çağrısına sahip `LineTo` ancak çağrısıyla dağılımı bittikten [ `Close` ](xref:SkiaSharp.SKPath.Close), dağılımı kapatır. Önemli bir fark vardır:

[![](paths-images/twotrianglecontours-small.png "Üçlü sayfasının ekran görüntüsü iki üçgen dağılımlarını")](paths-images/twotrianglecontours-large.png#lightbox "Üçlü sayfasının ekran görüntüsü iki üçgen dağılımlarını")

Gördüğünüz gibi ilk dağılımı açıkça üç bağlı çizgiler dizisidir, ancak son başlayarak bağlanmıyor. Aşağıdaki iki satırı üstünde çakışıyor. İkinci dağılımı açıkça kapatılır ve daha az biriyle gerçekleştirilmiştir `LineTo` çağırır çünkü `Close` yöntemi, bir son satırı dağılımı kapatmak için otomatik olarak ekler.

`SKCanvas` yalnızca bir adet tanımlanır [ `DrawPath` ](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) doldurun ve yolun vuruş yapmak için bu gösteride adlı iki kez yöntemi. Tüm dağılımlarını doldurulur, olanlar kapatılmamış. Olmayan yollar doldurma amacıyla, düz bir çizgi başlangıç ve bitiş noktaları dağılımlarını arasında mevcut varsayılır. Son kaldırırsanız `LineTo` ilk dağılımı ya da remove `Close` ikinci dağılımı, her dağılımı çağrısından bu üçgen gibi doldurulması yalnızca iki kenara ancak olacaktır.

`SKPath` diğer birçok yöntemleri ve özellikleri tanımlar. Aşağıdaki yöntemlerden yöntemine bağlı olarak kapalı değil veya kapalı, yolun tamamı dağılımlarını ekleyin:

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) bir elipsin çevresi üzerinde bir eğri eklemek için
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*) Geçerli bir yol için başka bir yol eklemek için
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath)) geriye doğru başka bir yol eklemek için

Aklınızda bir `SKPath` nesne tanımlar yalnızca geometri &mdash; bir dizi noktaları ve bağlantıları. Yalnızca bir `SKPath` ile birleştirilmiş bir `SKPaint` nesne belirli bir renk, darbe genişliği ve diğerleri ile işlenen yoludur. Ayrıca, aklınızda `SKPaint` geçirilen nesne `DrawPath` yöntemi yolun tamamını özelliklerini tanımlar. Birkaç renkleri gerektiren bir şeyler çizmek istiyorsanız, her renk için ayrı bir yol kullanmalısınız.

Başlangıç ve bitiş satır görünümünü bir vuruş uç tarafından yalnızca tanımlandığı gibi iki satır arasında bağlantı görünümünü tarafından tanımlanan bir *vuruş birleştirme*. Ayarlayarak belirtin [ `StrokeJoin` ](xref:SkiaSharp.SKPaint.StrokeJoin) özelliği `SKPaint` üyesinin [ `SKStrokeJoin` ](xref:SkiaSharp.SKStrokeJoin) sabit listesi:

- `Miter` noktalı bir birleştirme için
- `Round` yuvarlak bir birleştirme için
- `Bevel` Birleştirme kesilmiş Tasarımlı-kapatmak için

**Vuruş birleştirmeler** sayfasında bu üç vuruş benzer şekilde kod birleştirmelerle gösterir **vuruş uçları** sayfası. Bu `PaintSurface` olay işleyicisinde [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) sınıfı:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Right
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

Üç platformlarda çalışan bir program şöyledir:

[![](paths-images/strokejoins-small.png "Üçlü sayfasının ekran görüntüsü vuruş birleşimler")](paths-images/strokejoins-large.png#lightbox "Üçlü sayfasının ekran görüntüsü vuruş birleştirir")

Gönye satırları eriştikleri bir sharp noktası oluşur. İki satırı, küçük bir açıyla katıldığında, gönye oldukça uzun olabilir. Aşırı uzun gönye birleştirmeler önlemek için gönye uzunluğu değeriyle sınırlıdır [ `StrokeMiter` ](xref:SkiaSharp.SKPaint.StrokeMiter) özelliği `SKPaint`. Bu uzunluğu aşan gönye kapalı bir Eğim birleştirme olmasını kesilmiş Tasarımlı.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
