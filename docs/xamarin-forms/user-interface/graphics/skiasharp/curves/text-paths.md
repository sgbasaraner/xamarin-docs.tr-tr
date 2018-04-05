---
title: Yollar ve metin
description: Yollar ve metin kesişimi keşfedin
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: c0b793a495278d91429045d7e396917d02c1412e
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="paths-and-text"></a>Yollar ve metin

_Yollar ve metin kesişimi keşfedin_

Modern grafik sistemlerinde metin yazı tipi karakter anahatları, genelde ikinci derece Bézier eğrileri tarafından tanımlanan bir koleksiyonlarıdır. Sonuç olarak, birçok modern grafik sistemi bir grafik yola metin karakterlerinin dönüştürmek için bir özellik içerir. 

Metin karakterlerinin anahatları vuruş yanı sıra, onları doldurmak gördünüz. Bu sayede açıklandığı gibi bu karakter anahatları belirli vuruşun genişliğini ve yol etkisi görüntülenecek [ **yolu etkileri** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) makale. Ancak bir karakter dizesine dönüştürmek mümkündür bir `SKPath` nesnesi. Bu metin anahatları bölümünde açıklanan teknikleri ile kırpma için kullanılabileceği anlamına gelir [ **yolları ve bölgeler kırpma** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) makalesi. 

Karakter ana hattını vuruş yapmak için bir yol efekti kullanarak yanı sıra yollarına bağlı etkileri karakter dizeleri türetilen yol oluşturabilirsiniz ve iki etkileri bile birleştirebilirsiniz:

![](text-paths-images/pathsandtextsample.png "Metin yolu efekti")

İçinde [önceki makalede](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) gördüğünüz nasıl [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) yöntemi `SKPaint` konturlu bir yolu bir özetini elde edebilirsiniz. Bu yöntem karakter anahatları türetilen yollar ile de kullanabilirsiniz.

Son olarak, bu makalede yolları ve metin başka bir kesişimi gösterilmektedir: [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) yöntemi `SKCanvas` metnin taban eğik bir yolu aşağıdaki metin dizesini görüntülemenizi sağlar.

## <a name="text-to-path-conversion"></a>Metin yolu dönüştürme

[ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) Yöntemi `SKPaint` bir karakter dizesine dönüştürür bir `SKPath` nesnesi:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x` Ve `y` bağımsız değişkenleri metnin sol tarafındaki temel başlangıç noktası belirtin. Aynı role burada olarak yürütmek `DrawText` yöntemi `SKCanvas`. Yol içinde metnin sol tarafındaki temel koordinatları (x, y) sahip olacaktır. 

`GetTextPath` Yöntemdir gereğinden fazla yalnızca doldurun veya sonuç yolu vuruş yapmak istiyorsanız. Normal `DrawText` yöntemi, yapmanıza olanak sağlar. `GetTextPath` Yöntemdir yollarını içeren diğer görevler için daha kullanışlı.

Bu görevlerden birini kırpma. **Kırpma metin** sayfa kırpması yol "CODE" word karakter ana hatlarını temelli oluşturur Bu yolu bir görüntüsünü içeren bir bit eşlem küçük sayfa boyutunu genişletilir **kırpma metin** kaynak kodu:

[![](text-paths-images/clippingtext-small.png "Üçlü sayfasının ekran görüntüsü kırpma metin")](text-paths-images/clippingtext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü kırpma metin")

[ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) Sınıfı oluşturucusu katıştırılmış bir kaynak olarak depolanan bit eşlemi yükleyen **medya** klasörü çözümün:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

`PaintSurface` İşleyici başlar oluşturarak bir `SKPaint` nesnesi için metin uygun. `Typeface` Özelliği ayarlanmış yanı sıra `TextSize`, bu belirli bir uygulama olsa da `TextSize` tamamen arbirtrary bir özelliktir. Ayrıca fark hiçbir `Style` ayarı.

`TextSize` Ve `Style` özellik ayarları, gerekli değildir çünkü bu `SKPaint` nesnesi yalnızca için kullanılır `GetTextPath` "CODE" metin dizesini kullanarak çağırın. İşleyici sonra sonuç ölçer `SKPath` nesne ve onu merkezi ve sayfa boyutunu ölçeklendirmek için üç dönüşümler uygular. Yolun kırpma yolu olarak ayarlayabilirsiniz:

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

Kırpma yolunu ayarladıktan sonra bit eşlem görüntülenebilir ve karakter anahatları kırpılır. Kullanımına dikkat edin [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) yöntemi `SKRect` en boy oranını koruyarak sayfayı doldurmak için bir dikdörtgen hesaplar.

**Metin yolu etkisini** sayfa 1 D yolu efekti oluşturmak için bir yol için tek bir karakterini dönüştürür. Bu yol etkiyi boyama nesnesiyle daha sonra aynı karakterin büyük bir sürümü anahat vuruş yapmak için kullanılır:

[![](text-paths-images/textpatheffect-small.png "Üçlü sayfasının ekran görüntüsü metin yolu etkisini")](text-paths-images/textpatheffect-large.png#lightbox "Üçlü sayfasının ekran görüntüsü metin yolu efekti")

İş kadar [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) sınıfı alanlarını ve Oluşturucusu oluşur. İki `SKPaint` nesneleri iki farklı amaçlar için kullanılan alanları olarak tanımlanmamış: ilk (adlı `textPathPaint`) simgesi ile dönüştürmek için kullanılan bir `TextSize` 1 D yolu etkisi yoluna 50. İkinci (`textPaint`) simgesi yolu etkileyen ile büyük sürümünü görüntülemek için kullanılır. Bu nedenle, `Style` nesnesi bu ikinci boyama kümesine `Stroke`, ancak `StrokeWidth` özelliği 1 D yolu etkisi kullanırken bu özellik gerekli değildir çünkü ayarlı değil:

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
        SKRect textPathPaintBounds;
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

Oluşturucusu ilk kullanan `textPathPaint` simgesi ile ölçmek için nesne bir `TextSize` 50. Bu dikdörtgeni merkezi koordinatlarını negatif ardından geçirilen `GetTextPath` metni için bir yol dönüştürmek için yöntem. Sonuç yoluna sahip (0, 0) noktası için 1 D yolu efekti idealdir karakter Center'da.

Düşünebilirsiniz `SKPathEffect` Oluşturucusu sonunda oluşturulan nesne kümesine `PathEffect` özelliği `textPaint` yerine bir alan olarak kaydedilir. Ancak sonuçlarını bozuk olduğundan çok iyi çalışması için bu kullanıma açık değil `MeasureText` Çağır `PaintSurface` işleyici:

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
        SKRect textBounds;
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

Olduğunu `MeasureText` çağrı, sayfa karakteri merkezi için kullanılır. Sorunları önlemek için `PathEffect` özelliğini metin ölçülen sonra ancak gösterilmeden önce Boyama nesnesine ayarlayın.

## <a name="outlines-of-character-outlines"></a>Karakter anahatları ana hatlarını

Normalde [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) yöntemi `SKPaint` bir yol boyama özellikleri uygulayarak diğerine dönüştürür özellikle vuruşun genişliğini ve yol etkisi. Yol etkilerini kullanıldığında `GetFillPath` etkili bir şekilde başka bir yol özetlenmektedir bir yol oluşturur. Bu örnekte gösterildiği **yolu anahattına dokunun** sayfasındaki [ **yolu etkileri** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) makale.

Ayrıca, çağırabilirsiniz `GetFillPath` döndürülen yolunda `GetTextPath` ancak ilk başta, hangi, görünüm ister tamamen emin olmayabilir.

**Karakter ana hattını anahatları** sayfa tekniği gösterir. Tüm ilgili kod `PaintSurface` işleyicisine [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) sınıfı.

Oluşturucusu oluşturarak başlar bir `SKPaint` adlı nesne `textPaint` ile bir `TextSize` özellik sayfasının boyutuna bağlı. Bu yol kullanmaya dönüştürülür `GetTextPath` yöntemi. Koordinat değişkenleri `GetTextPath` etkili bir şekilde yolun ekranında Merkezi:

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
        SKRect textBounds;
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

`PaintSurface` İşleyicisi sonra adlı yeni bir yol oluşturur `outlinePath`. Bu çağrı hedef yolunda olur `GetFillPath`. `StrokeWidth` 25 nedenler özelliğinin `outlinePath` metin karakterlerinin vuruş yapması 25 piksel genişliğinde yol anahat açıklamak için. Bu yol ardından 5 vuruşun genişliğini kırmızı görüntülenir:

[![](text-paths-images/characteroutlineoutlines-small.png "Üçlü sayfasının ekran görüntüsü karakter ana hattını anahatları")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Üçlü sayfasının ekran görüntüsü karakter ana hattını anahatları")

Yakından arayın ve burada yol anahattı keskin köşe yapar çakışmalar görürsünüz. Bu işlemin normal yapılarını bunlar.

## <a name="text-along-a-path"></a>Yol boyunca metin

Metin üzerinde yatay bir taban çizgisi normal olarak görüntülenir. Metnin dikey veya çapraz olarak çalıştırmak için Döndürülmüş, ancak temel hala bir çizgide olur.

Bir eğri çalıştırmak için metin istediğinizde zamanlar vardır. Bu amacı [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) yöntemi `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

İlk bağımsız değişkeninde belirtilen metin, ikinci bağımsız değişkeni olarak belirtilen yolda çalıştırmak için yapılır. Yolu başından uzaklık metni başlayabilirsiniz `hOffset` bağımsız değişkeni. Normalde yolu metnin taban formlar: metin Üst Çıkıntısı yolunun bir tarafta, ve metin harfin alt diğer bağlı. Ancak metin taban çizgisini ile yolundan aralıklandırabilirsiniz `vOffset` bağımsız değişkeni.

Bu yöntem ayarda yönergeler sağlamak için hiçbir olanak sahip `TextSize` özelliği `SKPaint` yolu baştan sona çalıştırmak için mükemmel boyutta bir metin yapmak için. Bazen bu metin boyutu kendi anlayabilir. Bazen bir sonraki makalede açıklanan için yol ölçme işlevleri kullanmanız gerekir.

**Döngüsel metin** program daire metin sarmalar. Tam olarak sığması için metin boyutu kolaydır, dairenin çevresi belirlemek kolaydır. `PaintSurface` İşleyicisine [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) sınıfı bir RADIUS sayfa boyutuna göre dairenin hesaplar. Bu daireye dönüşür `circularPath`:

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

`TextSize` Özelliği `textPaint` böylece metin genişliği çevresini eşleşir sonra ayarlanır:

[![](text-paths-images/circulartext-small.png "Üçlü sayfasının ekran görüntüsü döngüsel metin")](text-paths-images/circulartext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü döngüsel metin")

Metnin kendisi de biraz döngüsel olarak seçildi: "daire" tümcenin konu hem edat tümcecik nesnesinin sözcüğüdür. 

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
