---
title: Döndürme dönüşümü
description: Bu makalede efektler ve animasyon SkiaSharp döndürme dönüşümü ile olası keşfediyor ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: 3726a93ccf43fd9a2afdc2c46bb63e0f6ef7ad51
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615255"
---
# <a name="the-rotate-transform"></a>Döndürme dönüşümü

_Efektler ve animasyon SkiaSharp döndürme dönüşümü ile olası keşfedin_

Döndürme dönüşümü ile yatay ve dikey ekseni olan SkiaSharp grafik nesneleri hizalama kısıtlamasını ücretsiz sonu:

![](rotate-images/rotateexample.png "Bir merkezi etrafında döndürülen metin")

Grafik nesnesini (0, 0) SkiaSharp destekler nokta etrafında döndürmek için bir [ `RotateDegrees` ](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single)) yöntemi ve bir [ `RotateRadians` ](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single)) yöntemi:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

İki birim arasında dönüştürmek kolay, bu nedenle bir daire 360 derece twoπ radyan aynıdır. Hangisi uygun kullanın. . NET'te tüm trigonometrik işlevler [ `Math` ](xref:System.Math) radyan ölçü sınıfını kullanın.

Döndürme açısı artırmaya yönelik saat yönünde. (Kartezyen koordinat sisteminde döndürme kurala göre yönünün olsa da, saat yönünde bir döndürme aşağısına olduğu gibi SkiaSharp gittikçe artan Y koordinatları ile uyumludur.) Açıları ve açıları 360 derece izin verilenden daha büyük negatif.

Döndürme dönüşümü formülleri çeviri ve ölçeklendirme için olandan daha karmaşıktır. α açısı için dönüştürme formülleri şunlardır:

x' x•cos(α) – = y•sin(α)   

y' x•sin(α) + y•cos(α) =

**Temel döndürme** sayfasını gösterir `RotateDegrees` yöntemi. [ **BasicRotate.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) dosya ortalanıp kendi temeli ile bazı metni görüntüler ve göre döndürür bir `Slider` 360 – 360 aralıklı. İlgili bölümü işte `PaintSurface` işleyicisi:

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

Bu programda ayarlanmış çoğu açıları için tuvalin sol üst köşesinin etrafında döndürme ortalanır çünkü metin ekranı döndürülür:

[![](rotate-images/basicrotate-small.png "Üçlü sayfasının ekran görüntüsü temel döndürme")](rotate-images/basicrotate-large.png#lightbox "Üçlü sayfasının ekran görüntüsü temel Döndür")

Çok sık bu sürümlerini kullanan bir belirtilen pivot noktası ortalanmış bir şey döndürmek isteyeceksiniz [ `RotateDegrees` ](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single,System.Single,System.Single)) ve [ `RotateRadians` ](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single,System.Single,System.Single)) yöntemleri:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**Ortalanmış döndürme** sayfasıdır gibi **temel döndürme** dışında genişletilmiş bir sürümü `RotateDegrees` dönme merkezi metin konumlandırmak için kullanılan aynı noktaya ayarlamak için kullanılır:

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

Şimdi metin metnin temel yatay merkezi metin konumlandırmak için kullanılan noktası döndürür:

[![](rotate-images/centeredrotate-small.png "Üçlü sayfasının ekran görüntüsü ortalanmış döndürme")](rotate-images/centeredrotate-large.png#lightbox "Üçlü sayfasının ekran görüntüsü ortalanmış Döndür")

Ortalanmış sürümü ile `Scale` yöntemi, ortalanmış sürümü `RotateDegrees` çağrıdır bir kısayol. Yöntem aşağıda verilmiştir:

```csharp
RotateDegrees (degrees, px, py);
```

Bu çağrı aşağıdakine eşdeğerdir:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Bazen birleştirebilirsiniz keşfedeceksiniz `Translate` ile çağırır `Rotate` çağırır. Örneğin, işte `RotateDegrees` ve `DrawText` çağrıları **ortalanmış döndürme** sayfasında;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees` Çağrıdır iki eşdeğer `Translate` çağırır ve bir olmayan merkezli `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText` Belirli bir konumda metin görüntülenecek çağrısı eşdeğer bir `Translate` çağrı ardından o konuma `DrawText` bir noktada (0, 0):

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

Kavramsal olarak, iki dönüştürmeler, kodda görüntülenme tersi sırada uygulanır. `DrawText` Çağrı tuvalin sol üst köşesinde bulunan metni görüntüler. `RotateDegrees` Çağrı metnin sol üst köşesine göre döndürür. Ardından `Translate` çağrı tuval merkezine metin taşır.

Genellikle çevirme ve döndürme birleştirmek için birkaç yol vardır. **Döndürülmüş metin** sayfası aşağıdaki görüntü oluşturur:

[![](rotate-images/rotatedtext-small.png "Üçlü sayfasının ekran görüntüsü döndürülmüş metin")](rotate-images/rotatedtext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü döndürülen metin")

İşte `PaintSurface` işleyicisi [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) sınıfı:

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

`xCenter` Ve `yCenter` değerler tuval merkezi gösterir. `yText` Değerdir, biraz uzaklığı. Bu sayfada gerçekten dikey ortalanacak şekilde metin yerleştirmek için gereken Y koordinatı değerdir. `for` Döngü, ardından tuval Merkezi'nde dayalı bir döndürme ayarlar. Döndürme 30 derece artışlarla ' dir. Metin kullanılarak çizilir `yText` değeri. Word önce boşluk sayısını "DÖNDÜRÜN" içinde `text` değer belirlendi türü bir on iki kenarlı olabilir görünmesine 12 metin dizelerinden arasında bağlantı kurmak için.

Bu kodu basitleştirmek için bir yol olan 30 derece döndürme açısını sonra döngü her zaman artırmak için `DrawText` çağırın. Bu çağrılar için gereksinimini ortadan kaldırır `Save` ve `Restore`. Dikkat `degrees` değişkeni gövdesi içinde artık kullanılmamaktadır `for` engelle:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Basit biçimini kullanmak da mümkündür `RotateDegrees` çağrısıyla döngü koyarak tarafından `Translate` her şeyi tuval merkezine taşımak için:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Değiştirilmiş `yText` hesaplama artık içerir `yCenter`. Artık `DrawText` çağrı merkezi metin tuval üstünde dikey.

Dönüşümler kavramsal olarak kodu nasıl görüneceklerini ters yönde uygulandığından, genellikle daha fazla yerel dönüştürmeler tarafından izlenen olası başlangıç daha genel dönüşümler olur. Bu genellikle çevirme ve döndürme birleştirmek için en kolay yoludur.

Örneğin, çok bir dünya kendi ekseninde döndürme gibi merkezi çevresinde döndürür bir grafik nesnesi çizmek istediğinizi varsayalım. Ancak bu nesne, ekranın sun uzayda dünya çok gibi merkezi etrafındaki döndürüleceğini da istiyoruz.

Nesne tuvalin sol üst köşesinde bulunan konumlandırma ve ardından bu köşe etrafında döndürmek için bir animasyon kullanarak bunu yapabilirsiniz. Ardından, bir nesne bir orbital RADIUS gibi yatay olarak çevir. Şimdi de kaynağı etrafında ikinci bir animasyonlu döndürme başvurun. Bu, köşe etrafında çalışmalarınızı nesnesi getirir. Artık tuval merkezine çevir.

İşte `PaintSurface` bunlar içeren işleyici çağrıları ters sırada dönüştürün:

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

`revolveDegrees` Ve `rotateDegrees` alanları bir animasyon görünür. Bu program Xamarin.Forms hakkında temel bir animasyon farklı teknik kullanır [ `Animation` ](xref:Xamarin.Forms.Animation) sınıfı. (Bu sınıf açıklanan [Bölüm 22, *Xamarin.Forms ile Mobile Apps oluşturma*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) `OnAppearing` geçersiz kılma oluşturur iki `Animation` nesneleri geri çağırma yöntemleri ile ve ardından çağırır`Commit` bunlara bir animasyon süresi:

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

İlk `Animation` nesne canlandırır `revolveDegrees` 360 derece 10 saniyenin üzerindeki 0 dereceye öğesinden. İkincisi canlandırır `rotateDegrees` 360 derece 0 dereceye gelen her 1 saniye ve ayrıca yüzeyine başka bir çağrıyı oluşturmak için geçersiz kılar `PaintSurface` işleyici. `OnDisappearing` Geçersiz kılma iki animasyonlarına iptal eder:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**Çirkin Analog Saat** (Bu nedenle daha cazip bir analog saat, bir sonraki makalede açıklanan çünkü denir) programı döndürme saatin saat ve dakika işaretler çizin ve uygulamalı döndürmek için kullanır. Program saatin noktada 100 (0, 0) ile bir RADIUS Orta dairenin göre rastgele bir koordinat sistemini kullanarak çizer. Genişletin ve o daire sayfasında merkezi çeviri ve ölçeklendirme kullanır.

`Translate` Ve `Scale` çağrıları genel olarak uygulanır saati, bu, başlatma çağrılacak ilk olanlardır şekilde `SKPaint` nesneler:

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
```

Bir daire sistemlerimizdeki çizilmesi iki farklı boyutlardaki 60 işaretleri vardır. `DrawCircle` Çağrısı noktasında, 12:00 için karşılık gelen saatin merkezine göre (0, – 90), daire çizer. `RotateDegrees` Çağrı, her değer çizgisi sonra 6 derece döndürme açısını artırır. `angle` Değişkeni yalnızca büyük bir daire veya küçük daire çizilip çizilmediğini belirlemek için kullanılır:

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

Son olarak, `PaintSurface` işleyicisi geçerli saati alır ve döndürme derece, saat, dakika ve ikinci uygulamalı hesaplar. Döndürme açısı göre böylece her el 12:00 konumu çizilmiştir:

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

Bire çok kaba olsa saat tam işlevsel:

[![](rotate-images/uglyanalogclock-small.png "Üçlü çirkin Analog saati metin sayfasının ekran görüntüsü")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")

Daha cazip bir saat için bkz [ **SkiaSharp SVG yol verileri**](../curves/path-data.md).

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
