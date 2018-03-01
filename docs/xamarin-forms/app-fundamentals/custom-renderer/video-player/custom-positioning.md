---
title: "Özel görüntü konumlandırma"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d4446d30491ee796ca93eadf2e107fc9d74748df
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="custom-video-positioning"></a>Özel görüntü konumlandırma

Her platform tarafından uygulanan Aktarım denetimleri bir konum çubuğu içerir. Bu çubuğu kaydırıcı veya kaydırma çubuğu benzer ve toplam süresi içinde video geçerli konumunu gösterir. Ayrıca, kullanıcı ileri ya da geri video yeni bir konuma taşımak için konum çubuğu yönetebilirsiniz.

Bu makale, kendi özel konum çubuğu nasıl uygulayacağınıza dair gösterir.

## <a name="the-duration-property"></a>Duration özelliği

Bilgilerin bir öğe, `VideoPlayer` özel desteklemesi gerekir konumu çubuk video süresi olur. `VideoPlayer` Salt okunur tanımlar `Duration` türünde özellik `TimeSpan`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Duration read-only property
        private static readonly BindablePropertyKey DurationPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Duration), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public static readonly BindableProperty DurationProperty = DurationPropertyKey.BindableProperty;

        public TimeSpan Duration
        {
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        TimeSpan IVideoPlayerController.Duration
        {
            set { SetValue(DurationPropertyKey, value); }
            get { return Duration; }
        }
        ···
    }
}
```

Gibi `Status` özelliği açıklanan [önceki makalede](custom-transport.md), bu `Duration` özelliği salt okunur durumdadır. Özel ile tanımlanan `BindablePropertyKey` ve yalnızca başvurarak ayarlanabilir `IVideoPlayerController` bu içerir arabirimi `Duration` özelliği:

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

Ayrıca adında bir yöntemi çağırır özelliği değiştirildi işleyici fark `SetTimeToEnd` bu makalenin sonraki bölümlerinde açıklanmıştır.

Bir video süresince *değil* hemen sonra kullanılabilir `Source` özelliği `VideoPlayer` ayarlanır. Temel alınan video oynatıcı süresinin belirleyebilirsiniz önce video dosyası kısmen yüklenmelidir.

Her platform Oluşturucu video süresi nasıl alacağını aşağıda verilmiştir:

### <a name="video-duration-in-ios"></a>İOS video süresi

İOS, gelen bir video süresi alınır `Duration` özelliği `AVPlayerItem`, ancak hemen sonra `AVPlayerItem` oluşturulur. Bir iOS gözlemci için ayarlamak mümkündür `Duration` özelliği, ancak `VideoPlayerRenderer` süresi elde `UpdateStatus` 10 kez saniyenin çağrılan yöntemi:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ((IVideoPlayerController)Element).Duration = ConvertTime(playerItem.Duration);
                ···
            }
        }

        TimeSpan ConvertTime(CMTime cmTime)
        {
            return TimeSpan.FromSeconds(Double.IsNaN(cmTime.Seconds) ? 0 : cmTime.Seconds);

        }
        ···
    }
}
```

`ConvertTime` Yöntemi dönüştürür bir `CMTime` nesnesine bir `TimeSpan` değeri.

### <a name="video-duration-in-android"></a>Android video süresi

`Duration` Android özelliğinin `VideoView` milisaniye olarak geçerli bir süre raporları zaman `Prepared` olayı `VideoView` tetiklenir. Android `VideoPlayerRenderer` sınıfını almak için bu işleyici kullanır `Duration` özelliği:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            ···
            ((IVideoPlayerController)Element).Duration = TimeSpan.FromMilliseconds(videoView.Duration);
        }
        ···
    }
}
```

### <a name="video-duration-in-uwp"></a>UWP video süresi

`NaturalDuration` Özelliği `MediaElement` olan bir `TimeSpan` değer ve zaman geçerli hale geldiği `MediaElement` ateşlenir `MediaOpened` olay:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementMediaOpened(object sender, RoutedEventArgs args)
        {
            ((IVideoPlayerController)Element).Duration = Control.NaturalDuration.TimeSpan;
        }
        ···
    }
}
```

## <a name="the-position-property"></a>Konum özelliği

`VideoPlayer` Ayrıca gereken bir `Position` sıfırdan artırır özelliği `Duration` video yürütme gibi. `VideoPlayer` Bu özellik gibi uygulayan `Position` UWP özelliğinde `MediaElement`, ortak normal bağlanabilirse özelliğiyle olduğu `set` ve `get` erişimciler:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Position property
        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }
        ···
    }
}
```

`get` Erişimci çalma gibi video geçerli konumunu döndürür ancak `set` erişimci video konumu ileri ya da geri taşıyarak konumu çubuğu kullanıcının işleme için yanıt verecek şekilde tasarlanmıştır.

İOS ve Android, geçerli konumunu alır özelliği yalnızca sahip bir `get` erişimci ve `Seek` yöntemi, ikinci bu görevi gerçekleştirmek için kullanılabilir. Bu konuda, ayrı bir düşünüyorsanız `Seek` yöntemi tek bir'den daha duyarlı bir yaklaşım olabilir görünüyor `Position` özelliği. Tek bir `Position` özellik devralınmış bir sorun var: video yürütür olarak `Position` özelliği sürekli yeni konumu yansıtacak için güncelleştirilmelidir. Ancak, çoğu değişiklikleri istemediğiniz `Position` video yeni bir konuma taşımak video oynatıcı neden özelliği. Bu, video oynatıcı yanıt son değerini aramayı tarafından ortaya çıkarsa `Position` özelliği ve video olmayacaktır ilerleyin.

Uygulama zorluklar rağmen bir `Position` özelliğiyle `set` ve `get` erişimciler, bu yaklaşım, UWP ile tutarlı olduğundan seçildi `MediaElement`, ve veri bağlama ile büyük bir avantajı vardır: `Position` özelliği `VideoPlayer` konumu görüntülemek ve arama için yeni bir konuma kullanılan kaydırıcıyı bağlanabilir. Ancak, bazı önlemler bu uygularken gerekli `Position` geri bildirim döngüleri önlemek için özellik.

### <a name="setting-and-getting-ios-position"></a>Ayarlama ve iOS konumu alma

İOS içinde `CurrentTime` özelliği `AVPlayerItem` nesnesini video yürütmeyi geçerli konumunu gösterir. İOS `VideoPlayerRenderer` ayarlar `Position` özelliğinde `UpdateStatus` işleyici ayarlar aynı anda `Duration` özelliği:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ···
                ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, ConvertTime(playerItem.CurrentTime));
            }
        }
        ···
    }
}
```

Oluşturucu ne zaman algılar `Position` kümeden özelliği `VideoPlayer` içinde değişti `OnElementPropertyChanged` geçersiz kılabilir ve çağırmak için yeni bir değer kullanır bir `Seek` yöntemi `AVPlayer` nesnesi:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                TimeSpan controlPosition = ConvertTime(player.CurrentTime);

                if (Math.Abs((controlPosition - Element.Position).TotalSeconds) > 1)
                {
                    player.Seek(CMTime.FromSeconds(Element.Position.TotalSeconds, 1));
                }
            }
        }
        ···
    }
}
```

Aklınızda her zaman `Position` özelliğinde `VideoPlayer` kümeden `OnUpdateStatus` işleyicisi `Position` özelliği ateşlenir bir `PropertyChanged` içinde algılandığında olay `OnElementPropertyChanged` geçersiz kılma. Bu değişikliklerin çoğu için `OnElementPropertyChanged` yöntemi bir şey yapmanız. Aksi takdirde, video konumda her bir değişiklikle, yalnızca sınıra aynı konuma taşınması!

Bu geri bildirim döngü önlemek için `OnElementPropertyChanged` yöntemi yalnızca çağırır `Seek` zaman arasındaki farkı `Position` özelliği ve geçerli konumunu `AVPlayer` bir saniyeden daha büyük.

### <a name="setting-and-getting-android-position"></a>Ayarlama ve Android konumu alma

Yalnızca iOS Oluşturucu, Android olduğu gibi `VideoPlayerRenderer` için yeni bir değer ayarlar `Position` özelliğinde `OnUpdateStatus` işleyicisi. `CurrentPosition` Özelliği `VideoView` milisaniye yeni konumunu birimlerindeki içerir:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            TimeSpan timeSpan = TimeSpan.FromMilliseconds(videoView.CurrentPosition);
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, timeSpan);
        }
        ···
    }
}
```

Ayrıca, yalnızca iOS Oluşturucu olduğu gibi Android oluşturucuyu çağırır `SeekTo` yöntemi `VideoView` zaman `Position` özelliği değişti, ancak değişiklik bir saniyeden daha farklı olduğunda yalnızca `CurrentPosition` değerini `VideoView`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs(videoView.CurrentPosition - Element.Position.TotalMilliseconds) > 1000)
                {
                    videoView.SeekTo((int)Element.Position.TotalMilliseconds);
                }
            }
        }
        ···
    }
}
```

### <a name="setting-and-getting-uwp-position"></a>Ayarlama ve UWP konumu alma

UWP `VideoPlayerRenderer` tanıtıcıları `Position` iOS ve Android Oluşturucu aynı şekilde ancak çünkü `Position` UWP özelliğinin `MediaElement` aynı zamanda bir `TimeSpan` değeri, dönüştürme gerekli değildir:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs((Control.Position - Element.Position).TotalSeconds) > 1)
                {
                    Control.Position = Element.Position;
                }
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, Control.Position);
        }
        ···
    }
}
```

## <a name="calculating-a-timetoend-property"></a>TimeToEnd özelliği hesaplama

Bazen video oynatıcı videoda kalan süreyi gösterir. Bu değer, ne zaman video başlar ve video sona erdiğinde aşağıya doğru sıfır azaltır video süre sonunda başlar.

`VideoPlayer` salt okunur içeren `TimeToEnd` tamamen sınıfı içinde işlenir özelliği ilgili değişiklikler temel alınarak `Duration` ve `Position` özellikleri:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        private static readonly BindablePropertyKey TimeToEndPropertyKey =
            BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan());

        public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

        public TimeSpan TimeToEnd
        {
            private set { SetValue(TimeToEndPropertyKey, value); }
            get { return (TimeSpan)GetValue(TimeToEndProperty); }
        }

        void SetTimeToEnd()
        {
            TimeToEnd = Duration - Position;
        }
        ···
    }
}
```

`SetTimeToEnd` Yöntemi, özelliği değiştirildi işleyicilerini hem çağrılır `Duration` ve `Position`.

## <a name="a-custom-slider-for-video"></a>Video için özel bir kaydırıcı

Özel denetim için bir konum çubuğu yazmak veya Xamarin.Forms kullanmak için `Slider` veya türeyen bir sınıf `Slider`, aşağıdaki gibi `PositionSlider` sınıfı. Adlı iki yeni özellik sınıfı tanımlayan `Duration` ve `Position` türü `TimeSpan` aynı adı taşıyan iki özelliği verilere bağlı olarak hedeflenen `VideoPlayer`. Modu bağlama varsayılan dikkat `Position` özelliktir çift yönlü:

```csharp
namespace FormsVideoLibrary
{
    public class PositionSlider : Slider
    {
        public static readonly BindableProperty DurationProperty =
            BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                    });

        public TimeSpan Duration
        {
            set { SetValue(DurationProperty, value); }
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                    defaultBindingMode: BindingMode.TwoWay,
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Value = seconds;
                                    });

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }

        public PositionSlider()
        {
            PropertyChanged += (sender, args) =>
            {
                if (args.PropertyName == "Value")
                {
                    TimeSpan newPosition = TimeSpan.FromSeconds(Value);

                    if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                    {
                        Position = newPosition;
                    }
                }
            };
        }
    }
}
```

Özellik değişti işleyicisi `Duration` özellik kümeleri `Maximum` temel özellik `Slider` için `TotalSeconds` özelliği `TimeSpan` değeri. Özellik değişti işleyicisi benzer şekilde, `Position` ayarlar `Value` özelliği `Slider`. Bu yolla, arka plandaki `Slider` konumunu izler `PositionSlider`.

`PositionSlider` Arka plandaki gelen güncelleştirilmiş `Slider` yalnızca bir örneği: ne zaman kullanıcı yöneten `Slider` video alınacağını veya yeni bir konuma tersine olduğunu belirtmek için. Bu, algılandığında `PropertyChanged` oluşturucusunun işleyicisinde `PositionSlider`. Bir değişiklik işleyici denetler `Value` özelliği ve farklı ise `Position` özelliği, sonra `Position` özellik kümesinden `Value` özelliği.

Teorik, iç `if` deyimi yazılmaya şuna benzer:

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

Ancak, Android uygulaması `Slider` bağımsız olarak, yalnızca 1.000 ayrık adımları sahip `Minimum` ve `Maximum` ayarlar. Bir video uzunluğu 1.000 saniye sonra iki farklı büyük olan `Position` değerleri aynı karşılık `Value` ayarıyla `Slider`ve bu `if` deyimi bir kullanıcı düzenleme için yanlış pozitif tetiklemek `Slider`. Bunun yerine yeni konumu ve varolan konumu yüzde genel Duration büyük olup olmadığını denetleyin. daha güvenlidir.

## <a name="using-the-positionslider"></a>PositionSlider kullanma

UWP belgelerine [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/) bağlama hakkında sizi uyarır `Position` özelliği özelliği sık güncelleştirdiğinden. Belge bir süreölçer kullanılmasını önerir sorguya `Position` özelliği.

Üç ancak iyi bir öneri olan `VideoPlayerRenderer` sınıfları dolaylı olarak zaten kullanıyor süreölçer güncelleştirmek için `Position` özelliği. `Position` Özelliği için bir işleyici değiştirildiğinde `UpdateStatus` ikinci yalnızca 10 kez tetiklenir olay.

Bu nedenle, `Position` özelliği `VideoPlayer` bağlanabilir `Position` özelliği `PositionSlider` performans sorunları, örnekte gösterildiği şekilde olmadan **özel konum çubuğu** sayfa:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomPositionBarPage"
             Title="Custom Position Bar">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource ElephantsDream}" />

        ···

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="10, 0"
                     BindingContext="{x:Reference videoPlayer}">

            <Label Text="{Binding Path=Position,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center"/>

            ···

            <Label Text="{Binding Path=TimeToEnd,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center" />
        </StackLayout>

        <video:PositionSlider Grid.Row="2"
                              Margin="10, 0, 10, 10"
                              BindingContext="{x:Reference videoPlayer}"
                              Duration="{Binding Duration}"           
                              Position="{Binding Position}">
            <video:PositionSlider.Triggers>
                <DataTrigger TargetType="video:PositionSlider"
                             Binding="{Binding Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </video:PositionSlider.Triggers>
        </video:PositionSlider>
    </Grid>
</ContentPage>
```

İlk üç nokta (···) gizler `ActivityIndicator`; önceki olduğu gibi aynı olduğundan **özel aktarım** sayfası. İki fark `Label` görüntüleme öğeleri `Position` ve `TimeToEnd` özellikleri. Bu iki arasında üç nokta `Label` öğeleri gizler iki `Button` gösterilen öğeleri **özel aktarım** sayfasında Oynat, Duraklat ve durdurun. Arka plan kodu mantığı de aynıdır **özel aktarım** sayfası.

[![Özel konumlandırmasını](custom-positioning-images/custompositioning-small.png "özel konumlandırmasını")](custom-positioning-images/custompositioning-large.png "özel konumlandırma")

Bu tartışma sonucuna `VideoPlayer`.

## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı gösterilerini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
