---
title: Yollar ve bölgeler ile kırpma
description: Bu makalede belirli alanlara küçük grafik SkiaSharp yollarını kullanın ve bölgeler oluşturmak için nasıl açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 87f1ad3956bdb43c82a7ab57ea9171e9a28dd558
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615294"
---
# <a name="clipping-with-paths-and-regions"></a>Yollar ve bölgeler ile kırpma

_Küçük grafik yolları belirli alanları ve bölgeler oluşturmak için kullanın._

Bazen, belirli bir alandaki grafik işlenmesini kısıtlamak gereklidir. Bu olarak bilinir *kırpma*. Bu bir anahtar deliği görülen bir monkey görüntüsü gibi özel efekt için kırpma kullanabilirsiniz:

![](clipping-images/clippingsample.png "Bir anahtar deliği aracılığıyla Monkey")

*Kırpma alan* grafik işlenir ekran alanıdır. Kırpma bölgesinin dışında görünür herhangi bir şey yazdırılmaz. Kırpma alanı genellikle bir dikdörtgen ile tanımlanan veya [ `SKPath` ](xref:SkiaSharp.SKPath) nesne, ancak bunun yerine tanımlayabilirsiniz kırpma alanı kullanarak bir [ `SKRegion` ](xref:SkiaSharp.SKRegion) nesne. Bir yoldan oluşturabileceğiniz bir bölge olduğundan bu iki tür nesneler ilk ilgili gibi görünüyor. Ancak, bir bölgeden bir yol oluşturulamıyor ve dahili olarak çok farklı olan: bölge, bir dizi yatay tarama satırları tarafından tanımlı bir yol çizgiler ve eğrilerle, bir dizi oluşur.

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

NET olarak söylemek gerekirse, kırpma alan "ayarlanmadı" `ClipPath` yöntemi. Bunun yerine, tuvale boyutları eşit dikdörtgen başlar mevcut kırpma yolu ile birleştirilir. Kırpma alanı kullanarak dikdörtgen sınırları edinebilirsiniz [ `ClipBounds` ](xref:SkiaSharp.SKCanvas.ClipBounds) özelliği veya [ `ClipDeviceBounds` ](xref:SkiaSharp.SKCanvas.ClipDeviceBounds) özelliği. `ClipBounds` Özelliği döndürür bir `SKRect` herhangi dönüştüren yansıtan bir değer geçerli olabilir. `ClipDeviceBounds` Özelliği döndürür bir `RectI` değeri. Bu bir dikdörtgen ile tamsayı boyutları ve gerçek piksel boyutları kırpma alanında açıklar.

Tüm çağrı `ClipPath` kırpma alan yeni bir alan birleştirerek kırpma alanını azaltır. Tam sözdizimini [ `ClipPath` ](xref:SkiaSharp.SKCanvas.ClipPath(SkiaSharp.SKPath,SkiaSharp.SKClipOperation,System.Boolean)) yöntemdir:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Ayrıca bir [ `ClipRect` ](xref:SkiaSharp.SKCanvas.ClipRect(SkiaSharp.SKRect,SkiaSharp.SKClipOperation,System.Boolean)) kırpma alan bir dikdörtgen ile birleştiren yöntemi:

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

[ `SKClipOperation` ](xref:SkiaSharp.SKClipOperation) Sabit listesi, yalnızca iki üyesi vardır:

- `Difference` Belirtilen yol veya dikdörtgen var olan bir kırpma alanından kaldırır.

- `Intersect` Belirtilen yol veya dikdörtgen mevcut kırpma alan ile kesişip

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

Bir kırpma alanı açısından da tanımlayabilirsiniz bir [ `SKRegion` ](xref:SkiaSharp.SKRegion) nesne.

Yeni oluşturulan bir `SKRegion` nesnesi boş bir alan tanımlar. Nesne ilk çağrıda genellikle olan [ `SetRect` ](xref:SkiaSharp.SKRegion.SetRect(SkiaSharp.SKRectI)) böylece dikdörtgen bir bölgesi açıklar. Parametre `SetRect` olduğu bir `SKRectI` değer &mdash; tamsayı değerine sahip bir dikdörtgen koordinatları çünkü dikdörtgenin piksel cinsinden belirtir. Ardından çağırabilirsiniz [ `SetPath` ](xref:SkiaSharp.SKRegion.SetPath(SkiaSharp.SKPath,SkiaSharp.SKRegion)) ile bir `SKPath` nesne. Bu yolun iç aynıdır, ancak ilk dikdörtgen bölge için kırpılmış bir bölgesi oluşturur.

Bölgeyi de birini çağırarak değiştirilebilir [ `Op` ](xref:SkiaSharp.SKRegion.Op*) gibi bu yöntem aşırı yüklemeleri:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

[ `SKRegionOperation` ](xref:SkiaSharp.SKRegionOperation) Numaralandırma benzer `SKClipOperation` ancak daha fazla üyesi vardır:

- `Difference`

- `Intersect`

- `Union`

- `XOR`

- `ReverseDifference`

- `Replace`

Hale getirildiği bölge `Op` çağrıda bağlı bir parametre olarak belirtilen bölge ile birlikte `SKRegionOperation` üyesi. Son olarak bir bölge kırpma için uygun aldığınızda, tuval kullanarak kırpma alanı olarak ayarlayabilirsiniz [ `ClipRegion` ](xref:SkiaSharp.SKCanvas.ClipRegion(SkiaSharp.SKRegion,SkiaSharp.SKClipOperation)) yöntemi `SKCanvas`:

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

Bu iş, bir dizi yatay tarama satırları, eski moda elektrikli boru TV de gibi her bir yol küçültülürse önemli ölçüde basitleştirilmiştir. Yalnızca yatay bir çizgi bir başlangıç noktası ve bir uç noktası ile her tarama satırıdır. Örneğin, bir RADIUS 10 piksel daire satırlarına her biri dairenin sol kısmından başlar ve doğru kısmından sona erer 20 yatay tarama, ayrılmış. Başlangıç ve bitiş koordinatları, karşılık gelen tarama satırları her çift inceleme, tek gereken bunların olduğu için iki daire herhangi bir bölge işlemi ile birleştirerek çok basit olur.

Bunun ne olduğunu bir bölgedir: bir alan tanımlayın yatay tarama satırları bir dizi.

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

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
