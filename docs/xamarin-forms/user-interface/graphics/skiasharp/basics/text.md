---
title: Metin ve grafikleri tümleştirme
description: Bu makalede metin SkiaSharp grafiklerle Xamarin.Forms uygulamalarınızı tümleştirmek için işlenen metin dizesi boyutunu belirlemek nasıl açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 45b959c2d9b40cee4d86eb5eefaad724d857b4b9
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615167"
---
# <a name="integrating-text-and-graphics"></a>Metin ve grafikleri tümleştirme

_Metin SkiaSharp grafik ile tümleştirmek için işlenen metin dizesi boyutunu belirlemek nasıl bakın_

Bu makalede, metin ölçmesine, metnin belirli bir boyuta ölçeklendirin ve metin ile diğer grafikleri tümleştirme gösterilmektedir:

![](text-images/textandgraphicsexample.png "Dikdörtgenler tarafından çevrilmiş metin")

Bu görüntüyü bir Yuvarlatılmış Dikdörtgen de içerir. SkiaSharp `Canvas` sınıfı içerir [ `DrawRect` ](xref:SkiaSharp.SKCanvas.DrawRect*) bir dikdörtgen çizmek için kullanılan yöntemleri ve [ `DrawRoundRect` ](xref:SkiaSharp.SKCanvas.DrawRoundRect*) yuvarlak köşeler bir dikdörtgen çizmek için yöntemleri. Bu yöntemler olarak tanımlanması dikdörtgen izin bir `SKRect` değeri ya da başka şekillerde.

**Çerçeveli metin** sayfa sayfa ve bir Yuvarlatılmış Dikdörtgen çiftinden oluşan bir çerçeve ile çevreleyen kısa metin dizesi ortalar. [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Sınıfı nasıl yapıldığını gösterir.

SkiaSharp kullandığınız `SKPaint` sınıf kümesi metin ve yazı tipi özniteliklerini, ancak ayrıca kullanabilirsiniz işlenen metin boyutunu elde edilir. Aşağıdaki başına `PaintSurface` olay işleyicisini çağırır iki farklı `MeasureText` yöntemleri. İlk [ `MeasureText` ](xref:SkiaSharp.SKPaint.MeasureText(System.String)) sahip basit bir çağrı `string` bağımsız değişkeni ve metnin piksel genişliği, geçerli yazı tipi özniteliklerini göre döndürür. Program daha sonra yeni hesaplar `TextSize` özelliği `SKPaint` nesne geçerli, işlenmiş genişliği tabanlı `TextSize` özelliği ve görüntü alanının genişliği. Bu hesaplama kümesi amaçlanmıştır `TextSize` metin dizesini böylece ekran genişliği %90 yeniden işlenmek üzere:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

İkinci [ `MeasureText` ](xref:SkiaSharp.SKPaint.MeasureText(System.String,SkiaSharp.SKRect@)) çağrı sahip bir `SKRect` bağımsız değişkeni için bir genişlik ve yükseklik işlenen metni alır. `Height` Bu özelliği `SKRect` değer, büyük harfler, Üst Çıkıntısı ve metin dizesindeki çıkıntılarını varlığını bağlıdır. Farklı `Height` değerler bildirilir metin dizelerini "mom", "cat" ve "köpek" Örneğin.

`Left` Ve `Top` özelliklerini `SKRect` yapısını metin olarak görüntüleniyorsa, işlenen metnin sol üst köşesinin koordinatlarını belirtmek bir `DrawText` çağıran 0 X ve Y konumunu. Örneğin, bu program çalışırken bir iPhone 7 simulator'da `TextSize` ilk çağrısından hesaplama sonucunda 90.6254 değeri atanır `MeasureText`. `SKRect` İkinci çağrısından alınan değer `MeasureText` aşağıdaki özellik değerleri vardır:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

X ve Y koordinatları etkilenebileceğini geçirmek için `DrawText` yöntemi sol tarafındaki taban metni belirtin. `Top` Değeri gösteren metin bu taban çizgisi ve (68 BT ekibinin 88 çıkarma) yukarıda 68 piksel genişletir temel aşağıda 20 piksel. `Left` 6 değerini gösteren metin altı piksel olarak X değeri sağındaki başlar `DrawText` çağırın. Bu, normal arası karakter aralığı sağlar. Metin sıkıca ekranın sol üst köşesinde görüntülemek istiyorsanız, bu negatif geçirmek `Left` ve `Top` değerleri, X ve Y koordinatları olarak `DrawText`, bu örnekte, &ndash;6 ve 68.

`SKRect` Yapısını tanımlayan çeşitli kullanışlı özellikler ve yöntemler, kalan içinde kullanılan bazıları `PaintSurface` işleyici. `MidX` Ve `MidY` değerler dikdörtgenin merkezi koordinatlarını belirtir. (İPhone 7 örnekte bu 338.4107 değerler ve &ndash;24.) Aşağıdaki kod merkezi metin ekranındaki için koordinat kolay hesaplama için bu değerleri kullanır:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

`SKImageInfo` De bilgi yapısını tanımlayan bir [ `Rect` ](xref:SkiaSharp.SKImageInfo.Rect) türünün özelliği `SKRect`, ayrıca hesaplayabilirsiniz `xText` ve `yText` şöyle:

```csharp
float xText = info.Rect.MidX - textBounds.MidX;
float yText = info.Rect.MidY - textBounds.MidY;
```

`PaintSurface` İşleyici sonucuna iki çağrılarıyla `DrawRoundRect`, bağımsız değişkenleri gerektirir `SKRect`. Bu `SKRect` değerini temel alarak `SKRect` alınan değeri `MeasureText` yöntemi, ancak aynı olamaz. İlk olarak, bu metni kenarlarına Yuvarlatılmış dikdörtgen çizin vermez böylece biraz daha büyük olması gerekir. İkincisi, alanda kaydırılmasına gerekir böylece `Left` ve `Top` değerler olduğu dikdörtgenin yerleştirilmesini sol üst köşesinin karşılık gelir. Bu iki iş yapılır [ `Offset` ](xref:SkiaSharp.SKRect.Offset*) ve [ `Inflate` ](xref:SkiaSharp.SKRect.Inflate*) tarafından tanımlanan yöntemleri `SKRect`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

, Yöntemin geri kalanı rahatça olmasıdır. Başka bir oluşturur `SKPaint` Kenarlıklar ve aramalar için nesne `DrawRoundRect` iki kez. İkinci çağrı, başka bir 10 piksel şişirileceğini dikdörtgen kullanır. İlk çağrıda bir köşe yarıçapı 20 piksel olarak belirtir. Paralel olarak göründüğü şekilde ikinci bir köşe yarıçapı 30 piksel sahiptir:

 [![](text-images/framedtext-small.png "Üçlü sayfasının ekran görüntüsü Çerçeveli metin")](text-images/framedtext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Çerçeveli metin")

Metin ve çerçeve boyutunda artış görmek için telefonunuzu veya simülatör yan çevirerek kapatabilirsiniz.

Merkezi metin ekranında gerekirse, yaklaşık metin ölçme olmadan yapabilirsiniz. Bunun yerine, [ `TextAlign` ](xref:SkiaSharp.SKPaint.TextAlign) özelliği `SKPaint` numaralandırma üyesine [ `SKTextAlign.Center` ](xref:SkiaSharp.SKTextAlign). Belirttiğiniz X koordinatı `DrawText` yöntemi ardından gösteren metnin yatay merkezine nereye konumlandırılır. Orta ekranının geçirirseniz `DrawText` yöntemi metni yatay olarak ortalanır ve *neredeyse* temel dikey ortalanmış olacağını çünkü dikey orta.

Metin çok diğer grafik nesnesi gibi ele alınabilir. Bir basit metin karakterleri anahattını görüntülemek için bir seçenektir:

[![](text-images/outlinedtext-small.png "Üç ana hatlarıyla belirtilen metin sayfasının ekran görüntüsü")](text-images/outlinedtext-large.png#lightbox "Triple screenshot of the Outlined Text page")

Bu normal değiştirerek gerçekleştirilir `Style` özelliği `SKPaint` kendi varsayılan nesneden `SKPaintStyle.Fill` için `SKPaintStyle.Stroke`ve darbe genişliği belirterek. `PaintSurface` İşleyicisi **özetlenen metin** sayfası nasıl yapıldığını gösterir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

Başka bir genel grafik bit eşlem nesnedir. Derinlik bölümünde ele büyük bir konu olduğu [ **SkiaSharp bit eşlemler**](../bitmaps/index.md), ancak sonraki makalede, [ **SkiaSharp bit eşlem temellerini**](bitmaps.md), briefer bir giriş sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
