---
title: Ölçekleme dönüşümü
description: Thhis makale nesnelerin çeşitli boyutlarına ölçeklendirmeye yönelik SkiaSharp ölçekleme dönüşümü keşfediyor ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
ms.openlocfilehash: d4ab7ad5a0fc645c13388d76eb11cbd4e2dd72f8
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615609"
---
# <a name="the-scale-transform"></a>Ölçekleme dönüşümü

_SkiaSharp ölçekleme dönüşümü, nesnelerin çeşitli boyutlarına ölçeklendirme için keşfetmek_

İçinde gördüğünüz gibi [ **Çevir dönüştürme** ](translate.md) makalesi, çeviri dönüşümü bir grafik nesnesi bir konumdan diğerine taşıyabilirsiniz. Buna karşılık, ölçekleme dönüşümü grafik nesnenin boyutunu değiştirir:

![](scale-images/scaleexample.png "Boyutu ölçeği genişletilmiş bir uzun sözcük")

Ölçekleme dönüşümü da genellikle daha büyük yapıldıkça taşımak grafik koordinatları neden olur.

Çeviri faktörünü etkilerini tanımlayan iki dönüştürme formülleri daha önce gördüğünüz `dx` ve `dy`:

x' = x + dx

y' = y + GN

Ölçek faktörünü `sx` ve `sy` yerine eklenebilir çarpma şunlardır:

x' sx · = x

y' sy · = y

0 için çeviri faktör, varsayılan değerler; 1 ölçek faktörlerin varsayılan değerlerdir.

`SKCanvas` Sınıfı tanımlar dört `Scale` yöntemleri. İlk [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single)) yöntemi, aynı yatay ve dikey ölçeklendirme istediğinizde çalışmaları faktörü için:

```csharp
public void Scale (Single s)
```

Bu olarak bilinir *isotropic* ölçeklendirme &mdash; diğer bir deyişle aynı her iki yönde ölçeklendirme. İsotropic ölçeklendirme nesnenin en boy oranını korur.

İkinci [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) yöntemi yatay ve dikey ölçekleme için farklı değerler belirtmenize olanak sağlar:

```csharp
public void Scale (Single sx, Single sy)
```

Sonuçlanır *anizotropik* ölçeklendirme.
Üçüncü [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) yöntemi tek bir iki ölçekleme faktörü birleştirir `SKPoint` değeri:

```csharp
public void Scale (SKPoint size)
```

Dördüncü `Scale` yöntemi açıklanan kısa bir süre.

**Basit bir ölçek** sayfasını gösterir `Scale` yöntemi. [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) dosyasını içeren iki `Slider` olanak tanıyan öğeleri 0 ile 10 arasındaki yatay ve dikey ölçekleme faktörü seçin. [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) arka plan kod dosyası aramak için bu değerleri kullanır `Scale` Yuvarlatılmış Dikdörtgen görüntüleme ile kesikli çizgiye konturlanan ve sol üst köşesinde metin sığdırmak için boyutta önce Tuval köşe:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Merak ediyor: ölçekleme faktörü etkilemesi döndürüldüğü değer `MeasureText` yöntemi `SKPaint`? Yanıt: hiç de değil. `Scale` bir yöntemidir `SKCanvas`. Herhangi bir şey ile bunu etkilemez bir `SKPaint` tuval üzerinde bir şey oluşturmak için bu nesneyi kullanana kadar nesne.

Her şeyi sonra çizilmiş gördüğünüz gibi `Scale` orantılı olarak artar çağırın:

[![](scale-images/basicscale-small.png "Üçlü sayfasının ekran görüntüsü basit bir ölçek")](scale-images/basicscale-large.png#lightbox "Üçlü sayfasının ekran görüntüsü basit bir ölçek")

Metin köşeler ve tuvalin sol ve üst kenarlar ve Yuvarlatılmış Dikdörtgen arasındaki piksel 10 kenar boşluğunu yuvarlama, o satırdaki tireler uzunluğunu kesik çizgi genişliğini olan tüm aynı ölçekleme faktörü tabidir.

> [!IMPORTANT]
> Evrensel Windows platformu anisotropicly ölçeklendirilmiş metin düzgün şekilde işlemez.

Yatay ve dikey ekseni olan hizalı satırları farklı olacak darbe genişliği anizotropik nedenleri ölçeklendirme. (Ayrıca bu sayfadaki ilk görüntüden yetkisiz değiştirmeye karşı korumalı budur.) Ölçeklendirme faktörlerden etkilendiği darbe genişliği istemiyorsanız 0 olarak ayarlayın ve her zaman bir piksel genişliğinde açmamasından olacaktır `Scale` ayarı.

Ölçeklendirme, tuvalin sol üst köşesine göre olan. Tam olarak ne istediğinizi kaynaklanıyor olabilir, ancak olmaması. Tuvalde metin ve başka bir yerde dikdörtgen yerleştirmek istediğiniz ve kendi merkezi göre ölçeklendirmek istediğinizi varsayalım. Bu durumda, dördüncü sürümünü kullanabilirsiniz [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) ölçeklendirmenin merkezi belirtmek için iki ek parametreler içeren yöntemi:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` Ve `py` parametreleri tanımlayan adlandırılır bir noktası *merkezi ölçeklendirme* içinde SkiaSharp belgeleri olarak adlandırılır, ancak bir *pivot noktası*. Bu bir sol üst köşesine göre ölçeklendirme tarafından etkilenmez tuvalinin noktadır. Tüm ölçeklendirme ilgili center göre gerçekleşir.

[ **Ortalanmış ölçek** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) sayfası, bunun nasıl çalıştığını gösterir. `PaintSurface` İşleyici benzer **basit bir ölçek** dışında programı `margin` değer hesaplanır metnin yatay Orta programın en iyi dikey modda çalışır anlamına gelir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Yuvarlatılmış Dikdörtgen sol üst köşesinde konumlandırılmış `margin` tuvalin solundaki piksel ve `margin` üst piksel. Son iki bağımsız değişkenleri `Scale` yöntemi, bu değerleri yanı sıra genişlik ve yükseklik genişlik ve yüksekliğini Yuvarlatılmış dikdörtgen olan metin için ayarlanır. Bu, tüm ölçeklendirme bu dikdörtgenin merkezi göreli olduğunu gösterir:

[![](scale-images/centeredscale-small.png "Üçlü sayfasının ekran görüntüsü ortalanmış ölçek")](scale-images/centeredscale-large.png#lightbox "Üçlü sayfasının ekran görüntüsü ortalanmış ölçek")

`Slider` Bu programda öğelere sahip bir dizi &ndash;10 ile 10. Gördüğünüz gibi dikey ölçeklendirme (Android Merkezi'nde ekran gibi), negatif değerler ölçeklendirme merkezi üzerinden geçirir yatay ekseni etrafında ters çevirmek nesneleri neden olur. Negatif değerler yatay (sağ UWP ekranında olduğu gibi) ölçeklendirme, ölçeklendirme merkezi üzerinden geçirir dikey ekseni etrafında ters çevirmek nesneleri neden olur.

Sürümü [ `Scale` ](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) yöntemdir pivot noktaları üç dizi için bir kısayol `Translate` ve `Scale` çağırır. Değiştirerek nasıl çalıştığını görmek isteyebilirsiniz `Scale` yönteminde **ortalanmış ölçek** aşağıdaki sayfası:

```csharp
canvas.Translate(-px, -py);
```

Pivot noktası koordinat negatif şunlardır.

Şimdi programı yeniden çalıştırın. Böylece merkezi tuvalin sol üst köşesinde bulunan metin ve dikdörtgen kaydırılacak olduğunu görürsünüz. Bu şekilleri görebilirsiniz. Program hiç şimdi ölçeklendirin değil çünkü kaydırıcıları Elbette çalışmaz.

Şimdi temel ekleyin `Scale` çağırın (ölçeklendirme olmadan bir merkezi) *önce* , `Translate` çağırın:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Alışık olduğunuz, diğer sistemler programlama grafikler, yanlış olduğunu düşünebilirsiniz, ancak bu alıştırmada değil. Skia art arda gelen dönüştürme çağrıları biraz farklı ne, alışkın olabileceğiniz işler.

Art arda ile `Scale` ve `Translate` çağrıları, Yuvarlatılmış Dikdörtgen merkezini yine de sol üst köşedeki olmakla birlikte, şimdi de Yuvarlatılmış Dikdörtgen merkezidir tuvalin sol üst köşesinin göre ölçeklendirebilirsiniz.

Şimdi, önceki `Scale` çağrı, başka bir `Translate` ortalama değerleriyle çağırın:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Bu ölçeklendirilmiş sonucu geri özgün konuma taşır. Bu üç çağrıları eşdeğerdir:

```csharp
canvas.Scale(sx, sy, px, py);
```

Böylece toplam dönüştürme formülü tek dönüşümleri compounded:

 x' sx · = (x-px) + piksel

 y' sy · = (y-py) + Kopyala

Aklınızda varsayılan değerlerini `sx` ve `sy` 1. Pivot noktası (px, Kopyala) tarafından bu formüller dönüştürülmüş değil kendiniz ikna etmek daha kolaydır. Tuval göre aynı konumda kalır.

Birleştirdiğinizde `Translate` ve `Scale` sırasını çağrılarını önemlidir. Varsa `Translate` sonra gelen `Scale`, çeviri Etkenler etkili bir şekilde ölçeklendirme faktörlerden ölçeklenir. Varsa `Translate` önce gelen `Scale`, çeviri Etkenler değil ölçeklenir. Bu işlem biraz daha anlaşılır hale gelir (paralelleştirmeye daha matematik) ne zaman dönüşümü matrislerde konusunu sunulmuştur.

`SKPath` Sınıfı tanımlar salt okunur [ `Bounds` ](xref:SkiaSharp.SKPath.Bounds) döndüren özellik bir `SKRect` yolunda koordinatları kapsamını tanımlama. Örneğin, `Bounds` özelliği daha önce oluşturduğunuz hendecagram yolundan elde `Left` ve `Top` özelliklerdir dikdörtgenin yaklaşık – 100 `Right` ve `Bottom` özellikleri yaklaşık 100 ve `Width` ve `Height` yaklaşık 200 özelliklerdir. (Gerçek değerlerle biraz daha az yıldızların noktaları bir RADIUS 100 olan bir daire tanımlanır, ancak yalnızca üst noktanın yatay veya dikey eksen ile paralel çünkü çoğu.)

Bu bilgiler kullanılabilirliğini ölçek türetilir ve tuval boyutu için bir yol ölçeklendirme için uygun olan Etkenler çevirmek mümkün olacağını gösterir. [ **Anizotropik ölçeklendirme** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) sayfası bu 11 işaret eden bir yıldızla gösterir. Bir *anizotropik* ölçek, yani yıldız özgün en boy oranı korumaz yatay ve dikey yönde eşit olup olmadığını gösterir. İlgili kod işte `PaintSurface` işleyicisi:

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

`pathBounds` Dikdörtgen bu kodu en üstüne elde ve daha sonra tuvalde yüksekliğini ve genişliğini kullanılan `Scale` çağırın. Tek başına çağrı tarafından işlendiğinde yolu koordinatlarını ölçeği, `DrawPath` çağrısı ancak yıldız ortalanmış tuvalin sağ üst köşesinde bulunan. Aşağı ve sola kaydırılacak gerekir. Bu görevi, `Translate` çağırın. Bu iki özellik, `pathBounds` yaklaşık – 100 olduğundan, yaklaşık 100 çeviri faktörlerdir. Çünkü `Translate` çağrıdır sonra `Scale` çağrısı, tuval merkezine yıldız merkezini taşınabilecek şekilde bu değerleri etkili bir şekilde ölçeklendirme unsurlar ölçeklenir:

[![](scale-images/anisotropicscaling-small.png "Üçlü sayfasının ekran görüntüsü Anizotropik ölçeklendirme")](scale-images/anisotropicscaling-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Anizotropik ölçeklendirme")

Başka bir şekilde düşünebilirsiniz `Scale` ve `Translate` çağrıdır ters sırada etkisini belirlemek için: `Translate` çağrı yönlendirilmiş tuvalin sol üst köşesinde, ancak tamamen görünür olacak şekilde yolunu geçirir. `Scale` Yöntemi sonra yapar, yıldız sol üst köşesine göre daha büyük.

Aslında, yıldız tuval biraz daha büyük olduğunu görünür. Darbe genişliği sorunudur. `Bounds` Özelliği `SKPath` boyutlarını koordinatları kodlanmış yolu ve programın ölçeklendirmek için kullandığını gösterir. Yolun bir belirli darbe genişliği ile işlendiğinde işlenmiş yolu tuvali büyüktür.

Bu sorunu düzeltmek için telafi için gerekir. Bu programda bir kolayca yaklaşım, aşağıdaki deyimi hemen önce eklemek için `Scale` çağırın:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Bu artırır `pathBounds` 1,5 birim üzerindeki dört kenara dikdörtgen. Bu, yalnızca vuruş birleştirme yuvarlandığında makul bir çözümdür. Gönye hesaplamak zordur ve daha uzun olabilir.

Bir metin ile benzer bir yöntem olarak kullanabilirsiniz **Anizotropik metin** sayfasını gösterir. İlgili bölümü işte `PaintSurface` işleyicisinden [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) sınıfı:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

Benzer bir mantık olduğunu ve döndürülen metin sınırları dikdörtgen dayanarak sayfadaki boyutunu metin genişletir `MeasureText` (olduğu gerçek metin biraz daha büyük):

[![](scale-images/anisotropictext-small.png "Üçlü sayfasının ekran görüntüsü Anizotropik Test")](scale-images/anisotropictext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Anizotropik Test")

Grafik nesneler en boy oranını koruma gerekiyorsa isotropic ölçeklendirme kullanmak isteyebilirsiniz. **Isotropic ölçeklendirme** sayfası bu için 11 işaret yıldız gösterir. Kavramsal olarak, grafik nesnesini isotropic ölçeklendirme sayfanın ortasındaki görüntülemek için adımlar şunlardır:

- Sol üst köşesinin grafik nesnenin merkezi çevir.
- Grafik nesne boyutlara göre bölünmüş yatay ve dikey sayfa boyutları en az temel nesne ölçeklendirin.
- Sayfanın merkezi ölçeklendirilmiş nesnenin merkezi çevir.

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) Yıldız görüntülemeden önce ters sırada aşağıdaki adımları gerçekleştirir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

Kod, yıldız 10 ayrıca birden fazla kez görüntüler., her zaman ölçeklendirme azalan faktörü % 10 ve aşamalı olarak kırmızı mavi rengi değiştirme:

[![](scale-images/isotropicscaling-small.png "Üçlü sayfasının ekran görüntüsü Isotropic ölçeklendirme")](scale-images/isotropicscaling-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Isotropic ölçeklendirme")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
