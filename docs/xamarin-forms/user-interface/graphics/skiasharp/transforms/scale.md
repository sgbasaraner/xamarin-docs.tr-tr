---
title: Ölçek dönüştürme
description: Çeşitli boyutlarda nesnelere ölçeklendirmeye yönelik SkiaSharp ölçeklendirme dönüşümü Bul
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 4c2650d4586f210b121c4c72b79e92ce72d135fe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="the-scale-transform"></a>Ölçek dönüştürme

_Çeşitli boyutlarda nesnelere ölçeklendirmeye yönelik SkiaSharp ölçeklendirme dönüşümü Bul_

İçinde gördüğünüz gibi [Çevir Dönüştür](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md) makalenin Çevir Dönüştür bir grafik nesnesi bir konumdan diğerine taşıyabilirsiniz. Buna karşılık, ölçeklendirme dönüşümü grafik nesnenin boyutu değişir:

![](scale-images/scaleexample.png "Boyutu ölçeği uzun bir sözcük")

Ölçek dönüştürme de genellikle daha büyük gerçekleştirilmediğinden taşımak grafik koordinatları neden olur.

Çeviri faktörleri etkilerini anlatan iki dönüştürme formüller daha önce gördüğünüzle `dx` ve `dy`:

x' = x + dx

y' = y + dy

Ölçeklendirme faktörleri `sx` ve `sy` ADDITIVE yerine çarpma şunlardır:

x' = sx · x

y' = sy · y

0 Çevir faktörlerin varsayılan değerlerdir; 1 ölçek faktörlerin varsayılan değerlerdir.

`SKCanvas` Sınıfı tanımlayan dört `Scale` yöntemleri. İlk [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/) yöntemdir yatay ve dikey aynı ölçekleme istediğinizde durumlarda faktörü için:

```csharp
public void Scale (Single s)
```

Bu olarak bilinir *isotropic* ölçeklendirme &mdash; yani aynı her iki yönde de ölçeklendirme. İsotropic ölçeklendirme nesnenin en boy oranını korur.

İkinci [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) yöntemi yatay ve dikey ölçekleme için farklı değerler belirtmenize olanak sağlar:

```csharp
public void Scale (Single sx, Single sy)
```

Bu, sonuçlanır *Eşyönsüz* ölçeklendirme.
Üçüncü [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/SkiaSharp.SKPoint/) yöntemi tek bir iki ölçeklendirme faktörlerinde birleştirir `SKPoint` değeri:

```csharp
public void Scale (SKPoint size)
```

Dördüncü `Scale` yöntemi açıklanan kısa süre içinde.

**Temel ölçek** sayfasını gösteren `Scale` yöntemi. [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) XAML dosyasını içeren iki `Slider` olanak tanıyan öğelerin yatay ve dikey ölçeklendirme etkenleri 0 ile 10 arasında seçin. [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) arka plan kodu dosyasını çağırmak için bu değerleri kullanan `Scale` yuvarlak dikdörtgen görüntüleme kesikli çizgi ile vuruş ve bazı metin sol üst köşede sığacak şekilde boyutlandırılmış önce Tuvale köşe:

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

Merak ediyor: ölçeklendirme etkenleri etkilemesi döndürülen değer `MeasureText` yöntemi `SKPaint`? Yanıt: hiç. `Scale` bir yöntemi `SKCanvas`. Herhangi bir şey yapmanız ile etkilemez bir `SKPaint` tuval üzerinde bir şey işlemek için o nesnenin kullanana kadar nesnesi.

Her şeyi sonra çizilmiş gördüğünüz gibi `Scale` artar orantılı olarak çağırın:

[![](scale-images/basicscale-small.png "Üçlü sayfasının ekran görüntüsü temel ölçek")](scale-images/basicscale-large.png#lightbox "Üçlü sayfasının ekran görüntüsü temel ölçek")

Metin köşeleri ve 10 piksel kenar boşluğu tuvalin üst ve sol kenarlarının yuvarlak dikdörtgen arasındaki yuvarlama, o satırdaki tireler uzunluğu kesikli çizgi genişliğini etken tüm aynı ölçeklendirme tabidir.

> [!IMPORTANT]
> Evrensel Windows platformu anisotropicly ölçeklendirilmiş metin düzgün işlemez.

Yatay ve dikey ekseni olan hizalı satırlar için farklı olmasını vuruşun genişliğini Eşyönsüz nedenler ölçeklendirme. (Bu da bu sayfadaki ilk görüntüden açıktır.) Ölçeklendirme faktörler tarafından etkilenir vuruşun genişliğini istemiyorsanız, 0 olarak ayarlayın ve her zaman bir piksel genişliğinde bakılmaksızın olacaktır `Scale` ayarı.

Ölçeklendirme tuvale sol üst köşesindeki göreli olur. Bu, istediğiniz tam olarak olabilir, ancak olmaması. Metin ve başka bir yere dikdörtgen tuvalde yerleştirmek istediğiniz ve kendi merkezi göre ölçeklendirmek istediğinizi varsayalım. Bu durumda dördüncü sürümünü kullanabilirsiniz [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/System.Single/System.Single/) ölçeklendirme veya merkezi belirtmek için iki ek parametreleri içeren yöntemi:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` Ve `py` parametreleri tanımlayan adlandırılır noktası *center ölçeklendirme* SkiaSharp belgeleri olarak adlandırılır, ancak bir *pivot noktası*. Bu bir sol üst köşesindeki göre ölçeklendirme tarafından etkilenmez tuvalin noktasıdır. Tüm ölçeklendirme bu merkezi göre gerçekleşir.

[ **Ortalanmış ölçek** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) sayfası bunun nasıl çalıştığını gösterir. `PaintSurface` İşleyici benzer **temel ölçek** dışında programı `margin` değeri hesaplanan metni yatay olarak ortalamak için program en iyi dikey modunda çalıştığı anlamına gelir:

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

Yuvarlak dikdörtgen sol üst köşesindeki konumlandırılmış `margin` tuvalin solundaki piksellerden ve `margin` pikseller yukarıdan. Son iki bağımsız değişken `Scale` yöntemi, bu değerleri artı genişlik ve yükseklik de genişlik ve yükseklik yuvarlak dikdörtgen olan metin için ayarlanır. Bu, tüm ölçeklendirme Bu dikdörtgeni merkezi göre olduğu anlamına gelir:

[![](scale-images/centeredscale-small.png "Üçlü sayfasının ekran görüntüsü ortalanmış ölçek")](scale-images/centeredscale-large.png#lightbox "Üçlü sayfasının ekran görüntüsü ortalanmış ölçek")

`Slider` Bu programda öğelerine sahip bir dizi &ndash;10-10. Gördüğünüz gibi (Android Center'da Ekran gibi) ölçeklendirme dikey negatif değerler ölçeklendirme merkezi üzerinden geçirir yatay ekseni etrafında ters çevirmek nesneleri neden olur. Negatif değerler (Windows ekranın sağ taraftaki olduğu gibi) ölçeklendirme yatay ölçekleme merkezi üzerinden geçirir dikey ekseni etrafında ters çevirmek nesneleri neden olur.

Dördüncü bu sürümü `Scale` gerçekte bir kısayol bir yöntemdir. Bunun değiştirerek nasıl çalıştığını görmek isteyebilirsiniz `Scale` bu kodu aşağıdakilerle yöntemi:

```csharp
canvas.Translate(-px, -py);
```

Pivot noktası koordinatları negatif bunlar.

Şimdi programı yeniden çalıştırın. Böylece merkezi tuvale sol üst köşesinde metin ve dikdörtgen gölgeye olduğunu görürsünüz. Ayrıca, bu neredeyse hiç görebilirsiniz. Şimdi program hiç ölçeklendirilmediğini çünkü kaydırıcılar Elbette çalışmıyor.

Şimdi temel ekleyin `Scale` çağrı (ölçekleme bir merkezi) *önce* , `Translate` çağırın:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Bilmiyorsanız, diğer sistemler programlama grafikler, yanlış düşünebilirsiniz, ancak bu alıştırmada değildir. Skia art arda dönüştürme çağrıları biraz farklı ne tanımanız işler.

Art arda ile `Scale` ve `Translate` çağrıları, yuvarlak dikdörtgen merkezi hala sol üst köşede olmakla birlikte, şimdi de yuvarlak dikdörtgen merkezidir tuvale sol üst köşesindeki göre ölçeklendirebilirsiniz.

Şimdi, önceki `Scale` çağrısı ekleyin başka `Translate` ortalama değerlerle çağırın:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Bu ölçeklendirilmiş sonuç geri özgün konuma taşır. Bu üç çağrıları eşdeğerdir:

```csharp
canvas.Scale(sx, sy, px, py);
```

Böylece toplam dönüştürme formülü tek tek dönüşümler araya geldiğinde:

 x' = sx · (x – px) + px

 y' = sy · (y – py) + py

Aklınızda varsayılan değerlerini `sx` ve `sy` 1. Pivot noktası (piksel, py) Bu formüller tarafından dönüştürülmüş değil kendiniz ikna kolaydır. Tuvale göre aynı konumda kalır.

Ne zaman, birleştirme `Translate` ve `Scale` çağrıları, sırası önemlidir. Varsa `Translate` sonra gelen `Scale`, çeviri Etkenler ölçeklendirme faktörler tarafından etkili bir şekilde ölçeklendirilir. Varsa `Translate` önce gelen `Scale`, çeviri Etkenler değil ölçeklenir. Bu işlem biraz daha anlaşılır hale (barındırabilir daha matematiksel) ne zaman Dönüştürme Matrislerini konusunun ilk kez sunulmuştur.

`SKPath` Sınıfı tanımlayan salt okunur [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) döndüren özelliği bir `SKRect` yolunda koordinatları kapsamını tanımlama. Örneğin, `Bounds` özelliği daha önce oluşturduğunuz hendecagram yolundan elde `Left` ve `Top` dikdörtgen özellikleridir yaklaşık – 100, `Right` ve `Bottom` özellikleri yaklaşık 100 ve `Width` ve `Height` yaklaşık 200 özelliklerdir. (Gerçek değerler biraz daha az yıldız noktalarının bir daire RADIUS 100 ile tanımlanır, ancak yalnızca üst noktası yatay veya dikey ekseni olan paralel çünkü çoğu.)

Bu bilgiler kullanılabilirliğini ölçek türetmek ve Etkenler tuvale boyutunu yoluna ölçekleme için uygun Çevir mümkün olmalıdır anlamına gelir. [ **Eşyönsüz ölçeklendirme** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) sayfa bu 11 işaret yıldızla gösterir. Bir *Eşyönsüz* ölçek anlamına gelir, yani yıldız, özgün en boy oranını korumak olmaz yatay ve dikey yönde eşit olduğunu. İlgili kod işte `PaintSurface` işleyici:

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

`pathBounds` Dikdörtgen bu kodu en üstüne yakın elde ve daha sonra genişliği ve yüksekliği tuvalin ile kullanılan `Scale` çağırın. Tek başına çağrı tarafından işlendiğinde yolu koordinatlarını ölçeklenir `DrawPath` çağrısı ancak yıldız ortalanmış tuvale sağ üst köşesinde. Aşağı ve sola gölgeye gerekir. Bu, iş `Translate` çağırın. Bu iki özelliklerini `pathBounds` yaklaşık – 100 olduğundan, yaklaşık 100 çeviri faktörlerdir. Çünkü `Translate` çağrıdır sonra `Scale` çağrısı, bunlar yıldız merkezi Kanvasın ortasına hareket edecek şekilde bu değerleri ölçeklendirme unsurlar etkili bir şekilde ölçeklenir:

[![](scale-images/anisotropicscaling-small.png "Üçlü sayfasının ekran görüntüsü Eşyönsüz ölçeklendirme")](scale-images/anisotropicscaling-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Eşyönsüz ölçeklendirme")

Bir başka yolu hakkında düşünmek `Scale` ve `Translate` çağrıdır ters sırada etkisini belirlemek için: `Translate` çağrısı tuvale sol üst köşesindeki yönlendirilmiş ancak tam olarak görünür olacak şekilde yol kaydırır. `Scale` Yöntemi sonra yapar, yıldız sol üst köşesindeki göre daha büyük.

Aslında, yıldız tuvale biraz daha büyük olduğunu görüntülenir. Vuruşun genişliğini sorunudur. `Bounds` Özelliği `SKPath` koordinatları boyutlarını kodlanmış yolu ve programın ölçeklendirmek için kullandığını gösterir. Yolun belirli vuruşun genişliğini ile işlendiğinde işlenmiş tuvale büyük yoludur.

Bu sorunu gidermek için için dengelemek gerekir. Bu programı kolay bir yaklaşım ise aşağıdaki deyimi hemen önce eklemek için `Scale` çağırın:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Bu artırır `pathBounds` dikdörtgeni dört kenara 1,5 birimlerde tarafından. Bu, yalnızca vuruş birleştirme yuvarlandığında makul bir çözümdür. Gönye uzun olamaz ve hesaplamak zordur.

Bir metin ile benzer bir teknik olarak kullanabilirsiniz **Eşyönsüz metin** sayfası gösterilmektedir. İlgili bölümü işte `PaintSurface` işleyicisinden [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) sınıfı:

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

Benzer mantığı olduğu ve döndürülen metin sınırları dikdörtgen göre sayfa boyutunu metin genişletir `MeasureText` (olduğu gerçek metin biraz daha büyük):

[![](scale-images/anisotropictext-small.png "Üçlü sayfasının ekran görüntüsü Eşyönsüz Test")](scale-images/anisotropictext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Eşyönsüz Test")

Grafik nesneleri en boy oranını korumak gerekiyorsa, isotropic ölçeklendirme kullanmak istersiniz. **Isotropic ölçeklendirme** sayfa bu için 11 işaret yıldız gösterir. Kavramsal olarak, bir grafik nesnesi isotropic ölçeklendirme sayfasının ortasında görüntüleme için adımlar şunlardır:

- Sol üst köşesindeki grafik nesnesine center çevir.
- Grafik nesne boyutlara göre bölünmüş yatay ve dikey sayfa boyutları en az temel nesne ölçeklendirin.
- Sayfanın ortasına ölçeklendirilmiş nesnesine center çevir.

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) Ters sırada yıldız görüntülemeden önce aşağıdaki adımları gerçekleştirir:

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

Kod yıldız on kez daha da görüntüler., ölçekleme azalan her zaman faktörü % 10 ve rengi mavi ve kırmızı aşamalı olarak değiştirerek:

[![](scale-images/isotropicscaling-small.png "Üçlü sayfasının ekran görüntüsü Isotropic ölçeklendirme")](scale-images/isotropicscaling-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Isotropic ölçeklendirme")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
