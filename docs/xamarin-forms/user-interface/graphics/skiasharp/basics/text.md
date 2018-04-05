---
title: Metin ve grafik tümleştirme
description: Bkz. metin SkiaSharp grafik ile tümleştirmek için işlenen metin dizesi boyutunu belirlemek için
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 4ef9f1b634d2ecfa73a94bfd562a68593dfdc575
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="integrating-text-and-graphics"></a>Metin ve grafik tümleştirme

_Bkz. metin SkiaSharp grafik ile tümleştirmek için işlenen metin dizesi boyutunu belirlemek için_

Bu makalede, metin ölçmek, büyük olasılıkla metnin belirli bir boyuta ölçeklendirme ve diğer grafiklerle metin tümleştirmek gösterilmiştir:

![](text-images/textandgraphicsexample.png "Dikdörtgenler tarafından metne")

SkiaSharp `Canvas` sınıfı ayrıca bir dikdörtgen çizmek için yöntemler içerir ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) ve yuvarlak köşeli dikdörtgen ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Bu yöntemleri olarak tanımlanması için dikdörtgen gerektiren bir `SKRect` değeri.

**Çerçeveli metin** sayfa merkezleri sayfası ve bir çerçeve ile yuvarlak dikdörtgen çiftinden oluşan çevreleyen kısa bir metin dizesi. [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Sınıfı nasıl yapıldığını gösterir.

İçinde SkiaSharp kullandığınız `SKPaint` sınıfı kümesi metin ve yazı tipi özniteliklerini, ancak Ayrıca, İşlenmiş metin boyutunu almak üzere kullanabilir. Aşağıdaki başlangıcını `PaintSurface` olay işleyici çağırması iki farklı `MeasureText` yöntemleri. İlk [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) çağrısı sahip basit bir `string` bağımsız değişkeni ve metin piksel genişliği göre geçerli yazı tipi özniteliklerini döndürür. Program sonra yeni hesaplar `TextSize` özelliği `SKPaint` nesne tabanlı geçerli bu işlenmiş genişliği `TextSize` özelliği ve görüntü alanının genişliği. Bunu ayarlamak için tasarlanmıştır `TextSize` metin dizesini böylece ekran genişliği % 90'ını işlenmek üzere:

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
    SKRect textBounds;
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

İkinci [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) çağrısı sahip bir `SKRect` genişlik ve yükseklik İşlenmiş metin alacağı için bağımsız değişken. `Height` Bu özelliği `SKRect` değeri, büyük harfler, Üst Çıkıntısı ve metin dizesindeki harfin alt varlığını bağlıdır. Farklı `Height` değerleri raporlanır metin dizelerini "mom", "kat" ve "köpek" Örneğin.

`Left` Ve `Top` özelliklerini `SKRect` yapısını belirtmek işlenmiş metnin sol üst köşesinin koordinatlarını metin olarak görüntüleniyorsa, bir `DrawText` çağıran 0 X ve Y konumunu. Örneğin, bu program çalışırken bir iPhone 7 simulator'da `TextSize` ilk çağrısından hesaplama sonucunda 90.6254 değeri atanmış `MeasureText`. `SKRect` İçin ikinci çağrısından alınan değer `MeasureText` aşağıdaki özellik değerleri içeriyor:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

X ve Y koordinatları unutmayın geçirmek için `DrawText` yöntemi solunda taban metni belirtin. `Top` Değeri gösterir metin bu taban çizgisi ve (68 BT 88 alanından çıkarılmasıyla) yukarıdaki 68 piksel kadar taşıyor taban aşağıda 20 piksel. `Left` 6 değeri gösterir metin X değeri sağındaki 6 piksel başlar `DrawText` çağırın. Bu, normal karakterler arası boşluk için sağlar. Metin sıkıca ekranın sol üst köşesinde görüntülemek istiyorsanız, bunların negatif geçirmek `Left` ve `Top` değerleri, X ve Y koordinatları olarak `DrawText`, bu örnekte, &ndash;6 ve 68.

`SKRect` Yapısını tanımlar birkaç kullanışlı özellikleri ve yöntemleri, geri kalan içinde kullanılan bazıları `PaintSurface` işleyicisi. `MidX` Ve `MidY` değerleri dikdörtgen merkezi koordinatlarını gösterir. (7 iPhone örnekte bu 338.4107 değerler ve &ndash;24.) Aşağıdaki kod ekranındaki metinleri merkezi için koordinatları kolay hesaplama için bu değerleri kullanır:

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

`PaintSurface` İşleyici sonucuna iki çağrıları ile `DrawRoundRect`, her ikisi de bağımsız gerektiren `SKRect`. Bu `SKRect` değeri benzer kesinlikle `SKRect` alınan değer `MeasureText` yöntemi, ancak aynı olamaz. İlk olarak, bu metnin kenarları yuvarlak dikdörtgen çizmek değil ve ikincisi, alan gölgeye gerekir böylece biraz daha büyük olması gerekir böylece `Left` ve `Top` değerler olduğu dikdörtgen olması sol üst köşesindeki karşılık gelir konumlandırıldı. Bu iki işleri yapılır `Offset` ve `Inflate` tarafından tanımlanan yöntemler `SKRect`:

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

, Yöntem kalanı düz İleri olmasıdır. Başka bir oluşturur `SKPaint` Kenarlıklar ve aramalar için nesne `DrawRoundRect` iki kez. İkinci çağrı başka bir 10 piksel şişirileceğini dikdörtgen kullanır. İlk çağrıda 20 piksel köşe yarıçapını belirler; İkinci bir köşe yarıçapı 30 piksel sahiptir, bu nedenle bunlar paralel görünüyor:

 [![](text-images/framedtext-small.png "Üçlü sayfasının ekran görüntüsü Çerçeveli metin")](text-images/framedtext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Çerçeveli metin")

Metin ve çerçeve boyutunda artış görmek için telefon veya yanları simulator kapatabilirsiniz.

Yalnızca bazı metinleri ekranında merkezi gerekiyorsa, yaklaşık olarak ayarlayarak metin ölçme olmadan bunu yapabilirsiniz `TextAlign` özelliği `SKPaint` için `SKTextAlign.Center`. Belirttiğiniz X koordinatı `DrawText` yöntemi sonra gösteren metnin yatay ortada nereye konumlandırılır. Ekrana Orta geçirirseniz `DrawText` yöntemi, metnin yatay olarak ortalanmış ve *neredeyse* taban dikey ortalanmış olduğundan dikey olarak ortalanır.

Metnin kendisi çok bir grafik seçeneği ele alınabilir. Normal doldurulmuş görüntü yerine metin karakterleri özetini görüntülemek için bir basit seçenek verilmiştir:

[![](text-images/outlinedtext-small.png "Üçlü özetlenen metin sayfasının ekran görüntüsü")](text-images/outlinedtext-large.png#lightbox "üç ana hatlarıyla metin sayfasının ekran görüntüsü")

Bu sadece normal değiştirilerek yapılır `Style` özelliği `SKPaint` kendi varsayılan nesnesinden `SKPaintStyle.Fill` için `SKPaintStyle.Stroke` ve vuruşun genişliğini belirterek. `PaintSurface` İşleyicisine **özetlenen metin** sayfası gösterir nasıl yapılır:

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
    SKRect textBounds;
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 Son birkaç makalelerinde sahip SkiaSharp metin çizmek için nasıl kullanılacağını görülen artık daireler, elipsler ve yuvarlak dikdörtgen. Sonraki adım [SkiaSharp satırları ve yolları](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) içinde bir grafik yolu bağlı çizgiler çizme hakkında bilgi edineceksiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
