---
title: Temel SkiaSharp animasyonu
description: Bu makalede Xamarin.Forms uygulamaları SkiaSharp grafiklerinizi animasyon açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 393716e17b042224f2b0bae8c526132489af26c6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994825"
---
# <a name="basic-animation-in-skiasharp"></a>Temel SkiaSharp animasyonu

_SkiaSharp grafiklerinizi animasyon öğrenin_

Xamarin.Forms içinde SkiaSharp grafik neden olarak canlandırabilirsiniz `PaintSurface` çok sık çağrılması yöntemi her zaman biraz daha farklı grafik çizim. Animasyonun görünüşte Merkezi'nden genişletin Eşmerkezli daire ile bu makalenin sonraki bölümlerinde gösterilen şekildedir:

![](animation-images/animationexample.png "Görünüşte Merkezi'nden genişleterek çeşitli Eşmerkezli daire")

**Pulsating elipsin** sayfasını [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program pulsating olması için görünmesi bir elipsin iki eksen canlandırır ve hatta denetleyebilirsiniz Bu pulsation oranı:


[ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) dosya örnekleyen bir Xamarin.Forms `Slider` ve `Label` slider'ın geçerli değerini görüntülemek için. Bu tümleştirmek için ortak bir yoludur bir `SKCanvasView` diğer Xamarin.Forms görünümler:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Arka plan kod dosyası örnekleyen bir `Stopwatch` yüksek duyarlıklı saat görev yapacak bir nesne. `OnAppearing` Ayarlar geçersiz kılma `pageIsActive` alanı `true` ve adında bir yöntemi çağıran `AnimationLoop`. `OnDisappearing` Geçersiz kılma ayarlar `pageIsActive` alanı `false`:

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

`AnimationLoop` Yöntemi başlatıldıktan `Stopwatch` ve ardından while döngülerini `pageIsActive` olduğu `true`. Bu temelde "sonsuz bir döngü" sayfasında etkindir, ancak program çağrısı ile döngü sonucuna çünkü kilitlenmesine neden olmaz, `Task.Delay` ile `await` diğer bölümlerinde program işlevi sağlayan işleci. Bağımsız değişkeni `Task.Delay` 1/30 saniye sonra tamamlanmasına neden. Bu, animasyonun kare hızı tanımlar.

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

`while` Döngüsü başlatan bir döngü süreden elde ederek `Slider`. Bu örnek, 5 saniye cinsinden süredir. İkinci deyim değerini hesaplar `t` için *zaman*. İçin bir `cycleTime` 5, `t` artırır 0 ile 1-5 saniyede. Bağımsız değişkeni `Math.Sin` ikinci deyim 2π 5 saniyede ' 0'dan aralıklarında. işlevi. `Math.Sin` İşlevi, 0, 0 ve ardından 1 arka arasında değişen bir değer döndürür &ndash;1-0 5 saniyede bir, ancak değeri 1 veya – 1 yakın olduğunda daha yavaş değişen değerlere sahip. 1 değeri her zaman pozitif değerler ve daha sonra değerleri aralıkları için ½ ½ 0 değeri yaklaşık 1 ve 0 olduğunda daha yavaş ancak bir ½-1, 2 ile bölünür eklenir. Bu depolanan `scale` alan ve `SKCanvasView` geçersiz kılınır.

`PaintSurface` Yöntemi kullanan bu `scale` elipsin iki eksen hesaplamak için değer:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

Yöntem ekran alanı boyutuna göre en fazla bir RADIUS ve en fazla RADIUS'u göre en az bir RADIUS hesaplar. `scale` Değeri animasyon 0 ile 1 arasındaki ve tekrar 0, yöntem, işlem kullanması için bir `xRadius` ve `yRadius` aralıkları arasında `minRadius` ve `maxRadius`. Bu değerler, çizme ve elips doldurmak için kullanılır:

[![](animation-images/pulsatingellipse-small.png "Üçlü sayfasının ekran görüntüsü Titreşen elipsin")](animation-images/pulsatingellipse-large.png#lightbox "Titreşen elipsin sayfanın üç ekran görüntüsü")

Dikkat `SKPaint` nesnesi oluşturulur bir `using` blok. Gibi birçok SkiaSharp sınıfları `SKPaint` türetildiği `SKObject`, öğesinden türetildiğini `SKNativeObject`, uygulayan [ `IDisposable` ](xref:System.IDisposable) arabirimi. `SKPaint` geçersiz kılmalar `Dispose` yöntemi yönetilmeyen kaynaklar serbest bırakılacaksa.

 Yerleştirme `SKPaint` içinde bir `using` blok sağlar `Dispose` bu yönetilmeyen kaynakları serbest bırakacak bloğunun sonunda çağrılır. Tarafından kullanılan bellek, yine de böyle `SKPaint` nesneyi serbest .NET atık toplayıcı tarafından ancak animasyon kod biraz daha düzenli bir şekilde bellek boşaltma proaktif en iyisidir.

 Bu örnekte daha iyi bir çözüm iki oluşturmak olacaktır `SKPaint` alanları olarak nesneleri, bir kez ve bunları kaydedin.

Budur **genişletme daireler** animasyon yapar. [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Sınıfı başlar dahil olmak üzere çeşitli alanları tanımlayarak bir `SKPaint` nesnesi:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

Bu programın üzerinde Xamarin.Forms bağlı animasyon için farklı bir yaklaşım kullanan `Device.StartTimer`. `t` Alan 1-0'dan animasyonlu her `cycleTime` milisaniye:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`PaintSurface` İşleyici animasyonlu yarıçaplarını 5 eşmerkezli daireler çizer. Varsa `baseRadius` değişkeni 100 ardından şöyle hesaplanır `t` 0 ile 1, 0'dan beş daireler artış 100, 100, 200, ila 200 300, 300-400 ve 500 400 köşelerinin yarıçaplarını animasyon görünür. Çoğu dairelerin `strokeWidth` ilk 50 ancak Circle `strokeWidth` 0 ile 50 canlandırın. Çoğu dairelerin rengini Mavi, ancak son dairenin rengi maviye için saydam bir animasyon görünür:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

Sonuç görüntüsü aynı görünür, `t` olarak ne zaman 0'a eşit `t` eşittir 1 ve daireler görünüyor sonsuza kadar genişletmeye devam edin:

[![](animation-images/expandingcircles-small.png "Üçlü sayfasının ekran görüntüsü genişletme daireler")](animation-images/expandingcircles-large.png#lightbox "Üçlü sayfasının ekran görüntüsü daireler genişletme")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
