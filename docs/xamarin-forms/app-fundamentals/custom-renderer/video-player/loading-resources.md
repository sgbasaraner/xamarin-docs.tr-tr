---
title: "Uygulama kaynak videoları yükleniyor"
ms.topic: article
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 4556c053eec4b28ea863743720fe346a57da8997
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="loading-application-resource-videos"></a>Uygulama kaynak videoları yükleniyor

Özel oluşturucu için `VideoPlayer` görünümü uygulama kaynakları olarak tek tek platform projelerinde katıştırılmış video dosyaları çalma yeteneğine sahiptir. Ancak, geçerli sürümü `VideoPlayer` taşınabilir Sınıf Kitaplığı'nda katıştırılmış kaynaklarına erişemez.

Bu kaynakları yüklemek için bir örneğini oluşturmak `ResourceVideoSource` ayarlayarak `Path` dosya adını (veya klasör ve dosya adı) kaynak özelliği. Alternatif olarak, statik çağırabilirsiniz `VideoSource.FromResource` referans yöntemi. Ardından, `ResourceVideoSource` nesnesini `Source` özelliği `VideoPlayer`. 

## <a name="storing-the-video-files"></a>Video dosyaların depolanması

Platform projesinde bir video dosyasını depolayan üç platformlar için farklıdır:

### <a name="ios-video-resources"></a>iOS video kaynakları

Bir iOS projesi bir video depolayabilir **kaynakları** klasörü veya alt **kaynakları** klasör. Video dosyası olmalıdır bir `Build Action` , `BundleResource`. Ayarlama `Path` özelliği `ResourceVideoSource` bu gibi bir durumda dosya için **MyFile.mp4** bir dosya için **kaynakları** klasörünü veya **MyFolder/MyFile.mp4**, Burada **Klasörüm'ün** bir alt klasörü **kaynakları**.

İçinde **VideoPlayerDemos** çözümü **VideoPlayerDemos.iOS** projeyi içeren bir alt klasörü **kaynakları** adlı **videolar** adlı bir dosyayı içeren **iOSApiVideo.mp4**. İOS için belgeleri bulmak için Xamarin web sitesi kullanmayı gösterir kısa bir video budur `AVPlayerViewController` sınıfı.

### <a name="android-video-resources"></a>Android video kaynakları

Android projesinde, bir alt klasöründe videolar depolanmalıdır **kaynakları** adlı **ham**. **Ham** klasör, alt klasörler içeremez. Video dosyası vermek bir `Build Action` , `AndroidResource`. Ayarlama `Path` özelliği `ResourceVideoSource` bu gibi bir durumda dosya için **MyFile.mp4**. 

**VideoPlayerDemos.Android** projeyi içeren bir alt klasörü **kaynakları** adlı **ham**, adlı bir dosyayı içeren **AndroidApiVideo.mp4**. 

### <a name="uwp-video-resources"></a>UWP video kaynakları

Bir evrensel Windows platformu projesinde Proje herhangi bir klasörde videoları depolayabilirsiniz. Dosyaya verin bir `Build Action` , `Content`. Ayarlama `Path` özelliği `ResourceVideoSource` klasör ve dosya adı, örneğin, **MyFolder/MyVideo.mp4**. 

**VideoPlayerDemos.UWP** projeyi içeren adlı bir klasör **videolar** dosyayla **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Video dosyaları yükleniyor

Her bir platform Oluşturucu sınıfı kodunu içerir, `SetSource` kaynaklar olarak depolanan video dosyaları yükleme yöntemi.

### <a name="ios-resource-loading"></a>iOS kaynak yükleme

İOS sürümü `VideoPlayerRenderer` kullanan `GetUrlForResource` yöntemi `NSBundle` kaynak yüklenmesi için. Tam yol, bir dosya adı, uzantı ve dizin ayrılmalıdır. Kod kullanan `Path` .NET sınıfında `System.IO` dosya yolu şu bileşenlerine bölmek için ad alanı:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhitespace(path))
                {
                    string directory = Path.GetDirectoryName(path);
                    string filename = Path.GetFileNameWithoutExtension(path);
                    string extension = Path.GetExtension(path).Substring(1);
                    NSUrl url = NSBundle.MainBundle.GetUrlForResource(filename, extension, directory);
                    asset = AVAsset.FromUrl(url);
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="android-resource-loading"></a>Android kaynak yükleme

Android `VideoPlayerRenderer` oluşturmak için dosya adı ve paket adı kullanan bir `Uri` nesnesi. Paket adı uygulama bu durumda adıdır **VideoPlayerDemos.Android**, hangi elde edilebilir statik `Context.PackageName` özelliği. Sonuç `Uri` nesne için geçirilen sonra `SetVideoURI` yöntemi `VideoView`:

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
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string package = Context.PackageName;
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string filename = Path.GetFileNameWithoutExtension(path).ToLowerInvariant();
                    string uri = "android.resource://" + package + "/raw/" + filename;
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="uwp-resource-loading"></a>UWP kaynak yükleme

UWP `VideoPlayerRenderer` oluşturan bir `Uri` nesne yolu için ve ayarlar `Source` özelliği `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = "ms-appx:///" + (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    Control.Source = new Uri(path);
                    hasSetSource = true;
                }
            }
        }
        ···
    }
}
```

## <a name="playing-the-resource-file"></a>Kaynak dosyanın çalma

**Çalmak Video kaynak** sayfasındaki **VideoPlayerDemos** çözüm kullanır `OnPlatform` sınıfı her platform için video belirtin:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayVideoResourcePage"
             Title="Play Video Resource">
    <video:VideoPlayer>
        <video:VideoPlayer.Source>
            <video:ResourceVideoSource>
                <video:ResourceVideoSource.Path>
                    <OnPlatform x:TypeArguments="x:String">
                        <On Platform="iOS" Value="Videos/iOSApiVideo.mp4" />
                        <On Platform="Android" Value="AndroidApiVideo.mp4" />
                        <On Platform="UWP" Value="Videos/UWPApiVideo.mp4" />
                    </OnPlatform>
                </video:ResourceVideoSource.Path>
            </video:ResourceVideoSource>
        </video:VideoPlayer.Source>
    </video:VideoPlayer>
</ContentPage>
```

İOS kaynak depolanıyorsa **kaynakları** klasörünü ve UWP kaynak projenin kök klasöründe depolanıyorsa, üç platformlar için aynı adı kullanabilirsiniz. Böyle sonra bu adı doğrudan ayarlayabilirsiniz `Source` özelliği `VideoPlayer`. 

Bu sayfa üç platformlarda çalışan şöyledir:

[![Video kaynak yürütmek](loading-resources-images/playvideoresource-small.png "çalmak Video kaynak")](loading-resources-images/playvideoresource-large.png "Video kaynak Yürüt")

Şimdi gördünüz nasıl [videolar Web URI'den yük](web-videos.md) ve katıştırılmış kaynakları yürütmek nasıl. Buna ek olarak, şunları yapabilirsiniz [videolar cihazın video Kitaplığı'ndan yük](accessing-library.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı gösterilerini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
