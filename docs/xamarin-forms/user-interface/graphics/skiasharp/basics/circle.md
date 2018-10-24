---
title: Basit bir daire içinde SkiaSharp çizimi
description: Bu makalede, temel SkiaSharp çizimi tuvaller ve bir boyama nesneleri Xamarin.Forms uygulamalarında da dahil olmak üzere, açıklanır ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 32fa126d1990839a98ce03cdbfa245a2df97671d
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615229"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>Basit bir daire içinde SkiaSharp çizimi

_SkiaSharp çizimi tuvaller dahil olmak üzere, temel bilgileri öğrenmek ve nesneleri boyama_

Bu makalede Xamarin.Forms içinde SkiaSharp oluşturma da dahil olmak üzere, kullanarak grafik çizim kavramlar tanıtılır bir `SKCanvasView` işleme grafik barındırmak için nesne `PaintSurface` olay ve kullanarak bir `SKPaint` nesnesi, renk ve diğer çizim belirtmek için öznitelikleri.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program bu makale serisi, SkiaSharp için tüm örnek kodunu içerir. İlk sayfa hak kazanan **Basit Daire** ve sayfa sınıfını çağıran [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Bu kod, bir RADIUS 100 piksel ile sayfasının ortasında bir daire çizin işlemi gösterilmektedir. Anahat dairenin kırmızı ve mavi daireye iç.

![](circle-images/circleexample.png "Kırmızı renkle mavi daire")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Sayfa sınıf türetilir `ContentPage` ve iki tane `using` SkiaSharp ad alanları için yönergeler:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Aşağıdaki sınıfının oluşturucusu, oluşturur bir [ `SKCanvasView` ](xref:SkiaSharp.Views.Forms.SKCanvasView) nesne, bir işleyici ekler [ `PaintSurface` ](xref:SkiaSharp.Views.Forms.SKCanvasView.PaintSurface) olay ve kümelerini `SKCanvasView` nesnesi olarak sayfasının içeriği:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` Sayfanın tüm içerik alanı kaplar. Alternatif olarak birleştirebilirsiniz bir `SKCanvasView` diğer Xamarin.Forms ile `View` türevleri gibi diğer örnekler göreceksiniz.

`PaintSurface` Olay işleyicisidir tüm çizim yaptığınız. Bu yöntem, program çalışırken birden çok kez çağrılabilir, tüm bilgileri yeniden oluşturmak gerekli korumanız gerekir böylece grafik görüntüler:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs) Olay eşlik eden bir nesne iki özelliği vardır:

- [`Info`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info) türü [`SKImageInfo`](xref:SkiaSharp.SKImageInfo)
- [`Surface`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface) türü [`SKSurface`](xref:SkiaSharp.SKSurface)

`SKImageInfo` Yapısı içeren çizim yüzeyi bilgilerini en önemlisi, genişlik ve yükseklik piksel cinsinden. `SKSurface` Nesne çizim yüzeyi temsil eder. Bu programda, çizim yüzeyindeki bir görüntü olduğu ancak diğer programları bir `SKSurface` nesne üzerine çizileceği SkiaSharp kullanan bir bit eşlem de temsil edebilir.

En önemli özelliği `SKSurface` olduğu [ `Canvas` ](xref:SkiaSharp.SKSurface.Canvas) türü [ `SKCanvas` ](xref:SkiaSharp.SKCanvas). Grafik gerçek çizim gerçekleştirmek için kullandığınız bağlam çizim sınıftır. `SKCanvas` Nesne, grafik dönüşümler ve kırpma içeren bir grafik durumu kapsüller.

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

[ `Clear` ](xref:SkiaSharp.SKCanvas.Clear) Yöntemi temizler tuval ile saydam rengi. Aşırı bir arka plan rengi tuvalin belirtmenize olanak sağlar.

Burada mavi ile doldurulmuş kırmızı bir daire çizin olmaktır. Bu belirli bir grafik görüntüsü iki farklı renkler içerdiğinden iş iki adımda yapılması gerekir. İlk adım, dairenin anahatlarını çizin sağlamaktır. Renk ve çizginin diğer özelliklerini belirtmek için oluşturma ve başlatma bir [ `SKPaint` ](xref:SkiaSharp.SKPaint) nesnesi:

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

[ `Style` ](xref:SkiaSharp.SKPaint.Style) Özelliği gösterir, istediğiniz *vuruş* bir satır (Bu durumda dairenin anahat) yerine *dolgu* iç. Üç üyelerinin [ `SKPaintStyle` ](xref:SkiaSharp.SKPaintStyle) sabit listesi şu şekildedir:

- [`Fill`](xref:SkiaSharp.SKPaintStyle.Fill)
- [`Stroke`](xref:SkiaSharp.SKPaintStyle.Stroke)
- [`StrokeAndFill`](xref:SkiaSharp.SKPaintStyle.StrokeAndFill)

Varsayılan, `Fill` değeridir. Satır vuruş yapmak ve iç aynı rengi ile doldurmak için üçüncü bir seçenek kullanın.

Ayarlama [ `Color` ](xref:SkiaSharp.SKPaint.Color) türünde bir değer özelliğini [ `SKColor` ](xref:SkiaSharp.SKColor). Almak için tek yönlü bir `SKColor` değerdir bir Xamarin.Forms dönüştürerek `Color` değerini bir `SKColor` genişletme yöntemini kullanarak değer [ `ToSKColor` ](SkiaSharp.Views.Forms.Extensions.ToSKColor*). [ `Extensions` ](xref:SkiaSharp.Views.Forms.Extensions) Sınıfını `SkiaSharp.Views.Forms` ad alanı Xamarin.Forms ve SkiaSharp değerleri arasında dönüştürme diğer yöntemleri içerir.

[ `StrokeWidth` ](xref:SkiaSharp.SKPaint.StrokeWidth) Özelliği çizgi kalınlığı gösterir. Burada, 25 piksel olarak ayarlanır.

Kullanarak `SKPaint` daire çizmek için nesne:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Koordinatları uzaklaştırabilir sol üst köşesine göre belirtilir. X artış sağa ve gitme Y koordinatları artış düzenler. Grafik ilgili tartışmasındaki, genellikle bir noktası belirtmek için matematiksel bir gösterimi (x, y) kullanılır. (0, 0) noktası uzaklaştırabilir sol üst köşesindeki olduğu ve genellikle çağrılır *kaynak*.

İlk iki bağımsız değişkenleri `DrawCircle` Orta dairenin X ve Y koordinatları gösterir. Bu orta dairenin uzaklaştırabilir Merkezi'nde koymak için yarı genişliğinde ve uzaklaştırabilir yüksekliği için atanır. Dairenin RADIUS üçüncü bağımsız değişken belirtir ve son bağımsız değişken `SKPaint` nesne.

İç dairenin doldurmak için iki özelliklerini değiştirebilir `SKPaint` nesne ve çağrı `DrawCircle` yeniden. Bu kod almanın alternatif bir yolu da gösterir. bir `SKColor` birçok alanlarını birinden değer [ `SKColors` ](xref:SkiaSharp.SKColors) yapısı:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
Bu kez, `DrawCircle` çağrı doldurur yeni özelliklerini kullanarak daire `SKPaint` nesne.

İOS, Android ve evrensel Windows platformu üzerinde çalışan bir program şöyledir:

[![](circle-images/simplecircle-small.png "Üçlü sayfasının ekran görüntüsü Basit Daire")](circle-images/simplecircle-large.png#lightbox "Üçlü sayfasının ekran görüntüsü basit daire")

Programı kendiniz çalıştırırken, telefon veya grafiğin nasıl çizilme görmek için simülatör yan çevirerek kapatabilirsiniz. Grafik çizilmesini için her durumda `PaintSurface` olay işleyici tekrar çağrılır.

Gradyan veya döşeme bit eşlem grafik nesneleriyle renk mümkündür. Bu seçenekler hakkında bölümünde ele alınmıştır [ **SkiaSharp gölgelendiricileri**](../effects/shaders/index.md).

Bir `SKPaint` grafik çizim özellikleri koleksiyonu biraz daha fazla nesne. Bu hafif nesnelerdir. Yeniden kullanabileceğiniz `SKPaint` nesneleri bu program yok veya birden çok oluşturabilirsiniz `SKPaint` özellikleri çizim çeşitli birleşimlerini nesneleri. Oluşturabilir ve bu nesneleri dışında `PaintSurface` olay işleyicisi ve kaydedebilir bunları alanları olarak sayfa Sınıfınız içinde.

> [!NOTE]
> `SKPaint` Sınıfı tanımlayan bir [ `IsAntialias` ](xref:SkiaSharp.SKPaint.IsAntialias) , grafiklerin görüntülenmesinde düzgünleştirme etkinleştirmek için. Düzgünleştirme muhtemelen bu özellik ayarlanacak güncelleştirdiğinden görsel olarak daha yumuşak kenarlar genellikle sonuçları `true` çoğunda, `SKPaint` nesneleri. Kolaylık olması amacıyla, bu özellik, _değil_ örnek sayfaların çoğu ayarlayın.

Dairenin Anahat kalınlığı 25 piksel olarak belirtilmiş olsa da &mdash; veya çeyrek daire yüzdesi &mdash; ince olarak görünür ve söz konusu iyi bir sebep: satırının yarım genişlikte tarafından mavi daire engellenmesidir. Bağımsız değişkenleri `DrawCircle` yöntemi soyut geometrik koordinatlarını bir daire tanımlayın. Mavi iç bu boyuta yakın piksel boyutlu, ancak 25-piksel genişliğinde anahat geometrik daire yayılan &mdash; yarısı iç ve dış yarısı.

Sonraki örnekte [Xamarin.Forms ile tümleştirme](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) makalesinde gösterilmiştir bu görsel olarak.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
