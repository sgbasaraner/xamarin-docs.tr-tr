---
title: Eğme dönüştürmesi
description: Eğme dönüştürmesi Eğimli grafik nesneleri SkiaSharp içinde nasıl oluşturabileceğinizi bakın
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: c9f5f9f20296b1c2443a8addeebd4d12ccaa1ab4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="the-skew-transform"></a>Eğme dönüştürmesi

_Eğme dönüştürmesi Eğimli grafik nesneleri SkiaSharp içinde nasıl oluşturabileceğinizi bakın_

SkiaSharp içinde bu görüntüdeki gölge gibi grafik nesneleri eğme dönüştürmesi Eğimli:

![](skew-images/skewexample.png "Gölge metin eğme programdan eğriltme örneği")

Eğme dikdörtgenler parallelograms dönüştürür, ancak çarpık elips hala elips olur.

Xamarin.Forms çevirisi, ölçeklendirme ve döndürme özelliklerini tanımlamasına rağmen eğme için Xamarin.Forms içinde karşılık gelen bir özellik yok.

[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) Yöntemi `SKCanvas` yatay eğme ve dikey için iki bağımsız değişken eğme kabul eder:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

İkinci bir [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) yöntemi bu bağımsız değişkenlerden tek bir araya getiren `SKPoint` değeri:

```csharp
public void Skew (SKPoint skew)
```

Ancak, bu iki yöntemden birini yalıtım modunda kullanıyor olmanız, olası değil.

**Eğme denemeler** eğme ile denemeler sayfa sağlar –10 ile 10 arasında bu aralık değerleri. Bir metin dizesi iki elde eğme değerlerle sayfanın sol üst köşesinde konumlandırılır `Slider` öğeleri. Burada `PaintSurface` işleyicisinde [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) sınıfı:

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

Değerleri `xSkew` bağımsız değişkeni kaydırma metni sağa pozitif değerler veya negatif değerler için sol alt. Değerleri `ySkew` aşağı metnin sağına pozitif değerler veya negatif değerler için kaydır:

[![](skew-images/skewexperiment-small.png "Üçlü sayfasının ekran görüntüsü eğme deneme")](skew-images/skewexperiment-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğme deneme")

Varsa `xSkew` , negatif olduğundan `ySkew`, sonuç döndürme olmakla birlikte, aynı zamanda biraz görüntü gösteren Windows ölçeklendirilmiş.

Dönüştürme formüller aşağıdaki gibidir:

x' = x + xSkew · y

y' = ySkew · x + y

Örneğin, bir pozitif `xSkew` değeri, dönüştürülmüş `x'` değeri artırır olarak `y` artırır. Ne Eğim neden olur.

Üçgen 200 piksel genişliğinde ve 100 piksel yüksek bir noktada (0, 0), sol üst köşesindeki ile konumlandırılmış ve ile işlenen bir `xSkew` 1.5, aşağıdaki paralel kenarı sonuçları değeri:

![](skew-images/skeweffect.png "Dikdörtgene eğme dönüştürmesi etkisi")

Kenar koordinatlara sahip `y` dolayısıyla 100 değerini gölgeye 150 piksel sağa.

Sıfır olmayan değerler için `xSkew` veya `ySkew`, sadece (0, 0) noktasına aynı kalır. Bu noktadan eğriltme veya merkezi kabul edilebilir. Başka bir konuda olmasını eğriltme veya Merkezi (genellikle söz konusu olduğu) gerekiyorsa, var olan hiçbir `Skew` sağlayan yöntemi. Açıkça birleştirmek gerekir `Translate` ile çağırır `Skew` çağırın. Konumundaki eğriltme orta `px` ve `py`, aşağıdaki çağrıları olun:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Bileşik dönüştürme formüller şunlardır:

x' = x + xSkew · (y-py)

y' = ySkew · (x – piksel) + y

Varsa `ySkew` sıfırsa ve yalnızca, sıfır olmayan bir değer belirlediniz `xSkew`, ardından `px` değeri kullanılmaz. İlgisiz, bir değerdir ve benzer şekilde `ySkew` ve `py`.

Bu diyagramda α açı gibi Eğim açısı olarak eğme belirterek daha rahat düşündüğünüz:

![](skew-images/skewangleeffect.png "Belirtilen bir eğme açısı olan bir dikdörtgen eğme dönüştürmesi etkisi")

100 piksel dikey 150 piksel kaydırma oranını bu örnekte, açının tanjantını 56.3 derece döndürülür.

XAML dosyası **eğme açı deneme** sayfa benzer **eğme açı** dışında sayfasında `Slider` öğeleri 90 derece – 90 ' aralığı. [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Arka plan kodu dosya sayfasında metni merkezleri ve kullandığı `Translate` sayfa merkezine eğriltme, bir merkezi ayarlamak için. Kısa bir `SkewDegrees` yöntemi kodu altındaki değerler eğme açıları dönüştürür:

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

Bir açının olumlu veya olumsuz 90 derece yaklaştığında tanjantını sonsuz yaklaşıyor, ancak yaklaşık 80 derece veya bunu kadar açıları kullanılabilir:

[![](skew-images/skewangleexperiment-small.png "Üçlü sayfasının ekran görüntüsü eğme açı deneme")](skew-images/skewangleexperiment-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğme açı deneme")

Küçük negatif yatay eğme olarak eğik veya italik metin taklit **eğik metin** sayfası gösterilmektedir. [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Sınıfı gösterir nasıl yapılır:

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

`TextAlign` Özelliği `SKPaint` ayarlanır `Center`. Dönüşüm olmadan `DrawText` çağrısı ile koordinatları (0, 0) ve yatay ortada temelinin metinle sol üst köşede getirin. `SkewDegrees` Metnin yatay olarak 20 derece taban göre Eğer. `Translate` Çağrısı ve yatay ortada metnin temelinin Kanvasın ortasına taşır:

[![](skew-images/obliquetext-small.png "Üçlü sayfasının ekran görüntüsü eğik metin")](skew-images/obliquetext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğik metin")

**Eğme gölge metin** sayfa 45 derecelik eğme ve dikey ölçek bileşimi çıktığınızda metin eğimli bir metin gölgesinin yapmak için nasıl kullanılacağını gösterir. İlgili bölümü işte `PaintSurface` işleyici:

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

[![](skew-images/skewshadowtext1-small.png "Üçlü sayfasının ekran görüntüsü eğme gölge metin")](skew-images/skewshadowtext1-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğme gölge metin")

Dikey koordinat geçirilen `DrawText` yöntemi metnin taban çizgisine göre konumunu belirtir. Eğriltme center için kullanılan aynı dikey koordinat olmasıdır. Metin dizesini harfin alt içeriyorsa bu yöntem çalışmaz. Örneğin, "" Gölge"ve burada geliştirilmesindeki" word DEVICEHIGH sonucu kullanıcının:

[![](skew-images/skewshadowtext2-small.png "Üçlü sayfasının ekran görüntüsü eğme gölge metin alternatif bir sözcükle harfin alt ile")](skew-images/skewshadowtext2-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğme gölge metin alternatif bir sözcükle harfin alt ile")

Temel metin ve gölge hala hizalanır ancak etkisi yalnızca yanlış görünüyor. Sorunu gidermek için Metin sınırları almanız gerekir:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate` Çağrıları uzanır yükseklik olarak ayarlanması gerekir:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Şimdi gölge bu harfin alt alt kısmından genişletir:

[![](skew-images/skewshadowtext3-small.png "Üçlü sayfasının ekran görüntüsü eğme gölge metin harfin alt ayarlamalar ile")](skew-images/skewshadowtext3-large.png#lightbox "Üçlü sayfasının ekran görüntüsü eğme gölge metin harfin alt ayarlamalar ile")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
