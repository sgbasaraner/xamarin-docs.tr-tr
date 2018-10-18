---
title: Dokunma işlemeleri
description: Bu makalede, matris dönüşümleri touch sürükleyerek, hareketinden ve döndürme uygulamak için kullanmayı açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: davidbritch
ms.author: dabritch
ms.date: 09/14/2018
ms.openlocfilehash: 6f7236a3650c04098edbef92f3d6ed620be501c3
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615398"
---
# <a name="touch-manipulations"></a>Dokunma işlemeleri

_Dokunma sürükleyerek, hareketinden ve döndürme uygulamak için kullanım matris dönüşümleri_

Bu mobil cihazlardaki gibi çok noktalı dokunma ortamlarında kullanıcılar kendi parmağınızı ekrandaki nesnelere işlemek için genellikle kullanın. Bir parmak sürükleyin ve iki parmak tabletinizde gibi ortak hareketleri taşıma ve nesneleri ölçeklendirme veya bile bunları döndür. Bu hareket, genellikle dönüştürme matrislerde kullanılarak uygulanır ve bu makalede bunu nasıl yapacağınız gösterilmektedir.

![](touch-images/touchmanipulationsexample.png "Çeviri, ölçeklendirme ve döndürme için tabi bir bit eşlem")

Burada gösterilen tüm örnekleri makalesinde sunulan Xamarin.Forms touch izleme etkisi kullanın [ **etkileri olayları çağırma**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

## <a name="dragging-and-translation"></a>Sürükleme ve çeviri

Matris dönüşümleri en önemli uygulamaları dokunma işleme biridir. Tek bir [ `SKMatrix` ](xref:SkiaSharp.SKMatrix) değer bir dizi touch işlem birleştirmek. 

Tek-parmak sürükleme, `SKMatrix` değeri çevirisi gerçekleştirir. Bu gösterilmiştir **bit eşlem sürükleyerek** sayfası. XAML dosyası örnekleyen bir `SKCanvasView` bir Xamarin.Forms içinde `Grid`. A `TouchEffect` nesne eklendi `Effects` , koleksiyonu `Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.BitmapDraggingPage"
             Title="Bitmap Dragging">
    
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

Teoride, `TouchEffect` nesne doğrudan eklenebiliyordu `Effects` koleksiyonunu `SKCanvasView`, ancak bu tüm platformlarda çalışmaz. Çünkü `SKCanvasView` olarak aynı boyutta olan `Grid` bu yapılandırmada eklemeyi `Grid` da çalışır.

Arka plan kod dosyası, yapıcısına bir bit eşlem kaynağındaki yükler ve görüntüler `PaintSurface` işleyicisi:

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    // Bitmap and matrix for display
    SKBitmap bitmap;
    SKMatrix matrix = SKMatrix.MakeIdentity();
    ···

    public BitmapDraggingPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, new SKPoint());
    }
}
```

Herhangi başka bir kod olmadan `SKMatrix` değeri her zaman kimlik matrisi ve bir bit eşlem görüntüsü üzerinde hiçbir etkisi yoktur. Amacı `OnTouchEffectAction` XAML dosyasında ayarlanan işleyicisidir dokunma işlemeleri yansıtacak şekilde matris değeri değiştirmek için.

`OnTouchEffectAction` İşleyici başlar Xamarin.Forms dönüştürerek `Point` bir SkiaSharp değerde `SKPoint` değeri. Bu temel ölçeklendirmenin basit bir konudur `Width` ve `Height` özelliklerini `SKCanvasView` (olan CİHAZDAN bağımsız birimler) ve `CanvasSize` piksel birimlerinde özelliğinin:

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    ···
    // Touch information
    long touchId = -1;
    SKPoint previousPoint;
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point = 
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point))
                {
                    touchId = args.Id;
                    previousPoint = point;
                }
                break;

            case TouchActionType.Moved:
                if (touchId == args.Id)
                {
                    // Adjust the matrix for the new position
                    matrix.TransX += point.X - previousPoint.X;
                    matrix.TransY += point.Y - previousPoint.Y;
                    previousPoint = point;
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = -1;
                break;
        }
    }
    ···
}
```

Ne zaman bir parmak ilk dokunduğu ekranında, ' % s'olay türü `TouchActionType.Pressed` tetiklenir. İlk görev parmak bit eşlem temas varsa belirlemektir. Bu tür bir görev genellikle adlandırılır _isabet sınaması_. Bu durumda, isabet sınaması oluşturarak gerçekleştirilebilir bir `SKRect` bunu matris dönüşümü uygulanarak, bit eşlem karşılık gelen değer `MapRect`ve ardından touch noktası dönüştürülmüş dikdörtgen içinde olup olmadığını belirleme.

Bu durum söz konusuysa sonra `touchId` alan için touch ID ayarlanır ve bu parmak konuma kaydedilir.

İçin `TouchActionType.Moved` olay, çeviri faktörünü `SKMatrix` değeri ayarlanmış tabanlı parmak geçerli konumunu ve parmak yeni konumu. Üzerinden, sonraki açışınızda için yeni bir konuma kaydedilir ve `SKCanvasView` geçersiz kılınır.

Bu programla denerken parmağınızı bit eşlem görüntüleyen bir alan dokunduğunda bit eşlem yalnızca sürükleyebilirsiniz not alın. Bu kısıtlama, bu program için çok önemli olmamasına karşın, birden çok bit eşlemler işlenirken önemli olur.

## <a name="pinching-and-scaling"></a>Hareketinden ve ölçeklendirme

Ne iki parmağınızı bit eşlem dokunduğunuzda olmasını istiyorsunuz? Ardından paralel olarak iki parmağınızı taşırsanız, muhtemelen parmağınızı birlikte taşımak için bit eşlem istersiniz. İki parmağınızı bir tabletinizde gerçekleştirmek veya esnetme işlemi, bit eşlem (sonraki bölümde açıklanmıştır) döndürülen ya da ölçeği isteyebilirsiniz. Bir bit eşlem olduğunda, çoğu ve buna uygun olarak ölçeklendirilmesi bit eşlem bit eşlem göre aynı konumlarda kalmasına iki parmağınızı için mantıklıdır.

Aynı anda iki parmağınızı işleme karmaşık görünüyor, ancak göz önünde bulundurun, `TouchAction` işleyici yalnızca aynı anda tek bir parmak hakkında bilgi alır. Bit eşlem iki parmağın hareket ettiği, her olay için tek bir parmak konumu değişti ancak diğer değiştirilmedi. İçinde **bit eşlem** sayfa kodu aşağıdaki konum değiştirilmedi parmak çağrılır _pivot_ dönüştürme o noktadan göreli olduğundan noktası.

Bu programın önceki program arasındaki tek fark, kimlikleri kaydedilmelidir birden çok dokunma ' dir. Bir sözlük burada touch ID sözlük anahtarı, bu parmak geçerli konumunu sözlük değeri ise bu amaçla kullanılır:

```csharp
public partial class BitmapScalingPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point) && !touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Add(args.Id, point);
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger scale and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index of non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points involved in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Scaling factors are ratios of those
                        float scaleX = newVector.X / oldVector.X;
                        float scaleY = newVector.Y / oldVector.Y;

                        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
                            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
                        {
                            // If smething bad hasn't happened, calculate a scale and translation matrix
                            SKMatrix scaleMatrix = 
                                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);

                            SKMatrix.PostConcat(ref matrix, scaleMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }
    ···
}
```

İşleme `Pressed` sözlüğe eklenen aynı önceki hariç program kimliği ve noktası dokunma eylemi neredeyse değildir. `Released` Ve `Cancelled` eylemleri sözlük girişi kaldırın.

İşleme için `Moved` eylem daha karmaşık ancak. Dahil olan yalnızca bir parmak ise ardından işleme çok önceki program aynıdır. İki veya daha fazla parmağınızı program de bilgi değil taşıma parmak içeren sözlükten edinmeniz gerekir. Sözlük anahtarları bir dizi içine kopyalayarak ve taşınan parmak kimliği ile ilk anahtar ardından karşılaştırarak bunu yapar. Bu değil taşıma parmağınızı karşılık gelen pivot noktası almak program sağlar.

Ardından, yeni parmak konumu pivot noktasına göreli ve eski parmak konumu pivot noktasına göreli iki vektör gelecektir. Bu vektörler, oranlarla Etkenler ölçeklendirme. Sıfıra bölme olasılığı olduğu için bu değerleri sonsuz veya NaN (sayı değil) değerleri için işaretlenmelidir. Tüm ise iyi ölçeklendirme dönüşüm ile birleştirilir `SKMatrix` değeri bir alan olarak kaydedilir.

Bu sayfayla denerken bir veya iki yola sahip bir bit eşlem sürükleyin veya iki parmağınızla ölçeği fark edeceksiniz. Ölçeklendirme _anizotropik_, ölçeklendirme yatay ve dikey yönde farklı olabileceğini anlamına gelir. Bu en boy oranını deforme eder, ancak Ayrıca, bit eşlemin bir Ayna görüntüsünü yapmak için ters çevirmek sağlar. Bit eşlemin bir sıfır boyuta Küçült ve kaybolur de fark edebilirsiniz. Üretim kodunda bu karşı koruma sağlamak isteyebilirsiniz.

## <a name="two-finger-rotation"></a>İki parmak döndürme

**Bit eşlemi Döndür** sayfa döndürme veya isotropic ölçeklendirme için iki parmağınızı kullanmanızı sağlar. Bit eşlemin her zaman doğru en boy oranını korur. Parmağınızı hareketini için her iki görevleri çok benzer olduğundan hem döndürme hem de anizotropik ölçeklendirme için iki parmağınızı kullanarak çok iyi çalışmaz.

Bu programa ilk büyük fark, isabet sınaması mantıksal ise. Önceki program kullanılan `Contains` yöntemi `SKRect` touch noktası karşılık gelen bit eşleme dönüştürülmüş dikdörtgen içinde olup olmadığını belirlemek için. Bit eşlem bit eşlem kullanıcı yönetir gibi olabilir, ancak döndürülmüş ve `SKRect` döndürülmüş bir dikdörtgen doğru şekilde temsil edemez. İsabet testi mantıksal yerine karmaşık analitik geometri durumda uygulamak gereken endişe.

Bununla birlikte, bir kısayol kullanılabilir: bir noktası dönüştürülen bir dikdörtgen sınırlar içinde yer alıyorsa belirleme aynıdır ters dönüştürülmüş noktanız dönüştürülmemiş dikdörtgenin sınırlar içinde yer alıyorsa belirleme. Bir çok daha kolay bir hesaplama ve mantıksal kullanışlı kullanmaya devam edebilirsiniz `Contains` yöntemi:

```csharp
public partial class BitmapRotationPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!touchDictionary.ContainsKey(args.Id))
                {
                    // Invert the matrix
                    if (matrix.TryInvert(out SKMatrix inverseMatrix))
                    {
                        // Transform the point using the inverted matrix
                        SKPoint transformedPoint = inverseMatrix.MapPoint(point);

                        // Check if it's in the untransformed bitmap rectangle
                        SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);

                        if (rect.Contains(transformedPoint))
                        {
                            touchDictionary.Add(args.Id, point);
                        }
                    }
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger rotate, scale, and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Find angles from pivot point to touch points
                        float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                        float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                        // Calculate rotation matrix
                        float angle = newAngle - oldAngle;
                        SKMatrix touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                        // Effectively rotate the old vector
                        float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                        oldVector.X = magnitudeRatio * newVector.X;
                        oldVector.Y = magnitudeRatio * newVector.Y;

                        // Isotropic scaling!
                        float scale = Magnitude(newVector) / Magnitude(oldVector);

                        if (!float.IsNaN(scale) && !float.IsInfinity(scale))
                        {
                            SKMatrix.PostConcat(ref touchMatrix,
                                SKMatrix.MakeScale(scale, scale, pivotPoint.X, pivotPoint.Y));

                            SKMatrix.PostConcat(ref matrix, touchMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }

    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
    ···
}
```

Mantığını `Moved` olay başlar gibi önceki program. Adlı iki vektör `oldVector` ve `newVector` önceki ve taşıma dokunmasına geçerli nokta ile unmoving parmak pivot noktasını alınarak hesaplanır. Ancak daha sonra bu vektörler açılarını belirlenir ve döndürme açısı farktır.

Eski vektör tabanlı üzerinde döndürme açısı döndürüldüğüne şekilde ölçeklendirme Ayrıca, söz konusu olabilecek. İki vektörün göreli büyüklüğünü Ölçeklendirme çarpanı sunulmuştur. Dikkat aynı `scale` değeri kullanılır yatay ve dikey ölçeklendirme, ölçeklendirme, isotropic. `matrix` Döndürme matris hem ölçek matris alanın ayarlandı.

Uygulamanızı touch uygulamanız gerekiyorsa, tek bir bit eşlem (veya diğer nesne) için işlem, bu üç örnekleri koddan kendi uygulamanız için uyarlayabilirsiniz. Ancak için birden çok bit eşlemlere işleme touch uygulamanız gerekiyorsa, büyük olasılıkla bu yalıtılacak istersiniz diğer sınıflar işlemlerinde touch.

## <a name="encapsulating-the-touch-operations"></a>Dokunma işlemlerini kapsülleme

**Düzenleme dokunma** sayfasında tek bir bit eşlemi, ancak daha yukarıda gösterilen mantığı kapsülleyen birkaç diğer dosya kullanarak touch düzenlenmesini gösterir. Bu dosyaların ilk [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) touch işleme gördüğünüz kodu tarafından gerçekleştirilen farklı türde belirten sabit listesi:

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly` Çeviri ile uygulanan bir parmak Sürükle olur. Sonraki tüm seçenekleri de kaydırma içerir ancak iki parmağınızı içerir: `IsotropicScale` eşit yatay ve dikey yönde ölçeklendirme nesne sonuçlanır tabletinizde işlemdir. `AnisotropicScale` eşit bir ölçeklendirme sağlar.

`ScaleRotate` Seçenektir iki parmak ölçeklendirme ve döndürme için. Ölçeklendirme isotropic. Parmak hareketleri temelde aynı olduğu için daha önce bahsedildiği gibi iki parmak döndürme anizotropik ölçeklendirme uygulayan sorunlu.

`ScaleDualRotate` Seçeneği bir parmak döndürme ekler. Böylece nesnenin merkezi sürükleyerek vektörü ile hizalanacak tek bir parmak nesneyi sürüklediğinde, sürüklenen nesnenin ilk merkezi geçici olarak döndürülmüştür.

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) dosya içeren bir `Picker` üyeleriyle `TouchManipulationMode` sabit listesi:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos.Transforms"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:TouchManipulationMode}">
                    <x:Static Member="local:TouchManipulationMode.None" />
                    <x:Static Member="local:TouchManipulationMode.PanOnly" />
                    <x:Static Member="local:TouchManipulationMode.IsotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.AnisotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.ScaleRotate" />
                    <x:Static Member="local:TouchManipulationMode.ScaleDualRotate" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>
        
        <Grid BackgroundColor="White"
              Grid.Row="1">
            
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

Alt bir `SKCanvasView` ve `TouchEffect` tek hücresine bağlı `Grid` kendisini kapsayan.

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) arka plan kod dosyasına sahip bir `bitmap` alan ancak değil türünü `SKBitmap`. Türü `TouchManipulationBitmap` (bir sınıf kısa bir süre içinde görürsünüz):

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            SKBitmap bitmap = SKBitmap.Decode(stream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Oluşturucu örnekleyen bir `TouchManipulationBitmap` nesnesinin oluşturucusuna bir `SKBitmap` katıştırılmış bir kaynaktan elde ettiğiniz. Oluşturucu ayarlayarak sonucuna `Mode` özelliği `TouchManager` özelliği `TouchManipulationBitmap` nesnenin bir üyesine `TouchManipulationMode` sabit listesi.

`SelectedIndexChanged` İşleyicisi `Picker` Ayrıca bu ayarlar `Mode` özelliği:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            bitmap.TouchManager.Mode = (TouchManipulationMode)picker.SelectedItem;
        }
    }
    ...
}
```

`TouchAction` İşleyicisi `TouchEffect` XAML dosyası çağrılarında iki metot örneği `TouchManipulationBitmap` adlı `HitTest` ve `ProcessTouchEvent`:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Varsa `HitTest` yöntemi döndürür `true` &mdash; nabzını bit eşlem tarafından kapladığı alanı ekranında dokunulan yani &mdash; touch ID eklemek `TouchIds` koleksiyonu. Parmak ekranından kaldırıncaya kadar bu kimliği bu parmak için dokunma olayların sırası temsil eder. Bit eşlem birden çok yola dokunma, ardından `touchIds` touch ID parmak her için koleksiyonu içerir.

`TouchAction` İşleyicisini de çağırır `ProcessTouchEvent` sınıfını `TouchManipulationBitmap`. Burada bazı (Tümü değil) gerçek dokunarak işleme gerçekleşir.

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) Sınıfı için bir sarmalayıcı sınıftır `SKBitmap` bit eşlem işlemek ve dokunma olayları işlemek için kod içerir. Daha fazla bilgi ile birlikte çalıştığını kodda genelleştirilmiş bir `TouchManipulationManager` (kısa bir süre içinde görürsünüz) sınıfı.

`TouchManipulationBitmap` Oluşturucusu kaydeder `SKBitmap` ve iki özellik başlatır `TouchManager` türünün özelliği `TouchManipulationManager` ve `Matrix` türünün özelliği `SKMatrix`:

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

Bu `Matrix` tüm touch etkinliğinden kaynaklanan birikmiş dönüştürme bir özelliktir. Gördüğünüz gibi her dokunma olay ardından ile birleştirilmiş bir matris içine çözülene `SKMatrix` tarafından depolanan değer `Matrix` özelliği.

`TouchManipulationBitmap` Nesne kendisi çizer kendi `Paint` yöntemi. Bağımsız değişken bir `SKCanvas` nesne. Bu `SKCanvas` zaten uygulanmış bir dönüştürme olabilir böylece `Paint` yöntemi art arda ekler `Matrix` özelliği mevcut dönüştürme için bit eşlem ile ilişkili ve tamamlandığında tuvaline geri yükler:

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

`HitTest` Yöntemi döndürür `true` kullanıcı ekrandaki bit eşlemin sınırları içinde bir noktadaki dokunursa. Bu, daha önce gösterilen giriş mantığı kullanır **bit eşlem döndürme** sayfası:

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

İkinci genel yöntem, `TouchManipulationBitmap` olduğu `ProcessTouchEvent`. Bu yöntem çağrıldığında, zaten touch olay bu belirli bit eşlemi ait kuruldu. Yöntem sözlüğü tutar [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) yalnızca önceki noktaya ve her dokunmasına yeni noktası olan nesneler:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Sözlük işte ve `ProcessTouchEvent` yöntemi:

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

İçinde `Moved` ve `Released` olaylar, yöntem çağrılarını `Manipulate`. Bu saatler `touchDictionary` birini veya daha fazlasını içeren `TouchManipulationInfo` nesneleri. Varsa `touchDictionary` içerir öğesi, bu büyük olasılıkla, `PreviousPoint` ve `NewPoint` değerleri eşit ve böylece parmağınızı hareketini temsil eder. Birden çok yola bit eşlem bitişik, sözlük birden fazla öğe içeriyor, ancak bu öğeler yalnızca birine sahip farklı `PreviousPoint` ve `NewPoint` değerleri. Geri kalan eşit olan `PreviousPoint` ve `NewPoint` değerleri.

Bu önemlidir: `Manipulate` yöntemi yalnızca bir parmak hareketini işliyor varsayabilirsiniz. Bu çağrının zaman diğer parmağınızı hiçbiri geçiş yapıyor ve bunlar gerçekten (büyük olasılıkla olduğu gibi) taşıyorsanız bu hareketleri gelecekteki çağrılar işlenir `Manipulate`.

`Manipulate` Yöntemi ilk kopyalar sözlük kolaylık sağlamak için bir dizi. İlk iki girişe dışında her şeyi yoksayar. Bit eşlemi yönlendirmek üzere ikiden fazla parmağınızı çalışıyorsanız, diğerleri yoksayılır. `Manipulate` Son üyesi olan `TouchManipulationBitmap`:

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

Tek bir parmak, bit eşlem düzenleme, `Manipulate` çağrıları `OneFingerManipulate` yöntemi `TouchManipulationManager` nesne. İki parmağınızı için çağırdığı `TwoFingerManipulate`. Bu yöntemlerinin bağımsız değişkenleri aynıdır: `prevPoint` ve `newPoint` bağımsız değişkenleri taşınıyor parmak temsil eder. Ancak `pivotPoint` bağımsız değişkeni iki çağrıları için farklıdır:

Bir parmak işleme için `pivotPoint` bit eşlem merkezidir. Bu bir parmak döndürme için izin vermektir. İki parmak işleme için olay yalnızca tek bir parmak hareketini gösterir. böylece `pivotPoint` değil taşıma parmak olduğu.

Her iki durumda da `TouchManipulationManager` döndürür bir `SKMatrix` yöntemi ile geçerli art arda ekler. değer, `Matrix` özelliği, `TouchManipulationPage` bit eşlem işlemek için kullanır.

`TouchManipulationManager` Genelleştirilmiş olduğundan ve başka hiçbir dosya dışındaki kullanan `TouchManipulationMode`. Uygulamalarınızda değişiklik olmadan bu sınıfı kullanmanız mümkün olabilir. Tek bir özellik türü tanımlayan `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Ancak, büyük olasılıkla önlemek isteyebilirsiniz `AnisotropicScale` seçeneği. Bir ölçekleme faktörü sıfır haline gelebilmesi bit eşlemi yönlendirmek için bu seçeneği ile oldukça kolaydır. Asla geri dönmemek üzere daha fazla görüş kayboluyor bit eşlem arar. Anizotropik ölçeklendirme gerçekten ihtiyacınız varsa, istenmeyen sonuçları önlemenize mantığını artırmak isteyebilirsiniz.

`TouchManipulationManager` vektör ancak olduğundan kullanır hiçbir `SKVector` SkiaSharp, yapısında `SKPoint` yerine kullanılır. `SKPoint` çıkarma işleci ve sonucu bir vektörü davranılıp destekler. Eklenmesi gereken yalnızca vektör özgü mantık bir `Magnitude` hesaplama:

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

Döndürme seçili olduğunda, bir parmak ve iki parmak yöntemleri döndürme ilk işleyin. Herhangi bir döndürme algılanırsa, döndürme bileşen etkili bir şekilde kaldırılır. Nelerin kaydırma ve ölçeklendirme olarak yorumlanır.

İşte `OneFingerManipulate` yöntemi. Bir parmak döndürme etkinleştirilmemiş sonra mantıksal basittir &mdash; yalnızca yeni noktası ve önceki noktaya adlı bir vektör oluşturmak için kullandığı `delta` çeviri için tam olarak karşılık gelir. Etkin bir parmak döndürme ile yöntemi açıları pivot noktası (bit eşlem Merkezi) yeni noktası ve önceki noktaya döndürme matrisi oluşturmak için kullanır:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

İçinde `TwoFingerManipulate` yöntemi pivot noktası olduğundan bu belirli touch etkinliğinde taşıma değil parmak konumu. Döndürme bir parmak dönüşü çok benzer ve ardından vektör adlı `oldVector` (önceki noktasında göre) döndürme için ayarlanır. Kalan taşıma ölçeklendirme olarak yorumlanır:

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

Bu yöntemde açıkça çeviri var. fark edeceksiniz. Bununla birlikte, her iki `MakeRotation` ve `MakeScale` yöntemleri pivot noktasında temel ve örtük çeviri dahildir. Bit eşlem ve aynı yönde sürükleyerek iki parmağınızı kullanıyorsanız `TouchManipulation` dokunma olayları iki parmağınızı arasında değişen bir dizi alırsınız. Diğer, ölçeklendirme veya döndürme sonuçları göre her parmak taşır ancak diğer parmak 's taşıma tarafından değilleme ve çeviri sonucudur.

Yalnızca kalan bölümü **düzenleme dokunma** sayfasıdır `PaintSurface` işleyicisinde `TouchManipulationPage` arka plan kod dosyası. Bu çağrı `Paint` yöntemi `TouchManipulationBitmap`, birikmiş touch etkinliği gösteren matris geçerlidir:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

`PaintSurface` İşleyici sonucuna görüntüleyerek bir `MatrixDisplay` birikmiş touch matris gösteren nesne:

[![](touch-images/touchmanipulation-small.png "Üçlü sayfasının ekran görüntüsü düzenleme dokunma")](touch-images/touchmanipulation-large.png#lightbox "Üçlü sayfasının ekran görüntüsü düzenleme dokunma")

## <a name="manipulating-multiple-bitmaps"></a>Birden çok bit eşlemler düzenleme

Sınıflardaki touch işleme kodu gibi yalıtma yararlarından biri `TouchManipulationBitmap` ve `TouchManipulationManager` birden çok bit eşlemler işlemek kullanıcıya izin veren bir program bu sınıfları yeniden yeteneğidir.

**Bit eşlem dağılımı görünümü** sayfasını nasıl yapıldığını gösterir. Türünde bir alan tanımlama yerine `TouchManipulationBitmap`, [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) sınıfı tanımlayan bir `List` bit eşlem nesnelerin:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                {
                    SKBitmap bitmap = SKBitmap.Decode(stream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

Oluşturucu tüm katıştırılmış kaynaklar olarak kullanılabilen bit eşlem yükler ve ekler `bitmapCollection`. Dikkat `Matrix` özelliğinin her başlatıldığı `TouchManipulationBitmap` her bit eşlem sol köşelerini 100 piksel kaydırılarak için nesne.

`BitmapScatterView` Sayfası için birden çok bit eşlemler dokunma olayları işlemek de gerekir. Tanımlama yerine bir `List` dokunarak kimliklerini şu anda yönetilen `TouchManipulationBitmap` nesneler, bu program bir sözlük gerektirir:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Bildirim nasıl `Pressed` mantıksal döngü `bitmapCollection` ters. Bit eşlemler genellikle birbirleriyle çakışmamalıdır. Daha sonra koleksiyondaki bit eşlemler, bit eşlemler koleksiyondaki üzerine görsel olarak yer. Ekrandaki tuşuna bastığında parmak altında birden çok bit eşlemler varsa, bu parmak tarafından yönetilen bir üstteki bir olmalıdır.

Ayrıca dikkat `Pressed` mantıksal görsel olarak diğer bir bit eşlem yığın üstüne hareket Bu, bit eşlem koleksiyonun sonuna taşır.

İçinde `Moved` ve `Released` olayları `TouchAction` işleyicisi çağrılarını `ProcessingTouchEvent` yönteminde `TouchManipulationBitmap` eski programın'olduğu gibi.

Son olarak, `PaintSurface` işleyicisi çağrılarını `Paint` yöntemi her `TouchManipulationBitmap` nesnesi:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

Kod, koleksiyonda döngüye girer ve bit eşlem koleksiyon başlangıcından sonuna yığını görüntüler:

[![](touch-images/bitmapscatterview-small.png "Üçlü sayfasının ekran görüntüsü bit eşlem dağılımı görünümü")](touch-images/bitmapscatterview-large.png#lightbox "Üçlü sayfasının ekran görüntüsü bit eşlem dağılımı görünümü")

## <a name="single-finger-scaling"></a>Tek-parmak ölçeklendirme

Bir ölçeklendirme işlemi, genellikle iki parmağınızı kullanarak tabletinizde hareket gerektirir. Ancak, bir bit eşlem köşelerini taşıma parmak sağlayarak tek Parmakla ölçeklendirme uygulamak mümkündür.

Bu gösterilmiştir **tek parmak köşe ölçek** sayfası. Bu örnek uygulanan, ölçeklendirmenin biraz farklı bir tür kullandığından `TouchManipulationManager` sınıfı, bu sınıf kullanmıyorsa veya `TouchManipulationBitmap` sınıfı. Bunun yerine, tüm touch mantığı arka plan kod dosyasıdır. Bir kerede yalnızca bir parmak izler ve yalnızca ekran temas herhangi bir ikincil parmağınızı yoksayar normalden biraz daha basit mantıksal olmasıdır.

[ **SingleFingerCornerScale.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) sayfa başlatır `SKCanvasView` oluşturur ve sınıfı bir `TouchEffect` dokunma olayları izleme nesnesi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.SingleFingerCornerScalePage"
             Title="Single Finger Corner Scale">

    <Grid BackgroundColor="White"
          Grid.Row="1">

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction"   />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

[ **SingleFingerCornerScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) dosya yükleyen bir bit eşlem kaynaktan **medya** dizin ve kullanarak görüntüler bir `SKMatrix` nesne olarak tanımlanan bir alan:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();
    ···

    public SingleFingerCornerScalePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.SetMatrix(currentMatrix);
        canvas.DrawBitmap(bitmap, 0, 0);
    }
    ···
}
```

Bu `SKMatrix` aşağıda gösterilen touch mantığı tarafından değiştirilmişse nesne.

Arka plan kod dosyasının kalan `TouchEffect` olay işleyicisi. Parmağınızı geçerli konumunu dönüştürerek başlayan bir `SKPoint` değeri. İçin `Pressed` eylem türü işleyicisi denetler başka bir parmak ekran temas ediyor ve parmak bit eşlem sınırları içinde olan.

Kodun önemli bir parçası olan bir `if` iki çağrıları içeren deyimi `Math.Pow` yöntemi. Bu matematik parmak konumu bit eşlem dolduran bir elips dışında olup olmadığını denetler. Bu nedenle, bir ölçeklendirme işlemi ise. Parmağınızı, bit eşlem köşelerini biridir ve zıt köşe olan pivot noktası belirlenir. İçinde bu elips parmak normal bir kaydırma işlemi şekildedir:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();

    // Information for translating and scaling
    long? touchId = null;
    SKPoint pressedLocation;
    SKMatrix pressedMatrix;

    // Information for scaling
    bool isScaling;
    SKPoint pivotPoint;
    ···

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Track only one finger
                if (touchId.HasValue)
                    return;

                // Check if the finger is within the boundaries of the bitmap
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = currentMatrix.MapRect(rect);
                if (!rect.Contains(point))
                    return;

                // First assume there will be no scaling
                isScaling = false;

                // If touch is outside interior ellipse, make this a scaling operation
                if (Math.Pow((point.X - rect.MidX) / (rect.Width / 2), 2) +
                    Math.Pow((point.Y - rect.MidY) / (rect.Height / 2), 2) > 1)
                {
                    isScaling = true;
                    float xPivot = point.X < rect.MidX ? rect.Right : rect.Left;
                    float yPivot = point.Y < rect.MidY ? rect.Bottom : rect.Top;
                    pivotPoint = new SKPoint(xPivot, yPivot);
                }

                // Common for either pan or scale
                touchId = args.Id;
                pressedLocation = point;
                pressedMatrix = currentMatrix;
                break;

            case TouchActionType.Moved:
                if (!touchId.HasValue || args.Id != touchId.Value)
                    return;

                SKMatrix matrix = SKMatrix.MakeIdentity();

                // Translating
                if (!isScaling)
                {
                    SKPoint delta = point - pressedLocation;
                    matrix = SKMatrix.MakeTranslation(delta.X, delta.Y);
                }
                // Scaling
                else
                {
                    float scaleX = (point.X - pivotPoint.X) / (pressedLocation.X - pivotPoint.X);
                    float scaleY = (point.Y - pivotPoint.Y) / (pressedLocation.Y - pivotPoint.Y);
                    matrix = SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);
                }

                // Concatenate the matrices
                SKMatrix.PreConcat(ref matrix, pressedMatrix);
                currentMatrix = matrix;
                canvasView.InvalidateSurface();
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = null;
                break;
        }
    }
}
```

`Moved` Eylem türü parmak basıldığında bu zamana kadar ekranın zamandan touch etkinliği karşılık gelen bir matris hesaplar. Bu, matris sunulmakta geçerli parmak ilk bit eşlem basılı zaman art arda ekler. Ölçeklendirme işlemi her zaman parmak dokunulan tek ters yönde köşe göredir.

Küçük veya dikdörtgen bit eşlemler iç elips ve bit eşlem çoğunu kaplayan küçük alanları bit eşlem ölçeklendirme köşelere bırakın. Biraz farklı bir yaklaşım tercih edebilirsiniz, bu durumda, tüm değiştirebilirsiniz `if` ayarlar blok `isScaling` için `true` Bu kod ile:

```csharp
float halfHeight = rect.Height / 2;
float halfWidth = rect.Width / 2;

// Top half of bitmap
if (point.Y < rect.MidY)
{
    float yRelative = (point.Y - rect.Top) / halfHeight;

    // Upper-left corner
    if (point.X < rect.MidX - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Bottom);
    }
    // Upper-right corner
    else if (point.X > rect.MidX + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Bottom);
    }
}
// Bottom half of bitmap
else
{
    float yRelative = (point.Y - rect.MidY) / halfHeight;

    // Lower-left corner
    if (point.X < rect.Left + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Top);
    }
    // Lower-right corner
    else if (point.X > rect.Right - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Top);
    }
}
```

Bu kod, bir iç Karo şekli bit eşleme alanı ve köşelere dört üçgenler etkili bir şekilde böler. Bu, çok büyük alanları alıp bit eşlem ölçeklendirme köşelere sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Etkileri olayları çağırma](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
