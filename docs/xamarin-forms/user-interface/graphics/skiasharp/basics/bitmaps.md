---
title: Bit eşlem temelleri
description: Bit eşlemler çeşitli kaynaklardan yüklemek ve bunları görüntüler.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: 688c6218f9ac66e3dfd6cd157e43f9b639e124c6
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="bitmap-basics"></a>Bit eşlem temelleri

_Bit eşlemler çeşitli kaynaklardan yüklemek ve bunları görüntüler._

Bit eşlemler SkiaSharp içinde destek oldukça kapsamlıdır. Bu makalede yalnızca temel kavramları kapsar &mdash; bit eşlemler yükleme ve bunların nasıl görüntüleneceği:

![](bitmaps-images/bitmapssample.png "İki bit eşlemler görüntüleme")

Türünde bir nesne bir SkiaSharp bit eşlem ise [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Bir bitmap oluşturmak için birçok yolu vardır, ancak bu makale kendisine kısıtlayan [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) bit eşlem yükler yöntemi bir [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) bir bit eşlem dosyası başvuran nesne. Kullanmak uygun olan [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) öğesinden türetilen sınıf `SKStream` bir .NET kabul eden bir oluşturucuya sahip olduğundan [ `Stream` ](https://developer.xamarin.com/api/type/System.IO.Stream/) nesnesi.

**Temel bit eşlemler** sayfasındaki **SkiaSharpFormsDemos** program bit eşlemler üç farklı kaynaklardan yükleme gösterilmektedir:

- Internet üzerinden
- Yürütülebilir dosya katıştırılmış bir kaynaktan
- Kullanıcının Fotoğraf Kitaplığı'ndan

Üç `SKBitmap` bu üç kaynaklarının alanlar olarak tanımlanan için nesneleri [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) sınıfı:

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

## <a name="loading-a-bitmap-from-the-web"></a>Web sunucusundan bir bit eşlem yükleniyor

Bir URL temel alınarak bir bit eşlem'i yüklemek için kullanabileceğiniz [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) yürütülme aşağıdaki kodda gösterildiği gibi sınıfı `BasicBitmapsPage` Oluşturucusu. Bir alanı, bazı örnek bit Eşlemleriyle Xamarin web sitesinde URL'sini burada işaret ediyor. Bit eşlem belirli bir genişliğe yeniden boyutlandırma için belirtimi ekleyerek web sitesinde bir paket sağlar:

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

Geri çağırma yöntemi bit eşlem başarıyla indirildi, geçirilen `BeginGetResponse` yöntemi çalışır. `EndGetResponse` Çağrısı olması gerekiyor bir `try` durumunda bir hata oluştu engelleyin. `Stream` Nesnesi elde `GetResponseStream` bit eşlem içeriği içine kopyalanır bazı platformlarda yeterli olmadığından bir `MemoryStream` nesnesi. Bu noktada, `SKManagedStream` nesne oluşturulabilir. Bu büyük olasılıkla bir JPEG veya PNG dosyası olan bit eşlem dosyası başvurur. `SKBitmap.Decode` Yöntemi bit eşlem dosyası kodunu çözer ve sonuçları bir iç SkiaSharp biçimde depolar.

Geri çağırma yöntemi geçirilen `BeginGetResponse` anlamına Oluşturucusu yürütme tamamlandıktan sonra çalışır `SKCanvasView` izin vermek için geçersiz kılınan gerekiyor `PaintSurface` ekranı güncelleştirmek için işleyici. Ancak, `BeginGetResponse` geri çağırma kullanmak gerekli olan bir ikincil yürütme, iş parçacığı içinde çalıştığından `Device.BeginInvokeOnMainThread` çalıştırmak için `InvalidateSurface` kullanıcı arabirimi iş parçacığı yöntemi.

## <a name="loading-a-bitmap-resource"></a>Bir bit eşlem kaynak yükleniyor

Bit eşlemler yüklenirken en kolay yaklaşım doğrudan uygulamanızda dahil bir bit eşlem kaynak kodu bakımından. **SkiaSharpFormsDemos** program adlı bir klasörü dahil **medya** adlı bir bit eşlem dosyası içeren **monkey.png**. İçinde **özellikleri** iletişim için bu dosya, böyle bir dosya vermelisiniz bir **yapı eylemi** , **katıştırılmış kaynak**!

Her katıştırılmış kaynağın bir *kaynak kimliği* proje adı, klasör ve dosya adı, dönemlere göre tüm bağlı oluşur: **SkiaSharpFormsDemos.Media.monkey.png**. Bu kaynak belirterek bu kaynağa erişim sağlayabilmek için bağımsız değişken olarak kimliği [ `GetManifestResourceStream` ](https://developer.xamarin.com/api/member/System.Reflection.Assembly.GetManifestResourceStream/p/System.String/) yöntemi [ `Assembly` ](https://developer.xamarin.com/api/type/System.Reflection.Assembly/) sınıfı:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

Bu `Stream` nesne doğrudan dönüştürülebilir bir `SKManagedStream` nesnesi.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Fotoğraf Kitaplığı'ndan bir bit eşlem yükleniyor

Ayrıca kullanıcının bir fotoğraf cihazın Resim Kitaplığı'ndan yüklemek mümkündür. Bu özellik Xamarin.Forms tarafından sağlanmadı. Makalede açıklanan gibi bir bağımlılık hizmeti iş gerektirir [Resim Kitaplığı'ndan bir fotoğraf çekme](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

**IPicturePicker.cs** dosya ve üç **PicturePickerImplementation.cs** bu makaledeki dosyaları için çeşitli projeleri kopyalanmış **SkiaSharpFormsDemos**çözümü ve yeni ad alanı adı verilir. Ayrıca, Android **MainActivity.cs** makalesinde açıklandığı gibi dosya değiştirildi ve iOS projesi en altına iki satır fotoğraf kitaplıkla erişmesine izin verilen **info.plist**  dosya.

`BasicBitmapsPage` Oluşturucusu ekler bir `TapGestureRecognizer` için `SKCanvasView` Tap'ları bildirim almak için. Bir dokunun girişinde `Tapped` işleyicisini alır resim Seçici bağımlılık hizmeti ve çağrıları erişim `GetImageStreamAsync`. Varsa bir `Stream` nesne döndürülür ve ardından içeriği içine kopyalanır bir `MemoryStream`, gerektirdiği gibi bazı platformlar. Kod kalan iki başka teknikler için benzer:

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

Dikkat `Tapped` işleyicisi çağrılarını `InvalidateSurface` yöntemi `SKCanvasView` nesnesi. Bu yeni bir çağrı oluşturur `PaintSurface` işleyicisi.

## <a name="displaying-the-bitmaps"></a>Bit eşlemler görüntüleme

`PaintSurface` İşleyici gereken üç bit eşlemler görüntülemek. İşleyici telefon dikey modundayken ve tuvale dikey olarak üç eşit bölüme ayırır varsayar.

İlk bit eşlem basit ile görüntülenen [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) yöntemi. Tüm belirtmeniz gerekir, bit eşlem sol üst köşesindeki konumlandırılması olduğu X ve Y koordinatları şunlardır:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Ancak bir `SKPaint` parametresi tanımlanır, varsayılan değerine sahip `null` ve onu yok sayabilirsiniz. Bit eşlem piksel yalnızca bire bir eşleme ile görüntü yüzeyini piksel aktarılır.

Bir program bir bit eşlem piksel boyutları edinebilirsiniz [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) ve [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) özellikleri. Bu özellikler, bit eşlem üst üçüncü Kanvasın ortasına yerleştirin koordinatları hesaplamak üzere program izin ver:

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

Diğer iki bit eşlemler bir sürümü ile görüntülenen [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) ile bir `SKRect` parametre:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Üçüncü bir sürümünü [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) iki sahip `SKRect` bağımsız bit eşlem görüntüleme, ancak bu sürüm için dikdörtgen kümesini belirtmek için bu makalede kullanılan değil.

Bir kaynağı bit eşlem yüklenen bit eşlem görüntülemek için kod aşağıdaki gibidir:

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

Bit eşlem monkey yatay bu ekran görüntülerinde genişletilir neden olan dikdörtgenin boyutları için genişletilir:

[![](bitmaps-images/basicbitmaps-small.png "Bir Üçlü sayfasının ekran görüntüsü temel bit eşlemler")](bitmaps-images/basicbitmaps-large.png#lightbox "bir Üçlü sayfasının ekran görüntüsü temel bit eşlemler")

Üçüncü görüntü &mdash; programını çalıştırın ve bir fotoğraf kendi Resim Kitaplığı'ndan yük yalnızca görebileceğiniz &mdash; dikdörtgen ancak dikdörtgenin içinde de görüntülenir konum ve boyut bit eşlem'ın en boy oranını korumak için ayarlanır. Bir bit eşlem ve hedef dikdörtgen boyutuna göre faktörü ölçekleme ve dikdörtgenin alanındaki ortalama hesaplama gerektirdiğinden bu hesaplama biraz daha karmaşıktır:

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

Hiçbir bit eşlem Resim Kitaplığı'ndan henüz yüklendiyse sonra `else` blok ekrana dokunun kullanıcıdan istemek için bazı metinleri görüntüler.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Resim Kitaplığı'ndan bir fotoğraf çekme](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
