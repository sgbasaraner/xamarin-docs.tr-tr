---
title: Nokta ve tire
description: İçinde SkiaSharp noktalı ve kesikli çizgi çizme, ayrıntılı olarak incelenmektedir Yöneticisi
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 274c8e9a79fa3fadff14f1174d86aad04d902b05
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="dots-and-dashes"></a>Nokta ve tire

_İçinde SkiaSharp noktalı ve kesikli çizgi çizme, ayrıntılı olarak incelenmektedir Yöneticisi_

SkiaSharp sağlam değildir ancak bunun yerine nokta ve tire oluşurlar çizgiler çizme olanak sağlar:

![](dots-images/dottedlinesample.png "Noktalı çizgi")

Bunu yapmak bir *yolu etkisi*, örneği olduğu [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) ayarlamak için sınıf [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) özelliği `SKPaint`. Bir yol oluşturabilirsiniz statik kullanarak etkin (veya birleştirme yolu etkileri) `Create` tarafından tanımlanan yöntemler `SKPathEffect`.

Noktalı veya kesik çizgi çizmek için kullandığınız [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) statik yöntemi. İki bağımsız değişkeni vardır: ilk önce bir dizi budur `float` uzunlukları nokta ve kısa çizgi ve boşluk aralarında uzunluğu gösteren değerler. Bu dizi öğeleri çift sayıda olmalıdır ve en az iki öğe olmalıdır. (Olabilir dizideki sıfır öğelerin ancak bu Kesiksiz bir çizgi sonuçlanır.) İki öğe varsa, ilk uzunluğunda bir nokta veya çizgi ve boşluk uzunluğu sonraki nokta veya çizgi önce saniyedir. İkiden fazla öğe varsa bu sırada oldukları: tire uzunluğu, aralık uzunluğu, tire uzunluğu, aralık uzunluğu ve benzeri.

Genellikle, tire ve boşluk uzunluklarını vuruşun genişliğini birden fazla olmak istersiniz. Vuruşun genişliğini 10 piksel ise, örneğin, ardından array {10, 10} noktalı çizgi nokta ve boşluk vuruş kalınlığı aynı uzunlukta nerede çizer.

Ancak, `StrokeCap` ayarıyla `SKPaint` nesne de etkiler bu nokta ve tire. Kısa süre içinde anlatıldığı gibi bu dizi öğeleri üzerinde bir etkisi yoktur.

Noktalı ve kesikli satır gösterilen üzerinde **nokta ve kısa çizgilerden** sayfası. [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) dosya başlatır iki `Picker` görünümleri, vuruş cap ve bir tire dizi seçmek için ikinci seçmenize izin vermek için bir tane:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.DotsAndDashesPage"
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
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>10, 10</x:String>
                <x:String>30, 10</x:String>
                <x:String>10, 10, 30, 10</x:String>
                <x:String>0, 20</x:String>
                <x:String>20, 20</x:String>
                <x:String>0, 20, 20, 20</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

İlk üç öğe `dashArrayPicker` vuruşun genişliğini 10 piksel olduğunu varsayın. {10, 10} dizidir noktalı çizgi için {30, 10} olan kesikli bir çizgi ve {10, 10, 30, 10 için} için nokta ve tire satırıdır. (Diğer üç kısa süre içinde incelenecektir.)

[ `DotsAndDashesPage` Arka plan kod dosyasına](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) içeren `PaintSurface` olay işleyicisi ve birkaç erişmek için yardımcı yordamları `Picker` görünümleri:

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
        StrokeCap = (SKStrokeCap)Enum.Parse(typeof(SKStrokeCap),
                        strokeCapPicker.Items[strokeCapPicker.SelectedIndex]),

        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }

    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string[] strs = picker.Items[picker.SelectedIndex].Split(new char[] { ' ', ',' },
                                                             StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

Aşağıdaki ekran görüntülerinde en solundaki iOS ekranda noktalı çizgi gösterir:

[![](dots-images/dotsanddashes-small.png "Üçlü sayfasının ekran görüntüsü nokta ve tire")](dots-images/dotsanddashes-large.png#lightbox "Üçlü sayfasının ekran görüntüsü nokta ve tire")

Ancak, Android ekran ayrıca {10, 10} dizisi kullanılarak noktalı çizgi göstermek için beklenen ancak bunun yerine satır doludur. Ne oldu? Android ekran de vuruş caps ayarı varsa sorunudur `Square`. Bu, bunları doldurmak neden yarım vuruşun genişliğini tüm çizgilerle genişletir.

Bu sorunu gidermek, vuruş cap kullanırken almak için `Square` veya `Round`, dizideki tire uzunlukları (bazen bir tire uzunluğu 0 kaynaklanan) Vuruş uzunluğuna göre azaltın ve boşluk uzunlukları vuruş uzunluğuna göre artırın. Son üç dizilerde nasıl tire budur `Picker` XAML dosyasında hesaplanmıştır:

- {10, 10} hale {0, 20} noktalı çizgi için
- {30, 10} hale {20, 20} kesikli çizgi için
- {10, 10, 30, 10} {0, 20, 20, 20} için bir noktalı ve kesikli satır haline gelir.

Noktalı ve kesikli satır bir vuruş yapmak için Windows ekran gösterir cap `Round`. `Round` Vuruş cap kalın satırlarında genellikle nokta ve tire en iyi görünümünü verir.

Şu ana kadar hiç ikinci parametrenin yapılmadığını `SKPathEffect.CreateDash` yöntemi. Bu parametre adlı `phase` satırının başına, nokta ve tire desen içindeki uzaklığı ifade eder. Örneğin, tire diziyse, {10, 10} ve `phase` 10'dur ve satır bir nokta yerine boşluk ile başlar.

Bir ilginç uygulaması `phase` animasyonda parametresidir. **Animasyonlu Spiral** sayfa benzer **Archimedean Spiral** , dışında sayfasında [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) sınıfı canlandırır `phase` parametresi. Sayfa ayrıca animasyon için başka bir yaklaşım gösterir. Önceki örneği [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) kullanılan `Task.Delay` animasyon denetlemek için yöntem. Bu örnekte bunun yerine Xamarin.Forms kullanan `Device.Timer` yöntemi:


```csharp
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
```

Elbette, animasyonun görmek için programı çalıştırdığı gerekir:

[![](dots-images/animatedspiral-small.png "Üçlü sayfasının ekran görüntüsü animasyonlu Spiral")](dots-images/animatedspiral-large.png#lightbox "Üçlü sayfasının ekran görüntüsü animasyonlu Spiral")

Şimdi nasıl çizgiler çizme ve parametrik denklemini kullanarak Eğriler tanımlamak için gördünüz. Daha sonra yayımlanmış bir bölümde Eğriler çeşitli türlerde ele alınacaktır, `SKPath` destekler.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
