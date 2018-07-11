---
title: DataPages ile çalışmaya başlama
description: Bu makalede, Xamarin.Forms DataPages kullanarak basit bir veri odaklı sayfası oluşturmaya başlamak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 1fb8a06111271d453c578cd3d2db97ec8689c995
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38828217"
---
# <a name="getting-started-with-datapages"></a>DataPages ile çalışmaya başlama

![](~/media/shared/preview.png "Bu API, şu anda Önizleme aşamasındadır")

> [!IMPORTANT]
> DataPages gerektiren bir [Xamarin.Forms tema](~/xamarin-forms/user-interface/themes/index.md) işlemek için başvuru.


DataPages Önizleme kullanarak basit bir veri odaklı sayfası oluşturmaya başlamak için aşağıdaki adımları izleyin. Bu Tanıtım kullanır ("olaylar") Önizleme'de bir sabit kodlanmış stili yapılar yalnızca çalışır kodda belirli JSON biçimine sahip.

[![](get-started-images/demo-sml.png "DataPages örnek uygulama")](get-started-images/demo.png#lightbox "DataPages örnek uygulaması")

## <a name="1-add-nuget-packages"></a>1. NuGet paketleri Ekle

Bu Nuget paketlerini Xamarin.Forms .NET Standard kitaplığı ve uygulama projelerinizi ekleyin:

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* Bir tema uygulaması Nuget (örn.) Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2. Tema Başvurusu Ekle

İçinde **App.xaml** dosya, özel bir ekleme `xmlns:mytheme` temanın ve temayı uygulamanın kaynak sözlüğüne birleştirilmiş emin olun:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

**Önemli:** adımlarını izleyin [tema derlemeleri (aşağıda) yükleme](#loadtheme) iOS bazı ortak kod ekleyerek `AppDelegate` ve Android `MainActivity`. Bu, gelecekteki Önizleme sürümünde geliştirilecektir.


## <a name="3-add-a-xaml-page"></a>3. XAML sayfa ekleme

Xamarin.Forms uygulaması için yeni bir XAML sayfası ekleyin ve *temel sınıfını değiştirmek* gelen `ContentPage` için `Xamarin.Forms.Pages.ListDataPage`. Bu, hem C# ve XAML içinde yapılması gerekir:

**C# dosyası**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**XAML dosyası**

Kök öğesi için değiştirme yanı sıra `<p:ListDataPage>` özel ad alanı için `xmlns:p` ayrıca eklenmesi gerekir:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Uygulama öğesinin alt sınıfı**

Değişiklik `App` sınıf oluşturucusu böylece `MainPage` ayarlanmış bir `NavigationPage` içeren yeni `SessionDataPage`. Bir gezinti sayfası *gerekir* kullanılır.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Veri Kaynağı Ekle

Silme `Content` öğesi değiştirin bir `p:ListDataPage.DataSource` sayfanın verilerle doldurmak için. Uzak bir Json aşağıdaki örnekte bir URL'den veri dosyasının yüklendiği.

**Not:** Önizleme *gerektirir* bir `StyleClass` işleme ipuçları için veri kaynağı sağlamak için özniteliği. `StyleClass="Events"` Önizlemede önceden tanımlanmış ve stilleri içeren bir düzene başvuruyor *sabit kodlanmış* kullanılan JSON veri kaynağı eşleştirilecek.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**JSON verileri**

Örnek JSON verileri [tanıtım kaynak](http://demo3143189.mockable.io/sessions) aşağıda gösterilmiştir:

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. Çalıştırın!

Yukarıdaki adımları çalışma veri sayfasındaki sağlamalıdır:

[![](get-started-images/demo-sml.png "DataPages örnek uygulama")](get-started-images/demo.png#lightbox "DataPages örnek uygulaması")

Bunun çalışmasının nedeni önceden oluşturulmuş stili **"Olaylar"** açık tema Nuget paketinin varsa ve (örn. veri kaynağı ile aynı tanımlanan stiller "title", "image", "Sunucu").

"Olaylar" `StyleClass` görüntülemek üzere tasarlanmış `ListDataPage` özel bir denetimle `CardView` denetiminin tanımlandığı Xamarin.Forms.Pages. `CardView` Denetim, üç özelliklere sahiptir: `ImageSource`, `Text`, ve `Detail`. Tema sabit kodlanmış veri kaynağının bu özellikleri görüntülemek için (JSON dosyasından) üç alan bağlamaktır.

## <a name="5-customize"></a>5. Özelleştir

Devralınan stili bir şablon belirtme ve veri kaynağı bağlamalar kullanılarak geçersiz kılınabilir. Özel bir şablon kullanarak yeni her satır için aşağıdaki XAML bildirir `ListItemControl` ve `{p:DataSourceBinding}` dahil edilen sözdizimi **Xamarin.Forms.Pages** Nuget:

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

Sağlayarak bir `DataTemplate` bu kodu geçersiz kılmalar `StyleClass` ve bunun yerine varsayılan düzenini kullanan bir `ListItemControl`.

[![](get-started-images/custom-sml.png "DataPages örnek uygulama")](get-started-images/custom.png#lightbox "DataPages örnek uygulaması")

C# için XAML veri oluşturabilir tercih geliştiriciler kaynak bağlamaları çok (dahil etmeyi unutmayın bir `using Xamarin.Forms.Pages;` deyimi):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


Temalar sıfırdan oluşturmak için biraz daha fazla iş olduğunu (bkz [temalara Kılavuzu](~/xamarin-forms/user-interface/themes/index.md)) ancak gelecekteki Önizleme sürümleri yapacak Bunu yapmak daha kolay.


## <a name="troubleshooting"></a>Sorun giderme

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Dosya veya derleme 'Xamarin.Forms.Theme.Light' veya bağımlılıklarından biri yüklenemedi

Önizleme sürümünde, temalar çalışma zamanında yüklemek mümkün olmayabilir. Aşağıda bu hatayı düzeltmek için ilgili projeleri içinde gösterilen kodu ekleyin.

**iOS**

İçinde **AppDelegate.cs** sonra aşağıdaki satırları ekleyin `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

İçinde **MainActivity.cs** sonra aşağıdaki satırları ekleyin `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```



## <a name="related-links"></a>İlgili bağlantılar

- [DataPagesDemo örnek](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
