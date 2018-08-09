---
title: SkiaSharp SVG yol verileri
description: Bu makalede ölçeklenebilir vektör grafiği biçiminde metin dizeleriyle SkiaSharp yollarını nasıl tanımlanacağı açıklanmaktadır ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: f3c06198ae9e677c667c9216b3ace8784a6056b2
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615333"
---
# <a name="svg-path-data-in-skiasharp"></a>SkiaSharp SVG yol verileri

_Ölçeklenebilir Vektör Grafiği biçiminde metin dizeleriyle tanımlar_

`SKPath` Sınıfı, ölçeklenebilir vektör grafiği (SVG) belirtimi tarafından oluşturulan bir biçimde metin dizelerinden tüm yol nesneleri tanımını destekler. Bu makalenin sonraki bölümlerinde, bu bir metin dizesindeki gibi yolun tamamını nasıl temsil görürsünüz:

![](path-data-images/pathdatasample.png "SVG yol verileri ile tanımlanan bir örnek yol")

SVG programlama dilinde web sayfaları için bir XML tabanlı grafik olur. SVG bir dizi işlev çağrısının yerine Biçimlendirme tanımlanacak yolları izin vermeniz gerekir çünkü standart SVG tüm grafik yolu belirten bir metin dizesi son derece kısa bir yol içerir.

İçinde SkiaSharp, bu biçim "SVG yol-veri." olarak adlandırılır Biçim da dahil olmak üzere Windows Presentation Foundation ve burada bilinen olarak Evrensel Windows platformu, Windows XAML tabanlı programlama ortamlarında desteklenir [yol işaretleme söz dizimi](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) veya [Taşı komut söz dizimi çizin](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Ayrıca, özellikle de metin tabanlı dosyalarda XML gibi vektör grafik görüntüleri için bir exchange biçiminde görebilir.

SkiaSharp sözcüklerini içeren iki yöntem tanımlar `SvgPathData` adlarında:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Statik [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) yöntemi bir dizeye dönüştürür bir `SKPath` nesnesi sırada [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) dönüştürür bir `SKPath` nesnesine bir dize.

SVG dizisi noktasında 100 (0, 0) ile bir RADIUS ortalanmış beş işaret eden bir yıldız şu şekildedir:

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Derleme komutları harflerdir bir `SKPath` nesne. `M` gösterir bir `MoveTo` çağrısı, `L` olduğu `LineTo`, ve `Z` olduğu `Close` bir dağılımı kapatın. Her numarası çiftini bir X ve Y koordinatını bir nokta sağlar. Dikkat `L` komut virgülle ayırarak birden çok nokta ardında. Dizi koordinatlar ve noktaları, virgül ve boşluk aynı kabul edilir. X ve Y koordinatları arasında yerine noktaları arasında virgül yerleştirmek bazı programcılar tercih et ancak virgül veya boşluklarla yalnızca belirsizlik önlemek için gereklidir. Bu mükemmel geçerlidir:

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

SVG yol verileri sözdizimi resmi olarak belgelenen [bölümü 8.3 SVG belirtiminin](http://www.w3.org/TR/SVG11/paths.html#PathData). Bir özeti aşağıda verilmiştir:

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

Bu, yeni bir yol dağılımını geçerli konumu ayarlayarak başlar. Yol verileri her zaman başlamalı bir `M` komutu.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

Bu komut, düz bir çizgi (veya satırları) yolunu ekler ve son satırın sonuna kadar yeni geçerli konumunu ayarlar. İzleyebileceğiniz `L` birden çok çiftlerini komutunu *x* ve *y* koordinatları.

## <a name="horizontal-lineto"></a>**Yatay LineTo**

```csharp
H x ...
```

Bu komut, yatay çizgi yolunu ekler ve yeni geçerli konumun satırın sonuna kadar ayarlar. İzleyebileceğiniz `H` birden çok komut *x* koordinatları, ancak pek olun değil.

## <a name="vertical-line"></a>**Dikey çizgi**

```csharp
V y ...
```

Bu komut dikey çizgi yolunu ekler ve yeni geçerli konumun satırın sonuna kadar ayarlar.

## <a name="close"></a>**Kapat**

```csharp
Z
```

`C` Komutu, geçerli konumdan dağılımı başlangıcına düz bir çizgi ekleyerek dağılımı kapatır.

## <a name="arcto"></a>**ArcTo**

Elips yay için dağılımı eklemek için komutu şu ana kadar en karmaşık tüm SVG yol verileri belirtiminde komuttur. Bu sayı gösterebilir koordinat değerleri dışında bir şey yalnızca komut şöyledir:

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx* ve *rında* elipsin yarıçaplarını yatay ve dikey parametrelerdir. *Döndürme açısı* derece saat yönünde olduğu.

Ayarlama *yay bayrağı büyük* büyük yay için 1 veya 0 küçük yay için.

Ayarlama *Süpürme bayrağı* 1 saat yönünde ve 0 için yönünün.

Yayı noktasına çizilir (*x*, *y*), yeni geçerli konum haline gelir.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

Bu komut için geçerli konumdan üçüncü dereceden Bézier eğrisi ekler (*x3*, *y3*), yeni geçerli konum haline gelir. Noktaları (*x1*, *y1*) ve (*x2*, *y2*) denetim noktaları.

Birden çok Bézier eğrileri tarafından tek bir belirtilebilir `C` komutu. Puan sayısı 3'ün katı olmalıdır.

"Yumuşak" Bézier eğrisi komutu vardır:

```csharp
S x2 y2 x3 y3 ...
```

(Gerektirmesidir zorunlu olsa da) Bu komutu normal bir Bézier komutu izlemelidir. İkinci denetim noktası karşılıklı kendi nokta etrafında önceki Bézier yansıması olmasını yumuşak Bézier komutu ilk denetim noktası hesaplar. Bu nedenle bu üç nokta colinear ve iki Bézier eğrileri arasındaki bağlantıyı çok kolaydır.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

İkinci dereceden Bézier eğrileri noktalarının sayısı 2'in katı olmalıdır. Denetim noktası (*x1*, *y1*) uç noktası (ve yeni geçerli konumu) (*x2*, *y2*)

Ayrıca bir kesintisiz ikinci dereceden bir eğri komutu verilmiştir:

```csharp
T x2 y2 ...
```

Denetim noktası önceki ikinci dereceden bir eğri denetim noktasında temel alınarak hesaplanır.

Geçerli konumun göreli koordinat noktaları olduğu Bu komutlar "göreli" sürümlerinde de mevcuttur. Bu göreli komutları örneğin küçük harf ile başlayan `c` yerine `C` göreli sürümü üçüncü dereceden Bézier komutu.

SVG yol verileri tanımı kapsamını budur. Yinelenen komut gruplar için veya herhangi bir türde hesaplama gerçekleştirmek için hiçbir olanak yoktur. Komutları `ConicTo` veya diğer türleri yay özellikleri kullanılabilir değil.

Statik [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) yöntemi bekliyor SVG komut geçerli bir dize. Herhangi bir söz dizimi hatası algılanmadı ise yöntem döndürür `null`. Yalnızca hata göstergesi olmasıdır.

[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) Yöntemi mevcut bir SVG yol verileri almak için kullanışlı `SKPath` aktarmak için başka bir program veya XML gibi bir metin tabanlı dosya biçiminde depolamak için bir nesne. ( `ToSvgPathData` Yöntemi bu makaledeki örnek kodda gösterilmiştir değil.) Yapmak *değil* beklediğiniz `ToSvgPathData` tam yolun oluşturulan yöntem çağrıları için karşılık gelen bir dize döndürecek şekilde. Özellikle, yaylara birden çok dönüştürülür keşfedeceksiniz `QuadTo` komutları ve nasıl döndürüldüğü yol verileri görünürler `ToSvgPathData`.

**Yolu veri Hello** sayfasında Word'ün kullanıma spells "HELLO" SVG yol verileri kullanarak. Hem `SKPath` ve `SKPaint` nesneleri alanlar olarak tanımlanan [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) sınıfı:

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

Yolun metin dizesi tanımlama, sol üst köşesinde bir noktada (0, 0) başlar. Her harfidir 50 Birim geniş ve 100 birimi uzun ve harf, yolun tamamını 350 birimleri geniş olduğu anlamına gelir. başka bir 25 birimleri tarafından ayrılır.

'Hello"'H', 'E', iki bağlı üçüncü dereceden Bézier eğri ederken üç satır dağılımlarını oluşur. Dikkat `C` komut altı noktaları tarafından izlendiği ve iki denetim noktaları –10 ve bunları diğer harfler Y koordinatları aralığı dışında koyar 110, Y koordinatları sahip. İki bağlı satırı, 'L' olduğu sırada ' O'ile işlenen elips olduğu bir `A` komutu.

Dikkat `M` son dağılımı başlayan komut dikey merkezine sol göre olan noktaya (350, 50) konumu ayarlar tarafında ' O'. İlk sayı aşağıdaki gösterildiği gibi `A` üç nokta işaretine sahip bir yatay yarıçapını 25 ve 50 dikey bir RADIUS komutu. Bitiş noktası tarafından son çift sayıların belirtilen `A` bir nokta (300, 49.9) komutu. Başlangıç noktasından kasıtlı olarak biraz farklı olmasıdır. Uç nokta için başlangıç noktası eşit olarak ayarlanırsa, Yayı işlenmez. Eksiksiz bir elips çizme için uç nokta kapatmak için (ancak eşit değildir) ayarlamanız gerekir veya başlangıç noktasının iki veya daha fazla kullanmanız gerekir `A` komutları, her biri için tam elips parçası.

Aşağıdaki deyim sayfanın oluşturucuyu ekleyin ve ardından sonuç dizesi incelemek için bir kesme noktası ayarlamak isteyebilirsiniz:

```csharp
string str = helloPath.ToSvgPathData();
```

Yayı uzun bir dizi ile değiştirilmiştir keşfedeceksiniz `Q` kullanarak ikinci dereceden Bézier eğrileri yay parçalı bir yaklaşığını komutları.

`PaintSurface` İşleyicisi 'E' kontrol noktası içermez yolu sıkı sınırları alır ve ' O'eğrileri. Üç dönüşümleri nesnenin yolun merkezine noktası (0, 0), tuval (ancak aynı zamanda darbe genişliği hesaba katılarak) boyutunu yolunu ölçeklendirin ve sonra nesnenin yolun merkezine tuval merkezine taşır:

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

Yolun yatay modda görüntülendiğinde daha makul görünen tuvali kaplar ve:

[![](path-data-images/pathdatahello-small.png "Üçlü sayfasının ekran görüntüsü yolu veri Hello")](path-data-images/pathdatahello-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yolu veri Merhaba")

**Yolu veri Cat** sayfa benzerdir. Yol ve bir boyama nesneleri hem alanlar olarak tanımlanan [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) sınıfı:

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

Bir daire Başkanı bir cat olduğu ve burada iki oluşturulduğunda `A` komutları, her biri bir Yarım Daireli çizer. Her ikisi de `A` baş 100 yatay ve dikey yarıçaplarını tanımlar komutları. İlk yay başlar (240, 100) biter (240, 300) geriye doğru biten ikinci yay için başlangıç noktası olur (240, 100).

İki yönünde de iki işlenen `A` komutları ile cat'ın head olduğu gibi ikinci `A` komut sonlandırır başlangıç ilk olarak aynı noktada `A` komutu. Ancak, bu çiftlerini `A` komutları elips tanımlamaz. İle her yay 40 birimleri ve RADIUS ayrıca 40 birimi, bu yay tam semicircles olmadığı anlamına gelir.

`PaintSurface` İşleyici önceki örnek benzer dönüşümleri gerçekleştirir, ancak tek bir ayarlar `Scale` en boy oranını korumak ve çok az bir kenar boşluğu sağlar, böylece cat'ın yatay çizgilerinin yüzde birlik ekran tarafında dokunmayın faktörü:

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

Üç tüm platformlarda çalışan bir program şöyledir:

[![](path-data-images/pathdatacat-small.png "Üçlü sayfasının ekran görüntüsü yolu veri Cat")](path-data-images/pathdatacat-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yolu veri kat")

Normalde, bir `SKPath` nesne bir alan olarak tanımlı, yolun dağılımlarını oluşturucu veya başka bir yöntem içinde tanımlanması gerekir. Ancak, SVG yol verileri kullanırken yolu tamamen alan tanımında belirtilebilir gördünüz.

Önceki **çirkin Analog saati** içinde örnek [ **döndürme dönüştürme** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) basit satırlar halinde makale görüntülenen saat kullanımına sunun. **Oldukça Analog saati** aşağıdaki program bu satırları ile değiştirir `SKPath` alanlar olarak tanımlanan nesneler [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) ile birlikte sınıfı `SKPaint` nesneler:

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

Saat ve dakika uygulamalı artık bu uygulamalı şekilde birbirinden ayrı yapmak için alanları içine, hem bir siyah ana hat ile Gri dolgu kullanılarak çizilir `handStrokePaint` ve `handFillPaint` nesneleri.

Önceki **çirkin Analog saati** örnek, biraz saat işaretlenen daireler ve dakikada bir döngüde çizilmiş. Bu **oldukça Analog saati** örnek, tamamen farklı bir yaklaşım kullanılır: saat ve dakika işaretleri ile noktalı çizgileri olan `minuteMarkPaint` ve `hourMarkPaint` nesneler:

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

[ **Nokta ve çizgi** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) kılavuzda ele alınan nasıl kullanabileceğinizi `SKPathEffect.CreateDash` kesik çizgi oluşturmak için yöntemi. İlk bağımsız değişken bir `float` genellikle iki öğelere sahip dizi: tireler uzunluğunu ilk öğedir ve tireler arasındaki boşluk ikinci öğedir. Zaman `StrokeCap` özelliği `SKStrokeCap.Round`, dash yuvarlatılmış ucunda dash her iki tarafında darbe genişliği ile dash uzunluğu etkili bir şekilde uzatın. sonra. Bu nedenle, ilk dizi öğesinin 0 olarak ayarlanması, noktalı çizgi oluşturur.

Bu nokta arasındaki uzaklığı ikinci dizi öğesi tarafından yönetilir. Kısa bir süre sonra bu iki göreceğiniz üzere `SKPaint` nesnesi ile bir RADIUS 90 birim daireler çizme için kullanılır. Bu daire çevresi, bu nedenle 60 dakikalık işaretleri her 3π birimleri görünmelidir anlamına gelir, 180π olan ikinci değer olduğu `float` içindeki dizi `minuteMarkPaint`. On iki saat işaretlerinde her 15π birimi, ikinci değer olduğu görünmelidir `float` dizisi.

`PrettyAnalogClockPage` Sınıfı her 16 milisaniye yüzey geçersiz kılmak için bir zamanlayıcı ayarlar ve `PaintSurface` hızı işleyicisi çağrılır. Önceki tanımlarını `SKPath` ve `SKPaint` nesneleri izin vermek için kod çizim çok temiz:

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

Ancak özel bir şey ikinci el ile gerçekleştirilir. Saatin güncelleştirildiğinden her 16 milisaniye `Millisecond` özelliği `DateTime` potansiyel olarak kullanılabilir ayrık atlar taşıyan bir yerine bir gözden geçirme elle ikinci animasyon uygulamak için ikinci ikinci gelen. Ancak, bu kod kesintisiz olacak şekilde taşıma izin vermiyor. Bunun yerine, bir Xamarin.Forms kullanır [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) ve [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) animasyon kolaylaştırıcı işlevler için farklı türde bir taşıma. Kolaylaştırıcı işlevleri saniyeyi jerkier bir şekilde taşımak neden &mdash; biraz önce onu taşır ve ardından biraz efekt hedefine ne yazık ki bu aşırı atma statik bu ekran görüntülerinde üretilemeyen geri çekme:

[![](path-data-images/prettyanalogclock-small.png "Üçlü sayfasının ekran görüntüsü oldukça Analog saati")](path-data-images/prettyanalogclock-large.png#lightbox "Üçlü sayfasının ekran görüntüsü oldukça Analog saati")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
