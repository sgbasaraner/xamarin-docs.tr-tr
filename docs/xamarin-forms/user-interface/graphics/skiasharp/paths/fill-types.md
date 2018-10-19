---
title: Yol dolgusu türleri
description: Bu makalede, farklı etkileri olası SkiaSharp yol dolgusu türleri ile inceler ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: d16f6f6023c1db0223d5d5863e19116147f948d1
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615476"
---
# <a name="the-path-fill-types"></a>Yol dolgusu türleri

_SkiaSharp yol dolgusu türleri ile olası farklı etkileri keşfedin_

Bir yolda iki dağılımlarını binebilir ve tek bir dağılımı satırlarını üstüne binebilir. Herhangi bir kapalı alan büyük olasılıkla doldurulabilir ancak iliştirilmiş tüm alanları doldurmak istemeyebilirsiniz. Örnek buradadır:

![](fill-types-images/filltypeexample.png "Beş işaret kısmen filles yıldız")

Bu üzerinde biraz denetim var. Doldurma algoritması tabidir [ `SKFillType` ](xref:SkiaSharp.SKPath.FillType) özelliği `SKPath`, üyesi için ayarlanan [ `SKPathFillType` ](xref:SkiaSharp.SKPathFillType) sabit listesi:

- `Winding`, varsayılan
- `EvenOdd`
- `InverseWinding`
- `InverseEvenOdd`

Herhangi bir kapalı alan dolu veya bu alandan sonsuza kuramsal çizgi temel alınarak doldurulmamış varsa sargı ve çift-tek algoritmaları belirleyin. Bu satırı yolu bir veya daha fazla sınır satırlarını çizer. Tek yönlü Bakiye çizgileri ve diğer yöndeki ve ardından alanı sayısı kullanıma sınır çizgileri sayısı değilse sargı moduyla doldurulur. Aksi takdirde alan doldurulur. Sınır satır sayısı çift ise, çift-tek algoritması bir alan doldurur.

Birçok rutin yollarıyla sargı algoritması genellikle kapalı bir yol alanlarının tüm doldurur. Çift-tek algoritması, genellikle daha ilginç sonuçlar üretir.

Klasik örnek gösterildiği şekilde beş işaret eden bir yıldız olan **Five-Pointed yıldız** sayfası. [ **FivePointedStarPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) dosya iki başlatır `Picker` görünümleri yolu seçmek için Dolgu türü ve yol konturlanan veya doldurulmuş veya her ikisi de ve hangi sırayla:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.FivePointedStarPage"
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
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPathFillType}">
                    <x:Static Member="skia:SKPathFillType.Winding" />
                    <x:Static Member="skia:SKPathFillType.EvenOdd" />
                    <x:Static Member="skia:SKPathFillType.InverseWinding" />
                    <x:Static Member="skia:SKPathFillType.InverseEvenOdd" />
                </x:Array>
            </Picker.ItemsSource>
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
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Fill only</x:String>
                    <x:String>Stroke only</x:String>
                    <x:String>Stroke then Fill</x:String>
                    <x:String>Fill then Stroke</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2"
                                PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Arka plan kod dosyasında hem de kullandığı `Picker` beş işaret eden bir yıldız çizmek için değerler:

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
        FillType = (SKPathFillType)fillTypePicker.SelectedItem
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

    switch ((string)drawingModePicker.SelectedItem)
    {
        case "Fill only":
            canvas.DrawPath(path, fillPaint);
            break;

        case "Stroke only":
            canvas.DrawPath(path, strokePaint);
            break;

        case "Stroke then Fill":
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case "Fill then Stroke":
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

Yolun Dolgu türü doldurur ve vuruş değil, ancak iki normalde etkiler `Inverse` modları doldurur ve vuruş etkiler. Dolgular, iki için `Inverse` yıldız dışında alan doldurulur böylece türleri alanları oppositely doldurun. Strokes, ikisi için `Inverse` türlerini stroke dışında her şeyi rengi. İOS ekran görüntüsünde gösterildiği gibi bu ters dolgu türleri kullanarak tek bazı etkilere neden olabilir:

[![](fill-types-images/fivepointedstar-small.png "Üçlü sayfasının ekran görüntüsü Five-Pointed yıldız")](fill-types-images/fivepointedstar-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Five-Pointed yıldız")

Android ve UWP ekran görüntüleri tipik çift-tek ve sargı etkilerini gösterir, ancak vuruş ve dolgu sırasını de sonuçları etkiler.

Sarım algoritma çizgileri çizilir yönü bağlıdır. Satırları bir noktasından diğerine çizildiğini belirttiğiniz genellikle bir yolu, oluşturmakta olduğunuz, bu yöndeki denetleyebilirsiniz. Ancak, `SKPath` sınıfı da tanımlar gibi yöntemleri `AddRect` ve `AddCircle` tüm dağılımlarını çizin. Bu nesnelerin nasıl çizildiğini kontrol etmek için yöntemleri türünde bir parametre içeren [ `SKPathDirection` ](xref:SkiaSharp.SKPathDirection), iki üyesi vardır:

- `Clockwise`
- `CounterClockwise`

Yöntemlere `SKPath` içeren bir `SKPathDirection` parametresi verin, varsayılan değeri `Clockwise`.

**Çakışan daireler** sayfası, bir çift-tek yolu Dolgu türü ile dört çakışan daire ile yol oluşturur:

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

Bu, oluşturulan kod en düşük ile ilgi çekici bir görüntü değil:

[![](fill-types-images/overlappingcircles-small.png "Üçlü sayfasının ekran görüntüsü çakışan daireler")](fill-types-images/overlappingcircles-large.png#lightbox "Üçlü sayfasının ekran görüntüsü çakışan daire")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
