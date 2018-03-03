---
title: "Piksel ve CİHAZDAN bağımsız birimler"
description: "SkiaSharp koordinatları ve Xamarin.Forms koordinatları arasındaki farklar inceleyin"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 2643e06474ffe0fd60830db3f315bf525c2f84eb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="pixels-and-device-independent-units"></a>Piksel ve CİHAZDAN bağımsız birimler

_SkiaSharp koordinatları ve Xamarin.Forms koordinatları arasındaki farklar inceleyin_

Bu makalede SkiaSharp ve Xamarin.Forms kullanılan koordinat sistemi farklılıkları araştırır. İki koordinat sistemleri arasında dönüştürmek ve ayrıca belirli bir alanını doldurun grafik çizmek için bilgi elde edebilirsiniz:

![](pixels-images/screenfillexample.png "Ekran doldurur bir oval")

Xamarin.Forms içinde biraz programlama Xamarin.Forms koordinatları ve boyutları için bir fikir olabilir. İki önceki makalelerinde çizilmiş daireler için biraz küçük görünebilir.

Bu daireler *olan* Xamarin.Forms boyutları karşılaştırıldığında küçük. Xamarin.Forms temel platform tarafından oluşturulan bir CİHAZDAN bağımsız birim üzerinde koordinatları ve boyutları taban sırasında varsayılan olarak, SkiaSharp piksel birimlerinde çizer. (Xamarin.Forms koordinat sistemi hakkında daha fazla bilgi bulunabilir [bölüm 5. Boyutlarıyla ilgilenme](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) defterinin *Xamarin.Forms ile Mobile Apps oluşturma*.)

Sayfanın [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) başlıklı program **yüzeyini boyutu** üç farklı kaynaklardan görüntü yüzeyini boyutunu göstermek için SkiaSharp metin çıktısı kullanır:

- Normal Xamarin.Forms [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) ve [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) özelliklerini `SKCanvasView` nesnesi.
- [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) Özelliği `SKCanvasView` nesnesi.
- [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) Özelliği `SKImageInfo` tutarlıdır değeri `Width` ve `Height` iki önceki sayfada kullanılan özellikler.

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Sınıfı bu değerleri görüntülemek nasıl gösterir. Oluşturucu kaydeder `SKCanvasView` nesnesi içinde erişilebilmeleri adına bir alan olarak `PaintSurface` olay işleyicisi:

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

`SKCanvas` altı farklı içeren `DrawText` yöntemleri, ancak bu [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) yöntemdir basit:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Metin dizesini belirtin, burada başlamak için metin, X ve Y koordinatları ve bir `SKPaint` nesnesi. Burada metnin sol tarafında, izleme ancak konumlandırılmış X koordinatını belirtir: Y koordinatını belirtir konumunu *temel* metin. Çizgili kağıda el ile herhangi bir zamanda yazdıysanız taban hangi karakter sit ve hangi harfin alt (olanlar gibi harf g, p, soru ve y) Düzen aşağıda satırıdır.

`SKPaint` Nesne metni, yazı tipi ailesi ve metin boyutu rengini belirtmenize olanak sağlar. Varsayılan olarak, [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) özellik çok küçük bir metin telefonlar gibi yüksek çözünürlüklü cihazlarda edilir 12 ' değerine sahip. İçinde basit uygulamaları dışında bir şeye, ayrıca bazı bilgiler boyutu görüntülediğiniz metin gerekir. `SKPaint` Sınıfı tanımlayan bir [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) özelliği ve birkaç [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) yöntemleri, ancak daha az süslü ihtiyaçları için [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) özellik aralığı art arda satırlık metin için önerilen değer sağlar.

Aşağıdaki `PaintSurface` işleyicisi oluşturur bir `SKPaint` için nesne bir `TextSize` harfin alt altına Üst Çıkıntısı üstünden metnin dikey istenen yüksekliği 40 piksel. `FontSpacing` Değerine `SKPaint` nesnesi döndüren biraz 47 piksel hakkında daha büyük.

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

Yöntem (için çok az kenar boşluğu sol) 20 X koordinatını ve Y koordinatını metnin ilk satırını başlar `fontSpacing`, tam metin ilk satırının yüksekliğini görüntü yüzeyini üstünde göstermek için gerekli nedir değerinden biraz daha fazla olduğu. Her çağrı sonra `DrawText`, Y koordinatını bir veya iki artışlarla artırılır `fontSpacing`.

Aşağıda, tüm üç platformlarında çalışan program verilmiştir:

[![](pixels-images/surfacesize-small.png "Üçlü sayfasının ekran görüntüsü yüzeyini boyutu")](pixels-images/surfacesize-large.png "Üçlü sayfasının ekran görüntüsü yüzeyini boyutu")

Gördüğünüz gibi `CanvasSize` özelliği `SKCanvasView` ve `Size` özelliği `SKImageInfo` değeri piksel boyutlarını raporlamada tutarlı. `Height` Ve `Width` özelliklerini `SKCanvasView` Xamarin.Forms özellikleri ve platform tarafından tanımlanan CİHAZDAN bağımsız birim görünümünde boyutunu bildirin.

CİHAZDAN bağımsız birim başına 2 piksel sol iOS 7 simulator'da sahip, Android Nexus 5 Merkezi'ndeki birim başına 3 piksel, ve Nokia Lumia 925 sağdaki birim başına 2,25 piksel sahiptir. Kullanıcının neden basit yuvarlak iPhone aynı boyutta hakkında gösterilen önceki görünüyor ve Windows phone, ancak Android phone'da küçük olduğunu.

Tamamen CİHAZDAN bağımsız birimler çalışmaya tercih ediyorsanız, bunu ayarlayarak yapabilirsiniz `IgnorePixelScaling` özelliği `SKCanvasView` için `true`. Ancak, sonuçlar beğenebileceğiniz değil. Grafik piksel boyutu CİHAZDAN bağımsız birimler görünümü boyutuna eşit olan küçük bir aygıt yüzeyinde SkiaSharp işler. (Örneğin, SkiaSharp 360 x 512 piksel görüntü yüzeyinin Nexus 5'te kullanırsınız.) Sonra görüntü boyutu, belirgin bit eşlem jaggies kaynaklanan ölçeklendirme yaparken.

Aynı görüntü çözünürlüğü korumak için daha iyi bir çözüm iki koordinat sistemleri arasında dönüştürmek için kendi basit işlevleri yazmaktır.

Ek olarak `DrawCircle` yöntemi, `SKCanvas` Ayrıca iki tanımlar `DrawOval` elips çizme yöntemleri. Elips tek bir RADIUS yerine iki yarıçaplarını tarafından tanımlanır. Bunlar olarak bilinir *önde gelen RADIUS* ve *ikincil RADIUS*. `DrawOval` Yöntemi elips iki yarıçaplarını ile paralel olarak X ve Y eksenleri çizer. Bu kısıtlama dönüşümler veya kullanın (daha sonra ele alınması için) bir grafik yolu ile üstesinden ancak [bu `DrawOval` yöntemi](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) iki yarıçaplarını bağımsız değişken adları `rx` ve `ry` paralel olarak belirtmek için X ve Y eksenleri:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Görüntü yüzeyini doldurur elips çizme mümkün mü? **Elips doldurun** sayfasını gösteren nasıl. `PaintSurface` Olay işleyicisini [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) sınıfı çıkarır vuruşun genişliğini yarım `xRadius` ve `yRadius` tüm elips sığması için değerleri ve kendi Görüntü yüzeyini özetlemektedir:

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

[![](pixels-images/ellipsefill-small.png "Üçlü sayfasının ekran görüntüsü yüzeyini boyutu")](pixels-images/ellipsefill-large.png "Üçlü sayfasının ekran görüntüsü yüzeyini boyutu")

[Diğer `DrawOval` yöntemi](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) sahip bir [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) sol üst ve sağ alt köşedeki X ve Y koordinatları bakımından tanımlanmış dikdörtgen şeklinde bağımsız değişkeni. Oval içinde kullanmak mümkün olabilir öneren Bu dikdörtgeni doldurur **elips doldurun** sayfa şuna benzer:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Ancak, dört yüze elips anahat tüm kenarlarına tamsayıya dönüştürür. Tüm ayarlamanız gereken `SKRect` oluşturucu bağımsız değişkenleri esas `strokeWidth` bu işi sağ yapmak için:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
