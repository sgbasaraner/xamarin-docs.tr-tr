---
title: Dokunma işlemeleri
description: Bu makalede, matris dönüşümleri touch sürükleyerek, hareketinden ve döndürme uygulamak için kullanmayı açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/03/2018
ms.openlocfilehash: e2c1529980681ed1013c53343c2d077297352b95
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615398"
---
# <a name="touch-manipulations"></a>Dokunma işlemeleri

_Dokunma sürükleyerek, hareketinden ve döndürme uygulamak için kullanım matris dönüşümleri_

Bu mobil cihazlardaki gibi çok noktalı dokunma ortamlarında kullanıcılar kendi parmağınızı ekrandaki nesnelere işlemek için genellikle kullanın. Bir parmak sürükleyin ve iki parmak tabletinizde gibi ortak hareketleri taşıma ve nesneleri ölçeklendirme veya bile bunları döndür. Bu hareket, genellikle dönüştürme matrislerde kullanılarak uygulanır ve bu makalede bunu nasıl yapacağınız gösterilmektedir.

![](touch-images/touchmanipulationsexample.png "Çeviri, ölçeklendirme ve döndürme için tabi bir bit eşlem")

## <a name="manipulating-one-bitmap"></a>Bir bit eşlem düzenleme

**Düzenleme dokunma** sayfası üzerinde tek bir bit eşlem dokunma işlemeleri gösterir.
Bu örnek kullanır makalesinde sunulan touch izleme etkisinin [etkileri olayları çağırma](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Birkaç diğer dosya için destek sağlayan **düzenleme dokunma** sayfası. İlk [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) touch işleme gördüğünüz kodu tarafından gerçekleştirilen farklı türde belirten sabit listesi:

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

`ScaleRotate` Seçenektir iki parmak ölçeklendirme ve döndürme için. Ölçeklendirme isotropic. Parmak hareketleri temelde aynı olduğundan anizotropik ölçeklendirme iki parmak döndürme Uygulama sorunlu.

`ScaleDualRotate` Seçeneği bir parmak döndürme ekler. Böylece nesnenin merkezi sürükleyerek vektörü ile hizalanacak tek bir parmak nesneyi sürüklediğinde, sürüklenen nesnenin ilk merkezi geçici olarak döndürülmüştür.

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) dosya içeren bir `Picker` üyeleriyle `TouchManipulationMode` sabit listesi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
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
            <Picker.Items>
                <x:String>None</x:String>
                <x:String>PanOnly</x:String>
                <x:String>IsotropicScale</x:String>
                <x:String>AnisotropicScale</x:String>
                <x:String>ScaleRotate</x:String>
                <x:String>ScaleDualRotate</x:String>
            </Picker.Items>
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
            TouchManipulationMode mode;
            Enum.TryParse(picker.Items[picker.SelectedIndex], out mode);
            bitmap.TouchManager.Mode = mode;
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

`HitTest` Yöntemi döndürür `true` kullanıcı ekrandaki bit eşlemin sınırları içinde bir noktadaki dokunursa. Bit eşlem kullanıcı yönetir gibi bit eşlem Döndürülmüş veya hatta (birleşimi anizotropik ölçeklendirme ve döndürme) bir eğdiğinizde Paralel Kenar şeklinde olabilir. Endişe `HitTest` yöntemi yerine karmaşık analitik geometri bu durumda uygulama gerekiyor.

Ancak, bir kısayol bulunur:

Bir nokta dönüştürülen bir dikdörtgen sınırlar içinde yer alıyorsa belirleyen bir ters dönüştürülmüş noktası dönüştürülmemiş dikdörtgenin sınırlar içinde yer alıyorsa belirleme aynıdır. Bir çok daha kolay bir hesaplama ve kullanışlı kullanabilirsiniz `Contains` yöntemi tarafından tanımlanan `SKRect`:

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

Küçük veya dikdörtgen bit eşlemler iç elips ve bit eşlem çoğunu kaplayan köşelere bit eşlem ölçeklendirmek için çok küçük alanları bırakın. Biraz farklı bir yaklaşım tercih edebilirsiniz, bu durumda, tüm değiştirebilirsiniz `if` ayarlar blok `isScaling` için `true` Bu kod ile:

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

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Etkileri olayları çağırma](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
