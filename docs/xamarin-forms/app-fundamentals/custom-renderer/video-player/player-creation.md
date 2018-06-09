---
title: Platform video oynatıcı oluşturma
description: Bu makalede, bir video oynatıcı özel Oluşturucu Xamarin.Forms kullanarak her platformda uygulamak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 205adf802bc0fc496d79e2b9df4a4360e6c27dc0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241130"
---
# <a name="creating-the-platform-video-players"></a>Platform video oynatıcı oluşturma

[ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) çözüm Xamarin.Forms için bir video oynatıcı uygulamak için tüm kod içerir. Ayrıca, uygulama içinde video oynatıcı kullanılacağını anlatan bir dizi sayfa içerir. Tüm `VideoPlayer` kodu ve platform Oluşturucu adlı proje klasörlerde bulunan `FormsVideoLibrary`ve ad alanı de `FormsVideoLibrary`. Bu, kendi uygulamasına dosyaları kopyalayın ve sınıflar başvuru kolaylaştırmak.

## <a name="the-video-player"></a>Video oynatıcı

[ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) Sınıftır parçası **VideoPlayerDemos** platformları arasında paylaşılan .NET standart kitaplığı. Öğesinden türetilen `View`:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
    }
}
```

Bu sınıf üyeleri (ve `IVideoPlayerController` arabirimi) izleyin makalelerinde açıklanmaktadır.

Her üç platformları adlı bir sınıf içerir `VideoPlayerRenderer` bir video oynatıcı uygulamak için platforma özgü kodu içerir. Bu oluşturucu birincil görevi, platform için bir video oynatıcı oluşturmaktır.

### <a name="the-ios-player-view-controller"></a>İOS player görünümü denetleyicisi

Birden fazla sınıf, bir video oynatıcı iOS uygularken ilgilidir. Uygulamayı ilk kez oluşturur bir [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) ve ardından ayarlar [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/) türünde bir nesne için özellik [ `AVPlayer` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/). Bir video kaynağı player atandığında ek sınıfları gereklidir.

Gibi tüm işleyiciler, iOS [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs) içeren bir `ExportRenderer` tanımlayan özniteliği `VideoPlayer` Oluşturucu görünümüyle:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using AVFoundation;
using AVKit;
using CoreMedia;
using Foundation;
using UIKit;

using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.iOS.VideoPlayerRenderer))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
    }
}
```

Bir platform denetimi ayarlar bir işleyici türeyen genellikle [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) sınıfı, burada `View` Xamarin.Forms olan `View` türevi (Bu durumda, `VideoPlayer`) ve `NativeView` bir iOS `UIView` Oluşturucu sınıfı için türetilen. Bu oluşturucu, bu genel bağımsız değişken yalnızca ayarlanır `UIView`, nedeniyle kısa bir süre sonra göreceksiniz.

Ne zaman bir işleyici temel bir `UIViewController` türevi (Bu durumdaki) sınıfı geçersiz kılmalıdır sonra `ViewController` özelliği ve bu durumda görünüm denetleyicisini return `AVPlayerViewController`. Diğer bir deyişle amacı `_playerViewController` alan:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Create AVPlayerViewController
                    _playerViewController = new AVPlayerViewController();

                    // Set Player property to AVPlayer
                    player = new AVPlayer();
                    _playerViewController.Player = player;

                    // Use the View from the controller as the native control
                    SetNativeControl(_playerViewController.View);
                }
                ···
            }
        }
        ···
    }
}
```

Birincil sorumluluğu `OnElementChanged` geçersiz kılma olup olmadığını denetlemek için `Control` özelliği `null` ve bu durumda, bir platform denetimi oluşturun ve ona geçirin `SetNativeControl` yöntemi. Bu durumda, söz konusu nesne yalnızca kullanılabilir `View` özelliği `AVPlayerViewController`. Olduğunu `UIView` türevi olur adlı özel bir sınıf olarak `AVPlayerView`, ancak özel olduğundan, açıkça ikinci genel bağımsız değişkeni olarak belirtilemez `ViewRenderer`.

Genellikle `Control` Oluşturucu sınıfın özelliği bundan sonra başvurduğu `UIView` Oluşturucu uygulamak için kullanılan, ancak bu durumda `Control` özelliği başka bir yerde kullanılmaz.

### <a name="the-android-video-view"></a>Android video görünümü

Android Oluşturucusu `VideoPlayer` Android tabanlı [ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) sınıfı. Ancak, varsa `VideoView` için alan ayrılan çalmasına bir Xamarin.Forms uygulamada, video dolgular kendisi tarafından kullanılan `VideoPlayer` doğru en boy oranını koruyarak olmadan. Bu nedenle (kısa süre içinde göreceğiniz gibi), `VideoView` bir Android alt yapılan `RelativeLayout`. A `using` yönergesi tanımlar `ARelativeLayout` Xamarin.Forms ayırt etmek için `RelativeLayout`, ve ikinci genel bağımsız değişken `ViewRenderer`:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using Android.Content;
using Android.Media;
using Android.Widget;
using ARelativeLayout = Android.Widget.RelativeLayout;

using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.Droid.VideoPlayerRenderer))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        public VideoPlayerRenderer(Context context) : base(context)
        {
        }
        ···
    }
}
```

Xamarin.Forms 2.5 başlayarak, Android Oluşturucu bir oluşturucu içermelidir bir `Context` bağımsız değişkeni.

`OnElementChanged` Geçersiz kılma oluşturur hem de `VideoView` ve `RelativeLayout` ve düzeni parametrelerini ayarlar `VideoView` içinde orta `RelativeLayout`.


```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Save the VideoView for future reference
                    videoView = new VideoView(Context);

                    // Put the VideoView in a RelativeLayout
                    ARelativeLayout relativeLayout = new ARelativeLayout(Context);
                    relativeLayout.AddView(videoView);

                    // Center the VideoView in the RelativeLayout
                    ARelativeLayout.LayoutParams layoutParams =
                        new ARelativeLayout.LayoutParams(LayoutParams.MatchParent, LayoutParams.MatchParent);
                    layoutParams.AddRule(LayoutRules.CenterInParent);
                    videoView.LayoutParameters = layoutParams;

                    // Handle a VideoView event
                    videoView.Prepared += OnVideoViewPrepared;

                    // Use the RelativeLayout as the native control
                    SetNativeControl(relativeLayout);
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            base.Dispose(disposing);
        }

        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
    }
}
```

İçin bir işleyici `Prepared` olay Bu yöntemde bağlı ve içinde ayrılmış `Dispose` yöntemi. Bu olay zaman ateşlenir `VideoView` bir video dosyasını yürütmeye başlamak için yeterli bilgiye sahip olacaktır.

### <a name="the-uwp-media-element"></a>UWP medya öğesi

Evrensel Windows Platformu (UWP içinde), en yaygın video oynatıcı olan [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/). Bu belge, `MediaElement` belirten [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/) bunun yerine yalnızca Windows 10 1607 yapı başlayarak sürümleri desteklemek için gerekli olduğunda kullanılmalıdır.

`OnElementChanged` Geçersiz kılma gereken oluşturmak bir `MediaElement`birkaç olay işleyicilerini ayarlama ve geçirin `MediaElement` nesnesine `SetNativeControl`:

```csharp
using System;
using System.ComponentModel;

using Windows.Storage;
using Windows.Storage.Streams;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.UWP.VideoPlayerRenderer))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    MediaElement mediaElement = new MediaElement();
                    SetNativeControl(mediaElement);

                    mediaElement.MediaOpened += OnMediaElementMediaOpened;
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                Control.MediaOpened -= OnMediaElementMediaOpened;
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }        
        ···
    }
}
```

İki olay işleyicileri içinde ayrılmış `Dispose` olay işleyici için.

## <a name="showing-the-transport-controls"></a>Aktarım denetimleri gösterme

Üç platformlarda dahil tüm video oynatıcı çalma ve duraklatma ve video içindeki geçerli konumu gösterir ve yeni bir konuma taşımak için bir çubuğu düğmelerini içeren Aktarım denetimleri varsayılan kümesini destekler.

`VideoPlayer` Sınıfı tanımlayan adlı bir özellik `AreTransportControlsEnabled` ve varsayılan değer olarak ayarlar `true`:


```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // AreTransportControlsEnabled property
        public static readonly BindableProperty AreTransportControlsEnabledProperty =
            BindableProperty.Create(nameof(AreTransportControlsEnabled), typeof(bool), typeof(VideoPlayer), true);

        public bool AreTransportControlsEnabled
        {
            set { SetValue(AreTransportControlsEnabledProperty, value); }
            get { return (bool)GetValue(AreTransportControlsEnabledProperty); }
        }
        ···
    }
}
```

Bu özellik her ikisi de olsa `set` ve `get` erişimciler, oluşturucu sahip yalnızca özelliği ayarlandığında durumlarında. `get` Erişimci yalnızca özelliğin geçerli değeri döndürür.

Gibi özellikler `AreTransportControlsEnabled` platform Oluşturucu iki yolla ele alınır:

- Xamarin.Forms oluşturduğunda, ilk kez olan bir `VideoPlayer` öğesi. Bu belirtilen `OnElementChanged` Oluşturucu geçersiz kılma zaman `NewElement` özelliği `null`. Şu anda Oluşturucu ayarlayabilirsiniz sınıfında tanımlandığı gibi kendi platform video oynatıcı özelliğin ilk değeri olan `VideoPlayer`.

- Varsa özelliğinde `VideoPlayer` daha sonra değiştirir, sonra `OnElementPropertyChanged` işleyicide yöntemi çağrılır. Bu yeni özellik ayarına göre platform video oynatıcı güncelleştirmek Oluşturucu sağlar.

İşte nasıl `AreTransportControlsEnabled` özelliği, üç platformlarda gerçekleştirilir:

### <a name="ios-playback-controls"></a>iOS kayıttan yürütme denetimleri

İOS özelliği `AVPlayerViewController` görüntü yöneten taşıma denetimleri olan [ `ShowsPlaybackControls` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/). İşte bu özelliğinin iOS nasıl belirlendiğini `VideoViewRenderer`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            ((AVPlayerViewController)ViewController).ShowsPlaybackControls = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

`Element` Renderer özelliği başvurduğu `VideoPlayer` sınıfı.

### <a name="the-android-media-controller"></a>Android medya denetleyicisi

Android, aktarım denetimlerini görüntüleme oluşturmak gerekir bir [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) nesne ve onunla ilişkili `VideoView` nesnesi. Mekanizması içinde gösterilmiştir `SetAreTransportControlsEnabled` yöntemi:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            if (Element.AreTransportControlsEnabled)
            {
                mediaController = new MediaController(Context);
                mediaController.SetMediaPlayer(videoView);
                videoView.SetMediaController(mediaController);
            }
            else
            {
                videoView.SetMediaController(null);

                if (mediaController != null)
                {
                    mediaController.SetMediaPlayer(null);
                    mediaController = null;
                }
            }
        }
        ···
    }
}
```

### <a name="the-uwp-transport-controls-property"></a>UWP Aktarım denetimleri özelliği

UWP `MediaElement` adlı bir özelliğini tanımlar [ `AreTransportControlsEnabled` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled), böylece özelliği kümeden `VideoPlayer` özelliği aynı ada sahip:

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
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            Control.AreTransportControlsEnabled = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Bir video yürütmeye başlamak bir daha fazla özelliği gereklidir: Bu önemlidir `Source` video bir dosyaya başvuruda bulunan özellik. Uygulama `Source` özelliği sonraki makalede açıklanan [Web Video oynatma](web-videos.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı gösterilerini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
