---
title: Bir Web videosunu oynatma
description: Bu makalede, Xamarin.Forms kullanarak bir video oynatıcı uygulamasında web videoları oynatın açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 566f056bd616c918ce274b9c7406d94fdc265ea2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994565"
---
# <a name="playing-a-web-video"></a>Bir Web videosunu oynatma

`VideoPlayer` Sınıfı tanımlayan bir `Source` video dosyası kaynağını belirtmek için kullanılan özellik yanı sıra bir `AutoPlay` özelliği. `AutoPlay` Varsayılan ayarı varsa `true`, yani video otomatik olarak sonra yürütmeyi başlayacağını `Source` ayarlandı:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Source property
        public static readonly BindableProperty SourceProperty =
            BindableProperty.Create(nameof(Source), typeof(VideoSource), typeof(VideoPlayer), null);

        [TypeConverter(typeof(VideoSourceConverter))]
        public VideoSource Source
        {
            set { SetValue(SourceProperty, value); }
            get { return (VideoSource)GetValue(SourceProperty); }
        }

        // AutoPlay property
        public static readonly BindableProperty AutoPlayProperty =
            BindableProperty.Create(nameof(AutoPlay), typeof(bool), typeof(VideoPlayer), true);

        public bool AutoPlay
        {
            set { SetValue(AutoPlayProperty, value); }
            get { return (bool)GetValue(AutoPlayProperty); }
        }
        ···
    }
}
```

`Source` Özelliği türüdür `VideoSource`, sonra Xamarin.Forms desenli [ `ImageSource` ](xref:Xamarin.Forms.ImageSource) soyut sınıf ve üç türevleri [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource), [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), ve [ `StreamImageSource` ](xref:Xamarin.Forms.StreamImageSource). Stream seçeneği kullanılabilir `VideoPlayer` ancak, iOS ve Android akıştan video oynatma desteklemediği.

## <a name="video-sources"></a>Görüntü kaynakları

Özet `VideoSource` sınıf, türetilen üç sınıf örneği yalnızca üç statik yöntemler oluşur `VideoSource`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        public static VideoSource FromUri(string uri)
        {
            return new UriVideoSource() { Uri = uri };
        }

        public static VideoSource FromFile(string file)
        {
            return new FileVideoSource() { File = file };
        }

        public static VideoSource FromResource(string path)
        {
            return new ResourceVideoSource() { Path = path };
        }
    }
}
```

`UriVideoSource` Sınıfı bir URI ile indirilebilir bir video dosyası belirtmek için kullanılır. Tek bir özellik türü tanımlayan `string`:

```csharp
namespace FormsVideoLibrary
{
    public class UriVideoSource : VideoSource
    {
        public static readonly BindableProperty UriProperty =
            BindableProperty.Create(nameof(Uri), typeof(string), typeof(UriVideoSource));

        public string Uri
        {
            set { SetValue(UriProperty, value); }
            get { return (string)GetValue(UriProperty); }
        }
    }
}
```

Türündeki nesneler işleme `UriVideoSource` aşağıda açıklanmıştır.

`ResourceVideoSource` Sınıfı katıştırılmış kaynak ile aynı zamanda belirtilen platform uygulaması olarak depolanan video dosyalarına erişmek için kullanılan bir `string` özelliği:

```csharp
namespace FormsVideoLibrary
{
    public class ResourceVideoSource : VideoSource
    {
        public static readonly BindableProperty PathProperty =
            BindableProperty.Create(nameof(Path), typeof(string), typeof(ResourceVideoSource));

        public string Path
        {
            set { SetValue(PathProperty, value); }
            get { return (string)GetValue(PathProperty); }
        }
    }
}
```

Türündeki nesneler işleme `ResourceVideoSource` makalesinde açıklanan [Uygulama kaynağı videolarını yükleme](loading-resources.md). `VideoPlayer` Sınıfında .NET Standard Kitaplığı'nda bir kaynak olarak depolanan bir video dosyası yüklemek için bulunmaz.

`FileVideoSource` Sınıfı cihazın video Kitaplığı'ndan video dosyalarına erişmek için kullanılır. Tek bir özellik de türünde `string`:

```csharp
namespace FormsVideoLibrary
{
    public class FileVideoSource : VideoSource
    {
        public static readonly BindableProperty FileProperty =
                  BindableProperty.Create(nameof(File), typeof(string), typeof(FileVideoSource));

        public string File
        {
            set { SetValue(FileProperty, value); }
            get { return (string)GetValue(FileProperty); }
        }
    }
}
```

Türündeki nesneler işleme `FileVideoSource` makalesinde açıklanan [cihazın Video kitaplığına erişme](accessing-library.md).

`VideoSource` Sınıfı içeren bir `TypeConverter` başvuran öznitelik `VideoSourceConverter`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        ···
    }
}
```

Bu tür dönüştürücüsü çağrılır, `Source` özelliği, XAML içindeki bir dizeye ayarlanır. İşte `VideoSourceConverter` sınıfı:

```csharp
namespace FormsVideoLibrary
{
    public class VideoSourceConverter : TypeConverter
    {
        public override object ConvertFromInvariantString(string value)
        {
            if (!String.IsNullOrWhiteSpace(value))
            {
                Uri uri;
                return Uri.TryCreate(value, UriKind.Absolute, out uri) && uri.Scheme != "file" ?
                                VideoSource.FromUri(value) : VideoSource.FromResource(value);
            }

            throw new InvalidOperationException("Cannot convert null or whitespace to ImageSource");
        }
    }
}
```

`ConvertFromInvariantString` Yöntemi çalışır dizeye dönüştürülecek bir `Uri` nesne. Bu başarılı ve düzeni değilse `file:`, sonra da bu yöntemi döndürür bir `UriVideoSource`. Aksi halde bir `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Video kaynağı ayarlama

Video kaynakları içeren diğer mantıksal ayrı bir platform işleyicilerde uygulanır. Aşağıdaki bölümlerde nasıl platform Oluşturucu videoları oynatın Göster zaman `Source` özelliği bir `UriVideoSource` nesne.

### <a name="the-ios-video-source"></a>İOS video kaynağı

İki bölümden `VideoPlayerRenderer` video oynatıcı video kaynağı ayarı kullanılır. Xamarin.Forms ilk oluşturduğunda türünde bir nesne `VideoPlayer`, `OnElementChanged` yöntemi çağrıldığında `NewElement` bağımsız değişkenleri nesnesinin özelliğini ayarlamak için `VideoPlayer`. `OnElementChanged` Yöntem çağrılarını `SetSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

Daha sonra üzerinde ne zaman `Source` özelliği değiştirildiğinde `OnElementPropertyChanged` yöntemi çağrıldığında bir `PropertyName` "Kaynak" özelliği ve `SetSource` yeniden adlandırılır.

İOS, türünde bir nesne bir video dosyasını oynatmak için [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) video dosyası kapsüllemek için ilk oluşturulur ve oluşturmak için kullanılan bir [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), hangi ardından devredildiği kapalı için `AVPlayer`nesne. İşte nasıl `SetSource` yöntemi tanıtıcıları `Source` türü olduğunda özellik `UriVideoSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        ···
        void SetSource()
        {
            AVAsset asset = null;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
            if (asset != null)
            {
                playerItem = new AVPlayerItem(asset);
            }
            else
            {
                playerItem = null;
            }

            player.ReplaceCurrentItemWithPlayerItem(playerItem);

            if (playerItem != null && Element.AutoPlay)
            {
                player.Play();
            }
        }
        ···
    }
}
```

`AutoPlay` Özelliği özellik sonunda incelenir. Bu nedenle bu yok analog iOS video sınıfları sahip `SetSource` çağrılacak yöntem `Play` metodunda `AVPlayer` nesne.

Bazı durumlarda, videoları sayfası sonra yürütmeyi devam `VideoPlayer` giriş sayfasına gitme. Video durdurmak için `ReplaceCurrentItemWithPlayerItem` de ayarlanır `Dispose` geçersiz kıl:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void Dispose(bool disposing)
        {
            base.Dispose(disposing);

            if (player != null)
            {
                player.ReplaceCurrentItemWithPlayerItem(null);
            }
        }
        ···
    }
}
```

### <a name="the-android-video-source"></a>Android video kaynağı

Android `VideoPlayerRenderer` video oynatıcı kaynağı için gerekli olduğunda `VideoPlayer` önce oluşturulan ve üzeri olduğunda `Source` özellik değişiklikleri:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

`SetSource` Yöntemi işler türünden nesnelerin `UriVideoSource` çağırarak `SetVideoUri` üzerinde `VideoView` bir Android `Uri` URI dizeden oluşturulan nesne. `Uri` Sınıfı tam burada. NET'ten ayırmak için `Uri` sınıfı:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···

            if (hasSetSource && Element.AutoPlay)
            {
                videoView.Start();
            }
        }
        ···
    }
}
```

Android `VideoView` karşılık gelen yok `AutoPlay` özelliği, bu nedenle `Start` yeni video ayarlarsanız yöntemi çağrılır.

Varsa davranışını iOS ve Android oluşturucular arasında bir fark `Source` özelliği `VideoPlayer` ayarlanır `null`, veya `Uri` özelliği `UriVideoSource` ayarlanır `null` ya da boş bir dize. İOS video oynatıcı şu anda bir video yürütülüyorsa ve `Source` ayarlanır `null` (veya bir dize `null` veya boş), `ReplaceCurrentItemWithPlayerItem` çağrılır `null` değeri. Geçerli görüntü değiştirilir ve yürütmeyi durdurur.

Android benzer bir özelliği desteklemiyor. Varsa `Source` özelliği `null`, `SetSource` yöntemi yalnızca göz ardı eder, ve geçerli video çalınmaya devam eder.

### <a name="the-uwp-video-source"></a>UWP video kaynağı

UWP `MediaElement` tanımlayan bir `AutoPlay` özelliği Oluşturucu herhangi bir özellik gibi işlenir:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                SetAutoPlay();
                ···    
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            else if (args.PropertyName == VideoPlayer.AutoPlayProperty.PropertyName)
            {
                SetAutoPlay();
            }
            ···
        }
        ···
    }
}
```

`SetSource` Özelliği tanıtıcıları bir `UriVideoSource` ayarlayarak nesne `Source` özelliği `MediaElement` kullanarak bir .NET `Uri` değeri veya `null` varsa `Source` özelliği `VideoPlayer` ayarlanır `null`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    Control.Source = new Uri(uri);
                    hasSetSource = true;
                }
            }
            ···
            if (!hasSetSource)
            {
                Control.Source = null;
            }
        }

        void SetAutoPlay()
        {
            Control.AutoPlay = Element.AutoPlay;
        }
        ···
    }
}
```

## <a name="setting-a-url-source"></a>Bir URL kaynağı ayarlama

Bu özellikler, üç Oluşturucu uygulamasıyla bir URL kaynağından bir videoyu oynatmak mümkündür. **Web videosunu Oynat** sayfasını [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) programı aşağıdaki XAML dosyası tarafından tanımlanır:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter` Sınıfı dizeye dönüştürür bir `UriVideoSource`. Ne zaman ulaşmanıza **Web Video oynatın** sayfasında video başlar yükleme ve yeterli veri miktarının indirilir ve arabelleğe yürütmeyi başlatır. Video için yaklaşık 10 dakika uzunluğunda olduğunda:

[![Web çalmasına](web-videos-images/playwebvideo-small.png "Web videosunu Oynat")](web-videos-images/playwebvideo-large.png#lightbox "Web videosunu Oynat")

Kullanılmayan ancak video dokunarak görüntülemek üzere geri her üç platformları üzerinde bir Aktarım denetimleri Kıs.

Video ayarlayarak otomatik olarak başlatılmasını engelleyebilirsiniz `AutoPlay` özelliğini `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Tuşuna basmanız gerekir **Play** videoyu başlatmak için düğme.

Benzer şekilde, ayarlayarak Aktarım denetimleri görüntülenmesini gözardı edebileceğini `AreTransportControlsEnabled` özelliğini `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Her iki özellik ayarlamanız `false`, sonra video yürütmeye başlamak olmaz ve başlatmak için yolu olacak! Çağrısı yapması `Play` arka plan kod dosyasından veya kendi Aktarım denetimleri makalesinde açıklandığı gibi oluşturmak için [özel Video Aktarım denetimleri uygulama](custom-transport.md).

**App.xaml** dosya iki ek video kaynakları içerir:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.App">
    <Application.Resources>
        <ResourceDictionary>

            <video:UriVideoSource x:Key="ElephantsDream"
                                  Uri="https://archive.org/download/ElephantsDream/ed_hd_512kb.mp4" />

            <video:UriVideoSource x:Key="BigBuckBunny"
                                  Uri="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

            <video:UriVideoSource x:Key="Sintel"
                                  Uri="https://archive.org/download/Sintel/sintel-2048-stereo_512kb.mp4" />

        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Bu diğer filmler biri için başvuru açık URL'de değiştirebilirsiniz **PlayWebVideo.xaml** ile dosya bir `StaticResource` durumda işaretleme uzantısı `VideoSourceConverter` oluşturmak için gerekli olmayan `UriVideoSource` nesnesi:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Alternatif olarak, ayarlayabileceğiniz `Source` bir video dosyasını özelliğinden bir `ListView`sonraki makalede açıklandığı gibi [Video Oynatıcı kaynaklarına bağlama](source-bindings.md).





## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
