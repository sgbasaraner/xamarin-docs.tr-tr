---
title: Basit bir daire çizme
description: SkiaSharp çizim tuvalini ve boyama dahil olmak üzere, temellerini öğrenin
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 402470c3a27ba4327afa6e77336d60748abad436
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="drawing-a-simple-circle"></a>Basit bir daire çizme

_SkiaSharp çizim tuvalini ve boyama dahil olmak üzere, temellerini öğrenin_

Bu makalede grafik oluşturma dahil SkiaSharp kullanarak Xamarin.Forms içinde çizim kavramlar tanıtılır bir `SKCanvasView` işleme grafikler, ana bilgisayar nesnesine `PaintSurface` olay ve kullanarak bir `SKPaint` renk ve diğer çizim belirtmek için nesne öznitelikler.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program SkiaSharp makaleleri bu dizisi için tüm örnek kodunu içerir. İlk sayfa alınarak **Basit Daire** ve sayfa sınıfının çağırır [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Bu kodu nasıl 100 piksel RADIUS ile sayfasının ortasında bir daire çizileceğini gösterir. Anahat dairenin kırmızı ve dairenin iç mavi.

![](circle-images/circleexample.png "Kırmızı renkle mavi bir daire")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Sayfa sınıfının türer `ContentPage` ve iki içeren `using` yönergeleri SkiaSharp ad alanları için:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Sınıfının aşağıdaki oluşturucuyu yaratır bir [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) nesne, bir işleyici iliştirir [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) olay ve kümelerini `SKCanvasView` nesnesi sayfasının içeriği olarak:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` Sayfanın tüm içerik alanı kaplar. Alternatif olarak birleştirebilirsiniz bir `SKCanvasView` diğer Xamarin.Forms ile `View` türevleri, siz başka örneklerde göreceksiniz.

`PaintSurface` Olay işleyicisidir burada tüm çizim yapın. Programınız çalışırken bu yöntem genellikle birden çok kez çağrılır, tüm bilgileri yeniden oluşturmak gerekli saklamalısınız şekilde grafik görüntüler:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) Olay eşlik nesnenin iki özellik vardır:

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) türü [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) türü [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

`SKImageInfo` Yapısı içeren çizim yüzeyini hakkında bilgi en önemlisi, genişlik ve yükseklik piksel cinsinden. `SKSurface` Nesnesi çizim yüzeyini temsil eder. Bu programda çizim yüzeyini bir görüntü değil ancak diğer programları bir `SKSurface` nesnesi de SkiaSharp Çiz için kullandığınız bir bit eşlemi temsil eder.

En önemli özelliği, `SKSurface` olan [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) türü [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/). Grafik gerçek çizim gerçekleştirmek için kullandığınız bağlam çizim sınıftır. `SKCanvas` Nesne, grafik dönüşümler ve kırpma içeren bir grafik durumu yalıtır.

Tipik bir başlangıcı İşte bir `PaintSurface` olay işleyicisi:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

[ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Yöntem saydam renkle tuvale temizler. Bir aşırı tuvale arka plan rengini belirtmenize olanak sağlar.

Burada mavi ile doldurulmuş kırmızı bir daire çizmek için belirtilir. Bu belirli bir grafik resim iki farklı renk içerdiğinden iş iki adımda yapılmalıdır. Anahat dairenin çizmek için ilk adımdır bakın. Renk ve diğer karakteristiğini satırının belirtmek için oluştur ve Başlat bir [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) nesnesi:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

[ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) Özelliği gösterir, istediğiniz *vuruş* bir satır (Bu durumda dairenin anahat) yerine *dolgu* iç. Üç üyeleri [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) numaralandırma aşağıdaki gibidir:

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

Varsayılan, `Fill` değeridir. Üçüncü seçenek satır vuruş yapmak ve iç aynı renkle doldurmak için kullanın.

Ayarlama [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) türünde bir değer özelliğine [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/). Tek yönlü almak için bir `SKColor` değeri olan bir Xamarin.Forms dönüştürerek `Color` değeri bir `SKColor` genişletme yöntemi kullanarak değer [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/). [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) Sınıfını `SkiaSharp.Views.Forms` ad alanı Xamarin.Forms ve SkiaSharp değerleri arasında dönüştürme diğer yöntemleri içerir.

[ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) Özelliği çizgi kalınlığını belirtir. Burada, 25 piksel olarak ayarlanır.

Kullanan `SKPaint` daire çizmek için nesnesi:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Koordinatları görüntü yüzeyini sol üst köşesindeki göre belirtilir. X sağa artış ve Y koordinatları artış giderek düzenler. Grafikler hakkında tartışma, genellikle bir noktası belirtmek için matematiksel bir gösterimi (x, y) kullanılır. (0, 0) noktası görüntü yüzeyini sol üst köşesindeki ve genellikle adlı *kaynak*.

İlk iki bağımsız değişkenleri `DrawCircle` Dairenin merkezinin X ve Y koordinatları belirtir. Bunlar, Yarı genişlik ve yükseklik görüntü yüzeyini Merkezi'nde dairenin merkezi yerleştirilecek görüntü yüzeyinin atanır. Üçüncü bağımsız değişken dairenin yarıçapını belirler ve son bağımsız değişken `SKPaint` nesnesi.

Daireye iç doldurmak için iki özelliklerini değiştirebilir `SKPaint` nesne ve çağrı `DrawCircle` yeniden. Bu kod ayrıca almak için alternatif bir yol gösterir bir `SKColor` birçok alanlarını birinden değeri [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) yapısı:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
Bu süre, `DrawCircle` çağrısı doldurur yeni özelliklerini kullanarak daire `SKPaint` nesnesi.

İOS, Android ve evrensel Windows platformu üzerinde çalışan program şöyledir:

[![](circle-images/simplecircle-small.png "Üçlü sayfasının ekran görüntüsü Basit Daire")](circle-images/simplecircle-large.png#lightbox "Üçlü sayfasının ekran görüntüsü basit daire")

Program kendiniz çalıştırırken, telefon veya grafiği nasıl çizilme görmek için yanları simulator kapatabilirsiniz. Grafiğin çizilmesi için gereksinim duyduğu her zaman `PaintSurface` olay işleyici tekrar çağrılır.

Bir `SKPaint` nesnesidir çizim özellikleri grafik koleksiyonu biraz daha fazla. Bu çok hafif nesnelerdir. Yeniden kullanabilir `SKPaint` nesneleri bu program, ya da birden çok oluşturabileceğiniz gibi `SKPaint` nesnelerin özelliklerini çizim çeşitli birleşimler için. Oluşturma ve bu nesnelerin dışında başlatma `PaintSurface` olay işleyicisi ve kaydedebilir bunları alanlar olarak sayfa sınıfınızda.

Dairenin kenarlık genişliğini 25 piksel olarak belirtilmiş olsa da &mdash; veya Çeyrek dairenin RADIUS &mdash; ince gibi görünüyor ve söz konusu iyi bir neden yoktur: mavi bir daire satırının yarı genişlikli getirilmemeli. Bağımsız değişkenleri `DrawCircle` yöntemi bir daire soyut geometrik koordinatlarını tanımlayın. Bu boyutuna yakın piksel mavi iç boyuta sahip olmadığından, ancak 25 piksel genişliğinde anahat geometrik daire yayılan &mdash; yarısı iç ve dış yarısı.

Sonraki örnekte [Xamarin.Forms ile tümleştirme](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) makalede gösterilir bu görsel olarak.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
