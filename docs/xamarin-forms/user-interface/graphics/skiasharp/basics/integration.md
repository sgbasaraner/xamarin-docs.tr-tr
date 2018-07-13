---
title: Xamarin.Forms ile tümleştirme
description: Bu makalede dokunmaya yanıt SkiaSharp grafik oluşturma ve Xamarin.Forms öğeleri açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 35aede1a541d0ff62f6a4a5b57256c389e5a8640
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997536"
---
# <a name="integrating-with-xamarinforms"></a>Xamarin.Forms ile tümleştirme

_Dokunma ve Xamarin.Forms öğeleri yanıt SkiaSharp grafikler oluşturun_

SkiaSharp grafik Xamarin.Forms geri kalanı farklı şekillerde tümleştirilebilir. SkiaSharp tuval ve Xamarin.Forms aynı sayfa üzerinde ve hatta konumu Xamarin.Forms öğeler SkiaSharp tuval üzerinde birleştirebilirsiniz:

![](integration-images/integrationexample.png "Kaydırıcıları ile bir renk seçme")

Xamarin.Forms içinde SkiaSharp etkileşimli grafik oluşturmak için başka bir yaklaşım touch ' dir.
İkinci sayfada [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programdır yetkili **dokunun, geçiş dolgu**. Basit bir daire iki şekilde çizer &mdash; dolgu olmadan ve dolgu &mdash; tarafından bir dokunma açılıp. [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Sınıfı göstermektedir SkiaSharp grafik kullanıcı girişine yanıt nasıl değiştirebilirsiniz.

Bu sayfa için `SKCanvasView` sınıfı örneği [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) ayrıca bir Xamarin.Forms ayarlar dosyasını [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) görünümü:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.TapToggleFillPage"
             Title="Tap Toggle Fill">

    <skia:SKCanvasView PaintSurface="OnCanvasViewPaintSurface">
        <skia:SKCanvasView.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnCanvasViewTapped" />
        </skia:SKCanvasView.GestureRecognizers>
    </skia:SKCanvasView>
</ContentPage>
```

Bildirim `skia` XML ad alanı bildirimi.

`Tapped` İşleyicisi `TapGestureRecognizer` nesne yalnızca Boole alanı ve aramalar değerini değiştirir [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) yöntemi `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Çağrı `InvalidateSurface` etkili bir şekilde bir çağrı oluşturur `PaintSurface` kullanan işleyici `showFill` daire dolgu yok veya doldurmak için alanı:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 50
    };
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);

    if (showFill)
    {
        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.Blue;
        canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    }
}
```

`StrokeWidth` Özellik farkı vurgulamak için 50 olarak ayarlandı. Ayrıca, ilk önce iç çizim ana hat ardından ve tüm çizgi genişliği görebilirsiniz. Varsayılan olarak, grafik ilerleyen bölümlerinde çizilen rakamları `PaintSurface` olanlar işleyicisinde çizilmiş gizlememeniz olay işleyicisi.

**Renk keşfedin** sayfası nasıl ayrıca SkiaSharp grafik diğer Xamarin.Forms öğeleri ile tümleştirebilirsiniz gösterir ve ayrıca renkler içinde SkiaSharp tanımlamak için iki alternatif yöntemler arasındaki farkı gösterir. Statik [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) yöntemi oluşturur bir `SKColor` değerine göre Hue doygunluğu açıklık model üzerinde:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Statik [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) yöntemi oluşturur bir `SKColor` değerine göre benzer Hue doygunluğu değer modeli:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

Her iki durumda da `h` 360 bağımsız değişkeni aralık 0. `s`, `l`, Ve `v` bağımsız değişken aralık 0 ile 100. `a` (Alfa veya opaklık) bağımsız değişken aralığı 0-255.

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) dosyası oluşturur iki `SKCanvasView` nesneler bir `StackLayout` yan yana ile `Slider` ve `Label` HSL seçmesini sağlayan görünümleri ve HSV renk değerleri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Basics.ColorExplorePage"
             Title="Color Explore">
    <StackLayout>
        <!-- Hue slider -->
        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference hueSlider},
                              Path=Value,
                              StringFormat='Hue = {0:F0}'}" />

        <!-- Saturation slider -->
        <Slider x:Name="saturationSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference saturationSlider},
                              Path=Value,
                              StringFormat='Saturation = {0:F0}'}" />

        <!-- Lightness slider -->
        <Slider x:Name="lightnessSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference lightnessSlider},
                              Path=Value,
                              StringFormat='Lightness = {0:F0}'}" />

        <!-- HSL canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hslCanvasView"
                               PaintSurface="OnHslCanvasViewPaintSurface" />

            <Label x:Name="hslLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>

        <!-- Value slider -->
        <Slider x:Name="valueSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference valueSlider},
                              Path=Value,
                              StringFormat='Value = {0:F0}'}" />

        <!-- HSV canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hsvCanvasView"
                               PaintSurface="OnHsvCanvasViewPaintSurface" />

            <Label x:Name="hsvLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>
    </StackLayout>
</ContentPage>
```

İki `SKCanvasView` öğeleri olan tek bir hücrede `Grid` ile bir `Label` sonuç RGB renk değeri görüntülemek için üstte yer.

[ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) arka plan kod dosyası oldukça basittir. Paylaşılan `ValueChanged` üç işleyicisi `Slider` öğeleri yalnızca geçersiz kılar hem `SKCanvasView` öğeleri. `PaintSurface` İşleyicileri tarafından belirtilen renk ile tuval Temizle `Slider` öğeleri ve ayrıca `Label` üst kısmındaki oturan `SKCanvasView` öğeleri:

```csharp
public partial class ColorExplorePage : ContentPage
{
    public ColorExplorePage()
    {
        InitializeComponent();

        hueSlider.Value = 0;
        saturationSlider.Value = 100;
        lightnessSlider.Value = 50;
        valueSlider.Value = 100;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        hslCanvasView.InvalidateSurface();
        hsvCanvasView.InvalidateSurface();
    }

    void OnHslCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsl((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)lightnessSlider.Value);
        args.Surface.Canvas.Clear(color);

        hslLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }

    void OnHsvCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsv((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)valueSlider.Value);
        args.Surface.Canvas.Clear(color);

        hsvLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }
}
```

Renk modellerdeki HSL hem HSV Hue değeri 0-360'aralıkları ve baskın ton rengini belirtir. Rainbow geleneksel renklerini şunlardır: kırmızı, turuncu, sarı, yeşil, mavi, Indigo, violet ve kırmızı bir daire içinde geri.

HSL modelinde açıklık için 0 değeri her zaman siyah ve 100 değeri her zaman beyaz. Doygunluk değeri 0 olduğunda açıklık 0 ile 100 arasında gri değerlerdir. Artan doygunluğu daha fazla renk ekler. (255, 0 ve üçüncü 0 ile 255 arasında eşit bir diğerine eşit olan bir bileşeni ile RGB değerleri olan) saf renk doygunluğu 100 ve 50 açıklık olduğunda oluşur.

Doygunluk ve değeri 100 olduğunda HSV modelinde, saf renkleri neden. Değer 0, diğer ayarlarından bağımsız olarak olduğunda rengi siyah olur. Gri doygunluğu 0 ile 100 değer aralıklarına 0 olduğunda oluşur.

Ancak bir genel görünüm iki model için en iyi yolu bunları kendiniz denemek için:

[![](integration-images/colorexplore-large.png "Üçlü sayfasının ekran görüntüsü renk keşfedin")](integration-images/colorexplore-small.png#lightbox "Üçlü sayfasının ekran görüntüsü renk keşfedin")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
