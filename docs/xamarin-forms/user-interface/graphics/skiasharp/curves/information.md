---
title: "Yol bilgisi ve numaralandırması"
description: "Yollar hakkında bilgi almak ve içeriği listeleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/12/2017
ms.openlocfilehash: c7edf0c8e563dad25693d184d3a44a3e66466126
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="path-information-and-enumeration"></a>Yol bilgisi ve numaralandırması

_Yollar hakkında bilgi almak ve içeriği listeleme_

[ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) Sınıfı, çeşitli özellikleri ve yolu hakkında bilgi edinmek izin yöntemleri tanımlar. [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) Ve [ `TightBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.TightBounds/) özellikleri (ve ilgili yöntemleri) elde metrical boyutları bir yolu. [ `Contains` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Contains/p/System.Single/System.Single/) Yöntemi, belirli bir noktaya yol içinde olup olmadığını belirlemenize olanak sağlar.

Bazen, tüm çizgiler ve yolunu oluşturur Eğriler toplam uzunluğu belirlemek yararlıdır. Sınıfının tümüne adlı bu algorithmically basit bir görev olmadığından [ `PathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) kendisine ayrılan.

Ayrıca bazen çizim işlemleri ve bir yolu noktalar elde etmek yararlıdır. İlk başta, bu özelliği gereksiz görünebilir: programınızı yolu oluşturduysa, program zaten içeriği bilir. Ancak, yolları tarafından da oluşturulabilir gördünüz [yolu etkileri](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) ve dönüştürerek [yolları metin dizeleri](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Çizim işlemler ve bu yolları yapmak noktaları edinebilirsiniz. Bunu yapmanın bir algoritmik dönüştürme tüm noktalarını uygulamaktır. Bu geçici bir küreyi metin kaydırma gibi teknikler sağlar:

![](information-images/pathenumerationsample.png "Üzerinde bir küreyi kaydırılan metin")

## <a name="getting-the-path-length"></a>Yol uzunluğu alma

Makalede [ **yolları ve metin** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) nasıl kullanılacağını gördünüz [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) , temel bir yolu seyri izleyen bir metin dizesi çizmek için yöntem. Ancak, tam yolunu uygun şekilde metin boyutlandırmak isterseniz? Dairenin çevresi hesaplamak basit bir daire metin çizme için kolay olmasıdır. Ancak elips çevresi veya bir Bézier eğrisi uzunluğunu kadar basit değildir. 

[ `SKPathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) Sınıfı yardımcı olabilir. [Oluşturucusu](https://developer.xamarin.com/api/constructor/SkiaSharp.SKPathMeasure.SKPathMeasure/p/SkiaSharp.SKPath/System.Boolean/System.Single/) kabul eden bir `SKPath` bağımsız değişkeni ve [ `Length` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPathMeasure.Length/) özelliği uzunluğunu gösterir.

Bu, gösterilmiştir **yol uzunluğu** dayanır örnek **Bezier eğrisi** sayfası. [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) dosya türer `InteractivePage` ve dokunma arabirimi içerir:

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

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) arka plan kod dosyasına uç noktaları tanımlamak ve bir küp Bézier eğrisi noktalarını kontrol etmek için dört dokunma noktaları taşımanızı sağlar. Üç alan bir metin dizesi tanımlayan bir `SKPaint` nesne ve metin hesaplanan genişliği:

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

`PaintSurface` İşleyicisi Bézier eğrisi çizer ve tam uzunluğu sığması için metin boyutu:

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

`Length` Özelliğinin yeni oluşturulan `SKPathMeasure` nesnesi yolunun uzunluğu alır. Bu bölünür `baseTextWidth` (10 metin boyutuna göre metin genişliğini olmayan) değeri ve 10 temel metin boyutu tarafından ile çarpılır. Bu yol boyunca metin görüntüleme için yeni bir metin boyutu oluşur:

[![](information-images/pathlength-small.png "Üçlü sayfasının ekran görüntüsü yol uzunluğu")](information-images/pathlength-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yol uzunluğu")

Daha uzun veya kısaysa Bézier eğrisi alır gibi değiştirmek metin boyutu görebilirsiniz.

## <a name="traversing-the-path"></a>Yolun çapraz geçiş yapma

`SKPathMeasure` daha fazlasını ölçü yolunun uzunluğu yapabilirsiniz. Yol uzunluğu sıfır arasındaki herhangi bir değer için bir `SKPathMeasure` nesne elde edebilirsiniz yolunu ve yol eğri teğet konumuna o noktada. Tanjantını biçiminde bir vektör olarak kullanılabilir bir `SKPoint` nesne veya döndürme kapsüllenmiş bir `SKMatrix` nesnesi. Yöntemlerinin işte `SKPathMeasure` esnek ve çeşitli yollarla bu bilgileri elde:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

[ `SKPathMeasureMatrixFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasureMatrixFlags/) Şunlardır:

- [`GetPosition`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPosition/)
- [`GetTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)
- [`GetPositionAndTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)

**Mi yarı-Pipe** sayfa canlandırır ve geriye bir küp Bézier eğrisi ride görünüyor mi üzerinde çubuk şekli:

[![](information-images/unicyclehalfpipe-small.png "Üçlü sayfasının ekran görüntüsü mi yarı-Pipe")](information-images/unicyclehalfpipe-large.png#lightbox "Üçlü sayfasının ekran görüntüsü mi yarı-kanal")

`SKPaint` Yarı-pipe ve mi vuruş yapması için kullanılan nesne alanı olarak tanımlanır [ `UnicycleHalfPipePage` ]() sınıfı. Ayrıca tanımlandığı `SKPath` nesne mi için:

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

Sınıfı, standart geçersiz kılmaları içeren `OnAppearing` ve `OnDisappearing` animasyon için yöntemleri. `PaintSurface` İşleyicisi yarı-pipe yolunu oluşturur ve ardından çizer. Bir `SKPathMeasure` nesne bu yola göre oluşturulur:

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

`PaintSurface` İşleyici değerini hesaplar `t` , giden 0 ile 1 için beş saniyede. Daha sonra kullanır `Math.Cos` değerine dönüştürmek için işlevi `t` , aralığı 0-1 ve tekrar 0, 1 sağ üst köşedeki mi karşılık gelir ancak burada 0 sol üst kısmında başında mi karşılık gelir. Kosinüsü işlevi hızı yavaş kanal üst ve alt hızlı olması neden olur.

Dikkat bu değeri `t` ilk bağımsız değişkeni için yol uzunluğu tarafından çarpılacağı `GetMatrix`. Matris sonra uygulanan `SKCanvas` mi yolu çizmek için nesne.

## <a name="enumerating-the-path"></a>Yolun numaralandırma

İki katıştırılmış sınıfları `SKPath` yolu içeriğini listeleme olanak sağlar. Bu sınıflar [ `SKPath.Iterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+Iterator/) ve [ `SKPath.RawIterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+RawIterator/). İki sınıf çok benzer, ancak `SKPath.Iterator` sıfır uzunluğunda veya sıfır uzunluk yakın yolunu öğelerinde ortadan kaldırabilirsiniz. `RawIterator` Aşağıdaki örnekte kullanılır.

Türünde bir nesne edinebilirsiniz `SKPath.RawIterator` çağırarak [ `CreateRawIterator` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CreateRawIterator()/) yöntemi `SKPath`. Yol üzerinden numaralandırma gerçekleştirilir art arda çağırarak [ `Next` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.Next/p/SkiaSharp.SKPoint[]/) yöntemi. İçin dört dizisi geçirin `SKPoint` değerler:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` Yöntemi döndürür üyesi [ `SKPathVerb` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathVerb/) numaralandırması. Bu değerler yolunda belirli çizim komutu belirtir. Dizideki eklenen geçerli noktası sayısı bu fiiline bağlıdır:

- [`Move`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Move/) tek bir nokta ile
- [`Line`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Line/) iki nokta ile
- [`Cubic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Cubic/) dört noktalarıyla
- [`Quad`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Quad/) üç noktalar
- [`Conic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Conic/) üç nokta ile (ve ayrıca [ `ConicWeight` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.ConicWeight/) yöntemi ağırlık)
- [`Close`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Close/) bir nokta ile
- [`Done`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Done/)

`Done` Fiil numaralandırması tam olduğunu gösterir.

Olduğuna dikkat edin hiçbir `Arc` fiilleri. Bu, tüm yaylar yolunu eklendiğinde Bézier eğrileri uygulamasına dönüştürülür gösterir.

Bazı bilgiler `SKPoint` dizi gereksizdir. Örneğin, varsa bir `Move` fiil tarafından izlenen bir `Line` fiil sonra eşlik iki nokta ilk `Line` aynı `Move` gelin. Uygulamada, bu artıklık çok yararlı olur. Ulaştığınızda bir `Cubic` fiili küp Bézier eğrisi tanımlayan tüm dört noktalarıyla eşlik. Önceki fiili tarafından belirlenen geçerli konumu korumak için gerek yoktur.

Sorunlu fiili ancak olduğunda `Close`. Bu komut bir çizgide geçerli konumundan göre daha önce oluşturulmuş dağılımı başlangıcına çizer `Move` komutu. İdeal olarak, `Close` tek nokta yerine bu iki nokta fiil temin etmelidir. Kötüsü noktası eşlik olan `Close` fiili olduğunda her zaman (0, 0). Bir yol listeleme, büyük olasılıkla korumak sağlamanız gerekir, yani `Move` noktası ve geçerli konumu.

Statik [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) sınıfı eğri yaklaşık küçük düz çizgiler bir dizi Bézier eğrileri üç tür dönüştürme birkaç yöntem içerir. (Parametrik formüller makalesinde sunulan [ **üç türleri, Bézier eğrileri**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) `Interpolate` Yöntemi yalnızca bir birim uzunluğu olan çok sayıda kısa çizgiler içine bir çizgide böler:

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

Tüm bu yöntemleri uzantısı yönteminden başvurulan `CloneWithTransform` aşağıda gösterilmektedir. Bu yöntem bir yolu yolu komutları numaralandırma ve verileri temel alan yeni bir yol oluşturma klonlar. Ancak, yeni yol yalnızca oluşan `MoveTo` ve `LineTo` çağrıları. Eğriler ve düz çizgiler küçük satırları seriye azaltılır.

Çağrılırken `CloneWithTransform`, yönteme geçirin bir `Func<SKPoint, SKPoint>`, bir işlev olduğu bir `SKPaint` döndürür parametresi bir `SKPoint` değeri. Bu işlev her noktasının özel algoritmik dönüşüm uygulamak çağrılır:

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

Kopyalanan yolu küçük düz satırlarına düşürüldüğünden, dönüştürme işlevi Eğrileri düz çizgiler dönüştürme yeteneğine sahiptir.

Yöntem adlı değişken her dağılımını ilk noktasını korur bildirim `firstPoint` ve her sonra geçerli konumu çizim komutu değişkende `lastPoint`. Bu son kapanış oluşturmak için gerekli olan zaman satır bir `Close` fiili karşılaştı.

**GlobularText** örnek gibi görünen bir 3B efekti bir küreyi etrafına metin kaydırma için bu genişletme yöntemi kullanır:

[![](information-images/globulartext-small.png "Üçlü sayfasının ekran görüntüsü Globular metin")](information-images/globulartext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Globular metin")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Sınıfı oluşturucusu bu dönüşüm gerçekleştirir. Oluşturduğu bir `SKPaint` nesnesi için metin ve ardından edinir bir `SKPath` nesnesinin `GetTextPath` yöntemi. Bu geçirilen yoludur `CloneWithTransform` genişletme yöntemi bir dönüşüm işlevi ile birlikte: 

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

Dönüşüm işlevinin ilk adlı iki değer hesaplar `longitude` ve `latitude` bu metnin alt ve sağ π/2 ile – π/2 metin solundaki ve üstündeki arasındadır. Tarafından 0,75 çarparak azaltılır şekilde bu değerleri aralığı görsel olarak kabul edilebilir değil. (Bu ayarlamalar olmadan kod deneyin. Metin çok belirsiz Kuzey ve Güney kutupları ve yanları en çok ince olur.) Bu üç boyutlu küresel koordinatlar için iki boyutlu dönüştürülür `x` ve `y` standart formüller tarafından koordinatları.

Yeni yol bir alan olarak depolanır. `PaintSurface` İşleyici sonra yalnızca gereken merkezi ve ekranda yolu ölçeklendirmek:

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

Bu çok yönlü bir tekniktir. Yol etkilerini dizisi açıklanan varsa [ **yolu etkileri** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) makale değil oldukça kapsayan bir şey Keçeli olmalıdır dahil, bu doldurmak için bir yoldur.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
