---
title: Yol bilgileri ve numaralandırması
description: Bu makalede SkiaSharp yolları hakkında bilgi almak ve içeriklerini listeleme anlatılmaktadır ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
ms.openlocfilehash: 6efefe11b31428f41bfa945aff93aa70aa764870
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615281"
---
# <a name="path-information-and-enumeration"></a>Yol bilgileri ve numaralandırması

_Yolları hakkında bilgi almak ve içeriğini listeleme_

[ `SKPath` ](xref:SkiaSharp.SKPath) Çeşitli özellikler ve yolu hakkında bilgi edinmenizi sağlayan yöntemler sınıfı tanımlar. [ `Bounds` ](xref:SkiaSharp.SKPath.Bounds) Ve [ `TightBounds` ](xref:SkiaSharp.SKPath.TightBounds) özellikleri (ve ilişkili yöntemleri) elde yol metrical boyutları. [ `Contains` ](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single)) Yöntemi, belirli bir noktaya bir yolda olup olmadığını belirlemenize olanak sağlar.

Bazen, tüm satırları ve bir yolu olun eğrileri toplam uzunluğu belirlemek yararlıdır. Bu süre hesaplama değil algorithmically basit bir görev sınıfının tümüne adlı şekilde [ `PathMeasure` ](xref:SkiaSharp.SKPathMeasure) için ayrılmıştır.

Ayrıca bazen çizim operations ve bir yolu noktalar elde etmek yararlıdır. İlk başta, bu özelliği gereksiz görünebilir: programınızı yolu oluşturduysa, program içeriğini zaten bilir. Yolları tarafından da oluşturulabilir ancak gördünüz [yol etkileri](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) ve dönüştürerek [yolları metin dizelerine](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Çizim işlemler ve bu yolların noktalar da elde edebilirsiniz. Bir olasılıktır algoritmik bir dönüşüm noktaları için örnek uygulama için metin bir yarım küre etrafında sarmalamak için:

![](information-images/pathenumerationsample.png "Üzerinde bir küreyi sarmalanmış metin")

## <a name="getting-the-path-length"></a>Yol uzunluğu alma

Bu makalede [ **yollar ve metin** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) kullanmayı öğrendiniz [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) bir metin dizesi olan temel izleyen bir yol boyunca çizmek için yöntemi. Ancak metin tam yolunu uygun şekilde boyutlandırmak istiyorsanız ne olur? Metin etrafında bir daire çizme dairenin çevresi hesaplamak basit olduğu için kolaydır. Ancak, bir elipsin çevresi veya Bézier eğrisi uzunluğu bu kadar basit değil.

[ `SKPathMeasure` ](xref:SkiaSharp.SKPathMeasure) Sınıfı yardımcı olabilir. [Oluşturucusu](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single)) kabul eden bir `SKPath` bağımsız değişken ve [ `Length` ](xref:SkiaSharp.SKPathMeasure.Length) özelliği uzunluğunu gösterir.

Bu sınıf gösterilmiştir **yol uzunluğu** temel örnek, **Bezier eğrisi** sayfası. [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) dosya türetilir `InteractivePage` ve Dokunmatik arayüzü içerir:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Curves.PathLengthPage"
                       Title="Path Length">
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</local:InteractivePage>
```

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) arka plan kod dosyasına uç noktaları tanımlamak ve üçüncü dereceden Bézier eğrinin noktaları denetlemek için dört dokunma taşımanıza olanak tanır. Üç alan bir metin dizesi tanımlayan bir `SKPaint` nesne ve hesaplanmış metnin genişliği:

```csharp
public partial class PathLengthPage : InteractivePage
{
    const string text = "Compute length of path";

    static SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Black,
        TextSize = 10,
    };

    static readonly float baseTextWidth = textPaint.MeasureText(text);
    ...
}
```

`baseTextWidth` Alandır göre metin genişliğini bir `TextSize` 10 ayarlama.

`PaintSurface` İşleyicisi Bézier eğrisi çizer ve sonra tam uzunluğunu sığdırmak için metin boyutu:

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

        // Get path length
        SKPathMeasure pathMeasure = new SKPathMeasure(path, false, 1);

        // Find new text size
        textPaint.TextSize = pathMeasure.Length / baseTextWidth * 10;

        // Draw text on path
        canvas.DrawTextOnPath(text, path, 0, 0, textPaint);
    }
    ...
}
```

`Length` Özelliği yeni oluşturulan `SKPathMeasure` nesnesi yol uzunluğunu alır. Yol uzunluğu bölünür `baseTextWidth` (10 metin boyutuna göre metin genişliğini olmayan) değerini alır ve ardından 10 temel metin boyutu tarafından ile çarpılır. Bu yol boyunca metni görüntülemek için yeni bir metin boyutu oluşur:

[![](information-images/pathlength-small.png "Üçlü sayfasının ekran görüntüsü yol uzunluğu")](information-images/pathlength-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yol uzunluğu")

Bézier eğrisi daha uzun veya kısaysa ettiği metin boyutunu değiştirme görebilirsiniz.

## <a name="traversing-the-path"></a>Geçiş yolu

`SKPathMeasure` daha fazlasını ölçü yolunun uzunluğu yapabilirsiniz. Yol uzunluğu, sıfır arasındaki herhangi bir değer için bir `SKPathMeasure` nesnesi elde edebilirsiniz konumu yolu ve tanjantını yolu eğri için bu noktada. Tanjantı vektör biçiminde olarak kullanılabilir bir `SKPoint` nesnesi veya bir döndürme kapsüllenmiş içinde bir `SKMatrix` nesne. Yöntemleri şunlardır `SKPathMeasure` çeşitli ve esnek bir şekilde bu bilgiler edinin:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

Üyeleri [ `SKPathMeasureMatrixFlags` ](xref:SkiaSharp.SKPathMeasureMatrixFlags) numaralandırma şunlardır:

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

**Mi yarı-kanal** sayfa canlandırır çubuk şekli üzerinde üçüncü dereceden Bézier eğrisi sürekli bulutumuzda görünen mi:

[![](information-images/unicyclehalfpipe-small.png "Üçlü sayfasının ekran görüntüsü mi yarı-kanal")](information-images/unicyclehalfpipe-large.png#lightbox "Üçlü sayfasının ekran görüntüsü mi yarı-kanal")

`SKPaint` Yarı-kanal hem mi konturlama için kullanılan nesne alan olarak tanımlanan [ `UnicycleHalfPipePage` ]() sınıfı. Ayrıca tanımlı olan `SKPath` mi nesnesi:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };

    SKPath unicyclePath = SKPath.ParseSvgPathData(
        "M 0 0" +
        "A 25 25 0 0 0 0 -50" +
        "A 25 25 0 0 0 0 0 Z" +
        "M 0 -25 L 0 -100" +
        "A 15 15 0 0 0 0 -130" +
        "A 15 15 0 0 0 0 -100 Z" +
        "M -25 -85 L 25 -85");
    ...
}
```

Standart geçersiz kılmalarına yönelik sınıfı içeren `OnAppearing` ve `OnDisappearing` animasyon için yöntemleri. `PaintSurface` İşleyicisi yarı-kanal yolu oluşturur ve ardından çizer. Bir `SKPathMeasure` nesnesi bu yola göre oluşturulur:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath pipePath = new SKPath())
        {
            pipePath.MoveTo(50, 50);
            pipePath.CubicTo(0, 1.25f * info.Height,
                             info.Width - 0, 1.25f * info.Height,
                             info.Width - 50, 50);

            canvas.DrawPath(pipePath, strokePaint);

            using (SKPathMeasure pathMeasure = new SKPathMeasure(pipePath))
            {
                float length = pathMeasure.Length;

                // Animate t from 0 to 1 every three seconds
                TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
                float t = (float)(timeSpan.TotalSeconds % 5 / 5);

                // t from 0 to 1 to 0 but slower at beginning and end
                t = (float)((1 - Math.Cos(t * 2 * Math.PI)) / 2);

                SKMatrix matrix;
                pathMeasure.GetMatrix(t * length, out matrix,
                                      SKPathMeasureMatrixFlags.GetPositionAndTangent);

                canvas.SetMatrix(matrix);
                canvas.DrawPath(unicyclePath, strokePaint);
            }
        }
    }
}
```

`PaintSurface` İşleyici değerini hesaplar `t` , giden 0 ile 1 olarak beş saniyede. Ardından kullanır `Math.Cos` , değerine dönüştürmek için işlevi `t` , aralığı 0-1 ve 0, geri dön 1 sağ üst köşedeki mi karşılık gelir ancak burada 0 sol üst köşedeki başında mi karşılık gelir. Cosine işlevi, en yavaş kanal üst ve alt hızlı hızını neden olur.

Dikkat edin, bu değer `t` ilk bağımsız değişkeni için yol uzunluğu ile çarpılmasına `GetMatrix`. Matris sonra uygulanan `SKCanvas` mi yol çizmek için nesne.

## <a name="enumerating-the-path"></a>Yolun numaralandırma

İki katıştırılmış sınıfları `SKPath` yolu içeriğini listeleme olanak sağlar. Bu sınıflar [ `SKPath.Iterator` ](xref:SkiaSharp.SKPath.Iterator) ve [ `SKPath.RawIterator` ](xref:SkiaSharp.SKPath.RawIterator). İki sınıf oldukça benzerdir ancak `SKPath.Iterator` yolun bir sıfır uzunlukta veya sıfır uzunluk yakın öğeler ortadan kaldırabilir. `RawIterator` Aşağıdaki örnekte kullanılır.

Bir nesnenin türü elde edebilirsiniz `SKPath.RawIterator` çağırarak [ `CreateRawIterator` ](xref:SkiaSharp.SKPath.CreateRawIterator) yöntemi `SKPath`. Yolundan numaralandırma gerçekleştirilir tekrar tekrar çağırarak [ `Next` ](xref:SkiaSharp.SKPath.RawIterator.Next*) yöntemi. Dört dizisi geçirin `SKPoint` değerleri:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` Yöntemi döndürür üyesi [ `SKPathVerb` ](xref:SkiaSharp.SKPathVerb) numaralandırma türü. Bu değerler yolunda belirli çizim komutu belirtir. Dizide eklenen geçerli noktalarının sayısı bu fiiline bağlıdır:

- `Move` tek bir nokta ile
- `Line` iki nokta ile
- `Cubic` dört noktalarıyla
- `Quad` üç nokta ile
- `Conic` üç nokta ile (ve ayrıca [ `ConicWeight` ](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*) ağırlık yöntemi)
- `Close` bir nokta ile
- `Done`

`Done` Fiili yolu numaralandırması tamamlandı olduğunu gösterir.

Olduğuna dikkat edin hiçbir `Arc` fiilleri. Bu, tüm yaylar yolunu eklendiğinde Bézier eğrileri içine dönüştürülür gösterir.

Bazı bilgileri `SKPoint` dizi gereksizdir. Örneğin, bir `Move` fiili tarafından izlenen bir `Line` fiil ve ardından ilk birlikte gelen iki nokta `Line` aynı `Move` noktası. Uygulamada, bu yedeklilik çok yararlıdır. Aldığınızda bir `Cubic` fiil, üçüncü dereceden Bézier eğrisi tanımlayan tüm dört noktalarıyla eşlik eder. Önceki fiili tarafından belirlenen geçerli konumunu korumak gerek yoktur.

Sorunlu fiili ancak olan `Close`. Bu komut tarafından daha önce oluşturulan dağılımı başlangıcına geçerli konumundan bu düz bir çizgi çizer `Move` komutu. İdeal olarak, `Close` fiil, tek bir nokta yerine bu iki nokta sağlamalıdır. Kötüsü eşlik eden noktası olan `Close` fiil, her zaman (0, 0). Bir yol listeleme, büyük olasılıkla korumak ihtiyacınız olacak `Move` noktası ve geçerli konumu.

## <a name="enumerating-flattening-and-malforming"></a>Numaralandırma, düzleştirme ve Malforming

Bazen bir algoritmik uygulamak için tercih edilir malform yoluna başka bir yolla dönüştürün:

![](information-images/pathenumerationsample.png "Üzerinde bir küreyi sarmalanmış metin")

Bu harfler çoğunu düz çizgilerden oluşur, ancak eğrisine bu düz çizgiler görünüşe göre bükümlü. Nasıl bu mümkün mü?

Özgün düz çizgiler daha küçük düz çizgiler bir dizi ayrılır anahtardır. Bu tek tek daha küçük düz çizgiler eğri oluşturmak için farklı şekillerde değişebilir. 

Bu işlemde size yardımcı olacak [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) örnek içeren bir statik [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) sınıfıyla birlikte bir `Interpolate` böler yöntemi bir düz çizgi içine yalnızca bir birim uzunlukta olan çok sayıda kısa çizgiler. Ayrıca, sınıf eğri yaklaşık küçük düz çizgiler bir dizi Bézier eğrileri üç tür dönüştürme çeşitli yöntemler içerir. (Parametrik formülleri makalesinde sunulan [ **üç türleri, Bézier eğrileri**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) Bu işlem çağrılırken _düzleştirme_ eğri:

```csharp
static class PathExtensions
{
    ...
    static SKPoint[] Interpolate(SKPoint pt0, SKPoint pt1)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * pt0.X + t * pt1.X;
            float y = (1 - t) * pt0.Y + t * pt1.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenCubic(SKPoint pt0, SKPoint pt1, SKPoint pt2, SKPoint pt3)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2) + Length(pt2, pt3));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * (1 - t) * pt0.X +
                        3 * t * (1 - t) * (1 - t) * pt1.X +
                        3 * t * t * (1 - t) * pt2.X +
                        t * t * t * pt3.X;
            float y = (1 - t) * (1 - t) * (1 - t) * pt0.Y +
                        3 * t * (1 - t) * (1 - t) * pt1.Y +
                        3 * t * t * (1 - t) * pt2.Y +
                        t * t * t * pt3.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenQuadratic(SKPoint pt0, SKPoint pt1, SKPoint pt2)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenConic(SKPoint pt0, SKPoint pt1, SKPoint pt2, float weight)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float denominator = (1 - t) * (1 - t) + 2 * weight * t * (1 - t) + t * t;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * weight * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * weight * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            x /= denominator;
            y /= denominator;
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

Şuradan genişleme metodu bu yöntemler tüm başvurulan `CloneWithTransform` Ayrıca bu sınıfta bulunan ve aşağıda gösterilmektedir. Bu yöntem bir yol, yol komutları numaralandırma ve verileri temel alan yeni bir yol oluşturmak kopyalar. Ancak, yalnızca yeni yol oluşur `MoveTo` ve `LineTo` çağırır. Bir dizi çok küçük satırları için tüm düz çizgiler ve eğriler azaltılır.

Çağrılırken `CloneWithTransform`, yönteme geçirin bir `Func<SKPoint, SKPoint>`, bir işlevle olduğu bir `SKPaint` döndüren parametresi bir `SKPoint` değeri. Bu işlev, her noktasının özel algoritmik dönüşüm uygulamak çağrılır:

```csharp
static class PathExtensions
{
    public static SKPath CloneWithTransform(this SKPath pathIn, Func<SKPoint, SKPoint> transform)
    {
        SKPath pathOut = new SKPath();

        using (SKPath.RawIterator iterator = pathIn.CreateRawIterator())
        {
            SKPoint[] points = new SKPoint[4];
            SKPathVerb pathVerb = SKPathVerb.Move;
            SKPoint firstPoint = new SKPoint();
            SKPoint lastPoint = new SKPoint();

            while ((pathVerb = iterator.Next(points)) != SKPathVerb.Done)
            {
                switch (pathVerb)
                {
                    case SKPathVerb.Move:
                        pathOut.MoveTo(transform(points[0]));
                        firstPoint = lastPoint = points[0];
                        break;

                    case SKPathVerb.Line:
                        SKPoint[] linePoints = Interpolate(points[0], points[1]);

                        foreach (SKPoint pt in linePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[1];
                        break;

                    case SKPathVerb.Cubic:
                        SKPoint[] cubicPoints = FlattenCubic(points[0], points[1], points[2], points[3]);

                        foreach (SKPoint pt in cubicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[3];
                        break;

                    case SKPathVerb.Quad:
                        SKPoint[] quadPoints = FlattenQuadratic(points[0], points[1], points[2]);

                        foreach (SKPoint pt in quadPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Conic:
                        SKPoint[] conicPoints = FlattenConic(points[0], points[1], points[2], iterator.ConicWeight());

                        foreach (SKPoint pt in conicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Close:
                        SKPoint[] closePoints = Interpolate(lastPoint, firstPoint);

                        foreach (SKPoint pt in closePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        firstPoint = lastPoint = new SKPoint(0, 0);
                        pathOut.Close();
                        break;
                }
            }
        }
        return pathOut;
    }
    ...
}
```

Kopyalanan yolu için küçük düz çizgiler düşürüldüğünden, dönüştürme işlevi eğrilere düz çizgiler dönüştürme özelliğine sahiptir.

Yöntem adlı değişken her dağılımını ilk noktasını korur bildirimi `firstPoint` ve sonra her bir geçerli konumu çizim komutu değişkeninde `lastPoint`. Bu değişkenler son kapanış oluşturmak için gerekli olan zamanı satır bir `Close` fiil karşılaştı.

**GlobularText** örnek metin bir yarım küre etrafında bir 3B efekti görünüşte sarmalamak için bu genişletme yöntemi kullanır:

[![](information-images/globulartext-small.png "Üçlü sayfasının ekran görüntüsü Globular metin")](information-images/globulartext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Globular metin")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Sınıf oluşturucu, bu dönüşüm gerçekleştirir. Oluşturur bir `SKPaint` nesne metnini ve ardından edinir bir `SKPath` nesnesinden `GetTextPath` yöntemi. Geçirilen yolu budur `CloneWithTransform` bir dönüşüm işlevi ile birlikte genişletme yöntemi:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;

    public GlobularTextPage()
    {
        Title = "Globular Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.Typeface = SKTypeface.FromFamilyName("Times New Roman");
            textPaint.TextSize = 100;

            using (SKPath textPath = textPaint.GetTextPath("HELLO", 0, 0))
            {
                SKRect textPathBounds;
                textPath.GetBounds(out textPathBounds);

                globePath = textPath.CloneWithTransform((SKPoint pt) =>
                {
                    double longitude = (Math.PI / textPathBounds.Width) *
                                            (pt.X - textPathBounds.Left) - Math.PI / 2;
                    double latitude = (Math.PI / textPathBounds.Height) *
                                            (pt.Y - textPathBounds.Top) - Math.PI / 2;

                    longitude *= 0.75;
                    latitude *= 0.75;

                    float x = (float)(Math.Cos(latitude) * Math.Sin(longitude));
                    float y = (float)Math.Sin(latitude);

                    return new SKPoint(x, y);
                });
            }
        }
    }
    ...
}
```

Dönüşüm işlevinin ilk adlı iki değerleri hesaplar `longitude` ve `latitude` – π/2'de üst ve sol metin arasındayken π/2 metnin alt ve sağ. 0,75 çarpılarak azaltılır için bu değerleri aralığı görsel olarak tatmin edici değil. (Bu ayarlama olmayan kod deneyin. Metin Kuzey ve Güney kutupları belirsiz ve yüz en çok ince olur.) Bu üç boyutlu küresel koordinatları için iki boyutlu dönüştürülür `x` ve `y` standart formüller tarafından koordinatları.

Yeni yol, bir alan olarak depolanır. `PaintSurface` İşleyicisi daha sonra yalnızca gerekir ve ekranda gösteren yol ölçeklendirmek:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint pathPaint = new SKPaint())
        {
            pathPaint.Style = SKPaintStyle.Fill;
            pathPaint.Color = SKColors.Blue;
            pathPaint.StrokeWidth = 3;
            pathPaint.IsAntialias = true;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(0.45f * Math.Min(info.Width, info.Height));     // radius
            canvas.DrawPath(globePath, pathPaint);
        }
    }
}
```

Bu çok yönlü bir tekniktir. Yol etkileri dizisi açıklanan varsa [ **yol etkileri** ](effects.md) makale değil oldukça kapsayacak düşünmüştür olmalıdır dahil, bir şey boşlukları doldurmak için bir yolu budur.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
