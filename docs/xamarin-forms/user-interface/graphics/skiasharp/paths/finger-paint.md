---
title: Parmak boyama
description: "Tuvalde boyamak için parmakları kullanın."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: c9894cac0064b91049b0142971896d7dd67dec5f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="finger-painting"></a>Parmak boyama

_Tuvalde boyamak için parmakları kullanın._

Bir `SKPath` nesne sürekli olarak güncelleştirilebilir ve görüntülenir. Bu özellik, etkileşimli çizim için bir finger-painting programında gibi kullanılması için bir yol sağlar.

![](finger-paint-images/fingerpaintsample.png "Bir alıştırmada parmak boyama")

Xamarin.Forms dokunma desteği Xamarin.Forms dokunma izleme etkili ek dokunma desteği sağlamak için geliştirilen için tek tek parmakları ekranda izleme izin vermiyor. Bu makalede açıklanan [ **çağırma olayları etkileri**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Örnek program [ **dokunma izleme etkisi gösterileri** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) finger-painting programı da dahil olmak üzere SkiaSharp kullanan iki sayfa içerir.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) çözümü bu dokunma izleme olayı içerir. Taşınabilir sınıf kitaplığı proje içeriyor `TouchEffect` sınıfı, `TouchActionType` numaralandırma, `TouchActionEventHandler` temsilci ve `TouchActionEventArgs` sınıfı. Her platform projeleri içeren bir `TouchEffect` sınıfı, platform için; iOS projesi de içerir bir `TouchRecognizer` sınıfı.

**Parmak boyama** sayfasındaki **SkiaSharpFormsDemos** parmak boyama basitleştirilmiş bir uygulamasıdır. Bırakmaz renk seçimine izin ver veya genişliği vuruş yapmak, tuvalin temizlemek için hiçbir yolu yoktur ve Elbette resminiz kaydedemezsiniz.

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) dosya yerleştirmelerin `SKCanvasView` tek hücreye `Grid` ve ekler `TouchEffect` , `Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Ekleme `TouchEffect` doğrudan `SKCanvasView` tüm platformlar altında çalışmaz.

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) arka plan kod dosyası tanımlar depolamak için iki koleksiyon `SKPath` nesneleri yanı sıra bir `SKPaint` nesne bu yolları işlemek için:

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

Ad önerilen olarak `inProgressPaths` sözlük bir veya daha fazla parmakları tarafından şu anda çizilmiş yolları depolar. Sözlük anahtarı dokunma olaylarına eşlik eden dokunma kimliğidir. `completedPaths` Alan ne zaman kaldırılmış yolu ekranından çizim bir parmak tamamlanmış yollar koleksiyonudur.

`TouchAction` İşleyici bu iki koleksiyonları yönetir. Bir parmak ilk ekran dokunduğunda yeni bir `SKPath` eklenen `inProgressPaths`. Bu parmak taşınırken, ek noktaları yolunu eklenir. Parmak yayımlandığında, yolun aktarılır `completedPaths` koleksiyonu. Aynı anda birden çok parmakları ile boyama yapabilirsiniz. Bir yol veya koleksiyonlar, her bir değişiklikten sonra `SKCanvasView` geçersiz:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

Touch-izleme olayları eşlik Xamarin.Forms koordinatları noktalarıdır; Bunlar piksel SkiaSharp koordinatları için dönüştürülmelidir. Amacı olan `ConvertToPixel` yöntemi.

`PaintSurface` İşleyici sonra her iki koleksiyon yol yalnızca işler. Önceden tamamlanmış yolları ilerleme yollarında altında görünür:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ,,,
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

Parmak resimler yalnızca, beceri tarafından sınırlıdır:

[![](finger-paint-images/fingerpaint-small.png "Üçlü sayfasının ekran görüntüsü parmak boyama")](finger-paint-images/fingerpaint-large.png "Üçlü sayfasının ekran görüntüsü parmak boyama")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [Dokunma izleme etkisi gösterileri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Olayları etkileri çağırma](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
