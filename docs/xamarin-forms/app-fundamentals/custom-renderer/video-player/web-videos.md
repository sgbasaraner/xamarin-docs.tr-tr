---
title: Bir Web video oynatma
ms.topic: article
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a5a98df4346c8720ae25fae4f27b5294993111c4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="playing-a-web-video"></a>Bir Web video oynatma

`VideoPlayer` Sınıfı tanımlayan bir `Source` video dosyanın kaynağını belirtmek için kullanılan özellik yanı sıra bir `AutoPlay` özelliği. `AutoPlay` Varsayılan ayarı varsa `true`, yani video otomatik olarak sonra çalma başlayacağını `Source` ayarlandı:

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

`Source` Özelliği türüdür `VideoSource`, sonra Xamarin.Forms desenleri [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) soyut sınıf ve üç türevleri [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/), [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/), ve [ `StreamImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/). Akış seçeneği kullanılabilir `VideoPlayer` ancak, iOS ve Android akıştan video oynatma desteklemediği için.

## <a name="video-sources"></a>Görüntü kaynakları

Özet `VideoSource` sınıf türetin üç sınıf örneği yalnızca üç statik yöntemler oluşur `VideoSource`:

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

`UriVideoSource` Sınıfı bir URI'sı ile indirilebilir bir video dosyası belirtmek için kullanılır. Türü tek bir özelliğini tanımlar `string`:

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

Nesne türü işleme `UriVideoSource` aşağıda açıklanmıştır.

`ResourceVideoSource` Sınıfı katıştırılmış kaynak ile aynı zamanda belirtilen platform uygulaması olarak depolanan video dosyalara erişmek için kullanılan bir `string` özelliği:

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

Nesne türü işleme `ResourceVideoSource` makalesinde açıklanan [yüklenirken uygulama kaynak videoları](loading-resources.md). `VideoPlayer` Sınıfı taşınabilir Sınıf Kitaplığı'nda bir kaynak olarak depolanan bir video dosyasını yüklemek için hiçbir olanak sahiptir.

`FileVideoSource` Sınıfı cihazın video Kitaplığı'ndan video dosyalara erişmek için kullanılır. Tek bir özellik de türünde `string`:

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

Nesne türü işleme `FileVideoSource` makalesinde açıklanan [cihazın Video kitaplığı erişimi](accessing-library.md).

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

Bu tür dönüştürücüsünü çağrılan zaman `Source` XAML içindeki bir dizeye özelliğini ayarlayın. Burada `VideoSourceConverter` sınıfı:

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

`ConvertFromInvariantString` Yöntemi çalışır dizeye dönüştürmek bir `Uri` nesnesi. Bu başarılı ve düzeni değilse `file:`, yöntem döndürür sonra bir `UriVideoSource`. Aksi takdirde, döndürür bir `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Video kaynağı ayarlama

Video kaynakları içeren diğer mantığı tek tek platform işleyicilerde uygulanır. Aşağıdaki bölümlerde nasıl platform Oluşturucu videoları oynat Göster zaman `Source` özelliği ayarlanmış bir `UriVideoSource` nesnesi.

### <a name="the-ios-video-source"></a>İOS video kaynağı

İki bölümden `VideoPlayerRenderer` video oynatıcı video kaynağının ayarı karmaşıktır. Xamarin.Forms ilk oluşturduğunda türünde bir nesne `VideoPlayer`, `OnElementChanged` yöntemi ile çağrılır `NewElement` bağımsız değişkenleri nesnesinin özelliğini ayarlamak için `VideoPlayer`. `OnElementChanged` Yöntem çağrılarını `SetSource`:

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

Daha sonra ne zaman hakkında `Source` özelliği değiştirildiğinde, `OnElementPropertyChanged` ile yöntemi çağrıldığında bir `PropertyName` "Kaynak" özelliği ve `SetSource` yeniden adlandırılır.

İOS, türünde bir nesne bir video dosyasını oynatmak için [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) ilk video dosyası kapsüllemek için oluşturduğunuz ve oluşturmak için kullanılan bir [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), hangi sonra karmalayan kapatmak için `AVPlayer`nesnesi. İşte nasıl `SetSource` yöntemi tanıtıcıları `Source` türü olduğunda özelliği `UriVideoSource`:

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

`AutoPlay` Özelliğinin hiçbir analog iOS video sınıfları özelliği sonunda incelenir şekilde `SetSource` çağrılacak yöntem `Play` yöntemi `AVPlayer` nesnesi.

Bazı durumlarda, videolar sayfasıyla sonra yürütmeye devam `VideoPlayer` giriş sayfasına geri gittiğinizde. Video durdurmak için `ReplaceCurrentItemWithPlayerItem` de ayarlanır `Dispose` geçersiz kıl:

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

Android `VideoPlayerRenderer` player video kaynağı ayarlamak gereken zaman `VideoPlayer` önce oluşturulan ve üzeri olduğunda `Source` özelliği değişiklikleri:

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

`SetSource` Yöntemi işler türündeki nesneleri `UriVideoSource` çağırarak `SetVideoUri` üzerinde `VideoView` bir Android ile `Uri` dize URI oluşturulan nesne. `Uri` Sınıfı tam olarak nitelenmiş burada .NET ayırt etmek için `Uri` sınıfı:

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

Varsa davranışını iOS ve Android Oluşturucu arasında bir fark `Source` özelliği `VideoPlayer` ayarlanır `null`, veya `Uri` özelliği `UriVideoSource` ayarlanır `null` ya da boş bir dize. Bir video iOS video oynatıcı yürütülmekte olan varsa ve `Source` ayarlanır `null` (veya bir dize `null` veya boş), `ReplaceCurrentItemWithPlayerItem` çağrılır `null` değeri. Geçerli video değiştirilir ve yürütmeyi durdurur.

Android benzer bir özelliği desteklemiyor. Varsa `Source` özelliği ayarlanmış `null`, `SetSource` yöntemi yalnızca yok sayıyor onu ve geçerli video çalınmaya devam eder.

### <a name="the-uwp-video-source"></a>UWP video kaynağı

UWP `MediaElement` tanımlayan bir `AutoPlay` herhangi bir özellik gibi işleyicide ele özelliği:

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

`SetSource` Özelliği tanıtıcıları bir `UriVideoSource` ayarlayarak nesne `Source` özelliği `MediaElement` bir .NET için `Uri` değeri veya `null` varsa `Source` özelliği `VideoPlayer` ayarlanır `null`:

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

Bu özellikler üç Oluşturucu, uygulama ile bir URL kaynaktan çalmasına mümkündür. **Yürüt Web Video** sayfasındaki [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) program aşağıdaki XAML dosyası tarafından tanımlanan:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter` Sınıfı dizeye dönüştürür bir `UriVideoSource`. Ne zaman gezinmek için **Web Video Yürüt** sayfası, video başlar yükleme ve yeterli bir miktar veri indirilir ve arabelleğe çalma başlatır. Video yaklaşık 10 dakika uzunluğu şöyledir:

[![Web çalmasına](web-videos-images/playwebvideo-small.png "Web Video Oynat")](web-videos-images/playwebvideo-large.png#lightbox "Web Video Oynat")

Kullanılmayan ancak video dokunarak görüntülemek üzere geri her üç platformlar üzerinde bir aktarım denetimlerini Kıs.

Video ayarlayarak otomatik olarak başlatılmasını engelleyebilirsiniz `AutoPlay` özelliğine `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Basın gerekir **Yürüt** video başlamak için Başlat.

Ayarlayarak aktarım denetimlerini görüntüleme benzer şekilde, gizleyebilirsiniz `AreTransportControlsEnabled` özelliğine `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Her iki özellik kümesine varsa `false`, sonra video yürütmeye başlayın olmaz ve başlatmak için hiçbir şekilde olacak! Çağrı gerek `Play` arka plan kodu dosyasından veya makalesinde açıklandığı gibi kendi Aktarım denetimleri oluşturmak için [özel Video Aktarım denetimleri uygulama](custom-transport.md). 

**App.xaml** dosyası, iki ek videoları kaynakları içerir:

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

Bu diğer filmler biri için başvuru açık URL'de değiştirebilirsiniz **PlayWebVideo.xaml** ile dosya bir `StaticResource` durumda biçimlendirme uzantısı `VideoSourceConverter` oluşturmak için gerekli olmayan `UriVideoSource` nesnesi:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Alternatif olarak, ayarlayabileceğiniz `Source` bir video dosyasını özelliğinden bir `ListView`sonraki makalesinde açıklandığı gibi [Video kaynaklarına Player bağlama](source-bindings.md).





## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı gösterilerini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
