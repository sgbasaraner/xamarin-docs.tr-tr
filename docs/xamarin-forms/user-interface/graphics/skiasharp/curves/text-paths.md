---
title: Yollar ve SkiaSharp metni
description: Bu makalede SkiaSharp yollar ve metin keşfediyor ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: e7ce6994541ae947fa714d3c67acbc5d5d816975
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130786"
---
# <a name="paths-and-text-in-skiasharp"></a>Yollar ve SkiaSharp metni

_Yollar ve metin keşfedin_

Modern grafikler sistemlerinde metin yazı tipleri genellikle ikinci dereceden Bézier eğrileri tarafından tanımlanan karakter ana hatlarını koleksiyonlarıdır. Sonuç olarak, birçok modern grafik sistemleri metin karakterleri grafik yola dönüştürmek için bir özellik içerir.

Metin karakterleri anahatlarını vuruş yapabilir, bunları doldurun gördünüz. Bu sayede belirli darbe genişliği ve yol etkisi ile bu karakter anahat bölümünde anlatıldığı gibi görüntülenecek [ **yol etkileri** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) makalesi. Bir karakter dizisine dönüştürmek mümkündür, ancak bir `SKPath` nesne. Bu metin anahatlarını açıklanan tekniklerle kırpma için kullanılabileceği anlamına gelir [ **yollar ve bölgeler ile kırpma** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) makalesi.

Karakter anahat vuruş yapmak için bir yol efekt kullanarak yanı sıra bir yollarına bağlı etkileri karakter dizeleri türetilen yol oluşturabilirsiniz ve iki etkisi bile birleştirebilirsiniz:

![](text-paths-images/pathsandtextsample.png "Metin yolu efekti")

İçinde [önceki makalede](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) gördüğünüz nasıl [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) yöntemi `SKPaint` konturlu yolu bir özetini elde edebilirsiniz. Bu yöntem, karakter anahatlarını türetilen yol ile de kullanabilirsiniz.

Son olarak, bu makalede, yollar ve metin başka bir kesişimi gösterilmektedir: [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) yöntemi `SKCanvas` eğik bir yolu düzleştirerek taban metni izleyen metin dizesi görüntülemenizi sağlar.

## <a name="text-to-path-conversion"></a>Metin dönüştürme yolu

[ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) Yöntemi `SKPaint` bir karakter dizesine dönüştürür bir `SKPath` nesnesi:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x` Ve `y` metnin sol tarafındaki taban başlangıç noktası bağımsız değişkenleri belirtin. Aynı role burada gibi oyun `DrawText` yöntemi `SKCanvas`. Yol, metnin sol tarafındaki taban koordinatları (x, y) sahip olacaktır.

`GetTextPath` Yöntemdir düşünülecek yalnızca doldurun veya sonuçta elde edilen yolu vuruş yapmak istiyorsanız. Normal `DrawText` yöntemi bunun olanak tanır. `GetTextPath` Yöntemdir daha yollarını içeren diğer görevler için kullanışlı.

Bu görevlerden birini kırpma. **Kırpma metin** "CODE" sözcük karakteri ana hatlarını dayalı bir kırpma yolu sayfası oluşturur Bu yol, bir bit eşlem görüntüsünü içeren küçük sayfa boyutunu esnetilmiş **metin kırpmayı** kaynak kodu:

[![](text-paths-images/clippingtext-small.png "Üçlü sayfasının ekran görüntüsü metin kırpmayı")](text-paths-images/clippingtext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü metni kırpma")

[ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) Sınıf oluşturucusu bir gömülü kaynak olarak depolanan bir bit eşlem yükler **medya** Çözüm klasörü:

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

`PaintSurface` İşleyici başlar oluşturarak bir `SKPaint` metin için uygun olan nesne. `Typeface` Özelliği yanı sıra `TextSize`, bu belirli bir uygulama olsa da `TextSize` tamamen arbirtrary özelliğidir. Ayrıca fark hiçbir `Style` ayarı.

`TextSize` Ve `Style` özelliği ayarlar gerekli değildir çünkü bu `SKPaint` nesnesi yalnızca için kullanılır `GetTextPath` "CODE" metin dizesi kullanarak çağırın. İşleyici ardından sonuç ölçer `SKPath` nesne ve onu merkezi ve sayfa boyutu için ölçeklendirmek için üç dönüşümleri uygular. Yolu kırpma yolu şekilde ayarlayabilirsiniz:

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap,
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

Kırpma yolunu ayarladıktan sonra bit eşlem görüntülenebilir ve için karakter anahatlarını kırpılır. Kullanımına dikkat edin [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) yöntemi `SKRect` en boy oranını koruyarak sayfayı doldurmak için bir dikdörtgen hesaplar.

**Metin yolu etkisini** sayfa 1 D yolu efektini oluşturmak için bir yol için tek bir karakterini dönüştürür. Bu yol etkisi olan bir boyama nesnesi daha sonra bu aynı karakterin büyük sürümünün anahat vuruş yapmak için kullanılır:

[![](text-paths-images/textpatheffect-small.png "Üçlü sayfasının ekran görüntüsü metin yolu etkisini")](text-paths-images/textpatheffect-large.png#lightbox "Üçlü sayfasının ekran görüntüsü metin yolu efekti")

İş kadar [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) sınıfı oluşturucusu ve alanlar içinde gerçekleşir. İki `SKPaint` iki farklı amaçlar için kullanılan alanlar olarak tanımlanan nesneleri: ilk (adlı `textPathPaint`) işareti ile dönüştürmek için kullanılan bir `TextSize` 50 1b yolunu etkili bir yolu. İkinci (`textPaint`) önekine bu yolu etkisi olan daha büyük sürümünü görüntülemek için kullanılır. Bu nedenle `Style` nesne bu ikinci Boya kümesine `Stroke`, ancak `StrokeWidth` 1b yolu efektini kullanırken bu özelliğin gerekli olmadığı için özellik ayarlanmadı:

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds = new SKRect();
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character,
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

İlk Oluşturucu kullanan `textPathPaint` ampersan ile ölçmek için nesne bir `TextSize` 50. Bu dikdörtgenin merkezi koordinat negatif sonra geçirilen `GetTextPath` metni bir yola dönüştürmek için yöntemi. Sonuç yoluna sahip (0, 0) işaret merkezinde karakterinin 1 D yolu efekt için idealdir.

Düşündüğünüzden `SKPathEffect` Oluşturucu sonunda oluşturulan nesne ayarlanabilir `PathEffect` özelliği `textPaint` yerine bir alan olarak kaydedilir. Ancak sonuçları bozuk olduğundan çok iyi çalışması için bu kullanıma açık değil `MeasureText` Çağır `PaintSurface` işleyicisi:

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

Olduğunu `MeasureText` çağrı Merkezi sayfasında karakter için kullanılır. Sorunları önlemek için `PathEffect` metin ölçülen sonra ancak gösterilmeden önce özelliği Boyama nesnesine ayarlanır.

## <a name="outlines-of-character-outlines"></a>Karakter anahatlarını ana hatlarını

Normalde [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) yöntemi `SKPaint` Boya özellikleri uygulayarak bir yolu dönüştürür özellikle vuruş genişlik ve yol etkisi. Yol etkileri kullanıldığında `GetFillPath` etkili bir şekilde başka bir yol özetleyen bir yol oluşturur. Bu, devremenin **anahattına yolu dokunun** sayfasını [ **yol etkileri** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) makale.

Ayrıca, çağırabilirsiniz `GetFillPath` döndürülen yolda `GetTextPath` ancak ilk olarak, aşağıdaki gibi görünür olduğundan emin olabilir değil.

**Karakter anahat anahatlarını** sayfa teknik gösterir. Tüm ilgili kodu `PaintSurface` işleyicisi [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) sınıfı.

Oluşturucu oluşturarak başlıyor bir `SKPaint` adlı nesne `textPaint` ile bir `TextSize` özellik sayfa boyutuna bağlı. Bu bir yol kullanarak dönüştürülür `GetTextPath` yöntemi. Koordinat bağımsız değişkenleri `GetTextPath` ekranında yolunu etkili bir şekilde Merkezi:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds = new SKRect();
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

`PaintSurface` İşleyicisi oluşturur sonra adlı yeni bir yol `outlinePath`. Bu çağrı için hedef yolda olur `GetFillPath`. `StrokeWidth` 25 neden özelliği `outlinePath` metin karakterleri konturlama 25-piksel genişliğinde yol ana hatlarını tanımlayacak. Bu yol, sonra bir vuruş genişliğiyle 5 kırmızı renkte görüntülenir:

[![](text-paths-images/characteroutlineoutlines-small.png "Üçlü sayfasının ekran görüntüsü karakter anahat anahatlarını")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Üçlü sayfasının ekran görüntüsü karakter anahat anahatlarını")

Yakından bakın ve burada yolu anahat keskin köşe yapar çakışıyor görürsünüz. Bu işlemin normal yapıtları şunlardır.

## <a name="text-along-a-path"></a>Yol metin

Metin üzerinde yatay bir taban çizgisi normal olarak görüntülenir. Metin dikey veya çapraz olarak çalıştırmak için Döndürülmüş, ancak temel düz bir çizgi hala olduğunu.

Eğri çalıştırmak için metin istediğinizde zamanlar vardır. Amacı budur [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) yöntemi `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

İkinci bağımsız değişkeni belirtilen yol boyunca çalıştırmak için ilk bağımsız değişkeni olarak belirtilen metin yapılır. Metin yoluyla başından itibaren bir uzaklıkta başlayabilirsiniz `hOffset` bağımsız değişken. Normalde yol metninin temel forms: metin Üst Çıkıntısı olan yolun bir tarafında ve metin çıkıntılarını olan diğer bağlı. Ancak yoluyla metin taban çizgisinden aralıklandırabilirsiniz `vOffset` bağımsız değişken.

Bu yöntem bulunmaz ayarda rehberlik sahip `TextSize` özelliği `SKPaint` yolu başından sonuna kadar çalıştırmak için mükemmel bir şekilde boyutlandırılmış metni yapmak için. Bazen, metin boyutu kendi anlayabilir. Bazen yolu ölçme işlevleri gelecek bir makalede açıklanan için kullanmanız gerekecektir.

**Döngüsel metin** program metin daire içine sarmalar. Tam olarak sığması için metin boyutu daha kolaydır, dairenin çevresi belirlemek kolay bir işlemdir. `PaintSurface` İşleyicisi [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) sınıfı bir sayfa boyutuna göre bir daire yarıçapını hesaplar. Bu daireye dönüşür `circularPath`:

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

`TextSize` Özelliği `textPaint` metin genişliğini çevresini eşleşmesi sonra ayarlanır:

[![](text-paths-images/circulartext-small.png "Üçlü sayfasının ekran görüntüsü döngüsel metin")](text-paths-images/circulartext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü döngüsel metin")

Metnin kendisi de biraz döngüsel olarak seçildi: "daire" cümlenin konu hem edat tümcecik nesnesinin sözcüktür.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
