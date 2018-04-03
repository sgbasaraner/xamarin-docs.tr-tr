---
title: Xamarin.Forms ile tümleştirme
description: Dokunma ve Xamarin.Forms öğeleri yanıt SkiaSharp grafik oluşturma
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 3edc71977820ca618447e02caa032cf908e1aae4
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="integrating-with-xamarinforms"></a>Xamarin.Forms ile tümleştirme

_Dokunma ve Xamarin.Forms öğeleri yanıt SkiaSharp grafik oluşturma_

SkiaSharp grafik Xamarin.Forms rest çeşitli şekillerde ile tümleştirebilirsiniz. SkiaSharp tuvale ve Xamarin.Forms öğeler aynı sayfada ve hatta konumu Xamarin.Forms öğeleri SkiaSharp tuval üzerinde birleştirebilirsiniz:

![](integration-images/integrationexample.png "Kaydırıcılar renkle seçme")

Xamarin.Forms içinde etkileşimli SkiaSharp grafik oluşturmak için başka bir touch bir yaklaşımdır.
İkinci sayfasında [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programdır alınarak **dokunun geçiş doldurmak**. Basit bir daire iki yolla çizer &mdash; dolgu olmadan ve dolgu ile &mdash; yükseğe tarafından dokunun. [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Sınıfı gösterir SkiaSharp grafik kullanıcı girişine yanıt nasıl değiştirebilirsiniz.

Bu sayfa için `SKCanvasView` sınıfı örneği [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) ayrıca bir Xamarin.Forms ayarlar dosyası [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) görünümündeki:

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

`Tapped` İşleyicisi `TapGestureRecognizer` nesne yalnızca bir Boole alanı ve çağrıları değerini değiştirir [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) yöntemi `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Çağrısı `InvalidateSurface` etkili bir şekilde bir çağrı oluşturur `PaintSurface` kullanan işleyici `showFill` alanını doldurun veya daireyi dolgu değil:

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

`StrokeWidth` Özelliği fark vurgulamak için 50 olarak ayarlandı. Tüm çizgi genişliği, iç ilk çizim ardından anahattı tarafından de görebilirsiniz. Varsayılan olarak, grafik devamındaki çizilmiş rakamları `PaintSurface` olay işleyicisi soyutlamaması olanlar işleyicisi çizilmiş.

**Renk keşfedin** sayfa nasıl ayrıca SkiaSharp grafik diğer Xamarin.Forms öğelerle tümleştirebilirsiniz gösterir ve ayrıca renkleri SkiaSharp tanımlamak için iki alternatif yöntemler arasındaki farkı gösterir. Statik [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) yöntemi oluşturur bir `SKColor` değerine göre Ton Doygunluk açıklık model üzerinde:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Statik [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) yöntemi oluşturur bir `SKColor` değerine göre benzer Ton Doygunluk değer modeli:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

Her iki durumda da `h` 360 0'dan bağımsız değişkeni aralık. `s`, `l`, Ve `v` bağımsız değişkenleri aralık 0 ile 100. `a` (Alfa veya opaklık) bağımsız değişken aralıklarının 0 ile 255.

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) dosyası oluşturur iki `SKCanvasView` nesnelerini bir `StackLayout` yana birimi ile `Slider` ve `Label` kullanıcının HSL seçmesine izin ver görünümleri ve HSV renk değerleri:

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

İki `SKCanvasView` öğeleridir tek bir hücrede `Grid` ile bir `Label` sonuç RGB renk değerini görüntülemek için üstteki durduğunu.

[ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) arka plan kod dosyasına oldukça basittir. Paylaşılan `ValueChanged` üç işleyicisi `Slider` öğeleri yalnızca geçersiz kılar her ikisi de `SKCanvasView` öğeleri. `PaintSurface` İşleyiciler tarafından gösterilen renkle tuvale temizleyin `Slider` öğeleri ve ayrıca `Label` üstünde durduğunu `SKCanvasView` öğeleri:

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

Renk modellerindeki HSL ve HSV ton değeri 0-360 için aralıkları ve rengi baskın ton gösterir. Şifre geleneksel renklerini şunlardır: kırmızı, turuncu, sarı, yeşil, mavi, Indigo, mor ve kırmızı bir daire içinde geri.

HSL modelinde açıklık için 0 değeri her zaman siyah ve 100 bir değer her zaman beyaz olur. Doygunluk değerini 0 olduğunda açıklık 0 ile 100 arasında gri değerlerdir. Daha fazla renk doygunluğu artırma ekler. Doygunluk 100'dür ve açıklık 50'dir (RGB değerleri 255, 0 ve üçüncü 0 ile 255 arasında eşit başka eşit bir bileşen ile olan) saf renkleri oluşur.

Hem Doygunluk hem de değeri 100 olduğunda HSV modelinde, saf renkleri neden. Değer 0, diğer ayarlarına bakılmaksızın rengi siyah olur. Doygunluk 0 ile 100 aralıklarını 0'dan gri meydana gelir.

Ancak iki model için bir fikir almak için en iyi yolu bunlarla kendiniz denemeler yapmak için:

[![](integration-images/colorexplore-large.png "Üçlü sayfasının ekran görüntüsü renk keşfedin")](integration-images/colorexplore-small.png#lightbox "Üçlü sayfasının ekran görüntüsü renk keşfedin")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
