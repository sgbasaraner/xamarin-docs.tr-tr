---
title: Yay çizmek için üç yol
description: Üç farklı yolla yaylar tanımlamak için SkiaSharp kullanmayı öğrenin
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: 668b1f437b78535bd4cdf3bb3f80154dbf281a02
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="three-ways-to-draw-an-arc"></a>Yay çizmek için üç yol

_Üç farklı yolla yaylar tanımlamak için SkiaSharp kullanmayı öğrenin_

Yayı eğri bu sonsuz oturum yuvarlatılmış bölümlerini gibi elips çevresi üzerinde şöyledir:

![](arcs-images/arcsample.png "Sonsuz oturum")

Bu tanım kolaylık rağmen tüm gereksinimlerini karşılayan bir yay çizim işlevi tanımlamak için bir yolu yoktur ve bu nedenle, bir yay çizmek için en iyi yolu, grafik sistemleri arasında hiçbir anlaşma. Bu nedenle, `SKPath` sınıfı değil kısıtlamak kendisi için yalnızca bir yaklaşım.

`SKPath` tanımlayan bir `AddArc` yöntemi, beş farklı `ArcTo` yöntemleri ve iki göreli `RArcTo` yöntemleri. Bu yöntemleri üç çok farklı yaklaşımlar temsil eden bir yay belirtmek için üç kategorilere ayrılır. Kullanacağınız bir Yayı ve bu yay çizim yaptığınız diğer grafikleri oturum nasıl uyduğunu tanımlamak kullanılabilir bilgi bağlıdır.

## <a name="the-angle-arc"></a>Açı yay

Yaylar çizim açı yay yaklaşım elips bounds bir dikdörtgen belirtmenizi gerektirir. Yay bu elips çevresi üzerine merkezi yay ve uzunluğu başlangıcı yapma elipsin açıları belirtilir. İki farklı yöntem açı yaylar çizin. Bunlar [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) yöntemi ve [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) yöntemi:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Bu yöntemler için Android özdeş [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) ve [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) yöntemleri. İOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) yöntemi benzer ancak dairenin çevresi üzerinde yaylar sınırlandırılır yerine elips genelleştirilmiş.

Her iki yöntem ile başlayan bir `SKRect` konum ve elips boyutunu tanımlayan değeri:

![](arcs-images/anglearcoval.png "Bir açının yay başlar oval")

Yayı bu elips çevresi parçasıdır.

`startAngle` Değişkendir elips Merkezi'nden sağa yatay çizgi göre derece saat yönünde açı. `sweepAngle` Değişkendir göreli `startAngle`. Burada `startAngle` ve `sweepAngle` 60 ve 100 derece değerleri sırasıyla:

![](arcs-images/anglearcangles.png "Bir açının yay tanımlamak açıları")

Yayı başlangıç açıyla başlar. Uzunluğu tarama açısı tarafından yönetilir:

![](arcs-images/anglearchighlight.png "Vurgulanan açı yay")

Yolu eklenen eğri `AddArc` veya `ArcTo` yöntemi parçasıdır yalnızca o elips 's çevresini, burada kırmızı olarak gösterilir:

![](arcs-images/anglearc.png "Tek başına açı yay")

`startAngle` Veya `sweepAngle` bağımsız değişken negatif olamaz: Yay pozitif değerleri için saat yönünde `sweepAngle` ve negatif değerler için yönünün.

Ancak, `AddArc` mu *değil* kapalı dağılımı tanımlayın. Çağırırsanız `LineTo` sonra `AddArc`, bir çizgi noktaya Yayı sonundan çizilir `LineTo` yöntemi, aynı ise true, `ArcTo`.

`AddArc` otomatik olarak yeni bir dağılım başlar ve bir çağrı işlevsel olarak eşdeğerdir `ArcTo` bir son bağımsız değişkeni ile `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Son bağımsız değişken olarak adlandırıldığını `forceMoveTo`, ve etkili bir şekilde durdurulmasına neden bir `MoveTo` Yayı başında çağırın. Bu yeni dağılımı başlar. Bu durum bir son bağımsız değişkeni ile `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Bu sürümü `ArcTo` bir çizgi geçerli konumundan Yayı başlangıcına çizer. Bu, Yayı daha büyük bir dağılım ortasında bir yerde olabileceği anlamına gelir.

**Açı yay** sayfa açıları sweep ve başlangıç belirtmek için iki kaydırıcılar kullanmanıza imkan tanır. İki XAML dosyası başlatır `Slider` öğeleri ve bir `SKCanvasView`. `PaintCanvas` İşleyicisinde [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) dosya çizer oval ve iki kullanarak yay `SKPaint` alanlar olarak tanımlı nesneler:

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

Gördüğünüz gibi Başlangıç açısı ve tarama açısı negatif değerleri temel alabilir:

[![](arcs-images/anglearc-small.png "Üçlü sayfasının ekran görüntüsü açı yay")](arcs-images/anglearc-large.png#lightbox "Üçlü sayfasının ekran görüntüsü açı yay")

Yayı oluşturmak için bu yaklaşım algorithmically en kolayıdır ve Yayı açıklamak parametrik denklemini türetilen kolaydır. Elips ve başlangıç ve tarama açıları konumunu ve boyutunu bilerek, başlangıç ve bitiş noktaları Yayı basit Trigonometri kullanılarak hesaplanabilir:

x oval =. MidX + (oval. Genişlik / 2) * cos(angle)

y oval =. MidY + (oval. Yükseklik / 2) * sin(angle)

`angle` Değerdir ya da `startAngle` veya `startAngle + sweepAngle`.

Yayı tanımlamak için iki açıları kullanımını çizin, örneğin bir pasta grafiği yapmak amacıyla, istediğiniz Yayı Açısal uzunluğu burada bildiğiniz durumlarda en iyisidir. **Ayrılıp pasta grafik** sayfa bu gösterir. [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Sınıfı, bazı üretilmiş veri ve renkleri tanımlamak için bir iç sınıf kullanır:

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

`PaintSurface` İşleyici ilk hesaplamak için öğeler arasında döngü bir `totalValues` numarası. Bu listeden, her öğenin boyutunu toplam kesir olarak belirleyebilir ve bir açının Dönüştür:

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

Yeni bir `SKPath` nesnesi, her pasta dilimi için oluşturulur. Merkezi satırından yolu oluşan bir `ArcTo` geri Yayı ve başka bir satır merkezi sonuçlarından çizmek için `Close` çağırın. Bu program, her şeyi Merkezi'nden 50 piksel taşıyarak "ayrılmış" pasta dilimi görüntüler. Bu görev için her bir dilim tarama açısı Orta yön vektörü gerektirir:

[![](arcs-images/explodedpiechart-small.png "Üçlü sayfasının ekran görüntüsü ayrılıp pasta grafik")](arcs-images/explodedpiechart-large.png#lightbox "Üçlü sayfasının ekran görüntüsü ayrılıp pasta grafiği")

Nasıl "açılımı" göründüğünü görmek için çıkış yalnızca açıklama `Translate` çağırın:

[![](arcs-images/explodedpiechartunexploded-small.png "Üçlü sayfasının ekran görüntüsü ayrılıp pasta grafiği açılımı olmadan")](arcs-images/explodedpiechartunexploded-large.png#lightbox "Üçlü sayfasının ekran görüntüsü ayrılıp pasta grafiği açılımı olmadan")

## <a name="the-tangent-arc"></a>Eğim yay

Tarafından desteklenen yay ikinci türü `SKPath` olan *Eğim yay*, Yayı iki bağlı satırlarına tanjantı dairenin çevresi olduğundan bu nedenle çağrılır.

Bir Eğim yay çağrısıyla bir yola eklenir [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) iki yöntemle `SKPoint` parametreleri veya [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) ayrı ile aşırı `Single` parametrelerini noktalar:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

Bu `ArcTo` yöntemi PostScript'i benzer [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) (PDF belgedeki sayfayı 532) işlevi ve iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) yöntemi.

`ArcTo` Yöntemi, üç nokta içerir:

- Geçerli, dağılım veya (0, 0) noktası varsa noktası `MoveTo` çağrılmadıysa
- İlk noktası bağımsız değişkenine `ArcTo` adlı yöntemi *köşe noktası*
- İkinci noktası bağımsız değişkeni `ArcTo`adlı *hedef noktası*:

![](arcs-images/tangentarcthreepoints.png "Bir Eğim yay başlamak üç nokta")

Bu üç nokta iki bağlı satırları tanımlayın:

![](arcs-images/tangentarcconnectinglines.png "Bir Eğim yay üç noktalarının bağlı çizgileri")

Üç noktalar colinear ise &mdash; diğer bir deyişle, aynı düz satırda kalan varsa &mdash; hiçbir yay çizileceğini.

`ArcTo` Yöntemi de içeren bir `radius` parametresi. Bu, bir daire RADIUS tanımlar:

![](arcs-images/tangentarccircle.png "Bir Eğim yay daire")

Eğim Yayı elips için genelleştirilmiş değil.

İki satır herhangi açıyla karşılıyorsa, iki satır teğet olmasını sağlamak, daire arasındaki bu satırları eklenebilir:

![](arcs-images/tangentarctangentcircle.png "İki satır arasında Eğim yay daire")

Dağılımı eklenen eğri ya da belirtilen noktaları dokunmaz `ArcTo` yöntemi. Geçerli noktasından bir çizgide ilk teğet noktası ve ikinci Eğim noktada biten yay oluşur:

![](arcs-images/tangentarchighlight.png "İki satır arasında vurgulanan Eğim yay")

Son çizgide ve dağılım eklenen arc şöyledir:

![](arcs-images/tangentarc.png "İki satır arasında vurgulanan Eğim yay")

Dağılımı ikinci Eğim noktasından devam edebilir.

**Tanjantını yay** sayfası Eğim yay denemeler sağlar. Öğesinden türetilen birkaç sayfa ilk budur [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/InteractivePage.cs), birkaç kullanışlı tanımlayan `SKPaint` nesneleri ve gerçekleştirir `TouchPoint` işleme:

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

`TangentArcPage` Sınıfı türer `InteractivePage`. Oluşturucuda [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) dosyasıdır örnek oluşturma ve başlatma sorumlu `touchPoints` dizi ve ayarı `baseCanvasView` (içinde `InteractivePage`) için `SKCanvasView` nesne örneği [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) dosyası:

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

`PaintSurface` İşleyici kullanan `ArcTo` tabanlı dokunma noktalarında Yayı çizmek için yöntemini ve bir `Slider`, ancak aynı zamanda algorithmically açı dayanır daire hesaplar:

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

Burada **tanjantını yay** üç tüm platformlarda çalışan sayfa:

[![](arcs-images/tangentarc-small.png "Üçlü sayfasının ekran görüntüsü tanjantını yay")](arcs-images/tangentarc-large.png#lightbox "Üçlü sayfasının ekran görüntüsü tanjantını yay")

Windows mobil aygıttaki üç nokta neredeyse colinear ve Yayı çok küçük.

Eğim Yayı yuvarlak dikdörtgen gibi yuvarlak köşeleri oluşturmak için idealdir. Çünkü `SKPath` zaten içeren bir `AddRoundedRect` yöntemi, **yuvarlanmasını Heptagon** sayfa nasıl kullanılacağı gösterilmektedir `ArcTo` yedi taraflı Çokgen köşelerinde yuvarlama. (Kod için normal bir Çokgen genelleştirilmiş.)

`PaintSurface` İşleyicisine [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) sınıfı içeren bir `for` heptagon ve bunlar yedi taraftan, Orta noktalar hesaplamak için ikinci bir yedi köşe koordinatlarını hesaplamak için döngü Köşeleri. Bu orta noktalar sonra yolu oluşturmak için kullanılır:

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

Üç platformlarda çalışan program şöyledir:

[![](arcs-images/roundedheptagon-small.png "Üçlü sayfasının ekran görüntüsü yuvarlanmış Heptagon")](arcs-images/roundedheptagon-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yuvarlanmış Heptagon")

## <a name="the-elliptical-arc"></a>Elips yay

Elips yay çağrısıyla bir yola eklenir [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) iki sahip yöntemi `SKPoint` parametreleri veya [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) aşırı ayrı X ve Y koordinatları:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Elips yay tutarlıdır [elips yay](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) ölçeklenebilir vektör grafikleri (SVG) ve evrensel Windows platformu dahil [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) sınıfı.

Bunlar `ArcTo` yöntemleri geçerli dağılımı noktasıdır, iki nokta arasında bir yay çizme ve son parametre `ArcTo` yöntemi ( `xy` parametresi ya da ayrı `x` ve `y` parametreleri):

![](arcs-images/ellipticalarcpoints.png "Elips yay tanımlanan iki nokta")

İlk noktası parametre olarak `ArcTo` yöntemi (`r`, veya `rx` ve `ry`) bir noktası hiç değil, ancak bunun yerine bir elips; yatay ve dikey yarıçaplarını belirtir

![](arcs-images/ellipticalarcellipse.png "Elips yay tanımlanan elips")

`xAxisRotate` Parametresi, bu elips döndürmek için saat yönünde derece sayısı:

![](arcs-images/ellipticalarctiltedellipse.png "Elips yay tanımlanan Eğimli elips")

İki nokta dokunur böylece bu Eğimli elips sonra konumlandırılmış, noktaları tarafından iki farklı yaylar bağlanır:

![](arcs-images/ellipticalarcellipse1.png "Elips yaylar ilk kümesi")

Bu iki yaylar iki yolla ayırt edilebilir: üst Yayı alt Yayı büyüktür ve Yayı soldan sağa çizilirken alt yay yönünün yönde alınırken üst Yayı bir yönünde çizilir.

Başka bir şekilde iki nokta arasında elips uyacak şekilde mümkündür:

![](arcs-images/ellipticalarcellipse2.png "Elips yaylar ikinci kümesi")

Artık yoktur saat yönünde çizilmiş üstte daha küçük bir yay ve daha büyük bir yay çizilir alt yönünün.

Bu iki nokta, bu nedenle toplam dört şekilde Eğimli elips tarafından tanımlanan bir yay tarafından bağlanabilir:

![](arcs-images/ellipticalarccolors.png "Tüm dört elips yaylar")

Bu dört yaylar dört birleşimlerini göre ayırt edilen [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) ve [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) numaralandırma türü bağımsız değişkenleri `ArcTo` yöntemi:

- kırmızı: SKPathArcSize.Large ve SKPathDirection.Clockwise
- Yeşil: SKPathArcSize.Small ve SKPathDirection.Clockwise
- Mavi: SKPathArcSize.Small ve SKPathDirection.CounterClockwise
- macenta: SKPathArcSize.Large ve SKPathDirection.CounterClockwise

Eğimli elips arasında iki nokta sığmayacak kadar büyük değil, büyüklükte kadar ardından onu aynı şekilde ölçeklendirilir. Bu durumda, yalnızca iki benzersiz yaylar iki nokta bağlayın. Bunlar birlikte ayırt edilebilen `SKPathDirection` parametresi.

Yayı tanımlamak için bu yaklaşım ilk karşılaştığınız karmaşık sesleri olsa da döndürülmüş elips bir yay tanımlanmasına olanak tanır yalnızca yaklaşımdır ve yaylar dağılımı diğer bölümleri ile tümleştirmek gerektiğinde en kolay yaklaşım genellikle olur.

**Elips yay** sayfası etkileşimli olarak iki nokta ve boyutunu ve rotasyonunu elipsin ayarlamanıza olanak sağlar. `EllipticalArcPage` Sınıfı türer `InteractivePage`ve `PaintSurface` işleyicisinde [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) arka plan kod dosyasına çizer dört yaylar:

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

**Yay sonsuz** sayfası sonsuz oturum çizmek için elips yay kullanır. Sonsuz oturum 100 birimleri 100 birimler tarafından ayrılmış yarıçaplarını iki daireler dayanır:

![](arcs-images/infinitycircles.png "İki daire")

İki satır birbirine geçmesinden hem daireler teğet şunlardır:

![](arcs-images/infinitycircleslines.png "İki daire teğet çizgili")

Sonsuz oturum bu daire ve iki satır bölümlerini birleşimidir. Sonsuz oturum çizmek için elips yay kullanmak için iki satırı daireleri teğet nerede koordinatları belirlenmesi gerekir.

Sağ dikdörtgene daireleri birini oluşturun:

![](arcs-images/infinitytriangle.png "İki daire teğet çizgili ve katıştırılmış daire")

RADIUS dairenin 100 birimdir ve α 150 veya 41,8 derece bölünmüş 100 arksinüsünü (ters sinüsünü) açıdır üçgen hipotenüsü 150 birimleri olduğundan. Üçgen diğer tarafını uzunluğu 41,8 derecenin kosinüsü 150 kez veya Pisagor Teoremi tarafından da hesaplanabilir 112 olabilir.

Eğim noktası koordinatları, ardından bu bilgileri kullanarak hesaplanabilir:

x = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

Dört teğet noktalarını ortalanmış bir sonsuz işareti (0, 0) noktasında 100 daire yarıçaplarını ile çizmek için gerekli olan tüm şunlardır:

![](arcs-images/infinitycoordinates.png "İki daire teğet çizgili ve koordinatları")

`PaintSurface` İşleyicisinde [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) sınıfı konumlandırır sonsuz oturum böylece (0, 0) noktası sayfasının ortasında yerleştirilir ve ekran boyutu yoluna ölçeklendirir:

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

Kod kullanan `Bounds` özelliği `SKPath` tuvale boyutunu ölçeklendirmek için sonsuz sinüsünü boyutlarını belirlemek için:

[![](arcs-images/arcinfinity-small.png "Üçlü sayfasının ekran görüntüsü yay sonsuz")](arcs-images/arcinfinity-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yay sonsuz")

Hangi öneren sonucu biraz küçük görünüyor `Bounds` özelliği `SKPath` yolun büyük bir boyutu raporlama.

Dahili olarak, birden çok ikinci derece Bézier eğrileri kullanarak yay Skia yakın. (Sonraki bölümde göreceğiniz gibi) bu Eğriler eğri nasıl çizilir yöneten ancak işlenmiş eğri parçası olmayan denetim noktaları içerir. `Bounds` Özelliği bu denetim noktaları içerir.

Sıkı bir sığdırma almak için `TightBounds` denetim noktaları dışlar özelliği. Yatay modunda çalışan ve kullanarak programı işte `TightBounds` yol sınırları almak için özellik:

[![](arcs-images/arcinfinitytightbounds-small.png "Üçlü sayfasının ekran görüntüsü yay sonsuz sıkı sınırları ile")](arcs-images/arcinfinitytightbounds-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yay sonsuz ile sıkı sınırları")

Düz çizgiler ve yaylar arasındaki bağlantıları matematiksel kesintisiz olsa da, arc değişikliği çizgide biraz ani görünebilir. Daha iyi sonsuz işareti sonraki sayfaya sunulur.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
