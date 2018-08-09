---
title: SkiaSharp bit eşlemler animasyon ekleme
description: Bit eşlem animasyon sırayla bir dizi bit eşlemler görüntüleme ve animasyonlu GIF dosyaları işleme tarafından yapmayı öğrenin.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: charlespetzold
ms.author: chape
ms.date: 07/12/2018
ms.openlocfilehash: 45a009757d84aa98acc41f6cd2bf672c8472c5bb
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615583"
---
# <a name="animating-skiasharp-bitmaps"></a>SkiaSharp bit eşlemler animasyon ekleme

SkiaSharp grafik genellikle animasyon uygulamaları `InvalidateSurface` üzerinde `SKCanvasView` sabit fiyat, genellikle her 16 milisaniye. Surface geçersiz kılmalarını çağrısı tetikleyen `PaintSurface` görüntü yeniden çizmek için işleyici. Görsellerin saniyede 60 kez yeniden düzenlenmiş olarak sorunsuz bir şekilde animasyon uygulanmasını görünür.

Ancak, grafik 16 milisaniye cinsinden oluşturulması çok karmaşıktır, animasyon dalgalanmalara hale gelebilir. Yenileme hızı 30 kata ya da ikinci kez 15 azaltmak Programcı seçebilirsiniz, ancak bazen bile yeterli değildir. Bazen bu nedenle bunlar yalnızca gerçek zamanlı olarak işlenemiyor, karmaşık grafiklerdir.

Animasyon, bit eşlemler bir dizi bireysel kareleri işleyerek animasyonun önceden hazırlama bir çözümdür. Animasyon görüntülemek için yalnızca bu bit eşlemler 60 kez ikinci bir sıralı olarak görüntülemek gereklidir. 

Elbette, potansiyel olarak çok sayıda bit eşlemler olmakla birlikte, nasıl büyük bütçe 3B animasyon filmler yapılır. 3B grafik gerçek zamanlı olarak işlenmek üzere daha çok karmaşık. Çok fazla işleme süresi, her karenin işlemek için gereklidir. Film izlerken gördüğünüz aslında bir bit eşlem dizisidir.

İçinde SkiaSharp benzer bir şey yapabilirsiniz. Bu makalede, bit eşlem animasyon iki tür gösterilmektedir. Animasyonun Mandelbrot kümenin ilk örnektir:

![Örnek hareketlendirme](animating-images/AnimatingSample.png "örnek animasyon ekleme")

İkinci örnek SkiaSharp animasyonlu GIF dosyası işlemek için nasıl kullanılacağını gösterir.

## <a name="bitmap-animation"></a>Bit eşlem animasyon

Mandelbrot görsel olarak verdiklerinden ancak computionally uzun kümesidir. (Mandelbrot kümesi ve burada kullanılan matematik tartışma için bkz: [Bölüm 20 _Xamarin.Forms ile Mobile Apps oluşturma_ ](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) 666 sayfasında başlatılıyor. Aşağıdaki açıklama, arka plan bilgileri varsayar.)

[ **Mandelbrot animasyon** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/) örnek bit eşlem animasyon Mandelbrot kümesindeki sabit bir nokta, sürekli bir yakınlaştırma benzetimini yapmak için kullanır. Yakınlaştırma uzaklaştırma takip eder ve ardından sonsuz veya programın sonlandırılması kadar döngü tekrarlanır. 

Program, bu uygulama yerel depolama alanında depolar 50 bit eşlemler kadar oluşturarak bu animasyon için hazırlar. Her bir bit eşlem önceki bit eşlem olarak yarım genişlik ve yükseklik karmaşık düzlemin kapsar. (Programda, integral temsil etmek için bu bit eşlemler söylenir _yakınlaştırma düzeyleri_.) Bit eşlemler, ardından sırayla görüntülenir. Ölçeklendirmeyi her bit eşlemin bir bit eşlem gelen kesintisiz ilerlemeyi diğerine sağlamak için bir animasyon görünür.

Son programın 20 bölüm'de açıklandığı gibi _Xamarin.Forms ile Mobile Apps oluşturma_, Mandelbrot kümesinde hesaplanması **Mandelbrot animasyon** sekiz ile zaman uyumsuz bir yöntem Parametreler. Parametreleri bir karmaşık merkez noktası ve genişlik ve yükseklik, merkez noktası çevresinde karmaşık düzlemin içerir. Sonraki üç piksel genişliği ve yüksekliği oluşturulacak bit eşlemin ve en yüksek bir özyinelemeli hesaplaması için yineleme sayısı parametrelerdir. `progress` Parametresi bu hesaplama ilerlemesini görüntülemek için kullanılır. `cancelToken` Parametresi, bu programa kullanılmaz:

```csharp
static class Mandelbrot
{
    public static Task<BitmapInfo> CalculateAsync(Complex center,
                                                  double width, double height,
                                                  int pixelWidth, int pixelHeight,
                                                  int iterations,
                                                  IProgress<double> progress,
                                                  CancellationToken cancelToken)
    {
        return Task.Run(() =>
        {
            int[] iterationCounts = new int[pixelWidth * pixelHeight];
            int index = 0;

            for (int row = 0; row < pixelHeight; row++)
            {
                progress.Report((double)row / pixelHeight);
                cancelToken.ThrowIfCancellationRequested();

                double y = center.Imaginary + height / 2 - row * height / pixelHeight;

                for (int col = 0; col < pixelWidth; col++)
                {
                    double x = center.Real - width / 2 + col * width / pixelWidth;
                    Complex c = new Complex(x, y);

                    if ((c - new Complex(-1, 0)).Magnitude < 1.0 / 4)
                    {
                        iterationCounts[index++] = -1;
                    }
                    // http://www.reenigne.org/blog/algorithm-for-mandelbrot-cardioid/
                    else if (c.Magnitude * c.Magnitude * (8 * c.Magnitude * c.Magnitude - 3) < 3.0 / 32 - c.Real)
                    {
                        iterationCounts[index++] = -1;
                    }
                    else
                    {
                        Complex z = 0;
                        int iteration = 0;

                        do
                        {
                            z = z * z + c;
                            iteration++;
                        }
                        while (iteration < iterations && z.Magnitude < 2);

                        if (iteration == iterations)
                        {
                            iterationCounts[index++] = -1;
                        }
                        else
                        {
                            iterationCounts[index++] = iteration;
                        }
                    }
                }
            }
            return new BitmapInfo(pixelWidth, pixelHeight, iterationCounts);
        }, cancelToken);
    }
}
```

Yöntem türü bir nesne döndürür `BitmapInfo` bir bit eşlem oluşturma hakkında bilgi sağlar:

```csharp
class BitmapInfo
{
    public BitmapInfo(int pixelWidth, int pixelHeight, int[] iterationCounts)
    {
        PixelWidth = pixelWidth;
        PixelHeight = pixelHeight;
        IterationCounts = iterationCounts;
    }

    public int PixelWidth { private set; get; }

    public int PixelHeight { private set; get; }

    public int[] IterationCounts { private set; get; }
}
```

**Mandelbrot animasyon** XAML dosyasını içeren iki `Label` görünümler, bir `ProgressBar`ve `Button` yanı sıra `SKCanvasView`:

```csharp
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="MandelAnima.MainPage"
             Title="Mandelbrot Animation">

    <StackLayout>
        <Label x:Name="statusLabel"
               HorizontalTextAlignment="Center" />
        <ProgressBar x:Name="progressBar" />

        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     Padding="5">
            <Label x:Name="storageLabel"
                   VerticalOptions="Center" />

            <Button x:Name="deleteButton"
                    Text="Delete All"
                    HorizontalOptions="EndAndExpand" 
                    Clicked="OnDeleteButtonClicked" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Arka plan kod dosyası üç önemli sabitleri ve bit eşlemler bir dizi tanımlayarak başlar:

```csharp
public partial class MainPage : ContentPage
{
    const int COUNT = 10;           // The number of bitmaps in the animation.
                                    // This can go up to 50!

    const int BITMAP_SIZE = 1000;   // Program uses square bitmaps exclusively

    // Uncomment just one of these, or define your own
    static readonly Complex center = new Complex(-1.17651152924355, 0.298520986549558);
    //   static readonly Complex center = new Complex(-0.774693089457127, 0.124226621261617);
    //   static readonly Complex center = new Complex(-0.556624880053304, 0.634696788141351);

    SKBitmap[] bitmaps = new SKBitmap[COUNT];   // array of bitmaps
    ···
}
```

Belirli bir noktada, büyük olasılıkla dönüştürmek istersiniz `COUNT` animasyon tam aralığını görmek için 50'ye değeri. Değerlerin 50 üzerinde kullanışlı değildir. 48 veya bunu yakınlaştırma düzeyini geçici olarak çift duyarlıklı kayan noktalı sayıların çözümleme Mandelbrot küme hesaplama için yeterli olur. Bu sorun 684 sayfasında açıklanan _Xamarin.Forms ile Mobile Apps oluşturma_.

`center` Değeri çok önemlidir. Animasyon yakınlaştırma odağını budur. Üç dosyayı bu üç son ekran görüntülerinde Bölüm 20 kullanılan değerler _Xamarin.Forms ile Mobile Apps oluşturma_ 684 sayfasında, ancak bu bölümde, kendi değerlerinizi biriyle görünmesi programı ile denemeler yapabilirsiniz. 

**Mandelbrot animasyon** örnek depolar bu `COUNT` bit eşlemler yerel uygulama depolama. Bu bit eşlemler kaplayan ne kadar depolama alanı bilmek isteyebilirsiniz bunu elli bit eşlemler üzerinde 20 megabayt Cihazınızda depolama gerektirir ve belirli bir noktada tüm bunları silmek isteyebilirsiniz. Bu iki yöntem alt kısmındaki amacı olan `MainPage` sınıfı:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void TallyBitmapSizes()
    {
        long fileSize = 0;

        foreach (string filename in Directory.EnumerateFiles(FolderPath()))
        {
            fileSize += new FileInfo(filename).Length;
        }

        storageLabel.Text = $"Total storage: {fileSize:N0} bytes";
    }

    void OnDeleteButtonClicked(object sender, EventArgs args)
    {
        foreach (string filepath in Directory.EnumerateFiles(FolderPath()))
        {
            File.Delete(filepath);
        }

        TallyBitmapSizes();
    }
}
```

Program bunları bellekte koruyacağından programın aynı söz konusu bit eşlemler hareketlendirme sırada, bit eşlemler yerel depolama alanındaki silebilirsiniz. Ancak program bir sonraki çalıştırmanızda bit eşlemleri yeniden oluşturmanız gerekir.

Yerel uygulama depolanan bit eşlemler birleştirmek `center` değeri, dosya adları, bunu değiştirirseniz `center` ayarlama, var olan bit eşlemler depolamada değiştirilmeyecek ve alanı kaplayan devam eder.

Yöntemleri şunlardır, `MainPage` dosya adları oluşturmak için kullandığı hem de bir `MakePixel` piksel değeri tanımlamak için yöntem tabanlı renk bileşenlerine:

```csharp
public partial class MainPage : ContentPage
{
    ···
    // File path for storing each bitmap in local storage
    string FolderPath() => 
        Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);

    string FilePath(int zoomLevel) => 
        Path.Combine(FolderPath(),
                     String.Format("R{0}I{1}Z{2:D2}.png", center.Real, center.Imaginary, zoomLevel));

    // Form bitmap pixel for Rgba8888 format
    uint MakePixel(byte alpha, byte red, byte green, byte blue) =>
        (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

`zoomLevel` Parametresi `FilePath` aralığı 0'dan `COUNT` eksi 1 sabit.

`MainPage` Oluşturucu çağrıları `LoadAndStartAnimation` yöntemi:

```csharp
public partial class MainPage : ContentPage
{
    ···
    public MainPage()
    {
        InitializeComponent();

        LoadAndStartAnimation();
    }
    ···
}
```

`LoadAndStartAnimation` Yöntemi, uygulama program daha önce çalıştırdığınızda, oluşturulmuş olabilir bit eşlemler yüklemek için yerel depolama erişimi için sorumludur. Aracılığıyla döngü `zoomLevel` değerleri 0'dan `COUNT`. Dosya varsa, içine yükler `bitmaps` dizisi. Aksi takdirde, için belirli bir bit eşlem oluşturmak için gereken `center` ve `zoomLevel` çağırarak değerleri `Mandelbrot.CalculateAsync`. Bu yöntem, bu yöntem renge dönüştürür her piksel yineleme sayısını alır:

```csharp
public partial class MainPage : ContentPage
{
    ···
    async void LoadAndStartAnimation()
    {
        // Show total bitmap storage
        TallyBitmapSizes();

        // Create progressReporter for async operation
        Progress<double> progressReporter =
            new Progress<double>((double progress) => progressBar.Progress = progress);

        // Create (unused) CancellationTokenSource for async operation
        CancellationTokenSource cancelTokenSource = new CancellationTokenSource();

        // Loop through all the zoom levels
        for (int zoomLevel = 0; zoomLevel < COUNT; zoomLevel++)
        {
            // If the file exists, load it
            if (File.Exists(FilePath(zoomLevel)))
            {
                statusLabel.Text = $"Loading bitmap for zoom level {zoomLevel}";

                using (Stream stream = File.OpenRead(FilePath(zoomLevel)))
                {
                    bitmaps[zoomLevel] = SKBitmap.Decode(stream);
                }
            }
            // Otherwise, create a new bitmap
            else
            {
                statusLabel.Text = $"Creating bitmap for zoom level {zoomLevel}";

                CancellationToken cancelToken = cancelTokenSource.Token;

                // Do the (generally lengthy) Mandelbrot calculation 
                BitmapInfo bitmapInfo =
                    await Mandelbrot.CalculateAsync(center,
                                                    4 / Math.Pow(2, zoomLevel),
                                                    4 / Math.Pow(2, zoomLevel),
                                                    BITMAP_SIZE, BITMAP_SIZE,
                                                    (int)Math.Pow(2, 10), progressReporter, cancelToken);

                // Create bitmap & get pointer to the pixel bits
                SKBitmap bitmap = new SKBitmap(BITMAP_SIZE, BITMAP_SIZE, SKColorType.Rgba8888, SKAlphaType.Opaque);
                IntPtr basePtr = bitmap.GetPixels();

                // Set pixel bits to color based on iteration count
                for (int row = 0; row < bitmap.Width; row++)
                    for (int col = 0; col < bitmap.Height; col++)
                    {
                        int iterationCount = bitmapInfo.IterationCounts[row * bitmap.Width + col];
                        uint pixel = 0xFF000000;            // black

                        if (iterationCount != -1)
                        {
                            double proportion = (iterationCount / 32.0) % 1;
                            byte red = 0, green = 0, blue = 0;

                            if (proportion < 0.5)
                            {
                                red = (byte)(255 * (1 - 2 * proportion));
                                blue = (byte)(255 * 2 * proportion);
                            }
                            else
                            {
                                proportion = 2 * (proportion - 0.5);
                                green = (byte)(255 * proportion);
                                blue = (byte)(255 * (1 - proportion));
                            }

                            pixel = MakePixel(0xFF, red, green, blue);
                        }

                        // Calculate pointer to pixel
                        IntPtr pixelPtr = basePtr + 4 * (row * bitmap.Width + col);

                        unsafe     // requires compiling with unsafe flag
                        {
                            *(uint*)pixelPtr.ToPointer() = pixel;
                        }
                    }

                // Save as PNG file
                SKData data = SKImage.FromBitmap(bitmap).Encode();

                try
                {
                    File.WriteAllBytes(FilePath(zoomLevel), data.ToArray());
                }
                catch
                {
                    // Probably out of space, but just ignore
                }

                // Store in array
                bitmaps[zoomLevel] = bitmap;

                // Show new bitmap sizes
                TallyBitmapSizes();
            }

            // Display the bitmap
            bitmapIndex = zoomLevel;
            canvasView.InvalidateSurface();
        }

        // Now start the animation
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }
    ···
}
```

Yerel uygulama depolaması yerine cihazın Fotoğraf Kitaplığı'ndaki program bu bit eşlemler depolar dikkat edin. Tanıdık kullanarak .NET Standard 2.0 kitaplığı sağlayan `File.OpenRead` ve `File.WriteAllBytes` bu görev için yöntemleri.

Bit eşlemler oluşturulmuş veya belleğe yüklenen sonra yöntem başlatır bir `Stopwatch` nesne ve çağrıları `Device.StartTimer`. `OnTimerTick` Her 16 milisaniye yöntemi çağrılır.

`OnTimerTick` hesaplar bir `time` 6000 süreleri 0'dan aralıkları değerini milisaniye cinsinden `COUNT`, her bir bit eşlem görüntüsü için altı saniye apportions. `progress` Değeri kullanan `Math.Sin` döneminin başlangıcında daha yavaş bir sinüzoidal animasyon oluşturmak için değer ve son olarak, daha yavaş yön tersine döner. 

`progress` Değeri 0'dan aralıkları `COUNT`. Tamsayı kısmını buna `progress` bir dizine olan `bitmaps` kesirli kısmını sırasında bir dizi `progress` , belirli bir bit eşlem bir yakınlaştırma düzeyini belirtir. Bu değerleri depolanan `bitmapIndex` ve `bitmapProgress` alanları ve tarafından görüntülenen `Label` ve `Slider` XAML dosyasında. `SKCanvasView` Bit eşlem görüntüsünü güncelleştirmek için geçersiz:

```csharp
public partial class MainPage : ContentPage
{
    ···
    Stopwatch stopwatch = new Stopwatch();      // for the animation
    int bitmapIndex;
    double bitmapProgress = 0;
    ···
    bool OnTimerTick()
    {
        int cycle = 6000 * COUNT;       // total cycle length in milliseconds

        // Time in milliseconds from 0 to cycle
        int time = (int)(stopwatch.ElapsedMilliseconds % cycle);

        // Make it sinusoidal, including bitmap index and gradation between bitmaps
        double progress = COUNT * 0.5 * (1 + Math.Sin(2 * Math.PI * time / cycle - Math.PI / 2));

        // These are the field values that the PaintSurface handler uses
        bitmapIndex = (int)progress;
        bitmapProgress = progress - bitmapIndex;

        // It doesn't often happen that we get up to COUNT, but an exception would be raised
        if (bitmapIndex < COUNT)
        {
            // Show progress in UI
            statusLabel.Text = $"Displaying bitmap for zoom level {bitmapIndex}";
            progressBar.Progress = bitmapProgress;

            // Update the canvas
            canvasView.InvalidateSurface();
        }

        return true;
    }
    ···
}
```

Son olarak, `PaintSurface` işleyicisi `SKCanvasView` en boy oranını koruyarak mümkün olduğunca büyük bit eşlem görüntülemek için bir hedef dikdörtgenin hesaplar. Kaynak dikdörtgenin dayanır `bitmapProgress` değeri. `fraction` Değeri 0 olduğunda aralığından burada hesaplanan `bitmapProgress` tüm bit eşlem 0,25 zaman görüntülemek için 0 `bitmapProgress` 1 yarım genişlik ve yükseklik etkili bir şekilde yakınlaştırma eşlemin görüntülemek için:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        if (bitmaps[bitmapIndex] != null)
        {
            // Determine destination rect as square in canvas
            int dimension = Math.Min(info.Width, info.Height);
            float x = (info.Width - dimension) / 2;
            float y = (info.Height - dimension) / 2;
            SKRect destRect = new SKRect(x, y, x + dimension, y + dimension);

            // Calculate source rectangle based on fraction:
            //  bitmapProgress == 0: full bitmap
            //  bitmapProgress == 1: half of length and width of bitmap
            float fraction = 0.5f * (1 - (float)Math.Pow(2, -bitmapProgress));
            SKBitmap bitmap = bitmaps[bitmapIndex];
            int width = bitmap.Width;
            int height = bitmap.Height;
            SKRect sourceRect = new SKRect(fraction * width, fraction * height, 
                                           (1 - fraction) * width, (1 - fraction) * height);

            // Display the bitmap
            canvas.DrawBitmap(bitmap, sourceRect, destRect);
        }
    }
    ···
}
```

Üç tüm platformlarda çalışan bir program şöyledir:

[![Mandelbrot animasyon](animating-images/MandelbrotAnimation.png "Mandelbrot animasyon")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>GIF animasyon

Grafik Değişim Biçimi (GIF) belirtimi, genellikle bir döngüde art arda görüntülenebilen sahnesi, birden çok sıralı çerçeve içeren tek bir GIF dosyası sağlayan bir özelliği içerir. Bu dosyalar olarak bilinen _Animasyonlu GIF'ler_. Animasyonlu GIF'ler Web tarayıcıları oynatabilirsiniz ve çerçeveleri bir animasyonlu GIF dosyasından ayıklamak için ve sıralı olarak görüntülemek için bir uygulama SkiaSharp sağlar.

[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) örnek adlı bir animasyonlu GIF kaynağı içeren **Newtons_cradle_animation_book_2.gif** DemonDeLuxe tarafından oluşturulan ve indirilen [Newton'ın Beşik ](https://en.wikipedia.org/wiki/Newton%27s_cradle) Wikipedia sayfasında. **Animasyonlu GIF** sayfası oluşturur ve bu bilgileri sağlayan bir XAML dosyası içeren bir `SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.AnimatedGifPage"
             Title="Animated GIF">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="GIF file by DemonDeLuxe from Wikipedia Newton's Cradle page"
               Grid.Row="1"
               Margin="0, 5"
               HorizontalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Arka plan kod dosyası, herhangi bir animasyonlu GIF dosyası yürütmek için genelleştirilmiş değil. Özellikle, kullanılabilir bilgilerden bazılarını bir yineleme sayısı ve yalnızca bir döngüye animasyonlu GIF yürütür yoksayar. 

Aşağıdaki kod açıklamasını normalden daha ayrıntılı şekilde animasyonlu GIF dosyası kareleri ayıklanacak SkisSharp kullanımını her yerden belgelenecektir görünmüyor:

Animasyonlu GIF dosyası kod çözme sayfanın oluşturucuda oluşur ve gerektiren `Stream` bit eşlem başvuran nesne oluşturmak için kullanılabilir bir `SKManagedStream` nesnesi ve ardından bir [ `SKCodec` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodec/) nesne. [ `FrameCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.FrameCount/) Özelliği animasyonu oluşturan çerçeve sayısını belirtir. 

Oluşturucu kullanması için bu kareler sonunda tek bit eşlemler kaydedilmiş `FrameCount` türünde bir dizi ayırmak için `SKBitmap` ikisinin yanı sıra `int` süresi (animasyon mantığı kolaylaştırmak için) ve her çerçeve için birikmiş diziler getirin süreleri.

[ `FrameInfo` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.FrameInfo/) Özelliği `SKCodec` sınıfı, bir dizi [ `SKCodecFrameInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodecFrameInfo/) değerler, her karenin, ancak bu program, yapısından gereken tek şey olan [ `Duration` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodecFrameInfo.Duration/) çerçevenin milisaniye cinsinden.

`SKCodec` adlı bir özellik [ `Info` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.Info/) türü [ `SKImageInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/), ancak bu `SKImageInfo` değeri belirtir (en az bu görüntü için) renk türü olduğunu `SKColorType.Index8`, anlamına her piksel renk türü bir dizindir. Renk tabloları ile rahatsız etme önlemek için program kullanan [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Width/) ve [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Height/) oluşturulacağına ilişkin bu yapıyı bilgilerinden tam renkli kendi `ImageInfo` değeri. Her `SKBitmap` değerinden oluşturulur.

`GetPixels` Yöntemi `SKBitmap` döndürür bir `IntPtr` , bit eşlemin piksel BITS başvuruyor. Bu piksel BITS henüz ayarlanmamış. Olduğunu `IntPtr` birine geçirilen [ `GetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCodec.GetPixels/p/SkiaSharp.SKImageInfo/System.IntPtr/SkiaSharp.SKCodecOptions/) yöntemlerinin `SKCodec`. Bu yöntem tarafından başvurulan bellek alanını GIF dosyasından çerçeve kopyalar `IntPtr`. [ `SKCodecOptions` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKCodecOptions.SKCodecOptions/p/System.Int32/System.Boolean/) Oluşturucusu çerçeve numarası gösterir:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;
    ···

    public AnimatedGifPage ()
    {
        InitializeComponent ();

        string resourceID = "SkiaSharpFormsDemos.Media.Newtons_cradle_animation_book_2.gif";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        using (SKCodec codec = SKCodec.Create(skStream))
        {
            // Get frame count and allocate bitmaps
            int frameCount = codec.FrameCount;
            bitmaps = new SKBitmap[frameCount];
            durations = new int[frameCount];
            accumulatedDurations = new int[frameCount];

            // Note: There's also a RepetitionCount property of SKCodec not used here

            // Loop through the frames
            for (int frame = 0; frame < frameCount; frame++)
            {
                // From the FrameInfo collection, get the duration of each frame
                durations[frame] = codec.FrameInfo[frame].Duration;

                // Create a full-color bitmap for each frame
                SKImageInfo imageInfo = code.new SKImageInfo(codec.Info.Width, codec.Info.Height);
                bitmaps[frame] = new SKBitmap(imageInfo);

                // Get the address of the pixels in that bitmap
                IntPtr pointer = bitmaps[frame].GetPixels();

                // Create an SKCodecOptions value to specify the frame
                SKCodecOptions codecOptions = new SKCodecOptions(frame, false);

                // Copy pixels from the frame into the bitmap
                codec.GetPixels(imageInfo, pointer, codecOptions);
            }

            // Sum up the total duration
            for (int frame = 0; frame < durations.Length; frame++)
            {
                totalDuration += durations[frame];
            }

            // Calculate the accumulated durations 
            for (int frame = 0; frame < durations.Length; frame++)
            {
                accumulatedDurations[frame] = durations[frame] + 
                    (frame == 0 ? 0 : accumulatedDurations[frame - 1]);
            }
        }
    }
    ···
}
```

Rağmen `IntPtr` değer, no `unsafe` çünkü kod gereklidir `IntPtr` hiçbir zaman bir C# işaretçi değerine dönüştürülür.

Her kare çıkarıldıktan sonra Oluşturucu tüm çerçeveleri sürelerini toplar ve ardından başka bir dizi birikmiş süreleri ile başlatır.

Arka plan kod dosyasına geri kalanında, animasyon için ayrılır. `Device.StartTimer` Yöntemi, bundan sonra bir zamanlayıcıyı başlatmak için kullanılır ve `OnTimerTick` geri çağırma kullanan bir `Stopwatch` geçen süreyi milisaniye cinsinden belirlemek için nesne. Döngü birikmiş süreleri dizi boyunca geçerli çerçeve bulmak yeterlidir:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;

    Stopwatch stopwatch = new Stopwatch();
    bool isAnimating;

    int currentFrame;
    ···
    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        int msec = (int)(stopwatch.ElapsedMilliseconds % totalDuration);
        int frame = 0;

        // Find the frame based on the elapsed time
        for (frame = 0; frame < accumulatedDurations.Length; frame++)
        {
            if (msec < accumulatedDurations[frame])
            {
                break;
            }
        }

        // Save in a field and invalidate the SKCanvasView.
        if (currentFrame != frame)
        {
            currentFrame = frame;
            canvasView.InvalidateSurface();
        }

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);
            
        // Get the bitmap and center it
        SKBitmap bitmap = bitmaps[currentFrame];
        canvas.DrawBitmap(bitmap,info.Rect, BitmapStretch.Uniform);
    }
}
```

Her zaman `currentframe` değişken değişiklikleri `SKCanvasView` geçersiz kılınır ve yeni çerçevenin görüntülenir:

[![Animasyonlu GIF](animating-images/AnimatedGif.png "animasyonlu GIF")](animating-images/AnimatedGif-Large.png#lightbox)

Elbette, programı çalıştırmak istersiniz kendiniz animasyon görmek için.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Mandelbrot animasyon (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/)
