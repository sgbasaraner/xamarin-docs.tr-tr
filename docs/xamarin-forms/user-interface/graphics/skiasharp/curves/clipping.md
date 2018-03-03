---
title: "Kırpma yolları ve bölgeler"
description: "Küçük grafik yollara belirli alanları ve bölgeler oluşturmak için kullanın"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: b1c5b64725a163e15f07d2aecaea4e56b7ecec2e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="clipping-with-paths-and-regions"></a>Kırpma yolları ve bölgeler

_Küçük grafik yollara belirli alanları ve bölgeler oluşturmak için kullanın_

Bazen, belirli bir alanın grafik işleme kısıtlamak gereklidir. Bu olarak bilinir *kırpma*. Bu bir anahtar deliği görülen monkey görüntüsü gibi özel efektler kırpma kullanabilirsiniz:

![](clipping-images/clippingsample.png "Bir anahtar deliği aracılığıyla Monkey")

*Kırpma alanı* grafik işlenir ekran alanıdır. Kırpma alanının dışında görüntülenen herhangi bir şey işlenmez. Kırpma alanı genellikle tarafından tanımlanan bir [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) nesne, ancak bunun yerine tanımlayabilirsiniz kırpma alanı kullanarak bir [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) nesnesi. Bir yolundan bir bölge oluşturmak için bu nesnede iki tür ilk ilgili gibi görünüyor. Ancak, bir bölgesinden bir yolunu oluşturamazsınız ve dahili olarak çok farklı: bir bölge yatay tarama satırları bir dizi tanımlı bir yol çizgiler ve eğrilerle, bir dizi oluşur.

Yukarıdaki resimde tarafından oluşturulan **anahtar deliği aracılığıyla Monkey** sayfası. [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) Sınıfı SVG verileri kullanarak bir yolu tanımlar ve program kaynaklardan bir bit eşlem yük için Oluşturucusu kullanır:

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    SKBitmap bitmap;
    SKPath keyholePath = SKPath.ParseSvgPathData(
        "M 300 130 L 250 350 L 450 350 L 400 130 A 70 70 0 1 0 300 130 Z");

    public MonkeyThroughKeyholePage()
    {
        Title = "Monkey through Keyhole";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

Ancak `keyholePath` nesneyi tanımlayan bir anahtar deliği özetini, koordinatları tamamen rastgele ve yol verileri geliştirmiştir olduğunda ne kullanışlı yansıtır. Bu nedenle, `PaintSurface` işleyici alır bu yol ve çağrıları sınırlarına `Translate` ve `Scale` yolu ekran merkezine taşımanızı ve neredeyse yüksekliğinde ekran yapmak için:


```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transform to center and enlarge clip path to window height
        SKRect bounds;
        keyholePath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.98f * info.Height / bounds.Height);
        canvas.Translate(-bounds.MidX, -bounds.MidY);

        // Set the clip path
        canvas.ClipPath(keyholePath);

        // Reset transforms
        canvas.ResetMatrix();

        // Display monkey to fill height of window but maintain aspect ratio
        canvas.DrawBitmap(bitmap,
            new SKRect((info.Width - info.Height) / 2, 0,
                       (info.Width + info.Height) / 2, info.Height));
    }
}
```

Ancak yol değil işlenir. Bunun yerine, Dönüşümlerin yolu bu deyim kırpması alanıyla ayarlamak için kullanılır:

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface` İşleyici sonra çağrısıyla dönüşümler sıfırlar `ResetMatrix` ve tam ekran yüksekliğine genişletmek için bit eşlem çizer. Bu kod bit eşlem bu belirli bit eşlemi olan kare, olduğunu varsayar. Bit eşlem yalnızca kırpma yolu tarafından tanımlanan alanda oluşturulur:

[![](clipping-images/monkeythroughkeyhole-small.png "Üçlü ekran görüntüsü anahtar deliği sayfa aracılığıyla Monkey")](clipping-images/monkeythroughkeyhole-large.png "anahtar deliği sayfa aracılığıyla Monkey Üçlü ekran görüntüsü")

Kırpma yolunu yürürlükte dönüşümler konu olduğunda `ClipPath` yöntemi çağrılır ve değil dönüşümler etkin olduğunda (örneğin, bir bit eşlem) bir grafik nesnesi görüntülenir. Kırpma yolu ile kaydedilen tuvale durumu parçası olan `Save` yöntemi ve geri yüklenen ile `Restore` yöntemi.

## <a name="combining-clipping-paths"></a>Kırpma yolları birleştirme

NET olarak söylemek kırpma alanı "ayarlanmadı" `ClipPath` yöntemi. Bunun yerine, bir dikdörtgen ekrana boyutları eşit olarak başlar varolan kırpma yolu ile birleştirilir. Kırpma alanı kullanmanın dikdörtgen sınırları edinebilirsiniz [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/) özelliği veya [ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/) özelliği. `ClipBounds` Özelliği döndürür bir `SKRect` herhangi dönüştüren yansıtır değeri etkili olabilir. `ClipDeviceBounds` Özelliği döndürür bir `RectI` değeri. Bu tamsayı boyutlarla dikdörtgen şeklinde ve gerçek piksel boyutlarını kırpma alanında açıklar.

Herhangi bir çağrıda için `ClipPath` yeni bir alan kırpma alanıyla birleştirerek kırpma alanı azaltır. Tam söz dizimi [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/) yöntemi:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Ayrıca bir [ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/) kırpma alanı olan bir dikdörtgen birleştirir yöntemi:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Varsayılan olarak, var olan kırpma alanı kesişimini sonuç kırpma alanıdır ve `SKPath` veya `SKRect` belirtilen `ClipPath` veya `ClipRect` yöntemi. Bu, gösterilmiştir **dört daireler kesiştiği küçük** sayfa. `PaintSurface` İşleyicisinde [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) sınıfı aynı yeniden kullanır `SKPath` kırpma alan art arda çağrılar yoluyla her biri azaltır, dört çakışan daire oluşturulacak nesne `ClipPath`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float size = Math.Min(info.Width, info.Height);
    float radius = 0.4f * size;
    float offset = size / 2 - radius;

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    using (SKPath path = new SKPath())
    {
        path.AddCircle(-offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(-offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColors.Blue;
            canvas.DrawPaint(paint);
        }
    }
}
```

Ne sol bu dört daire kesişimi şöyledir:

[![](clipping-images//fourcircleintersectclip-small.png "Üçlü sayfasının ekran görüntüsü dört daire kesiştiği küçük")](clipping-images/fourcircleintersectclip-large.png "Üçlü sayfasının ekran görüntüsü dört daire kesiştiği küçük")

[ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/) Numaralandırma yalnızca iki üye vardır:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) Belirtilen yol veya dikdörtgen varolan kırpma alanından kaldırır

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) Belirtilen yol ya da varolan kırpma alan ile dikdörtgen kesiştiğinden

Dört değiştirirseniz `SKClipOperation.Intersect` değişkenlerinde `FourCircleIntersectClipPage` ile sınıf `SKClipOperation.Difference`, aşağıdaki görürsünüz:

[![](clipping-images//fourcircledifferenceclip-small.png "Üçlü sayfasının ekran görüntüsü dört daire kesiştiği küçük fark işlemi ile")](clipping-images/fourcircledifferenceclip-large.png "Üçlü sayfasının ekran görüntüsü dört daire kesiştiği küçük fark işlemi ile")

Dört çakışan daire kırpma alanı'ndan kaldırıldı.

**Küçük işlemleri** sayfası yalnızca daireler çifti ile bu iki işlem arasındaki farkı gösterir. Soldaki ilk daire varsayılan küçük işlemini kırpma alanıyla eklenen `Intersect`, sağdaki ikinci daire metin etiketi tarafından belirtilen küçük işlemi ile kırpma bölgesine eklenir:

[![](clipping-images//clipoperations-small.png "Üçlü sayfasının ekran görüntüsü küçük işlemleri")](clipping-images/clipoperations-large.png "Üçlü sayfasının ekran görüntüsü küçük işlemleri")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) Sınıfı tanımlayan iki `SKPaint` nesne alanları olarak ve ardından iki dikdörtgen alanlarına ekran böler. Bu alanlar telefon dikey veya yatay modunda olmasına bağlı olarak farklılık gösterir. `DisplayClipOp` Sınıfı ardından görüntüler çağrıları ve metin `ClipPath` her küçük işlemi göstermek için iki daire yollar ile:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;

    foreach (SKClipOperation clipOp in Enum.GetValues(typeof(SKClipOperation)))
    {
        // Portrait mode
        if (info.Height > info.Width)
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width, y + info.Height / 2), clipOp);
            y += info.Height / 2;
        }
        // Landscape mode
        else
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width / 2, y + info.Height), clipOp);
            x += info.Width / 2;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKClipOperation clipOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(clipOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    canvas.Save();

    using (SKPath path1 = new SKPath())
    {
        path1.AddCircle(xCenter - radius / 2, yCenter, radius);
        canvas.ClipPath(path1);

        using (SKPath path2 = new SKPath())
        {
            path2.AddCircle(xCenter + radius / 2, yCenter, radius);
            canvas.ClipPath(path2, clipOp);

            canvas.DrawPaint(fillPaint);
        }
    }

    canvas.Restore();
}
```

Çağırma `DrawPaint` normalde ile doldurulacak Kanvasın tamamını neden `SKPaint` nesnesi, ancak bu durumda, yöntem yalnızca boyar kırpma alanı içinde.

## <a name="exploring-regions"></a>Bölgeler keşfetme

API belgelerine denedikten varsa `SKCanvas`, aşırı fark etmiş olabilirsiniz `ClipPath` ve `ClipRect` ancak bunun yerine, yukarıda açıklanan yöntemlere benzer yöntemlerine sahip adlı bir parametre [ `SKRegionOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/) yerine `SKClipOperation`. `SKRegionOperation` form kırpma alanlara yolları birleştirme biraz daha fazla esneklik sağlayan altı üyeler içeriyor:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

Ancak, aşırı `ClipPath` ve `ClipRect` ile `SKRegionOperation` parametreleri artık kullanılmıyor ve bunlar kullanılamaz.

Kullanmaya devam edebilirsiniz `SKRegionOperation` numaralandırma ancak gerektirir kırpma alanı cinsinden tanımladığınız bir [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) nesnesi.

Yeni oluşturulan bir `SKRegion` nesnesi boş bir alana açıklar. Genellikle ilk nesne üzerinde çağrıdır [ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/) dikdörtgen bir bölgesi açıklamak böylece. Parametre `SetRect` olan bir bir `SKRectI` değeri & #x 2014; dikdörtgen değeri tamsayı özelliklere sahip. Ardından çağırabilirsiniz [ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/) ile bir `SKPath` nesnesi. Bu yolun iç ile aynıdır, ancak ilk dikdörtgen bölgesine kırpılmış bölge oluşturur.

`SKRegionOperation` Numaralandırma birini çağırdığınızda oyuna yalnızca gelir [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/) bunun gibi yöntemi aşırı:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Değişiklik yapıyorsunuz bölge `Op` çağrıda bağlı bir parametre olarak belirtilen bölgeyi birlikte `SKRegionOperation` üyesi. Son olarak bir bölge kırpma için uygun aldığınızda, tuvalin kullanmanın kırpma alanı olarak ayarlayabilirsiniz [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/) yöntemi `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

Aşağıdaki ekran görüntüsünde altı bölge işlemlerde dayalı kırpma alanlarını gösterir. Sol daire bölgedir, `Op` yöntemi çağrılır ve sağ daire geçirilen bölgedir `Op` yöntemi:

[![](clipping-images//regionoperations-small.png "Üçlü sayfasının ekran görüntüsü bölge işlemleri")](clipping-images/regionoperations-large.png "Üçlü sayfasının ekran görüntüsü bölge işlemleri")

Bu iki daire birleştirmenin bu tüm olanakları misiniz? İçinde görülen, kendilerini tarafından bir bileşimidir üç bileşeni olarak sonuç görüntü göz önünde bulundurun `Difference`, `Intersect`, ve `ReverseDifference` işlemleri. Toplam bileşimleri üçüncü güç için iki veya sekiz sayısıdır. Eksik olan iki özgün bölge olan (değil çağırma sonuçları `Op` hiç) ve tamamen boş bir bölge.

İlk olarak bu yolundan bir yol ve bir bölge oluşturmanız ve daha sonra birden çok bölgeye birleştirmeniz gerekir çünkü kırpma bölgeleri kullanmak için daha zor olduğu. Genel yapısı **bölge işlemleri** sayfasıdır çok benzer **küçük işlemleri** ancak [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) sınıfı ekran altı alanlarına böler ve Bu proje için bölgeleri kullanmak için gereken ek iş gösterir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;
    float width = info.Height > info.Width ? info.Width / 2 : info.Width / 3;
    float height = info.Height > info.Width ? info.Height / 3 : info.Height / 2;

    foreach (SKRegionOperation regionOp in Enum.GetValues(typeof(SKRegionOperation)))
    {
        DisplayClipOp(canvas, new SKRect(x, y, x + width, y + height), regionOp);

        if ((x += width) >= info.Width)
        {
            x = 0;
            y += height;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKRegionOperation regionOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(regionOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    SKRectI recti = new SKRectI((int)rect.Left, (int)rect.Top,
                                (int)rect.Right, (int)rect.Bottom);

    using (SKRegion wholeRectRegion = new SKRegion())
    {
        wholeRectRegion.SetRect(recti);

        using (SKRegion region1 = new SKRegion(wholeRectRegion))
        using (SKRegion region2 = new SKRegion(wholeRectRegion))
        {
            using (SKPath path1 = new SKPath())
            {
                path1.AddCircle(xCenter - radius / 2, yCenter, radius);
                region1.SetPath(path1);
            }

            using (SKPath path2 = new SKPath())
            {
                path2.AddCircle(xCenter + radius / 2, yCenter, radius);
                region2.SetPath(path2);
            }

            region1.Op(region2, regionOp);

            canvas.Save();
            canvas.ClipRegion(region1);
            canvas.DrawPaint(fillPaint);
            canvas.Restore();
        }
    }
}
```

Arasında büyük farklar işte `ClipPath` yöntemi ve `ClipRegion` yöntemi:

> [!IMPORTANT]
> Farklı `ClipPath` yöntemi, `ClipRegion` yöntemi tarafından dönüşümler etkilenmez.

Bu farkı stratejinin anlamak için onu anlamak faydalıdır hangi bir bölgedir. Nasıl küçük işlemler veya bölge işlemleri dahili olarak uygulanabilir hakkında zorlayıcı, büyük olasılıkla çok karmaşık görünüyor. Birkaç olası çok karmaşık yolları birleştirilir ve sonuç yolu anahat olasılıkla algoritmik onarımı kabus olur.

Ancak her bir yol yatay tarama satırları, eski moda vakum boru TV de gibi bir dizi azaltıldığında bu işi oldukça Basitleştirilmiş. Yalnızca yatay bir satırda bir başlangıç ve bitiş noktası ile her tarama satırıdır. Örneğin, bir daire RADIUS 10 ile 20 yatay tarama satırlarını her biri bir daire sol kısmından başlar ve doğru kısmından biter içine ayrılmış. Her çifti ilgili tarama satırlar, başlangıç ve bitiş koordinatlarını inceleyerek, basitçe kurabilirsiniz olduğundan iki daire herhangi bir bölge işlemi ile birleştirerek çok basit olur.

Bu nedir bir bölgedir: alanını tanımlayan bir dizi yatay tarama satırları.

Ancak, bir alanı tarama bir dizi için ne zaman sınırlı satırlarını, bu tarama satırları bir belirli piksel boyutunda temel alır. NET olarak söylemek bölge vektör grafik nesnesi değil. Doğası gereği daha yakın bir sıkıştırılmış tek renkli bit eşlemi yol. Sonuç olarak, bölgeler ölçeklendirilmiş veya değiştirilemez uygunluğunu kaybetmeden ve bu nedenle bunlar kırpma alanları için kullanıldığında dönüştürülen değil döndürülebilir.

Ancak, boyama amaçları için bölgeler dönüşümler uygulayabilirsiniz. **Bölge boyama** program Canlı bölgeler iç yapısını gösterir. [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) Sınıfı oluşturur bir `SKRegion` nesne temel alarak bir `SKPath` 10 birim RADIUS dairenin. Bir dönüştürme sonra sayfayı doldurmak için bu daire genişletir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int radius = 10;

    // Create circular path
    using (SKPath circlePath = new SKPath())
    {
        circlePath.AddCircle(0, 0, radius);

        // Create circular region
        using (SKRegion circleRegion = new SKRegion())
        {
            circleRegion.SetRect(new SKRectI(-radius, -radius, radius, radius));
            circleRegion.SetPath(circlePath);

            // Set transform to move it to center and scale up
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(Math.Min(info.Width / 2, info.Height / 2) / radius);

            // Fill region
            using (SKPaint fillPaint = new SKPaint())
            {
                fillPaint.Style = SKPaintStyle.Fill;
                fillPaint.Color = SKColors.Orange;

                canvas.DrawRegion(circleRegion, fillPaint);
            }

            // Stroke path for comparison
            using (SKPaint strokePaint = new SKPaint())
            {
                strokePaint.Style = SKPaintStyle.Stroke;
                strokePaint.Color = SKColors.Blue;
                strokePaint.StrokeWidth = 0.1f;

                canvas.DrawPath(circlePath, strokePaint);
            }
        }
    }
}
```

`DrawRegion` Çağrısı doldurur turuncu, bölgede sırada `DrawPath` çağrısı mavi karşılaştırma için kullanılacak özgün yolu konturlar:

[![](clipping-images//regionpaint-small.png "Üçlü sayfasının ekran görüntüsü bölge boyama")](clipping-images/regionpaint-large.png "Üçlü sayfasının ekran görüntüsü bölge boyama")

Bölge açıkça ayrık koordinatları dizisidir.

Dönüşümler kırpma alanlarınızı bağlantılı olarak kullanmanız gerekmez, bölgeler kırpma için olarak kullanabileceğiniz **dört – yaprak Yonca** sayfası gösterilmektedir. [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) Sınıfı dört döngüsel bölgelerinden bileşik bir bölge oluşturur, o bileşik bölgeyle kırpma alanı olarak ayarlar ve bir dizi sayfa Merkezi'nden yayılan 360 düz çizgiler çizer:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float xCenter = info.Width / 2;
    float yCenter = info.Height / 2;
    float radius = 0.24f * Math.Min(info.Width, info.Height);

    using (SKRegion wholeScreenRegion = new SKRegion())
    {
        wholeScreenRegion.SetRect(new SKRectI(0, 0, info.Width, info.Height));

        using (SKRegion leftRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion rightRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion topRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion bottomRegion = new SKRegion(wholeScreenRegion))
        {
            using (SKPath circlePath = new SKPath())
            {
                // Make basic circle path
                circlePath.AddCircle(xCenter, yCenter, radius);

                // Left leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, 0));
                leftRegion.SetPath(circlePath);

                // Right leaf
                circlePath.Transform(SKMatrix.MakeTranslation(2 * radius, 0));
                rightRegion.SetPath(circlePath);

                // Make union of right with left
                leftRegion.Op(rightRegion, SKRegionOperation.Union);

                // Top leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, -radius));
                topRegion.SetPath(circlePath);

                // Combine with bottom leaf
                circlePath.Transform(SKMatrix.MakeTranslation(0, 2 * radius));
                bottomRegion.SetPath(circlePath);

                // Make union of top with bottom
                bottomRegion.Op(topRegion, SKRegionOperation.Union);

                // Exclusive-OR left and right with top and bottom
                leftRegion.Op(bottomRegion, SKRegionOperation.XOR);

                // Set that as clip region
                canvas.ClipRegion(leftRegion);

                // Set transform for drawing lines from center
                canvas.Translate(xCenter, yCenter);

                // Draw 360 lines
                for (double angle = 0; angle < 360; angle++)
                {
                    float x = 2 * radius * (float)Math.Cos(Math.PI * angle / 180);
                    float y = 2 * radius * (float)Math.Sin(Math.PI * angle / 180);

                    using (SKPaint strokePaint = new SKPaint())
                    {
                        strokePaint.Color = SKColors.Green;
                        strokePaint.StrokeWidth = 2;

                        canvas.DrawLine(0, 0, x, y, strokePaint);
                    }
                }
            }
        }
    }
}
```

Gerçekten dört – yaprak Yonca gibi görünmüyor ancak Aksi halde olmadan kırpma işlemek sabit olabilecek bir görüntü değil:

[![](clipping-images//fourleafclover-small.png "Üçlü sayfasının ekran görüntüsü dört – yaprak Yonca")](clipping-images/fourleafclover-large.png "Üçlü sayfasının ekran görüntüsü dört – yaprak Yonca")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
