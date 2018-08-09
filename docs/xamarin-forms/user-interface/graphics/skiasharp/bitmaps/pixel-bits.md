---
title: SkiaSharp piksel BITS erişme
description: Erişim ve SkiaSharp bit eşlem piksel BITS değiştirmeye yönelik çeşitli teknikler keşfedin.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: charlespetzold
ms.author: chape
ms.date: 07/11/2018
ms.openlocfilehash: 5d79dd89b5313d5d7ead665c54e9a27026cea38c
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615632"
---
# <a name="accessing-skiasharp-pixel-bits"></a>SkiaSharp piksel BITS erişme

Makalesinde anlatıldığı gibi [ **dosyaları kaydetme SkiaSharp bit eşlemlere**](saving.md), bit eşlemler dosyalarında sıkıştırılmış biçimde, JPEG veya PNG gibi genel olarak depolanır. Bellekte bir SkiaSharp bit eşlem constrast içinde sıkıştırılmış değil. Bu, piksel sıralı bir dizi olarak depolanır. Sıkıştırılmamış Bu biçim bir uzaklaştırabilir bit eşlemler aktarımını kolaylaştırır.

SkiaSharp bit eşlem tarafından kapladığı bellek bloğu çok basit bir şekilde düzenlenmiştir: ilk satırın piksel, soldan sağa, ile başlar ve sonra da ikinci satır ile devam eder. İçin tam renk bit eşlemler, her pikselin dört bayt, bit eşlem tarafından gereken toplam bellek alanı dört kez çarpımını genişlik ve yükseklik, yani etkin oluşur.

Nasıl bir uygulama bu piksel cinsinden eşleşmemin piksel bellek bloğu erişerek doğrudan ya da erişebilir bu makalede veya dolaylı olarak. Bazı durumlarda, bir programın görüntü pikselleri analiz edin ve tür histogram oluşturmak isteyebilirsiniz. Daha yaygın olarak, uygulamalar, bit eşlem ' olun piksel algorithmically oluşturarak benzersiz görüntüleri oluşturabilirsiniz:

![Piksel BITS örnekleri](pixel-bits-images/PixelBitsSample.png "piksel bitler örnek")

## <a name="the-techniques"></a>Teknikleri

SkiaSharp bir bit eşleşmemin piksel BITS erişmek için çeşitli teknikler sağlar. Hangi seçtiğiniz genellikle kodlama (Bu Bakım ve hata ayıklama kolaylığı ilgili) kolaylık ve performans arasında bir güvenlik ihlali biridir. Çoğu durumda, aşağıdaki yöntemlerden birini ve özelliklerini kullanacaksınız `SKBitmap` eşleşmemin piksel erişmek için:

- `GetPixel` Ve `SetPixel` elde etmek veya tek bir piksel rengini ayarlamak için yöntemler sağlar.
- `Pixels` Özelliği tüm bit eşlemin piksel renkleri dizisini alır veya renkleri dizisini ayarlar.
- `GetPixels` bit eşlem tarafından kullanılan piksel bellek adresini döndürür.
- `SetPixels` bit eşlem tarafından kullanılan piksel bellek adresini değiştirir.

"Yüksek düzeyde" olarak ilk iki teknikleri ve ikinci iki "düşük düzey." olarak düşünebilirsiniz Diğer yöntemler ve kullanabileceğiniz özellikleri vardır, ancak en değerli şunlardır.

Bu teknikler performans farklarını görmenize olanak [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) adlı bir sayfa içeren uygulama **gradyan bit eşlem** , bir bit eşlem gradyan oluşturmak için kırmızı ve mavi gri birleştiren piksel oluşturur. Program, sekiz farklı kopyalarını oluşturur. Bu eşlemin tüm farklı teknik bit eşlem piksel ayarlamak için kullanma. Her sekiz bu bit eşlem ayrıca teknik kısa metin açıklaması ayarlar ve tüm pikselleri ayarlamak için gereken süreyi hesaplar ayrı bir yöntemde oluşturulur. Her yöntem piksel ayarı mantıksal 100 kat daha iyi bir performans tahmin almak için döngüde kalır. 

### <a name="the-setpixel-method"></a>SetPixel yöntemi

Yalnızca birkaç bağımsız piksel cinsinden ayarlayın veya gerekiyorsa [ `SetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixel/p/System.Int32/System.Int32/SkiaSharp.SKColor/) ve [ `GetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixel/p/System.Int32/System.Int32/) yöntemleri uygundur. Her iki bu yöntemlerin, tamsayı sütun ve satır belirtin. Piksel biçimi'ne olursa olsun, bu iki yöntem piksel olarak ayarlayın ya da elde izin bir `SKColor` değeri:

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

`col` Bağımsız değişkeni 0'dan aralığı olmalıdır bir küçüktür `Width` özelliği bit eşlemin ve `row` bir aralığı 0'dan küçüktür `Height` özelliği.

Yöntem işte **gradyan bit eşlem** kullanarak bir bit eşlem içeriğini ayarlayan `SetPixel` yöntemi. Bit eşlem 256 x 256 piksel olan ve `for` döngüler sabit kodlu değer aralığının:

```csharp
public class GradientBitmapPage : ContentPage
{
    const int REPS = 100;
        
    Stopwatch stopwatch = new Stopwatch();
    ···
    SKBitmap FillBitmapSetPixel(out string description, out int milliseconds)
    {
        description = "SetPixel";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        for (int rep = 0; rep < REPS; rep++)
            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    bitmap.SetPixel(col, row, new SKColor((byte)col, 0, (byte)row));
                }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
}
```

Kırmızı bileşeni eşit bir bit eşlem sütun ve satır mavi bileşeni eşit bir renk için her bir pikseli kümesi vardır. Sonuç bit eşlemi sol, sağ, sol alt ve başka bir yerde gradyanlar ile-sağ, alt Eflatun mavi, red, siyah olur.

`SetPixel` Yöntemi 65.536 kez çağrılan ve bakılmaksızın bu yöntemin nasıl verimli olabilir, bu genellikle bir alternatif varsa bu kadar fazla sayıda API çağrıları gerçekleştirmek için iyi bir fikir değildir. Neyse ki, çeşitli alternatifler de vardır.

### <a name="the-pixels-property"></a>Piksel özelliği

`SKBitmap` tanımlayan bir [ `Pixels` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Pixels/) bir dizi döndürür bir özellik `SKColor` tüm eşlemiyle değerleri. Ayrıca `Pixels` renk değerlerinin bit eşlemin bir dizi ayarlamak için:

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

Piksel ikinci satırın ardından, soldan sağa, ilk satırın ile başlayarak dizideki düzenlenir ve VS. Renkleri dizi toplam sayısı, bit eşlem genişliği ve yüksekliği ürüne eşittir.

Bu özellik etkili olması için görünür, ancak piksel diziye bir bit eşlem ve bit eşlem dizisine geri kopyalanıyor ve piksel ve dönüştürülür aklınızda bulundurun `SKColor` değerleri.

Yöntem işte `GradientBitmapPage` ayarlar kullanarak bit eşlem sınıfı `Pixels` özelliği. Yöntem ayırır bir `SKColor` gerekli boyutu, ancak dizi kullanmış `Pixels` özelliği, dizi oluşturmak için:

```csharp
SKBitmap FillBitmapPixelsProp(out string description, out int milliseconds)
{
    description = "Pixels property";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    SKColor[] pixels = new SKColor[256 * 256]; 

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                pixels[256 * row + col] = new SKColor((byte)col, 0, (byte)row);
            }

    bitmap.Pixels = pixels;

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Dikkat dizinini `pixels` dizi hesaplanmasını gerekiyor `row` ve `col` değişkenleri. Satır, her bir satırdaki (Bu durumda 256) piksel sayısı çarpılır ve ardından sütun eklenir.

`SKBitmap` Ayrıca benzer tanımlar `Bytes` özelliği, tüm bit eşlemin bir bayt dizisi döndürür, ancak tam renk bit işlemini için daha yavaşlatan bir yöntemdir.

### <a name="the-getpixels-pointer"></a>GetPixels işaretçi

Potansiyel olarak bit eşlem piksel erişmek için en güçlü bir tekniktir [ `GetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixels()/), ile karıştırılmamalıdır `GetPixel` yöntemi veya `Pixels` özelliği. Hemen bir fark görürsünüz `GetPixels` içeren C# programlama çok bilinen bir sorun döndürür:

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

.NET [ `IntPtr` ](xref:System.IntPtr) türünü temsil eden bir işaretçi. Çağrıldığı `IntPtr` çünkü bu program çalıştırıldığı, genellikle 32 bit veya 64 bit uzunluğu makinenin yerel işlemcide bir tamsayı uzunluğu. `IntPtr` , `GetPixels` Döndürür, gerçek bit eşlem nesne piksellerinin depolamak için kullandığı bellek bloğunu adresidir. 

Dönüştürebilir `IntPtr` bir C# işaretçi türü kullanarak [ `ToPointer` ](xref:System.IntPtr.ToPointer) yöntemi. C# işaretçi sözdizimi C ve C++ ile aynı aşağıdaki gibidir:

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

`ptr` Değişkendir türünü _bayt işaretçi_. Bu `ptr` değişkeni eşleşmemin piksel depolamak için kullanılan tek tek bayt bellek erişmenize olanak sağlar. Bu bellekten baytı okur veya bir bayt bellek yazmak için bu kodu kullanın:

```sharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

Bu bağlamda, yıldız işareti C# olan _yöneltme işleci_ ve tarafından işaret edilen bellek içeriğini başvurmak için kullanılan `ptr`. Başlangıçta `ptr` bit eşlemi, ancak ilk satırının ilk pikselin ilk bayta noktaları üzerinde aritmetik gerçekleştirebilir `ptr` bit eşlem içindeki diğer konumlara taşımak için değişkeni.

Bir eksisi ise bu kullanabileceğiniz `ptr` ile işaretlenmiş bir kod bloğunda yalnızca değişken `unsafe` anahtar sözcüğü. Ayrıca, derleme güvenli olmayan bloklar izin verecek şekilde işaretlenmiş gerekir. Bu projenin özelliklerini gerçekleştirilir.

İşaretçileri C# ' ta kullanmak çok güçlü, ancak ayrıca çok tehlikeli olabilir. Ötesinde ne işaretçi başvurmak için beklenen bellek erişmediğiniz dikkatli olmanız gerekir. İşaretçi kullan "güvenli." sözcüğüyle ilişkili olmasının nedenidir

Yöntem işte `GradientBitmapPage` kullanan sınıf `GetPixels` yöntemi. Bildirim `unsafe` bayt işaretçiyi kullanarak tüm kod kapsayan bir blok:

```csharp
SKBitmap FillBitmapBytePtr(out string description, out int milliseconds)
{
    description = "GetPixels byte ptr";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            byte* ptr = (byte*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (byte)(col);   // red
                    *ptr++ = 0;             // green
                    *ptr++ = (byte)(row);   // blue
                    *ptr++ = 0xFF;          // alpha
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Zaman `ptr` değişkeni öğesinden alınan ilk `ToPointer` yöntemi, onu işaret en soldaki piksel bit eşlem ilk satırının ilk baytı. `for` İçin döngüsü `row` ve `col` ayarlanır böylece `ptr` ile artan `++` her byte her pikselin ayarlandıktan sonra işleci. Piksel ile 99 diğer döngü `ptr` bit eşlem başlangıcına kadar geri ayarlamanız gerekir.

Her piksel bellek, dört bayt olduğundan her byte ayrı olarak ayarlanması gerekir. Kodu buraya bayt sırası kırmızı, yeşil, mavi ve alfa, ile tutarlı olan olmadığını varsayar `SKColorType.Rgba8888` renk türü. Evrensel Windows platformu için değil ancak, iOS ve Android için varsayılan rengi türü budur Hatırlayacağınız. Varsayılan olarak, bit eşlemler UWP oluşturur `SKColorType.Bgra8888` renk türü. Bu nedenle, bu platform üzerinde farklı sonuçlar bekliyoruz!

Öğesinden döndürülen değerde dönüştürme yapmak mümkündür `ToPointer` için bir `uint` işaretçi yerine `byte` işaretçi. Bu, tek bir deyimde erişilecek bir tüm piksel sağlar. Uygulama `++` işaretçiyle işlecine sonraki piksel işaret edecek şekilde dört bayt tarafından da artırır:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    SKBitmap FillBitmapUintPtr(out string description, out int milliseconds)
    {
        description = "GetPixels uint ptr";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        IntPtr pixelsAddr = bitmap.GetPixels();

        unsafe
        {
            for (int rep = 0; rep < REPS; rep++)
            {
                uint* ptr = (uint*)pixelsAddr.ToPointer();

                for (int row = 0; row < 256; row++)
                    for (int col = 0; col < 256; col++)
                    {
                        *ptr++ = MakePixel((byte)col, 0, (byte)row, 0xFF);
                    }
            }
        }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    uint MakePixel(byte red, byte green, byte blue, byte alpha) =>
            (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

Piksel kullanılarak ayarlanan `MakePixel` yöntemi bir tamsayı piksel kırmızı, yeşil, mavi ve alfa bileşenlerinden oluşturur. Aklınızda `SKColorType.Rgba8888` şöyle sıralama piksel byte biçimdedir:

RR GG BB AA

Ancak bu bayt karşılık gelen tamsayıdır:

AABBGGRR

Tamsayı en az önemli bayt little endian mimarisi uygun olarak ilk depolanır. Bu `MakePixel` yöntemi için bit eşlemler çalışmaz `Bgra8888` renk türü.

`MakePixel` Yöntemi ile işaretlenmiş [ `MethodImplOptions.AggressiveInlining` ](xref:System.Runtime.CompilerServices.MethodImplOptions) seçeneği Derleyici bunu ayrı bir yöntem, ancak bunun yerine Kodu derlemek için burada bu yöntem çağrıldığında yapmaktan kaçınmak için önerilir. Bu, performansı artırmak.

İlginçtir ki, `SKColor` yapısını tanımlayan açık bir dönüştürme `SKColor` bir işaretsiz tamsayı anlamına gelecek şekilde, bir `SKColor` değeri oluşturulabilir ve dönüştürme `uint` yerine kullanılan `MakePixel`:

```csharp
SKBitmap FillBitmapUintPtrColor(out string description, out int milliseconds)
{
    description = "GetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            uint* ptr = (uint*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (uint)new SKColor((byte)col, 0, (byte)row);
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Yalnızca soru ise şudur: tamsayı biçimi `SKColor` sırasına göre değer `SKColorType.Rgba8888` renk türü veya `SKColorType.Bgra8888` renk türü veya başka bir tamamen olabilir? Bu sorunun yanıtı kısa bir süre içinde ortaya.

### <a name="the-setpixels-method"></a>SetPixels yöntemi

`SKBitmap` adlı bir yöntemi de tanımlar [ `SetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixels/p/System.IntPtr/), olduğu gibi çağırın:

```csharp
bitmap.SetPixels(intPtr);
```

Bu geri çağırma `GetPixels` alır bir `IntPtr` piksellerinin depolamak için bit eşlem tarafından kullanılan bellek bloğu başvuruyor. `SetPixels` Çağrı _değiştirir_ bu bellek bloğu tarafından başvurulan bellek bloğu ile `IntPtr` belirtildiği şekilde `SetPixels` bağımsız değişken. Bit eşlem, ardından daha önce kullanarak bellek bloğunu serbest bırakır. Sonraki `GetPixels` olan çağrılır, kümesi bellek bloğu alır `SetPixels`.

İlk olarak göründüğü gibi `SetPixels` size daha fazla güç ve performans sağlar `GetPixels` kullanışsız olan sırasında. İle `GetPixels` bit eşlem bellek bloğu almak ve erişebilirsiniz. İle `SetPixels` ayırmak ve bazı bellek erişim ve ardından, bit eşlem bellek bloğu ayarlayın.

Ancak kullanarak `SetPixels` ayrı bir söz dizimi avantaj sunar: bit eşlem piksel BITS kullanarak bir dizi erişim sağlar. Yöntem işte `GradientBitmapPage` , bu tekniği gösterir. Yöntemin ilk bayt bit eşleşmemin piksel karşılık gelen bir çok boyutlu bir bayt dizisi tanımlar. İlk boyutun satır, İkinci boyuttaki sütunu ve Üçüncü boyutun corresonds dört bileşenin her pikselin için:

```csharp
SKBitmap FillBitmapByteBuffer(out string description, out int milliseconds)
{
    description = "SetPixels byte buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    byte[,,] buffer = new byte[256, 256, 4];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col, 0] = (byte)col;   // red
                buffer[row, col, 1] = 0;           // green
                buffer[row, col, 2] = (byte)row;   // blue
                buffer[row, col, 3] = 0xFF;        // alpha
            }
    
    unsafe
    {
        fixed (byte* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Ardından dizi piksel ile doldurulmuş sonra bir `unsafe` blok ve `fixed` deyimi bu diziyi işaret bayt işaretçisi elde etmek için kullanılır. Bayt işaretçiyle ardından atanabilecek bir `IntPtr` geçirilecek `SetPixels`.

Oluşturduğunuz dizi bir bayt dizisi olmak zorunda değildir. Bu, satır ve sütunları için yalnızca iki boyutlu bir tamsayı dizisi olabilir:

```csharp
SKBitmap FillBitmapUintBuffer(out string description, out int milliseconds)
{
    description = "SetPixels uint buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = MakePixel((byte)col, 0, (byte)row, 0xFF);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

`MakePixel` Yöntemi bir 32-bit piksel renk bileşenlerine birleştirmek için yeniden kullanılır.

Yalnızca bütünlük açısından buraya aynı kodu verilmiştir ancak bir `SKColor` değer işaretsiz tamsayıya dönüştürün:

```csharp
SKBitmap FillBitmapUintBufferColor(out string description, out int milliseconds)
{
    description = "SetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = (uint)new SKColor((byte)col, 0, (byte)row);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

### <a name="comparing-the-techniques"></a>Teknikleri karşılaştırma

Oluşturucusuna **gradyan renk** sayfası tüm sekiz yukarıda gösterilen yöntemleri çağırır ve sonuçları kaydeder:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    string[] descriptions = new string[8];
    SKBitmap[] bitmaps = new SKBitmap[8];
    int[] elapsedTimes = new int[8];

    SKCanvasView canvasView;

    public GradientBitmapPage ()
    {
        Title = "Gradient Bitmap";

        bitmaps[0] = FillBitmapSetPixel(out descriptions[0], out elapsedTimes[0]);
        bitmaps[1] = FillBitmapPixelsProp(out descriptions[1], out elapsedTimes[1]);
        bitmaps[2] = FillBitmapBytePtr(out descriptions[2], out elapsedTimes[2]);
        bitmaps[4] = FillBitmapUintPtr(out descriptions[4], out elapsedTimes[4]);
        bitmaps[6] = FillBitmapUintPtrColor(out descriptions[6], out elapsedTimes[6]);
        bitmaps[3] = FillBitmapByteBuffer(out descriptions[3], out elapsedTimes[3]);
        bitmaps[5] = FillBitmapUintBuffer(out descriptions[5], out elapsedTimes[5]);
        bitmaps[7] = FillBitmapUintBufferColor(out descriptions[7], out elapsedTimes[7]);

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

Oluşturucu oluşturarak sonucuna bir `SKCanvasView` sonuç bit eşlemler görüntülenecek. `PaintSurface` İşleyici sekiz dikdörtgenler ve çağrıları, yüzeyini böler `Display` her biri görüntülemek için:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        int width = info.Width;
        int height = info.Height;

        canvas.Clear();

        Display(canvas, 0, new SKRect(0, 0, width / 2, height / 4));
        Display(canvas, 1, new SKRect(width / 2, 0, width, height / 4));
        Display(canvas, 2, new SKRect(0, height / 4, width / 2, 2 * height / 4));
        Display(canvas, 3, new SKRect(width / 2, height / 4, width, 2 * height / 4));
        Display(canvas, 4, new SKRect(0, 2 * height / 4, width / 2, 3 * height / 4));
        Display(canvas, 5, new SKRect(width / 2, 2 * height / 4, width, 3 * height / 4));
        Display(canvas, 6, new SKRect(0, 3 * height / 4, width / 2, height));
        Display(canvas, 7, new SKRect(width / 2, 3 * height / 4, width, height));
    }

    void Display(SKCanvas canvas, int index, SKRect rect)
    {
        string text = String.Format("{0}: {1:F1} msec", descriptions[index], 
                                    (double)elapsedTimes[index] / REPS);

        SKRect bounds = new SKRect();

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.TextSize = (float)(12 * canvasView.CanvasSize.Width / canvasView.Width);
            textPaint.TextAlign = SKTextAlign.Center;
            textPaint.MeasureText("Tly", ref bounds);

            canvas.DrawText(text, new SKPoint(rect.MidX, rect.Bottom - bounds.Bottom), textPaint);
            rect.Bottom -= bounds.Height;
            canvas.DrawBitmap(bitmaps[index], rect, BitmapStretch.Uniform);
        }
    }
}
```

Kod derleyicinin izin vermek için bu sayfayı çalıştırıldığı **yayın** modu. MacBook Pro, Nexus 5 Android telefon ve Surface Pro 3 Windows 10 çalıştıran bir 8 iPhone simülatörü üzerinde çalışan bu sayfaya aşağıda verilmiştir. Donanım farklılıkları nedeniyle performans zamanları cihazlar arasında karşılaştırma kaçının ancak göreli zaman her bir cihaz arayın:

[![Gradyan bit eşlem](pixel-bits-images/GradientBitmap.png "gradyan bit eşlem")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

Yürütme sürelerini milisaniye cinsinden birleştiren bir tablo şu şekildedir:

| API       | Veri türü | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel  |           | 3.17 ile |   10.77 | 3.49 |
| Piksel    |           | 0,32 |    1,23 | 0,07 |
| GetPixels | byte      | 0,09 |    0.24 | 0.10 |
|           | uint      | 0,06 |    0.26 | 0.05 |
|           | SKColor   | 0.29 |    0.99 | 0,07 |
| SetPixels | byte      | 1.33 |    6.78 | 0,11 |
|           | uint      | 0.14 |    0.69 | 0,06 |
|           | SKColor   | 0.35 |    1.93 | 0.10 |

Beklendiği gibi çağırma `SetPixel` 65.536 kez bir bit eşleşmemin piksel ayarlamak için en az effeicient yoludur. Doldurma bir `SKColor` dizisi ve ayarı `Pixels` özelliği çok daha iyi ve perakendecilerden karşılaştırır bazı ile bile `GetPixels` ve `SetPixels` teknikleri. Çalışma `uint` piksel değerleri genellikle ayrı ayarından daha hızlı `byte` bileşenleri ve dönüştürme `SKColor` işaretsiz bir tamsayı değerine işlemi için bazı ek yükler ekler.

Çeşitli gradyanlar Karşılaştırılacak ilginçtir: üç tüm platformlarda üst satırları aynıdır ve geçişin, istendiği gibi gösterir. Diğer bir deyişle `SetPixel` yöntemi ve `Pixels` özelliği doğru oluşturduğunuzda piksel renk temel piksel biçimi'ne olursa olsun.

İOS ve Android ekran görüntüleri, sonraki iki satır da, doğrulayan bırakmaya aynıdır `MakePixel` yöntemi için varsayılan doğru tanımlandığından `Rgba8888` bu platformlar için piksel biçimi.

İOS ve Android ekran görüntüleri en alt satırında geriye doğru olduğu işaretsiz tamsayı atayarak aldığınız gösteren bir `SKColor` biçiminde değerdir:

GEÇİRGENLİĞİNİ BELİRTTİĞİ

Bayt sırasına göre verilmiştir:

BB GG RR AA

Bu `Bgra8888` sıralama yerine `Rgba8888` sıralama. `Brga8888` Biçimi gradyanlar bu ekran görüntüsü en son sıradaki ilk satırın aynıdır neden olan evrensel Windows platformu için varsayılan değerdir. Ancak bu bit eşlemler oluşturma kodu kabul çünkü Orta iki satır yanlış bir `Rgba8888` sıralama.

Üç tüm platformlarda piksel BITS erişmek için aynı kodu kullanmak istiyorsanız, açıkça oluşturabileceğiniz bir `SKBitmap` kullanarak `Rgba8888` veya `Bgra8888` biçimi. Cast istiyorsanız `SKColor` bit eşlem piksel değerlerinde `Bgra8888`.

## <a name="random-access-of-pixels"></a>Rastgele erişim piksel

`FillBitmapBytePtr` Ve `FillBitmapUintPtr` yöntemleri **gradyan bit eşlem** sayfa benefited gelen `for` döngüler bit eşlem üst satırdaki alttaki ve her satırda soldan sağa ardışık olarak doldurmak için tasarlanmıştır. Piksel işaretçi artan aynı deyimde ile ayarlayabilirsiniz. 

Bazen piksel rastgele yerine sıralı olarak erişmek gereklidir. Kullanıyorsanız `GetPixels` yaklaşımı satır ve sütun göre işaretçiler hesaplamak gerekir. Bu gösterilmiştir **Rainbow sinüs** sayfasında, bir döngüyü sinüs eğrinin biçiminde bir rainbow gösteren bir bit eşlem oluşturur. 

Rainbow renklerini HSL (ton, Doygunluk, parlaklık) renk modelini kullanarak oluşturmak en kolay. `SKColor.FromHsl` Yöntemi oluşturur bir `SKColor` değeri 0-360 (gibi bir daire ancak kırmızı, yeşil ve mavi ve kırmızı dön gitmek açılarını) aralığında hue değerleri kullanarak ve 0 ile 100 arasında doygunluğu ve parlaklık değerleri. Bir rainbow renkleri için en fazla 100 ve 50 bir orta için parlaklık doygunluğu ayarlanmalıdır.

**Rainbow sinüs** bu görüntü bit eşlem satırlarda döngü ve ardından 360 hue değerler arasında döngü oluşturur. Her hue değerden bir sinüs değerini de temel alır bir bit eşlem sütun hesaplar:

```csharp
public class RainbowSinePage : ContentPage
{
    SKBitmap bitmap;

    public RainbowSinePage()
    {
        Title = "Rainbow Sine";

        bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);

        unsafe
        {
            // Pointer to first pixel of bitmap
            uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();

            // Loop through the rows
            for (int row = 0; row < bitmap.Height; row++)
            {
                // Calculate the sine curve angle and the sine value
                double angle = 2 * Math.PI * row / bitmap.Height;
                double sine = Math.Sin(angle);

                // Loop through the hues
                for (int hue = 0; hue < 360; hue++)
                {
                    // Calculate the column
                    int col = (int)(360 + 360 * sine + hue);

                    // Calculate the address
                    uint* ptr = basePtr + bitmap.Width * row + col;

                    // Store the color value
                    *ptr = (uint)SKColor.FromHsl(hue, 100, 50);
                }
            }
        }

        // Create the SKCanvasView
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

Oluşturucu temel bit eşlem oluşturduğunu görürsünüz `SKColorType.Bgra8888` biçimi:

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

Bu programın dönüştürülmesi kullanmasını sağlar `SKColor` değerleri `uint` piksel olmadan endişelenmeyin. Bu belirli programda bir rol oynamıyor olsa da, herhangi bir zamanda kullandığınız `SKColor` de belirtmeniz piksel cinsinden ayarlamak için dönüştürme `SKAlphaType.Unpremul` çünkü `SKColor` renk bileşenlerinin alfa değerle çarpmak değil.

Oluşturucu ardından kullanan `GetPixels` ilk piksel bit eşlemin bir işaretçi alma yöntemi:

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

Herhangi belirli bir satır ve sütun için bir uzaklık değeri eklenmelidir `basePtr`. Bu uzaklık, bit eşlem genişliği sütunu çarpılıp satırdır:

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

`SKColor` Değeri bu işaretçisi kullanılarak bellekte depolanır:

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

İçinde `PaintSurface` işleyicisi `SKCanvasView`, bit eşlem görüntü alanı dolduracak şekilde genişletilir:

[![Rainbow sinüs](pixel-bits-images/RainbowSine.png "Rainbow Sinüs")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>Bir bit eşlem diğerine

Çok çok sayıda görüntü işleme görevler gibi başka bir bit eşlem transfer piksel değiştirme içerir. Bu teknik gösterilmiştir **rengi ayarlama** sayfası. Sayfa bit eşlem kaynaklardan biri yükler ve ardından üç kullanarak görüntüyü değiştirebilmenizi sağlar `Slider` görünümler:

[![Rengi ayarlama](pixel-bits-images/ColorAdjustment.png "rengini ayarlama")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

Her piksel renk için ilk `Slider` 360 hue ' 0'dan bir değer ekler, ancak ardından kullanır (UWP ekran görüntüsünde gösterildiği gibi) 0-360 arasında sonucu tutmak mod işleci, etkili bir şekilde renkleri spektrumun boyunca kaydırma. İkinci `Slider` 0,5 Doygunluk ve üçüncü uygulamak için 2 arasındaki çarpma bir faktör sağlar `Slider` parlaklık aynı Android ekran görüntüsünde gösterildiği gibi yapar.

Program iki bit eşlemler, özgün kaynak bit eşlemi adlı tutar `srcBitmap` ve adlı ayarlanmış hedef bit eşlemi `dstBitmap`. Her zaman bir `Slider` taşınır, programın tüm yeni piksel cinsinden hesaplar `dstBitmap`. Kullanıcılar taşıyarak, denemeler `Slider` çok hızlı bir şekilde yönetebileceğiniz en iyi performansı istediğiniz şekilde görüntüler. Bu içerir `GetPixels` hem kaynak hem de hedef bit eşlemler için yöntemi. 

**Rengi ayarlama** sayfa, kaynak ve hedef bit eşlem renk biçimi denetleme değil. Biraz farklı mantığı için bunun yerine, içerdiği `SKColorType.Rgba8888` ve `SKColorType.Bgra8888` biçimleri. Kaynak ve hedef farklı biçimlerde olabilir ve program çalışmaya devam eder.

Programın önemli dışında işte `TransferPixels` piksel aktaran yöntemi kaynaktan hedefe form. Oluşturucu kümeleri `dstBitmap` eşit `srcBitmap`. `PaintSurface` İşleyici görüntüler `dstBitmap`:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    SKBitmap srcBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKBitmap dstBitmap;

    public ColorAdjustmentPage()
    {
        InitializeComponent();

        dstBitmap = new SKBitmap(srcBitmap.Width, srcBitmap.Height);
        OnSliderValueChanged(null, null);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        float hueAdjust = (float)hueSlider.Value;
        hueLabel.Text = $"Hue Adjustment: {hueAdjust:F0}";

        float saturationAdjust = (float)Math.Pow(2, saturationSlider.Value);
        saturationLabel.Text = $"Saturation Adjustment: {saturationAdjust:F2}";

        float luminosityAdjust = (float)Math.Pow(2, luminositySlider.Value);
        luminosityLabel.Text = $"Luminosity Adjustment: {luminosityAdjust:F2}";

        TransferPixels(hueAdjust, saturationAdjust, luminosityAdjust);
        canvasView.InvalidateSurface();
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(dstBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

`ValueChanged` İşleyicisi `Slider` görünümleri, değerleri ayarlama ve aramalar hesaplar `TransferPixels`.

Tüm `TransferPixels` yöntemi olarak işaretlenmiş `unsafe`. Her iki bit eşlem piksel BITS bayt işaretçileri elde ederek başlar ve ardından tüm satırları ve sütunları döngü. Kaynak bit eşlemi, her pikselin için dört bayt yöntemi alır. Bunlar ya da olabilir `Rgba8888` veya `Bgra8888` sırası. Renk türü sağlar denetimi bir `SKColor` oluşturulacak değer. HSL bileşenleri ardından ayıklanan ayarlandı ve yeniden oluşturmak için kullanılan `SKColor` değeri. Hedef bit eşlemi olup olmamasına bağlı olarak `Rgba8888` veya `Bgra8888`, baytlar içinde hedef bitmp depolanır:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    ···
    unsafe void TransferPixels(float hueAdjust, float saturationAdjust, float luminosityAdjust)
    {
        byte* srcPtr = (byte*)srcBitmap.GetPixels().ToPointer();
        byte* dstPtr = (byte*)dstBitmap.GetPixels().ToPointer();

        int width = srcBitmap.Width;       // same for both bitmaps
        int height = srcBitmap.Height;

        SKColorType typeOrg = srcBitmap.ColorType;
        SKColorType typeAdj = dstBitmap.ColorType;

        for (int row = 0; row < height; row++)
        {
            for (int col = 0; col < width; col++)
            {
                // Get color from original bitmap
                byte byte1 = *srcPtr++;         // red or blue
                byte byte2 = *srcPtr++;         // green
                byte byte3 = *srcPtr++;         // blue or red
                byte byte4 = *srcPtr++;         // alpha

                SKColor color = new SKColor();

                if (typeOrg == SKColorType.Rgba8888)
                {
                    color = new SKColor(byte1, byte2, byte3, byte4);
                }
                else if (typeOrg == SKColorType.Bgra8888)
                {
                    color = new SKColor(byte3, byte2, byte1, byte4);
                }

                // Get HSL components
                color.ToHsl(out float hue, out float saturation, out float luminosity);

                // Adjust HSL components based on adjustments
                hue = (hue + hueAdjust) % 360;
                saturation = Math.Max(0, Math.Min(100, saturationAdjust * saturation));
                luminosity = Math.Max(0, Math.Min(100, luminosityAdjust * luminosity));

                // Recreate color from HSL components
                color = SKColor.FromHsl(hue, saturation, luminosity);

                // Store the bytes in the adjusted bitmap
                if (typeAdj == SKColorType.Rgba8888)
                {
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Alpha;
                }
                else if (typeAdj == SKColorType.Bgra8888)
                {
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Alpha;
                }
            }
        }
    }
    ···
}
```

Bu yöntemin performansını ayrı yöntemleri için kaynak ve hedef bit eşlem renk türleri çeşitli birleşimlerini oluşturarak daha da iyileşme ve her piksel türü, iade etmekten kaçının olma olasılığı yüksektir. Başka bir seçeneği birden çok sağlamaktır `for` için döngüsü `col` değişkeni renk türüne dayalı.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
