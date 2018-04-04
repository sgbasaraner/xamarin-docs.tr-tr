---
title: Temel animasyon
description: SkiaSharp grafiklerinizi animasyon nasıl Bul
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 7435807e77a9a79d7fc3821675c1d959a16caa8f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="basic-animation"></a>Temel animasyon

_SkiaSharp grafiklerinizi animasyon nasıl Bul_

Neden olarak Xamarin.Forms SkiaSharp grafik animasyon uygulayabilirsiniz `PaintSurface` çok sık çağrılacak yöntem her zaman grafikleri biraz farklı çizim. Görünen Merkezi'nden genişletin eşmerkezli daireler ile bu makalenin sonraki bölümlerinde gösterilen bir animasyon şöyledir:

![](animation-images/animationexample.png "Birkaç eşmerkezli daireler görünen Merkezi'nden genişletme")

**Pulsating elips** sayfasındaki [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program canlandırır elips iki eksenlerinin böylece pulsating olması için görünür ve hatta kontrol edebilirsiniz Bu pulsation oranı:


[ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) dosya başlatır bir Xamarin.Forms `Slider` ve `Label` kaydırıcıyı geçerli değerini görüntülemek için. Bu tümleştirmek için ortak bir yoludur bir `SKCanvasView` diğer Xamarin.Forms görünümlerle:

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

Arka plan kod dosyasına başlatır bir `Stopwatch` Yüksek duyarlılık saat hizmet vermek için nesne. `OnAppearing` Kümeleri geçersiz kılma `pageIsActive` alanı `true` ve adında bir yöntemi çağırır `AnimationLoop`. `OnDisappearing` Geçersiz kılma ayarlar `pageIsActive` alanı `false`:

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

`AnimationLoop` Yöntemi başlatır `Stopwatch` ve ardından while döngülerini `pageIsActive` olan `true`. Bu sayfa etkin, ancak programın çağrısıyla döngü sonucuna çünkü kilitlenmesine neden olmaz temelde "sonsuz bir döngüde" olan `Task.Delay` ile `await` program işlevi diğer bölümleri sağlayan işleci. Bağımsız değişkeni `Task.Delay` 1/30 saniye sonra tamamlamak için neden olur. Bu, animasyonun kare hızı tanımlar.

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

`while` Döngüsü başlatan bir döngü süresi elde ederek `Slider`. Örneğin, 5 saniye cinsinden bir zaman budur. İkinci ifade değerini hesaplar `t` için *zaman*. İçin bir `cycleTime` 5, `t` artırır 0 ile 1 ile 5 saniyede. Bağımsız değişkeni `Math.Sin` ikinci deyimi 2π 5 saniyede ' 0'dan aralıkları. işlev. `Math.Sin` İşlevi 0 ile 1 arka 0 ve ardından arasında değişen bir değer döndürür &ndash;1 ve 0 5 saniyede ancak değeri 1 veya – 1 olduğunda daha yavaş değiştirmek değerlere sahip. 1 değeri, böylece her zaman pozitif değerler ve ardından değerleri aralıkları için ½ ½ 0 değeri yaklaşık 1 ve 0 olduğunda daha yavaş ancak ½-1, 2 ile ayrılmıştır eklenir. Bu depolanan `scale` alan ve `SKCanvasView` geçersiz.

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

Yöntemi, görüntü alanını boyutuna göre en çok bir RADIUS ve en fazla RADIUS üzerinde dayalı en az bir RADIUS hesaplar. `scale` Değeri animasyonlu 0 ile 1 arasında daha sonra tekrar 0, işlem için kullanan yöntemi bir `xRadius` ve `yRadius` , aralıkları arasında `minRadius` ve `maxRadius`. Bu değerler, çizme ve elips doldurmak için kullanılır:

[![](animation-images/pulsatingellipse-small.png "Üçlü sayfasının ekran görüntüsü Titreşen elips")](animation-images/pulsatingellipse-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Titreşen elips")

Dikkat `SKPaint` nesnesi oluşturulur bir `using` bloğu. Gibi birçok SkiaSharp sınıfları `SKPaint` türetilen `SKObject`, den türetilen `SKNativeObject`, hangi uygulayan [ `IDisposable` ](https://developer.xamarin.com/api/type/System.IDisposable/) arabirimi. `SKPaint` geçersiz kılmaları `Dispose` yönetilmeyen kaynakları serbest bırakmak yöntemi.

 Koyma `SKPaint` içinde bir `using` blok sağlar `Dispose` bu yönetilmeyen kaynakları serbest bloğun sonunda çağrılır. Bu yine de tarafından kullanılan bellek ortaya çıkar `SKPaint` nesne serbest .NET Atık toplayıcısının; ancak animasyon kodda, biraz daha düzenli bir şekilde bellek boşaltma içindeki etkin olması en iyisidir.

 Bu örnekte daha iyi bir çözüm iki oluşturmak olacaktır `SKPaint` nesneleri alanlar olarak bir kez ve bunları kaydedin.

Ne olduğunu **genişletme daireler** animasyon yapar. [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Sınıfı başlar dahil olmak üzere çeşitli alanları tanımlayarak bir `SKPaint` nesnesi:

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

Bu programın üzerinde Xamarin.Forms tabanlı animasyon için farklı bir yaklaşım kullandığı `Device.StartTimer`. `t` Alan 1 0'dan animasyonlu her `cycleTime` milisaniye:

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

`PaintSurface` İşleyici animasyonlu yarıçaplarını 5 eşmerkezli daireler çizer. Varsa `baseRadius` değişkeni 100 ardından olarak hesaplanır `t` 0 ile 1, 0'dan beş daireler artış 100, 100 ile 200, 300 için 200, 300-400 ve 400 için 500 yarıçaplarını animasyonlu. Çoğu daireleri için `strokeWidth` 50 ancak ilk kez daire `strokeWidth` 0 ile 50'ye canlandırır. Çoğu daireleri için mavi renkte, ancak son daire için renk mavi saydam olarak animasyonlu:

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

Görüntünün aynı arar sonucudur `t` ne zaman olarak 0'a eşit `t` 1'e eşittir ve sürekli genişleyen devam etmek için daireler gibi görünebilir:

[![](animation-images/expandingcircles-small.png "Üçlü sayfasının ekran görüntüsü genişletme daireler")](animation-images/expandingcircles-large.png#lightbox "Üçlü sayfasının ekran görüntüsü daireler genişletme")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
