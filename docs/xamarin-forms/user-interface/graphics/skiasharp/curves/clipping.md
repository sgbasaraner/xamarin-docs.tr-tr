---
title: Yollar ve bölgeler ile kırpma
description: Bu makalede belirli alanlara küçük grafik SkiaSharp yollarını kullanın ve bölgeler oluşturmak için nasıl açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: 0c07d68535349004eeefeaa18daa9c59b889a6a7
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615294"
---
# <a name="clipping-with-paths-and-regions"></a>Yollar ve bölgeler ile kırpma

_Küçük grafik yolları belirli alanları ve bölgeler oluşturmak için kullanın._

Bazen, belirli bir alandaki grafik işlenmesini kısıtlamak gereklidir. Bu olarak bilinir *kırpma*. Bu bir anahtar deliği görülen bir monkey görüntüsü gibi özel efekt için kırpma kullanabilirsiniz:

![](clipping-images/clippingsample.png "Bir anahtar deliği aracılığıyla Monkey")

*Kırpma alan* grafik işlenir ekran alanıdır. Kırpma bölgesinin dışında görünür herhangi bir şey yazdırılmaz. Kırpma alanı genellikle tarafından tanımlanan bir [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) nesne, ancak bunun yerine tanımlayabilirsiniz kırpma alanı kullanarak bir [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) nesne. Bir yoldan oluşturabileceğiniz bir bölge olduğundan bu iki tür nesneler ilk ilgili gibi görünüyor. Ancak, bir bölgeden bir yol oluşturulamıyor ve dahili olarak çok farklı olan: bölge, bir dizi yatay tarama satırları tarafından tanımlı bir yol çizgiler ve eğrilerle, bir dizi oluşur.

Yukarıdaki resimde tarafından oluşturulan **Monkey anahtar deliği aracılığıyla** sayfası. [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) Sınıfı SVG verileri kullanarak bir yolunu tanımlar ve bir bit eşlem program kaynaklardan yüklemeye Oluşturucusu kullanır:

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
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

Ancak `keyholePath` nesneyi tanımlayan bir anahtar deliği özetini, koordinatlar tamamen isteğe bağlı ve yol verileri'nda olduğunda ne kullanışlı yansıtır. Bu nedenle, `PaintSurface` işleyici bu yol ve çağrıları sınırları alır `Translate` ve `Scale` yolu ekranın merkezine taşımanızı ve neredeyse yüksekliğinde ekran yapmak için:


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

Ancak, yolu olmayan işlenir. Bunun yerine, dönüşümler yol ile bu bildirimi bir kırpma alanı ayarlamak için kullanılır:

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface` İşleyici ardından bir çağrı dönüşümleriyle sıfırlar `ResetMatrix` ve tam ekran yüksekliği için genişletmek için bit eşlem çizer. Bu kod bir bit eşlem bu belirli bir bit eşlem olan kare, olduğunu varsayar. Bit eşlem, yalnızca kırpma yolu tarafından tanımlanan alan içinde işlenir:

[![](clipping-images/monkeythroughkeyhole-small.png "Üç ekran görüntüsü anahtar deliği sayfası aracılığıyla Monkey")](clipping-images/monkeythroughkeyhole-large.png#lightbox "anahtar deliği sayfası aracılığıyla Monkey üç ekran görüntüsü")

Kırpma yolunu geçerli dönüşümler konu olduğunda `ClipPath` yöntemi çağrılır ve olmayan dönüşümler etkin olduğunda grafik nesnesini (örneğin, bir bit eşlem) görüntülenir. Kırpma yolu ile kaydedilen tuval durumu parçasıdır `Save` yöntemi ve geri yüklenen ile `Restore` yöntemi.

## <a name="combining-clipping-paths"></a>Kırpma yolları birleştirme

NET olarak söylemek gerekirse, kırpma alan "ayarlanmadı" `ClipPath` yöntemi. Bunun yerine, ekrana boyutları eşit dikdörtgen başlar mevcut kırpma yolu ile birleştirilir. Kırpma alanı kullanarak dikdörtgen sınırları edinebilirsiniz [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/) özelliği veya [ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/) özelliği. `ClipBounds` Özelliği döndürür bir `SKRect` herhangi dönüştüren yansıtan bir değer geçerli olabilir. `ClipDeviceBounds` Özelliği döndürür bir `RectI` değeri. Bu bir dikdörtgen ile tamsayı boyutları ve gerçek piksel boyutları kırpma alanında açıklar.

Tüm çağrı `ClipPath` kırpma alan yeni bir alan birleştirerek kırpma alanını azaltır. Tam sözdizimini [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/) yöntemdir:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Ayrıca bir [ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/) kırpma alan bir dikdörtgen ile birleştiren yöntemi:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Varsayılan olarak, varolan kırpma alan kesişimini sonuç kırpma alanıdır ve `SKPath` veya `SKRect` içinde belirtilen `ClipPath` veya `ClipRect` yöntemi. Bu gösterilmiştir **dört daireler kesişen küçük** sayfası. `PaintSurface` İşleyicisinde [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) sınıfı kullanır aynı `SKPath` kırpma alan art arda çağrılar yoluyla her biri azaltır, dört çakışan daire oluşturmak için nesne `ClipPath`:

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

Ne sol dört şu çevrelerdeki kesişimi şöyledir:

[![](clipping-images//fourcircleintersectclip-small.png "Üçlü sayfasının ekran görüntüsü dört daire kesişen küçük")](clipping-images/fourcircleintersectclip-large.png#lightbox "Üçlü sayfasının ekran görüntüsü dört daire kesişen küçük")

[ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/) Sabit listesi, yalnızca iki üyesi vardır:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) Belirtilen yol veya dikdörtgen var olan bir kırpma alanından kaldırır.

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) Belirtilen yol veya dikdörtgen mevcut kırpma alan ile kesişip

Dört değiştirirseniz `SKClipOperation.Intersect` değişkenlerinde `FourCircleIntersectClipPage` sınıfıyla `SKClipOperation.Difference`, aşağıdaki görüntüyle karşılaşırsınız:

[![](clipping-images//fourcircledifferenceclip-small.png "Üçlü sayfasının ekran görüntüsü dört daire kesişen küçük fark alma işlemi")](clipping-images/fourcircledifferenceclip-large.png#lightbox "Üçlü sayfasının ekran görüntüsü dört daire kesişen küçük fark alma işlemi")

Dört çakışan daire kırpma alanı'ndan kaldırıldı.

**Küçük Operations** sayfası yalnızca daireler çifti ile bu iki işlem arasındaki farkı gösterir. Sol taraftaki ilk daire varsayılan küçük işlemi ile kırpma alanı eklenir `Intersect`, ikinci daire sağdaki metin etiketi tarafından belirtilen küçük işlemi ile kırpma alanına eklenirken:

[![](clipping-images//clipoperations-small.png "Üçlü sayfasının ekran görüntüsü küçük Operations")](clipping-images/clipoperations-large.png#lightbox "Üçlü sayfasının ekran görüntüsü küçük işlemleri")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) Sınıfı tanımlayan iki `SKPaint` nesneleri alanları olarak ve ardından iki dikdörtgen alanlarına ekran böler. Bu alanlar telefonun dikey veya yatay modda olduğuna bağlı olarak farklılık gösterir. `DisplayClipOp` Sınıfı ardından görüntüler aramaları ve metin `ClipPath` her küçük işlem göstermek için iki daire yollarla:

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

Çağırma `DrawPaint` normalde ile doldurulacak tuvalin tamamını neden `SKPaint` nesne, ancak bu durumda, yöntemi yalnızca boyar kırpma alanı içinde.

## <a name="exploring-regions"></a>Bölgeleri keşfetme

API belgelerini denedikten varsa `SKCanvas`, aşırı fark etmiş olabilirsiniz `ClipPath` ve `ClipRect` ancak bunun yerine, yukarıda açıklanan yöntemlerden benzer şekilde yöntemlerine sahip adlı bir parametre [ `SKRegionOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/) yerine `SKClipOperation`. `SKRegionOperation` form kırpma bölgeleri için yolları birleştirme biraz daha fazla esneklik, altı üyeleri içerir:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

Ancak, aşırı yüklemeleri `ClipPath` ve `ClipRect` ile `SKRegionOperation` parametreleri artık kullanılmıyor ve bunlar kullanılamaz.

Kullanmaya devam edebilirsiniz `SKRegionOperation` numaralandırma ancak gerektirir bir kırpma alanı açısından tanımladığınız bir [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) nesne.

Yeni oluşturulan bir `SKRegion` nesnesi boş bir alan tanımlar. Nesne ilk çağrıda genellikle olan [ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/) dikdörtgen bir bölgesi açıklayan böylece. Parametre `SetRect` olduğu bir bir `SKRectI` değer &mdash; dikdörtgen değeri tamsayı özelliklere sahip. Ardından çağırabilirsiniz [ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/) ile bir `SKPath` nesne. Bu yolun iç aynıdır, ancak ilk dikdörtgen bölge için kırpılmış bir bölgesi oluşturur.

`SKRegionOperation` Numaralandırma birini çağırdığınızda oyuna yalnızca gelen [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/) gibi bu yöntem aşırı yüklemeleri:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Hale getirildiği bölge `Op` çağrıda bağlı bir parametre olarak belirtilen bölge ile birlikte `SKRegionOperation` üyesi. Son olarak bir bölge kırpma için uygun aldığınızda, tuval kullanarak kırpma alanı olarak ayarlayabilirsiniz [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/) yöntemi `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

Aşağıdaki ekran görüntüsünde, altı bölgede işlemleri temel kırpma alanlar gösterilmektedir. Sol daire bölgedir, `Op` yöntemi çağrıldığında ve doğru daire geçirilen bölgedir `Op` yöntemi:

[![](clipping-images//regionoperations-small.png "Üçlü sayfasının ekran görüntüsü bölge Operations")](clipping-images/regionoperations-large.png#lightbox "Üçlü sayfasının ekran görüntüsü bölge işlemleri")

Bu iki daire birleştirme bu tüm olasılıkları misiniz? Sonuçta elde edilen görüntü içinde görülen, kendileri tarafından üç bileşeni birleşimi olarak düşünün `Difference`, `Intersect`, ve `ReverseDifference` operations. Toplam birleşimleri iki üçüncü veya sekiz sayısıdır. Eksik olan iki bölge özgün olan (değil çağırma sonuçları `Op` hiç) ve tamamen boş bir bölge.

İlk olarak bu yolu bir yol ve bir bölge oluşturmanız ve daha sonra birden çok bölgede birleştirmeniz gerekir çünkü kırpma için bölge kullanmaya daha zor olan. Genel yapısını **bölge Operations** sayfasıdır çok benzer **küçük Operations** ancak [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) divides sınıfı ekranı altı alanlarına ve Bu proje için bölgeleri kullanmak için gereken ek iş gösterir:

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

İşte arasında büyük bir fark `ClipPath` yöntemi ve `ClipRegion` yöntemi:

> [!IMPORTANT]
> Farklı `ClipPath` yöntemi `ClipRegion` yöntemi dönüşümler tarafından etkilenmez.

Bu fark için stratejinin anlamak için bunu anlamak faydalıdır hangi bir bölgedir. Nasıl küçük işlemlerini ya da bölge işlemleri dahili olarak uygulanabilir hakkında düşündüğünüz, büyük olasılıkla çok karmaşık hatırlıyorum. Potansiyel olarak çok karmaşık çeşitli yolları birleştirilir ve sonuçta elde edilen yolu özetini büyük olasılıkla bir algoritmik onarımı kabus olur.

Ancak, bir dizi yatay tarama satırları, eski moda elektrikli boru TV de gibi her bir yol küçültülürse bu işin önemli ölçüde basitleştirilmiştir. Yalnızca yatay bir çizgi bir başlangıç noktası ve bir uç noktası ile her tarama satırıdır. Örneğin, her biri dairenin sol kısmından başlar ve doğru kısmından sona erer 20 yatay tarama satır içine bir daire ile bir RADIUS 10 ayrıştırılmasını. Başlangıç ve bitiş koordinatları, karşılık gelen tarama satırları her çift inceleme, tek gereken bunların olduğu için iki daire herhangi bir bölge işlemi ile birleştirerek çok basit olur.

Bunun ne olduğunu bir bölgedir: bir alan tanımlar yatay tarama satırları bir dizi.

Ancak, bir alan bir dizi tarama için zaman sınırlı satırları, bu tarama satırları belirli piksel boyuta göre temel alır. NET olarak söylemek gerekirse, bölge bir vektör grafik nesnesi değil. Doğası gereği bir sıkıştırılmış tek renkli bit eşlem bir yola yakın. Sonuç olarak, bölgeler ölçeği veya olamaz uygunluk kaybetmeden ve bu nedenle bunlar kırpma alanları için kullanıldığında dönüştürülür değil döndürülebilir.

Ancak, boyama amacıyla bölgelere dönüşümler uygulayabilirsiniz. **Bölge Boya** program Canlı bölge iç yapısını gösterir. [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) Sınıfı oluşturur bir `SKRegion` nesnesini temel alan bir `SKPath` 10 birim RADIUS daire. Dönüşüm sonra sayfayı doldurmak için bu daire genişletir:

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

`DrawRegion` Çağrı doldurur turuncu bölgede sırada `DrawPath` çağrı mavi karşılaştırma için orijinal yol konturlar:

[![](clipping-images//regionpaint-small.png "Üçlü sayfasının ekran görüntüsü bölge Boya")](clipping-images/regionpaint-large.png#lightbox "Üçlü sayfasının ekran görüntüsü bölge boyama")

Açıkça bir dizi ayrı koordinatları bölgedir.

Kırpma alanlarınızı bağlantılı olarak dönüşümler kullanma gerekmiyorsa, bölgeler kırpma için olarak kullanabileceğiniz **dört – yaprak Yonca** sayfasını gösterir. [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) Sınıf dört döngüsel bölgeleri bileşik bir bölgeden oluşturur, bu bileşik bölge kırpma alan olarak ayarlar ve ardından sayfanın Merkezi'nden yayılan 360 düz çizgiler bir dizi çizer:

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

Gerçekten bir dört – yaprak Yonca gibi görünüyor, ancak bu görüntüyü aksi kırpma olmadan oluşturulacak zor olabilir:

[![](clipping-images//fourleafclover-small.png "Üçlü sayfasının ekran görüntüsü dört – yaprak Yonca")](clipping-images/fourleafclover-large.png#lightbox "dört – yaprak Yonca sayfanın üç ekran görüntüsü")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
