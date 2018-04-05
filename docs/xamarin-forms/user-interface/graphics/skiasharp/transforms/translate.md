---
title: Çevir Dönüştür
description: SkiaSharp grafik kaydırılacak Çevir Dönüştür kullanmayı öğrenin
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 98bf81df3eed951893c6bb717d933cfb61e029d3
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="the-translate-transform"></a>Çevir Dönüştür

_SkiaSharp grafik kaydırılacak Çevir Dönüştür kullanmayı öğrenin_

En basit SkiaSharp dönüştürme türüdür *çevir* veya *çeviri* dönüştürün. Bu dönüştürme yatay ve dikey yönde grafik nesneleri geçirir. Çizim işlevinde kullanmakta olduğunuz koordinatları değiştirerek genellikle aynı etkiyi gerçekleştirebilirsiniz bir anlamda çeviri en gereksiz dönüştürme olmasıdır. Ancak, tüm yol kaydırılacak Çevir dönüşümü uygulanarak çok daha kolay olacak şekilde bir yolu işlenirken, tüm koordinatların yolunda kapsüllenir.

Çeviri basit metin efektleri ve animasyon için de yararlıdır:

![](translate-images/translateexample.png "Metin gölgesinin engraving ve çevirisi kabartma")

[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) Yönteminde `SKCanvas` sonradan çizilmiş grafik yatay ve dikey olarak gölgeye nesnelere neden iki parametreye sahiptir:

```csharp
public void Translate (Single dx, Single dy)
```

Bu bağımsız değişken negatif olabilir. İkinci bir [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/) yöntemi tek bir iki çeviri değerleri birleştirir `SKPoint` değeri:

```csharp
public void Translate (SKPoint point)
```

**Birikmiş çevir** sayfasında [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) örnek program gösterir, birden çok çağıran `Translate` yöntemi toplu. [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Çapraz uzatmak için her biri önceki dikdörtgenin yetecek kadar uzaklığı, sınıfı aynı dikdörtgen 20 sürümlerini görüntüler. Burada `PaintSurface` olay işleyicisi:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Color = SKColors.Black;
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = 3;

        int rectangleCount = 20;
        SKRect rect = new SKRect(0, 0, 250, 250);
        float xTranslate = (info.Width - rect.Width) / (rectangleCount - 1);
        float yTranslate = (info.Height - rect.Height) / (rectangleCount - 1);

        for (int i = 0; i < rectangleCount; i++)
        {
            canvas.DrawRect(rect, strokePaint);
            canvas.Translate(xTranslate, yTranslate);
        }
    }
}
```

Art arda dikdörtgenler page down trickle:

[![](translate-images/accumulatedtranslate-small.png "Üçlü sayfasının ekran görüntüsü birikmiş çevir")](translate-images/accumulatedtranslate-large.png#lightbox "Üçlü sayfasının ekran görüntüsü birikmiş Çevir")

Birikmiş çeviri Etkenler varsa `dx` ve `dy`, çizim işlevinde belirtin noktası (`x`, `y`), grafik nesnesi bir noktada işlenmeden sonra (`x'`, `y'`), burada:

x' = x + dx

y' = y + dy

Bunlar olarak bilinir *dönüştürme formüller* çeviri için. Varsayılan değerleri `dx` ve `dy` için yeni bir `SKCanvas` 0.

Çeviri dönüştürme olarak gölge etkiler ve benzer teknikleri kullanmak için ortak olan **Çevir metin efektleri** sayfası gösterilmektedir. İlgili bölümü işte `PaintSurface` işleyicisinde [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) sınıfı:

```csharp
float textSize = 150;

using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = textSize;
    textPaint.FakeBoldText = true;

    float x = 10;
    float y = textSize;

    // Shadow
    canvas.Translate(10, 10);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("SHADOW", x, y, textPaint);
    canvas.Translate(-10, -10);
    textPaint.Color = SKColors.Pink;
    canvas.DrawText("SHADOW", x, y, textPaint);

    y += 2 * textSize;

    // Engrave
    canvas.Translate(-5, -5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("ENGRAVE", x, y, textPaint);
    canvas.ResetMatrix();
    textPaint.Color = SKColors.White;
    canvas.DrawText("ENGRAVE", x, y, textPaint);

    y += 2 * textSize;

    // Emboss
    canvas.Save();
    canvas.Translate(5, 5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("EMBOSS", x, y, textPaint);
    canvas.Restore();
    textPaint.Color = SKColors.White;
    canvas.DrawText("EMBOSS", x, y, textPaint);
}
```

Her üç örneklerin `Translate` tarafından verilen konumdan uzaklığı için metin görüntüleme için adlı `x` ve `y` değişkenleri. Ardından metin yeniden çeviri etkisiz başka bir renkle görüntülenir:

[![](translate-images/translatetexteffects-small.png "Üçlü sayfasının ekran görüntüsü Çevir metin efektleri")](translate-images/translatetexteffects-large.png#lightbox "Üçlü sayfasının ekran görüntüsü metin efektleri Çevir")

Her üç örneklerin negating farklı bir şekilde gösterilir `Translate` çağırın:

Yalnızca ilk örneği çağırır `Translate` yeniden ancak negatif değerler. Çünkü `Translate` çağrıları toplu, çağrısı bu sırası, toplam çeviri yalnızca sıfır varsayılan değerlere geri yükler.

İkinci örnek çağrıları [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/). Bu, varsayılan durumlarına döndürmek tüm dönüşümler neden olur.

Üçüncü örnek durumunu kaydeder, `SKCanvas` çağrısıyla nesne [ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/) ve çağrısıyla durumuna geri yükler [ `Restore` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/). Bu, bir dizi işlemleri çizim için dönüşümler işlemek için en çok yönlü yoludur. Bunlar `Save` ve `Restore` çağıran işlevi bir yığın gibi: çağırabilirsiniz `Save` birden çok zaman ve ardından arama `Restore` içinde ters önceki durumlarına geri dönmek için dizisi. `Save` Yöntemi bir tamsayı döndürür ve bu tamsayıya geçirebilirsiniz [ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/) etkili bir şekilde çağrılacak `Restore` birden çok kez. [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/) Özelliği şu anda yığında kaydedilmiş durumların sayısını döndürür.

Ancak bir çağrısından taşıyan dönüşümler hakkında endişelenmenize gerek yoktur `PaintSurface` sonraki işleyici. Her yeni çağrı `PaintSurface` baştan sunar `SKCanvas` varsayılan dönüşümler nesnesiyle.

Başka bir yaygın kullanımı `Translate` dönüştürme olduğu için görsel bir nesne oluşturma başlangıçta çizim için uygun koordinatları kullanılarak oluşturuldu. Örneğin, merkezi bir noktada (0, 0) ile bir analog saat koordinatlarını belirtmek isteyebilirsiniz. Dönüşümler görüntülemek için istediğiniz yere sonra kullanabilirsiniz. Bu, gösterilmiştir [**Hendecagram dizi**] sayfası. [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) Sınıfı başlar oluşturarak bir `SKPath` 11 işaret yıldız nesnesi. `HendecagramPath` Nesne tanımlanmış olarak ortak statik ve salt okunur böylece diğer sunum programlarından erişilebilir. Statik bir oluşturucu oluşturulur:

```csharp
public class HendecagramArrayPage : ContentPage
{
    ...
    public static readonly SKPath HendecagramPath;

    static HendecagramArrayPage()
    {
        // Create 11-pointed star
        HendecagramPath = new SKPath();
        for (int i = 0; i < 11; i++)
        {
            double angle = 5 * i * 2 * Math.PI / 11;
            SKPoint pt = new SKPoint(100 * (float)Math.Sin(angle),
                                    -100 * (float)Math.Cos(angle));
            if (i == 0)
            {
                HendecagramPath.MoveTo(pt);
            }
            else
            {
                HendecagramPath.LineTo(pt);
            }
        }
        HendecagramPath.Close();
    }
}
```

Yıldızın Merkezi (0, 0) noktası ise, tüm yıldızın o noktadan çevreleyen bir daireye noktalarıdır. Her noktası 360 derece 5/11ths tarafından artırır bir açının sinüsünü ve kosinüsünü değerleri birleşimidir. (2 açıyla artırarak 11 işaret yıldız oluşturmak mümkündür/11, 3/11ths ya da 4/dairenin 11.) Bu daire RADIUS 100 olarak ayarlanır.

Bu yol dönüşümleri işlenir, sol üst köşesinde merkezi yerleştirileceğini `SKCanvas`ve bunu bir Çeyreğin görünür olacaktır. `PaintSurface` İşleyicisine `HendecagramPage` yerine kullanır `Translate` tuvale yıldız birden çok kopyası ile döşeme için her birini rastgele renkli:

```csharp
public class HendecagramArrayPage : ContentPage
{
    Random random = new Random();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            for (int x = 100; x < info.Width + 100; x += 200)
                for (int y = 100; y < info.Height + 100; y += 200)
                {
                    // Set random color
                    byte[] bytes = new byte[3];
                    random.NextBytes(bytes);
                    paint.Color = new SKColor(bytes[0], bytes[1], bytes[2]);

                    // Display the hendecagram
                    canvas.Save();
                    canvas.Translate(x, y);
                    canvas.DrawPath(HendecagramPath, paint);
                    canvas.Restore();
                }
        }
    }
}

```

Sonuç şöyledir:

[![](translate-images/hendecagramarray-small.png "Üçlü sayfasının ekran görüntüsü Hendecagram dizi")](translate-images/hendecagramarray-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Hendecagram dizisi")

Animasyon genellikle dönüşümler içerir. **Hendecagram animasyon** sayfa 11 işaret yıldız bir daire içinde taşır. [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) Sınıfı bazı alanlar ile başlar ve, geçersiz kılmalar `OnAppearing` ve `OnDisappearing` yöntemleri Xamarin.Forms süreölçer durdurmak ve başlatmak için:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    const double cycleTime = 5000;      // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float angle;

    public HendecagramAnimationPage()
    {
        Title = "Hedecagram Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            angle = (float)(360 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`angle` Alan animasyonlu 0-360 derece 5 saniyede. `PaintSurface` İşleyici kullanır `angle` özelliği iki yolla: renginin belirtmek için `SKColor.FromHsl` yöntemi ve bağımsız değişken olarak `Math.Sin` ve `Math.Cos` yöntemleri yıldız konumunu yönetmek için:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.Translate(info.Width / 2, info.Height / 2);
        float radius = (float)Math.Min(info.Width, info.Height) / 2 - 100;

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColor.FromHsl(angle, 100, 50);

            float x = radius * (float)Math.Sin(Math.PI * angle / 180);
            float y = -radius * (float)Math.Cos(Math.PI * angle / 180);
            canvas.Translate(x, y);
            canvas.DrawPath(HendecagramPage.HendecagramPath, paint);
        }
    }
}
```

`PaintSurface` İşleyicisi çağrılarını `Translate` ilk Kanvasın ortasına çevirmek için iki kez, yöntemi ve ardından kalmaz dairenin çevresi çevir (0, 0). RADIUS dairenin hala sayfasının sınırlar içinde yıldız tutarken mümkün olduğunca büyük olacak şekilde ayarlanmıştır:

[![](translate-images/hendecagramanimation-small.png "Üçlü sayfasının ekran görüntüsü Hendecagram animasyon")](translate-images/hendecagramanimation-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Hendecagram animasyon")

Sayfa merkezi etrafında döner olarak yıldız aynı yönü tutar dikkat edin. Hiç döndürme değil. Bir işi döndürme dönüşümü için olmasıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
