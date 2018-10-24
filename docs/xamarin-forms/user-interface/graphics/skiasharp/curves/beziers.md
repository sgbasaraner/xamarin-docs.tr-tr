---
title: Üç tür Bézier Eğriler
description: Bu makalede SkiaSharp küp, ikinci dereceden ve conic Bézier Eğriler Xamarin.Forms uygulamaları oluşturmak için nasıl kullanılacağını açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: davidbritch
ms.author: dabritch
ms.date: 05/25/2017
ms.openlocfilehash: 1da0ee6155548a38057e4c7bf49ae5b90d445d79
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615346"
---
# <a name="three-types-of-bzier-curves"></a>Üç tür Bézier Eğriler

_SkiaSharp küp, ikinci dereceden ve conic Bézier eğrileri işlemek için nasıl kullanılacağını keşfedin_

Bézier eğrisi Pierre Bézier (1910 – 1999), Fransız bir mühendislik eğri araba gövdeleri bilgisayar destekli tasarım için kullanılan Renault, otomotiv şirket adlandırılır.

Bézier eğrileri etkileşimli bir tasarımı için uygun olan bilinir: iyi çalışan oldukları &mdash; başka bir deyişle, sonsuz veya zahmetli hale eğrinin neden singularities yok &mdash; ve genellikle aesthetically güzel :

![](beziers-images/beziersample.png "Bir örnek Bezier eğrisi")

Bilgisayar tabanlı yazı tiplerinin karakter anahatlarını genellikle Bézier eğrileri ile tanımlanır.

Wikipedia makaleyi [ **Bézier eğrisi** ](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) bazı yararlı bilgiler içerir. Terim *Bézier eğrisi* gerçekten benzer eğrileri ailesi için ifade eder. SkiaSharp destekleyen üç tür olarak adlandırılan Bézier eğrilerinin *üçüncü dereceden*, *ikinci dereceden*ve *conic*. Conic olarak da bilinir *rasyonel dereceden*.

## <a name="the-cubic-bzier-curve"></a>Üçüncü dereceden Bézier eğrisi

Cubic Bézier eğrileri konusunu ortaya çıktığında, çoğu geliştirici, düşündüğünüz Bézier eğrisi türüdür.

Bir küp Bézier eğriye ekleyebileceğiniz bir `SKPath` kullanarak nesne [ `CubicTo` ](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint)) üç yöntemi `SKPoint` parametreleri veya [ `CubicTo` ](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single)) ayrı aşırıyüklemesi`x` ve `y` parametreleri:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Eğriyi dağılımı geçerli noktada başlar. Tam üçüncü dereceden Bezier eğrisi dört noktaları tarafından tanımlanır:

- başlangıç noktası: geçerli, dağılımı veya (0, 0), işaret `MoveTo` çağrılmadıysa
- ilk noktası'nı denetleyen: `point1` içinde `CubicTo` çağırın
- İkinci noktası'nı denetleyen: `point2` içinde `CubicTo` çağırın
- uç noktası: `point3` içinde `CubicTo` çağırın

Sonuç eğri başlangıç noktadan başlar ve bitiş noktasında sona erer. Eğriyi iki denetim noktaları aracılığıyla genellikle geçirmez; Bunun yerine denetimi gibi eğriyi bunları doğrultusunda almayı çok mıknatıs işlevi işaret eder.

Deneme tarafından bir küp Bézier eğrisini almak için en iyi yoludur. Amacı budur **Bezier eğrisi** türetilen sayfa `InteractivePage`. [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) dosya başlatır `SKCanvasView` ve `TouchEffect`. [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) arka plan kod dosyası oluşturur dört `TouchPoint` oluşturucusuna nesneleri. `PaintSurface` Olay işleyicisi oluşturur bir `SKPath` dört temel Bézier eğri işlenecek `TouchPoint` nesneleri ve ayrıca noktalı Eğim satırları Uç noktalara denetim noktalarını çizer:

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

Burada üç tüm platformlarda çalıştığı:

[![](beziers-images/beziercurve-small.png "Üçlü sayfasının ekran görüntüsü Bezier eğrisi")](beziers-images/beziercurve-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Bezier eğrisi")

Matematiksel olarak, bir küp Polinomun eğri olur. Eğriyi en fazla üç noktalarda düz bir çizgi keser. Başlangıç noktasına eğri her zaman olduğu için ve aynı yönde tanjant, düz bir çizgi başlangıç noktası için ilk denetim noktası. Bitiş noktasında eğri her zaman olduğu için ve aynı yönde tanjant, düz bir çizgi ikinci denetim noktası uç noktasına.

Üçüncü dereceden Bézier eğrisi her zaman dört noktası bağlanan bir dışbükey quadrilateral ile sınırlıdır. Bu adlı bir *dışbükey Kabuk*. Başlangıç ve bitiş noktasına arasında düz bir çizgi üzerindeki denetim noktaları yer alan, Bézier eğri düz bir çizgi işler. Ancak üçüncü ekran görüntüsünde gösterildiği gibi eğriyi ayrıca kendisi arası.

Yol dağılımı birden çok bağlı üçüncü dereceden Bézier eğrileri içerebilir, ancak iki küp Bézier eğrileri arasındaki bağlantı yalnızca, aşağıdaki üç nokta colinear kesintisiz (diğer bir deyişle, düz bir satırda bulunan):

- İkinci ilk eğri denetim noktası
- Ayrıca ikinci eğrinin başlangıç noktası olan ilk eğrisinin uç noktası
- ilk ikinci eğrisi denetim noktası

Sonraki makalede [ **SVG yol verileri**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md), kesintisiz bağlı Bézier eğrileri tanımını kolaylaştırmak için bir özellik keşfedeceksiniz.

Bazı durumlarda, üçüncü dereceden Bézier eğrisi işleme temel alınan parametreli denklemler bilmek de yararlı olabilir. İçin *t* 0 ile 1 arasında parametreli denklemler aşağıdaki gibidir:

x(t) = (1 – t)³x₀ + 3t(1 – t)²x₁ + 3t²(1 – t)x₂ + t³x₃

y(t) = (1-t) ³y₀ + 3t (1-t) ²y₁ + 3t² (1-t) y₂ + t³y₃

En yüksek üs 3 bunların üçüncü dereceden polynomials olduğunu onaylar. Gerektiğini doğrulamak kolayca `t` 0 değerine eşittir; noktası (x₀ y₀), başlangıç noktası olduğu ve ne zaman `t` eşittir 1, (x₃ y₃), bitiş noktası olduğu noktasıdır. Neredeyse başlangıç noktası (düşük değerler için `t`), ilk denetim noktası (x₁, y₁) güçlü efekt ve neredeyse uç noktası olan (yüksek değerler 'T ') ikinci denetim noktası (x₂, y₂) güçlü bir etkisi.

## <a name="bezier-curve-approximation-to-circular-arcs"></a>Bezier eğrisi yaklaşık dairesel yaylara için

Bazen, Bézier eğriyi dairesel bir yay işlemek için kullanmak kullanışlıdır. Dört bağlı Bézier eğrileri tam bir daire bu sayede üçüncü dereceden Bézier eğrisi dairesel bir yay çok iyi çeyrek daire kadar yaklaşık. Bu yaklaşık 25 yılı önce yayımlanan iki makalelerde ele alınmıştır:

> Tor Dokken, tarayıcılarınızda, "Daireler eğrilik sürekli Bézier eğrileri tarafından iyi yaklaşığını" *bilgisayar destekli geometrik tasarım 7* 33 41 (1990).

> Michael Goldapp, "Küp Polynomials tarafından dairesel yaylara yaklaşığını" *bilgisayar destekli geometrik tasarım 8* 227 238 (1991).

Aşağıdaki diyagramda dört noktası etiketli gösterilmektedir `pto`, `pt1`, `pt2`, ve `pt3` tanımlama dairesel bir yay yakın Bézier eğrisi (kırmızı renkte gösterilir):

![](beziers-images/bezierarc45.png "Yaklaşık olarak ile Bézier eğriyi dairesel bir yay")

Tanjantı daire ve Bézier eğri için başlangıç ve bitiş noktalarını satırlarından denetim noktalarına olan ve uzunluğuna sahip oldukları *L*. En iyi Bézier eğriyi dairesel bir yay oluşturma, yukarıda bahsedilen ilk makale belirten olduğunda bu uzunluğu *L* şöyle hesaplanır:

M = 4 × tan(α / 4) / 3

M 0.265 eşittir bir açısını 45 derecenin çizimde gösterilmektedir. Kod içinde bu değer dairenin istenen yarıçap çarpılmasına.

**Dairesel Yayı Bezier** sayfası dairesel bir yay en fazla 180 derece arasında değişen açı yaklaşık olarak belirlemenizi sağlayan Bézier eğri tanımlamayla denemenizi sağlar. [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) dosya başlatır `SKCanvasView` ve `Slider` açı seçme. `PaintSurface` Olay işleyicisinde [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) arka plan kod dosyası dönüşüm tuval merkezine noktası (0, 0) ayarlamak için kullanır. Karşılaştırma için o noktasında ortalanmış bir daire çizen ve ardından iki denetim noktaları Bézier eğri için hesaplar:

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

Başlangıç ve bitiş noktalarını (`point0` ve `point3`) normal parametreli denklemler daire için temel alınarak hesaplanır. Daire adresindeki ortalanır olduğundan (0, 0), bu noktaları ayrıca Radyal vektör olarak dairenin Merkezi'nden çevresi için işlenebilir. Dik Açılı Radyal bu vektörler için olur böylece daireye tanjantı satırları denetim noktaları açıktır. Bir diğerine sağ açıyla yalnızca özgün vektörün takas X ve Y koordinatları ve bunlardan birinin negatif yapılan vektördür.

Üç farklı açıları ile üç platformları üzerinde çalışan bir program şöyledir:

[![](beziers-images/beziercirculararc-small.png "Üçlü sayfasının ekran görüntüsü dairesel Yayı Bezier")](beziers-images/beziercirculararc-large.png#lightbox "Üçlü sayfasının ekran görüntüsü dairesel Yayı Bezier")

Üçüncü ekran yakından bakın ve Bézier eğrisi özellikle bir Yarım Daireli 180 derece açı, ancak 90 derece açıdır görüntülendiğinde çeyrek-daire düzgün uyacak şekilde görünüyor iOS ekranındaki gösterir farklılık göstermesi gerektiğini görürsünüz.

Çeyrek daire şöyle yönlendirilmiş olduğunda, iki denetim noktası koordinatları hesaplama oldukça kolaydır:

![](beziers-images/bezierarc90.png "Çeyrek daire Bézier eğrisi yaklaşığını")

RADIUS dairenin 100, ardından olup olmadığını *L* 55 olduğu ve bir kolayca numarası unutmayın olmasıdır.

**Daire karesini** sayfası bir daire ve bir kare arasında bir rakam canlandırın. Daire koordinatlarının bu dizi tanımında ilk sütununda gösterilen dört Bézier eğrileri tarafından benzetilir [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) sınıfı:

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

İkinci sütunda, alanı yaklaşık dairenin alanının aynı olan bir kare tanımlayan dört Bézier eğrileri içerir. (Çizim ile bir kare *tam* alanıdır verilmiş bir daire olarak Klasik çözümlenemeyen geometrik sorununu [daire karesini](https://en.wikipedia.org/wiki/Squaring_the_circle).) Bir kare Bézier eğrileri ile işleme için her eğri için iki denetim noktaları aynıdır ve Bézier eğri düz bir çizgi işlenebilmesi başlangıç ve bitiş noktalarını ile colinear oldukları.

Üçüncü sütunda dizinin bir animasyonun ilişkilendirilmiş değerleri içindir. Sayfanın bir zamanlayıcı 16 milisaniye cinsinden ayarlar ve `PaintSurface` işleyicisi hızı çağrılır:

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

Sinusoidally salınım yapan bir değeri temel alınarak noktaları ilişkilendirilmiş `t`. İlişkilendirilmiş noktaları, ardından dört bağlı Bézier eğrileri bir dizi oluşturmak için kullanılır. Bir daire kare ilerlemesini gösteren üç platformlarda çalışan animasyonu şu şekildedir:

[![](beziers-images/squaringthecircle-small.png "Üç ekran görüntüsü Squaring daire sayfanın")](beziers-images/squaringthecircle-large.png#lightbox "üç ekran görüntüsü Squaring daire sayfası")

Böyle bir animasyonu dairesel yaylara ve düz çizgiler işlenecek algorithmically esnektir eğrileri olmadan mümkün olacaktır.

**Bezier sonsuz** sayfa da yararlanır Bézier eğriyi dairesel bir yay yaklaşık olanağı. İşte `PaintSurface` işleyicisinden [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) sınıfı:

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

Bu koordinatları birbirleriyle nasıl ilişkili olduğunu görmek için graf kağıda çizmek için iyi bir alıştırma olabilir. Sonsuz oturum (0, 0) nokta çevresinde ortalanır ve iki döngüler merkezlerinin sahip (–150, 0) ve (150, 0) ve 100 yarıçaplarını. Dizi `CubicTo` komutları, gördüğünüz –95 ve –205 değerleri üzerinde alma denetim noktalarının koordinatları X (Bu değerler –150 artı ve eksi 55), 205 ve 95 (150 artı ve eksi 55) yanı sıra 250 ve –250 sağ ve sol kenarı için. Tek özel durum, sonsuzluk oturum kendisini Merkezi'nde aştığında andır. Bu durumda, kontrol noktası koordinatları 50 ve – 50 eğrinin ortasında kullanıma düzeltmek için birlikte bulunur.

Üç tüm platformlarda sonsuz oturum şöyledir:

[![](beziers-images/bezierinfinity-small.png "Üçlü sayfasının ekran görüntüsü Bézier sonsuz")](beziers-images/bezierinfinity-large.png#lightbox "Bézier sonsuz sayfanın üç ekran görüntüsü")

Tarafından işlenen sonsuzluk işareti merkezi biraz daha yumuşak **yay sonsuz** gelen sayfasında [ **yay çizmenin üç yolu** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) makalesi.

## <a name="the-quadratic-bzier-curve"></a>İkinci dereceden Bézier eğrisi

İkinci dereceden Bézier eğri yalnızca bir denetim noktası vardır ve yalnızca üç noktayla eğri tanımlanır: başlangıç noktasını, denetim noktası ve bitiş noktası. İkinci dereceden bir polinom eğri, bu nedenle, 2, en yüksek üs olması dışında parametreli denklemler üçüncü dereceden Bézier eğriye oldukça benzerdir:

x(t) = (1-t) ²x₀ + 2t (1-t) x₁ + t²x₂

y(t) = (1-t) ²y₀ + 2t (1-t) y₁ + t²y₂

Bir yol Bézier ikinci dereceden bir eğri eklemek için [ `QuadTo` ](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint)) yöntemi veya [ `QuadTo` ](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single)) aşırı yüklemesi olan ayrı `x` ve `y` koordinatları:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Yöntemleri için geçerli konumdan bir eğri eklemek `point2` ile `point1` denetim noktası olarak.

İkinci dereceden Bézier Eğriler deneyimleyebilirsiniz **ikinci dereceden bir eğri** çok benzer sayfasında **Bezier eğrisi** yalnızca üç dokunma sahip dışında sayfa. İşte `PaintSurface` işleyicisinde [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) arka plan kod dosyası:

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

Ve burada üç tüm platformlarda çalışıyor:

[![](beziers-images/quadraticcurve-small.png "Üçlü sayfasının ekran görüntüsü ikinci dereceden bir eğri")](beziers-images/quadraticcurve-large.png#lightbox "ikinci dereceden bir eğri sayfanın üç ekran görüntüsü")

Noktalı çizgilerin tanjantı eğrinin başlangıç noktası ve bitiş noktası için olan ve denetim noktasına karşılamak.

İkinci dereceden Bézier genel bir şeklin eğri gerekir, ancak yalnızca bir denetim noktası yerine iki kolaylık tercih uygundur. İkinci dereceden Bézier, dahili Skia elips yaylar işlemek için kullanılan neden olan tüm diğer eğrisi değerinden daha verimli bir şekilde işler.

Ancak, ikinci dereceden Bézier eğrinin şeklini birden çok ikinci dereceden Béziers elips yay yaklaşık olarak belirlemenizi sağlayan gerekli olmasının nedeni de budur elips, değildir. İkinci dereceden Bézier bir parabol bir parçası olur.

## <a name="the-conic-bzier-curve"></a>Conic Bézier eğrisi

Conic Bézier eğrisi &mdash; olarak da bilinen rasyonel ikinci dereceden Bézier eğrisi &mdash; Bézier eğrileri ailesinin görece son ektir. İkinci dereceden Bézier eğri gibi bir başlangıç noktası, bir uç noktası ve bir denetim noktası rasyonel ikinci dereceden Bézier eğrisi gerektirir. Ayrıca rasyonel ikinci dereceden Bézier eğrisi gerektirir, ancak bir *ağırlık* değeri. Çağrıldığı bir *rasyonel* parametrik formülleri oranları hatalarıyla ilgili olduğundan ikinci dereceden.

Parametreli denklemler X ve Y aynı paydası paylaşan oranları için. İşte paydası denklemi *t* 0 ile arasında değişen 1 ve bir ağırlık değeri *w*:

d(t) = (1 – t)² + 2wt(1 – t) + t²

Teoride, üç ayrı ağırlık değerleri her üç terimlerin bir rasyonel dereceden içerebilir ancak bu yalnızca bir ağırlık değeri orta terimi için basitleştirilebilir.

Orta terimi ağırlık değeri de içerir ve ifade bölündüğünde dışında X ve Y koordinatları için parametreli denklemler ikinci dereceden Bézier için parametreli denklemler benzerdir:

x(t) = ((1 – t) ²x₀ + 2wt (1-t) x₁ + t²x₂)) bölü; d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1-t) y₁ + t²y₂)) bölü; d(t)

İkinci dereceden rasyonel Bézier eğrileri da verilir *conics* herhangi bir conic bölümü parçalarını tam olarak temsil edebilir çünkü &mdash; hiperboller, paraboller, elips ve daireler.

İkinci dereceden bir rasyonel Bézier eğri için bir yol eklemek için [ `ConicTo` ](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) yöntemi veya [ `ConicTo` ](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single)) aşırı yüklemesi olan ayrı `x` ve `y` koordinatları:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

En son fark `weight` parametresi.

**Conic eğri** sayfası bu Eğriler denemenizi sağlar. `ConicCurvePage` Sınıf türetilir `InteractivePage`. [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) dosya örnekleyen bir `Slider` – 2. ve 2 arasındaki bir ağırlık değeri seçin. [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) arka plan kod dosyası oluşturur üç `TouchPoint` nesneleri ve `PaintSurface` işleyici yalnızca sonuç eğri denetim Eğim satırlarla işler noktaları:

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

Burada üç tüm platformlarda çalıştığı:

[![](beziers-images/coniccurve-small.png "Üçlü sayfasının ekran görüntüsü Conic eğri")](beziers-images/coniccurve-large.png#lightbox "Conic eğri sayfanın üç ekran görüntüsü")

Gördüğünüz gibi denetim noktası daha doğru eğri ağırlığını daha yüksek olduğunda çekme gibi görünüyor. Ağırlık sıfır eğri uç noktasına düz bir çizgi başlangıç noktasından olur.

Teoride, negatif ağırlık verilir ve Eğ eğrinin neden *hemen* denetim noktasından. Ancak, belirli değerleri için negatif olmasını parametreli denklemler içinde paydası getirilen-1 veya nedeni aşağıda ağırlık verme *t*. Büyük olasılıkla bu nedenle negatif ağırlıkları göz ardı edilmektedirler `ConicTo` yöntemleri. **Conic eğri** program negatif ağırlıkları ayarlamanızı sağlar, ancak oynamalar yaparak görebileceğiniz gibi negatif ağırlıkları ağırlık sıfır ile aynı etkiye sahip ve işlenecek düz bir çizgi neden.

Denetim noktası ve kullanılacak ağırlık türetmek çok kolaydır `ConicTo` dairesel bir yay kadar çizmek için yöntemi (ancak değil de dahil olmak üzere) bir Yarım Daireli. Aşağıdaki diyagramda, başlangıç ve bitiş noktalarını Eğim satırlarından denetim noktasına karşılayın.

![](beziers-images/conicarc.png "Dairesel yay conic yay işlenmesi")

Trigonometri dairenin merkezi denetim noktasından uzaklığı belirlemek için kullanabilirsiniz: Açının kosinüsü yarım α tarafından ayrılmış halka yarıçapı öyledir. Başlangıç ve bitiş noktaları arasında dairesel bir yay çizmek için bu aynı yarı açının kosinüsünü için ağırlık ayarlayın. 180 derece açı, ardından hiçbir zaman Eğim satırları karşılamak ve Ağırlık sıfır olduğuna dikkat edin. Ancak açıları değerinden için 180 derece, matematik düzgün çalışır.

**Conic dairesel yay** sayfası bu gösterir. [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) dosya örnekleyen bir `Slider` açı seçme. `PaintSurface` İşleyicisinde [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) arka plan kod dosyası hesaplar denetim noktası ve ağırlığı:

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

Gördüğünüz gibi görsel arasında fark yoktur `ConicTo` kırmızıyla gösterilen yolu ve görüntülenen başvurusu için temel alınan daire:

[![](beziers-images/coniccirculararc-small.png "Üçlü sayfasının ekran görüntüsü Conic dairesel yay")](beziers-images/coniccirculararc-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Conic dairesel yay")

Ancak, açı 180 derece ve matematik başarısız olarak ayarlayın.

Bu durumda talihsiz, `ConicTo` negatif ağırlıkları desteklemez (üzerinde parametreli denklemler göre) teorik olarak, başka bir çağrı ile daire tamamlanabilir çünkü `ConicTo` aynı noktalarını ancak ağırlığı negatif bir değer. Bu tam bir daire ile iki oluşturmaya izin `ConicTo` eğrileri, her açı arasında (ancak dahil değil) temelinde derece ve 180 derece sıfır.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
