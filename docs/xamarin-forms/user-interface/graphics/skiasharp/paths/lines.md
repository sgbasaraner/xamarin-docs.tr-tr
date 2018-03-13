---
title: "Satırları ve vuruş büyük harfler"
description: "Farklı vuruş caps satırıyla çizmek için SkiaSharp kullanmayı öğrenin"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: abcda680f6cfbde802f7b666cf2aade2c6e11093
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="lines-and-stroke-caps"></a>Satırları ve vuruş büyük harfler

_Farklı vuruş caps satırıyla çizmek için SkiaSharp kullanmayı öğrenin_

SkiaSharp içinde tek bir satırı işleme bağlı düz çizgiler bir dizi işleme çok farklıdır. Bile tek satırlar çizme, ancak genellikle belirli vuruş genişliği ve daha geniş bir çizgi satırları vermek gerekli olan, adlı satırlar sonuna görünümünü daha önemli hale *vuruş cap*:

![](lines-images/strokecapsexample.png "Üç vuruş caps seçenekleri")

Tek satırlar çizme için `SKCanvas` basit bir tanımlar [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) olan bağımsız değişkenler belirtmek başlangıç ve bitiş satırıyla koordinatlarını yöntemi bir `SKPaint` nesnesi:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Varsayılan olarak, `StrokeWidth` özelliğinin yeni oluşturulmuş `SKPaint` bir piksel satırının içinde kalınlığı işleme 1 değerini aynı etkiye sahip 0 nesnesidir. Ayarlama istersiniz, bu çok ince telefonlar gibi yüksek çözünürlüklü cihazlarda görünür `StrokeWidth` daha büyük bir değer. Ancak boyutlandırılabilir kalınlığı satırlar çizme başladıktan sonra başka bir sorun olayını: Başlangıç ve bitişini bu kalın satırların işlenme?

Başlangıç ve bitişini satırların görünüşünü adlı bir *satır cap* veya Skia, bir *vuruş cap*. Bu bağlamda "cap" word tür hat & #x 2014 gösterir; bir şey satırın sonuna bulunur. Ayarladığınız [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) özelliği `SKPaint` aşağıdaki üyeleri birini nesnesine [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) numaralandırma:

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (varsayılan)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

Bu, en iyi bir örnek programla gösterilmiştir. Giriş sayfasının ikinci bölümü [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) programı adlı bir sayfa ile başlayan **vuruş Caps** göre [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) sınıfı. Bu sayfayı tanımlayan bir `PaintSurface` üç üyelerini döngü olay işleyicisi `SKStrokeCap` ad numaralandırma üyesi görüntüleme ve o vuruş cap kullanarak çizgi çizme numaralandırması:

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

Her bir üyesi için `SKStrokeCap` numaralandırma, işleyici çizer vuruş kalınlığı 50 piksel ve 2 piksel ile bir vuruş kalınlığı üstte konumlandırılmış başka bir satır ile iki satır. Bu ikinci satır geometrik başlangıç ve bitiş çizgi kalınlığı ve vuruş cap bağımsız satırının göstermeye yöneliktir:

[![](lines-images/strokecaps-small.png "Üçlü sayfasının ekran görüntüsü vuruş Caps")](lines-images/strokecaps-large.png#lightbox "Üçlü sayfasının ekran görüntüsü vuruş Caps")

Gördüğünüz gibi `Square` ve `Round` vuruş caps etkili bir şekilde genişletmek satırın uzunluğuna yarım vuruşun genişliğini satırın başındaki ve sonundaki yeniden tarafından. İşlenen grafik nesnesi boyutlarını belirlemek gerekli olduğunda bu uzantıyı önemli hale gelir.

`SKCanvas` Sınıfı biraz özgü olan birden fazla satır çizim için başka bir yöntem de içerir:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` Parametredir bir dizi `SKPoint` değerleri ve `mode` üyesi [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) üç üyeler içeriyor numaralandırma:

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) tek tek noktaları oluşturmak için
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) Her nokta çiftleri bağlanmak için
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) Tüm art arda gelen noktaları bağlanmak için

**Birden fazla satır** sayfa bu yöntem gösterilmektedir. [ `MultipleLinesPage` XAML dosyası](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) iki başlatır `Picker` olanak tanıyan görünümleri seçin üyesi `SKPointMode` numaralandırma ve üyesi `SKStrokeCap` numaralandırma:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.MultipleLinesPage"
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
            <Picker.Items>
                <x:String>Points</x:String>
                <x:String>Lines</x:String>
                <x:String>Polygon</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
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

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

`SelectedIndexChanged` Her ikisi için de işleyici `Picker` görünümler yalnızca geçersiz kılar `SKCanvasView` nesnesi:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Bu işleyici varlığını denetlemek gereken `SKCanvasView` nesne olay işleyicisi ilk olduğundan çağrılır `SelectedIndex` özelliği `Picker` XAML dosyasında 0 olarak ayarlayın ve önce oluşan `SKCanvasView` başlatıldı.

`PaintSurface` İşleyici erişen iki seçili öğelerden alma genel yöntemini `Picker` görünümleri ve numaralandırma değerlerini dönüştürme:

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
        StrokeCap = GetPickerItem<SKStrokeCap>(strokeCapPicker)
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = GetPickerItem<SKPointMode>(pointModePicker);
    canvas.DrawPoints(pointMode, points, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }
    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}
```

Ekran görüntüsü, çeşitli gösterir `Picker` üç platformlarda seçimleri:

[![](lines-images/multiplelines-small.png "Üçlü sayfasının ekran görüntüsü birden fazla satır")](lines-images/multiplelines-large.png#lightbox "Üçlü sayfasının ekran görüntüsü birden fazla satır")

Sol gösterir, iPhone nasıl `SKPointMode.Points` numaralandırma üyesi neden `DrawPoints` her nokta işlemek için `SKPoint` satır uç ise bir kare olarak dizi `Butt` veya `Square`. Satır ucun ise daireler işlenir `Round`.

Bunun yerine kullandığınızda `SKPointMode.Lines`Center'da Android ekranda gösterildiği gibi `DrawPoints` yöntemi her çifti arasında bir satır çizer `SKPoint` değerlerini, belirtilen satır ucun bu durumda kullanarak `Round`.

Windows mobil aygıtı sonucunu gösterir `SKPointMode.Polygon` değeri. Dizideki art arda gelen noktaları arasında bir çizgi çizilir, ancak çok yakından bakarsanız, bu satırları bağlanmamış görürsünüz. Bu ayrı satırların her biri başlar ve belirtilen satır ucun ile biter. Seçerseniz `Round` caps, satırları bağlanacağı görünebilir, ancak gerçekten bağlı.

Satırları bağlı veya bağlı grafik yolları ile çalışmanın kritik önem taşıyan bir boyut değil.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
