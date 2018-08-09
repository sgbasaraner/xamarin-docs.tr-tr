---
title: SkiaSharp bit eşlemler kırpma
description: Kırpma dikdörtgenini desribing etkileşimli bir kullanıcı arabirimi tasarlamanıza SkiaSharp kullanmayı öğrenin.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 0A79AB27-C69F-4376-8FFE-FF46E4783F30
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 3dd9011d19e77f52d1fe89a37e4d992c23c72ab1
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615554"
---
# <a name="cropping-skiasharp-bitmaps"></a>SkiaSharp bit eşlemler kırpma

[ **Oluşturma ve çizme SkiaSharp bit eşlemler** ](drawing.md) makalede açıklanan nasıl bir `SKBitmap` nesne geçilebilir bir `SKCanvas` Oluşturucusu. Bit eşlem işlenmek üzere bu tuval nedenleri grafik üzerinde çağrılır, çizim bir yöntem. Çizim yöntemleri bunlar `DrawBitmap`, yani bu tekniği bölümünü veya tümünü bir bit eşlem için başka bir bit eşlem, belki de ile uygulanan Dönüşümlerin aktarma sağlar.

Bir bit eşlem çağırarak kırpma için bu tekniği kullanabilirsiniz [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) ile kaynak ve hedef dikdörtgenin yöntemi:

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

Ancak, genellikle kırpma kullanan uygulamalar, kırpma dikdörtgenini etkileşimli olarak seçmek kullanıcı arabirimi sağlar:

![Örnek kırpma](cropping-images/CroppingSample.png "örnek kırpma")

Bu makalede, bu arabirimdeki odaklanır.

## <a name="encapsulating-the-cropping-rectangle"></a>Kırpma dikdörtgenini kapsülleme

Kırpma mantık adlı bir sınıf, bazıları yalıtmak yararlıdır `CroppingRectangle`. Oluşturucu parametresi, genel olarak kırpılmış bit eşlem boyutudur, en fazla bir dikdörtgen ve isteğe bağlı bir en boy oranını içerir. Oluşturucu içinde ortak olmasını sağlayan bir ilk kırpma dikdörtgenini ilk tanımlar `Rect` türünün özelliği `SKRect`. Bu ilk kırpma dikdörtgenini genişliğinin % 80'i ve bit eşlem dikdörtgenin yüksekliğini olduğu halde bir en boy oranını belirtilirse ardından ayarlanır:

```csharp
class CroppingRectangle
{
    ···
    SKRect maxRect;             // generally the size of the bitmap
    float? aspectRatio;

    public CroppingRectangle(SKRect maxRect, float? aspectRatio = null)
    {
        this.maxRect = maxRect;
        this.aspectRatio = aspectRatio;

        // Set initial cropping rectangle
        Rect = new SKRect(0.9f * maxRect.Left + 0.1f * maxRect.Right,
                          0.9f * maxRect.Top + 0.1f * maxRect.Bottom,
                          0.1f * maxRect.Left + 0.9f * maxRect.Right,
                          0.1f * maxRect.Top + 0.9f * maxRect.Bottom);

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            SKRect rect = Rect;
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;
                rect.Left = (maxRect.Width - width) / 2;
                rect.Right = rect.Left + width;
            }
            else
            {
                float height = rect.Width / aspect;
                rect.Top = (maxRect.Height - height) / 2;
                rect.Bottom = rect.Top + height;
            }

            Rect = rect;
        }
    }
    
    public SKRect Rect { set; get; }
    ···
}
```

Yararlı bir bilgi parçasıdır, `CroppingRectangle` ayrıca kullanılabilir hale getirir, bir dizi `SKPoint` dört sırada sol, sağ üst, sağ ve sol alt kırpma dikdörtgenin köşelerini karşılık gelen değerleri:

```csharp
class CroppingRectangle
{
    ···
    public SKPoint[] Corners
    {
        get
        {
            return new SKPoint[]
            {
                new SKPoint(Rect.Left, Rect.Top),
                new SKPoint(Rect.Right, Rect.Top),
                new SKPoint(Rect.Right, Rect.Bottom),
                new SKPoint(Rect.Left, Rect.Bottom)
            };
        }
    }
    ···
}
```

Bu dizi olarak adlandırılan aşağıdaki yönteminde kullanılan `HitTest`. `SKPoint` Parametredir bir parmak touch için karşılık gelen bir nokta veya bir fare tıklatın. Bir dizini (0, 1, 2 veya 3) yöntem döndürür tarafından verilen uzaklıkta parmak veya fare işaretçisi dokunulan köşe karşılık gelen `radius` parametresi: 

```csharp
class CroppingRectangle
{
    ···
    public int HitTest(SKPoint point, float radius)
    {
        SKPoint[] corners = Corners;

        for (int index = 0; index < corners.Length; index++)
        {
            SKPoint diff = point - corners[index];
                
            if ((float)Math.Sqrt(diff.X * diff.X + diff.Y * diff.Y) < radius)
            {
                return index;
            }
        }

        return -1;
    }
    ···
}
```

Dokunma veya fare noktası içinde olmasaydı `radius` birimleri herhangi bir köşesinin yöntemi döndürür &ndash;1.

Son yöntemi `CroppingRectangle` çağrılır `MoveCorner`, dokunma veya fare hareketini yanıt olarak çağrılır. İki parametrenin dizinini taşınan köşe ve bu köşe yeni konumunu belirtin. Yöntemin ilk yarısında köşesinin ama her zaman sınırları içinde yeni konumuna bağlı olarak kırpma dikdörtgenini ayarlar `maxRect`, bit eşlem boyutunu olduğu. Bu mantık Ayrıca, hesaba katar `MINIMUM` kırpma dikdörtgenini bir şey daraltma önlemek için alan:

```csharp
class CroppingRectangle
{
    const float MINIMUM = 10;   // pixels width or height
    ···
    public void MoveCorner(int index, SKPoint point)
    {
        SKRect rect = Rect;

        switch (index)
        {
            case 0: // upper-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 1: // upper-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 2: // lower-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;

            case 3: // lower-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;
        }

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;

                switch (index)
                {
                    case 0:
                    case 3: rect.Left = rect.Right - width; break;
                    case 1:
                    case 2: rect.Right = rect.Left + width; break;
                }
            }
            else
            {
                float height = rect.Width / aspect;

                switch (index)
                {
                    case 0:
                    case 1: rect.Top = rect.Bottom - height; break;
                    case 2:
                    case 3: rect.Bottom = rect.Top + height; break;
                }
            }
        }

        Rect = rect;
    }
}
```

Yönteminin ikinci yarısında hiç için isteğe bağlı en boy oranını ayarlar.

Bu sınıftaki her şeyi piksel birimleri cinsinden olduğunu aklınızda bulundurun.

## <a name="a-canvas-view-just-for-cropping"></a>Kırpma için yalnızca bir tuval Görünüm

`CroppingRectangle` Yalnızca görülen sınıfı tarafından kullanılan `PhotoCropperCanvasView` türetilen sınıf `SKCanvasView`. Bu sınıf, kırpma dikdörtgenini değiştirmek için dokunma veya fare olayları işleme yanı sıra bit eşlem ve kırpma dikdörtgenini görüntüleme sorumludur.

`PhotoCropperCanvasView` Oluşturucusu bir bit eşlem gerektirir. En boy oranını isteğe bağlıdır. Oluşturucu türü bir nesneyi başlatır `CroppingRectangle` bu bit eşlem ve en boy oranını temel alan ve bir alan olarak kaydeder:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        this.bitmap = bitmap;

        SKRect bitmapRect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
        croppingRect = new CroppingRectangle(bitmapRect, aspectRatio);
        ···
    }
    ···
}
```

Bu sınıfın türetildiği için `SKCanvasView`, yüklemek için bir işleyici gerekli olmayan `PaintSurface` olay. Bunun yerine kılabilirsiniz, `OnPaintSurface` yöntemi. Bu yöntem, bit eşlem görüntüler ve birkaç kullanır `SKPaint` nesneleri geçerli kırpma dikdörtgen çizmek için alanları olarak kaydedildi:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    const int CORNER = 50;      // pixel length of cropper corner
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;
    ···
    // Drawing objects
    SKPaint cornerStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 10
    };

    SKPaint edgeStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 2
    };
    ···
    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        base.OnPaintSurface(args);

        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Gray);

        // Calculate rectangle for displaying bitmap 
        float scale = Math.Min((float)info.Width / bitmap.Width, (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect bitmapRect = new SKRect(x, y, x + scale * bitmap.Width, y + scale * bitmap.Height);
        canvas.DrawBitmap(bitmap, bitmapRect);

        // Calculate a matrix transform for displaying the cropping rectangle
        SKMatrix bitmapScaleMatrix = SKMatrix.MakeIdentity();
        bitmapScaleMatrix.SetScaleTranslate(scale, scale, x, y);

        // Display rectangle
        SKRect scaledCropRect = bitmapScaleMatrix.MapRect(croppingRect.Rect);
        canvas.DrawRect(scaledCropRect, edgeStroke);

        // Display heavier corners
        using (SKPath path = new SKPath())
        {
            path.MoveTo(scaledCropRect.Left, scaledCropRect.Top + CORNER);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Left + CORNER, scaledCropRect.Top);

            path.MoveTo(scaledCropRect.Right - CORNER, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top + CORNER);

            path.MoveTo(scaledCropRect.Right, scaledCropRect.Bottom - CORNER);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Right - CORNER, scaledCropRect.Bottom);

            path.MoveTo(scaledCropRect.Left + CORNER, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom - CORNER);

            canvas.DrawPath(path, cornerStroke);
        }

        // Invert the transform for touch tracking
        bitmapScaleMatrix.TryInvert(out inverseBitmapMatrix);
    }
    ···
}
```

Kodda `CroppingRectangle` sınıfı kırpma dikdörtgenini bit eşlemin piksel boyutuna göre ayarlar. Ancak, bit eşlem tarafından görüntülenmesini `PhotoCropperCanvasView` sınıfı ölçeği temel görüntü alanının boyutuna. `bitmapScaleMatrix` Cinsinden hesaplanır `OnPaintSurface` görüntülendiği bit eşlem piksel haritalar'dan boyutuna ve bit eşlem konumunu geçersiz kılar. Bu matris sonra bit eşlem göre görüntülenebilen böylece kırpma dikdörtgenini dönüştürmek için kullanılır.

Son satırının `OnPaintSurface` geçersiz kılma alan tersini `bitmapScaleMatrix` olarak kaydeder `inverseBitmapMatrix` alan. Bu, dokunma işleme için kullanılır.

A `TouchEffect` nesnesi bir alan olarak örneği ve bir işleyici oluşturucuya iliştirir `TouchAction` olay, ancak `TouchEffect` eklenmesi gereken `Effects` koleksiyonunu _üst_ ,`SKCanvasView`türetme, böylece Bitti `OnParentSet` geçersiz kıl:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    const int RADIUS = 100;     // pixel radius of touch hit-test
    ···
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;

    // Touch tracking 
    TouchEffect touchEffect = new TouchEffect();
    struct TouchPoint
    {
        public int CornerIndex { set; get; }
        public SKPoint Offset { set; get; }
    }

    Dictionary<long, TouchPoint> touchPoints = new Dictionary<long, TouchPoint>();
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        ···
        touchEffect.TouchAction += OnTouchEffectTouchAction;
    }
    ···
    protected override void OnParentSet()
    {
        base.OnParentSet();

        // Attach TouchEffect to parent view
        Parent.Effects.Add(touchEffect);
    }
    ···
    void OnTouchEffectTouchAction(object sender, TouchActionEventArgs args)
    {
        SKPoint pixelLocation = ConvertToPixel(args.Location);
        SKPoint bitmapLocation = inverseBitmapMatrix.MapPoint(pixelLocation);

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Convert radius to bitmap/cropping scale
                float radius = inverseBitmapMatrix.ScaleX * RADIUS;

                // Find corner that the finger is touching
                int cornerIndex = croppingRect.HitTest(bitmapLocation, radius);

                if (cornerIndex != -1 && !touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = new TouchPoint
                    {
                        CornerIndex = cornerIndex,
                        Offset = bitmapLocation - croppingRect.Corners[cornerIndex]
                    };

                    touchPoints.Add(args.Id, touchPoint);
                }
                break;

            case TouchActionType.Moved:
                if (touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = touchPoints[args.Id];
                    croppingRect.MoveCorner(touchPoint.CornerIndex, 
                                            bitmapLocation - touchPoint.Offset);
                    InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchPoints.ContainsKey(args.Id))
                {
                    touchPoints.Remove(args.Id);
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Xamarin.Forms.Point pt)
    {
        return new SKPoint((float)(CanvasSize.Width * pt.X / Width),
                           (float)(CanvasSize.Height * pt.Y / Height));
    }
}
```

Dokunma olayları tarafından işlenebilir `TouchAction` işleyici olan CİHAZDAN bağımsız birimler. Bu ilk kullanarak piksel olarak dönüştürülmesi gereken `ConvertToPixel` sınıfının altındaki yöntemi ve dönüştürülen `CroppingRectangle` kullanarak birimlerini `inverseBitmapMatrix`.

İçin `Pressed` olayları `TouchAction` işleyicisi çağrılarını `HitTest` yöntemi `CroppingRectangle`. Bu dizin dışındaki döndürürse &ndash;1, ardından kırpma dikdörtgenin köşelerini biri yönetilebilir. Dizin ve Köşeden gerçek touch noktasının bir uzaklık konumunda depolanır, bir `TouchPoint` nesne ve eklenen `touchPoints` sözlüğü.

İçin `Moved` olay `MoveCorner` yöntemi `CroppingRectangle` olası düzeltmeleri için en boy oranı ile köşesine taşıyın için çağrılır.

Herhangi bir zamanda programını kullanarak `PhotoCropperCanvasView` erişip `CroppedBitmap` özelliği. Bu özelliği kullanan `Rect` özelliği `CroppingRectangle` kırpılmış boyutta yeni bir bit eşlem oluşturmak için. Sürümü `DrawBitmap` hedef ve kaynak ile dikdörtgenler ardından ayıklar özgün bit eşlemin bir alt kümesi:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public SKBitmap CroppedBitmap
    {
        get
        {
            SKRect cropRect = croppingRect.Rect;
            SKBitmap croppedBitmap = new SKBitmap((int)cropRect.Width, 
                                                  (int)cropRect.Height);
            SKRect dest = new SKRect(0, 0, cropRect.Width, cropRect.Height);
            SKRect source = new SKRect(cropRect.Left, cropRect.Top, 
                                       cropRect.Right, cropRect.Bottom);

            using (SKCanvas canvas = new SKCanvas(croppedBitmap))
            {
                canvas.DrawBitmap(bitmap, source, dest);
            }

            return croppedBitmap;
        }
    }
    ···
}
```

## <a name="hosting-the-photo-cropper-canvas-view"></a>Fotoğraf cropper tuval görünümü barındırma

Söz konusu iki sınıfın işleme kırpma mantığı ile **fotoğraf kırpma** sayfasını **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** uygulama yapmak için çok az iş sahiptir. XAML dosyası oluşturur bir `Grid` konağa `PhotoCropperCanvasView` ve **Bitti** düğmesi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoCroppingPage"
             Title="Photo Cropping">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid x:Name="canvasViewHost"
              Grid.Row="0"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="1"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

`PhotoCropperCanvasView` Türünde bir parametre gerektirdiğinden XAML dosyasında oluşturulamaz `SKBitmap`.

Bunun yerine, `PhotoCropperCanvasView` kaynak bit eşlemleri birini kullanarak arka plan kod dosyası oluşturucuda örneği:

```csharp
public partial class PhotoCroppingPage : ContentPage
{
    PhotoCropperCanvasView photoCropper;
    SKBitmap croppedBitmap;

    public PhotoCroppingPage ()
    {
        InitializeComponent ();

        SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        photoCropper = new PhotoCropperCanvasView(bitmap);
        canvasViewHost.Children.Add(photoCropper);
    }

    void OnDoneButtonClicked(object sender, EventArgs args)
    {
        croppedBitmap = photoCropper.CroppedBitmap;

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(croppedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Kullanıcı, ardından kırpma dikdörtgenini değiştirebilirsiniz:

[![Fotoğraf Cropper 1](cropping-images/PhotoCropping1.png "fotoğraf Cropper 1")](cropping-images/PhotoCropping1-Large.png#lightbox)

İyi bir kırpma dikdörtgenini tanımlandığında tıklayın **Bitti** düğmesi. `Clicked` İşleyici alır kırpılmış eşlemden `CroppedBitmap` özelliği `PhotoCropperCanvasView`ve yeni bir sayfanın tüm içeriğini değiştirir `SKCanvasView` kırpılmış bu bit eşlem görüntüleyen nesnesi:

[![Fotoğraf Cropper 2](cropping-images/PhotoCropping2.png "fotoğraf Cropper 2")](cropping-images/PhotoCropping2-Large.png#lightbox)

İkinci bağımsız değişkeni yapmayı deneyin `PhotoCropperCanvasView` 1.78f (örneğin) için:

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

16 9 en boy oranı yüksek tanımlı televizyon özellik için kısıtlı kırpma dikdörtgenini görürsünüz.

<a name="tile-division" />

## <a name="dividing-a-bitmap-into-tiles"></a>Bir bit eşlem döşemesine bölme

Xamarin.Forms sürümünü ünlü kitabın bölüm 22'de 14-15 Bulmacanın görünen [ _Xamarin.Forms ile Mobile Apps oluşturma_ ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) ve olarak indirilebilir [  **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle). Ancak, daha fazla bilgi sahibi olmanın eğlenceli (ve genellikle daha zor) Bulmacanın olur kendi Fotoğraf Kitaplığı'ndan bir görüntüye tabanlıysa.

14-15 Bulmacanın bu sürümü parçasıdır **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** uygulama ve bir dizi başlıklı sayfaları oluşur **fotoğraf Bulmaca**.

**PhotoPuzzlePage1.xaml** dosya oluşur bir `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage1"
             Title="Photo Puzzle">
    
    <Button Text="Pick a photo from your library"
            VerticalOptions="CenterAndExpand" 
            HorizontalOptions="CenterAndExpand"
            Clicked="OnPickButtonClicked"/>
    
</ContentPage>
```

Arka plan kod dosyası uygulayan bir `Clicked` kullanan işleyici `IPhotoLibrary` fotoğraf fotoğraf kitaplığından çekme izin vermek için bağımlılık hizmeti:

```csharp
public partial class PhotoPuzzlePage1 : ContentPage
{
    public PhotoPuzzlePage1 ()
    {
        InitializeComponent ();
    }

    async void OnPickButtonClicked(object sender, EventArgs args)
    {
        IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
        using (Stream stream = await photoLibrary.PickPhotoAsync())
        {
            if (stream != null)
            {
                SKBitmap bitmap = SKBitmap.Decode(stream);

                await Navigation.PushAsync(new PhotoPuzzlePage2(bitmap));
            }
        }
    }
}
```

Yöntemi ardından gider `PhotoPuzzlePage2`, seçili bit eşlem Oluşturucu için geçen.

Fotoğraf Kitaplığı'nda görünen ancak döndürülen veya baş aşağı fotoğraf kitaplığından seçilen yönlendirilmiş olmadığını mümkündür. (İOS cihazları özellikle bir sorun budur.) Bu nedenle `PhotoPuzzlePage2` görüntünün istenen yönlendirmeye döndürmenizi sağlar. Etiketli üç düğme XAML dosyasını içeren **90&#x00B0; sağ** (saat yönünde anlamına gelir), **90&#x00B0; sol** (saatin) ve **Bitti**.

Arka plan kod dosyası makalesinde gösterilen bit eşlem döndürme mantığını uygular  **[oluşturma ve SkiaSharp bit eşlemler üzerinde çizim](drawing.md#rotating-bitmaps)**. Kullanıcının kaç kez 90 derece saat yönünde veya saat yönünün görüntü döndürebilirsiniz: 

```csharp
public partial class PhotoPuzzlePage2 : ContentPage
{
    SKBitmap bitmap;

    public PhotoPuzzlePage2 (SKBitmap bitmap)
    {
        this.bitmap = bitmap;

        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnRotateRightButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(bitmap.Height, 0);
            canvas.RotateDegrees(90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnRotateLeftButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(0, bitmap.Width);
            canvas.RotateDegrees(-90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        await Navigation.PushAsync(new PhotoPuzzlePage3(bitmap));
    }
}
```

Kullanıcı tıkladığında **Bitti** düğmesi `Clicked` işleyici gider `PhotoPuzzlePage3`, sayfanın oluşturucuda son döndürülen bir bit eşlem.

`PhotoPuzzlePage3` Kırpılacak fotoğraf sağlar. Program 4 x 4 karelerde döşemeler bölmek için kare şeklinde bir bit eşlem gerektirir.

**PhotoPuzzlePage3.xaml** dosyasını içeren bir `Label`, `Grid` konağa `PhotoCropperCanvasView`ve başka bir **Bitti** düğmesi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage3"
             Title="Photo Puzzle">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Label Text="Crop the photo to a square"
               Grid.Row="0"
               FontSize="Large"
               HorizontalTextAlignment="Center"
               Margin="5" />

        <Grid x:Name="canvasViewHost"
              Grid.Row="1"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="2"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

Arka plan kod dosyası başlatır `PhotoCropperCanvasView` , oluşturucuya geçirilen bit eşlem ile. İkinci bağımsız değişkeni olarak geçirilen bir 1 Uyarısı `PhotoCropperCanvasView`. Bu 1 en boy oranını kare olacak şekilde kırpma dikdörtgenini zorlar:

```csharp
public partial class PhotoPuzzlePage3 : ContentPage
{
    PhotoCropperCanvasView photoCropper;

    public PhotoPuzzlePage3(SKBitmap bitmap)
    {
        InitializeComponent ();

        photoCropper = new PhotoCropperCanvasView(bitmap, 1f);
        canvasViewHost.Children.Add(photoCropper);
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        SKBitmap croppedBitmap = photoCropper.CroppedBitmap;
        int width = croppedBitmap.Width / 4;
        int height = croppedBitmap.Height / 4;

        ImageSource[] imgSources = new ImageSource[15];

        for (int row = 0; row < 4; row++)
        {
            for (int col = 0; col < 4; col++)
            {
                // Skip the last one!
                if (row == 3 && col == 3)
                    break;

                // Create a bitmap 1/4 the width and height of the original
                SKBitmap bitmap = new SKBitmap(width, height);
                SKRect dest = new SKRect(0, 0, width, height);
                SKRect source = new SKRect(col * width, row * height, (col + 1) * width, (row + 1) * height);

                // Copy 1/16 of the original into that bitmap
                using (SKCanvas canvas = new SKCanvas(bitmap))
                {
                    canvas.DrawBitmap(croppedBitmap, source, dest);
                }

                imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
            }
        }

        await Navigation.PushAsync(new PhotoPuzzlePage4(imgSources));
    }
}
```

**Bitti** düğmesi işleyicisi genişlik ve yükseklik (Bu iki değeri aynı olmalıdır) kırpılmış bit eşlemin edinir ve ardından her biri 1/4 olan 15 ayrı eşlemleri böler özgün yükseklik ve genişlik. (Son olası 16 bit eşlem oluşturulmaz.) `DrawBitmap` Kaynak ve hedef dikdörtgenin yöntemiyle daha büyük bir bit eşlem alt kümesi üzerinde tabanlı oluşturulacak bir bit eşlem sağlar.

## <a name="converting-to-xamarinforms-bitmaps"></a>Xamarin.Forms bit eşlemlere dönüştürme

İçinde `OnDoneButtonClicked` yöntemi için 15 bit eşlemler oluşan dizi türü olduğundan [ `ImageSource` ](xref:Xamarin.Forms.ImageSource):

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource` bir bit eşlemi Kapsüller Xamarin.Forms temel türüdür. Neyse ki, SkiaSharp SkiaSharp bit eşlemler Xamarin.Forms bit eşlemlere dönüştürme sağlar. **SkiaSharp.Views.Forms** derleme tanımlayan bir [ `SKBitmapImageSource` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKBitmapImageSource/) türetilen sınıf `ImageSource` ancak bir SkiaSharp alınarak oluşturulamaz `SKBitmap` nesne. `SKBitmapImageSource` hatta arasında dönüştürmelerini tanımlayan `SKBitmapImageSource` ve `SKBitmap`ve bu yüzden nasıl `SKBitmap` nesneleri, bir dizi Xamarin.Forms bit eşlemler olarak depolanır:

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

Bit eşlem bu dizi için bir oluşturucu olarak geçirilen `PhotoPuzzlePage4`. Bu sayfa tamamen Xamarin.Forms ve tüm SkiaSharp kullanmaz. Çok benzer [ **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle), burada açıklanan olmaz, ancak 15 kare döşemesine bölünmüş Seçili fotoğraf görüntüler:

[![Fotoğraf Bulmacanın 1](cropping-images/PhotoPuzzle1.png "fotoğraf Bulmacanın 1")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

Tuşuna basarak **rastgele** düğmesi karıştırır tüm kutucuklar:

[![Fotoğraf Bulmacanın 2](cropping-images/PhotoPuzzle2.png "fotoğraf Bulmacanın 2")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

Artık bunları doğru sırayla koyabilirsiniz. Tüm kutucuklar aynı satır veya sütun boş bir kare olarak boş köşeli taşımak dokunulduğunda. 

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
