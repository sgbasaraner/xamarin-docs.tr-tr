---
title: DataPages ile çalışmaya başlama
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: d5e73839f889234c816bfff08f3e46dade8dffc9
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846847"
---
# <a name="getting-started-with-datapages"></a>DataPages ile çalışmaya başlama

![](~/media/shared/preview.png "Bu API şu anda önizlemede değil")

> [!IMPORTANT]
> DataPages gerektiren bir [Xamarin.Forms tema](~/xamarin-forms/user-interface/themes/index.md) işlemek için başvuru.


Basit veri sürücüsü sayfasını DataPages Önizleme kullanarak oluşturmaya başlamak için aşağıdaki adımları izleyin. Yalnızca Önizleme'de kodlanmış stili ("olay") derlemeler bu demo kullanır çalışır kodu belirli JSON biçiminde ile.

[![](get-started-images/demo-sml.png "DataPages örnek uygulama")](get-started-images/demo.png#lightbox "DataPages örnek uygulama")

## <a name="1-add-nuget-packages"></a>1. NuGet paketleri ekleme

Bu Nuget paketleri Xamarin.Forms .NET standart kitaplığı ve uygulama projelerinizi ekleyin:

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* Bir tema uygulaması Nuget (ör.) Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2. Tema başvuru ekleme

İçinde **App.xaml** dosya, özel bir ekleme `xmlns:mytheme` temanın ve tema uygulamanın kaynak sözlükteki birleştirilmiş emin olun:

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

**Önemli:** adımları izlemeniz gereken [tema derlemeler (aşağıda) yükleme](#loadtheme) iOS bazı Demirbaş kod ekleyerek `AppDelegate` ve Android `MainActivity`. Bu, gelecekteki Önizleme sürümünde geliştirilmiş.


## <a name="3-add-a-xaml-page"></a>3. XAML sayfası Ekle

Xamarin.Forms uygulaması için yeni bir XAML sayfa ekleyin ve *temel sınıfını değiştirme* gelen `ContentPage` için `Xamarin.Forms.Pages.ListDataPage`. Bu, hem C# ve XAML yapılması gerekir:

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

Kök öğesi için değiştirme yanı sıra `<p:ListDataPage>` özel ad alanı için `xmlns:p` de eklenmesi gerekir:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Uygulama alt sınıfı**

Değişiklik `App` sınıfı oluşturucusu böylece `MainPage` ayarlanmış bir `NavigationPage` içeren yeni `SessionDataPage`. Gezinti sayfasında *gerekir* kullanılabilir.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Veri kaynağı ekleme

Silme `Content` öğesi ve bunların yerine bir `p:ListDataPage.DataSource` sayfanın verilerle doldurmak için. Uzak bir Json aşağıdaki örnekte veri dosyası bir URL'den yükleniyor.

**Not:** Önizleme *gerektirir* bir `StyleClass` işleme ipuçları için veri kaynağı sağlamak üzere özniteliği. `StyleClass="Events"` Önizlemede önceden tanımlanmış ve stilleri içeren bir düzene başvuruyor *sabit kodlanmış* kullanılan JSON veri kaynağı eşleşecek şekilde.

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

**JSON veri**

JSON verileri örneği [demo kaynak](http://demo3143189.mockable.io/sessions) aşağıda gösterilmiştir:

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

Yukarıdaki adımları çalışma veri sayfasındaki neden:

[![](get-started-images/demo-sml.png "DataPages örnek uygulama")](get-started-images/demo.png#lightbox "DataPages örnek uygulama")

Bu çalışır çünkü önceden derlenmiş stili **"Olayları"** açık tema Nuget paketi varsa ve veri kaynağı (ör. aynı tanımlanan stiller "title", "Görüntü", "Sunucu").

"Olayları" `StyleClass` görüntülemek için yerleşik `ListDataPage` özel bir denetimle `CardView` kontrol tanımlıysa Xamarin.Forms.Pages. `CardView` Denetimi üç özellik vardır: `ImageSource`, `Text`, ve `Detail`. Temanın sabit kodlanmış görüntülenmesi için bu özellikleri (JSON dosyasından) alanları üç veri bağlamaktır.

## <a name="5-customize"></a>5. Özelleştir

Devralınan stili şablon belirtme ve veri kaynağı bağlamalar kullanılarak geçersiz kılınabilir. Özel bir şablon kullanarak yeni her satır için XAML bildirir `ListItemControl` ve `{p:DataSourceBinding}` dahil sözdizimi **Xamarin.Forms.Pages** Nuget:

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

Sağlayarak bir `DataTemplate` bu kodu geçersiz kılmaları `StyleClass` ve bunun yerine varsayılan düzeni kullanan bir `ListItemControl`.

[![](get-started-images/custom-sml.png "DataPages örnek uygulama")](get-started-images/custom.png#lightbox "DataPages örnek uygulama")

C# XAML için veri oluşturabilirsiniz tercih geliştiricilerin kaynak bağlamaları çok (dahil etmeyi unutmayın bir `using Xamarin.Forms.Pages;` deyimi):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


Temalar sıfırdan oluşturmak için biraz daha fazla iş olduğu (bkz [Temalar Kılavuzu](~/xamarin-forms/user-interface/themes/index.md)) ancak gelecekteki preview sürümlerinde yapacak Bunu yapmak daha kolay.


## <a name="troubleshooting"></a>Sorun giderme

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Dosya veya derleme 'Xamarin.Forms.Theme.Light' ya da bağımlılıklarından biri yüklenemedi

Önizleme sürümünde Temalar çalışma zamanında yük mümkün olmayabilir. İlgili projelere bu hatayı düzeltmek için aşağıdaki kodu ekleyin.

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
