---
title: Döndürme dönüşümü
description: Bu makalede, etkiler ve animasyonları SkiaSharp döndürme dönüşümü ile olası inceler ve bu örnek kodu ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 514ecd16fedd7d3fda39fe20641cf0ee9ecb119e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244626"
---
# <a name="the-rotate-transform"></a>Döndürme dönüşümü

_Etkiler ve animasyonları SkiaSharp döndürme dönüşümü ile olası keşfedin_

Döndürme dönüşümü ile SkiaSharp grafik nesneleri hizalama kısıtlaması, ücretsiz, yatay ve dikey ekseni olan Kes:

![](rotate-images/rotateexample.png "Bir merkezi etrafında döndürülen metin")

Bir grafik nesnesi SkiaSharp destekler noktası (0, 0) etrafında döndürme için bir [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/) yöntemi ve [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/) yöntemi:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

İki birimleri arasında dönüştürme kolaydır bir daire 360 derece 2π radyan ile aynıdır. Uygun hangisi kullanın. Statik tüm trigonometrik işlevler [ `Math` ](https://developer.xamarin.com/api/type/System.Math/) sınıfı radyan birimleri kullanın.

Döndürme açısı artırmak için saat yönünde ' dir. (Döndürme Kartezyen koordinat sistemi üzerinde kurala göre yönünün olsa da, saat yönünde bir döndürme aşağıya giderek artan Y koordinatları ile tutarlıdır.) Açıları ve açıları 360 derece izin verilenden daha büyük negatif.

Döndürme dönüştürme formüller Çevir ve ölçek olandan daha karmaşıktır. α açısı için dönüştürme formüller şunlardır:

x' x•cos(α) – = y•sin(α)   

y' = x•sin(α) + y•cos(α)

**Temel döndürme** sayfasını gösteren `RotateDegrees` yöntemi. [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) Dosya sayfasında ortalanmış kendi temeli ile bazı metni görüntüleyen ve onu göre döndürür bir `Slider` – 360 360 aralıklı. İlgili bölümü işte `PaintSurface` işleyici:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Döndürme tuval çoğu açıları bu programda ayarlamak için sol üst köşesindeki kalmaz çünkü metin ekranı döndürülür:

[![](rotate-images/basicrotate-small.png "Üçlü sayfasının ekran görüntüsü temel döndürme")](rotate-images/basicrotate-large.png#lightbox "Üçlü sayfasının ekran görüntüsü temel Döndür")

Sıklıkla bir şey bu sürümlerini kullanan bir belirtilen pivot noktası ortalanmış döndürmek istersiniz [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/) ve [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/) yöntemleri:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**Ortalanmış döndürme** sayfasıdır tıpkı **temel döndürme** dışında genişletilmiş bir sürümü `RotateDegrees` metni konumlandırmak için kullanılan aynı noktasına döndürme merkezi ayarlamak için kullanılır:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Şimdi metni metnin temel yatay merkezi metni konumlandırmak için kullanılan nokta etrafında döndürür:

[![](rotate-images/centeredrotate-small.png "Üçlü sayfasının ekran görüntüsü ortalanmış döndürme")](rotate-images/centeredrotate-large.png#lightbox "Üçlü sayfasının ekran görüntüsü ortalanmış Döndür")

Yalnızca sürümüyle ortalanmış olarak `Scale` yöntemi, ortalanmış sürümü `RotateDegrees` çağrıdır bir kısayol:

```csharp
RotateDegrees (degrees, px, py);
```

Bu şuna eşdeğerdir:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Bazen birleştirebilirsiniz öğreneceksiniz `Translate` ile çağırır `Rotate` çağrıları. Örneğin, işte `RotateDegrees` ve `DrawText` çağrıları **ortalanmış döndürme** sayfa;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees` Çağrıdır iki eşdeğer `Translate` çağrıları ve bir harici merkezli `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText` Belirli bir konumda metin görüntülenecek çağrıdır eşdeğer bir `Translate` arayın ve ardından bu konum için `DrawText` bir noktada (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

İki ardışık `Translate` çağrıları birbirine iptal:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

Kavramsal olarak, iki dönüşümler kodda görüntülenme tersi sırada uygulanır. `DrawText` Çağrısı tuvale sol üst köşesinde metin görüntüler. `RotateDegrees` Çağrısı metnin sol üst köşesindeki göre döndürür. Ardından `Translate` arama metni Kanvasın ortasına taşır.

Genellikle çevirme ve döndürme birleştirmek için birkaç yolu vardır. **Döndürülen metin** sayfası aşağıdaki görüntü oluşturur:

[![](rotate-images/rotatedtext-small.png "Üçlü sayfasının ekran görüntüsü döndürülen metin")](rotate-images/rotatedtext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü döndürülen metin")

Burada `PaintSurface` işleyicisine [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) sınıfı:

```csharp
static readonly string text = "    ROTATE";
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 72
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float yText = yCenter - textBounds.Height / 2 - textBounds.Top;

        for (int degrees = 0; degrees < 360; degrees += 30)
        {
            canvas.Save();
            canvas.RotateDegrees(degrees, xCenter, yCenter);
            canvas.DrawText(text, xCenter, yText, textPaint);
            canvas.Restore();
        }
    }
}

```

`xCenter` Ve `yCenter` değerleri Kanvasın ortasına gösterir. `yText` Değer biraz değerinden uzaklığı. Bu sayfada gerçekten dikey ortalanacak şekilde metin yerleştirmek için gereken Y koordinatını belirtir. `for` Döngü ardından tuvale Merkezi'nde ortalanmış döndürme ayarlar. Döndürme 30 derece artışları gösterir. Metin kullanılarak çizilip `yText` değeri. Word önce boşlukları sayısı "DÖNDÜRÜN" içinde `text` değer belirlendi empirically bir on iki Kenarlı göründüğü bağlantısı 12 metin dizelerinden arasında yapma.

Bu kodu basitleştirmek için bir yoldur sonra döngü her zaman 30 derece döndürme açısı artırılacağını `DrawText` çağırın. Bu çağrı gereksinimini ortadan kaldırır `Save` ve `Restore`. Dikkat `degrees` değişkeni gövdesi içinde artık kullanılmayan `for` engelle:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Basit biçiminde kullanmak da mümkündür `RotateDegrees` çağrısıyla döngü Sunuş yapma tarafından `Translate` her şeyi Kanvasın ortasına taşımak için:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Değiştirilen `yText` hesaplama artık içerir `yCenter`. Şimdi `DrawText` çağrı merkezleri tuvale üstünde dikey metin.

Dönüşümler kavramsal kodda görüntülenme olmadığını ters yönde uygulandığından, genellikle daha fazla yerel dönüşümler tarafından izlenen olası başından itibaren daha genel dönüşümler olur. Bu genellikle çevirme ve döndürme birleştirmek için en kolay yoludur.

Örneğin, kendi ekseninde döndürme planet çok gibi kendi merkezi çevresinde döndüğü grafik bir nesne çizmek istediğinizi varsayalım. Ancak çok sun uzayda bir planet gibi ekranın merkezi çevresinde döndürüleceğini bu nesne de istersiniz.

Nesne tuvale sol üst köşesindeki konumlandırma ve ardından bu köşe etrafında döndürmek için bir animasyon kullanarak bunu yapabilirsiniz. Ardından, bir nesne orbital RADIUS gibi Yatay Çevir. Şimdi de başlangıcı etrafında bir ikinci animasyonlu döndürme uygulayın. Bu, köşe etrafında Uzayda Döndür nesnesi haline getirir. Şimdi Kanvasın ortasına çevir.

Burada `PaintSurface` bunlar içeren işleyici çağrıları ters sırada Dönüştür:

```csharp
float revolveDegrees, rotateDegrees;
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Red
    })
    {
        // Translate to center of canvas
        canvas.Translate(info.Width / 2, info.Height / 2);

        // Rotate around center of canvas
        canvas.RotateDegrees(revolveDegrees);

        // Translate horizontally
        float radius = Math.Min(info.Width, info.Height) / 3;
        canvas.Translate(radius, 0);

        // Rotate around center of object
        canvas.RotateDegrees(rotateDegrees);

        // Draw a square
        canvas.DrawRect(new SKRect(-50, -50, 50, 50), fillPaint);
    }
}
```

`revolveDegrees` Ve `rotateDegrees` alanları animasyonlu. Bu program üzerinde Xamarin.Forms göre farklı animasyon tekniği kullanır `Animation` sınıfı. (Bu sınıf açıklanan [, Bölüm 22 *Xamarin.Forms ile Mobile Apps oluşturma*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) `OnAppearing` geçersiz kılma iki oluşturur `Animation` nesneleri geri çağırma yöntemleri ve çağırır`Commit` bunlara bir animasyon süresi için:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    new Animation((value) => revolveDegrees = 360 * (float)value).
        Commit(this, "revolveAnimation", length: 10000, repeat: () => true);

    new Animation((value) =>
    {
        rotateDegrees = 360 * (float)value;
        canvasView.InvalidateSurface();
    }).Commit(this, "rotateAnimation", length: 1000, repeat: () => true);
}
```

İlk `Animation` nesne canlandırır `revolveDegrees` 0-360 derece üzerinde 10 saniye. İkincisi canlandırır `rotateDegrees` 0-360 derece her 1 ikinci ve aynı zamanda başka bir çağrıyı oluşturmak için yüzeyini geçersiz kılar `PaintSurface` işleyicisi. `OnDisappearing` Geçersiz kılma iki animasyonlarına iptal eder:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**Çirkin Analog Saat** program (Bu nedenle bir sonraki makalede daha cazip bir analog saat açıklanacaktır çünkü denir), saat, dakika ve saat işaretler çizin ve ellerini döndürmek için döndürme kullanır. Program noktada 100 (0, 0) bir RADIUS ile ortalanmış bir daire göre rastgele bir koordinat sistemini kullanarak saat çizer. ' Nı genişletin ve bu daire sayfasında merkezi çeviri ve ölçeklendirme kullanır.

`Translate` Ve `Scale` çağrıları uygulamak genel saati, bu, başlatılması çağrılacak ilk olanlardır şekilde `SKPaint` nesneler:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    using (SKPaint fillPaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.Color = SKColors.Black;
        strokePaint.StrokeCap = SKStrokeCap.Round;

        fillPaint.Style = SKPaintStyle.Fill;
        fillPaint.Color = SKColors.Gray;

        // Transform for 100-radius circle centered at origin
        canvas.Translate(info.Width / 2f, info.Height / 2f);
        canvas.Scale(Math.Min(info.Width / 200f, info.Height / 200f));
        ...
    }
}

```csharp
There are 60 marks of two different sizes that must be drawn in a circle around the clock. The `DrawCircle` call draws that circle at the point (0, –90), which relative to the center of the clock corresponds to 12:00. The `RotateDegrees` call increments the rotation angle by 6 degrees after every tick mark. The `angle` variable is used solely to determine if a large circle or a small circle is drawn:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        // Hour and minute marks
        for (int angle = 0; angle < 360; angle += 6)
        {
            canvas.DrawCircle(0, -90, angle % 30 == 0 ? 4 : 2, fillPaint);
            canvas.RotateDegrees(6);
        }
    ...
    }
}
```

Son olarak, `PaintSurface` işleyicisi geçerli saati alır ve saat, dakika ve ikinci ellerini döndürme derece hesaplar. Döndürme açısını göre böylece her el 12:00 konumda çizilmiştir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        DateTime dateTime = DateTime.Now;

        // Hour hand
        strokePaint.StrokeWidth = 20;
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawLine(0, 0, 0, -50, strokePaint);
        canvas.Restore();

        // Minute hand
        strokePaint.StrokeWidth = 10;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawLine(0, 0, 0, -70, strokePaint);
        canvas.Restore();

        // Second hand
        strokePaint.StrokeWidth = 2;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Second);
        canvas.DrawLine(0, 10, 0, -80, strokePaint);
        canvas.Restore();
    }
}
```

Ellerini yerine kaba olmasına rağmen kesinlikle işlevsel bir saattir:

[![](rotate-images/uglyanalogclock-small.png "Üçlü çirkin Analog Saat metin sayfasının ekran görüntüsü")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
