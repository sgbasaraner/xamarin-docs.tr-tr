---
title: SkiaSharp içindeki parmak boyama
description: Bu makalede, bir Xamarin.Forms uygulaması SkiaSharp tuvalde boyanacak parmaklarınızın kullanmayı açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2017
ms.openlocfilehash: 03a6de3b6297e57620655e3697fe729e6fb06501
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615827"
---
# <a name="finger-painting-in-skiasharp"></a>SkiaSharp içindeki parmak boyama

_Boyama tuval üzerinde için parmaklarınızın kullanın._

Bir `SKPath` nesne sürekli olarak güncelleştirilebilir ve görüntülenir. Bu özellik, etkileşimli çizim için bir dokunmalı programında olduğu gibi kullanılması için bir yol sağlar.

![](finger-paint-images/fingerpaintsample.png "Bir alıştırmada parmak boyama")

Xamarin.Forms içinde dokunma desteği ek dokunma desteği sağlamak için bir Xamarin.Forms touch izleme etkisi geliştirilmiştir. Bu nedenle ekranda bireysel parmağınızı izleme izin vermez. Bu makalede açıklanan [ **etkileri olayları çağırma**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Örnek program [ **Touch izleme etkin tanıtımları** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) SkiaSharp dokunmalı programı da dahil olmak üzere, kullanan iki sayfa içerir.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) çözümü içerir. Bu touch izleme olayı. .NET Standard kitaplığı projesi içeren `TouchEffect` sınıfı `TouchActionType` numaralandırma `TouchActionEventHandler` temsilcisi ve `TouchActionEventArgs` sınıfı. Her platform projeleri içeren bir `TouchEffect` sınıfı için bu platform; iOS projesine de içeren bir `TouchRecognizer` sınıfı.

**Parmak boyama** sayfasını **SkiaSharpFormsDemos** parmak boyama basitleştirilmiş bir uygulamasıdır. Renk seçimine izin ver veya desteklemez genişliği vuruş yapmak, tuval temizlemek için hiçbir yolu yoktur ve Elbette resmi kaydedilemiyor.

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) dosya puts `SKCanvasView` tek hücresinde `Grid` ve ekler `TouchEffect` o `Grid`:

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

İliştirme `TouchEffect` doğrudan `SKCanvasView` tüm platformlar altında çalışmaz.

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) arka plan kod dosyasında depolamak için iki koleksiyonu tanımlar `SKPath` nesneleri bir `SKPaint` bu yollar işlemek için nesne:

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

Adından da anlaşılacağı gibi `inProgressPaths` sözlük bir veya daha fazla parmağınızı tarafından şu anda çizilmiş yolları depolar. Dokunma olayları eşlik eden touch ID sözlük anahtardır. `completedPaths` Alan yolu çizim bir parmak ekranından yükseltilmiş yükleyen tamamlanmış yollar koleksiyonudur.

`TouchAction` İşleyici bu iki koleksiyon yönetir. Böylece parmağınızı ilk ekran dokunduğunda yeni `SKPath` eklenir `inProgressPaths`. Bu parmak taşınırken, ek noktaları yolunu eklenir. Parmak yayınlandığı zaman, yolun aktarılır `completedPaths` koleksiyonu. Aynı anda birden çok parmağınızla boyama yapabilirsiniz. Bir yol veya koleksiyonlar, her değişiklikten sonra `SKCanvasView` geçersiz:

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

Touch-izleme olayları eşlik eden noktaları Xamarin.Forms koordinatları; Bu, pikseller SkiaSharp koordinatları için dönüştürülmelidir. Amacı olan `ConvertToPixel` yöntemi.

`PaintSurface` İşleyici ardından her iki koleksiyon yollarının yalnızca işler. Daha önce tamamlanan yolları sürüyor yolları altında görünür:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
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

Parmak resimler, yalnızca, beceri tarafından sınırlandırılmıştır:

[![](finger-paint-images/fingerpaint-small.png "Üçlü sayfasının ekran görüntüsü parmak boyama")](finger-paint-images/fingerpaint-large.png#lightbox "Üçlü sayfasının ekran görüntüsü parmak boyama")

Şimdi çizgi çizmek için ve parametreli denklemler kullanarak eğrileri tanımla gördünüz. Bir sonraki bölüm [ **SkiaSharp eğrileri ve yolları** ](../curves/index.md) eğrileri çeşitli türlerde kapsar, `SKPath` destekler. Ancak yararlı bir önkoşul İnceleme [ **SkiaSharp dönüşümleri**](../transforms/index.md).

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Dokunma izleme etkin tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Etkileri olayları çağırma](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
