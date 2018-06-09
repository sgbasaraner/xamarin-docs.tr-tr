---
title: Özel görüntü Aktarım denetimleri
description: Bu makalede, Xamarin.Forms kullanarak bir video oynatıcı uygulamasında özel taşıma denetimler uygulamak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a20c68d5f86dad852a4425206846292c1c6c5838
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241666"
---
# <a name="custom-video-transport-controls"></a>Özel görüntü Aktarım denetimleri

Bir video oynatıcı aktarım denetimlerini işlevleri gerçekleştirmek düğmeleri dahil **Yürüt**, **duraklatma**, ve **durdurmak**. Bu düğmeleri, genellikle metnin yerine tanıdık simgeler ile tanımlanır ve **Yürüt** ve **duraklatma** işlevler tek düğmesinde genellikle birleştirilir.

Varsayılan olarak, `VideoPlayer` görüntüler Aktarım her platformu tarafından desteklenen kontrol eder. Ayarladığınızda `AreTransportControlsEnabled` özelliğine `false`, bu denetimlerin görüntülenmez. Daha sonra denetleyebilir `VideoPlayer` program aracılığıyla veya kendi Aktarım denetimleri sağlayın.

## <a name="the-play-pause-and-stop-methods"></a>Oynatma, duraklatma ve durdurma yöntemleri

`VideoPlayer` Sınıfı tanımlayan üç yöntem adlı `Play`, `Pause`, ve `Stop` olaylarını tetikleme göre uygulanır:

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

Bu olayları için olay işleyicileri tarafından belirlenen `VideoPlayerRenderer` aşağıda gösterildiği gibi her platformda sınıfı:

### <a name="ios-transport-implementations"></a>iOS uygulamaları taşıma

İOS sürümü `VideoPlayerRenderer` kullanan `OnElementChanged` yöntemi işleyiciler üç bu olayların ayarlamak için zaman `NewElement` özelliği değil `null` ve olay işleyicileri ayırır zaman `OldElement` değil `null`:

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

Olay işleyicileri yöntemleri çağırma uygulanan `AVPlayer` nesnesi. Yoktur hiçbir `Stop` yöntemi `AVPlayer`, video duraklatma ve konumu başına taşıyarak benzetimli.

### <a name="android-transport-implementations"></a>Android aktarım uygulamaları

Android uygulaması, iOS uygulamasına benzer. Üç işlevleri için işleyiciler zaman ayarlanır `NewElement` değil `null` ve ne zaman ayrılmış `OldElement` değil `null`:

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

Üç işlevleri tarafından tanımlanan yöntemleri çağırmak `VideoView`.

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

Uygulama **Yürüt**, **duraklatma**, ve **durdurmak** işlevleri Aktarım denetimleri desteklemek için yeterli değil. Genellikle **Yürüt** ve **duraklatma** komutları video şu anda yürütülmekte ya da duraklatılmış olup olmadığını belirtmek için görünümünü değiştirir aynı düğmesi ile uygulanır. Video henüz yüklü değilse Ayrıca, düğme bile etkinleştirilmesi gerekir.

Bu gereksinimleri video oynatıcı geçerli durumunu gösteren çalma ise veya duraklatılmış ya da henüz bir videoyu yürütmek hazır değil kullanılabilir duruma getirmek için gerekli olduğunu belirtir. (Üç platformları da video duraklatılıp veya yeni bir konuma taşındı, ancak bunlar desteklenmez böylece bu özellikleri video dosyaları yerine akış video için geçerli olan belirtmek özellikleri destekleyecek `VideoPlayer` burada açıklanan.)

**VideoPlayerDemos** proje içeren bir `VideoStatus` numaralandırması üç üyeleriyle:

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

`VideoPlayer` Sınıfı tanımlayan adlı yalnızca gerçek bir bağlanabilirse özellik `Status` türü `VideoStatus`. Yalnızca platform Oluşturucu ayarlanmalıdır çünkü bu özellik salt okunur olarak tanımlanır:

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

Genellikle, bir özelliği salt okunur bağlanabilirse özel olurdu `set` erişimcisine `Status` sınıf içinde ayarlanmasına izin vermek için özellik. İçin bir `View` işleyiciler tarafından ancak, desteklenen türevi özelliği ayarlanmalıdır gelen sınıfı dışında ancak yalnızca platform Oluşturucu tarafından.

Bu nedenle, başka bir özellik adıyla tanımlanan `IVideoPlayerController.Status`. Bu bir açık arabirim uygulaması ve tarafından mümkün hale getirilir `IVideoPlayerController` arabirim `VideoPlayer` sınıfını uygular:

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

Bu için nasıl benzer [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) kontrol kullanır [ `IWebViewController` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IWebViewController/) uygulamak için arabirimi `CanGoBack` ve `CanGoForward` özellikleri. (Kaynak kodu bkz [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) ve Ayrıntılar için kendi işleyiciler.)

Bu, harici bir sınıf için mümkün kılar `VideoPlayer` ayarlamak için `Status` başvuran özelliğini `IVideoPlayerController` arabirimi. (Kısa süre içinde kodu görürsünüz.) Özellik diğer sınıflardan ayarlanabilir, ancak yanlışlıkla ayarlanması düşüktür. En önemlisi, `Status` veri bağlama aracılığıyla özelliği ayarlanamaz.

Bu güvenliğini koruma açısından Oluşturucu yardımcı olmak için `Status` özelliği güncelleştirildi, `VideoPlayer` sınıfı tanımlayan bir `UpdateStatus` olan olayı tetikleyen saniyenin her onuncu:

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

### <a name="the-ios-status-setting"></a>İOS durumu ayarı

İOS `VideoPlayerRenderer` için bir işleyici ayarlar `UpdateStatus` olay (ve bu işleyici ayırır, arka plandaki `VideoPlayer` öğesi yok) ve ayarlamak için işleyici kullanan `Status` özelliği:

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

İki özelliklerini `AVPlayer` erişilmelidir: [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) türündeki özelliği `AVPlayerStatus` ve [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) türündeki özelliği `AVPlayerTimeControlStatus`. Dikkat `Element` özelliği (olduğu `VideoPlayer`) için dönüştürülmelidir `IVideoPlayerController` ayarlamak için `Status` özelliği.

### <a name="the-android-status-setting"></a>Android durumu ayarı

[ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Android özelliğinin `VideoView` yalnızca video yürütmeyi ya da duraklatılmış olup olmadığını belirten bir Boole değeri değil. Belirlemek için `VideoView` hiçbiri play olabilir ya da video duraklatmak henüz `Prepared` olayı `VideoView` ele alınması gerekir. Bu iki işleyicileri kümesinde `OnElementChanged` yöntemi ve ayrılan sırasında `Dispose` geçersiz kıl:

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

`UpdateStatus` İşleyici kullanan `isPrepared` alan (kümesinde `Prepared` işleyici) ve `IsPlaying` ayarlamak için özellik `Status` özelliği:

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

### <a name="the-uwp-status-setting"></a>UWP durumu ayarı

UWP `VideoPlayerRenderer` kullanır `UpdateStatus` olay, ancak gerekmez, ayarı `Status` özelliği. `MediaElement` Tanımlayan bir [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) Oluşturucu sağlayan olay ne zaman bildirim almak için [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) özelliği değişti. Özelliği, ayrılmış `Dispose` geçersiz kıl:

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

`CurrentState` Özelliği türüdür [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate)ve kolayca içine eşler `VideoStatus`:

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

## <a name="play-pause-and-stop-buttons"></a>Oynat, Duraklat ve düğmeleri Durdur

Unicode karakterler kullanarak simgesel **Yürüt**, **duraklatma**, ve **durdurmak** görüntüleri sorunlu. [Çeşitli teknik](https://unicode-table.com/en/blocks/miscellaneous-technical/) Unicode standart bölümünü tanımlayan üç sembol karakterlerinin bu amaç için görünen uygun. Bunlar:

- 0x23F5 (siyah Orta sağı üçgen) veya &#x23F5; için **Yürüt**
- 0x23F8 (çift dikey çubuk) veya &#x23F8; için **duraklatma**
- 0x23F9 (siyah kare) veya &#x23F9; için **Durdur**

Ne olursa olsun nasıl bu simgeleri tarayıcınızda görüntülenir (ve farklı tarayıcılar işlemek bunları farklı şekillerde), tutarlı bir şekilde Xamarin.Forms tarafından desteklenen platformlarda görüntülendikleri değil. İOS ve UWP cihazlarda **duraklatma** ve **durdurmak** karakter içerebilir mavi 3B arka plan ve ön plan beyaz grafiksel bir görünüm. Android'de, simge yalnızca mavi olduğu durumda değil. Ancak, için 0x23F5 codepoint **Yürüt** UWP ve onu aynı görünüme iOS ve Android bile desteklenir yok.

Bu nedenle, 0x23F5 codepoint kullanılamaz **Yürüt**. İyi bir alternatif aşağıdaki gibidir:

- 0x25B6 (siyah sağı üçgen) veya &#x25B6; için **Yürüt**

3B görünümünü benzemez düz bir siyah üçgen olması dışında bu üç tüm platformlar tarafından desteklenir **duraklatma** ve **durdurmak**. Bunu yapmanın bir değişken koduyla 0x25B6 codepoint izlemektir:

- 0x25B6 0xFE0F (değişken 16) tarafından izlenen veya &#x25B6; &#xFE0F; için **Yürüt**

Aşağıda gösterilen biçimlendirmede kullanılan budur. İOS, verir **Yürüt** aynı 3B görünüme sembol **duraklatma** ve **durdurmak** düğmeleri, ancak değişken Android ve UWP üzerinde çalışmıyor.

**Özel aktarım** Ayarlar sayfasında **AreTransportControlsEnabled** özelliğine **false** ve içeren bir `ActivityIndicator` video yüklenirken görüntülenen ve iki düğmeler. `DataTrigger` nesneleri etkinleştirme ve devre dışı bırakmak için kullanılan `ActivityIndicator` ve düğmeleri ve ilk düğme arasında geçiş yapmak için **Yürüt** ve **duraklatma**:

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

Veri tetikleyicileri ayrıntılı makalesinde açıklanmıştır [veri Tetikleyicileri](~/xamarin-forms/app-fundamentals/triggers.md#data).

Arka plan kod dosyasına düğmesi işleyicileri var. `Clicked` olayları:

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

Çünkü `AutoPlay` ayarlanır `false` içinde **CustomTransport.xaml** dosyası basın gerekir **Yürüt** düğmesini video başlamak için etkin hale gelir. Yukarıda açıklanan Unicode karakterler metin eşdeğerlerine tarafından yayımlanır böylece düğmeleri tanımlanır. Video yürütürken düğmelerin her platformda tutarlı bir görünüm vardır:

[![Özel taşıma çalma](custom-transport-images/customtransportplaying-small.png "özel taşıma çalma")](custom-transport-images/customtransportplaying-large.png#lightbox "özel taşıma çalma")

Ancak Android ve UWP, **Yürüt** video duraklatıldığında düğmesi çok farklı arar:

[![Özel taşıma duraklatıldı](custom-transport-images/customtransportpaused-small.png "özel taşıma duraklatıldı")](custom-transport-images/customtransportpaused-large.png#lightbox "özel taşıma duraklatıldı")

Bir üretim uygulamasında, büyük olasılıkla visual bütünlüğünü sağlamak için kendi bit eşlem görüntüleri düğmelerinin kullanmak istersiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı gösterilerini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
