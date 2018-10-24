---
title: Temel SkiaSharp, bit eşlem bilgileri
description: Bu makalede, bit eşlemler SkiaSharp, çeşitli kaynaklardan veri yükleme ve bunları Xamarin.Forms uygulamalarında görüntüleme açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 7732bc2ea9a9c5a896b27ca9bd73433ecdcfd9fa
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615216"
---
# <a name="bitmap-basics-in-skiasharp"></a>Temel SkiaSharp, bit eşlem bilgileri

_Bit eşlemler, çeşitli kaynaklardan veri yükleme ve bunları görüntüler._

Oldukça geniş bit eşlem içinde SkiaSharp desteğidir. Bu makalede yalnızca temel kavramları kapsar &mdash; bit eşlemler yükleme ve bunları görüntülemek nasıl:

![](bitmaps-images/bitmapssample.png "İki bit eşlem görüntüleme")

Bit eşlem bir çok daha ayrıntılı keşfi bölümünde bulunabilir [SkiaSharp bit eşlemler](../bitmaps/index.md).

SkiaSharp bit eşlem türünde bir nesnedir [ `SKBitmap` ](xref:SkiaSharp.SKBitmap). Bir bit eşlem oluşturmak için birçok yolu vardır, ancak bu makalede kendisine sınırlar [ `SKBitmap.Decode` ](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) bit eşlemi yükleyen bir .NET yöntemi `Stream` nesne.

**Temel bit eşlemler** sayfasını **SkiaSharpFormsDemos** program üç farklı kaynaklardan gelen bit eşlemler yükleme işlemini gösterir:

- Örneklerimizde Internet
- Yürütülebilir dosya katıştırılmış bir kaynaktan
- Kullanıcının Fotoğraf Kitaplığı'ndan

Üç `SKBitmap` için bu üç kaynağı alanlar olarak tanımlanan nesneleri [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) sınıfı:

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>Bir bit eşlem Web'den yükleme

Bir URL'sini temel alarak bir bit eşlem yüklemek için kullanabileceğiniz [ `HttpClient` ](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) sınıfı. Yalnızca bir örneğini örneğini oluşturmalıdır `HttpClient` ve yeniden, bu nedenle, bir alan olarak depolama:

```csharp
HttpClient httpClient = new HttpClient();
```

Kullanırken `HttpClient` iOS ve Android uygulamaları ile şirket belgelerinde açıklandığı gibi proje özelliklerini ayarlamak da istersiniz  **[Aktarım Katmanı Güvenliği (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)**.

Kullanmak en uygun olduğundan `await` işleciyle `HttpClient`, kod içinde yürütülemez `BasicBitmapsPage` Oluşturucusu. Bunun yerine, parçası olduğu `OnAppearing` geçersiz kılar. URL'sini burada bazı örnek bit eşlemler ile Xamarin web sitesinden bir alana işaret eder. Bit eşlem belirli bir genişliğe yeniden boyutlandırma bir belirtim ekleyerek web sitesinde bir paket sağlar:


```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    // Load web bitmap.
    string url = "https://developer.xamarin.com/demo/IMG_3256.JPG?width=480";

    try
    {
        using (Stream stream = await httpClient.GetStreamAsync(url))
        using (MemoryStream memStream = new MemoryStream())
        {
            await stream.CopyToAsync(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            webBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        };
    }
    catch
    {
    }
}
```

Android işletim sistemi kullanırken yalnızca özel durum harekete `Stream` döndürüldüğü `GetStreamAsync` içinde `SKBitmap.Decode` yöntemi bir ana iş parçacığı üzerinde uzun bir işlem yapmakta olduğundan. Bit eşlem dosyasının içeriğini kopyalanır bu nedenle, bir `MemoryStream` kullanarak nesne `CopyToAsync`.

Statik `SKBitmap.Decode` yöntemi, bit eşlem dosyaları kod çözme için sorumludur. JPEG, PNG ve GIF bit eşlem biçimleri ile çalışır ve sonuçları bir iç SkiaSharp biçiminde depolar. Bu noktada, `SKCanvasView` izin vermek için geçersiz kılınabilir gerekiyor `PaintSurface` ekranı güncelleştirmek için işleyici. 

## <a name="loading-a-bitmap-resource"></a>Bir bit eşlem kaynağı yükleniyor

Bit eşlemler yükleme için en kolay yaklaşım, uygulamanızda doğrudan dahil bir bit eşlem kaynağındaki kod bakımından. **SkiaSharpFormsDemos** programı adlı bir klasör içerir **medya** bit eşlem dosyaları, bir adlı de dahil olmak üzere birkaç içeren **monkey.png**. Program kaynaklarını depolanan bit eşlemler kullanmalısınız **özellikleri** dosyaya vermek için iletişim kutusu bir **derleme eylemi** , **gömülü kaynak**!

Katıştırılmış her kaynağın bir *kaynak kimliği* proje adı, klasör ve dosya adı noktalarla tüm bağlı oluşur: **SkiaSharpFormsDemos.Media.monkey.png**. Bu kaynak belirterek bu kaynağa erişim sağlayabilmek için bağımsız değişken olarak kimliği [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) yöntemi [ `Assembly` ](xref:System.Reflection.Assembly) sınıfı:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    resourceBitmap = SKBitmap.Decode(stream);
}
```

Bu `Stream` nesne doğrudan geçirilebilir `SKBitmap.Decode` yöntemi.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Bir bit eşlem Fotoğraf Kitaplığı'ndan yükleme

Kullanıcının cihazın Resim Kitaplığı'ndan bir fotoğraf yüklemek de mümkündür. Bu özellik, Xamarin.Forms tarafından sağlanmaz. Makalede açıklanan gibi bir bağımlılık hizmet iş gerektirir [Resim Kitaplığı'ndan bir fotoğraf çekme](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

**IPhotoLibrary.cs** dosyası **SkiaSharpFormsDemos** proje ve üç **PhotoLibrary.cs** platformu projelerinde dosyalarında bu makaleden uyarlanmış olmuştur. Ayrıca, Android **MainActivity.cs** makalesinde açıklandığı gibi dosya değiştirildi ve iOS projesine iki satır sonuna doğru fotoğraf kitaplığınıza erişmesine izin verilip verilmediğini **Info.plist**  dosya.

`BasicBitmapsPage` Oluşturucu ekler bir `TapGestureRecognizer` için `SKCanvasView` Tap'ları bildirim almak için. Bir dokunun girişinde `Tapped` işleyicisini alır çağrıları ve resim Seçici bağımlılık hizmeti erişim `PickPhotoAsync`. Varsa bir `Stream` nesne döndürülür ve ardından geçer `SKBitmap.Decode` yöntemi:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();

    using (Stream stream = await photoLibrary.PickPhotoAsync())
    {
        if (stream != null)
        {
            libraryBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Dikkat `Tapped` işleyicisini de çağırır `InvalidateSurface` yöntemi `SKCanvasView` nesne. Bu işlem için yeni bir çağrı oluşturur. `PaintSurface` işleyici.

## <a name="displaying-the-bitmaps"></a>Bit eşlemler görüntüleme

`PaintSurface` İşleyici gereken üç bit eşlemler görüntülenecek. İşleyici telefonun dikey modda ve tuval dikey üç eşit bölüme ayırır varsayar.

İlk bit eşlem ile basit görüntülenen [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) yöntemi. Tüm belirtmeniz gerekir, bit eşlem sol üst köşesinde konumlandırılan olduğu X ve Y koordinatları şunlardır:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Ancak bir `SKPaint` parametresi tanımlanır, varsayılan değeri olan `null` ve onu yok sayabilirsiniz. Bit eşlemin piksel yalnızca bire bir eşleme ile uzaklaştırabilir piksellerini aktarılır. Bir uygulama için görürsünüz `SKPaint` bağımsız değişken sonraki kısmında [ **SkiaSharp saydamlık**](transparency.md).

Bir program olan bir bit eşlem piksel boyutunu edinebilirsiniz [ `Width` ](xref:SkiaSharp.SKBitmap.Width) ve [ `Height` ](xref:SkiaSharp.SKBitmap.Height) özellikleri. Bu özellikler, bit eşlem tuvalin üst üçüncü Merkezi'nde konumlandırmak için koordinatları hesaplamasını izin ver:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

Diğer iki bit eşlemler bir sürümü ile görüntülenen [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) ile bir `SKRect` parametresi:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Üçüncü bir sürümünü [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) iki `SKRect` bağımsız değişkenlerini görüntüleme, ancak bu sürüm için bit eşlemin bir dikdörtgen alt belirtmek için bu makaledeki kullanılmaz.

Yüklenen bir gömülü kaynak bit eşlem bit eşlem görüntülemek için kod aşağıdaki gibidir:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

Bit eşlem monkey yatay olarak bu ekran görüntülerinde esnetilmiş neden olan dikdörtgenin boyutlarına genişletilir:

[![](bitmaps-images/basicbitmaps-small.png "Üçlü sayfasının ekran görüntüsü temel bit eşlemler")](bitmaps-images/basicbitmaps-large.png#lightbox "Üçlü sayfasının ekran görüntüsü temel bit eşlemler")

Üçüncü görüntü &mdash; yalnızca programı çalıştırın ve kendi Resim Kitaplığı'ndan bir fotoğraf yükle görebileceğiniz gibi &mdash; dikdörtgen, ancak dikdörtgenin birlikte yeniden görüntülenir konum ve boyut eşleşmemin en boy oranını korumak için ayarlanır. Bir Ölçeklendirme çarpanı bit eşlem ve hedef dikdörtgenin boyutuna göre ve dikdörtgenin alanındaki ortalama hesaplama gerektiğinden, bu hesaplama biraz daha karmaşıktır:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

Hiçbir bit eşlem Resim Kitaplığı'ndan, henüz yüklenmemiş, ardından `else` blok ekran dokunun kullanıcıdan istemek için metin görüntüler.

Bit eşlemler, saydamlık ve sonraki makalede çeşitli derece olan görüntüleyebilirsiniz [ **SkiaSharp saydamlık** ](transparency.md) açıklar nasıl.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Resim Kitaplığı'ndan bir fotoğraf çekme](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
