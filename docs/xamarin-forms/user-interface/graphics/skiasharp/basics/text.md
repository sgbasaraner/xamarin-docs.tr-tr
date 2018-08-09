---
title: Metin ve grafikleri tümleştirme
description: Bu makalede metin SkiaSharp grafiklerle Xamarin.Forms uygulamalarınızı tümleştirmek için işlenen metin dizesi boyutunu belirlemek nasıl açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: be1524029ada79896f83517c3b439f2ad0e2c6d9
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615167"
---
# <a name="integrating-text-and-graphics"></a>Metin ve grafikleri tümleştirme

_Metin SkiaSharp grafik ile tümleştirmek için işlenen metin dizesi boyutunu belirlemek nasıl bakın_

Bu makalede, metin ölçmesine, büyük olasılıkla metnin belirli bir boyuta ölçeklendirin ve metin ile diğer grafikleri tümleştirme gösterilmektedir:

![](text-images/textandgraphicsexample.png "Dikdörtgenler tarafından çevrilmiş metin")

SkiaSharp `Canvas` sınıfı ayrıca bir dikdörtgen çizmek için yöntemler içerir ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) ve yuvarlak köşeli dikdörtgen ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Bu yöntemler olarak tanımlanması dikdörtgen gerektiren bir `SKRect` değeri.

**Çerçeveli metin** sayfa sayfa ve bir Yuvarlatılmış Dikdörtgen çiftinden oluşan bir çerçeve ile çevreleyen kısa metin dizesi ortalar. [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Sınıfı nasıl yapıldığını gösterir.

İçinde SkiaSharp kullandığınız `SKPaint` sınıf kümesi metin ve yazı tipi özniteliklerini, ancak ayrıca kullanabilirsiniz işlenen metin boyutunu elde edilir. Aşağıdaki başına `PaintSurface` olay işleyicisini çağırır iki farklı `MeasureText` yöntemleri. İlk [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) sahip basit bir çağrı `string` bağımsız değişkeni ve metnin piksel genişliği, geçerli yazı tipi özniteliklerini göre döndürür. Program daha sonra yeni hesaplar `TextSize` özelliği `SKPaint` nesne geçerli, işlenmiş genişliği tabanlı `TextSize` özelliği ve görüntü alanının genişliği. Bunu ayarlamak için kullanılmaya `TextSize` metin dizesini böylece ekran genişliği %90 yeniden işlenmek üzere:

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

İkinci [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) çağrı sahip bir `SKRect` bağımsız değişkeni için bir genişlik ve yükseklik işlenen metni alır. `Height` Bu özelliği `SKRect` değer, büyük harfler, Üst Çıkıntısı ve metin dizesindeki çıkıntılarını varlığını bağlıdır. Farklı `Height` değerler bildirilir metin dizelerini "mom", "cat" ve "köpek" Örneğin.

`Left` Ve `Top` özelliklerini `SKRect` yapısını metin olarak görüntüleniyorsa, işlenen metnin sol üst köşesinin koordinatlarını belirtmek bir `DrawText` çağıran 0 X ve Y konumunu. Örneğin, bu program çalışırken bir iPhone 7 simulator'da `TextSize` ilk çağrısından hesaplama sonucunda 90.6254 değeri atanır `MeasureText`. `SKRect` İkinci çağrısından alınan değer `MeasureText` aşağıdaki özellik değerleri vardır:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

X ve Y koordinatları etkilenebileceğini geçirmek için `DrawText` yöntemi sol tarafındaki taban metni belirtin. `Top` Değeri gösteren metin bu taban çizgisi ve (68 BT ekibinin 88 çıkarma) yukarıda 68 piksel genişletir temel aşağıda 20 piksel. `Left` 6 değerini gösteren metin 6 piksel olarak X değeri sağındaki başlar `DrawText` çağırın. Bu, normal arası karakter aralığı sağlar. Metin sıkıca ekranın sol üst köşesinde görüntülemek istiyorsanız, bu negatif geçirmek `Left` ve `Top` değerleri, X ve Y koordinatları olarak `DrawText`, bu örnekte, &ndash;6 ve 68.

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

`PaintSurface` İşleyici sonucuna iki çağrılarıyla `DrawRoundRect`, bağımsız değişkenleri gerektirir `SKRect`. Bu `SKRect` değeri benzer kesinlikle `SKRect` alınan değeri `MeasureText` yöntemi, ancak aynı olamaz. İlk olarak, metni kenarlarına Yuvarlatılmış dikdörtgen çizin değil ve ikincisi, alanda kaydırılmasına gerekir böylece biraz daha büyük olması gerekir böylece `Left` ve `Top` değerler dikdörtgen olduğu olması için sol üst köşesinin karşılık gelir konumlandırıldı. Bu iki iş yapılır `Offset` ve `Inflate` tarafından tanımlanan yöntemleri `SKRect`:

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

, Yöntemin geri kalanı rahatça olmasıdır. Başka bir oluşturur `SKPaint` Kenarlıklar ve aramalar için nesne `DrawRoundRect` iki kez. İkinci çağrı, başka bir 10 piksel şişirileceğini dikdörtgen kullanır. İlk çağrı, bir köşe yarıçapı 20 piksel belirtir. paralel olarak göründüğü şekilde ikinci bir köşe yarıçapı 30 piksel sahiptir:

 [![](text-images/framedtext-small.png "Üçlü sayfasının ekran görüntüsü Çerçeveli metin")](text-images/framedtext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Çerçeveli metin")

Metin ve çerçeve boyutunda artış görmek için telefonunuzu veya simülatör yan çevirerek kapatabilirsiniz.

Merkezi metin ekranında gerekirse, yaklaşık olarak ayarlayarak metnin ölçme olmadan bunu yapabilirsiniz `TextAlign` özelliği `SKPaint` için `SKTextAlign.Center`. Belirttiğiniz X koordinatı `DrawText` yöntemi ardından gösteren metnin yatay merkezine nereye konumlandırılır. Orta ekranının geçirirseniz `DrawText` yöntemi metni yatay olarak ortalanır ve *neredeyse* temel dikey ortalanmış olacağını çünkü dikey orta.

Metnin kendisini çok bir grafik seçeneği gibi ele alınabilir. Normal doldurulmuş görünen yerine metin karakterleri anahattını görüntülemek için basit bir seçenek verilmiştir:

[![](text-images/outlinedtext-small.png "Üç ana hatlarıyla belirtilen metin sayfasının ekran görüntüsü")](text-images/outlinedtext-large.png#lightbox "üç ana hatlarıyla belirtilen metin sayfasının ekran görüntüsü")

Bu normal değiştirerek yapılır `Style` özelliği `SKPaint` kendi varsayılan nesneden `SKPaintStyle.Fill` için `SKPaintStyle.Stroke` ve darbe genişliği belirterek. `PaintSurface` İşleyicisi **özetlenen metin** sayfası nasıl yapıldığını gösterir:

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

 Son birkaç makalelerde, sahip olduğunuz metin çizmek için SkiaSharp kullanma gördüğünüze daire, üç nokta ve Yuvarlatılmış Dikdörtgen Sonraki adım [SkiaSharp satırları ve yolları](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) içinde bir grafik yolu bağlı çizgi çizmek hakkında bilgi edineceksiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
