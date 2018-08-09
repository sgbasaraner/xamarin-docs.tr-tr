---
title: Dosyalara SkiaSharp bit eşlemleri kaydetme
description: Kullanıcının Fotoğraf Kitaplığı'nda bit eşlemleri kaydetme için SkiaSharp tarafından desteklenen çeşitli dosya biçimleri keşfedin.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 2D696CB6-B31B-42BC-8D3B-11D63B1E7D9C
author: charlespetzold
ms.author: chape
ms.date: 07/10/2018
ms.openlocfilehash: e957134ecceee84962e5a4fc153285ea0a2a5906
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615567"
---
# <a name="saving-skiasharp-bitmaps-to-files"></a>Dosyalara SkiaSharp bit eşlemleri kaydetme

SkiaSharp uygulama oluşturan veya değiştiren bir bit eşlem sonra uygulamanın bir bit eşlem kullanıcının Fotoğraf Kitaplığı'na kaydetmek isteyebilirsiniz:

![Bit eşlemleri kaydetme](saving-images/SavingSample.png "bit eşlemleri kaydetme")

Bu görevi, iki adımı kapsar:

- SkiaSharp bit eşlem JPEG veya PNG gibi belirli dosya biçiminde veri dönüştürme.
- Sonuç, platforma özgü kod kullanarak Fotoğraf Kitaplığı'na kaydediliyor.

## <a name="file-formats-and-codecs"></a>Dosya biçimleri ve codec bileşenleri

Günümüzün popüler bit eşlem dosyası çoğunu depolama alanını azaltmak için kullanım sıkıştırma biçimlendirir. Sıkıştırma tekniklerden birini iki kategoriden adlı _kayıplı_ ve _kayıpsız_. Bu terimler sıkıştırma algoritması veri kaybıyla sonuçlanır olup olmadığını gösterir.

En popüler kayıplı biçim JPEG Dosya değişim biçimi tarafından geliştirilmiştir ve JPEG çağrılır. JPEG sıkıştırma algoritmasının adı verilen matematik bir araç kullanarak görüntü çözümler _ayrık cosine dönüştürme_ve görsel görüntü kalitesini koruyarak için önemli değil veri kaldırmayı dener. Sıkıştırma derecesini denen bir ayar ile denetlenebilir _kalite_. Daha büyük dosyaları yüksek kalite ayarları sonucu.

Buna karşılık, kayıpsız sıkıştırma algoritması, resim yineleme ve veri azaltır, ancak herhangi bir bilgi kaybına neden olmayan bir biçimde kodlanmış piksel desenleri için analiz eder. Özgün bit eşlem veri tamamen sıkıştırılmış dosyayı geri yüklenebilir. Birincil kayıpsız sıkıştırılmış dosya kullanımda bugün Taşınabilir Ağ Grafikleri (PNG) biçimidir.

Genellikle, PNG veya algorithmically el ile oluşturulmuş görüntüleri için kullanılırken JPEG fotoğraflar için kullanılır. Bazı dosyaların boyutunu azaltır herhangi kayıpsız sıkıştırma algoritması mutlaka diğerleri boyutunu artırmanız gerekir. Neyse ki, bu artış boyutu genellikle yalnızca birçok rastgele (veya görünüşte rastgele) bilgi içeren veri için gerçekleşir.

Sıkıştırma algoritmaları iki koşulları garanti etmeye yetecek kadar karmaşık sıkıştırma ve açma işlemleri açıklar:

- _kod çözme_ &mdash; bir bit eşlem dosyası biçimi okuyun ve iptal
- _kodlama_ &mdash; bit eşlem sıkıştırma ve bir bit eşlem dosya biçimine yazma

[ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/) Sınıfı içeren adlandırılmış çeşitli yöntemler `Decode` oluşturan bir `SKBitmap` sıkıştırılmış bir kaynaktan. Gerekli olan bir dosya adı, akış veya bayt dizisi sağlamak için. Kod Çözücü, dosya biçimini belirlemek ve teslim bunu devre dışı iç bir uygun kod çözme işlevi.

Ayrıca, [ `SKCodec` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodec/) sınıfında adlı iki yöntem `Create` oluşturabilir, bir `SKCodec` sıkıştırılmış bir kaynaktan nesnesi ve bir uygulamanın kod çözme işleminde daha fazla katılım gösterin izin. ( `SKCodec` Sınıfı makalesinde gösterilen [ **hareketlendirme SkiaSharp bit eşlemler** ](animating.md#gif-animation) animasyonlu GIF dosyası kod çözme bağlantılı.)

Bir bit eşlem kodlarken daha fazla bilgi gerekiyor: Kodlayıcı uygulamanın istediği (JPEG veya PNG veya başka bir şey) kullanmak için belirli dosya biçimini bilmeniz gerekir. Kayıplı biçimi isterseniz, kodla istenen kalite düzeyini de bilmeniz gerekir. 

`SKBitmap` Sınıfı tanımlar [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Encode/p/SkiaSharp.SKWStream/SkiaSharp.SKEncodedImageFormat/System.Int32/) şu sözdizimiyle yöntemi:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

Bu yöntem, kısa süre içinde daha ayrıntılı olarak açıklanmıştır. Kodlanmış bir bit eşlem yazılabilir akışına yazılır. ('W' `SKWStream` "için yazılabilir" anlamına gelir.) Dosya biçimi ikinci ve üçüncü bağımsız değişkenleri belirtin ve (kayıplı biçimleri) 0 ile 100 arasında istenen kalite.

Ayrıca, [ `SKImage` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImage/) ve [ `SKPixmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPixmap/) sınıfları da tanımlamanız `Encode` yöntemleri, biraz daha verimli olan ve tercih ettiğiniz. Kolayca oluşturabileceğiniz bir `SKImage` nesnesinden bir `SKBitmap` 'using static nesne [ `SKImage.FromBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.FromBitmap/p/SkiaSharp.SKBitmap/) yöntemi. Alabilirsiniz bir `SKPixmap` nesnesinden bir `SKBitmp` kullanarak nesne [ `PeekPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.PeekPixels()/) yöntemi.

Aşağıdakilerden birini [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.Encode()/) tarafından tanımlanan yöntemleri `SKImage` hiç parametresi yok ve bir PNG biçimi otomatik olarak kaydeder. Bu bir parametresiz yöntemin kullanımı çok kolaydır.

## <a name="platform-specific-code-for-saving-bitmap-files"></a>Bit eşlem dosyaları kaydetmek için platforma özgü kod

Kodlama ne zaman bir `SKBitmap` belirli bir dosya nesnesine biçimlendirmek, genellikle olacaksınız sol çeşit bir akış nesnesi veya bir veri dizisi. Bazı `Encode` yöntemleri (tarafından tanımlanan parametre olmadan de dahil olmak üzere `SKImage`) iade bir [ `SKData` ](https://developer.xamarin.com/api/type/SkiaSharp.SKData/) kullanarak bir bayt dizisi için dönüştürülebilir nesnesi [ `ToArray` ](https://developer.xamarin.com/api/member/SkiaSharp.SKData.ToArray()/) yöntemi. Bu veriler daha sonra bir dosyaya kaydedilmelidir. 

Standart kullanabileceğinizden uygulama yerel depolama alanındaki bir dosyaya kaydetme oldukça kolay `System.IO` sınıflar ve yöntemler bu görev için. Bu teknik makalesinde gösterilmiştir [ **hareketlendirme SkiaSharp bit eşlemler** ](animating.md#bitmap-animation) animasyon, bit eşlemler Mandelbrot kümesinin bir dizi bağlantılı olarak.

Diğer uygulamalar tarafından paylaşılacak dosyayı istiyorsanız, kullanıcının Fotoğraf Kitaplığı'na kaydedilmiş olması gerekir. Bu görev, platforma özgü kod ve Xamarin.Forms kullanımını gerektirir [ `DependencyService` ](xref:Xamarin.Forms.DependencyService).

**SkiaSharpFormsDemo** projesi [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) uygulama tanımlayan bir `IPhotoLibrary` ile kullanılan arabirimi `DependencyService` sınıfı. Bu söz dizimini tanımlar bir `SavePhotoAsync` yöntemi:

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

Bu arabirim de tanımlar `PickPhotoAsync` platforma özgü dosya Seçici cihazın Fotoğraf Kitaplığı'nı açmak için kullanılan yöntem.

İçin `SavePhotoAsync`, ilk bağımsız değişken zaten JPEG veya PNG gibi belirli dosya formatına kodlanmış bit eşlem içeren bayt dizisidir. Bu, uygulamanın dosya adından önce gelen bir sonraki parametreyi belirtilen belirli bir klasöre oluşturduğu tüm bit eşlemler yalıtmak isteyebilirsiniz mümkündür. Yöntem başarılı olmadığını gösteren bir Boole değeri döndürür.

İşte nasıl `SavePhotoAsync` üç platformları üzerinde uygulanır:

### <a name="the-ios-implementation"></a>İOS uygulaması

İOS uygulaması `SavePhotoAsync` kullanan [ `SaveToPhotosAlbum` ](https://developer.xamarin.com/api/member/UIKit.UIImage.SaveToPhotosAlbum/) yöntemi `UIImage`:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        NSData nsData = NSData.FromArray(data);
        UIImage image = new UIImage(nsData);
        TaskCompletionSource<bool> taskCompletionSource = new TaskCompletionSource<bool>();

        image.SaveToPhotosAlbum((UIImage img, NSError error) =>
        {
            taskCompletionSource.SetResult(error == null);
        });

        return taskCompletionSource.Task;
    }
}
```

Ne yazık ki, bir dosya adı veya görüntü klasörü belirtmek için hiçbir yolu yoktur. 

**Info.plist** iOS proje dosyasında bu görüntüleri Fotoğraf Kitaplığı'na ekler belirten bir anahtar gerektirir:

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

Dikkat edin! Fotoğraf Kitaplığı yalnızca erişim izni anahtarı aynı değildir çok benzer:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Android uygulaması

Android uygulaması `SavePhotoAsync` ilk denetler, `folder` bağımsız değişkeni `null` ya da boş bir dize. Bu durumda, bit eşlem Fotoğraf Kitaplığı'nın kök dizininde kaydedildi. Aksi takdirde, klasörü alınır ve yoksa, oluşturulur:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        try
        {
            File picturesDirectory = Environment.GetExternalStoragePublicDirectory(Environment.DirectoryPictures);
            File folderDirectory = picturesDirectory;

            if (!string.IsNullOrEmpty(folder))
            {
                folderDirectory = new File(picturesDirectory, folder);
                folderDirectory.Mkdirs();
            }

            using (File bitmapFile = new File(folderDirectory, filename))
            {
                bitmapFile.CreateNewFile();

                using (FileOutputStream outputStream = new FileOutputStream(bitmapFile))
                {
                    await outputStream.WriteAsync(data);
                }

                // Make sure it shows up in the Photos gallery promptly.
                MediaScannerConnection.ScanFile(MainActivity.Instance,
                                                new string[] { bitmapFile.Path },
                                                new string[] { "image/png", "image/jpeg" }, null);
            }
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

Çağrı `MediaScannerConnection.ScanFile` kesinlikle gerekli değildir ancak hemen Fotoğraf Kitaplığı denetleyerek programınızı test ediyorsanız, çok kitaplığı Galeri görünümü güncelleştirerek yardımcı olur.

**AndroidManifest.xml** dosyası aşağıdaki izin etiketi gerektirir:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>UWP uygulaması

UWP uygulaması `SavePhotoAsync` Android uygulamasına yapısında çok benzer:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        StorageFolder picturesDirectory = KnownFolders.PicturesLibrary;
        StorageFolder folderDirectory = picturesDirectory;

        // Get the folder or create it if necessary
        if (!string.IsNullOrEmpty(folder))
        {
            try
            {
                folderDirectory = await picturesDirectory.GetFolderAsync(folder);
            }
            catch
            { }

            if (folderDirectory == null)
            {
                try
                {
                    folderDirectory = await picturesDirectory.CreateFolderAsync(folder);
                }
                catch
                {
                    return false;
                }
            }
        }

        try
        {
            // Create the file.
            StorageFile storageFile = await folderDirectory.CreateFileAsync(filename,
                                                CreationCollisionOption.GenerateUniqueName);

            // Convert byte[] to Windows buffer and write it out.
            IBuffer buffer = WindowsRuntimeBuffer.Create(data, 0, data.Length, data.Length);
            await FileIO.WriteBufferAsync(storageFile, buffer);
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

**Özellikleri** bölümünü **Package.appxmanifest** dosyası gerektirir **resim kitaplığı**.

## <a name="exploring-the-image-formats"></a>Görüntü biçimlerini keşfetme

İşte [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Encode/p/SkiaSharp.SKWStream/SkiaSharp.SKEncodedImageFormat/System.Int32/) yöntemi `SKImage` yeniden:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](https://developer.xamarin.com/api/type/SkiaSharp.SKEncodedImageFormat/) bazıları yerine belirsiz olan on bit eşlem dosyası biçimleri için başvuran üyeleri olan bir sabit listesi verilmiştir:

- `Astc` &mdash; Uyarlamalı ölçeklenebilir doku sıkıştırma
- `Bmp` &mdash; Windows bit eşlem
- `Dng` &mdash; Adobe dijital negatif
- `Gif` &mdash; Grafik Değişim Biçimi
- `Ico` &mdash; Windows simge görüntüleri
- `Jpeg` &mdash; JPEG Dosya Değişim Biçimi
- `Ktx` &mdash; OpenGL Khronos doku biçimi
- `Pkm` &mdash; Pokémon dosyayı Kaydet
- `Png` &mdash; Taşınabilir Ağ Grafikleri
- `Wbmp` &mdash; Kablosuz uygulama protokolü bit eşlem biçimi (1 piksel başına bit cihazları)
- `Webp` &mdash; Google WebP biçimi

Kısa bir süre içinde anlatıldığı gibi bunlar sadece üç dosya biçimleri (`Jpeg`, `Png`, ve `Webp`) gerçekte SkiaSharp tarafından desteklenir.

Kaydetmek için bir `SKBitmap` adlı nesne `bitmap` kullanıcının fotoğraf kitaplığına üyesi etmeniz `SKEncodedImageFormat` adlı numaralandırma `imageFormat` ve (kayıplı biçimleri için) bir tamsayı `quality` değişkeni. Aşağıdaki kod, bit eşlem ada sahip bir dosyaya kaydetmek için kullanabileceğiniz `filename` içinde `folder` klasörü:

```csharp
using (MemoryStream memStream = new MemoryStream())
using (SKManagedWStream wstream = new SKManagedWStream(memStream))
{
    bitmap.Encode(wstream, imageFormat, quality);
    byte[] data = memStream.ToArray();

    // Check the data array for content!

    bool success = await DependencyService.Get<IPhotoLibrary>().SavePhotoAsync(data, folder, filename);

    // Check return value for success!
}
```

`SKManagedWStream` Sınıf türetilir `SKWStream` (olduğu anlamına gelir "yazılabilir akış için"). `Encode` Yöntemi, kodlanmış bir bit eşlem dosyası bu akışa yazar. Bu kod açıklamaları denetimi gerçekleştirmek ihtiyacınız olabilecek bazı hataya bakın. 

**Dosya biçimlerini kaydetme** sayfasını [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) uygulama bir bit eşlem çeşitli biçimlerde kaydetme ile denemenizi izin vermek için benzer bir kod kullanır.

XAML dosyası içeren bir `SKCanvasView` bir bit eşlem görüntüleyen, uygulama sayfanın geri kalanını her şeyi içerirken çağırmayı gerektiren `Encode` yöntemi `SKBitmap`. Sahip bir `Picker` üyesi için `SKEncodedImageFormat` numaralandırma bir `Slider` kayıplı bit eşlem biçimleri için kalite bağımsız değişkeni için iki `Entry` dosya ve klasör adı, görünümleri ve `Button` dosyayı kaydetmek için.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.SaveFileFormatsPage"
             Title="Save Bitmap Formats">

    <StackLayout Margin="10">
        <skiaforms:SKCanvasView PaintSurface="OnCanvasViewPaintSurface"
                                VerticalOptions="FillAndExpand" />

        <Picker x:Name="formatPicker"
                Title="image format"
                SelectedIndexChanged="OnFormatPickerChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKEncodedImageFormat}">
                    <x:Static Member="skia:SKEncodedImageFormat.Astc" />
                    <x:Static Member="skia:SKEncodedImageFormat.Bmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Dng" />
                    <x:Static Member="skia:SKEncodedImageFormat.Gif" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ico" />
                    <x:Static Member="skia:SKEncodedImageFormat.Jpeg" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ktx" />
                    <x:Static Member="skia:SKEncodedImageFormat.Pkm" />
                    <x:Static Member="skia:SKEncodedImageFormat.Png" />
                    <x:Static Member="skia:SKEncodedImageFormat.Wbmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Webp" />
                </x:Array>
            </Picker.ItemsSource>
        </Picker>

        <Slider x:Name="qualitySlider"
                Maximum="100"
                Value="50" />

        <Label Text="{Binding Source={x:Reference qualitySlider},
                              Path=Value,
                              StringFormat='Quality = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal">
            <Label Text="Folder Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="folderNameEntry"
                   Text="SaveFileFormats"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="File Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="fileNameEntry"
                   Text="Sample.xxx"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <Button Text="Save" 
                Clicked="OnButtonClicked">
            <Button.Triggers>
                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference formatPicker},
                                               Path=SelectedIndex}"
                             Value="-1">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>

                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference fileNameEntry},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Button.Triggers>
        </Button>

        <Label x:Name="statusLabel"
               Text="OK"
               Margin="10, 0" />
    </StackLayout>
</ContentPage>
```

Arka plan kod dosyasında bir bit eşlem kaynağındaki yükler ve kullandığı `SKCanvasView` görüntülenecek. Hiçbir zaman değişiklik bit eşlem. `SelectedIndexChanged` İşleyicisi `Picker` numaralandırma üyesi aynı uzantılı bir dosya adı değiştirir:

```csharp
public partial class SaveFileFormatsPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(typeof(SaveFileFormatsPage),
        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    public SaveFileFormatsPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        args.Surface.Canvas.DrawBitmap(bitmap, args.Info.Rect, BitmapStretch.Uniform);
    }

    void OnFormatPickerChanged(object sender, EventArgs args)
    {
        if (formatPicker.SelectedIndex != -1)
        {
            SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
            fileNameEntry.Text = Path.ChangeExtension(fileNameEntry.Text, imageFormat.ToString());
            statusLabel.Text = "OK";
        }
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
        int quality = (int)qualitySlider.Value;

        using (MemoryStream memStream = new MemoryStream())
        using (SKManagedWStream wstream = new SKManagedWStream(memStream))
        {
            bitmap.Encode(wstream, imageFormat, quality);
            byte[] data = memStream.ToArray();

            if (data == null)
            {
                statusLabel.Text = "Encode returned null";
            }
            else if (data.Length == 0)
            {
                statusLabel.Text = "Encode returned empty array";
            }
            else
            {
                bool success = await DependencyService.Get<IPhotoLibrary>().
                    SavePhotoAsync(data, folderNameEntry.Text, fileNameEntry.Text);

                if (!success)
                {
                    statusLabel.Text = "SavePhotoAsync return false";
                }
                else
                {
                    statusLabel.Text = "Success!";
                }
            }
        }
    }
}
```

`Clicked` İşleyicisi `Button` gerçek çalışır. İçin iki bağımsız değişkeni alır `Encode` gelen `Picker` ve `Slider`ve ardından oluşturmak için daha önce gösterilen kodu kullanan bir `SKManagedWStream` için `Encode` yöntemi. İki `Entry` görünümleri için klasör ve dosya adlarını furnish `SavePhotoAsync` yöntemi.

Bu yöntem çoğunu zamanının sorunları veya hataları işlemek için. Varsa `Encode` boş bir dizi oluşturur belirli dosya biçimi desteklenmiyorsa anlamına gelir. Varsa `SavePhotoAsync` döndürür `false`, sonra da dosyayı başarıyla kaydedilmemiş. 

Üç platformlarda çalışan bir program şöyledir:

[![Dosya biçimleri Kaydet](saving-images/SaveFileFormats.png "dosya biçimlerini kaydetme")](saving-images/SaveFileFormats-Large.png#lightbox)

Bu ekran görüntüsünde bu platformlarda desteklenir yalnızca üç biçimleri gösterilmiştir:

- JPEG
- PNG
- WebP

Tüm diğer biçimlere için `Encode` yöntemi hiçbir şey akışa yazar ve sonuç bayt dizisinin boştur.

Bit eşlem, **dosya biçimlerini kaydetme** olan 600 piksel kare sayfa kaydeder. Piksel başına 4 bayt ile 1,440,000 bayt cinsinden bellek toplam olmasıdır. Aşağıdaki tabloda, çeşitli dosya biçimi ve kalite bileşimleri için dosya boyutu gösterilmektedir:

|Biçimi|Kalite|Boyut|
|------|------:|---:|
| PNG | Yok | 492K |
| JPEG | 0 | 2,95 K |
|      | 50 | 22.1 K |
|      | 100 | 206K |
| WebP | 0 | 2,71 K |
|      | 50 | 11,9 K |
|      | 100 | 101K |

Çeşitli kalite ayarları ile denemeler yapın ve sonuçları inceleyin.

## <a name="saving-finger-paint-art"></a>Finger-Paint resim kaydetme

Yaygın olarak kullanıldığı bir bit eşlem olan bir şey adlı burada işlevleri programlar, çizim bir _gölge bit eşlem_. Tüm çizim, program tarafından görüntülenir bit eşlem üzerinde tutulur. Bit eşlem de çizim kaydetmek için faydalı olur.

[ **Parmak boyama içinde SkiaSharp** ](../paths/finger-paint.md) makale touch ilkel dokunmalı program uygulamak için izleme kullanma gösterilmektedir. Programın yalnızca bir renk ve yalnızca bir darbe genişliği desteklenir, ancak tüm koleksiyonu içinde çizim korunur `SKPath` nesneleri.

**Parmak boyama ile Kaydet** sayfasını [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) örnek de korur koleksiyonu tüm çizim `SKPath` , ama aynı zamanda nesneleri Fotoğraf kitaplığınıza kaydedebilirsiniz bir bit eşlem çizimdeki işler.

Bu program çoğunu özgün benzer **parmak boyama** program. Bir geliştirmedir XAML dosyası artık etiketli düğmeleri başlatır **Temizle** ve **Kaydet**:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Bitmaps.FingerPaintSavePage"
             Title="Finger Paint Save">

    <StackLayout>
        <Grid BackgroundColor="White"
              VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
        </Grid>

        <Button Text="Clear"
                Grid.Row="0"
                Margin="50, 5"
                Clicked="OnClearButtonClicked" />

        <Button Text="Save"
                Grid.Row="1"
                Margin="50, 5"
                Clicked="OnSaveButtonClicked" />

    </StackLayout>
</ContentPage>
```

Türünde bir alan arka plan kod dosyası tutar `SKBitmap` adlı `saveBitmap`. Bu bit eşlem oluşturulduğunda veya de yeniden `PaintSurface` görüntü boyutunu yüzey değiştiğinde işleyici. Bit eşlem yeniden oluşturulması gerekiyorsa, böylece her şeyi uzaklaştırabilir boyutu nasıl değiştiğini fark etmeksizin saklanır, var olan bit eşlem içeriğini yeni bit eşleme kopyalanır:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    SKBitmap saveBitmap;

    public FingerPaintSavePage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        // Create bitmap the size of the display surface
        if (saveBitmap == null)
        {
            saveBitmap = new SKBitmap(info.Width, info.Height);
        }
        // Or create new bitmap for a new size of display surface
        else if (saveBitmap.Width < info.Width || saveBitmap.Height < info.Height)
        {
            SKBitmap newBitmap = new SKBitmap(Math.Max(saveBitmap.Width, info.Width),
                                              Math.Max(saveBitmap.Height, info.Height));

            using (SKCanvas newCanvas = new SKCanvas(newBitmap))
            {
                newCanvas.Clear();
                newCanvas.DrawBitmap(saveBitmap, 0, 0);
            }

            saveBitmap = newBitmap;
        }

        // Render the bitmap
        canvas.Clear();
        canvas.DrawBitmap(saveBitmap, 0, 0);
    }
    ···
}
```

Tarafından çizim `PaintSurface` işleyicisi çok sonunda oluşur ve yalnızca bir bit eşlem rendering oluşur.

Önceki program touch işleme benzer. Program iki koleksiyon tutar `inProgressPaths` ve `completedPaths`, kullanıcı görüntü temizlenmiş son daraltılmasından çizilmiş her şeyi içeren. Her dokunma olay için `OnTouchEffectAction` işleyicisi çağrılarını `UpdateBitmap`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                            (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }

    void UpdateBitmap()
    {
        using (SKCanvas saveBitmapCanvas = new SKCanvas(saveBitmap))
        {
            saveBitmapCanvas.Clear();

            foreach (SKPath path in completedPaths)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }

            foreach (SKPath path in inProgressPaths.Values)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }
        }

        canvasView.InvalidateSurface();
    }
    ···
}
```

`UpdateBitmap` Yöntemi yuva `saveBitmap` yeni bir oluşturarak `SKCanvas`temizlemek ve ardından tüm yollarında bit eşlem oluşturma. Geçersiz kılmalarını tarafından sonucuna `canvasView` böylece ekranda bit eşlem kurulabilir.

İki düğme için işleyiciler aşağıda verilmiştir. **Temizle** düğmesi her iki yol koleksiyon güncelleştirmeleri temizler `saveBitmap` (bit eşlem temizlenmesine sonuçlanır) ve geçersiz kılar `SKCanvasView`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    void OnClearButtonClicked(object sender, EventArgs args)
    {
        completedPaths.Clear();
        inProgressPaths.Clear();
        UpdateBitmap();
        canvasView.InvalidateSurface();
    }

    async void OnSaveButtonClicked(object sender, EventArgs args)
    {
        using (SKImage image = SKImage.FromBitmap(saveBitmap))
        {
            SKData data = image.Encode();
            DateTime dt = DateTime.Now;
            string filename = String.Format("FingerPaint-{0:D4}{1:D2}{2:D2}-{3:D2}{4:D2}{5:D2}{6:D3}.png",
                                            dt.Year, dt.Month, dt.Day, dt.Hour, dt.Minute, dt.Second, dt.Millisecond);

            IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
            bool result = await photoLibrary.SavePhotoAsync(data.ToArray(), "FingerPaint", filename);

            if (!result)
            {
                await DisplayAlert("FingerPaint", "Artwork could not be saved. Sorry!", "OK");
            }
        }
    }
}
```

**Kaydet** düğmesi işleyicisi kullanan Basitleştirilmiş [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.Encode()/) yönteminden `SKImage`. Bu yöntem, PNG biçiminde kodlar. `SKImage` Nesnesi temel alınarak oluşturulur `saveBitmap`ve `SKData` nesne kodlanmış bir PNG dosya içerir. 

`ToArray` Yöntemi `SKData` bayt dizisini alır. Bu ne geçirilir, `SavePhotoAsync` sabit klasör adı ve geçerli tarih ve saat oluşturulan benzersiz bir dosya adı ile birlikte bir yöntem.

Eylem program şöyledir:

[![Boya kaydetme finger](saving-images/FingerPaintSave.png "Finger Boya Kaydet")](saving-images/FingerPaintSave-Large.png#lightbox)

Çok benzer bir teknik kullanılır [ **SpinPaint** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/) örnek. Ardından, bir dört quadrants tasarımlarına yakalandığı dönen diskte kullanıcı boyar bu da dokunmalı program olmasıdır. Parmak boyama değişiklikleri rengini disk olarak dönen:

[![Boya döndürme](saving-images/SpinPaint.png "döndürme boyama")](saving-images/SpinPaint-Large.png#lightbox)

**Kaydet** düğmesini `SpinPaint` sınıfı benzer **parmak boyama** içeren bir sabit klasör adı için resmi kaydeder (**SpainPaint**) ve oluşturulan bir dosya adı Tarih ve saat.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [SpinPaint (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/)
