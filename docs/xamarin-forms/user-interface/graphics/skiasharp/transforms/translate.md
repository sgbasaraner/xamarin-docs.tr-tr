---
title: Çeviri dönüşümü
description: Bu çeviri dönüşümü Xamarin.Forms uygulamaları SkiaSharp grafiklerde kaydırma için nasıl kullanılacağını examiens makalesi ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 02361b5b2d00015ce168c075dc19522b6c04e446
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615453"
---
# <a name="the-translate-transform"></a>Çeviri dönüşümü

_Çeviri dönüşümü SkiaSharp grafik kaydırmak için kullanmayı öğrenin_

SkiaSharp dönüşüm basit türü *çevir* veya *çeviri* dönüştürün. Bu dönüşüm, yatay ve dikey yönde grafik nesneleri geçirir. Genellikle, çizme işlevde kullanmakta olduğunuz koordinatları değiştirerek aynı etkiyi gerçekleştirebilirsiniz bir anlamda, çeviri en gereksiz dönüştürme olmasıdır. Ancak, yolun tamamını kaydırmak için çeviri dönüşümü uygulanarak çok daha kolay olacak şekilde bir yol işlenirken, tüm koordinatların yolunda kapsüllenir.

Çeviri, ayrıca basit metin efektler ve animasyon için kullanışlıdır:

![](translate-images/translateexample.png "Metin gölgesinin engraving ve çevirisi kabartma")

[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) Yönteminde `SKCanvas` sonradan çizilen grafik nesneleri Yatayda ve Dikeyde kaydırılmasına neden iki parametreye sahiptir:

```csharp
public void Translate (Single dx, Single dy)
```

Bu bağımsız değişken negatif olabilir. İkinci [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/) yöntemi tek bir iki çeviri değerleri birleştirir `SKPoint` değeri:

```csharp
public void Translate (SKPoint point)
```

**Birikmiş çevir** sayfasının [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) örnek program gösterir, birden çok çağıran `Translate` toplu yöntemi. [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Sınıfı aynı dikdörtgen 20 sürümlerini görüntüler, bunlar köşegen esnetme için her bir önceki dikdörtgenden yetecek kadar uzaklık. İşte `PaintSurface` olay işleyicisi:

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

Birikmiş çeviri Etkenler yoksa `dx` ve `dy`, ve bir çizme işlevde belirttiğiniz noktası (`x`, `y`), grafik nesnesini noktada işlenen sonra (`x'`, `y'`), burada:

x' = x + dx

y' = y + GN

Bunlar olarak bilinen *dönüştürme formülleri* çeviri için. Varsayılan değerleri `dx` ve `dy` için yeni bir `SKCanvas` 0.

Çeviri dönüşümü olarak gölge efektleri ve benzer teknikler için kullanımı yaygındır **Çevir Yazı efektleri** sayfasını gösterir. İlgili bölümü işte `PaintSurface` işleyicisinde [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) sınıfı:

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

Her üç örnek `Translate` tarafından verilen konumda uzaklığı için metin görüntülemek için çağrılır `x` ve `y` değişkenleri. Sonra metni yeniden çeviri etkisi ile başka bir renkte görüntülenir:

[![](translate-images/translatetexteffects-small.png "Üçlü sayfasının ekran görüntüsü Çevir Yazı efektleri")](translate-images/translatetexteffects-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Yazı efektleri Çevir")

Her üç örnek gösterir negating farklı bir yol `Translate` çağırın:

Yalnızca ilk örneği çağrıları `Translate` yeniden ancak negatif değerlere sahip. Çünkü `Translate` çağrılarıdır toplu, bu sırasını çağrılarını, toplam çeviri yalnızca sıfır varsayılan değerlere geri yükler.

İkinci örnek çağrıları [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/). Bu, varsayılan durumlarına geri döndürülecek tüm dönüşümler neden olur.

Üçüncü örnek durumunu kaydeder, `SKCanvas` nesne çağrısıyla [ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/) çağrısıyla durumunu geri yüklemek [ `Restore` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/). Bu dönüştürmeler için bir dizi işlem çizim işlemek için en verimli yoludur. Bunlar `Save` ve `Restore` çağıran işlevi bir yığın gibi: çağırabilirsiniz `Save` birden çok zaman ve sonra çağrı `Restore` içinde ters önceki duruma geri dönmek için dizisi. `Save` Yöntemi, bir tamsayı döndürür ve bu tamsayı geçirerek [ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/) etkili bir şekilde çağırmak için `Restore` birden çok kez. [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/) Özelliği şu anda yığında kaydedilmiş durumların sayısını döndürür.

Ancak, bir çağrıdan taşıyan dönüşümleri hakkında endişelenmeniz gerekmez `PaintSurface` sonraki işleyici. Her yeni çağrı `PaintSurface` yeni teslim `SKCanvas` varsayılan dönüşümlerle nesne.

Başka bir yaygın kullanımı `Translate` dönüşüm olduğu için bir görsel nesneyi işleme ilk çizim için kullanışlı olan koordinatları kullanılarak oluşturuldu. Örneğin, bir merkezi bir noktada (0, 0) ile bir analog saat koordinatlarını belirtmek isteyebilirsiniz. Dönüşümler görüntülemek için istediğiniz yerde sonra kullanabilirsiniz. Bu gösterilmiştir [**Hendecagram dizi**] sayfa. [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) Sınıfı başlar oluşturarak bir `SKPath` 11 işaret eden bir yıldız nesnesi. `HendecagramPath` Nesne tanımlanmış olarak genel, statik ve salt okunur böylece başka tanıtım programlar tarafından erişilebilir. Statik Oluşturucu oluşturuldu:

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

Yıldız merkezini noktası (0, 0) ise, tüm yıldızın o noktadan çevresindeki bir daireye noktalarıdır. Her bir birleşimi 360 derece 5/11ths tarafından sunan açı Sinüs ve Kosinüs değerlerini noktasıdır. (2 açıyı artırarak 11 işaret eden bir yıldız oluşturmak da mümkündür/11, 3/11ths ya da 4/dairenin 11.) RADIUS, dairenin 100 ayarlanır.

Bu yolu dönüşümleri işlenen, sol üst köşesinde merkezi yerleştirileceğini `SKCanvas`ve bunu bir Çeyreğin görünür olur. `PaintSurface` İşleyicisi `HendecagramPage` bunun yerine kullanır `Translate` tuval birden çok kopyasını yıldız ile döşeme için her birine rastgele renkli:

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

Sonuç şu şekildedir:

[![](translate-images/hendecagramarray-small.png "Üçlü sayfasının ekran görüntüsü Hendecagram dizi")](translate-images/hendecagramarray-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Hendecagram dizi")

Animasyonları genellikle dönüşümler içerir. **Hendecagram animasyon** sayfa 11 işaret yıldız bir daire taşır. [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) Sınıfı bazı alanları ile başlar ve, geçersiz kılmalar `OnAppearing` ve `OnDisappearing` Xamarin.Forms Zamanlayıcıyı durdurmak ve başlatmak yöntemleri:

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

`angle` Alan animasyon 0-360 derece 5 saniyede. `PaintSurface` İşleyicisi kullanır `angle` iki yolla özelliği: ton rengi belirtmek için `SKColor.FromHsl` yöntemi ve bağımsız değişken olarak `Math.Sin` ve `Math.Cos` yöntemleri yıldız konumunu yönetmek için:

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

`PaintSurface` İşleyicisi çağrılarını `Translate` ilk tuval merkezine çevirmek için iki kez yöntemi ve ardından geçici Orta dairenin çevresi çevrilecek (0, 0). Hala sayfanın sınırlar içinde yıldız mümkün olduğunca büyük olmasına olanak tanırken dairenin RADIUS ayarlayın:

[![](translate-images/hendecagramanimation-small.png "Üçlü sayfasının ekran görüntüsü Hendecagram animasyon")](translate-images/hendecagramanimation-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Hendecagram animasyon")

Sayfanın merkezi etrafında döner olarak yıldız aynı yönünü korur dikkat edin. Hiç döndürme değil. Bir işi döndürme dönüşümü için olmasıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
