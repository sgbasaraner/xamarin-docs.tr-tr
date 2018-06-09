---
title: Dokunma işlemeleri
description: Bu makalede, matris dönüşümleri dokunma sürükleyerek, çimdik ve döndürme uygulamak için nasıl kullanılacağını açıklar ve bu örnek kodu ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/03/2018
ms.openlocfilehash: a53fe287e74070adb22c2a7c67d4b7cc10b35d3e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244292"
---
# <a name="touch-manipulations"></a>Dokunma işlemeleri

_Dokunma sürükleyerek, çimdik ve döndürme uygulamak için kullanım matris dönüşümleri_

Mobil cihazlarda olanlar gibi çok dokunma ortamlarda kullanıcıları genellikle kendi parmakları ekrandaki nesneleri yönetmek için kullanın. Bir parmak sürükleyin ve iki parmak tutarak gibi ortak hareketleri taşıyabilir ve nesneleri ölçeklemek veya bile bunları döndür. Bu hareketleri, genellikle Dönüştürme Matrislerini kullanma uygulanır ve bu makalede, bunun nasıl yapılacağını gösterir.

![](touch-images/touchmanipulationsexample.png "Çeviri, ölçeklendirme ve döndürme tabi bir bit eşlem")

## <a name="manipulating-one-bitmap"></a>Bir bit eşlem düzenleme

**Touch işleme** sayfasında tek bir bit eşlem üzerinde dokunma işlemeleri gösterir.
Bu örnek makalesinde sunulan dokunma izleme etkisi kullanır [çağırma olayları etkileri](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Diğer bazı dosyaları için destek sağlayan **Touch işleme** sayfası. İlk [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) gördüğünüz kodu tarafından gerçekleştirilen dokunma işleme farklı türde gösteren numaralandırma:

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

`PanOnly` Çeviri ile uygulanan bir parmak Sürükle olur. Sonraki tüm seçenekleri de kaydırma içerir, ancak iki parmakları içerir: `IsotropicScale` eşit yatay ve dikey yönde ölçeklendirme nesnesindeki sonuçları tutarak işlemdir. `AnisotropicScale` eşit olmayan ölçeklendirme sağlar.

`ScaleRotate` Seçenektir iki parmak ölçekleme ve döndürme için. Ölçeklendirme isotropic. Parmak hareketleri temelde aynı olduğundan Eşyönsüz ölçeklendirme iki parmak döndürme Uygulama sorunlu oluşturur.

`ScaleDualRotate` Seçeneği bir parmak döndürme ekler. Tek bir parmak nesne sürüklendiğinde, böylece sürükleme vektörü ile nesnenin merkezi hizalanacak sürüklenen nesnenin ilk kendi merkezi etrafında döndürülür.

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) dosya içeren bir `Picker` üyeleriyle `TouchManipulationMode` numaralandırma:

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

Alt doğru bir `SKCanvasView` ve `TouchEffect` tek hücreye bağlı `Grid` , barındırır.

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) arka plan kod dosyasına var. bir `bitmap` alan ancak değil türü `SKBitmap`. Tür `TouchManipulationBitmap` (kısa süre içinde göreceksiniz bir sınıf):

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            SKBitmap bitmap = SKBitmap.Decode(skStream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Oluşturucusu başlatır bir `TouchManipulationBitmap` nesnesinin oluşturucusuna geçirerek, bir `SKBitmap` katıştırılmış bir kaynaktan alınan. Ayarlayarak Oluşturucusu sonucuna `Mode` özelliği `TouchManager` özelliği `TouchManipulationBitmap` üyesi nesnesine `TouchManipulationMode` numaralandırması.

`SelectedIndexChanged` İşleyicisi `Picker` de bu ayarlar `Mode` özelliği:

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

`TouchAction` İşleyicisine `TouchEffect` XAML dosyası çağrılarında öğesinde iki yöntem örneği `TouchManipulationBitmap` adlı `HitTest` ve `ProcessTouchEvent`:

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

Varsa `HitTest` yöntemi döndürür `true` &mdash; ekran tarafından bit eşlem kapladığı alanı içinde bir parmak işlemdeki anlamına &mdash; sonra da touch ID eklenen `TouchIds` koleksiyonu. Parmak ekranından kaldırır kadar bu kimliği bu parmak için touch olayların sırası temsil eder. Birden çok parmakları bit eşlem touch, sonra `touchIds` koleksiyonu, her parmak için touch ID içerir.

`TouchAction` İşleyici de çağırır `ProcessTouchEvent` sınıfını `TouchManipulationBitmap`. Bu yerdir bazı (ancak tüm) gerçek dokunarak işleme oluşur.

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) Sınıftır sarmalayıcı sınıfı için `SKBitmap` bit eşlem işlemek ve dokunma olayları işlemek için kod içerir. Daha fazla ile birlikte çalışır kodda genelleştirilmiş bir `TouchManipulationManager` (kısa süre içinde görürsünüz) sınıfı.

`TouchManipulationBitmap` Oluşturucusu kaydeder `SKBitmap` ve iki özellik başlatır `TouchManager` türündeki özelliği `TouchManipulationManager` ve `Matrix` türünde özellik `SKMatrix`:

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

Bu `Matrix` tüm dokunma etkinliğinden kaynaklanan birikmiş dönüştürme bir özelliktir. Göreceğiniz gibi her dokunma olay sonra ile birleştirilmiş bir matris içine çözülene `SKMatrix` tarafından depolanan değer `Matrix` özelliği.

`TouchManipulationBitmap` Nesne kendisi çizer kendi `Paint` yöntemi. Bağımsız değişken bir `SKCanvas` nesnesi. Bu `SKCanvas` uygulanmış bir dönüşüm zaten sahip olabilir böylece `Paint` yöntemi art arda ekler `Matrix` özelliği mevcut dönüştürmeye bit eşlem ile ilişkili ve tamamlandığında tuvale geri yükler:

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

`HitTest` Yöntemi döndürür `true` bit eşlem sınırları içinde bir noktada ekran kullanıcı etki ediyorsa. Bit eşlem kullanıcı yönetir gibi bit eşlem Döndürülmüş veya hatta (birlikte Eşyönsüz ölçekleme ve döndürme) bir paralel kenarı şeklinde olmalıdır. Endişe `HitTest` yöntemi yerine karmaşık analitik geometri bu durumda uygulama gerekiyor.

Ancak, kısayol kullanılabilir:

Bir noktayı dönüştürülmüş dikdörtgen sınırlar içinde yer alıyorsa belirleme ters dönüştürülmüş noktanız dönüştürülmemiş dikdörtgen sınırlar içinde yer alıyorsa belirleme aynıdır. Çok daha kolay hesaplama ve uygun kullanabilirsiniz `Contains` yöntemi tarafından tanımlanan `SKRect`:

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

İkinci genel yönteminde `TouchManipulationBitmap` olan `ProcessTouchEvent`. Bu yöntem çağrıldığında, zaten dokunma olay bu belirli bit eşlemi ait kuruldu. Sözlüğü yöntemi tutar [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) yalnızca önceki noktası ve her parmak yeni noktası nesneler:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Sözlük işte ve `ProcessTouchEvent` yöntemin kendisi:

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

İçinde `Moved` ve `Released` olaylar, yöntem çağrılarını `Manipulate`. Şu zamanlarda `touchDictionary` birini veya birkaçını içeriyor `TouchManipulationInfo` nesneleri. Varsa `touchDictionary` içeriyor öğesi, bu büyük olasılıkla, `PreviousPoint` ve `NewPoint` değerleri eşit olmayan ve bir parmak hareketini temsil eder. Birden çok parmakları bit eşlem temas, sözlük birden fazla öğe içeriyor, ancak bu öğeler yalnızca biri farklı varsa `PreviousPoint` ve `NewPoint` değerleri. Geri kalan eşit olan `PreviousPoint` ve `NewPoint` değerleri.

Bu önemlidir: `Manipulate` yöntemi yalnızca bir parmak hareketini işliyor varsayabilirsiniz. Bu çağrı aynı anda diğer parmakları hiçbiri taşıma ve bunlar gerçekten (büyük olasılıkla olduğu gibi) taşıyorsanız, bu hareketleri gelecekteki çağrıları işlenecek `Manipulate`.

`Manipulate` Yöntemi ilk kopyalar sözlük kolaylık sağlamak için bir dizi. İlk iki girişleri dışında her şeyi yoksayar. Bit eşlem işlemek ikiden fazla parmakları çalışıyorsanız, diğerleri yoksayılır. `Manipulate` Son üyesi `TouchManipulationBitmap`:

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

Tek bir parmak bitmap düzenleme durumunda `Manipulate` çağrıları `OneFingerManipulate` yöntemi `TouchManipulationManager` nesnesi. İki parmakları için çağırır `TwoFingerManipulate`. Bu yöntemlerin bağımsız değişkenlerinden aynıdır: `prevPoint` ve `newPoint` bağımsız değişkenleri taşınırken parmak temsil eder. Ancak `pivotPoint` bağımsız değişkeni, iki çağrıları için farklıdır:

Bir parmak işleme için `pivotPoint` bit eşlem merkezidir. Bu bir parmak dönüş izin vermektir. İki parmak işleme için olay yalnızca tek bir parmak hareketini gösterir. böylece `pivotPoint` değil taşıma parmak değil.

Her iki durumda da `TouchManipulationManager` döndürür bir `SKMatrix` geçerli yöntemi art arda ekler değeri `Matrix` özelliği, `TouchManipulationPage` bit eşlem işlemek için kullanır.

`TouchManipulationManager` Genelleştirilmiş ve dışındaki başka bir dosyaları kullanan `TouchManipulationMode`. Kendi uygulamalarında değişiklik olmadan bu sınıfı kullanmanız mümkün olabilir. Türü tek bir özelliğini tanımlar `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Ancak, büyük olasılıkla kaçının istersiniz `AnisotropicScale` seçeneği. Bit eşlem ölçeklendirme etkenlerden biri sıfır hale işlemek için bu seçeneği çok kolaydır. Hiçbir zaman döndürülecek görüş kayboluyor bit eşlem yapıyorsa. Eşyönsüz ölçeklendirme gerçekten ihtiyacınız varsa, istenmeyen sonuçları önlemek için mantığı geliştirmek istersiniz.

`TouchManipulationManager` vektörler, ancak olduğundan kullanır hiçbir `SKVector` SkiaSharp, yapısında `SKPoint` onun yerine kullanılır. `SKPoint` çıkarma işleci ve sonucu bir vektör olarak değerlendirilmesi destekler. Eklenecek gerekli yalnızca vektör özgü mantık bir `Magnitude` hesaplama:

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

Döndürme seçili olduğunda, bir parmak ve iki parmak işleme yöntemlerin her ikisi de döndürme ilk işleyin. Herhangi bir döndürme algılanırsa, döndürme bileşen etkili bir şekilde kaldırılır. Nelerin kaydırma ve ölçeklendirme olarak yorumlanır.

Burada `OneFingerManipulate` yöntemi. Bir parmak döndürme etkinleştirilmemiş sonra mantığı basittir &mdash; yalnızca yeni noktası ve önceki noktası adlı bir vektör oluşturmak için kullandığı `delta` tam olarak çeviri karşılık gelir. Etkin bir parmak döndürme ile yöntemi açıları pivot noktası (bit eşlem Merkezi) yeni noktası ve önceki noktası döndürme matrisi oluşturmak için kullanır:

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

İçinde `TwoFingerManipulate` pivot noktası yöntemidir bu belirli dokunma olay taşıma değil parmak konumu. Döndürme bir parmak döndürme çok benzer ve vektör adlı `oldVector` (önceki noktasında göre) döndürme için ayarlanır. Kalan taşıma ölçeklendirme olarak yorumlanır:

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

Bu yöntemde açık çeviri var. fark edeceksiniz. Bununla birlikte, her iki `MakeRotation` ve `MakeScale` yöntemleri Özet noktasında temel ve örtük çevirisi içerir. Bit eşlem ve aynı yönde sürükleyerek iki parmakları kullanıyorsanız `TouchManipulation` dokunma olayları iki parmakları arasında değişen bir dizi alırsınız. Diğer ölçekleme veya dönüş sonuçları göre her parmak taşır, ancak diğer parmak 's taşıma tarafından tasarruflarını ve çeviri sonucudur.

Yalnızca kalan kısmını **Touch işleme** sayfası `PaintSurface` işleyicisinde `TouchManipulationPage` arka plan kod dosyası. Bu çağrı `Paint` yöntemi `TouchManipulationBitmap`, birikmiş dokunma etkinliği temsil eden matris geçerlidir:

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

`PaintSurface` İşleyici sonucuna görüntüleyerek bir `MatrixDisplay` birikmiş dokunma matris gösteren nesne:

[![](touch-images/touchmanipulation-small.png "Üçlü sayfasının ekran görüntüsü Touch işleme")](touch-images/touchmanipulation-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Touch düzenleme")

## <a name="manipulating-multiple-bitmaps"></a>Birden çok bit eşlemler düzenleme

Dokunma işleme kodunda sınıflardaki gibi yalıtma yararları birini `TouchManipulationBitmap` ve `TouchManipulationManager` Çoklu bit eşlemler işlemek kullanıcı sağlayan bir program bu sınıfları yeniden yeteneğidir.

**Bit eşlem dağılım Görünüm** sayfa bunun nasıl yapılacağı gösterilmektedir. Türünde bir alan tanımlama yerine `TouchManipulationBitmap`, [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) sınıfı tanımlayan bir `List` bit eşlem nesnelerin:

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
                using (SKManagedStream skStream = new SKManagedStream(stream))
                {
                    SKBitmap bitmap = SKBitmap.Decode(skStream);
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

Oluşturucu yükler tüm katıştırılmış kaynaklar olarak kullanılabilir bit eşlemler ve eklenmektedir `bitmapCollection`. Dikkat `Matrix` özelliğinin başlatıldığı her `TouchManipulationBitmap` her bit eşlem sol üst köşesindeki 100 piksel kaydırmak için nesne.

`BitmapScatterView` Sayfa Çoklu bit eşlemler dokunma olayları işlemek de gerekir. Tanımlama yerine bir `List` dokunarak kimliklerini şu anda yönetilen `TouchManipulationBitmap` nesneler, bu program için bir sözlük gereklidir:

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

Bildirim nasıl `Pressed` mantığı döngü `bitmapCollection` ters. Bit eşlemler genellikle birbirinin. Bit eşlemler daha sonra koleksiyonundaki görsel olarak bit eşlemler koleksiyondaki en üstünde yer. En üstteki bir ekranda bastığında parmak altında birden çok bit eşlemler varsa, bu parmak tarafından yönetilen bir olmalıdır.

Ayrıca, fark `Pressed` mantığı diğer bit eşlemler yığını dön görsel olarak hareket Bu, bit eşlem Topluluğun sonuna taşır.

İçinde `Moved` ve `Released` olayları `TouchAction` işleyicisi çağrılarını `ProcessingTouchEvent` yönteminde `TouchManipulationBitmap` önceki program'olduğu gibi.

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

Kod koleksiyonunda döngü ve bit eşlemler koleksiyonunun başından sonuna yığını görüntüler:

[![](touch-images/bitmapscatterview-small.png "Üçlü sayfasının ekran görüntüsü bit eşlem dağılım Görünüm")](touch-images/bitmapscatterview-large.png#lightbox "Üçlü sayfasının ekran görüntüsü bit eşlem dağılım görünümü")

## <a name="single-finger-scaling"></a>Tek parmak ölçeklendirme

Bir ölçeklendirme işlemi genellikle iki parmakları kullanarak tutarak hareketi gerektirir. Ancak, bir bit eşlem köşelerinde taşıma parmak sağlayarak ile tek bir parmak ölçeklendirme uygulamak mümkündür.

Bu, gösterilmiştir **tek parmak köşe ölçek** sayfası. Bu örnek, uygulanan olduğunu ölçeklendirme bir biraz farklı tür kullandığından `TouchManipulationManager` sınıfı, o sınıfın kullanmaz veya `TouchManipulationBitmap` sınıfı. Bunun yerine, tüm dokunma mantığı arka plan kodu dosyasıdır. Aynı anda yalnızca tek bir parmak izler ve yalnızca ekran temas tüm ikincil parmakları yoksayar normalden biraz daha basit mantığı olmasıdır.

[ **SingleFingerCornerScale.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) sayfa başlatır `SKCanvasView` sınıfı ve oluşturan bir `TouchEffect` dokunma olayları izlemek için nesne:

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

[ **SingleFingerCornerScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) dosyasını yükler bir bit eşlem kaynaktan **medya** dizin ve kullanarak görüntüler bir `SKMatrix` nesne olarak tanımlı bir alan:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

Bu `SKMatrix` aşağıda gösterilen dokunma mantığı tarafından nesne değiştirdi.

Arka plan kod dosyasının kalan `TouchEffect` olay işleyicisi. Parmak geçerli konumunu dönüştürerek başlayan bir `SKPoint` değeri. İçin `Pressed` eylem türü işleyici denetler başka bir parmak ekran değecek ve parmak bit eşlem sınırları içinde olduğunu.

Kodun önemli bir parçası olan bir `if` iki çağrıları içeren deyimi `Math.Pow` yöntemi. Bu matematik parmak konumun bit eşlem doldurur elips dışında olup olmadığını denetler. Bu nedenle, bir ölçeklendirme işlemi ise. Parmak bit eşlem köşelerinde biridir ve bir pivot noktası karşı köşe olan belirlenir. Parmak bu elips içinde ise, normal bir kaydırma işlemi.:

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

`Moved` Eylem türü parmak basılı bu zamana kadar ekran zamandan dokunma etkinlik karşılık gelen bir matris hesaplar. Bu matris ile bu Matris yürürlükte parmak ilk bit eşlem basılı aynı anda art arda ekler. Ölçeklendirme işlemi parmak işlemdeki bir ters yönde köşe her zaman görelidir.

Küçük veya dikdörtgen bit eşlemler iç elips ve bit eşlem çoğunu kaplar bit eşlem ölçeklendirmek için köşelerinde çok küçük alanları bırakın. Biraz farklı bir yaklaşım tercih edebilirsiniz, bu durumda, tüm değiştirebilirsiniz `if` ayarlar blok `isScaling` için `true` Bu kod:

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

Bu kod bir iç baklava şekli içine bit eşlem alanını ve dört üçgenler köşelerinde etkili bir şekilde böler. Bu kadar büyük alanları alın ve bit eşlem ölçeklendirmek için köşelerinde sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Olayları etkileri çağırma](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
