---
title: Noktalar ve tireler içinde SkiaSharp
description: Bu makalede ayrıntılı olarak incelenmektedir içinde SkiaSharp noktalı ve kesikli çizgi çizme, ana nasıl keşfediyor ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: a28e4bbaae28befd91278ac5c2b9e7c9c0b522b9
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615424"
---
# <a name="dots-and-dashes-in-skiasharp"></a>Noktalar ve tireler içinde SkiaSharp

_Ana içinde SkiaSharp noktalı ve kesikli çizgi çizme, ayrıntılı olarak incelenmektedir_

SkiaSharp sağlam değildir ancak bunun yerine nokta ve tire oluşan çizgileri çizmek sağlar:

![](dots-images/dottedlinesample.png "Noktalı çizgi")

Bunu bir *yolu efektini*, bir örneği olduğu [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect) ayarlamak için sınıf [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect) özelliği `SKPaint`. Bir yol oluşturabilirsiniz tarafından tanımlanan statik oluşturma yöntemlerinden birini kullanarak etkin (veya birleştirme yol etkileri) `SKPathEffect`. (`SKPathEffect` SkiaSharp tarafından desteklenen bir altı etkileri; diğerleri bölümünde açıklanan [ **SkiaSharp etkisi**](../effects/index.md).)

Noktalı veya kesik çizgi çizmek için kullandığınız [ `SKPathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) statik yöntem. İki bağımsız değişkeni vardır: önce bir dizi budur `float` noktalar ve tireler uzunluklarının ve aralarında boşluk uzunluğunu gösteren değerleri. Bu dizinin öğeleri çift sayıda olmalıdır ve en az iki öğe olmalıdır. (Bulunabilir sıfır dizideki öğelerin ancak bu sonuçları Kesiksiz bir çizgi.) İki öğe varsa, ilk nokta veya tire uzunluğu ise ve aralık uzunluğu sonraki nokta veya tire önce saniyedir. İkiden fazla öğe varsa bu sırada oldukları: çizgi uzunluğu, aralık uzunluğu, dash uzunluğu, aralık uzunluğu ve benzeri.

Genellikle, tire ve boşluk uzunlukları darbe genişliği katları yapmak isteyebilirsiniz. Darbe genişliği 10 piksel ise, örneğin, ardından dizi {10, 10} noktalı çizgi nokta ve boşluk vuruş kalınlığı aynı uzunlukta nerede çizer.

Ancak, `StrokeCap` ayarıyla `SKPaint` nesne bu noktalar ve tireler de etkiler. Kısa bir süre içinde anlatıldığı gibi bu dizinin öğeleri üzerinde bir etkisi yoktur.

Noktalı ve kesikli satır üzerinde gösterilen **nokta ve çizgi** sayfası. [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) dosya iki başlatır `Picker` görünümleri, bir fırça darbesi ucu ve bir tire dizi seçmek için ikinci seçmenize izin vermek için:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKStrokeCap}">
                    <x:Static Member="skia:SKStrokeCap.Butt" />
                    <x:Static Member="skia:SKStrokeCap.Round" />
                    <x:Static Member="skia:SKStrokeCap.Square" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>10, 10</x:String>
                    <x:String>30, 10</x:String>
                    <x:String>10, 10, 30, 10</x:String>
                    <x:String>0, 20</x:String>
                    <x:String>20, 20</x:String>
                    <x:String>0, 20, 20, 20</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

 İlk üç öğe `dashArrayPicker` darbe genişliği 10 piksel olduğunu varsayın. {10, 10} dizidir bir noktalı çizgi için {30, 10} bir kesikli çizgiye ve {10, 10, 30 ve 10 için} için bir nokta ve tire satırıdır. (Diğer üç kısa bir süre içinde açıklanmıştır.)

[ `DotsAndDashesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) Arka plan kod dosyasını içeren `PaintSurface` olay işleyicisi ve birkaç erişmek için yardımcı yordamları `Picker` görünümler:

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
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem,
        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint); 
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string str = (string)picker.SelectedItem;
    string[] strs = str.Split(new char[] { ' ', ',' }, StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

Aşağıdaki ekran görüntülerinde, iOS ekranındaki en sol tarafındaki bir noktalı çizgi gösterir:

[![](dots-images/dotsanddashes-small.png "Üç noktalar ve tireler sayfasının ekran")](dots-images/dotsanddashes-large.png#lightbox "Üçlü sayfasının ekran görüntüsü noktalar ve tireler")

Bununla birlikte, Android ekran ayrıca {10, 10} dizisi kullanarak bir noktalı çizgi göstermek için beklenen ancak bunun yerine satır doludur. Ne oldu? Android ekran vuruş caps ayarı olduğunu sorunudur `Square`. Bu, onları boşlukları neden yarı darbe genişliği, tüm çizgilerle genişletir.

Bu sorunu geçici olarak, bir fırça darbesi ucu kullanırken almak için `Square` veya `Round`, dizideki dash uzunlukları (bazen bir tire uzunluğu 0 sonuç) Vuruş uzunluğuna göre azaltmak ve boşluk uzunlukları vuruş uzunluğuna göre artırın. Son üç dizilerde nasıl tire budur `Picker` XAML dosyasında hesaplanmıştır:

- {10, 10} olur {0, 20} için bir noktalı çizgi
- {30, 10} olur {20, 20} kesik çizgi için
- {10, 10, 30, 10} {0, 20, 20, 20} için bir noktalı ve kesikli satır haline gelir.

Cap noktalı ve kesikli satır için bir vuruş UWP ekran gösterir `Round`. `Round` Fırça darbesi ucu kalın satırları genellikle en iyi noktalar ve tireler görünümünü sağlar.

Şu ana kadar hiç ikinci parametrenin yapılmış `SKPathEffect.CreateDash` yöntemi. Bu parametre adlı `phase` ve bir uzaklık içindeki satırın başına nokta ve tire desenini gösterir. Örneğin dash dizi ise, {10, 10} ve `phase` 10'dur ve satırın nokta yerine bir boşluk ile başlar.

İlginizi `phase` animasyonda bir parametredir. **Animasyonlu Gönderilerinizi** sayfasına benzer **Archimedean Gönderilerinizi** sayfasında, hariç [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs) sınıfı canlandırır `phase` parametresini kullanma Xamarin.Forms `Device.Timer` yöntemi:


```csharp
public class AnimatedSpiralPage : ContentPage
{
    const double cycleTime = 250;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float dashPhase;

    public AnimatedSpiralPage()
    {
        Title = "Animated Spiral";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            dashPhase = (float)(10 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }
    ···  
}
```

Elbette, animasyon görmek için programı çalıştırdığı gerekir:

[![](dots-images/animatedspiral-small.png "Üçlü sayfasının ekran görüntüsü animasyonlu Gönderilerinizi")](dots-images/animatedspiral-large.png#lightbox "animasyonlu Gönderilerinizi sayfanın üç ekran görüntüsü")

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
