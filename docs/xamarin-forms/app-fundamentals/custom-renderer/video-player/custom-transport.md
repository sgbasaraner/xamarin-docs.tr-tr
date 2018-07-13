---
title: Özel video Aktarım denetimleri
description: Bu makalede, özel Aktarım denetimleri Xamarin.Forms kullanarak bir video oynatıcı uygulamasında uygulamak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 84870de28ffd30b2d29fb5d8fbea815e1fd0d9c4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996444"
---
# <a name="custom-video-transport-controls"></a>Özel video Aktarım denetimleri

Video oynatıcı Aktarım denetimleri işlevleri gerçekleştiren düğmeleri içerir **Play**, **duraklatma**, ve **Durdur**. Bu düğmeler, genellikle metin yerine tanıdık simgeler ile tanımlanır ve **Play** ve **duraklatma** işlevleri genellikle bir düğme birleştirilir.

Varsayılan olarak, `VideoPlayer` Aktarım denetimleri her platformu tarafından desteklenen görüntüler. Ayarladığınızda `AreTransportControlsEnabled` özelliğini `false`, bu denetimlerin görüntülenmez. Daha sonra kontrol edebilirsiniz `VideoPlayer` programlama yoluyla veya kendi Aktarım denetimleri sağlayın.

## <a name="the-play-pause-and-stop-methods"></a>Yürüt, Duraklat ve durdurma yöntemi

`VideoPlayer` Sınıf adlı üç yöntemi tanımlar `Play`, `Pause`, ve `Stop` olayları tetikleme tarafından uygulanır:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        public event EventHandler PlayRequested;

        public void Play()
        {
            PlayRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler PauseRequested;

        public void Pause()
        {
            PauseRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler StopRequested;

        public void Stop()
        {
            StopRequested?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

Bu olayları için olay işleyicileri ayarlanmış `VideoPlayerRenderer` aşağıda gösterildiği gibi platformlarının her birinde sınıfı:

### <a name="ios-transport-implementations"></a>iOS uygulamaları taşıma

İOS sürümü `VideoPlayerRenderer` kullanır `OnElementChanged` bu üç olayları için işleyiciler ayarlamak için yöntemin zaman `NewElement` özelliği değil `null` ve olay işleyicileri ayırır, `OldElement` değil `null`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            player.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            player.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            player.Pause();
            player.Seek(new CMTime(0, 1));
        }
    }
}
```

Olay işleyicileri üzerinde yöntemleri çağırarak uygulanan `AVPlayer` nesne. Yok hiçbir `Stop` yöntemi `AVPlayer`, videoyu duraklatma ve konumu başına taşıyarak benzetimi.

### <a name="android-transport-implementations"></a>Android aktarım uygulamaları

Android uygulaması, iOS uygulamasına benzer. Üç işlev işleyicileri zaman ayarlanır `NewElement` değil `null` ve ne zaman ayrılmış `OldElement` değil `null`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        void OnPlayRequested(object sender, EventArgs args)
        {
            videoView.Start();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            videoView.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            videoView.StopPlayback();
        }
    }
}
```

Üç işlev tarafından tanımlanan yöntemleri çağırma `VideoView`.

### <a name="uwp-transport-implementations"></a>UWP aktarım uygulamaları

Üç aktarım işlevleri UWP uygulaması iOS ve Android uygulamaları için çok benzer:

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
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            Control.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            Control.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            Control.Stop();
        }
    }
}
```

## <a name="the-video-player-status"></a>Video oynatıcı durumu

Uygulama **Play**, **duraklatma**, ve **Durdur** işlevleri Aktarım denetimleri desteklemek için yeterli değildir. Genellikle **Play** ve **duraklatma** komutları video şu anda Yürütülüyor ya da duraklatılmış olup olmadığını belirtmek için görünümünü değiştirir düğmeyle uygulanır. Video henüz yüklü değilse Ayrıca, düğme bile etkin olmamalıdır.

Bu gereksinimler, video oynatıcı geçerli durumunu gösteren yürütülürken veya duraklatıldı durumunda veya henüz bir videoyu oynatmak hazır değilse kullanılabilir hale getirmek gereken kapsıyor. (Üç platformda da video duraklatılıp veya yeni bir konuma taşınmış, ancak içinde desteklenmeyen için bu özellikleri video dosyaları yerine akış video için geçerli olan belirten özellikleri için destek `VideoPlayer` burada açıklanmıştır.)

**VideoPlayerDemos** proje içeren bir `VideoStatus` numaralandırması üç üyesiyle:

```csharp
namespace FormsVideoLibrary
{
    public enum VideoStatus
    {
        NotReady,
        Playing,
        Paused
    }
}
```

`VideoPlayer` Sınıfı tanımlar adlı yalnızca gerçek bir bağlanılabilir özellik `Status` türü `VideoStatus`. Yalnızca platform işleyicisi bağlantısı ayarlanmalıdır, çünkü bu özellik salt okunur tanımlanır:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Status read-only property
        private static readonly BindablePropertyKey StatusPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Status), typeof(VideoStatus), typeof(VideoPlayer), VideoStatus.NotReady);

        public static readonly BindableProperty StatusProperty = StatusPropertyKey.BindableProperty;

        public VideoStatus Status
        {
            get { return (VideoStatus)GetValue(StatusProperty); }
        }

        VideoStatus IVideoPlayerController.Status
        {
            set { SetValue(StatusPropertyKey, value); }
            get { return Status; }
        }
        ···
    }
}
```

Genellikle, bir salt okunur bağlanılabilir özellik özel gerekir `set` erişimcisine `Status` sınıf içinde ayarlanması izin verecek şekilde özelliği. İçin bir `View` oluşturucular tarafından ancak desteklenen Türev özelliği ayarlanmalıdır gelen sınıfın dışında ancak yalnızca platform işleyici tarafından.

Bu nedenle, başka bir özellik adıyla tanımlanan `IVideoPlayerController.Status`. Bu açık arabirim uygulaması olduğundan ve olası tarafından yapılan `IVideoPlayerController` arabirim `VideoPlayer` sınıfının uygular:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

Bu şekilde nasıl benzer [ `WebView` ](xref:Xamarin.Forms.WebView) denetimi kullanan [ `IWebViewController` ](xref:Xamarin.Forms.IWebViewController) uygulamak için arabirimi `CanGoBack` ve `CanGoForward` özellikleri. (Kaynak kodunu [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) ve Ayrıntılar için kendi oluşturucular.)

Bu, harici bir sınıf için mümkün kılar `VideoPlayer` ayarlanacak `Status` başvuran özelliğini `IVideoPlayerController` arabirimi. (Kısa bir süre içinde kod görürsünüz.) Özellik diğer sınıflardan ayarlanabilir, ancak yanlışlıkla ayarlanmış olması beklenmez. En önemlisi de `Status` veri bağlama aracılığıyla özelliği ayarlanamaz.

Oluşturucu içinde tutarak bu yardımcı olmak için `Status` özelliği güncelleştirildi, `VideoPlayer` sınıfı tanımlar bir `UpdateStatus` olay tetiklenen her saniyenin onda biri:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        public event EventHandler UpdateStatus;

        public VideoPlayer()
        {
            Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
            {
                UpdateStatus?.Invoke(this, EventArgs.Empty);
                return true;
            });
        }
        ···
    }
}
```

### <a name="the-ios-status-setting"></a>İOS durum ayarı

İOS `VideoPlayerRenderer` ayarlar için bir işleyici `UpdateStatus` olay (ve bu işleyici ayırır, arka plandaki `VideoPlayer` öğe yok) ve işleyici ayarlamak için kullanır `Status` özelliği:

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
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (player.Status)
            {
                case AVPlayerStatus.ReadyToPlay:
                    switch (player.TimeControlStatus)
                    {
                        case AVPlayerTimeControlStatus.Playing:
                            videoStatus = VideoStatus.Playing;
                            break;

                        case AVPlayerTimeControlStatus.Paused:
                            videoStatus = VideoStatus.Paused;
                            break;
                    }
                    break;
                }
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
            ···
        }
        ···
    }
}
```

İki özelliklerini `AVPlayer` erişilmelidir: [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) türünün özelliği `AVPlayerStatus` ve [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) türünün özelliği `AVPlayerTimeControlStatus`. Dikkat `Element` özelliği (olduğu `VideoPlayer`) e dönüştürmelisiniz `IVideoPlayerController` ayarlanacak `Status` özelliği.

### <a name="the-android-status-setting"></a>Android durum ayarı

[ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Android özelliğini `VideoView` yalnızca video oynatma ya da duraklatılmış olup olmadığını gösteren bir Boole değeri. Belirlemek için `VideoView` hiçbiri play olabilir veya videoyu Duraklat henüz `Prepared` olayı `VideoView` işlenmelidir. Bu iki işleyici kümesinde `OnElementChanged` yöntemi ve ayrılmış sırasında `Dispose` geçersiz kıl:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    videoView.Prepared += OnVideoViewPrepared;
                    ···
                }
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }

        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            if (Element != null)
            {
                Element.UpdateStatus -= OnUpdateStatus;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`UpdateStatus` İşleyicisi kullanır `isPrepared` alan (kümesinde `Prepared` işleyicisi) ve `IsPlaying` ayarlamak için özellik `Status` özelliği:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus status = VideoStatus.NotReady;

            if (isPrepared)
            {
                status = videoView.IsPlaying ? VideoStatus.Playing : VideoStatus.Paused;
            }
            ···
        }
        ···
    }
}
```

### <a name="the-uwp-status-setting"></a>UWP durum ayarı

UWP `VideoPlayerRenderer` kullanır `UpdateStatus` olay, ancak gerekmez, ayarı `Status` özelliği. `MediaElement` Tanımlar bir [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) Oluşturucu sağlayan bir olay olduğunda bildirim almak [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) özelliği değişti. Özelliği, ayrılmış `Dispose` geçersiz kıl:

```csharp
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
                    ···
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                };
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                ···
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`CurrentState` Özelliği türüdür [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate)ve kolayca haritalarınızı `VideoStatus`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementCurrentStateChanged(object sender, RoutedEventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (Control.CurrentState)
            {
                case MediaElementState.Playing:
                    videoStatus = VideoStatus.Playing;
                    break;

                case MediaElementState.Paused:
                case MediaElementState.Stopped:
                    videoStatus = VideoStatus.Paused;
                    break;
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
        }
        ···
    }
}
```

## <a name="play-pause-and-stop-buttons"></a>Yürüt, Duraklat ve düğmeler Durdur

Unicode karakter kullanarak sembolik **Play**, **duraklatma**, ve **Durdur** görüntüleri sorunlu. [Çeşitli teknik](https://unicode-table.com/en/blocks/miscellaneous-technical/) bölümü Unicode standardı, bu amaçla görünüşte uygun üç simge karakterleri tanımlar. Bunlar:

- 0x23F5 (siyah Orta sağ üçgen) veya &#x23F5; için **Yürüt**
- 0x23F8 (çift dikey çubuk) veya &#x23F8; için **Duraklat**
- 0x23F9 (siyah kare) veya &#x23F9; için **Durdur**

Bakılmaksızın nasıl bu sembolleri tarayıcınızda görünür (ve farklı tarayıcılar bunları farklı yollarla işlemek), tutarlı bir şekilde Xamarin.Forms tarafından desteklenen platformlardaki görüntülenmez. İOS ve UWP cihazlarda **duraklatma** ve **Durdur** karakterlerin 3B mavi bir arka plan ve beyaz bir ön plan ile bir grafiksel görünüm vardır. Bu, android, simge yalnızca mavi olduğu durum geçerli değildir. Ancak, 0x23F5 kod noktası için **Play** aynı görünüme ve UWP üzerinde iOS ve Android'de bile desteklenip desteklenmediğini sahip değil.

Bu nedenle, 0x23F5 kod noktası için kullanılamaz **Play**. İyi bir alternatif aşağıdaki gibidir:

- 0x25B6 (siyah üçgen sağı gösteren) veya &#x25B6; için **Yürüt**

3B Görünümü benzemez düz bir siyah üçgen olması dışında bu üç tüm platformlar tarafından desteklenir **duraklatma** ve **Durdur**. Bir seçenek, bir değişken kod ile 0x25B6 kod noktası takip etmektir:

- 0x25B6 0xFE0F (değişken 16) tarafından izlenen veya &#x25B6; &#xFE0F; için **Yürüt**

Aşağıda gösterilen biçimlendirmede kullanılan budur. İos'ta bu verir **Play** aynı 3B görünüme sembol **duraklatma** ve **Durdur** düğmeleri, ancak değişken Android ve UWP üzerinde çalışmaz.

**Özel aktarım** Ayarlar sayfasında **AreTransportControlsEnabled** özelliğini **false** ve içeren bir `ActivityIndicator` video yüklenirken görüntülenen iki düğmeler. `DataTrigger` nesneleri etkinleştirme ve devre dışı bırakmak için kullanılır `ActivityIndicator` ve düğmeler ve ilk düğmeyi arasında geçiş yapmak için **Play** ve **duraklatma**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomTransportPage"
             Title="Custom Transport">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AutoPlay="False"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource BigBuckBunny}" />

        <ActivityIndicator Grid.Row="0"
                           Color="Gray"
                           IsVisible="False">
            <ActivityIndicator.Triggers>
                <DataTrigger TargetType="ActivityIndicator"
                             Binding="{Binding Source={x:Reference videoPlayer},
                                               Path=Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsVisible" Value="True" />
                    <Setter Property="IsRunning" Value="True" />
                </DataTrigger>
            </ActivityIndicator.Triggers>
        </ActivityIndicator>

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="0, 10"
                     BindingContext="{x:Reference videoPlayer}">

            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.Playing}">
                        <Setter Property="Text" Value="&#x23F8; Pause" />
                    </DataTrigger>

                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>

            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

Veri tetikleyicileri makalesinde ayrıntılı olarak açıklanmıştır [veri Tetikleyicileri](~/xamarin-forms/app-fundamentals/triggers.md#data).

Düğme için işleyiciler arka plan kod dosyasına sahip `Clicked` olayları:

```csharp
namespace VideoPlayerDemos
{
    public partial class CustomTransportPage : ContentPage
    {
        public CustomTransportPage()
        {
            InitializeComponent();
        }

        void OnPlayPauseButtonClicked(object sender, EventArgs args)
        {
            if (videoPlayer.Status == VideoStatus.Playing)
            {
                videoPlayer.Pause();
            }
            else if (videoPlayer.Status == VideoStatus.Paused)
            {
                videoPlayer.Play();
            }
        }

        void OnStopButtonClicked(object sender, EventArgs args)
        {
            videoPlayer.Stop();
        }
    }
}
```

Çünkü `AutoPlay` ayarlanır `false` içinde **CustomTransport.xaml** dosyası basın gerekir **Play** düğmesini video başlamak için etkin hale gelir. Yukarıda açıklanan Unicode karakterleri metin eşdeğerleri tarafından yayımlanır böylece düğmeler tanımlanır. Video yürütürken düğmelerin her platformda tutarlı bir görünüm vardır:

[![Özel taşıma yürütmeyi](custom-transport-images/customtransportplaying-small.png "özel taşıma yürütmeyi")](custom-transport-images/customtransportplaying-large.png#lightbox "özel taşıma yürütülüyor")

Ancak Android ve UWP, **Play** video duraklatıldığında düğmesi çok farklı görünüyor:

[![Özel taşıma duraklatıldı](custom-transport-images/customtransportpaused-small.png "özel taşıma duraklatıldı")](custom-transport-images/customtransportpaused-large.png#lightbox "özel taşıma duraklatıldı")

Bir üretim uygulamasında, büyük olasılıkla visual gerekmemesi elde etmek için kendi düğmeleri için bit eşlem resimleri kullanmak isteyebilirsiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
