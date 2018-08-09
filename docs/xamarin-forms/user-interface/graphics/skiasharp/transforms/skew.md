---
title: Eğme dönüşümü
description: Bu makalede, eğme dönüşümü içinde SkiaSharp Eğimli grafik nesneleri nasıl oluşturacağınızı açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: 951fc02dfff1721c1391c5d0c8a21452a156cfdb
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615359"
---
# <a name="the-skew-transform"></a>Eğme dönüşümü

_Eğme dönüşümü içinde SkiaSharp Eğimli grafik nesneleri nasıl oluşturabileceğiniz konusuna bakın._

SkiaSharp gölge bu görüntüdeki gibi grafik nesneleri eğme dönüşümü Eğimli:

![](skew-images/skewexample.png "Eğriltme gölge metin programından eğriltme örneği")

Eğriltme dikdörtgenler parallelograms dönüştürür ancak dengesiz bir elips elips olmaya devam eder.

Xamarin.Forms, çeviri, ölçeklendirme ve döndürme için özellikleri tanımlar. ancak, eğme için Xamarin.Forms içinde karşılık gelen bir özellik yok.

[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) Yöntemi `SKCanvas` eğriltmek için eğriltme yatay ve dikey iki bağımsız değişkeni kabul eder:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

İkinci [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) yöntemi tek bir bağımsız değişkenleri birleştiren `SKPoint` değeri:

```csharp
public void Skew (SKPoint skew)
```

Ancak, bu iki yöntemden birini yalıtım modunda kullanacaksınız, düşüktür.

**Eğme deneme** değerleri –10 ve 10 arasındaki eğriltme ile denemeler sayfa sağlar. Bir metin dizesi sayfanın sol üst köşesinde bulunan iki elde eğriltme değerlerle konumlandırılmış `Slider` öğeleri. İşte `PaintSurface` işleyicisinde [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) sınıfı:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        canvas.Skew((float)xSkewSlider.Value, (float)ySkewSlider.Value);
        canvas.DrawText(text, 0, -textBounds.Top, textPaint);
    }
}
```

Değerleri `xSkew` bağımsız değişken pozitif değerler için hemen metin veya negatif değerler için sol alt kısmındaki kaydır. Değerleri `ySkew` metnin sağına pozitif değerler için veya negatif değerler için shift:

[![](skew-images/skewexperiment-small.png "Üçlü sayfasının ekran görüntüsü eğme deneme")](skew-images/skewexperiment-large.png#lightbox "Üçlü sayfasının ekran görüntüsü deneme eğme")

Varsa `xSkew` 'in negatifi olduğu `ySkew`, döndürme, sonuç, ancak biraz UWP görünen gösterir ayrıca Ölçeklendirildi.

Dönüştürme formülleri aşağıdaki gibidir:

x' = x + xSkew · y

y' ySkew · = x + y

Örneğin, bir pozitif `xSkew` değeri, dönüştürülen `x'` değeri artırır olarak `y` artırır. Eğim ne neden olur.

Bir üçgen 200 piksel genişliğinde ve 100 piksel yüksekliğinde bir noktada (0, 0), sol üst köşesinin ile konumlandırılır ve ile işlenen bir `xSkew` 1.5, aşağıdaki eğdiğinizde Paralel Kenar sonuçları değeri:

![](skew-images/skeweffect.png "Bir dikdörtgen eğme dönüşümü etkisi")

Alt kenarı koordinatlarını sahip `y` değerleri, bu nedenle 100 150 piksel sağa kaydırılacak.

Sıfır olmayan değerler için `xSkew` veya `ySkew`yalnızca noktası (0, 0) aynı kalır. O noktadan eğriltme merkezi kabul edilebilir. Başka bir olacak şekilde eğriltme Merkezi (genellikle söz konusu olduğu) gerekiyorsa, var olan hiçbir `Skew` sağlayan yöntemi. Açıkça birleştirmek ihtiyacınız olacak `Translate` ile çağırır `Skew` çağırın. Eğriltme sırasında orta `px` ve `py`, aşağıdaki çağrıları yapın:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Bileşik dönüştürme formülleri şunlardır:

x' = x + xSkew · (y – Kopyala)

y' ySkew · = (x-px) + y

Varsa `ySkew` sıfır ve sıfır dışında değeri yalnızca belirlediniz `xSkew`, ardından `px` değeri kullanılmaz. Değer ilgisizdir ve benzer şekilde `ySkew` ve `py`.

Bu diyagramda α açı gibi bir eğme açısı olarak eğriltme belirterek daha iyi ilerlediğini düşünebilirsiniz:

![](skew-images/skewangleeffect.png "Belirtilen bir dikdörtgen bir eğme açısı eğme dönüşümü etkisi")

150 piksel 100 piksel dikey kaydırma Bu örnekte, bir açının tanjantını 56.3 derece oranıdır.

XAML dosyası **eğme açısı deneme** sayfasına benzer **eğme açısı** sayfasında hariç `Slider` öğeleri – 90 ' 90 derece aralığında. [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Arka plan kod dosyası merkezi metin sayfasında ve kullandığı `Translate` sayfanın merkezine eğriltme, bir merkezi ayarlamak için. Kısa `SkewDegrees` yöntemi kodu alt kısmındaki değerleri eğriltmek için açıları dönüştürür:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;

        canvas.Translate(xCenter, yCenter);
        SkewDegrees(canvas, xSkewSlider.Value, ySkewSlider.Value);
        canvas.Translate(-xCenter, -yCenter);
        canvas.DrawText(text, xText, yText, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

Açı pozitif veya negatif 90 derece yaklaştığında, sonsuzluk tanjantını yaklaştığında, ancak yaklaşık 80 derece veya bunu açıları kullanılabilir:

[![](skew-images/skewangleexperiment-small.png "Üçlü sayfasının ekran görüntüsü eğme açısı deneme")](skew-images/skewangleexperiment-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğme açısı deneme")

Küçük negatif yatay eğme olarak eğik veya italik metin taklit **eğik metin** sayfasını gösterir. [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Sınıfı nasıl yapıldığını gösterir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Maroon,
        TextAlign = SKTextAlign.Center,
        TextSize = info.Width / 8       // empirically determined
    })
    {
        canvas.Translate(info.Width / 2, info.Height / 2);
        SkewDegrees(canvas, -20, 0);
        canvas.DrawText(Title, 0, 0, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

`TextAlign` Özelliği `SKPaint` ayarlanır `Center`. Dönüşüm olmadan `DrawText` koordinatları ile çağırın (0, 0) yatay merkezine göre temel metinle sol üst köşedeki konumlandırma. `SkewDegrees` Metnin yatay olarak 20 derece taban çizgisine göre eğriltir. `Translate` Çağrı tuval merkezine yatay merkezi metin temelinin taşır:

[![](skew-images/obliquetext-small.png "Üçlü sayfasının ekran görüntüsü eğik metin")](skew-images/obliquetext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğik metin")

**Eğme gölge metin** sayfa 45 derece eğriltme ve dikey ölçek bileşimi uzağa metin eğimli bir metin gölgesinin yapma işlemini gösterir. İlgili bölümü işte `PaintSurface` işleyicisi:

```csharp
using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = info.Width / 6;   // empirically determined

    // Common to shadow and text
    string text = "Shadow";
    float xText = 20;
    float yText = info.Height / 2;

    // Shadow
    textPaint.Color = SKColors.LightGray;
    canvas.Save();
    canvas.Translate(xText, yText);
    canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
    canvas.Scale(1, 3);
    canvas.Translate(-xText, -yText);
    canvas.DrawText(text, xText, yText, textPaint);
    canvas.Restore();

    // Text
    textPaint.Color = SKColors.Blue;
    canvas.DrawText(text, xText, yText, textPaint);
}
```

Gölge görüntülenen ilk ve metin şöyledir:

[![](skew-images/skewshadowtext1-small.png "Üçlü sayfasının ekran görüntüsü eğme gölge metin")](skew-images/skewshadowtext1-large.png#lightbox "Üçlü sayfasının ekran görüntüsü gölge metin eğme")

Dikey koordinat geçirilen `DrawText` yöntemi taban çizgisine göre metnin konumunu gösterir. Eğriltme center için kullanılan aynı dikey koordinat olmasıdır. Metin dizesi çıkıntılarını içeriyorsa bu yöntem çalışmaz. Örneğin, "" Gölge"ve burada geliştirilmesindeki" sözcüğü DEVICEHIGH sonucu kullanıcının:

[![](skew-images/skewshadowtext2-small.png "Üçlü sayfasının ekran görüntüsü eğme gölge metin çıkıntılarını alternatif bir sözcükle")](skew-images/skewshadowtext2-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğme gölge metin çıkıntılarını alternatif bir sözcükle")

Metin ve gölge temel hala hizalanır ancak etkisi yalnızca yanlış görünüyor. Sorunu gidermek için Metin sınırları almanız gerekir:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate` Çağrıları çıkıntılarını yüksekliğini tarafından ayarlanması gerekir:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Şimdi gölge bu çıkıntılarını aşağıdan genişletir:

[![](skew-images/skewshadowtext3-small.png "Üçlü sayfasının ekran görüntüsü eğme gölge metin çıkıntılarını ayarlamalarını")](skew-images/skewshadowtext3-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğme gölge metin düzeltmeleri çıkıntılarını için")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
