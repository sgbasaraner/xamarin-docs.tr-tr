---
title: Cihazın video kitaplığı erişimi
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: eb3d66630613225c9b2becaa20f73a82f409ce7e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="accessing-the-devices-video-library"></a>Cihazın video kitaplığı erişimi

Çoğu modern mobil aygıtları ve masaüstü bilgisayarları cihazın kamera kullanarak kayıt videolara sahipsiniz. Bir kullanıcı oluşturur videolar sonra aygıtta dosyaları olarak depolanır. Bu dosyalar Resim Kitaplığı'ndan alınan ve tarafından yürütülen `VideoPlayer` gibi herhangi bir video sınıfı.

## <a name="the-photo-picker-dependency-service"></a>Fotoğraf Seçici bağımlılık hizmeti

Her üç platformları, kullanıcının bir fotoğraf veya video cihazın Resim Kitaplığı'ndan seçmesine olanak veren bir olanağı içerir. Cihazın Resim Kitaplığı'ndan bir video oynatma ilk adımı, her platformda Resmi Seçici çağıran bir bağımlılık hizmeti oluşturuyor. Aşağıda açıklanan bağımlılık hizmeti bir tanımlanmış çok benzer [ **Resim Kitaplığı'ndan bir fotoğraf çekme** ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) video seçici bir yerinebirdosyaadıverirancakbumakale`Stream`nesnesi.

PCL proje adlı bir arabirim tanımlar `IVideoPicker` bağımlılık hizmeti için:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

Her üç platformları adlı bir sınıf içerir `VideoPicker` bu arabirimi uygular.

### <a name="the-ios-video-picker"></a>İOS video Seçici

İOS `VideoPicker` iOS kullanan [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) iOS videoları ("filmler" adlandırılır) için kısıtlanması gerektiğini belirten resim kitaplığı erişmek için `MediaType` özelliği. Dikkat `VideoPicker` açıkça uygulayan `IVideoPicker` arabirimi. Ayrıca fark `Dependency` bağımlılık hizmet olarak bu sınıfı tanımlayan özniteliği. Bu, bağımlılık hizmeti platform projesinde bulmak Xamarin.Forms sağlayan iki gereksinimleri şunlardır:

```csharp
using System;
using System.Threading.Tasks;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.iOS.VideoPicker))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPicker : IVideoPicker
    {
        TaskCompletionSource<string> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<string> GetVideoFileAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.SavedPhotosAlbum,
                MediaTypes = new string[] { "public.movie" }
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<string>();
            return taskCompletionSource.Task;
        }

        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            if (args.MediaType == "public.movie")
            {
                taskCompletionSource.SetResult(args.MediaUrl.AbsoluteString);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}
```

### <a name="the-android-video-picker"></a>Android video Seçici

Android uygulaması `IVideoPicker` uygulamanın etkinlik parçası olan bir geri çağırma yöntemi gerektirir. Bu nedenle, `MainActivity` sınıfı, iki özellik, bir alan ve bir geri çağırma yöntemi tanımlar:

```csharp
namespace VideoPlayerDemos.Droid
{
    ···
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            Current = this;
            ···
        }

        // Field, properties, and method for Video Picker
        public static MainActivity Current { private set; get; }

        public static readonly int PickImageId = 1000;

        public TaskCompletionSource<string> PickImageTaskCompletionSource { set; get; }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);

            if (requestCode == PickImageId)
            {
                if ((resultCode == Result.Ok) && (data != null))
                {
                    // Set the filename as the completion of the Task
                    PickImageTaskCompletionSource.SetResult(data.DataString);
                }
                else
                {
                    PickImageTaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`OnCreate` Yönteminde `MainActivity` statik kendi örneği depolar `Current` özelliği. Bu uygulanması sağlar `IVideoPicker` almak için `MainActivity` başlatmak için örnek **seçin Video** Seçici:

```csharp
using System;
using System.Threading.Tasks;
using Android.Content;
using Xamarin.Forms;

// Need application's MainActivity
using VideoPlayerDemos.Droid;

[assembly: Dependency(typeof(FormsVideoLibrary.Droid.VideoPicker))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPicker : IVideoPicker
    {
        public Task<string> GetVideoFileAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("video/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = MainActivity.Current;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Video"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<string>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Eklemeler `MainActivity` nesne olan tek kodda [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) normal uygulama kodu nereye gereken desteklemek için değiştirilmesi çözüm `FormsVideoLibrary` sınıfları.

### <a name="the-uwp-video-picker"></a>UWP video Seçici

UWP uygulaması `IVideoPicker` arabirimi kullanan UWP [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/). Dosya arama resim kitaplığı ile başlar ve MP4 ve WMV (Windows Media Video) dosya türlerini kısıtlar:

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Pickers;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.UWP.VideoPicker))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPicker : IVideoPicker
    {
        public async Task<string> GetVideoFileAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary
            };

            openPicker.FileTypeFilter.Add(".wmv");
            openPicker.FileTypeFilter.Add(".mp4");

            // Get a file and return the path
            StorageFile storageFile = await openPicker.PickSingleFileAsync();
            return storageFile?.Path;
        }
    }
}
```

## <a name="invoking-the-dependency-service"></a>Bağımlılık hizmeti çağırma

**Kitaplığı Video Yürüt** sayfasında [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) program video Seçici bağımlılık hizmetini kullanmayı gösterir. XAML dosyasını içeren bir `VideoPlayer` örneği ve `Button` etiketli **Göster Video Kitaplığı**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayLibraryVideoPage"
             Title="Play Library Video">
    <StackLayout>
        <video:VideoPlayer x:Name="videoPlayer"
                           VerticalOptions="FillAndExpand" />

        <Button Text="Show Video Library"
                Margin="10"
                HorizontalOptions="Center"
                Clicked="OnShowVideoLibraryClicked" />
    </StackLayout>
</ContentPage>
```

Arka plan kodu dosyasını içeren `Clicked` işleyicisi `Button`. Bağımlılık hizmetini çağırmak için bir çağrı gerektirir `DependencyService.Get` uygulanması elde etmek için bir `IVideoPicker` platform projesinde arabirimi. `GetVideoFileAsync` Bu örneğinde yöntemi çağrıldıktan sonra:

```csharp
namespace VideoPlayerDemos
{
    public partial class PlayLibraryVideoPage : ContentPage
    {
        public PlayLibraryVideoPage()
        {
            InitializeComponent();
        }

       async void OnShowVideoLibraryClicked(object sender, EventArgs args)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();

            if (!String.IsNullOrWhiteSpace(filename))
            {
                videoPlayer.Source = new FileVideoSource
                {
                    File = filename
                };
            }

            btn.IsEnabled = true;
        }
    }
}
```

`Clicked` İşleyici sonra kullanır, dosya adı oluşturmak için bir `FileVideoSource` nesne ve ayarlamak için `Source` özelliği `VideoPlayer`.

Her biri `VideoPlayerRenderer` sınıfları içerir kodda kendi `SetSource` yöntemi türündeki nesneler için `FileVideoSource`. Bunlar aşağıda gösterilir:

### <a name="handling-ios-files"></a>İOS dosya işleme

İOS sürümü `VideoPlayerRenderer` işlemleri `FileVideoSource` statik kullanarak nesneleri `Asset.FromUrl` yöntemi dosya adı. Bu bir `AVAsset` cihazın görüntü kitaplığı dosyasında temsil eden nesnesi:

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
            else if (Element.Source is FileVideoSource)
            {
                string uri = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-android-files"></a>Android dosyaları işleme

Nesne türü işlenirken `FileVideoSource`, Android uygulaması `VideoPlayerRenderer` kullanan `SetVideoPath` yöntemi `VideoView` cihazın Resim Kitaplığı'nda dosyası belirtmek için:

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
            else if (Element.Source is FileVideoSource)
            {
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    videoView.SetVideoPath(filename);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-uwp-files"></a>UWP dosyaları işleme

Nesne türü işlerken `FileVideoSource`, UWP uygulaması `SetSource` yönteminin gerekir oluşturmak bir `StorageFile` nesnesi, bu dosya için okuma açın ve akış nesnesine aktarmak `SetSource` yöntemi `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                // Code requires Pictures Library in Package.appxmanifest Capabilities to be enabled
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    StorageFile storageFile = await StorageFile.GetFileFromPathAsync(filename);
                    IRandomAccessStreamWithContentType stream = await storageFile.OpenReadAsync();
                    Control.SetSource(stream, storageFile.ContentType);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

Tüm üç platformları için hemen sonra video oynatma video başlar kaynak dosya cihazda ve indirilmesi gerekmez çünkü ayarlanır.



## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı gösterilerini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [Resim Kitaplığı'ndan bir fotoğraf çekme](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
