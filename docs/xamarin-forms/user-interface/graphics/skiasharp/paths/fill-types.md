---
title: Yolun dolgu türleri
description: SkiaSharp yolu dolgu türleriyle olası farklı efektler Bul
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: d22ebf0e150c064835fa73765a65025f10ef4c2a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="the-path-fill-types"></a>Yolun dolgu türleri

_SkiaSharp yolu dolgu türleriyle olası farklı efektler Bul_

Bir yolunda iki dağılımlarını çakışma ve tek bir dağılım satırlarını binebilir. Herhangi bir kapalı alan olası doldurulabilir, ancak tüm kapalı alanları doldurmak istemeyebilirsiniz. Örnek buradadır:

![](fill-types-images/filltypeexample.png "Beş işaret kısmen filles yıldız")

Bu küçük bir denetime sahip. Doldurma algoritması tabidir [ `SKFillType` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.FillType/) özelliği `SKPath`, üyesi için ayarlanan [ `SKPathFillType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathFillType/) numaralandırma:

- [`Winding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.Winding/), varsayılan
- [`EvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.EvenOdd/)
- [`InverseWinding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseWinding/)
- [`InverseEvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseEvenOdd/)

Sarma ve çift-tek algoritmaları herhangi bir kapalı alan doldurulmuş veya bu alandan sonsuza kadar kuramsal çizgi dayanan doldurulmamış olmadığını belirleyin. Bu satırı yolu bir veya daha fazla sınır satırlarını kestiği. Başka bir yöne, sonra alanı çizilmiş satır sayısı, tek yönlü dengeyi çizilmiş sınır satır sayısı değilse sarma modu ile doldurulur. Aksi takdirde alan doldurulur. Sınır satır sayısı, tek ise çift-tek algoritması alanını doldurur.

Birçok rutin yollarını ile sarma algoritması genellikle kapalı bir yol alanlarının tüm doldurur. Çift-tek algoritması genellikle daha ilginç sonuçlar üretir.

Klasik beş işaret yıldız, örnekte gösterildiği şekilde örnektir **Five-Pointed yıldız** sayfası. [FivePointedStarPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) dosya başlatır iki `Picker` yolu seçmek için görünümleri Dolgu türü ve yol vuruş veya dolu ya da her ikisini ve hangi sırayla:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Winding</x:String>
                <x:String>EvenOdd</x:String>
                <x:String>InverseWinding</x:String>
                <x:String>InverseEvenOdd</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Fill only</x:String>
                <x:String>Stroke only</x:String>
                <x:String>Stroke then Fill</x:String>
                <x:String>Fill then Stroke</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Arka plan kod dosyası her ikisi de kullanır `Picker` beş işaret Yıldızı çizmek için değerler:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)Enum.Parse(typeof(SKPathFillType),
                        fillTypePicker.Items[fillTypePicker.SelectedIndex])
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle),
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch (drawingModePicker.SelectedIndex)
    {
        case 0:
            canvas.DrawPath(path, fillPaint);
            break;

        case 1:
            canvas.DrawPath(path, strokePaint);
            break;

        case 2:
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case 3:
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

Normalde, yolun Dolgu türü dolgular ve vuruşları değil, ancak iki etkiler, `Inverse` modları dolgular ve vuruşları etkiler. İki dolgular için `Inverse` yıldız dışında alan doldurulur böylece türleri alanları oppositely doldurun. Vuruşları, iki için `Inverse` türleri vuruşun dışında her şeyi rengi. Bu ters dolgu türlerini kullanarak, iOS ekran görüntüsü gösterilmektedir gibi bazı tek efektler üretmek:

[![](fill-types-images/fivepointedstar-small.png "Üçlü sayfasının ekran görüntüsü Five-Pointed yıldız")](fill-types-images/fivepointedstar-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Five-Pointed yıldız")

Android ve UWP ekran görüntüleri tipik çift-tek ve sarma etkileri gösterir ancak vuruş ve dolgu sırasını sonuçları de etkiler.

Sarma algoritması çizgileri çizilir yönü bağlıdır. Satırları bir noktadan diğerine çizildiğini belirtebilir genellikle ne zaman bir yolu oluşturmakta olduğunuz, bu yön denetleyebilirsiniz. Ancak, `SKPath` sınıfı ayrıca yöntemler gibi tanımlayan `AddRect` ve `AddCircle` tüm dağılımlarını çizin. Bu nesnelerin nasıl çizilir denetlemek için bir parametre türü yöntemleri dahil [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/), iki üye vardır:

- [`Clockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.Clockwise/)
- [`CounterClockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.CounterClockwise/)

Yöntemlere `SKPath` içeren bir `SKPathDirection` parametresi verin, varsayılan değer olan `Clockwise`.

**Çakışan daireler** sayfa dört çakışan daire çift-tek yolu Dolgu türü ile birlikte bir yol oluşturur:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

Kod en az ile oluşturulan bir ilginç görüntüsü verilmiştir:

[![](fill-types-images/overlappingcircles-small.png "Üçlü sayfasının ekran görüntüsü çakışan daireler")](fill-types-images/overlappingcircles-large.png#lightbox "Üçlü sayfasının ekran görüntüsü çakışan daireler")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
