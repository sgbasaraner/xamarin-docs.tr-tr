---
title: SVG yol verileri
description: Ölçeklenebilir vektör grafikleri biçiminde metin dizelerini kullanarak yolları tanımlayın
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: 7ea99612f85a853bcd045b773df0a01f33427a89
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="svg-path-data"></a>SVG yol verileri

_Ölçeklenebilir vektör grafikleri biçiminde metin dizelerini kullanarak yolları tanımlayın_

`SKPath` Sınıfı, ölçeklenebilir vektör grafikleri (SVG) belirtimi tarafından oluşturulan bir biçimde metin dizelerini yolun tamamını nesnelerden tanımını destekler. Bu makalenin sonraki bölümlerinde, bu bir metin dizesindeki gibi yolun tamamını nasıl gösterebilir görürsünüz:

![](path-data-images/pathdatasample.png "SVG yol verileri ile tanımlanan bir örnek yol")

SVG programlama dili web sayfaları için bir XML tabanlı grafik uygundur. SVG bir dizi işlev çağrısı yerine biçimlendirme tanımlı yolları izin vermesi gerekir çünkü standart SVG bir tüm grafik yolu bir metin dizesi belirterek, son derece kısa bir yol içerir.

SkiaSharp içinde bu biçim için "SVG yol-veriler." denir Biçim da Windows Presentation Foundation ve burada bu bilinen olarak Evrensel Windows platformu dahil olmak üzere Windows XAML tabanlı programlama ortamlarının desteklenir [biçimlendirme sözdizimi yolu](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) veya [Taşı ve komutları sözdizimi çizin](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Ayrıca, özellikle de metin tabanlı dosyalarda XML gibi vektör grafik görüntüleri için exchange biçimi olarak görebilir.

SkiaSharp tanımlar sözcükleri iki yöntemle `SvgPathData` adlarında:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Statik [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) yöntemi bir dizeye dönüştürür bir `SKPath` nesnesi sırada [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) dönüştürür bir `SKPath` bir dize nesnesi.

Bir noktasında 100 (0, 0) bir RADIUS ile ortalanmış beş işaret yıldız SVG dize şöyledir:

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Derleme komutları harf olan bir `SKPath` nesnesi. `M` gösterir bir `MoveTo` çağrısı, `L` olan `LineTo`, ve `Z` olan `Close` bir dağılım kapatın. Her numarası çifti bir noktasının X ve Y koordinatı sağlar. Dikkat `L` komutu virgülle ayırarak birden çok nokta ardında. Dizi koordinatları, noktaları, virgül ve boşluk özdeş olarak kabul edilir. Bazı programcıları X ve Y koordinatları arasında yerine noktaları arasında virgül koymak tercih ettiğiniz, ancak virgül veya boşluk yalnızca Karışıklığı önlemek için gereklidir. Mükemmel yasal şudur:

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

SVG yol verileri söz dizimi resmi olarak belgelenen [bölüm 8.3 SVG belirtimi](http://www.w3.org/TR/SVG11/paths.html#PathData). Bir özeti aşağıda verilmiştir:

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

Bu, yeni bir dağılım yolunda geçerli konumu ayarlayarak başlar. Yol verileri ile her zaman başlamalıdır bir `M` komutu.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

Bu komut bir çizgide (veya satırlar) yolunu ekler ve son satırın sonuna yeni geçerli konumunu ayarlar. İzleyebileceğiniz `L` birden çok çiftlerini komutunu *x* ve *y* koordinatları.

## <a name="horizontal-lineto"></a>**Yatay LineTo**

```csharp
H x ...
```

Bu komut yolunu yatay çizgi ekler ve satırın sonuna yeni geçerli konumunu ayarlar. İzleyebileceğiniz `H` birden çok komutuyla *x* koordinatları, ancak daha anlamlı olun değil.

## <a name="vertical-line"></a>**Dikey çizgi**

```csharp
V y ...
```

Bu komut, dikey çizgi yolu ekler ve yeni geçerli konumu satırın sonuna ayarlar.

## <a name="close"></a>**Kapat**

```csharp
Z
```

`C` Komutu bir çizgide geçerli konumundan dağılımı başlangıcına ekleyerek dağılımı kapatır.

## <a name="arcto"></a>**ArcTo**

Elips yay dağılımı eklemek için komutu kullanarak en karmaşık tüm SVG yol verileri belirtiminde komuttur. Sayı gösterebilir koordinat değerleri dışında bir şey yalnızca komut şöyledir:

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx* ve *rında* yatay ve dikey elipsin yarıçaplarını parametreleridir. *Döndürme açısı* derece saat yönünde değil.

Ayarlama *yay bayrağı büyük* için büyük Yayı 1 veya 0 küçük yay için.

Ayarlama *tarama bayrağı* için 1 saat yönünde ve için 0 yönünün.

Yayı noktasına çizilir (*x*, *y*), geçerli konuma haline gelir.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

Bu komut için geçerli konumundan küp Bézier eğrisi ekler (*x3*, *y3*), geçerli konuma haline gelir. Noktaları (*x1*, *y1*) ve (*x2*, *y2*) denetim noktaları.

Birden çok Bézier eğrileri tarafından tek bir belirtilebilir `C` komutu. Noktası sayısı 3 katı olmalıdır.

"Yumuşak" Bézier eğrisi komutu vardır:

```csharp
S x2 y2 x3 y3 ...
```

Bu komut, (, kesinlikle olmadığında gerekli rağmen) normal bir Bézier komutu uygun olmalıdır. Kendi karşılıklı noktası etrafında önceki Bézier ikinci denetim noktası yansımasını olmasını kesintisiz Bézier komutu ilk denetim noktası hesaplar. Bu üç nokta bu nedenle colinear ve iki Bézier eğrileri arasındaki bağlantıyı çok kolaydır.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

İkinci derece Bézier eğrileri noktası sayısı 2'ın katları olmalıdır. Denetim noktası (*x1*, *y1*) ve uç noktası (ve yeni geçerli konumu) (*x2*, *y2*)

Kesintisiz ikinci derece eğrisi komutu vardır:

```csharp
T x2 y2 ...
```

Denetim noktası önceki ikinci dereceden eğrisi denetim noktası temel alınarak hesaplanır.

Bu komutlar de, koordinat noktaları geçerli konumuna göre nerede "göreli" sürümlerinde kullanılabilir. Bu göreli komutlar örneğin küçük harflerle başlayan `c` yerine `C` küp Bézier komutu göreli sürümü.

SVG yolu veri tanımı kapsamını budur. Yinelenen komutların gruplar için veya herhangi bir türde hesaplama gerçekleştirmek için hiçbir olanak yoktur. Komutları `ConicTo` veya yay belirtimleri diğer türleri kullanılabilir değil.

Statik [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) yöntemi SVG komutların geçerli bir dize bekliyor. Herhangi bir sözdizimi hatası algılandı, yöntem `null`. Yalnızca hata göstergesi olmasıdır.

[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) Yöntemdir mevcut bir SVG yol verileri almak için kullanışlı `SKPath` nesne başka bir programa aktarmak veya XML gibi metin tabanlı bir dosya biçiminde depolamak için. ( `ToSvgPathData` Yöntemi bu makaledeki örnek kodda gösterildiği değil.) Yapmak *değil* beklediğiniz `ToSvgPathData` tam yolunu oluşturulan yöntem çağrıları için karşılık gelen bir dize döndürecek şekilde. Özellikle, birden çok yaylar dönüştürülür öğreneceksiniz `QuadTo` komutları ve nasıl döndürüldüğü yolu veri göründükleri `ToSvgPathData`.

**Yolu veri Hello** sayfasında spells word çıkışı "MERHABA" SVG yol verileri kullanarak. Hem `SKPath` ve `SKPaint` nesneleri alanlar olarak tanımlanan [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) sınıfı:

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

Yolun metin dizesi tanımlama (0, 0) noktası sol üst köşede başlar. 50 birime geniş ve 100 birim uzun her harfi olduğu ve harf yolun tamamını 350 birimleri geniş olduğu anlamına gelir başka bir 25 birimler tarafından ayrılır.

İki bağlı küp Bézier eğrileri 'E' iken 'Hello"'Y' üç tek satır dağılımlarını oluşur. Dikkat `C` komutu altı noktaları tarafından izlendiği ve iki denetim noktalarının Y koordinatları –10 ve diğer harfler Y koordinatları aralığın dışında koyar 110 sahip. İki bağlı satırdan 'L' olduğu sırada ' O'ile işlenen elips olan bir `A` komutu.

Dikkat `M` son dağılımı başlar komut Sol dikey merkezine olan noktasına (350, 50) konumunu ayarlar tarafında ' O'. İlk sayı aşağıdaki tarafından belirtildiği şekilde `A` komutunu elips yatay RADIUS 25 ve 50 dikey bir RADIUS sahiptir. Uç noktası numaraları son çiftinin belirtilir `A` (300, 49.9) noktasını temsil eder komutu. Başlangıç noktasından biraz kasıtlı olarak farklı olmasıdır. Uç nokta için başlangıç noktası eşit olarak ayarlanırsa, Yayı işlenmez. Tam bir Elips çizmek için uç nokta kapatmak için (ancak eşit değil) ayarlamanız gerekir başlangıç noktası için veya iki veya daha fazla kullanmalıdır `A` komutlar, her tam elips parçası için.

Aşağıdaki deyim sayfanın oluşturucuya ekleyin ve ardından sonuç dize incelemek için bir kesme noktası ayarlayın isteyebilirsiniz:

```csharp
string str = helloPath.ToSvgPathData();
```

Yayı uzun bir dizi ile değiştirilmiştir öğreneceksiniz `Q` parça parça yaklaşık ikinci derece Bézier eğrileri kullanarak yay olarak için komutları.

`PaintSurface` İşleyicisi 'E' için denetim noktalarını içermez yolu sıkı sınırları alır ve ' O'Eğriler. Üç dönüşümler yolun Merkezi (0, 0) noktasına, tuvale (ancak aynı zamanda vuruşun genişliğini dikkate alarak) boyutunu yoluna ölçeklendirme ve sonra yolu merkezi Kanvasın ortasına taşır:

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

Yolun yatay modunda görüntülendiğinde daha makul arar tuvale doldurur:

[![](path-data-images/pathdatahello-small.png "Üçlü sayfasının ekran görüntüsü yolu veri Hello")](path-data-images/pathdatahello-large.png#lightbox "sayfasının yolu veri Hello Üçlü ekran görüntüsü")

**Yolu veri kat** sayfa benzerdir. Yol ve boyama nesneleri her ikisi de alanlar olarak tanımlanan [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) sınıfı:

```csharp
public class PathDataCatPage : ContentPage
{
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

Bir kat head bir daire ve burada bu iki işlenen `A` komutları, her biri bir Yarım Daireli çizer. Her ikisi de `A` komutları, head 100 yatay ve dikey yarıçaplarını tanımlar. İlk Yayı başlar (240, 100) ve bitişi (240, 300) geri bitişi ikinci Yayı için başlangıç noktası olur (240, 100).

İki gözler ikisinde de işlendiğini `A` komutları ile kedilerle ilgili head gibi ikinci `A` komut sonlandırır ilk başlangıcı aynı noktada `A` komutu. Ancak, bu çiftlerini `A` komutları elips tanımlamaz. İle her yay 40 birimleri ve RADIUS ayrıca 40 birimleri, bu yaylar tam semicircles anlamına gelir.

`PaintSurface` İşleyici önceki örnek benzer dönüşümleri gerçekleştirir, ancak tek bir ayarlar `Scale` faktörü en boy oranını korumak ve çok az kenar boşluğu sağlar, böylece kedilerle ilgili yatay çizgilerinin ekran yanlarından touch yok:

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

Aşağıda, tüm üç platformlarında çalışan program verilmiştir:

[![](path-data-images/pathdatacat-small.png "Üçlü sayfasının ekran görüntüsü yolu veri kat")](path-data-images/pathdatacat-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yolu veri kat")

Normalde, bir `SKPath` nesnesi, bir alan olarak tanımlanır, yolun dağılımlarını oluşturucu veya başka bir yöntem içinde tanımlanması gerekir. Ancak, SVG yol verileri kullanırken yolu tamamen alan tanımında belirtilebilir gördünüz.

Önceki **çirkin Analog Saat** içinde örnek [ **döndürme dönüştürme** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) makale saatin ellerini basit satır olarak görüntülenir. **Oldukça Analog Saat** aşağıdaki program bu satırlarla değiştirir `SKPath` alanlar olarak tanımlı nesneler [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) ile birlikte sınıf `SKPaint` nesneler:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

Saat ve dakika ellerini şimdi bu nedenle bu ellerini birbirinden ayrı yapmak için alanları içine, hem bir siyah anahat ile Gri dolgu kullanarak çizilir `handStrokePaint` ve `handFillPaint` nesneleri.

Önceki içinde **çirkin Analog Saat** örnek, az saatleri işaretlenen daireler ve dakikada bir döngüde çizilmiş. Bu **oldukça Analog Saat** örnek, tamamen farklı bir yaklaşım kullanılır: saat ve dakika işaretleri noktalı ile çizilmiş çizgiler kalan `minuteMarkPaint` ve `hourMarkPaint` nesneler:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

[ **Nokta ve kısa çizgilerden** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) Kılavuzu ele alınan nasıl kullanacağınızı `SKPathEffect.CreateDash` kesikli çizgi oluşturmak için yöntemi. İlk bağımsız değişken bir `float` genellikle iki öğeye sahip bir dizi: ilk öğe tireler uzunluğu ve tireler arasındaki boşluk ikinci öğedir. Zaman `StrokeCap` özelliği ayarlanmış `SKStrokeCap.Round`, yuvarlak çizgi uçlarından tire her iki tarafında vuruşun genişliğini tarafından tire uzunluğu etkili bir şekilde uzatmak sonra. Bu nedenle, ilk dizi öğesi 0 olarak ayarlanması noktalı çizgi oluşturur.

Bu noktalar arasındaki uzaklığı ikinci bir dizi öğesi tarafından yönetilir. Kısa bir süre sonra bu iki göreceğiniz gibi `SKPaint` nesnesi ile bir RADIUS 90 birim daireler çizin için kullanılır. Bu daire çevresi bu nedenle 60 dakika işaretleri her 3π birimleri görünmelidir anlamına gelir, 180π olan ikinci değeri olduğu `float` içinde dizi `minuteMarkPaint`. İkincisi değer olan her 15π birimleri için on iki saat işaretleri görünmesi gerekir `float` dizi.

`PrettyAnalogClockPage` Sınıfı, her 16 milisaniye yüzeyini geçersiz kılmak için bir zamanlayıcı ayarlar ve `PaintSurface` işleyicisi o oranda çağrılır. Önceki tanımlarını `SKPath` ve `SKPaint` nesneleri izin vermek için kod çizim çok temiz:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

Ancak özel bir şey ikinci el ile gerçekleştirilir. Saatin güncelleştirildiğinden her 16 milisaniye `Millisecond` özelliği `DateTime` değer büyük olasılıkla bir tarama ayrık atlar içinde taşınan bir yerine ikinci elle animasyon için kullanılabilir ikinci ikinci gelen. Ancak bu kodu kesintisiz olması için taşıma izin vermiyor. Bunun yerine, Xamarin.Forms kullanan [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) ve [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) animasyon işlevleri farklı türde bir taşıma için kolaylaştırma. Bu işlevler kolaylaştırma saniyeyi jerkier bir biçimde taşımanızı neden &mdash; biraz önce onu taşır ve ardından biraz efekt hedefine ne yazık ki bu aşırı çekim statik bu ekran görüntülerinde çoğaltılamayan geri çekme:

[![](path-data-images/prettyanalogclock-small.png "Üçlü sayfasının ekran görüntüsü oldukça Analog Saat")](path-data-images/prettyanalogclock-large.png#lightbox "Üçlü sayfasının ekran görüntüsü oldukça Analog Saat")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
