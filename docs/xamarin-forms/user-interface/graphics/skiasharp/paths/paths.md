---
title: Yol temelleri
description: "Bağlı çizgiler ve eğrilerle birleştirmek SkiaSharp SKPath nesne keşfedin"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 6d2e600ccc85f6e72e7f913e7ffb501bf62ff69a
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="path-basics"></a>Yol temelleri

_Bağlı çizgiler ve eğrilerle birleştirmek SkiaSharp SKPath nesne keşfedin_

Grafik yolunun en önemli işlevselliğinden birden fazla satır olduğunda bağlı ve ne zaman bağlı tanımlama yeteneği biridir. Bu iki üçgenler taraflarının görüldüğü gibi fark oldukça görünür olabilir:

![](paths-images/connectedlinesexample.png "Bağlı ve bağlantısı kesilmiş satırları arasındaki farkı gösteren iki üçgenler")

Bir grafik yolu yalıtılan [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) nesnesi. Bir veya daha fazla koleksiyonu yoludur *dağılımlarını*. Her dağılımı koleksiyonudur *bağlı* düz çizgiler ve eğrilerle. Dağılımlarını birbirlerine bağlı olmadığı, ancak görsel olarak birbiriyle. Bazen tek bir dağılım kendisini binebilir.

Bir dağılımı genellikle aşağıdaki yöntemine bir çağrı ile başlayan `SKPath`:

- `MoveTo` Yeni bir dağılım başlatmak için

Bu yöntem bağımsız değişkeni olarak ya da express tek bir nokta olan bir `SKPoint` değer veya ayrı X ve Y koordinatları. `MoveTo` Çağrı başına bir noktada dağılımı ve bir başlangıç kurar *geçerli noktası*. Dağılım çizgi veya yeni bir geçerli noktası olur yönteminde belirtilen bir noktasına geçerli noktasından eğri ile devam etmek için aşağıdaki yöntemleri çağırabilirsiniz:

- `LineTo` bir çizgide yolunu eklemek için
- `ArcTo` bir daire veya elips çevresi satırındaki bir yay eklemek için
- `CubicTo` küp Bezier eğrisi eklemek için
- `QuadTo` İkinci derece Bezier eğrisi eklemek için
- `ConicTo` hangi conic bölümleri (üç nokta, paraboller ve hiperboller) doğru şekilde işleyebilen bir rasyonel ikinci derece Bezier eğrisi, eklemek için

Bu beş yöntemlerden hiçbiri çizgi veya eğri açıklamak gerekli tüm bilgileri içerir. Beş bu yöntemlerin her biri hemen önceki yöntemi çağrısı tarafından belirlenen geçerli noktasıyla birlikte çalışır. Örneğin, `LineTo` yöntemi ekler bir çizgide dağılımı dayalı geçerli noktasında böylece parametresi `LineTo` yalnızca tek bir noktasıdır.

`SKPath` Sınıfı ayrıca bu altı yöntem olarak ancak ile aynı ada sahip yöntemleri tanımlayan bir `R` başında:

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

`R` Anlamına gelir *göreli*. Karşılık gelen yöntemleri aynı söz dizimini sahip oldukları `R` ancak göre geçerli noktasıdır. Bu, birden çok kez çağıran bir yöntem yolunda benzer bölümlerini çizim için kullanışlıdır.

Başka bir çağrıyı bir dağılım biter `MoveTo` veya `RMoveTo`, yeni bir dağılım veya yapılan bir çağrı başlar `Close`, dağılımı kapatır. `Close` Yöntemi otomatik olarak geçerli noktasından bir çizgide dağılımı ilk noktasına ekler ve yolun kapalı olarak işaretler herhangi vuruş caps işlenir anlamına gelir.

Açık ve kapalı dağılımlarını arasındaki farkı gösterilmiştir **iki üçgen dağılımlarını** sayfası, kullanan bir `SKPath` iki üçgenler işlemek için iki dağılımlarını nesnesiyle. İlk dağılımı açık ve ikinci kapalı. Burada [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) sınıfı:

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

Çağrı ilk dağılımı oluşan [ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) X ve Y koordinatları kullanarak yerine bir `SKPoint` değeri, üç çağrı arkasından [ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/) üç yanlarından çizmek için üçgen. Yalnızca iki çağrıları ikinci dağılımı sahip `LineTo` ancak çağrısıyla dağılımı sonlanana [ `Close` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/), dağılımı kapatır. Fark önemlidir:

[![](paths-images/twotrianglecontours-small.png "Üçlü sayfasının ekran görüntüsü iki üçgen dağılımlarını")](paths-images/twotrianglecontours-large.png#lightbox "Üçlü sayfasının ekran görüntüsü iki üçgen dağılımlarını")

Gördüğünüz gibi ilk dağılım açıkça üç bağlantılı çizgilerin dizisi olan, ancak son başlayarak bağlanmıyor. İki satır en üstünde çakışıyor. İkinci dağılımı açıkça kapatılır ve daha az biriyle gerçekleştirilmiştir `LineTo` çağırır çünkü `Close` yöntemi otomatik olarak dağılımı kapatmak için son bir satır ekler.

`SKCanvas` yalnızca bir adet tanımlanır [ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/) doldurun ve yolun vuruş yapmak için iki kez olarak adlandırılan bu gösteride yöntemi. Tüm dağılımlarını doldurulur, olanlar kapatılmamış. Kapatılmamış yolları doldurma amacıyla bir çizgide başlangıç ve bitiş noktaları dağılımlarını arasında mevcut varsayılır. Son kaldırırsanız `LineTo` ilk dağılımı ya da kaldırma `Close` ikinci dağılımı, her dağılımı çağrısından, ancak yalnızca iki kenara bir üçgen değilmiş gibi doldurulması olacaktır.

`SKPath` diğer birçok yöntemleri ve özellikleri tanımlar. Aşağıdaki yöntemlerden yöntemine bağlı olarak kapatılmamış veya kapalı olabilir yolu tüm dağılımlarını ekleyin:

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) Elips çevresi eğri eklemek için
- `AddPath` başka bir yolu geçerli yolunu eklemek için
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) geriye doğru başka bir yol eklemek için

Aklınızda bir `SKPath` nesnesi tanımlar yalnızca geometri &mdash; bir dizi noktaları ve bağlantıları. Yalnızca bir `SKPath` ile birleştirilmiş bir `SKPaint` nesne işlenen belirli rengi, vuruşun genişliğini ve benzeri yoludur. Ayrıca, aklınızda `SKPaint` nesne geçirilen `DrawPath` yöntemi yolun tamamını özelliklerini tanımlar. Birkaç renkleri gerektiren bir şey çizmek istiyorsanız, her renk için ayrı bir yol kullanmanız gerekir.

Başlangıç ve bitiş satırının görünümünü vuruş cap tarafından yalnızca tanımlandığı gibi iki satır arasındaki bağlantıyı görünümünü tarafından tanımlanan bir *vuruş birleştirme*. Bu ayarlayarak belirttiğiniz [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/) özelliği `SKPaint` üyesi için [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/) numaralandırma:

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) noktalı join için
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) yuvarlak bir birleştirme için
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) kesilmiş Tasarımlı dışı bir birleştirme için

**Vuruş birleştirmeler** sayfasında bu üç vuruş benzer şekilde kod ile birleştirme gösterir **vuruş Caps** sayfası. Bu `PaintSurface` olay işleyicisini [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) sınıfı:

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

Üç platformlarda çalışan program şöyledir:

[![](paths-images/strokejoins-small.png "Üçlü sayfasının ekran görüntüsü vuruş birleştirir")](paths-images/strokejoins-large.png#lightbox "Üçlü sayfasının ekran görüntüsü vuruş birleştirir")

Gönye satırları eriştikleri sharp noktası oluşur. İki satır küçük bir açıda katıldığında, gönye oldukça uzun olabilir. Aşırı uzun Köşeden birleştirmeler önlemek için gönye uzunluğu değeriyle sınırlı [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/) özelliği `SKPaint`. Bu uzunluğunu aşan gönye devre dışı bir Eğim birleştirme olmasını kesilmiş Tasarımlı.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
