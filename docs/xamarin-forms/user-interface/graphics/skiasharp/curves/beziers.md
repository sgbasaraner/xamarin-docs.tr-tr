---
title: "Üç tür Bézier eğrileri"
description: "Küp, ikinci derece ve conic Bézier eğrileri işlemek için SkiaSharp kullanmayı keşfedin"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: dcfcf43c89f26b4e721c9752b9cbad1f4a30cfc2
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="three-types-of-bzier-curves"></a>Üç tür Bézier eğrileri

_Küp, ikinci derece ve conic Bézier eğrileri işlemek için SkiaSharp kullanmayı keşfedin_

Bézier eğrisi Pierre Bézier (1910 – 1999) öğesini şirket eğri araba gövdeleri bilgisayar destekli tasarım için kullanılan Renault, Fransızca mühendislik adlandırılır.

Bézier eğrileri etkileşimli tasarım için oldukça uygun olması için bilinir: iyi çalışan &mdash; diğer bir deyişle, eğriyi sonsuz veya yönetilmeleri hale gelmesine neden singularities yok &mdash; ve genellikle aesthetically güzel . Yazı tipleri bilgisayar tabanlı karakter ana hatlarını genellikle Bézier eğrileri tanımlanmıştır:

![](beziers-images/beziersample.png "Bir örnek Bezier eğrisi")

Wikipedia makaleyi [Bézier eğrisi](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) bazı yararlı bilgiler içerir. Terim *Bézier eğrisi* gerçekten benzer Eğriler ailesine başvuruyor. SkiaSharp adlı Bézier eğrileri üç tür destekler *küp*, *ikinci derece*ve *conic*. Conic olarak da bilinir *rasyonel ikinci derece*.

## <a name="the-cubic-bzier-curve"></a>Küp Bézier eğrisi

Cubic Bézier eğrileri konu geldiğinde, çoğu Geliştirici düşünmek Bézier eğrisi türüdür.

Küp bir Bézier eğrisi ekleyebilirsiniz bir `SKPath` kullanarak nesne [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) üç yöntemiyle `SKPoint` parametreleri veya [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) ayrı ileaşırıyüklemesi`x` ve `y` Parametreler:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Eğriyi dağılımı anki noktada başlar. Tam küp Bezier eğrisini dört noktaları tarafından tanımlanır:

- başlangıç noktası: geçerli, dağılım veya (0, 0), noktası `MoveTo` çağrılmadıysa
- İlk denetim noktası: `point1` içinde `CubicTo` çağırın
- ikinci denetim noktası: `point2` içinde `CubicTo` çağırın
- uç noktası: `point3` içinde `CubicTo` çağırın

Sonuç eğri başlangıç noktadan başlar ve bir uç noktada sona erer. Eğriyi iki denetim noktaları aracılığıyla genellikle iletmez; Bunun yerine bunları doğru eğri çekmesini kadar LIKE mıknatıs işlev.

Deneme tarafından için küp Bézier eğrisi bir fikir almak için en iyi yoludur. Bu amacı **Bezier eğrisi** türetilen sayfa `InteractivePage`. [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) dosya başlatır `SKCanvasView` ve `TouchEffect`. [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) arka plan kod dosyası oluşturur dört `TouchPoint` kurucusu nesneleri. `PaintSurface` Olay işleyicisi oluşturur bir `SKPath` sekmesindeki dört temel bir Bézier eğrisi işlemek için `TouchPoint` nesneleri ve ayrıca noktalı tanjant çizgileri denetim noktalarından Uç noktalara çizer:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

Burada, tüm üç platformlarda çalışıyor:

[![](beziers-images/beziercurve-small.png "Üçlü sayfasının ekran görüntüsü Bezier eğrisi")](beziers-images/beziercurve-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Bezier eğrisi")

Matematiksel, eğri küp Polinomun ' dir. Eğriyi en çok üç noktalarda bir çizgide kesiştiğinden. Başlangıç noktasına eğri her zaman olduğu için ve aynı yönde tanjant, bir çizgide başlangıç noktası ilk denetim noktası. Uç noktada eğri her zaman olduğu için ve aynı yönde tanjant, ikinci denetiminden bir çizgide noktası bitiş noktası.

Küp Bézier eğrisi her zaman dört noktası bağlanma dışbükey quadrilateral ile sınırlıdır. Bu adlı bir *dışbükey hull*. Başlangıç ve bitiş noktası arasındaki çizgide üzerinde denetim noktaları kalan, Bézier eğrisi düz bir çizgi işler. Ancak Windows mobil aygıttan ekran gösterdiği gibi Eğri ayrıca kendisini arası.

Bir yol dağılımı birden çok bağlı küp Bézier eğrileri içerebilir, ancak iki küp Bézier eğrileri arasındaki bağlantıyı yalnızca aşağıdaki üç nokta colinear ise kesintisiz (diğer bir deyişle, bir çizgide bulunan):

- İkinci ilk eğrisi denetim noktası
- Ayrıca ikinci eğrinin başlangıç noktası olan ilk eğri bitiş noktası
- ikinci eğri ilk denetim noktası

Sonraki makalede [ **SVG yol verileri** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) kesintisiz bağlı Bézier eğrileri tanımını kolaylaştırmak için bir tesis öğreneceksiniz.

Bazen, bir küp Bézier eğrisi işlemek temel parametrik denklemini bilmek yararlıdır. İçin *t* 0 ile 1 arasında parametrik denklemini aşağıdaki gibidir:

x(t) = (1 – t)³x₀ + 3t(1 – t)²x₁ + 3t²(1 – t)x₂ + t³x₃

y(t) = (1 – t)³y₀ + 3t(1 – t)²y₁ + 3t²(1 – t)y₂ + t³y₃

3'ün en yüksek üs küp polynomials bunlar onaylar. Gerektiğini doğrulamak kolayca `t` 0'a eşit, nokta (x₀, y₀), başlangıç noktası olduğu ve ne zaman `t` eşittir 1, (x₃, y₃), uç noktası olduğu noktasıdır. Başlangıç noktasını yakınında (düşük değerler için `t`), ilk denetim noktası (x₁, y₁) güçlü bir efekt ve yakınında uç noktasına sahip ('T yüksek değerler ') ikinci denetim noktası (x₂, y₂) güçlü bir etkisi olmaz.

## <a name="bzier-curve-approximation-to-circular-arcs"></a>Dairesel yaylara için Bézier eğrisi yaklaşık

Bazen, bir Bézier eğrisi dairesel yay işlemek için kullanılacak uygundur. Dört bağlı Bézier eğrileri tam bir daire tanımlayabilirsiniz şekilde küp Bézier eğrisi dairesel yay çok iyi bir Çeyreğin daire kadar yaklaşık. Bu yaklaşık 25 yıldan önce yayımlanan iki makalelerinde ele alınmıştır:

> Tor Dokken, et al "Yaklaşık olarak eğimi sürekli Bézier eğrileri tarafından daire" *bilgisayar destekli geometrik tasarım 7* 33 41 (1990).

> Michael Goldapp, "Küp Polynomials tarafından dairesel yaylara yaklaşık" *bilgisayar destekli geometrik tasarım 8* 227 238 (1991).

Aşağıdaki diyagramda etiketli dört noktalarını gösterir `pto`, `pt1`, `pt2`, ve `pt3` dairesel yay yakın Bézier eğrisi (kırmızı olarak gösterilir) tanımlama:

![](beziers-images/bezierarc45.png "Dairesel yay Bézier eğrisi ile yaklaşık")

Başlangıç ve bitiş noktalarını satırlarından denetim noktalarına tanjantını daire ve Bézier eğrisi ve bunlar uzunluğu *L*. En iyi Bézier eğrisi dairesel yay yakın yukarıda Alıntı yapılan ilk makale gösterir, bu uzunluğu *L* aşağıdaki gibi hesaplanır:

L = 4 × tan(α / 4) / 3

Dolayısıyla L 0.265 eşittir 45 derece açı çizimde gösterilmektedir. Kod içinde bu değeri dairenin istenen yarıçap çarpılan.

**Bezier dairesel yay** sayfası bir Bézier eğrisi en fazla 180 derece arasında değişen açı dairesel yay yaklaşık tanımlamayla denemeler sağlar. [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) dosya başlatır `SKCanvasView` ve `Slider` açı seçme. `PaintSurface` Olay işleyicisini [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) arka plan kod dosyasına (0, 0) noktası Kanvasın ortasına ayarlamak için bir dönüşüm kullanır. Karşılaştırma için bu noktasında ortalanmış bir daire çizer ve Bézier eğrisi için iki denetim noktaları hesaplar:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
    }
}

// Vector methods
SKPoint Normalize(SKPoint v)
{
    float magnitude = Magnitude(v);
    return new SKPoint(v.X / magnitude, v.Y / magnitude);
}

float Magnitude(SKPoint v)
{
    return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
}

```

Başlangıç ve bitiş noktaları (`point0` ve `point3`) normal parametrik denklemini daire için temel alınarak hesaplanır. Daireye adresindeki ortalanır olduğundan (0, 0), aşağıdaki noktaları da radyal vektörleri olarak daireye Merkezi'nden çevresini işlenebilir. Daireye tanjantını dik bu Radyal vektörüne altındadır şekilde olan satırları denetim noktaları açıktır. Vektör sağ açıyla yalnızca özgün vektör takas X ve Y koordinatları ve bunlardan birini negatif yapılan türüdür.

Üç farklı açıları ile üç platformlarda çalışan program şöyledir:

[![](beziers-images/beziercirculararc-small.png "Üçlü sayfasının ekran görüntüsü Bezier dairesel yay")](beziers-images/beziercirculararc-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Bezier dairesel yay")

Yakından Windows Mobile ekranında aramak ve Bézier eğrisi özellikle Yarım Daireli 180 derece açıdır, ancak 90 derece açıdır görüntülendiğinde çeyrek daire yalnızca daha iyi uyacak şekilde göründüğü iOS ekran gösterir farklılık göstermesi gerektiğini görürsünüz.

Çeyrek daire şöyle yönlendirilmiş olduğunda, iki denetim noktası koordinatları hesaplama oldukça kolaydır:

![](beziers-images/bezierarc90.png "Yaklaşık bir Bézier eğrisi çeyrek daire olarak")

RADIUS dairenin 100, ardından olup olmadığını *L* 55 ve anımsaması kolay bir sayı ise.

**Daireye karesini** sayfa canlandırır şekil bir daire ve kare arasında. Daireye olan koordinatları bu dizi tanımında ilk sütununda gösterilen dört Bézier eğrileri tarafından benzetilir [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) sınıfı:

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

İkinci sütun, yaklaşık dairenin alanı ile aynı alanıdır karesini tanımlayan dört Bézier eğrileri koordinatlarını içerir. (Köşeli ile çizim *tam* alanıdır verilmiş bir daire olarak Klasik çözümlenemeyen geometrik sorun [daireye karesini](https://en.wikipedia.org/wiki/Squaring_the_circle).) Kare Bézier eğrileri ile işlemeye, her eğri için iki denetim noktaları aynıdır ve Bézier eğrisi düz bir çizgi işlenen için başlangıç ve bitiş noktalarını colinear oldukları.

Ara değerli değerleri animasyon dizisinin üçüncü sütun içindir. Sayfa bir süreölçer 16 milisaniye cinsinden için ayarlar ve `PaintSurface` işleyicisi o oranda çağrılır:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

Puanları bir sinusoidally salınım yapan değerine göre ilişkilendirileceğini `t`. Ara değerli noktaları ardından dört bağlı Bézier eğrileri bir dizi oluşturmak için kullanılır. Bir daire ilerlemesini kare gösteren üç platformlarda çalışan animasyon şöyledir:

[![](beziers-images/squaringthecircle-small.png "Üçlü ekran görüntüsü Squaring daire sayfa")](beziers-images/squaringthecircle-large.png#lightbox "Üçlü ekran görüntüsü Squaring daire sayfası")

Bu tür bir animasyon dairesel yaylara ve düz çizgiler işlenmek üzere algorithmically yeterince esnektir Eğriler olmadan olanaksız olacaktır.

**Bezier sonsuz** sayfası aynı zamanda bir Bézier eğrisi dairesel yay yaklaşık yeteneğini avantajlarından yararlanır. Burada `PaintSurface` işleyicisinden [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) sınıfı:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
                                     info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

Bunun nasıl ilişkili olduğunu görmek için bu koordinatları grafik kağıda çizmek için iyi bir alıştırma olabilir. Sonsuz oturum (0, 0) nokta etrafında ortalanır ve iki döngüler merkezlerinin sahip (–150, 0) ve (150, 0) ve 100 yarıçaplarını. Dizi `CubicTo` komutları, gördüğünüz –95 ve –205 değerlerine alma denetim noktaları koordinatlarını X (Bu değerler –150 artı ve eksi 55), 205 ve 95 (150 artı ve eksi 55) yanı sıra 250 ve sağ ve sol kenarı –250. Tek özel durum, sonsuzluk oturum kendisini Merkezi'nde kestiği durumdur. Bu durumda, kontrol noktası koordinatları ile 50 ve – 50 eğrinin merkezini yakın çıkışı düzeltmek için birlikte bulunur.

Tüm üç platformlarda sonsuz oturum şöyledir:

[![](beziers-images/bezierinfinity-small.png "Üçlü sayfasının ekran görüntüsü Bézier sonsuz")](beziers-images/bezierinfinity-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Bézier sonsuz")

Tarafından işlenen sonsuz oturum daha doğru merkezi biraz daha yumuşak **yay sonsuz** gelen sayfa [ **yay çizmek için üç yol** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) makalesi.

## <a name="the-quadratic-bzier-curve"></a>İkinci dereceden Bézier eğrisi

İkinci dereceden Bézier eğrisi yalnızca bir denetim noktası varsa ve eğri yalnızca üç noktaları tarafından tanımlanır: başlangıç noktasını, denetim noktası ve bitiş noktası. İkinci derece Polinomun eğri olacak şekilde, 2, en yüksek üs olması dışında parametrik denklemini küp Bézier eğrisi çok benzer:

x(t) = (1 – t)²x₀ + 2t(1 – t)x₁ + t²x₂

y(t) = (1 – t)²y₀ + 2t(1 – t)y₁ + t²y₂

İkinci dereceden Bézier eğrisi yolunu eklemek için kullanın [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) yöntemi veya [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/) ayrı ile aşırı `x` ve `y` koordinatları:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

İçin geçerli konumundan eğri yöntemleri eklemek `point2` ile `point1` denetim noktası olarak.

İkinci derece Bézier eğrileri deneyimleyebilirsiniz **ikinci dereceden eğrisi** çok benzer sayfa **Bezier eğrisi** yalnızca üç dokunma noktaları sahip dışında sayfa. Burada `PaintSurface` işleyicisinde [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) arka plan kod dosyası:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Ve burada tüm üç platformlarda çalışan:

[![](beziers-images/quadraticcurve-small.png "Üçlü sayfasının ekran görüntüsü ikinci dereceden eğrisi")](beziers-images/quadraticcurve-large.png#lightbox "Üçlü sayfasının ekran görüntüsü ikinci dereceden eğrisi")

Noktalı çizgiler başlangıç noktasını ve bitiş noktası eğri teğet ve denetim noktasına karşıladığından.

İkinci derece Bézier eğrisi genel şeklin gerekiyorsa, ancak yalnızca bir denetim noktası yerine iki kolaylık tercih iyi olur. İkinci derece Bézier, dahili Skia elips yaylar işlemek için kullanılan neden olan tüm diğer eğri den daha verimli bir şekilde işler.

Ancak, ikinci dereceden Bézier eğrisi şeklini birden çok ikinci derece Béziers elips yay yaklaşık için gereken neden olduğu elips, değil. İkinci derece Bézier bunun yerine bir parabol parçasıdır.

## <a name="the-conic-bzier-curve"></a>The Conic Bézier Curve

Conic Bézier eğrisi &mdash; olarak da bilinen rasyonel ikinci dereceden Bézier eğrisi &mdash; Bézier eğrileri ailesine görece son ektir. İkinci dereceden Bézier eğrisi gibi bir başlangıç noktası, bir uç noktası ve bir denetim noktası rasyonel ikinci dereceden Bézier eğrisi içerir. Ancak rasyonel ikinci dereceden Bézier eğrisi de gerektirir bir *ağırlık* değeri. Bu adlı bir *rasyonel* parametrik formüller oranları içerdiğinden ikinci derece.

Parametrik denklemini X ve Y aynı payda paylaşmak oranları için. Paydası denklemi işte *t* 0'dan arasında değişen 1 ve bir ağırlık değeri *w*:

d(t) = (1 – t)² + 2wt(1 – t) + t²

Teorik, rasyonel ikinci derece, her üç koşullarını üç ayrı ağırlık değerleri içerebilir ancak bu yalnızca bir ağırlık değeri orta terimini için basit hale getirilebilir.

Orta terim de ağırlık değeri içerir ve ifade paydası tarafından bölünür dışında X ve Y koordinatları için parametrik denklemini parametrik denklemini ikinci derece Bézier için benzerdir:

x(t) = ((1 – t)²x₀ + 2wt(1 – t)x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1-t) y₁ + t²y₂)) ÷ d(t)

Rasyonel ikinci derece Bézier eğrileri de denir *conics* tam olarak herhangi bir conic bölümü parçalarını gösterebilir çünkü &mdash; hiperboller, paraboller, üç nokta ve daireler.

Rasyonel ikinci dereceden Bézier eğrisi yolunu eklemek için kullanın [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) yöntemi veya [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) ayrı ile aşırı `x` ve `y` koordinatları:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

En son fark `weight` parametresi.

**Conic eğri** sayfası bu Eğriler denemeler sağlar. `ConicCurvePage` Sınıfı türer `InteractivePage`. [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) dosya başlatır bir `Slider` ağırlık değeri 2 ile –2 arasındaki seçin. [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) arka plan kod dosyası oluşturur üç `TouchPoint` nesneleri ve `PaintSurface` işleyici yalnızca denetime teğet çizgili sonuç eğri işler noktalar:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Burada, tüm üç platformlarda çalışıyor:

[![](beziers-images/coniccurve-small.png "Üçlü sayfasının ekran görüntüsü Conic eğri")](beziers-images/coniccurve-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Conic eğri")

Gördüğünüz gibi denetim noktası ağırlık daha yüksek olduğunda daha doğru eğri çekme görünüyor. Ağırlık sıfır eğri başlangıç noktasından bir çizgide uç noktasına olur.

Teorik, negatif ağırlıkları izin verilir ve Eğ eğriye neden *hemen* denetim noktasından. Ancak, belirli değerleri için negatif olmasını parametrik denklemini paydanın – 1 veya nedeni aşağıda ağırlık verir *t*. Büyük olasılıkla bu nedenle, negatif ağırlıkları içinde göz ardı edilir `ConicTo` yöntemleri. **Conic eğri** program negatif ağırlıkları ayarlamanıza olanak tanır ancak deneyerek görebileceğiniz gibi negatif ağırlıkları sıfır ağırlığını aynı etkiye sahip ve işlenmek üzere bir çizgide neden.

Denetim noktası ve ağırlığını kullanmayı türetilen çok kolaydır `ConicTo` dairesel yay kadar çizmek için yöntemi (ancak değil de dahil olmak üzere) bir Yarım Daireli. Aşağıdaki şemada denetim noktasından başlangıç ve bitiş noktalarını Eğim satırlarından karşılar.

![](beziers-images/conicarc.png "Dairesel yay conic yay işlenmesi")

Trigonometri dairenin merkezi denetim noktasından uzaklığını belirlemek için kullanabilirsiniz: Açının kosinüsü yarım α tarafından bölünmüş daire RADIUS değil. Başlangıç ve bitiş noktaları arasında dairesel yay çizmek için aynı bu yarım açının kosinüsünü için ağırlığını ayarlayın. Açı 180 derece ise, ardından tanjant çizgileri hiçbir zaman karşılamak ve Ağırlık sıfırdır dikkat edin. Ancak açıları küçük için 180 derece, matematik düzgün çalışır.

**Conic dairesel yay** sayfa bu gösterir. [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) dosya başlatır bir `Slider` açı seçme. `PaintSurface` İşleyicisinde [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) arka plan kod dosyasına hesaplar denetim noktası ve Ağırlık:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

Gördüğünüz gibi arasında visual fark yoktur `ConicTo` yolu kırmızı olarak gösterilir ve başvuru için görüntülenen temel daire:

[![](beziers-images/coniccirculararc-small.png "Üçlü sayfasının ekran görüntüsü Conic dairesel yay")](beziers-images/coniccirculararc-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Conic dairesel yay")

Ancak, açı 180 derece ve matematik başarısız olarak ayarlayın.

Bu durumda talihsiz, `ConicTo` negatif ağırlıkları desteklemez (üzerinde parametrik denklemini göre) teorik olarak, başka bir çağrıyı ile daire tamamlanabilir çünkü `ConicTo` aynı noktaları ancak ağırlık negatif bir değer. Bu tam bir daire yalnızca iki oluşturma olanak tanır `ConicTo` Eğriler bağlı herhangi bir açıda arasında (ancak dahil değil) olarak sıfır derece ve 180 derece.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
