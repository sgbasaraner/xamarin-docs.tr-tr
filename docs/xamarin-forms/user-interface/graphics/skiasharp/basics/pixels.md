---
title: Piksel ve CİHAZDAN bağımsız birimler
description: Bu makalede SkiaSharp koordinatları ve Xamarin.Forms koordinatları arasındaki farklar keşfediyor ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: b0655934b0389b06dca5ef6bab3badc0023400b4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997393"
---
# <a name="pixels-and-device-independent-units"></a>Piksel ve CİHAZDAN bağımsız birimler

_SkiaSharp koordinatları ve Xamarin.Forms koordinatları arasındaki farklar keşfedin_

Bu makalede farklar SkiaSharp ve Xamarin.Forms kullanılan koordinat sisteminde keşfediyor. Belirli bir alanı dolduran bir grafik de çizin ve iki koordinat sistemi arasında dönüştürmek için bilgi edinebilirsiniz:

![](pixels-images/screenfillexample.png "Elips ekran doldurur.")

Xamarin.Forms içinde bir süredir programlama Xamarin.Forms koordinatları ve boyutları için bir sahip olabilir. Çizilen iki önceki makaleler, daire, biraz küçük görünebilir.

Bu daireler *olan* Xamarin.Forms boyutları karşılaştırıldığında daha küçük. Xamarin.Forms temel platform tarafından oluşturulan bir CİHAZDAN bağımsız birim üzerinde koordinatları ve boyutları tabanları sırasında varsayılan olarak, piksel birimlerinde SkiaSharp çizer. (Xamarin.Forms koordinat sistemi hakkında daha fazla bilgi bulunabilir [bölümün 5. Boyutlarla ilgilenme](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) kitabın *Xamarin.Forms ile Mobile Apps oluşturma*.)

Sayfanın [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) başlıklı program **yüzey boyutunu** SkiaSharp metin çıktısı uzaklaştırabilir üç farklı kaynaklardan boyutunu göstermek için kullanır:

- Normal Xamarin.Forms [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) ve [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) özelliklerini `SKCanvasView` nesne.
- [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) Özelliği `SKCanvasView` nesne.
- [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) Özelliği `SKImageInfo` ile tutarlı olan değerini `Width` ve `Height` iki önceki sayfalarında kullanılan özellik.

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Sınıfı bu değerleri görüntülemek nasıl gösterir. Oluşturucu kaydeder `SKCanvasView` nesnesi içinde erişilebilir için bir alan olarak `PaintSurface` olay işleyicisi:

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas` altı farklı içerir `DrawText` yöntemleri, ancak bu [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) yöntemdir basit:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Metin dizesi belirtirseniz, başlamak için metin olduğu X ve Y koordinatları ve `SKPaint` nesne. Burada metnin sol tarafında, ancak izleme konumlandırılmış X koordinatını belirtir: Y koordinatı belirtir konumunu *temel* metin. El ile hiç olmadığı kadar çizgili kağıda yazdıysanız, temel hangi karakter sit ve hangi çıkıntılarını (örneğin harf g, p, soru ve y) Düzen aşağıda satırıdır.

`SKPaint` Nesne, metnin yazı tipi ailesi ve metin boyutunu rengi belirtmenize olanak sağlar. Varsayılan olarak, [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) özellik çok küçük bir metin telefon gibi yüksek çözünürlüklü cihazlarda sonuçlanır 12 ' değerine sahip. İçinde dışında hiçbir şeyde basit uygulamalar, ayrıca görüntülediğiniz metin boyutu bazı bilgiler gerekir. `SKPaint` Sınıfı tanımlayan bir [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) özelliği ve birkaç [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) yöntemleri, ancak daha az başvurmaktan ihtiyaçları için [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) özelliği, önerilen değer aralığı için metin ardışık satırları sağlar.

Aşağıdaki `PaintSurface` işleyicisi oluşturur bir `SKPaint` nesnesi bir `TextSize` çıkıntılarını altına en üst çıkıntısı metnin dikey istenen yüksekliği olan 40 piksel. `FontSpacing` Değerine `SKPaint` nesnesi döndüren biraz 47 piksel hakkında daha büyük.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

Yöntemin ilk 20 (sol çok az bir boşluğu için) bir X koordinatını ve Y koordinatını içeren metin satırının başlar `fontSpacing`, tam metin ilk satırının yüksekliğini uzaklaştırabilir üstünde görüntülenecek gerekli olanla değerinden biraz daha fazla olduğu. Her çağrı sonra `DrawText`, Y koordinatı bir veya iki artışlarla artırılır `fontSpacing`.

Üç tüm platformlarda çalışan bir program şöyledir:

[![](pixels-images/surfacesize-small.png "Üçlü sayfasının ekran görüntüsü yüzey boyutunu")](pixels-images/surfacesize-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Surface boyutu")

Gördüğünüz gibi `CanvasSize` özelliği `SKCanvasView` ve `Size` özelliği `SKImageInfo` piksel boyutlarını raporlamada tutarlı değeri. `Height` Ve `Width` özelliklerini `SKCanvasView` Xamarin.Forms özellikleri ve rapor görünümü'nde platform tarafından tanımlanan CİHAZDAN bağımsız birimler boyutu.

İOS 7 simülatör soldaki CİHAZDAN bağımsız birim başına 2 piksel, ve birim başına 3 piksel merkezinde Android Nexus 5 sahiptir. İşte bu nedenle daha önce gösterilen basit bir daire farklı platformlarda farklı boyutları vardır.

Tamamen CİHAZDAN bağımsız birimler çalışacak şekilde tercih ediyorsanız, ayarlayarak bunu yapabilirsiniz `IgnorePixelScaling` özelliği `SKCanvasView` için `true`. Ancak, sonuçları beğenebileceğiniz değil. SkiaSharp CİHAZDAN bağımsız birimler eşit görünümün boyutunu piksel boyutlu daha küçük bir cihaz yüzeyinde grafik çizer. (Örneğin, SkiaSharp 360 x 512 piksel görünen yüzeyinin Nexus 5'te kullanmanız gerekir.) Ardından bu görüntüyü belirgin bir bit eşlem jaggies kaynaklanan boyutunda ölçeklendirilebilir.

Aynı görüntü çözünürlüğünü korumak için daha iyi bir çözüm iki koordinat sistemi arasında dönüştürmek için kendi basit işlevler yazmaktır.

Ek olarak `DrawCircle` yöntemi `SKCanvas` Ayrıca iki tanımlar `DrawOval` elips çizin yöntemleri. Bir elips tek bir RADIUS yerine iki yarıçaplarını tarafından tanımlanır. Bunlar olarak bilinen *ana RADIUS* ve *küçük RADIUS*. `DrawOval` Yöntemi iki yarıçaplarını ile bir elipsin X ve Y eksenleri için paralel çizer. Bu kısıtlama, dönüşümler veya (daha sonra ele alınacak) bir grafik yolu, kullanımı ile üstesinden gelebilir ancak [bu `DrawOval` yöntemi](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) iki yarıçaplarını bağımsız değişken adları `rx` ve `ry` paralel olarak olduğunu belirtmek için X ve Y eksenleri:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Bu, uzaklaştırabilir dolduran bir elips Çiz mümkün mü? **Elipsin dolgu** sayfasını gösterir nasıl. `PaintSurface` Olay işleyicisinde [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) sınıfı çıkarır den yarı darbe genişliği `xRadius` ve `yRadius` tüm elips uyacak şekilde değerleri ve uzaklaştırabilir içinde özetlemektedir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

Burada üç platformlarda çalıştığı:

[![](pixels-images/ellipsefill-small.png "Üçlü sayfasının ekran görüntüsü yüzey boyutunu")](pixels-images/ellipsefill-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Surface boyutu")

[Diğer `DrawOval` yöntemi](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) sahip bir [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) dikdörtgenin sol üst ve sağ alt köşedeki X ve Y koordinatları bakımından tanımlanmış olan bağımsız değişkeni. Oval kullanmayı mümkün olabilir önerir, dikdörtgen doldurur **elipsin dolgu** sayfa şöyle:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Ancak, dört tarafında elipsin anahat kenarlarına keser. Tüm ayarlamanız gereken `SKRect` oluşturucu bağımsız değişkenleri temel alan `strokeWidth` doğru Bunun çalışmasını sağlamak için:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
