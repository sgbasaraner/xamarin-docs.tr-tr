---
title: Satırlar ve vuruş uçları
description: Bu makalede SkiaSharp satırları farklı vuruş uçları Xamarin.Forms uygulamalarında çizmek için kullanmayı açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 6dc7737290bf7eacb3ba0e0bca0ddcfcd4aacba3
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615268"
---
# <a name="lines-and-stroke-caps"></a>Satırlar ve vuruş uçları

_SkiaSharp farklı vuruş uçlarıyla çizgi çizmek için kullanmayı öğrenin_

SkiaSharp içinde tek bir satır işleme bağlı düz çizgiler bir dizi işlemesini çok farklı değildir. Hatta tek satırlar çizme, ancak genellikle belirli darbe genişliği satırları vermek gereklidir. Bu satırlar geniş haline geldiğinden, satır görünümünü de önemli hale gelir. Satır görünümünü adlı *fırça darbesi ucu*:

![](lines-images/strokecapsexample.png "Üç vuruş caps seçenekleri")

Çoklu çizgi çizme için `SKCanvas` basit tanımlar [ `DrawLine` ](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) yöntem bağımsız değişkenleri, başlangıç ve bitiş koordinatları içeren satırı belirten bir `SKPaint` nesnesi:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Varsayılan olarak, [ `StrokeWidth` ](xref:SkiaSharp.SKPaint.StrokeWidth) özelliği yeni oluşturulan `SKPaint` nesnedir 0 olarak varsayılır; bir pikselin bir çizgi kalınlığı işleme, 1 değerini aynı etkiye sahiptir. Büyük olasılıkla ayarlamak da istersiniz, böylece bu telefonlar gibi yüksek çözünürlüklü cihazlarda çok ince görünür `StrokeWidth` için daha büyük bir değer. Ancak, hacimle kalınlığı satırlar çizme başlattıktan sonra başka bir sorun oluşturan: nasıl başlangıç ve bitişini kalın satırı işlenecek?

Başlangıç ve bitişini satırların görünüşünü adlı bir *çizgi ucu* veya Skia, bir *fırça darbesi ucu*. Hat tür için bu bağlamda "sınır" sözcüğünü ifade eder &mdash; satırın sonuna üzerinde yer alan bir şey. Ayarladığınız [ `StrokeCap` ](xref:SkiaSharp.SKPaint.StrokeCap) özelliği `SKPaint` aşağıdaki üyeleri birine nesne [ `SKStrokeCap` ](xref:SkiaSharp.SKStrokeCap) sabit listesi:

- `Butt` (varsayılan)
- `Square`
- `Round`

Bu, en iyi bir örnek program ile gösterilmiştir. **SkiaSharp satırları ve yolları** bölümünü [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program başlıklı bir sayfa ile başlayan **vuruş uçları** göre [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) sınıfı. Bu sayfayı tanımlayan bir `PaintSurface` üç üyelerinin döngü olay işleyicisi `SKStrokeCap` hem sabit listesi üye adı görüntüleme ve vuruş CAP'ye kullanarak çizgi çizme numaralandırma:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Center
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

Her üye için `SKStrokeCap` numaralandırma işleyici, bir vuruş kalınlığı 50 piksel ve üst kısımdaki iki piksel ile bir vuruş kalınlığı konumlandırılmış başka bir satır ile iki satır çizer. Bu ikinci satır, geometrik başlangıç ve bitiş satırı çizgi kalınlığı ve bir fırça darbesi ucu bağımsız açıklamak amacıyla oluşturulmuştur:

[![](lines-images/strokecaps-small.png "Üçlü sayfasının ekran görüntüsü vuruş uçları")](lines-images/strokecaps-large.png#lightbox "Üçlü sayfasının ekran görüntüsü vuruş uçları")

Gördüğünüz gibi `Square` ve `Round` vuruş uçları etkili bir şekilde genişletme satırın uzunluğuna göre yarı darbe genişliği çizginin başında ve sonunda yeniden. Bir işlenen bir grafik nesnesinin boyutunu belirlemek gerekli olduğunda bu uzantı önemli hale gelir.

`SKCanvas` Sınıfı da biraz özgü olan çok satırlı çizmek için başka bir yöntem içerir:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` Parametresi, bir dizi `SKPoint` değerleri ve `mode` üyesi [ `SKPointMode` ](xref:SkiaSharp.SKPointMode) üç üyesi olan sabit listesi:

- `Points` tek tek noktaları oluşturmak için
- `Lines` Her noktası çifti bağlanmak için
- `Polygon` Tüm ardışık noktalar bağlanmak için

**Birden fazla satır** sayfası, bu yöntem gösterir. [ **MultipleLinesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) dosya iki başlatır `Picker` olanak tanıyan görünümleri seçin üyesi `SKPointMode` numaralandırma ve üyesi `SKStrokeCap` sabit listesi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPointMode}">
                    <x:Static Member="skia:SKPointMode.Points" />
                    <x:Static Member="skia:SKPointMode.Lines" />
                    <x:Static Member="skia:SKPointMode.Polygon" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
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

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

SkiaSharp ad alanı bildirimi biraz farklı olduğundan olduğunu fark `SkiaSharp` üyelerine başvuru için gereken ad alanı `SKPointMode` ve `SKStrokeCap` numaralandırma. `SelectedIndexChanged` Hem işleyici `Picker` görünümleri yalnızca geçersiz kılar `SKCanvasView` nesnesi:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Bu işleyici varlığını denetlemek gereken `SKCanvasView` nesne olay işleyici ilk olduğundan çağrılır `SelectedIndex` özelliği `Picker` XAML dosyasında 0 olarak ayarlanır ve oluşan önce `SKCanvasView` başlatıldı.

`PaintSurface` İşleyici alır iki numaralandırma değerlerinden `Picker` görünümler:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = (SKPointMode)pointModePicker.SelectedItem;
    canvas.DrawPoints(pointMode, points, paint);
}
```

Çeşitli ekran görüntüsünde gösterilmektedir `Picker` üç platformlarda seçimleri:

[![](lines-images/multiplelines-small.png "Üçlü sayfasının ekran görüntüsü birden fazla satır")](lines-images/multiplelines-large.png#lightbox "Üçlü sayfasının ekran görüntüsü birden fazla satır")

Sol gösterir, iPhone nasıl `SKPointMode.Points` numaralandırma üyesi neden `DrawPoints` her noktalar işlemek için `SKPoint` satır sınır ise bir kare dizi `Butt` veya `Square`. Satır sınırı ise daireler işlenir `Round`.

Bunun yerine kullandığınızda `SKPointMode.Lines`Merkezi'nde Android ekranda gösterilen şekilde `DrawPoints` yöntemi her çifti arasında bir çizgi çizer `SKPoint` değerlerini, belirtilen satır sınırı, bu durumda kullanarak `Round`.

UWP ekran sonucunu gösterir `SKPointMode.Polygon` değeri. Bir çizgi dizisinde ardışık noktalar arasındaki çizilir, ancak çok yakından bakarsanız, bu satırlar bağlanmamış görürsünüz. Bu ayrı satırların her biri başlatır ve belirtilen satır cap ile sona erer. Seçerseniz `Round` büyük harfler, satırlar, bağlanması görünebilir, ancak bunlar gerçekten bağlı olmasanız.

Satırları bağlı veya bağlı değil grafik yolları ile çalışmanın önemli bir yönüdür.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
