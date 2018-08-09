---
title: SkiaSharp bit eşlemler görüntüleme
description: Piksel, bit eşlemler boyutları ve en boy oranını koruyarak dikdörtgenler doldurmak için Genişletilebilir SkiaSharp görüntüleme hakkında bilgi edinin.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: cbe3166c4edb147f7179f2c719901b382db8ec80
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615320"
---
# <a name="displaying-skiasharp-bitmaps"></a>SkiaSharp bit eşlemler görüntüleme

SkiaSharp bit eşlem konu makalesinde sunulmuştur  **[SkiaSharp bit eşlem temellerini](../basics/bitmaps.md)**. Bu makalede, üç yöntemi yük bit eşlemler ve bit eşlemler görüntülemek için üç yol göstermiştir. Bu makalede, bit eşlemler yüklemek için teknik incelemeleri ve daha ayrıntılı kullanımını giden `DrawBitmap` yöntemlerinin `SKCanvas`.

![Örnek görüntüleme](displaying-images/DisplayingSample.png "örnek görüntüleme")

`DrawBitmapLattice` Ve `DrawBitmapNinePatch` yöntemleri makalesinde açıklanan  **[bölümlenmiş görünen SkiaSharp bit eşlem](segmented.md)**.

Bu sayfadaki örneklerdir **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** uygulama. Bu uygulamanın giriş sayfasından **SkiaSharp bit eşlemler**ve ardından **görüntüleme bit eşlemler** bölümü.

## <a name="loading-a-bitmap"></a>Bir bit eşlem yükleniyor

SkiaSharp uygulaması tarafından genel olarak kullanılan bir bit eşlem üç farklı kaynaklardan birinden gelir:

- Örneklerimizde Internet
- Yürütülebilir dosya katıştırılmış bir kaynaktan
- Kullanıcının Fotoğraf Kitaplığı'ndan

Ayrıca, bir SkiaSharp uygulamanın yeni bir bit eşlem oluşturma üzerinde çizin veya bit eşlem bitleri algorithmically ayarlamak için de mümkündür. Bu teknik makalelerinde açıklanan **[oluşturma ve SkiaSharp bit eşlemler üzerinde çizim](drawing.md)** ve **[erişme SkiaSharp bit eşlem piksel](pixel-bits.md)**.

Bir bit eşlem yükleme aşağıdaki üç kod örneklerinde, sınıf türünde bir alan içeren varsayılır `SKBitmap`:

```csharp
SKBitmap bitmap;
```

Makale olarak **[SkiaSharp bit eşlem temellerini](../basics/bitmaps.md)** belirtildiği gibi Internet üzerinden bir bit eşlem yüklemek için en iyi yolu olan [ `HttpClient` ](xref:System.Net.Http.HttpClient) sınıfı. Tek bir sınıf örneği, bir alan olarak tanımlanabilir:

```csharp
HttpClient httpClient = new HttpClient();
```

Kullanırken `HttpClient` iOS ve Android uygulamaları ile şirket belgelerinde açıklandığı gibi proje özelliklerini ayarlamak da istersiniz  **[Aktarım Katmanı Güvenliği (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)**.

Kullanan kod `HttpClient` genellikle içerir `await` içinde bulunmalıdır şekilde işleci bir `async` yöntemi:

```csharp
try
{
    using (Stream stream = await httpClient.GetStreamAsync("https:// ··· "))
    using (MemoryStream memStream = new MemoryStream())
    {
        await stream.CopyToAsync(memStream);
        memStream.Seek(0, SeekOrigin.Begin);

        bitmap = SKBitmap.Decode(stream);
        ···
    };
}
catch
{
    ···
}
```

Dikkat `Stream` nesne öğesinden alınan `GetStreamAsync` kopyalanır bir `MemoryStream`. Android izin vermiyor `Stream` gelen `HttpClient` bulunan ana iş parçacığı dışında zaman uyumsuz yöntemler tarafından işlenecek. 

[ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/System.IO.Stream/) Birçok iş yapar: `Stream` geçirilen nesne ortak bit eşlem dosyası biçimleri, genellikle JPEG, PNG veya GIF biriyle tamamını bir bit eşlem içeren bir bellek bloğunu başvuruyor. `Decode` Yöntemi gerekir biçimi belirleyin ve ardından SkiaSharp'ın kendi iç bit eşlem biçime bit eşlem dosyası kod çözme.

Kod çağrılarınızı sonra `SKBitmap.Decode`, büyük olasılıkla kılacak `CanvasView` böylece `PaintSurface` işleyicisi yeni yüklenen bit eşlem görüntüleyebilirsiniz.

İkinci bir bit eşlem yük şekilde bit eşlem .NET Standard Kitaplığı'nda katıştırılmış bir kaynağı olarak ekleyerek ayrı bir platform projeleri tarafından başvuruluyor. Bir kaynak kimliği geçirildiğinde [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) yöntemi. Bu kaynak kimliği, derleme adı, klasör adı ve dosya adı noktalarla ayrılmış kaynak oluşur:

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

Bit eşlem dosyaları, iOS, Android ve evrensel Windows Platformu (UWP) için ayrı bir platform projesinde kaynaklar olarak da depolanabilir. Ancak, bu bit eşlemler yüklenirken platform projesinde bulunan kod gerektirir.

Kullanıcının Resim Kitaplığı'ndan bir bit eşlem almak için üçüncü bir yaklaşımdır. Aşağıdaki kodu yer aldığı bir bağımlılık hizmeti kullanan **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** uygulama. **SkiaSharpFormsDemo** .NET Standard kitaplığı içeren `IPhotoLibrary` arabirim platformu projelerin her biri içerirken bir `PhotoLibrary` arabirimi uygulayan bir sınıf.

```csharp
IPhotoicturePicker picturePicker = DependencyService.Get<IPhotoLibrary>();

using (Stream stream = await picturePicker.GetImageStreamAsync())
{
    if (stream != null)
    {
        bitmap = SKBitmap.Decode(stream);
        ···
    }
}
```

Genellikle, bu kod ayrıca çıkarır `CanvasView` böylece `PaintSurface` işleyicisi, yeni bir bit eşlem görüntüleyebilirsiniz.

`SKBitmap` Sınıfı dahil olmak üzere çeşitli kullanışlı özellikler tanımlar [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) ve [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/), dahil olmak üzere birçok yöntem yanı sıra, bit eşlemin piksel boyutunu göster bit eşlemler, bunları kopyalamak ve piksel BITS kullanıma sunmak için oluşturmak için yöntemleri. 

## <a name="displaying-in-pixel-dimensions"></a>Piksel boyutları olarak görüntüleme

SkiaSharp [ `Canvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) sınıfı tanımlar dört `DrawBitmap` yöntemleri. Bu yöntemler, tamamen farklı iki şekilde görüntülenecek bit eşlemleri izin ver: 

- Belirten bir `SKPoint` değeri (veya ayrı `x` ve `y` değerleri) bit eşlemin piksel boyutlarıyla görüntüler. Bit eşlemin piksel doğrudan görüntü piksele eşlenir.
- Bir dikdörtgen belirterek boyutu ve şekli dikdörtgenin uzatılmış bit eşlem neden olur. 

Bir bit eşlem piksel boyutlarıyla kullanarak görüntü [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKPoint/SkiaSharp.SKPaint/) ile bir `SKPoint` parametresi veya [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) ayrı ile `x` ve `y` parametreleri:

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

Bu iki yöntem işlevsel olarak aynıdır. Belirtilen nokta bit eşlem tuvali göre sol üst köşesinin konumunu gösterir. Küçük bit eşlemler genellikle Mobil cihazların piksel çözünürlüğü kadar yüksek olduğundan, bu cihazlar üzerinde oldukça küçük görünür.

İsteğe bağlı `SKPaint` parametresi, karışım modlarının kullanarak bit eşlem görüntülemek veya filtre efektleri olanak sağlar. Bu, sonraki makalelerinde açıklanacaktır.

**Piksel boyutları** sayfasını **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** örnek program 320 piksel genişliğinde 240 piksel yüksekliğinde bir bit eşlem kaynağı görüntüler:

```csharp
public class PixelDimensionsPage : ContentPage
{
    SKBitmap bitmap;

    public PixelDimensionsPage()
    {
        Title = "Pixel Dimensions";

        // Load the bitmap from a resource
        string resourceID = "SkiaSharpFormsDemos.Media.Banana.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        // Create the SKCanvasView and set the PaintSurface handler
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

        float x = (info.Width - bitmap.Width) / 2;
        float y = (info.Height - bitmap.Height) / 2;

        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

`PaintSurface` İşleyici hesaplayarak bit eşlem merkezleri `x` ve `y` değerler temel uzaklaştırabilir piksel boyutunu ve bit eşlemin piksel boyutları:

[![Piksel boyutları](displaying-images/PixelDimensions.png "piksel boyutları")](displaying-images/PixelDimensions-Large.png#lightbox)

Bit eşlem, sol üst köşesinde görüntülenecek uygulama isterse yalnızca koordinatlarını geçip geçmeyeceğini (0, 0). 

## <a name="a-method-for-loading-resource-bitmaps"></a>Kaynak bit eşlemleri yükleme yöntemi

Çok yakında örnekleri bit eşlem kaynakları yüklemek gerekecektir. Statik `BitmapExtensions` sınıfını **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** çözüm yardımcı olması için bir yöntem içerir:

```csharp
static class BitmapExtensions
{
    public static SKBitmap LoadBitmapResource(Type type, string resourceID)
    {
        Assembly assembly = type.GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            return SKBitmap.Decode(stream);
        }
    }
    ···
}
```

Bildirim `Type` parametresi. Bu, `Type` bit eşlem kaynağındaki depolayan derlemedeki herhangi bir türü ile ilişkili nesne.

Bu `LoadBitmapResource` bit eşlem kaynakları gerektiren tüm sonraki örnekleri yöntemi kullanılır.

## <a name="stretching-to-fill-a-rectangle"></a>Bir dikdörtgen doldurmak için uzatma

`SKCanvas` Sınıfı da tanımlar bir [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) dikdörtgen ve başka bir bit eşlem işleyen yöntemi [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) bit eşleme dikdörtgen bir kısmını işleyen yöntemi bir Dikdörtgen:

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

Her iki durumda da, bit eşlem adlı dikdörtgen dolduracak şekilde genişletilir `dest`. İkinci yöntemde, `source` dikdörtgen bit eşlemin bir alt seçmenize olanak sağlar. `dest` Dikdörtgen olan çıktı cihazına göre; `source` dikdörtgen olan bit eşlem göre.

**Dolgu dikdörtgen** sayfası aynı bit eşlem göstererek bu yöntemlerin ilki olan iki kullanılan bir dikdörtgen önceki örnekte aynı boyutta tuval gösterir: 

```csharp
public class FillRectanglePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public FillRectanglePage ()
    {
        Title = "Fill Rectangle";

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

        canvas.DrawBitmap(bitmap, info.Rect);
    }
}
```

Yeni kullanımına dikkat edin `BitmapExtensions.LoadBitmapResource` ayarlanacak yöntemi `SKBitmap` alan. Hedef dikdörtgenin elde edilen [ `Rect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Rect/) özelliği `SKImageInfo`, hangi desribes uzaklaştırabilir boyutu:

[![Dikdörtgen doldurun](displaying-images/FillRectangle.png "dikdörtgen doldurun")](displaying-images/FillRectangle-Large.png#lightbox)

Bu genellikle, _değil_ ne istiyorsunuz. Görüntüyü farklı yatay ve dikey yönde esnetilmiş tarafından bozulmuş. Genellikle bir bit eşlem piksel boyutuna dışında bir görüntülenirken eşleşmemin özgün en boy oranını korumak istiyorsunuz.

## <a name="stretching-while-preserving-the-aspect-ratio"></a>En boy oranını koruyarak uzatma

En boy oranını koruyarak bir bit eşlem uzatma olduğundan bir işlem olarak da bilinen _Tekdüzen ölçeklendirme_. Bu terim, algoritmik bir yaklaşım önerir. Olası bir çözüm gösterilir **Tekdüzen ölçeklendirme** sayfası:

```csharp
public class UniformScalingPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(UniformScalingPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public UniformScalingPage()
    {
        Title = "Uniform Scaling";

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

        float scale = Math.Min((float)info.Width / bitmap.Width, 
                               (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect destRect = new SKRect(x, y, x + scale * bitmap.Width, 
                                           y + scale * bitmap.Height);

        canvas.DrawBitmap(bitmap, destRect);
    }
}
```

`PaintSurface` İşleyici hesaplar bir `scale` görüntüsü genişliği ve yüksekliği için bit eşlem genişliği ve yüksekliği oranını en az faktörü. `x` Ve `y` değerleri ardından hesaplanabilir ölçeklendirilmiş bir bit eşlem görüntüsü genişliği ve yüksekliği içinde ortalama için. Hedef dikdörtgenin bir sol üst köşesinde sahip `x` ve `y` ve bu değerleri artı ölçeği genişliği sağ alt köşesindeki ve bit eşlem yüksekliği:

[![Tekdüzen ölçeklendirme](displaying-images/UniformScaling.png "Tekdüzen ölçeklendirme")](displaying-images/UniformScaling-Large.png#lightbox)

Bu alanı esnetilmiş bit eşlem görmek için telefonu yan çevirerek açın:

[![Yatay ölçeklendirme Tekdüzen](displaying-images/UniformScaling-Landscape.png "yatay Tekdüzen ölçeklendirme")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

Bu değeri kullanmanın avantajı `scale` biraz farklı bir algoritma uygulamak istediğinizde faktörü belirgin olur. Bit eşleşmemin en boy oranını korumak ancak aynı zamanda hedef dikdörtgenin dolgu istediğinizi varsayalım. Bu, olası yalnızca görüntünün kırparak yoludur, ancak yalnızca değiştirerek, bu algoritmayı uygulayabileceğiniz `Math.Min` için `Math.Max` Yukarıdaki kod. Sonuç şu şekildedir: 

[![Tekdüzen bir ölçeklendirme alternatif](displaying-images/UniformScaling-Alternative.png "alternatif Tekdüzen ölçeklendirme")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

Bit eşleşmemin en boy oranı korunur ancak alanları sola ve sağa bit eşlemin kırpılır.

## <a name="a-versatile-bitmap-display-function"></a>Çok yönlü bir bit eşlem görünen işlevi

XAML tabanlı programlama ortamlarında (örneğin, UWP ve Xamarin.Forms) genişletmek veya kendi en boy oranlarını koruma sırasında bit eşlemler boyutunu küçültmek için bir olanağı vardır. SkiaSharp bu özelliği içermediği, ancak kendiniz uygulayabilirsiniz. `BitmapExtensions` Sınıfı dahil [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) uygulama gösterir nasıl. Sınıfı iki yeni tanımlar `DrawBitmap` en boy oranını hesaplamayı gerçekleştiren yöntemler. Genişletme yöntemleri, bu yeni yöntemlerdir `SKCanvas`.

Yeni `DrawBitmap` yöntemleri türünde bir parametre içerir `BitmapStretch`, tanımlanan numaralandırma **BitmapExtensions.cs** dosyası:

```csharp
public enum BitmapStretch
{
    None,
    Fill,
    Uniform,
    UniformToFill,
    AspectFit = Uniform,
    AspectFill = UniformToFill
}
```

`None`, `Fill`, `Uniform`, Ve `UniformToFill` üyeleridir UWP de aynı [ `Stretch` ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) sabit listesi. Benzer Xamarin.Forms [ `Aspect` ](xref:Xamarin.Forms.Aspect) numaralandırma üyeleri tanımlar `Fill`, `AspectFit`, ve `AspectFill`.

**Tekdüzen ölçeklendirme** yukarıda gösterilen sayfasında merkezleri dikdörtgenin içindeki bit eşlemi, ancak sol veya sağ tarafında dikdörtgen veya üstüne veya altına bir bit eşlem konumlandırma gibi diğer seçenekleri isteyebilirsiniz. Amacı olan `BitmapAlignment` sabit listesi:

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

Hizalama ayarları etkisiz ile kullanıldığında `BitmapStretch.Fill`.

İlk `DrawBitmap` hedef dikdörtgene ancak hiçbir kaynak dikdörtgenin uzantı işlevi içerir. Varsayılanları ortalanmış bit eşlem istiyorsanız, yalnızca belirttiğiniz böylece tanımlanmış bir `BitmapStretch` üye:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect dest, 
                                  BitmapStretch stretch, 
                                  BitmapAlignment horizontal = BitmapAlignment.Center, 
                                  BitmapAlignment vertical = BitmapAlignment.Center, 
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * bitmap.Width, scale * bitmap.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, display, paint);
        }
    }
    ···
}
```

Adlı bir Ölçeklendirme çarpanı hesaplamak için bu yöntem birincil amacı olan `scale` sonra uygulanan bit eşlem genişliği ve yüksekliği için çağrılırken `CalculateDisplayRect` yöntemi. Bu hesaplar için yatay ve dikey hizalamayı temel alarak bit eşlem görüntüleyen bir dikdörtgen yöntemi.

```csharp
static class BitmapExtensions
{
    ···
    static SKRect CalculateDisplayRect(SKRect dest, float bmpWidth, float bmpHeight, 
                                       BitmapAlignment horizontal, BitmapAlignment vertical)
    {
        float x = 0;
        float y = 0;

        switch (horizontal)
        {
            case BitmapAlignment.Center:
                x = (dest.Width - bmpWidth) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                x = dest.Width - bmpWidth;
                break;
        }

        switch (vertical)
        {
            case BitmapAlignment.Center:
                y = (dest.Height - bmpHeight) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                y = dest.Height - bmpHeight;
                break;
        }

        x += dest.Left;
        y += dest.Top;

        return new SKRect(x, y, x + bmpWidth, y + bmpHeight);
    }
}
```

`BitmapExtensions` Sınıfı içeren ek bir `DrawBitmap` bit eşlem kümesini belirtmek için bir kaynak dikdörtgen yöntemi. Ölçeklendirme çarpanı temel alınarak hesaplanır. Bu yöntem birincisine benzer `source` dikdörtgen ve ardından uygulanan `source` dikdörtgen çağrısında `CalculateDisplayRect`:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect source, SKRect dest,
                                  BitmapStretch stretch,
                                  BitmapAlignment horizontal = BitmapAlignment.Center,
                                  BitmapAlignment vertical = BitmapAlignment.Center,
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, source, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / source.Width, dest.Height / source.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / source.Width, dest.Height / source.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * source.Width, scale * source.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, source, display, paint);
        }
    }
    ···
}
```

Bu iki yeni ilk `DrawBitmap` yöntemler gösterilmiştir **ölçeklendirme modları** sayfası. Üç XAML dosyasını içeren `Picker` olanak tanıyan öğeleri seçin üyeleri `BitmapStretch` ve `BitmapAlignment` numaralandırmalar:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.ScalingModesPage"
             Title="Scaling Modes">

    <Grid Padding="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="Stretch:"
               Grid.Row="1" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="stretchPicker"
                Grid.Row="1" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapStretch}">
                    <x:Static Member="local:BitmapStretch.None" />
                    <x:Static Member="local:BitmapStretch.Fill" />
                    <x:Static Member="local:BitmapStretch.Uniform" />
                    <x:Static Member="local:BitmapStretch.UniformToFill" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Horizontal Alignment:"
               Grid.Row="2" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="horizontalPicker" 
                Grid.Row="2" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Vertical Alignment:"
               Grid.Row="3" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="verticalPicker" 
                Grid.Row="3" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

Yalnızca arka plan kod dosyasına çıkarır `CanvasView` herhangi `Picker` öğesi değişti. `PaintSurface` İşleyici erişen üç `Picker` görünümleri için arama `DrawBitmap` genişletme yöntemi:

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public ScalingModesPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, dest, stretch, horizontal, vertical);
    }
}
```

Bazı birleşimleri seçenekleri şunlardır:

[![Modları ölçeklendirme](displaying-images/ScalingModes.png "modları ölçeklendirme")](displaying-images/ScalingModes-Large.png#lightbox)

**Dikdörtgen alt** sayfasına sahip neredeyse aynı XAML dosyası olarak **ölçeklendirme modları**, ancak arka plan kod dosyası tarafından verilen bit eşlemin bir dikdörtgen alt tanımlar `SOURCE` alan: 

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");

    static readonly SKRect SOURCE = new SKRect(94, 12, 212, 118);

    public RectangleSubsetPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, SOURCE, dest, stretch, horizontal, vertical);
    }
}
```

Bu rectangle kaynağı monkey'nın head, bu ekran görüntülerinde gösterildiği gibi yalıtır:

[![Dikdörtgen alt](displaying-images/RectangleSubset.png "dikdörtgen alt")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

