---
title: Player video kaynaklarına bağlama
description: Bu makalede, video kaynakları Xamarin.Forms kullanarak video oynatıcı bağlamak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: b0efdc1a20f52231f15b7a08eb86962e2079c678
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240036"
---
# <a name="binding-video-sources-to-the-player"></a>Player video kaynaklarına bağlama

Zaman `Source` özelliği `VideoPlayer` görünümü için yeni bir video dosyası ayarlama, varolan video yürütmeyi durdurur ve yeni video başlar. Bu tarafından gösterilen **seçin Web Video** sayfasında [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) örnek. Sayfa içeren bir `ListView` içinden başvuruda üç videolar başlıklarını ile **App.xaml** dosyası:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.SelectWebVideoPage"
             Title="Select Web Video">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0" />

        <ListView Grid.Row="1"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Elephant's Dream</x:String>
                    <x:String>Big Buck Bunny</x:String>
                    <x:String>Sintel</x:String>
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

Bir video seçildiğinde `ItemSelected` olay işleyicisi arka plan kod dosyasına gerçekleştirilir. İşleyici boşluklar ve kesme başlığını kaldırır ve tanımlanan kaynaklardan biri elde etmek için bir anahtar olarak kullanır, **App.xaml** dosya. Olduğunu `UriVideoSource` nesne ayarlanmış sonra `Source` özelliği `VideoPlayer`.

```csharp
namespace VideoPlayerDemos
{
    public partial class SelectWebVideoPage : ContentPage
    {
        public SelectWebVideoPage()
        {
            InitializeComponent();
        }

        void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
        {
            if (args.SelectedItem != null)
            {
                string key = ((string)args.SelectedItem).Replace(" ", "").Replace("'", "");
                videoPlayer.Source = (UriVideoSource)Application.Current.Resources[key];
            }
        }
    }
}
```

Sayfa ilk kez yüklediğinde öğe seçilir `ListView`, bir video yürütmeye başlayın seçmeniz gerekir:

[![Web Video seçin](source-bindings-images/selectwebvideo-small.png "seçin Web Video")](source-bindings-images/selectwebvideo-large.png#lightbox "Web Video seçin")

`Source` Özelliği `VideoPlayer` veri bağlamanın hedef olabileceği anlamına gelir bir bağlanabilirse özelliği tarafından desteklenir. Bu tarafından gösterilen **VideoPlayer için bağ** sayfası. Biçimlendirme **BindToVideoPlayer.xaml** dosyası bir video ve karşılık gelen bir başlık yalıtır aşağıdaki sınıfı tarafından desteklenen `VideoSource` nesnesi:

```csharp
namespace VideoPlayerDemos
{
    public class VideoInfo
    {
        public string DisplayName { set; get; }

        public VideoSource VideoSource { set; get; }

        public override string ToString()
        {
            return DisplayName;
        }
    }
}
```

`ListView` İçinde **BindToVideoPlayer.xaml** dosyasını içeren bir dizi bu `VideoInfo` nesneleri, bir video başlık başlatılan her birini ve `UriVideoSource` kaynak sözlükte nesneden  **App.XAML**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VideoPlayerDemos"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.BindToVideoPlayerPage"
             Title="Bind to VideoPlayer">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           Source="{Binding Source={x:Reference listView},
                                            Path=SelectedItem.VideoSource}" />

        <ListView x:Name="listView"
                  Grid.Row="1">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:VideoInfo}">
                    <local:VideoInfo DisplayName="Elephant's Dream"
                                     VideoSource="{StaticResource ElephantsDream}" />

                    <local:VideoInfo DisplayName="Big Buck Bunny"
                                     VideoSource="{StaticResource BigBuckBunny}" />

                    <local:VideoInfo DisplayName="Sintel"
                                     VideoSource="{StaticResource Sintel}" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

`Source` Özelliği `VideoPlayer` bağlı `ListView`. `Path` Bağlamasının olarak belirtilen `SelectedItem.VideoSource`, iki özelliklerini oluşan bileşik bir yol olduğu: `SelectedItem` bir özelliktir `ListView`. Seçilen öğe türünde `VideoInfo`, sahip olduğu bir `VideoSource` özelliği.

İlk olduğu gibi **seçin Web Video** sayfasında, öğe başlangıçta seçili gelen `ListView`, çalma başlamadan önce videolar birini seçmeniz gerekir.


## <a name="related-links"></a>İlgili bağlantılar

- [Video oynatıcı gösterilerini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
