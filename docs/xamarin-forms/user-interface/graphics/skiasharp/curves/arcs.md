---
title: Yay çizmenin üç yolu
description: Bu makalede, üç farklı yolla yaylar tanımlamak için SkiaSharp kullanmayı açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: e862a663b35124c1470ae5239c93409c298b19ba
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615411"
---
# <a name="three-ways-to-draw-an-arc"></a>Yay çizmenin üç yolu

_SkiaSharp yaylar üç farklı yolla tanımlamak için kullanmayı öğrenin_

Yay bu sonsuz oturum yuvarlatılmış bölümlerini gibi bir elipsin çevresi Eğride şöyledir:

![](arcs-images/arcsample.png "Sonsuz oturum")

Bu tanım basitliği rağmen tüm gereksinimlerini karşılayan bir yay çizmenin işlevi tanımlamak için bir yolu yoktur ve bu nedenle, hiçbir fikir birliğine varılmış yay çizmenin en iyi yolu, grafik sistemler arasında. Bu nedenle, `SKPath` sınıfı kısıtlamaz kendisi için yalnızca bir yaklaşım.

`SKPath` tanımlayan bir `AddArc` yöntemi, beş farklı `ArcTo` yöntemleri ve iki göreli `RArcTo` yöntemleri. Bu yöntemler üç çok farklı yaklaşımlar yay belirtmeye temsil eden üç kategoriye ayrılır. Kullanacağınız bir Yayı Yaya Bu çizim yaptığınız diğer grafikleri oturum uyarlama ve tanımlamak mevcut olan bilgiler bağlıdır.

## <a name="the-angle-arc"></a>Açı yay

Yaylara çizim açı yay yaklaşım, bir elips sınırların bir dikdörtgen belirtmesini gerektirir. Yay bu elipsin çevresi üzerine elips yay ve uzunluğunu başlangıç yapma ortasından açıları belirtilir. İki farklı yöntemle açı yaylar çizin. Bunlar [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) yöntemi ve [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) yöntemi:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Bu yöntemler için Android özdeş [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) ve [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) yöntemleri. İOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) yöntemi benzer ancak yaylar dairenin çevresi üzerinde sınırlı yerine, genelleştirilmiş bir elips.

Her iki yöntem ile başlayan bir `SKRect` konumunu ve boyutunu elips tanımlayan değeri:

![](arcs-images/anglearcoval.png "Açı yay başlayan oval")

Yayı çevresi bu elipsin parçasıdır.

`startAngle` Olmayan bir açıyı derece elips Merkezi'nden sağa yatay çizgi göre bağımsız değişken. `sweepAngle` Bağımsız değişkeni olan göreli `startAngle`. İşte `startAngle` ve `sweepAngle` 60 ve 100 derece değerlerini sırasıyla:

![](arcs-images/anglearcangles.png "Açı yay tanımlama açısı")

Yayı başlangıç açıyla başlar. Uzunluğu, tarama açısı tarafından yönetilir:

![](arcs-images/anglearchighlight.png "Vurgulanan açı yay")

Yol ile eklenen eğri `AddArc` veya `ArcTo` elips'ın çevresini, burada kırmızıyla gösterilen yalnızca söz konusu bölümünün yöntemdir:

![](arcs-images/anglearc.png "Tek başına açı yay")

`startAngle` Veya `sweepAngle` bağımsız değişken negatif olabilir: Yay pozitif değerleri için saat yönünde `sweepAngle` ve yönünün negatif değerleri.

Ancak, `AddArc` mu *değil* kapalı dağılımı tanımlayın. Eğer `LineTo` sonra `AddArc`, bir çizgi noktasına Yayı sonundan çizilir `LineTo` yöntemi ve aynı doğru `ArcTo`.

`AddArc` otomatik olarak yeni bir dağılımı başlar ve bir çağrı işlevsel olarak eşdeğer `ArcTo` son bir bağımsız değişkeni ile `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Son bağımsız değişken olarak adlandırılan `forceMoveTo`, ve etkili bir şekilde neden bir `MoveTo` Yayı başında çağırın. Bu yeni dağılımı başlar. Diğer bir deyişle durum ile bir son bağımsız değişkeninin `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Bu sürümü `ArcTo` Yayı başlangıcına geçerli konumu bir çizgi çizer. Bu, Yayı ortasında daha büyük bir dağılımı yere olabileceği anlamına gelir.

**Açı yay** sayfa başlangıç belirtmek ve tarama açısı için iki kaydırıcıları kullanmanıza imkan tanır. İki XAML dosyası başlatır `Slider` öğeleri ve bir `SKCanvasView`. `PaintCanvas` İşleyicisinde [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) dosyası oval hem iki kullanarak Yayı çizer `SKPaint` alanları olarak tanımlanan nesneler:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

Gördüğünüz gibi hem Başlangıç açısı hem de tarama açısı negatif değerleri alabilir:

[![](arcs-images/anglearc-small.png "Üçlü sayfasının ekran görüntüsü açı yay")](arcs-images/anglearc-large.png#lightbox "Üçlü sayfasının ekran görüntüsü açı yay")

Yay oluşturma bu algorithmically en basit yaklaşımdır ve Yayı açıklayan parametreli denklemler türetmek kolaydır. Elips ve başlangıç ve tarama açısı konumunu ve boyutunu bilerek, başlangıç ve bitiş noktaları Yayı basit Trigonometri kullanılarak hesaplanabilir:

x oval =. MidX + (oval. Genişliği / 2) * cos(angle)

y oval =. MidY + (oval. Yükseklik / 2) * sin(angle)

`angle` Değerdir ya da `startAngle` veya `startAngle + sweepAngle`.

Yay tanımlamak için iki açıları kullanımını çizme, örneğin, bir pasta grafiğinin yapmak istediğiniz yay angular uzunluğunu bildiğiniz durumlar için idealdir. **Ayrılmış pasta grafiği** sayfası bu gösterir. [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Sınıfı, bazı üretilmiş veri ve renkler tanımlamak için bir iç sınıf kullanır:

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

`PaintSurface` İşleyici ilk hesaplamak için öğeler arasında döngü bir `totalValues` sayı. Bu listeden, toplam kesirle her öğenin boyutunu belirlemek ve bu bir açısına çevir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

Yeni bir `SKPath` nesnesi, her bir pasta diliminin oluşturulur. Merkezi satırından yolu oluşan bir `ArcTo` geri Yayı ve başka bir satır merkezi sonuçlardan çizmek için `Close` çağırın. Bu program, her şeyi Merkezi'nden 50 piksel taşıyarak "ayrılmış" pasta dilimi görüntüler. Bu görev için her bir dilimi tarama açısı Orta yönünde bir vektör gerektirir:

[![](arcs-images/explodedpiechart-small.png "Üçlü sayfasının ekran görüntüsü ayrılmış pasta grafiği")](arcs-images/explodedpiechart-large.png#lightbox "Üçlü sayfasının ekran görüntüsü ayrılmış pasta grafiği")

Bunu "patlama" nasıl göründüğünü görmek için yalnızca açıklama `Translate` çağırın:

[![](arcs-images/explodedpiechartunexploded-small.png "Üçlü sayfasının ekran görüntüsü ayrılmış pasta grafiği açılımı olmadan")](arcs-images/explodedpiechartunexploded-large.png#lightbox "Üçlü sayfasının ekran görüntüsü ayrılmış pasta grafiği açılımı olmadan")

## <a name="the-tangent-arc"></a>Eğim yay

İkinci yay tarafından desteklenen türünü `SKPath` olduğu *Ark tanjant*, Yayı çevresi tanjantı iki bağlı çizgiler için olan bir dairenin alanının olduğundan, bu nedenle çağrılır.

Bir Eğim yay çağrısı ile bir yol eklenir [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) iki yöntemle `SKPoint` parametreleri veya [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) aşırı yüklemesi olan ayrı `Single` parametreleri noktaları:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

Bu `ArcTo` yöntemi için PostScript'i benzer [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (sayfa 532 PDF belgesinde) işlevi ve iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) yöntemi.

`ArcTo` Yöntemi, üç nokta içerir:

- Geçerli, dağılımı veya noktası (0, 0), işaret `MoveTo` çağrılmadıysa
- İlk nokta bağımsız değişkenine `ArcTo` adlı yöntem *köşe noktası*
- İkinci noktası bağımsız değişkeni `ArcTo`adlı *hedef noktası*:

![](arcs-images/tangentarcthreepoints.png "Bir Eğim yay başlayan üç nokta")

Bu üç nokta, iki bağlı satırlarını tanımlayın:

![](arcs-images/tangentarcconnectinglines.png "Bir Eğim yay üç noktalarını bağlı çizgileri")

Üç nokta colinear varsa &mdash; diğer bir deyişle, aynı çizgide üzerinde aracılarla varsa &mdash; hiçbir yay çizileceğini.

`ArcTo` Yöntemi de içeren bir `radius` parametresi. Bu, RADIUS dairenin tanımlar:

![](arcs-images/tangentarccircle.png "Bir Eğim yay dairesinin")

Eğim yay için bir elips genelleştirilmiş değil.

Aşağıdaki iki satırı herhangi bir açıyla karşılıyorsanız, teğet iki satır olması, daire bu satırlar arasında eklenebilir:

![](arcs-images/tangentarctangentcircle.png "Aşağıdaki iki satırı arasındaki Ark tanjant daire")

Eğrinin için dağılımı eklenir ya da belirtilen noktaları dokunmaz `ArcTo` yöntemi. İlk Eğim noktası ve ikinci Eğim noktada biten yay düz bir çizgi zamandaki geçerli noktadan oluşur:

![](arcs-images/tangentarchighlight.png "İki satır arasında vurgulanan Eğim yay")

Son düz çizgi ve dağılımı için eklenen yay aşağıda verilmiştir:

![](arcs-images/tangentarc.png "İki satır arasında vurgulanan Eğim yay")

Dağılımı ikinci Eğim noktadan devam edebilir.

**Ark tanjant** sayfası Eğim yay denemenizi sağlar. Öğesinden türetilen birkaç sayfadaki ilk budur [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs), kullanışlı birkaç tanımlayan `SKPaint` nesneleri ve gerçekleştiren `TouchPoint` işleme:

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

`TangentArcPage` Sınıf türetilir `InteractivePage`. Oluşturucuda [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) dosyasıdır örnekleme ve başlatma sorumlu `touchPoints` dizisi ve ayarı `baseCanvasView` (içinde `InteractivePage`) için `SKCanvasView` nesnesi örneği de [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) dosyası:

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

`PaintSurface` İşleyicisi kullanır `ArcTo` yay çizmenin yöntemine temel dokunma noktalarında ve `Slider`, ancak ayrıca algorithmically açı dayanır daire hesaplar:

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
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
}
```

İşte **Ark tanjant** üç tüm platformlarda çalışan sayfası:

[![](arcs-images/tangentarc-small.png "Üçlü sayfasının ekran görüntüsü Ark tanjant")](arcs-images/tangentarc-large.png#lightbox "Üçlü sayfasının ekran görüntüsü tanjantı yay")

Eğim Yayı bir Yuvarlatılmış Dikdörtgen gibi yuvarlatılmış köşeler oluşturmak için idealdir. Çünkü `SKPath` zaten içeren bir `AddRoundedRect` yöntemi **Heptagon yuvarlatılmış** sayfasını nasıl kullanılacağını gösterir `ArcTo` yedi taraflı bir Çokgen köşelerini yuvarlama. (Kod için normal bir Çokgen genelleştirildiğinden.)

`PaintSurface` İşleyicisi [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) sınıfı içeren bir `for` heptagon yanı sıra, bu yedi yüz sayısı başlığının hesaplamak için ikinci bir yedi köşelerini koordinatlarını hesaplamak için döngü Köşe. Bu başlığının ardından yolu oluşturmak için kullanılır:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

Üç platformlarda çalışan bir program şöyledir:

[![](arcs-images/roundedheptagon-small.png "Üçlü sayfasının ekran görüntüsü Heptagon yuvarlatılmış")](arcs-images/roundedheptagon-large.png#lightbox "Heptagon yuvarlatılmış sayfanın üç ekran görüntüsü")

## <a name="the-elliptical-arc"></a>Elips yay

Elips yay çağrısıyla bir yola eklenir [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) iki yöntemin `SKPoint` parametreleri veya [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) aşırı ayrı X ve Y koordinatları:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Elips yay tutarlıdır [elips yay](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) ölçeklenebilir vektör grafiği (SVG) ve evrensel Windows platformu ile birlikte [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) sınıfı.

Bunlar `ArcTo` yöntemleri geçerli dağılımı noktasıdır, iki nokta yay çizin ve en son parametreye `ArcTo` yöntemi ( `xy` parametresi veya ayrı `x` ve `y` parametreleri):

![](arcs-images/ellipticalarcpoints.png "Elips yay tanımlı iki nokta")

İlk noktası parametresi `ArcTo` yöntemi (`r`, veya `rx` ve `ry`) bir noktası hiç değil ancak bunun yerine bir elips; yatay ve dikey yarıçaplarını belirtir

![](arcs-images/ellipticalarcellipse.png "Elips yay tanımlı olan elipsin")

`xAxisRotate` Parametredir bu elips döndürmek için saat yönünde derece sayısı:

![](arcs-images/ellipticalarctiltedellipse.png "Elips yay tanımlanan Eğimli üç nokta")

Böylece iki nokta dokunduğu bu Eğimli elips sonra konumlandırılır, noktaları tarafından iki farklı yay bağlanır:

![](arcs-images/ellipticalarcellipse1.png "Elips yaylar ilk kümesi")

Bu iki yay iki yolla ayırt edilebilir: üst Yayı alt Yayı büyüktür ve Yayı soldan sağa çizilirken alt Yayı saat yönünün tersi yönde alınırken üst Yayı yönünde bir çizilir.

Başka bir şekilde iki nokta arasındaki elips uyacak şekilde mümkündür:

![](arcs-images/ellipticalarcellipse2.png "Elips yaylar ikinci kümesi")

Artık var. daha küçük bir yay saat yönünde çizilir üstte ve daha büyük bir yay çizilir alt yönünün

Bu iki nokta, bu nedenle bir toplam dört yoldan Eğimli elips tarafından tanımlanan bir yay tarafından bağlanabilir:

![](arcs-images/ellipticalarccolors.png "Tüm dört elips yaylar")

Bu dört yaylar dört birleşimlerini göre ayırt edilir [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) ve [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) sabit listesi türü bağımsız değişkenleri `ArcTo` yöntemi:

- kırmızı: SKPathArcSize.Large ve SKPathDirection.Clockwise
- Yeşil: SKPathArcSize.Small ve SKPathDirection.Clockwise
- Mavi: SKPathArcSize.Small ve SKPathDirection.CounterClockwise
- Eflatun: SKPathArcSize.Large ve SKPathDirection.CounterClockwise

Eşit büyüklükte olana kadar ölçeklendirilir Eğimli elips iki nokta sığmayacak kadar büyük değil ise. Bu durumda, yalnızca iki benzersiz yay iki nokta bağlanın. İle bu ayırt edilebilir `SKPathDirection` parametresi.

Yay tanımlamak için bu yaklaşım ilk karşılaştığınız karmaşık sesleri olsa da, bu, bir döndürülmüş bir elips yay tanımlanmasına olanak sağlar, yalnızca yaklaşımdır ve yaylar dağılımı diğer bölümlerini tümleştirmek, ihtiyacınız olduğunda en kolay yaklaşım genellikle olur.

**Elips yay** sayfası, iki nokta ve boyutunu ve rotasyonunu elipsin etkileşimli olarak ayarlamanıza olanak sağlar. `EllipticalArcPage` Sınıf türetilir `InteractivePage`ve `PaintSurface` işleyicisinde [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) arka plan kod dosyası dört yaylar çizer:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

Burada üç platformlarda çalıştığı:

[![](arcs-images/ellipticalarc-small.png "Üçlü sayfasının ekran görüntüsü elips yay")](arcs-images/ellipticalarc-large.png#lightbox "Üçlü sayfasının ekran görüntüsü elips yay")

**Yay sonsuz** sayfası elips yay sonsuz oturum çizmek için kullanır. Sonsuz oturum 100 birimi tarafından ayrılmış 100 birimi yarıçaplarını ile iki daire dayanır:

![](arcs-images/infinitycircles.png "İki daire")

Birbirine geçmeden iki satırı her iki daire teğet şunlardır:

![](arcs-images/infinitycircleslines.png "Eğim satırlarla iki daire")

Sonsuz oturum bölümlerini şu çevrelerdeki ve satırlarını birleşimidir. Elips yay sonsuz oturum çizmek için kullanmak için aşağıdaki iki satırı daireler teğet nerede koordinatları belirlenmesi gerekir.

Doğru dikdörtgene dairelerden birini oluşturun:

![](arcs-images/infinitytriangle.png "Eğim satırları ve katıştırılmış bir daire ile iki daire")

RADIUS dairenin 100 birim ve α açı 150 veya 41,8 derece bölünmüş 100 arksinüsünü (ters sinüsünü), bu nedenle bu üçgen hipotenüs 150 birimleri. Bu üçgen diğer tarafında uzunluğunu 41,8 derecenin kosinüsünü 150 kez ya da ayrıca Pisagor Teoremi tarafından hesaplanan 112 ' dir.

Eğim noktası koordinatları, ardından bu bilgileri kullanarak hesaplanabilir:

x 112·cos(41.8) = 83 =

y 112·sin(41.8) = 75 =

Ortalanmış bir sonsuz işareti ile daire yarıçaplarını 100 (0, 0) noktasında çizmek gerekli olan tüm dört Eğim noktaları şunlardır:

![](arcs-images/infinitycoordinates.png "Eğim satırları ve koordinatları ile iki daire")

`PaintSurface` İşleyicisinde [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) sınıfı konumlandırır sonsuz oturum böylece (0, 0) noktası sayfasının ortasında konumlandırılır ve ekran boyutu yolunu ölçeklendirir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
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

Kod `Bounds` özelliği `SKPath` tuval boyutu için ölçeklendirmek için sonsuz sinüs boyutunu belirlemek için:

[![](arcs-images/arcinfinity-small.png "Üçlü sayfasının ekran görüntüsü yay sonsuz")](arcs-images/arcinfinity-large.png#lightbox "yay sonsuz sayfanın üç ekran görüntüsü")

Hangi öneren sonucu biraz küçük görünüyor `Bounds` özelliği `SKPath` yoldan daha büyük bir boyut raporlama.

Dahili olarak kullanarak birden çok ikinci dereceden Bézier eğrileri yay Skia yaklaştırır. (Sonraki bölümde göreceğiniz gibi) Bu eğrileri eğri nasıl çizilmeden yöneten ancak işlenmiş eğri parçası olmayan denetim noktaları içerir. `Bounds` Özelliği bu denetim noktaları içerir.

Sıkı bir uyum sağlamak için kullanın `TightBounds` özelliğini kontrol noktalarını içermez. Yatay modda çalışan ve kullanarak program işte `TightBounds` yol sınırları edinme özelliği:

[![](arcs-images/arcinfinitytightbounds-small.png "Üçlü sayfasının ekran görüntüsü yay sonsuz sıkı sınırları")](arcs-images/arcinfinitytightbounds-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yay sonsuz sıkı sınırları")

Yay ve düz çizgiler arasındaki bağlantıları matematiksel olarak kesintisiz olsa da, değişiklik yay düz çizgi için biraz sert görünebilir. Sonraki sayfada daha iyi bir sonsuz oturum sunulur.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
