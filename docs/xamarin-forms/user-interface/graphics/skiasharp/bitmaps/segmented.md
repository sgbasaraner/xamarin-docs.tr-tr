---
title: SkiaSharp bit eşlem segmentli görüntüleme
description: SkiaSharp bit eşlem bazı alan uzatılır ve bazı alanlar olmayan görüntüler.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79AE2033-C41C-4447-95A6-76D22E913D19
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: e5bfa076a8746abd6275e9d7a8393c7c8ab53294
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615242"
---
# <a name="segmented-display-of-skiasharp-bitmaps"></a>SkiaSharp bit eşlem segmentli görüntüleme

SkiaSharp `SKCanvas` nesne adında bir yöntemi tanımlar `DrawBitmapNinePatch` adlı iki yöntem `DrawBitmapLattice` , da oldukça benzerdir. Hem bu yöntemler bir bit eşlemi hedef dikdörtgenin boyutlarına işlemek, ancak bit eşlem birörnek esnetme amacıyla, yerine bölümleri bit eşlemin piksel boyutlarıyla görüntüler ve böylece dikdörtgen sığar diğer bölümlerini bir bit eşlem esnetme:

![Örnekleri bölümlenmiş](segmented-images/SegmentedSample.png "bölümlenmiş örnek")

Bu yöntemler, bit eşlemler düğmeler gibi kullanıcı arabirimi nesneleri parçalarını oluşturan işlemeye yönelik genel olarak kullanılır. Genellikle, düğmenin içeriğine göre için bir düğmenin boyutu istediğiniz düğme tasarlarken, ancak büyük olasılıkla düğmenin kenarlık düğmenin içerik bağımsız olarak aynı genişliğe olmasını istediğiniz. İdeal bir uygulamayı olan `DrawBitmapNinePatch`.

`DrawBitmapNinePatch` özel bir durumdur, `DrawBitmapLattice` ancak kullanın ve anlamak için iki yöntem daha kolay.

## <a name="the-nine-patch-display"></a>Dokuz düzeltme eki görüntüleme 

Kavramsal olarak, [ `DrawBitmapNinePatch` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapNinePatch/p/SkiaSharp.SKBitmap/SkiaSharp.SKRectI/SkiaSharp.SKRect/SkiaSharp.SKPaint/) bir bit eşlem dokuz dikdörtgenler ayırır:

![Dokuz düzeltme eki](segmented-images/NinePatch.png "dokuz düzeltme eki")

Dört köşelere dikdörtgenler kendi piksel boyutunda görüntülenir. Bit eşlem kenarlarına diğer alanlara oklar göstermek gibi yatay veya dikey olarak hedef dikdörtgenin alana uzatılır. Dikdörtgenin merkezinde yatay ve dikey olarak genişletilir. 

Değil yeterli alan yoksa bile dört köşe, piksel boyutta görüntülemek için hedef dikdörtgenin içindeki, bunlar kullanılabilir boyutu ve hiçbir şey ölçeklenir ancak farkına görüntülenir.

Bir bit eşlem bu dokuz dikdörtgenler bölmek için yalnızca dikdörtgen Merkezi'nde belirtmek gereklidir. Bu sözdizimi şöyledir `DrawBitmapNinePatch` yöntemi:

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

Orta dikdörtgen bit eşlemi göreli olur. Bu bir `SKRectI` değer (tamsayı sürümünü `SKRect`) ve tüm boyutları ve koordinatları piksel birimleri cinsinden. Hedef dikdörtgenin göre görünen yüzeyidir. `paint` Bağımsız değişken isteğe bağlıdır.

**Dokuz düzeltme eki görüntüleme** sayfasını [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) örnek ilk statik Oluşturucu bir ortak statik özelliği türü oluşturmak için kullanır `SKBitmap`:

```csharp
public partial class NinePatchDisplayPage : ContentPage
{
    static NinePatchDisplayPage()
    {
        using (SKCanvas canvas = new SKCanvas(FiveByFiveBitmap))
        using (SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 10
        })
        {
            for (int x = 50; x < 500; x += 100)
                for (int y = 50; y < 500; y += 100)
                {
                    canvas.DrawCircle(x, y, 40, paint);
                }
        }
    }

    public static SKBitmap FiveByFiveBitmap { get; } = new SKBitmap(500, 500);
    ···
}
```

Bu makalede iki sayfa aynı, bit eşlem kullanın. Bit eşlem 500 piksel kare ve 25 daireler dizisi aynı boyut, her 100 piksel bir kare alanı kaplayan oluşur:

![Daire kılavuz](segmented-images/CircleGrid.png "daire kılavuz")

Programın örnek oluşturucusu oluşturur bir `SKCanvasView` ile bir `PaintSurface` kullanan işleyici `DrawBitmapNinePatch` , tüm görüntü yüzeyine esnetilmiş bit eşlem görüntülemek için:

```csharp
public class NinePatchDisplayPage : ContentPage
{
    ···
    public NinePatchDisplayPage()
    {
        Title = "Nine-Patch Display";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRectI centerRect = new SKRectI(100, 100, 400, 400);
        canvas.DrawBitmapNinePatch(FiveByFiveBitmap, centerRect, info.Rect);
    }
}
```

`centerRect` Dikdörtgen 16 daireler merkezi dizisi kapsar. Bulunan daireler kendi piksel boyutları olarak görüntülenir ve diğer her şey uygun şekilde genişletilir:

[![Dokuz düzeltme eki görüntüleme](segmented-images/NinePatchDisplay.png "dokuz düzeltme eki görüntüleme")](segmented-images/NinePatchDisplay-Large.png#lightbox)

UWP sayfası 500 piksel genişliğinde olması ve bu nedenle üst ve alt satırları daireler aynı boyutta bir dizi olarak görüntüler. Aksi takdirde köşeleri olmayan tüm daireler için form üç nokta uzatılır.

Böylece satırlar ve sütunlar daireler çakışıyor merkezi dikdörtgen tanımlama garip bir daire ve elipsler bileşiminden oluşan nesneler için görüntüsünü deneyin:

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>Kafes görüntüleme

İki `DrawBitmapLattice` yöntemlerdir benzer `DrawBitmapNinePatch`, ancak bunlar herhangi bir yatay veya dikey bölümler sayıda genelleştirilmiş. Bu bölüm, piksele karşılık gelen bir tamsayı dizileri tarafından tanımlanır. 

[ `DrawBitmapLattice` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapLattice/p/SkiaSharp.SKBitmap/System.Int32[]/System.Int32[]/SkiaSharp.SKRect/SkiaSharp.SKPaint/) Parametrelerle yöntemi bu dizileri tamsayılar için çalışmaya görünen değil. [ `DrawBitmapLattice` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapLattice/p/SkiaSharp.SKBitmap/SkiaSharp.SKLattice/SkiaSharp.SKRect/SkiaSharp.SKPaint/) Türünde bir parametreye yöntem `SKLattice` çalışır, ve aşağıda gösterilen örnekte kullanılan olmasıdır.

[ `SKLattice` ](https://developer.xamarin.com/api/type/SkiaSharp.SKLattice/) Dört özellik yapısını tanımlar:

- [`XDivs`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.XDivs/), tamsayı dizisi
- [`YDivs`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.YDivs/), tamsayı dizisi
- [`Flags`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.Flags/), bir dizi `SKLatticeFlags`, bir numaralandırma türü
- [`Bounds`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.Bounds/) tür `Nullable<SKRectI>` bit eşlem içinde bir isteğe bağlı kaynak dikdörtgenin belirtmek için

`XDivs` Dizi Dikey Şeritler bit eşlem genişliği böler. Piksel 0 sol gelen ilk Şerit genişletir `XDivs[0]`. Bu Şerit piksel genişliği işlenir. İkinci Şerit alanından genişletir `XDivs[0]` için `XDivs[1]`ve genişletilir. Üçüncü Şerit alanından genişletir `XDivs[1]` için `XDivs[2]` ve piksel genişliği işlenir. Son Şerit dizinin son öğeden bir bit eşlem sağ kenarına genişletir. Dizi öğelerin bir çift sayı ise, piksel genişliği görüntülenir. Aksi takdirde, uzatılır. Biridir Dikey Şeritler toplam sayısı dizideki öğelerin sayısından daha fazla.

`YDivs` Dizi benzerdir. Bu dizi yüksekliğini yatay şeritler böler.

Birlikte `XDivs` ve `YDivs` dizi bit eşlem dikdörtgenler bölün. Dikdörtgenler sayısını ürüne yatay şeritler sayısı ve Dikey Şeritler sayısı eşittir.

Skia belgelere göre `Flags` en üst satır, dikdörtgenlerin sonra ekleyelim ve benzeri dizi için her bir dikdörtgen bir öğesinin ilk içeriyor. `Flags` Dizidir türünü [ `SKLatticeFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKLatticeFlags/), aşağıdaki üyeleri olan bir sabit listesi:

- `Default` 0 değeri ile
- `Transparent` değer 1 ile

Ancak, bu bayrakları için beklenen ve bunların yoksayılması en iyi şekilde çalışması için görülüyor. Ancak ayarlamamanız `Flags` özelliğini `null`. Bir dizisi olarak ayarlamak `SKLatticeFlags` dikdörtgenler toplam sayısı kapsayacak şekilde büyük değerler.

**Dokuz kafes Patch** sayfasında kullandığı `DrawBitmapLattice` taklit edecek şekilde `DrawBitmapNinePatch`. Oluşturulan aynı bit eşlem kullanan `NinePatchDisplayPage`:

```csharp
public class LatticeNinePatchPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeNinePatchPage ()
    {
        Title = "Lattice Nine-Patch";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    `
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 400 };
        lattice.YDivs = new int[] { 100, 400 };
        lattice.Flags = new SKLatticeFlags[9]; 

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

Hem `XDivs` ve `YDivs` özellikleri diziler yalnızca iki tamsayı, bit eşlem üç şeritler yatay ve dikey bölme için ayarlanır: (esnetilmiş) piksele 0 (piksel boyutunda işlenmiş), 100 piksel 100 piksel 400'den gelen ve pikselden 400 piksel 500 (piksel boyutunu). Birlikte `XDivs` ve `YDivs` boyutu toplam 9 dikdörtgenlerin tanımlayın, `Flags` dizisi. Yalnızca bir dizi oluşturma, bir dizi oluşturmak yeterli `SKLatticeFlags.Default` değerleri.

Görüntü, önceki programa aynıdır:

[![Dokuz-Patch kafes](segmented-images/LatticeNinePatch.png "dokuz-Patch kafes")](segmented-images/LatticeNinePatch-Large.png#lightbox)

**Kafes görünen** sayfası, bit eşlem 16 dikdörtgenler böler:

```csharp
public class LatticeDisplayPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeDisplayPage()
    {
        Title = "Lattice Display";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 200, 400 };
        lattice.YDivs = new int[] { 100, 300, 400 };

        int count = (lattice.XDivs.Length + 1) * (lattice.YDivs.Length + 1);
        lattice.Flags = new SKLatticeFlags[count];

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

`XDivs` Ve `YDivs` dizilerdir oldukça önceki örnek olarak simetrik olmaması için ekranı neden biraz farklı:

[![Kafes görünen](segmented-images/LatticeDisplay.png "kafes görüntüleme")](segmented-images/LatticeDisplay-Large.png#lightbox)

İOS ve Android görüntülerinin soldaki, yalnızca küçük daire kendi piksel boyutunda işlenir. Diğer her şey uzatılır.

**Kafes görünen** sayfa oluşturulmasını genelleştirir `Flags` denemeler olanak tanıyan bir dizi `XDivs` ve `YDivs` daha kolay. Özellikle, ilk öğesinin ne görmek isteyebilirsiniz `XDivs` veya `YDivs` 0 dizisi. 

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
