---
title: Oluşturma ve SkiaSharp bit eşlemler üzerinde çizim
description: SkiaSharp bit eşlemleri oluşturmanıza ve bunları temel alan tuval oluşturarak bu bit eşlemler üzerinde çizin öğrenin.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79BD3266-D457-4E50-BDDF-33450035FA0F
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: c8ddf8c0829cea319dd93dd9c3686b94ed8eb89e
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615596"
---
# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>Oluşturma ve SkiaSharp bit eşlemler üzerinde çizim

Nasıl bir uygulama bit eşlemler Web, uygulama kaynakları ve kullanıcının Fotoğraf Kitaplığı'ndan yükleyebilir gördünüz. Uygulamanızdaki yeni bit eşlemleri oluşturmanıza da mümkündür. En kolay yaklaşım bir oluşturucular içerir [ `SKBitmap` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKBitmap.SKBitmap/p/System.Int32/System.Int32/System.Boolean/):

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

`width` Ve `height` parametreleri tamsayılar ve bit eşlemin piksel boyutunu belirtin. Bu oluşturucu, piksel başına dört bayt ile tam renkli bir bit eşlem oluşturur: bir bayt her kırmızı, yeşil, mavi ve alfa (opaklık) bileşenleri için. 

Yeni bir bit eşlem oluşturduktan sonra bir bit eşlem yüzeyinde almanız gerekir. Genellikle bunu iki yoldan biriyle yapabilirsiniz:

- Standart kullanarak bit eşlemi çizme `Canvas` çizim yöntemleri.
- Piksel BITS doğrudan erişir.

Bu makalede, ilk yaklaşım gösterilmektedir:

![Örnek çizim](drawing-images/DrawingSample.png "örnek çizme")

İkinci yaklaşım makalesinde açıklanan [ **erişme SkiaSharp bit eşlem piksel**](pixel-bits.md).

## <a name="drawing-on-the-bitmap"></a>Üzerinde bir bit eşlemi çizme

Bir bit eşlem yüzeyinde çizerek bir görüntü çizme aynıdır. Bir görüntü çizin için edinmeniz bir `SKCanvas` nesnesinden `PaintSurface` olay bağımsız değişkenleri. Üzerinde bir bit eşlemi çizme için oluşturduğunuz bir `SKCanvas` kullanarak nesne [ `SKCanvas` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKCanvas.SKCanvas/p/SkiaSharp.SKBitmap/) Oluşturucusu:

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

Bit eşlem çizim tamamladığınızda, elden `SKCanvas` nesne. Bu nedenle, `SKCanvas` Oluşturucusu çağırılmadan genellikle bir `using` deyimi:

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

Bit eşlem sonra görüntülenebilir. Program daha sonraki bir zamanda yeni bir oluşturabilir `SKCanvas` nesnesini temel alan aynı bit eşlem ve üzerinde biraz daha çizin.

**Hello bit eşlem** sayfasını **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** uygulama, "Hello, bit eşlem!" metin yazar bir bit eşlem ve birden çok kez bit eşlem sonra görüntüler.  

Oluşturucusuna `HelloBitmapPage` oluşturarak başlıyor bir `SKPaint` metin görüntülemek için nesne. Bir metin dizesi boyutunu belirler ve bu boyutları ile bir bit eşlem oluşturur. Ardından oluşturur bir `SKCanvas` nesne tabanlı bu bitmap, çağrıları `Clear`ve ardından çağırır `DrawText`. Bu her zaman çağırmak için iyi bir fikirdir `Clear` yeni bir bit eşlem ile yeni oluşturulan bir bit eşlem rastgele veriler içerebileceğinden.

Oluşturucu oluşturarak sonucuna bir `SKCanvasView` bit eşlem görüntülemek için nesne:

```csharp
public partial class HelloBitmapPage : ContentPage
{
    const string TEXT = "Hello, Bitmap!";
    SKBitmap helloBitmap;

    public HelloBitmapPage()
    {
        Title = TEXT;

        // Create bitmap and draw on it
        using (SKPaint textPaint = new SKPaint { TextSize = 48 })
        {
            SKRect bounds = new SKRect();
            textPaint.MeasureText(TEXT, ref bounds);

            helloBitmap = new SKBitmap((int)bounds.Right,
                                       (int)bounds.Height);

            using (SKCanvas bitmapCanvas = new SKCanvas(helloBitmap))
            {
                bitmapCanvas.Clear();
                bitmapCanvas.DrawText(TEXT, 0, -bounds.Top, textPaint);
            }
        }

        // Create SKCanvasView to view result 
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Aqua);

        for (float y = 0; y < info.Height; y += helloBitmap.Height)
            for (float x = 0; x < info.Width; x += helloBitmap.Width)
            {
                canvas.DrawBitmap(helloBitmap, x, y);
            }
    }
}
```

`PaintSurface` İşleyici içinde birden çok kez satırları ve sütunları görüntü bit eşlem işler. Dikkat `Clear` yönteminde `PaintSurface` işleyici olan bağımsız değişkeninin `SKColors.Aqua`, uzaklaştırabilir, arka plan renkleri:

[![Bit eşlem Merhaba! ] (drawing-images/HelloBitmap.png "Merhaba, bit eşlem!")](drawing-images/HelloBitmap-Large.png#lightbox)

Bit eşlem dışında metin saydamdır Deniz Mavisi arka plan görünümünü gösterir.

## <a name="clearing-and-transparency"></a>Temizleme ve saydamlığı

Görüntülenmesini **Hello bit eşlem** sayfa bit eşlem programda saydam siyah metin dışında olduğunu gösterir. İşte bu aracılığıyla uzaklaştırabilir, Açık Deniz Mavisi rengindeki gösterir.

Belgelerine `Clear` yöntemlerinin `SKCanvas` deyimiyle açıklar: "tuval geçerli küçük tüm pikselleri değiştirir." Bu yöntemlerin bir önemli özelliği "değiştirir" sözcük kullanımını gösterir: çizim yöntemlerinin `SKCanvas` bir şey var olan görüntü yüzeyine ekleyin. `Clear` Yöntemleri _değiştirin_ ne zaten oradadır.

`Clear` iki farklı sürümde var: 

- [ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear/p/SkiaSharp.SKColor/) Yöntemi ile bir `SKColor` parametresi uzaklaştırabilir piksellerini renge piksel ile değiştirir.

- [ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Yönteminin parametresiz değiştirir piksel [ `SKColors.Empty` ](https://developer.xamarin.com/api/property/SkiaSharp.SKColors.Empty/) içindeki tüm bileşenleri (kırmızı, yeşil, mavi ve alfa) sıfır olarak ayarlanmış bir renk olan renk. Bu renk bazen "saydam siyah olarak" adlandırılır

Çağırma `Clear` bağımsız değişken içermeyen yeni bir bit eşlem üzerinde tamamen saydam olarak tüm bit eşlem başlatır. Bit eşlem sonradan alınan herhangi bir şey genellikle opak ya da kısmen donuk olur.

Denemek için bir şey şöyledir: içinde **Hello bit eşlem** sayfasında, yerine `Clear` uygulanan yöntemi `bitmapCanvas` bu bir:

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

Sırasını `SKColor` Oluşturucu parametresi, kırmızı, yeşil, mavi ve alfa, burada her değer 0 ile 255 arasında değişebilir. 255 alfa değeri donuk ederken alfa değeri 0'ın saydam olduğunu aklınızda bulundurun.

Bit eşlem piksel % 50 opaklık ile kırmızı piksel (255, 0, 0, 128) değeri temizler. Bu, bit eşlem arka plan yarı saydam olduğu anlamına gelir. Bit eşlemin kırmızı yarı saydam arka Deniz Mavisi gri renkli bir arka plan oluşturmak için görüntü yüzey arka plan ile birleştirir.

Metin rengi aşağıdaki atama koyarak saydam siyah olarak ayarlamayı deneyin `SKPaint` Başlatıcı:

```csharp
Color = new SKColor(0, 0, 0, 0)
```

Bu saydam bir metin uzaklaştırabilir Deniz Mavisi arka planını görür bit eşlemin tamamen saydam alanlara oluşturacak düşünebilirsiniz. Ancak diğer bir deyişle bu nedenle değil. Metin, bit eşlem olduğunu üzerine çizilir. Saydam bir metin hiç görünür olmaz.

Hayır `Draw` yöntemi her zamankinden kılar bir bit eşlem daha saydam. Yalnızca `Clear` bunu yapabilirsiniz.

## <a name="bitmap-color-types"></a>Bit eşlem renk türleri

En basit `SKBitmap` Oluşturucu, bir tamsayı piksel genişlik ve yükseklik eşlemiyle belirtmenize olanak sağlar. Diğer `SKBitmap` oluşturucular daha karmaşık. Bu oluşturucular iki sabit listesi türü bağımsız değişkenleri gerektirir: [ `SKColorType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColorType/) ve [ `SKAlphaType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKAlphaType/). Diğer oluşturucular kullan [ `SKImageInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/) yapısı, bu bilgileri bir araya getirir.

`SKColorType` Numaralandırma 9 üyeleri vardır. Bu üyeler bit eşlem piksel depolamak için belirli bir şekilde açıklanmıştır:

- `Unknown`
- `Alpha8` &mdash; Alfa değeri tam opak gösteren tamamen saydam 8 bitlik her piksel olan
- `Rgb565` &mdash; her piksel olan 5 BITS kırmızı ve mavi ve yeşil için 6 16 bit
- `Argb4444` &mdash; 16 bit, 4: her alfa, kırmızı, yeşil ve mavi her piksel olan
- `Rgba8888` &mdash; 32 bit ve 8 her kırmızı, yeşil, mavi ve alfa her piksel olan
- `Bgra8888` &mdash; 32 bit ve 8 her için mavi yeşil kırmızı ve alfa her piksel olan
- `Index8` &mdash; her piksel 8 bittir ve bir dizine temsil eden bir [`SKColorTable`](https://developer.xamarin.com/api/type/SkiaSharp.SKColorTable/)
- `Gray8` &mdash; bir gri tonu siyahtan beyaza temsil eden 8 bitlik her piksel olan
- `RgbaF16` &mdash; her piksel olan kırmızı, yeşil, mavi ve alfa 16-bit kayan nokta biçiminde ile 64 bit

Genellikle her piksel 32 piksel (4 bayt) olduğu iki biçim adlı _tam renkli_ biçimleri. Birçok video kendilerini görüntülendiğinde bir zamana ait diğer biçimleri tarihin tam rengini özelliğine sahip değildi. Sınırlı renk bit eşlemler, bu görüntüler için yeterli ve bellekte daha az alan kaplaması bit eşlemleri izin. 

Bugünlerde, programcılar neredeyse her zaman tam renk bit eşlemler kullanın ve sahip diğer biçimlere rahatsız yok. Özel durum `RgbaF16` biçiminde tam renkli biçimleri bile daha büyük çözünürlüğü sağlar. Ancak, bu biçim, tıbbi görüntüleme gibi özelleştirilmiş amaçlar için kullanılır ve ile standart tam renkli görüntüler kullanıldığında kadar anlam ifade etmez.

Bu makale serisi, kendisine kısıtlar `SKBitmap` renk hiçbir varsayılan olarak kullanılan biçimleri `SKColorType` belirtilen üyesidir. Bu varsayılan biçimi, temel alınan platformu üzerinde temel alır. Xamarin.Forms tarafından desteklenen platformlar için varsayılan rengi türüdür:

- `Rgba8888` iOS ve Android için
- `Bgra8888` UWP için

Tek fark, bellek, 4 bayt sırası ve doğrudan piksel BITS eriştiğinizde, bu yalnızca bir sorun haline gelir. Makaleyi gelene kadar bu önemli hale gelmiştir olmaz [ **erişme SkiaSharp bit eşlem piksel**](pixel-bits.md).

`SKAlphaType` Numaralandırma dört üyeleri vardır:

- `Unknown`
- `Opaque` &mdash; bit eşlem yok saydamlık var
- `Premul` &mdash; renk bileşenlerine alfa bileşeni tarafından önceden çarpıldığı
- `Unpremul` &mdash; renk bileşenlerine alfa bileşeni tarafından önceden çarpıldığı değil

Bir 4 baytlık kırmızı bit eşlem piksel ile gösterilen sırada kırmızı, yeşil, mavi, alfa bayt olan % 50 saydamlık şu şekildedir:

0xFF 0x00 0x00 0x80

Yarı saydam piksel içeren bir bit eşlem görünen yüzeyinde işlendiğinde her bit eşlem piksel renk bileşenlerinin pikselin alfa değeri ile çarpılmasına ve uzaklaştırabilir, karşılık gelen piksel renk bileşenlerinin çarpılmasına gerekir Alfa değeri eksi 255 tarafından. İki piksel ardından birleştirilebilir. Bit eşlem piksel renk bileşenlerine öncesi mulitplied zaten verilmiş olması durumunda bit eşlemin daha hızlı alfa değeri tarafından işlenebilir. Bu aynı kırmızı piksel şöyle önceden çarpılan bir biçimde depolanır:

0x80 0x00 0x00 0x80

Bu performans iyileştirmesi işte `SkiaSharp` ile bit eşlemler varsayılan olarak oluşturulan bir `Premul` biçimi. Ancak yeniden sadece erişmek ve piksel BITS işlemek bilmek gerekli olur.

## <a name="drawing-on-existing-bitmaps"></a>Var olan bit eşlemler üzerinde çizim

Üzerinde çizmek için yeni bir bit eşlem oluşturmak gerekli değildir. Ayrıca, mevcut bir bit eşlemi çizebilirsiniz. 

**Monkey Moustache** sayfası yüklemek için yapıcısına kullanır **MonkeyFace.png** görüntü. Ardından oluşturur bir `SKCanvas` nesne, bit eşlem tabanlı ve kullandığı `SKPaint` ve `SKPath` bir moustache çizmek için nesneler:

```csharp
public partial class MonkeyMoustachePage : ContentPage
{
    SKBitmap monkeyBitmap;

    public MonkeyMoustachePage()
    {
        Title = "Monkey Moustache";

        monkeyBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MonkeyFace.png");

        // Create canvas based on bitmap
        using (SKCanvas canvas = new SKCanvas(monkeyBitmap))
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 24;
                paint.StrokeCap = SKStrokeCap.Round;

                using (SKPath path = new SKPath())
                {
                    path.MoveTo(380, 390);
                    path.CubicTo(560, 390, 560, 280, 500, 280);

                    path.MoveTo(320, 390);
                    path.CubicTo(140, 390, 140, 280, 200, 280);

                    canvas.DrawPath(path, paint);
                }
            }
        }

        // Create SKCanvasView to view result 
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Oluşturucu oluşturarak sonucuna bir `SKCanvasView` olan `PaintSurface` işleyici yalnızca sonuç görüntüler:

[![Monkey Moustache](drawing-images/MonkeyMoustache.png "Monkey Moustache")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>Kopyalama ve bit eşlemler değiştirme

Yöntemlerinin `SKCanvas` çizmek için kullanabileceğiniz bir bit eşlem dahil `DrawBitmap`. Bu, bir bit eşlem başka genellikle bir şekilde değiştirmeye çizebilirsiniz olduğunu anlamına gelir.

Bir bit eşlem değiştirmek için en verimli şekilde gerçek piksel BITS erişim aracılığıyla, bir konu makalede ele alınan  **[erişme SkiaSharp bit eşlem piksel](pixel-bits.md)**. Ancak diğer birçok tekniği piksel BITS erişme gerektirmeyen bit eşlemler değişiklik vardır.

Bulunan aşağıdaki bit eşlem **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** uygulamasıdır 360 piksel genişliğinde ve 480 piksel cinsinden yüksekliği:

![Sıradağlar Climbers](drawing-images/MountainClimbers.jpg "Sıradağlar Climbers")

Bu fotoğraf yayımlamak için soldaki monkey izni edinmemiş varsayalım. Tek bir çözüm olarak adlandırılan tekniği kullanarak monkey'nın yüz gizlememeniz etmektir _pixelization_. Özellikleri yapamazsınız. Bu nedenle yüzü piksel renk taşları ile değiştirilir. Renk bloklarını, genellikle bu bloklar için karşılık gelen piksel renklerini ortalayarak orijinal görüntüden türetilir. Ancak, kendinizi bu ortalama gerçekleştirmeniz gerekmez. Bir bit eşlem daha küçük bir piksel boyutuna göre kopyaladığınızda otomatik olarak gerçekleşir. 

Sol monkey's yüz yaklaşık bir 72 piksel kare alanıyla (112, 238) noktasında bir sol üst köşesinin kaplar. Şimdi bu 72 piksel kare alan her biri 8-tarafından-8 piksel kare olan bir 9 x 9 dizisi renkli bloğu ile değiştirin.

**Pixelize görüntü** sayfası, bit eşlem içinde yükler ve ilk adlı küçük bir 9-piksel kare bit eşlem oluşturur `faceBitmap`. Yalnızca monkey'nın yüz kopyalamak için bir hedef budur. Hedef dikdörtgenin yalnızca 9-piksel kare, ancak kaynak dikdörtgenin 72 piksel kare. Kaynak piksel her 8-tarafından-8 bloğu tek piksel aşağı renkleri ortalayarak birleştirilir.

Özgün bit eşlem aynı boyutta adlı yeni bir bitmap içine kopyalamak için sonraki adımdır `pixelizedBitmap`. Küçük `faceBitmap` ardından üstüne ile 72 piksel kare hedef dikdörtgene kopyalanır böylece, her pikselin `faceBitmap` 8 katı boyutuna genişletilir:

```csharp
public class PixelizedImagePage : ContentPage
{
    SKBitmap pixelizedBitmap;

    public PixelizedImagePage ()
    {
        Title = "Pixelize Image";

        SKBitmap originalBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        // Create tiny bitmap for pixelized face
        SKBitmap faceBitmap = new SKBitmap(9, 9);

        // Copy subset of original bitmap to that
        using (SKCanvas canvas = new SKCanvas(faceBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(originalBitmap,
                              new SKRect(112, 238, 184, 310),   // source
                              new SKRect(0, 0, 9, 9));          // destination

        }

        // Create full-sized bitmap for copy
        pixelizedBitmap = new SKBitmap(originalBitmap.Width, originalBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(pixelizedBitmap))
        {
            canvas.Clear();

            // Draw original in full size
            canvas.DrawBitmap(originalBitmap, new SKPoint());

            // Draw tiny bitmap to cover face
            canvas.DrawBitmap(faceBitmap, 
                              new SKRect(112, 238, 184, 310));  // destination
        }

        // Create SKCanvasView to view result
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(pixelizedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Oluşturucu oluşturarak sonucuna bir `SKCanvasView` sonucu görüntülemek için:

[![Görüntü pixelize](drawing-images/PixelizeImage.png "görüntü Pixelize")](drawing-images/PixelizeImage-Large.png#lightbox)

<a name="rotating-bitmaps" />

## <a name="rotating-bitmaps"></a>Bit eşlemler döndürme

Başka bir yaygın görev, bit eşlemler döndürme. Bit eşlemler bir iPhone veya iPad fotoğraf kitaplığından alınırken özellikle yararlıdır. Fotoğraf alındığında, cihaz belirli bir yönde tutulan sürece, resmi baş aşağı veya sideways olması olasıdır.

Bir bit eşlem kapatma baş aşağı ilkiyle aynı boyutta başka bir bit eşlem oluşturma ve ardından ikinci ilk kopyalanırken 180 derece döndürme dönüşümü ayarlama gerektirir. Bu bölümdeki örneklerde tümünde `bitmap` olduğu `SKBitmap` döndürmek için ihtiyacınız olan nesne:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

90 derece döndürme yaparken yüksekliğini ve genişliğini değiştirerek orijinalden farklı boyutta olan bit eşlem oluşturma gerekir. Örneğin, özgün bit eşlem 1200 piksel genişliğinde ve 800 piksel yüksekliğinde ise, döndürülen bir bit eşlem 800 piksel genişliğinde ve 1200 piksel genişliğinde olur. Bit eşlem, sol üst köşesinin döndürülebilir ve ardından görünüme geçiş yaptı, çeviri ve döndürme ayarlayın. (Aklınızda `Translate` ve `RotateDegrees` yöntemleri, bunlar uygulanır şekilde ters sırada denir.) Saat yönünde 90 derece döndürme için kod aşağıdaki gibidir:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(bitmap.Height, 0);
    canvas.RotateDegrees(90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```                        

Ve 90 derece saat yönünün döndürme için benzer bir işlev şu şekildedir:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(0, bitmap.Width);
    canvas.RotateDegrees(-90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Bu iki yöntem kullanılır **fotoğraf Bulmaca** makalesinde açıklanan sayfaları [ **kırpma SkiaSharp bit eşlemler**](cropping.md#tile-division).

Bir bit eşlem 90 derecelik artışlarla döndürmek kullanıcıya izin veren bir program yalnızca 90 derece döndürme için bir işlev uygulamanız. Kullanıcı, daha sonra yinelenen bu bir işlevin yürütülmesini ile 90 derece herhangi bir artış döndürebilirsiniz.

Bir program, herhangi bir miktarda bir bit eşlem da döndürebilirsiniz. Bir basit yaklaşımdır 180 genelleştirilmiş ile değiştirerek 180 derece döndürür işlevi değiştirilecek `angle` değişkeni:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Ancak, genel durumda bu mantık döndürülen bir bit eşlem köşelerini kırpma. Bu köşeler içerecek şekilde Trigonometri kullanarak Döndürülmüş bit eşlem boyutunu hesaplamak daha iyi bir yaklaşımdır. 

Bu Trigonometri gösterilen **bit eşlem değiştirici** sayfası. XAML dosyası oluşturur bir `SKCanvasView` ve `Slider` , aralığında 0-360 derece ile aracılığıyla bir `Label` geçerli değeri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapRotatorPage"
             Title="Bitmap Rotator">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="slider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Rotate by {0:F0}&#x00B0;'}"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Arka plan kod dosyasında bir bit eşlem kaynağındaki yükler ve adlı bir statik salt okunur bir alan olarak kaydeder `originalBitmap`. Görüntülenen bit eşlem `PaintSurface` işleyicisi `rotatedBitmap`, başlangıçta Hosted `originalBitmap`:

```csharp
public partial class BitmapRotatorPage : ContentPage
{
    static readonly SKBitmap originalBitmap = 
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap rotatedBitmap = originalBitmap;

    public BitmapRotatorPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(rotatedBitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double angle = args.NewValue;
        double radians = Math.PI * angle / 180;
        float sine = (float)Math.Abs(Math.Sin(radians));
        float cosine = (float)Math.Abs(Math.Cos(radians));
        int originalWidth = originalBitmap.Width;
        int originalHeight = originalBitmap.Height;
        int rotatedWidth = (int)(cosine * originalWidth + sine * originalHeight);
        int rotatedHeight = (int)(cosine * originalHeight + sine * originalWidth);

        rotatedBitmap = new SKBitmap(rotatedWidth, rotatedHeight);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear(SKColors.LightPink);
            canvas.Translate(rotatedWidth / 2, rotatedHeight / 2);
            canvas.RotateDegrees((float)angle);
            canvas.Translate(-originalWidth / 2, -originalHeight / 2);
            canvas.DrawBitmap(originalBitmap, new SKPoint());
        }

        canvasView.InvalidateSurface();
    }
}
```

`ValueChanged` İşleyicisi `Slider` yeni bir işlemler gerçekleştiren `rotatedBitmap` döndürme açısı üzerinde temel. Yeni genişlik ve yükseklik absolute values sinüs kuralı ve özgün genişlik ve yükseklik Kosinüs kuralı bağlıdır. Özgün bit eşlem Döndürülmüş bit eşlem çizmek için kullanılan dönüşümler özgün bit eşlem merkezi başlangıcına Taşı sonra belirtilen derece sayısına göre döndürme ve ardından bu merkezi döndürülen bir bit eşlem merkezine çevir. ( `Translate` Ve `RotateDegrees` yöntemleri nasıl uygulanacağını daha ters sırada çağrılır.)

Kullanımına dikkat edin `Clear` arka planı yapmak yöntemi `rotatedBitmap` açık pembe. Yalnızca boyutunu göstermek için budur `rotatedBitmap` ekranında:

[![Bit eşlem değiştirici](drawing-images/BitmapRotator.png "bit eşlem değiştirici")](drawing-images/BitmapRotator-Large.png#lightbox)

Döndürülen bir bit eşlem büyük ancak tüm özgün bitmap içerecek şekilde yalnızca gerektirecek kadar büyüktür.

## <a name="flipping-bitmaps"></a>Bit eşlemler çevirme

Başka bir işlem üzerinde bit eşlemler yaygın olarak gerçekleştirilen çağrılır _çevirme_. Kavramsal olarak, dikey eksen ya da bit eşlem ortasından yatay ekseni etrafında üç farklı boyutta bir bit eşlem döndürülür. Dikey olarak çevirme bir Ayna görüntüsünü oluşturur.

**Bit eşlem Çevirici** sayfasını **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** uygulama demonstates bu işlemler. XAML dosyası içeren bir `SKCanvasView` ve dikey ve yatay çevirme için iki düğme:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapFlipperPage"
             Title="Bitmap Flipper">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Flip Vertical"
                Grid.Row="1" Grid.Column="0"
                Margin="0, 10"
                Clicked="OnFlipVerticalClicked" />

        <Button Text="Flip Horizontal"
                Grid.Row="1" Grid.Column="1"
                Margin="0, 10"
                Clicked="OnFlipHorizontalClicked" />
    </Grid>
</ContentPage>
```

Bu iki işlemleri arka plan kod dosyası uygulayan `Clicked` düğmeleri için işleyiciler: 

```csharp
public partial class BitmapFlipperPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public BitmapFlipperPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnFlipVerticalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(-1, 1, bitmap.Width / 2, 0);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnFlipHorizontalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(1, -1, 0, bitmap.Height / 2);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }
}
```

Dikey Çevir yatay ölçekleme faktörü ile ölçekleme dönüşümü tarafından gerçekleştirilir &ndash;1. Bit eşlemin dikey merkezine ölçeklendirme merkezidir. Ölçekleme dönüşümü dikey bir ölçekleme faktörü ile Yatay Çevir olan &ndash;1. 

Monkey'nın shirt ters yitirmeyi gelen görebileceğiniz gibi çevirme döndürme aynı değildir. Ancak, sağdaki UWP ekran görüntüsünde gösterildiği gibi her ikisi de yatay ve dikey olarak çevirme 180 derece döndürme aynıdır:

[![Bit eşlem Çevirici](drawing-images/BitmapFlipper.png "bit eşlem Çevirici")](drawing-images/BitmapFlipper-Large.png#lightbox)

Benzer teknikler kullanılarak işlenebilir başka bir yaygın görev bir bit eşlem dikdörtgen alt kırpma. Bu sonraki makalede açıklanan [ **kırpma SkiaSharp bit eşlemler**](cropping.md).

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
