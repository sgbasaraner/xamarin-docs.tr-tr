---
title: Temel SkiaSharp, bit eşlem bilgileri
description: Bu makalede, bit eşlemler SkiaSharp, çeşitli kaynaklardan veri yükleme ve bunları Xamarin.Forms uygulamalarında görüntüleme açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: dec6fa1534f14836ae98677ad33e280ff510fb97
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995196"
---
# <a name="bitmap-basics-in-skiasharp"></a>Temel SkiaSharp, bit eşlem bilgileri

_Bit eşlemler, çeşitli kaynaklardan veri yükleme ve bunları görüntüler._

Oldukça geniş bit eşlem içinde SkiaSharp desteğidir. Bu makalede yalnızca temel kavramları kapsar &mdash; bit eşlemler yükleme ve bunları görüntülemek nasıl:

![](bitmaps-images/bitmapssample.png "İki bit eşlem görüntüleme")

SkiaSharp bit eşlem türünde bir nesnedir [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Bir bit eşlem oluşturmak için birçok yolu vardır, ancak bu makalede kendisine sınırlar [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) eşlemden yükleyen yöntemi bir [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) nesnesini bir bit eşlem dosyasına başvurur. Kullanmak uygun olan [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) türetilen sınıf `SKStream` .NET kabul eden bir oluşturucu olduğundan [ `Stream` ](xref:System.IO.Stream) nesne.

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

Bir URL'sini temel alarak bir bit eşlem yüklemek için kullanabileceğiniz [ `WebRequest` ](xref:System.Net.WebRequest) yürütüldü aşağıdaki kodda gösterildiği gibi sınıf `BasicBitmapsPage` Oluşturucusu. URL'sini burada bazı örnek bit eşlemler ile Xamarin web sitesinden bir alana işaret eder. Bit eşlem belirli bir genişliğe yeniden boyutlandırma bir belirtim ekleyerek web sitesinde bir paket sağlar:

```csharp
Uri uri = new Uri("http://developer.xamarin.com/demo/IMG_3256.JPG?width=480");
WebRequest request = WebRequest.Create(uri);
request.BeginGetResponse((IAsyncResult arg) =>
{
    try
    {
        using (Stream stream = request.EndGetResponse(arg).GetResponseStream())
        using (MemoryStream memStream = new MemoryStream())
        {
            stream.CopyTo(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            using (SKManagedStream skStream = new SKManagedStream(memStream))
            {
                webBitmap = SKBitmap.Decode(skStream);
            }
        }
    }
    catch
    {
    }

    Device.BeginInvokeOnMainThread(() => canvasView.InvalidateSurface());

}, null);
```

Bit eşlem başarıyla indirildi, geri çağırma yöntemi geçirilen `BeginGetResponse` yöntemi çalışır. `EndGetResponse` Çağrı olması gereken bir `try` durumda bir hata oluştu engelleyin. `Stream` Nesne öğesinden alınan `GetResponseStream` bit eşlem içeriğini kopyalanır bazı platformlarda, yeterli değil, bu nedenle bir `MemoryStream` nesne. Bu noktada, `SKManagedStream` nesne oluşturulabilir. Bunu şimdi, büyük olasılıkla bir JPEG veya PNG dosyası olan bit eşlem dosyası başvuruyor. `SKBitmap.Decode` Yöntemi bit eşlem dosyası kodunu çözer ve sonuçları bir iç SkiaSharp biçiminde depolar.

Geçirilen geri çağırma yöntemi `BeginGetResponse` anlamına Oluşturucusu yürütme tamamlandıktan sonra çalışır `SKCanvasView` izin vermek için geçersiz kılınabilir gerekiyor `PaintSurface` ekranı güncelleştirmek için işleyici. Ancak, `BeginGetResponse` geri çağırma kullanmak için gerekli olacak şekilde yürütme, ikincil bir dizi çalıştıran `Device.BeginInvokeOnMainThread` çalıştırılacak `InvalidateSurface` kullanıcı arabirimi iş parçacığında yöntem.

## <a name="loading-a-bitmap-resource"></a>Bir bit eşlem kaynağı yükleniyor

Bit eşlemler yükleme için en kolay yaklaşım, uygulamanızda doğrudan dahil bir bit eşlem kaynağındaki kod bakımından. **SkiaSharpFormsDemos** programı adlı bir klasör içerir **medya** adlı bir bit eşlem dosyası içeren **monkey.png**. İçinde **özellikleri** iletişim bu dosya için böyle bir dosya vermelisiniz bir **derleme eylemi** , **gömülü kaynak**!

Katıştırılmış her kaynağın bir *kaynak kimliği* proje adı, klasör ve dosya adı noktalarla tüm bağlı oluşur: **SkiaSharpFormsDemos.Media.monkey.png**. Bu kaynak belirterek bu kaynağa erişim sağlayabilmek için bağımsız değişken olarak kimliği [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) yöntemi [ `Assembly` ](xref:System.Reflection.Assembly) sınıfı:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

Bu `Stream` nesne doğrudan dönüştürülebilir bir `SKManagedStream` nesne.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Bir bit eşlem Fotoğraf Kitaplığı'ndan yükleme

Kullanıcının cihazın Resim Kitaplığı'ndan bir fotoğraf yüklemek de mümkündür. Bu özellik, Xamarin.Forms tarafından sağlanmaz. Makalede açıklanan gibi bir bağımlılık hizmet iş gerektirir [Resim Kitaplığı'ndan bir fotoğraf çekme](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

**IPicturePicker.cs** dosya ve üç **PicturePickerImplementation.cs** dosyalarından bu makalede çeşitli projeler için kopyalanmış **SkiaSharpFormsDemos**çözümü ve yeni ad alanı adı verilir. Ayrıca, Android **MainActivity.cs** makalesinde açıklandığı gibi dosya değiştirildi ve iOS projesine iki satır sonuna doğru fotoğraf kitaplığınıza erişmesine izin verilip verilmediğini **Info.plist**  dosya.

`BasicBitmapsPage` Oluşturucu ekler bir `TapGestureRecognizer` için `SKCanvasView` Tap'ları bildirim almak için. Bir dokunun girişinde `Tapped` işleyicisini alır çağrıları ve resim Seçici bağımlılık hizmeti erişim `GetImageStreamAsync`. Varsa bir `Stream` nesne döndürülür ve ardından içeriği içine kopyalanır bir `MemoryStream`gerektirdiği gibi bazı platformlar. Kodun geri kalanını iki diğer teknikleri ile benzerdir:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPicturePicker picturePicker = DependencyService.Get<IPicturePicker>();

    using (Stream stream = await picturePicker.GetImageStreamAsync())
    {
        if (stream != null)
        {
            using (MemoryStream memStream = new MemoryStream())
            {
                stream.CopyTo(memStream);
                memStream.Seek(0, SeekOrigin.Begin);

                using (SKManagedStream skStream = new SKManagedStream(memStream))
                {
                    libraryBitmap = SKBitmap.Decode(skStream);
                }
            }
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Dikkat `Tapped` işleyicisi çağrılarını `InvalidateSurface` yöntemi `SKCanvasView` nesne. Bu işlem için yeni bir çağrı oluşturur. `PaintSurface` işleyici.

## <a name="displaying-the-bitmaps"></a>Bit eşlemler görüntüleme

`PaintSurface` İşleyici gereken üç bit eşlemler görüntülenecek. İşleyici telefonun dikey modda ve tuval dikey üç eşit bölüme ayırır varsayar.

İlk bit eşlem ile basit görüntülenen [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) yöntemi. Tüm belirtmeniz gerekir, bit eşlem sol üst köşesinde konumlandırılan olduğu X ve Y koordinatları şunlardır:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Ancak bir `SKPaint` parametresi tanımlanır, varsayılan değeri olan `null` ve onu yok sayabilirsiniz. Bit eşlemin piksel yalnızca bire bir eşleme ile uzaklaştırabilir piksellerini aktarılır.

Bir program olan bir bit eşlem piksel boyutunu edinebilirsiniz [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) ve [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) özellikleri. Bu özellikler, bit eşlem tuvalin üst üçüncü Merkezi'nde konumlandırmak için koordinatları hesaplamasını izin ver:

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

Diğer iki bit eşlemler bir sürümü ile görüntülenen [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) ile bir `SKRect` parametresi:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Üçüncü bir sürümünü [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) iki `SKRect` bağımsız değişkenlerini görüntüleme, ancak bu sürüm için bit eşlemin bir dikdörtgen alt belirtmek için bu makaledeki kullanılmaz.

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


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Resim Kitaplığı'ndan bir fotoğraf çekme](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
