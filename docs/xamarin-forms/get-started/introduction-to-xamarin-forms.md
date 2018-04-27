---
title: Xamarin.Forms giriş
description: Xamarin.Forms bir platformlar arası geliştiricilerin kolayca Android, iOS ve evrensel Windows platformu arasında paylaşılan kullanıcı arabirimleri oluşturmanıza olanak tanır UI Araç Seti soyutlama yerel olarak yedeklenen ' dir. Kullanıcı arabirimleri, her platform için uygun görünüm ve kullanımında korumak Xamarin.Forms uygulamaların izin verecek şekilde hedef platformu yerel denetimleri kullanarak işlenir. Bu makale Xamarin.Forms ve bu uygulamalarla yazmaya başlamak nasıl bir giriş sağlar.
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 9f7c9d1b410d9d1d699644148903fdc6cfeec4fd
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="an-introduction-to-xamarinforms"></a>Xamarin.Forms giriş

_Xamarin.Forms bir platformlar arası geliştiricilerin kolayca Android, iOS, Windows ve evrensel Windows platformu arasında paylaşılan kullanıcı arabirimleri oluşturmanıza olanak tanır UI Araç Seti soyutlama yerel olarak yedeklenen ' dir. Kullanıcı arabirimleri, her platform için uygun görünüm ve kullanımında korumak Xamarin.Forms uygulamaların izin verecek şekilde hedef platformu yerel denetimleri kullanarak işlenir. Bu makale Xamarin.Forms ve bu uygulamalarla yazmaya başlamak nasıl bir giriş sağlar._

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

Xamarin.Forms geliştiricilerin hızla platformlar arası kullanıcı arabirimleri oluşturmalarına olanak sağlayan bir çerçevedir. Bu, iOS, Android veya evrensel Windows Platformu (UWP) yerel denetimlerini kullanarak işlenen kullanıcı arabirimi için kendi soyutlama sağlar. Bu uygulamaları kendi kullanıcı arabirimi kodu büyük bir kısmı paylaşabilir ve hala hedef platformu yerel Görünüm ve yapısını korur anlamına gelir.

Xamarin.Forms karmaşık uygulamalar için zamanla gelişmesi uygulamaların hızlı prototipi oluşturulurken sağlar. Xamarin.Forms uygulamaları yerel uygulamalardır çünkü bunlar tarayıcı korumalı alan, sınırlı API'leri veya düşük performans gibi diğer araç takımları sınırlamaları yok. Xamarin.Forms kullanılarak yazılmış uygulamalar için herhangi bir verecek API'nin veya temel platform özelliklerini gibi (ancak bunlarla sınırlı olmamak üzere) CoreMotion, PassKit ve StoreKit iOS; NFC ve Google Play hizmetlerini android'de; ve Windows döşeme. Ayrıca, yerel kullanıcı Arabirimi Araç Seti kullanarak diğer bölümleri oluşturulduğu sırada Xamarin.Forms ile oluşturulan kendi kullanıcı arabirimi bölümlerini sahip uygulamalar oluşturmak mümkündür.

Xamarin.Forms uygulamalar, geleneksel platformlar arası uygulamalar aynı şekilde tasarlanmış. En yaygın yaklaşımını kullanmaktır [taşınabilir kitaplıklara](~/cross-platform/app-fundamentals/pcl.md) veya [paylaşılan projeleri](~/cross-platform/app-fundamentals/shared-projects.md) paylaşılan kod barındırmak ve paylaşılan kod tüketir platform belirli uygulamaları oluşturmak için.

Kullanıcı arabirimleri Xamarin.Forms oluşturmak için iki tekniği vardır. İlk tekniği, tamamen C# kaynak kodu ile Uı'lar oluşturmaktır. Kullanılacak ikinci tekniktir *Genişletilebilir uygulama biçimlendirme dili* kullanıcı tanımlamak için kullanılan bir tanımlayıcı biçimlendirme dili (XAML) arabirimleri. XAML hakkında daha fazla bilgi için bkz: [XAML Temelleri](~/xamarin-forms/xaml/xaml-basics/index.md).

Bu makalede Xamarin.Forms framework temelleri açıklanır ve aşağıdaki konular ele alınmaktadır:

-  [Xamarin.Forms uygulaması inceleniyor](#Examining_A_Xamarin.Forms_Application).
-  [Xamarin.Forms sayfalar ve denetimler nasıl kullanıldığını](#Views_and_Layouts).
-  [Görüntü kullanma veri listesini](#Lists_in_Xamarin.Forms).
-  [Veri bağlama ayarlamak nasıl](#Data_Binding).
-  [Sayfaları arasında gezinmek nasıl](#Navigation).
-  [Sonraki adımlar](#Next_Steps).

<a name="Examining_A_Xamarin_Forms_Application" />

### <a name="examining-a-xamarinforms-application"></a>Xamarin.Forms uygulaması inceleniyor

Mac ve Visual Studio için Visual Studio'da kullanıcıya metin görüntüleyen basit Xamarin.Forms çözümünü olası, varsayılan Xamarin.Forms uygulaması şablonu oluşturur. Uygulama çalıştırıyorsanız, aşağıdaki ekran görüntüleri için benzer görünmelidir:

[![](introduction-to-xamarin-forms-images/image05-sml.png "Varsayılan Xamarin.Forms uygulaması")](introduction-to-xamarin-forms-images/image05.png#lightbox "varsayılan Xamarin.Forms uygulaması")

Ekran görüntüleri her ekranında karşılık gelen bir *sayfa* Xamarin.Forms içinde. A [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) temsil eden bir *etkinlik* Android içinde bir *View Controller* , iOS, veya bir *sayfa* Windows evrensel olarak Platformu (UWP). Yukarıdaki ekran görüntüleri örnek başlatır bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) nesne ve görüntülemek için kullanan bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/).

Başlangıç kodu kullanılmasını en üst düzeye çıkarmak için adlı tek bir sınıf Xamarin.Forms uygulamaların olması `App` ilk örneği için sorumlu [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) , görüntülenir. Bir örneği `App` sınıfı aşağıdaki kodda görülen:

```csharp
public class App : Application
{
  public App ()
  {
    MainPage = new ContentPage {
      Content =  new Label
      {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
      }
    };
  }
}
```

Bu kod yeni bir örneğini oluşturur [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) tek bir görüntüler nesne [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) sayfasında hem de dikey ve yatay olarak ortalanır.

<a name="Launching_the_Initial_Xamarin_Forms_Page_on_Each_Platform" />

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>Her Platform ilk Xamarin.Forms sayfasında başlatma

Bunu kullanmak için [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) bir uygulama içinde her platform uygulaması gerekir Xamarin.Forms çerçevesini başlatmak ve örneği sağlayan [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) , başlangıç olarak. Bu başlatma adım platformdan platforma değişir ve aşağıdaki bölümlerde ele alınmıştır.

<a name="Launching_in_iOS" />

#### <a name="ios"></a>iOS

İOS ilk Xamarin.Forms sayfasında başlatmak için platform proje içeriyor `AppDelegate` devraldığı sınıfı `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
      global::Xamarin.Forms.Forms.Init ();
      LoadApplication (new App ());
      return base.FinishedLaunching (app, options);
    }
}
```

`FinishedLoading` Geçersiz kılma çağırarak Xamarin.Forms framework başlatır `Init` yöntemi. Bu kök görünüm denetleyicisini çağrısıyla ayarlanmadan önce uygulamada yüklenecek Xamarin.Forms iOS özgü uyarlamasını neden `LoadApplication` yöntemi.

<a name="Launching_in_Android" />

#### <a name="android"></a>Android

Android ilk Xamarin.Forms sayfasında başlatmak için platform projesinde oluşturan kod içerir bir `Activity` ile `MainLauncher` özniteliğiyle etkinlik inherting `FormsApplicationActivity` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication (new App ());
        }
    }
}
```

`OnCreate` Geçersiz kılma çağırarak Xamarin.Forms framework başlatır `Init` yöntemi. Bu uygulamada Xamarin.Forms uygulaması yüklenmeden önce yüklenmesi Xamarin.Forms Android özgü uyarlamasını neden olur.

#### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Evrensel Windows Platformu (UWP) uygulamaları `Init` Xamarin.Forms framework başlatır yöntemi çağrılır `App` sınıfı:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Bu uygulamanın yüklenmesi Xamarin.Forms UWP özgü uyarlamasını neden olur. İlk Xamarin.Forms sayfası tarafından başlatılan `MainPage` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Xamarin.Forms uygulaması ile yüklenen `LoadApplication` yöntemi.

<a name="Views_and_Layouts" />

### <a name="views-and-layouts"></a>Görünümleri ve düzenleri

Kullanıcı arabirimi, bir Xamarin.Forms uygulaması oluşturmak için kullanılan dört ana denetim grubu yok.

1. **Sayfaları** – Xamarin.Forms sayfaları platformlar arası mobil uygulama ekranlar temsil eder. Sayfaları hakkında daha fazla bilgi için bkz: [Xamarin.Forms sayfaları](~/xamarin-forms/user-interface/controls/pages.md).
1. **Düzenleri** – Xamarin.Forms düzenleri olan mantıksal yapılara görünümleri oluşturmak için kullanılan kapsayıcı. Düzenleri hakkında daha fazla bilgi için bkz: [Xamarin.Forms düzenleri](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Görünümler** – Xamarin.Forms görünümlerdir etiketleri, düğmeler ve metin girişi kutuları gibi kullanıcı arabiriminde görüntülenen kontrol eder. Görünümler hakkında daha fazla bilgi için bkz: [Xamarin.Forms görünümleri](~/xamarin-forms/user-interface/controls/views.md).
1. **Hücreleri** – Xamarin.Forms hücreleri olan bir liste öğeleri için kullanılan özel öğeleri ve listedeki her öğeye nasıl çizileceğini açıklar. Hücreleri hakkında daha fazla bilgi için bkz: [Xamarin.Forms hücreleri](~/xamarin-forms/user-interface/controls/cells.md).

Çalışma zamanında her denetim ne işlenmiş olan kendi yerel eşdeğer eşleşecektir.

Denetimleri bir düzen içinde barındırılır. [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Yaygın olarak kullanılan bir düzen uygulayan sınıf şimdi inceledi.

<a name="StackLayout" />

#### <a name="stacklayout"></a>StackLayout

[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Otomatik olarak ekran boyutuna bakılmaksızın ekran denetimleri düzenleme tarafından platformlar arası uygulama geliştirme basitleştirir. Her alt öğenin konumlandırılmış art arda, ya da yatay olarak ya da dikey sırayla eklendikleri. Ne kadar alan `StackLayout` kullanma şekline bağlıdır [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ve [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) özellikler ayarlanır, ancak varsayılan olarak `StackLayout` tüm ekranı kullanmayı dener.

Aşağıdaki XAML kodu kullanarak bir örnek gösterilir bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) üç düzenlemek için [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetimleri:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample1" Padding="20">
  <StackLayout Spacing="10">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

```csharp
public class StackLayoutExample : ContentPage
{
    public StackLayoutExample()
    {
        Padding = new Thickness(20);
        var red = new Label
        {
            Text = "Stop", BackgroundColor = Color.Red, FontSize = 20
        };
        var yellow = new Label
        {
            Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20
        };
        var green = new Label
        {
            Text = "Go", BackgroundColor = Color.Green, FontSize = 20
        };

        Content = new StackLayout
        {
            Spacing = 10,
            Children = { red, yellow, green }
        };
    }
}
```

Varsayılan olarak [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) aşağıdaki ekran görüntülerinde gösterildiği gibi dikey yönlendirme varsayar:

[![](introduction-to-xamarin-forms-images/image09-sml.png "Dikey StackLayout")](introduction-to-xamarin-forms-images/image09.png#lightbox "dikey StackLayout")

Yönünü [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) yatay yönlendirme için aşağıdaki XAML kod örneğinde gösterildiği şekilde değiştirilebilir:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample2" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

```csharp
public class StackLayoutExample: ContentPage
{
    public StackLayoutExample()
    {
        // Code that creates labels removed for clarity
        Content = new StackLayout
        {
            Spacing = 10,
            VerticalOptions = LayoutOptions.End,
            Orientation = StackOrientation.Horizontal,
            HorizontalOptions = LayoutOptions.Start,
            Children = { red, yellow, green }
        };
    }
}
```

Aşağıdaki ekran görüntüleri, sonuçta elde edilen düzeni göster:

[![](introduction-to-xamarin-forms-images/image10-sml.png "Yatay StackLayout")](introduction-to-xamarin-forms-images/image10.png#lightbox "yatay StackLayout")

Denetimleri boyutunu aracılığıyla ayarlanabilir `HeightRequest` ve `WidthRequest` aşağıdaki XAML kod örneğinde gösterildiği gibi özellikleri:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample3" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" WidthRequest="100" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" WidthRequest="100" />
    <Label Text="Go" BackgroundColor="Green" Font="20" WidthRequest="200" />
  </StackLayout>
</ContentPage>
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

```csharp
var red = new Label
{
    Text = "Stop", BackgroundColor = Color.Red, FontSize = 20, WidthRequest = 100
};
var yellow = new Label
{
    Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20, WidthRequest = 100
};
var green = new Label
{
    Text = "Go", BackgroundColor = Color.Green, FontSize = 20, WidthRequest = 200
};

Content = new StackLayout
{
    Spacing = 10,
    VerticalOptions = LayoutOptions.End,
    Orientation = StackOrientation.Horizontal,
    HorizontalOptions = LayoutOptions.Start,
    Children = { red, yellow, green }
};
```

Aşağıdaki ekran görüntüleri, sonuçta elde edilen düzeni göster:

[![](introduction-to-xamarin-forms-images/image11-sml.png "Yatay StackLayout LayoutOptions ile")](introduction-to-xamarin-forms-images/image11.png#lightbox "LayoutOptions ile yatay StackLayout")

Hakkında daha fazla bilgi için [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) sınıfı için bkz: [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

<a name="Lists_in_Xamarin_Forms" />

## <a name="lists-in-xamarinforms"></a>Xamarin.Forms listeleri

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Denetimidir ekranında – her öğe öğeleri koleksiyonu görüntülemek için sorumlu `ListView` tek bir hücrede yer alır. Varsayılan olarak, bir `ListView` yerleşik kullanacağı [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) şablonu ve işleme tek satırlık metin.

Aşağıdaki kod örneği, basit bir gösterir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) örnek:

```csharp
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = new string []
{
    "Buy pears", "Buy oranges", "Buy mangos", "Buy apples", "Buy bananas"
};
Content = new StackLayout
{
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = { listView }
};
```

Aşağıdaki ekran görüntüsü, elde edilen gösterir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/):

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

Hakkında daha fazla bilgi için [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetlemek için bkz: [ListView](~/xamarin-forms/user-interface/listview/index.md).

<a name="Binding_to_a_Custom_Class" />

### <a name="binding-to-a-custom-class"></a>Özel bir sınıf bağlama

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Denetim Ayrıca varsayılan kullanarak özel nesneleri görüntüleme [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) şablonu.

Aşağıdaki örnekte gösterildiği kod `TodoItem` sınıfı:

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Denetimi, aşağıdaki kod örneğinde gösterildiği gibi doldurulabilir:

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

Hangi ayarlamak için bir bağlama oluşturulabilir `TodoItem` özelliği tarafından görüntülenen [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

Bu yolu belirten bir bağlama oluşturur `TodoItem.Name` özelliği, daha önce görüntülenen ekran görüntüsünde neden olacak.

Özel bir sınıf bağlama hakkında daha fazla bilgi için bkz: [ListView veri kaynakları](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

<a name="Selecting_an_Item_in_a_ListView" />

### <a name="selecting-an-item-in-a-listview"></a>ListView içinde bir öğe seçme

Bir hücreye temas kullanıcı yanıtlamak için bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) olay işlenecek, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

İçinde bağımsız bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) yöntemi, yeni bir sayfa ile yerleşik geri gezinti açmak için kullanılabilir. [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Olay hücreyle ilişkili nesne erişebilir [ `e.SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem/) özelliği, yeni bir sayfaya bağlayın ve yeni sayfa kullanarak görüntüleme `PushAsync`, olarak Aşağıdaki kod örneğinde gösterilmektedir:

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

Her platform yerleşik geri gezinti kendi şekilde uygular. Daha fazla bilgi için bkz: [Gezinti](#Navigation).

Hakkında daha fazla bilgi için [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) seçimi, bkz: [ListView etkileşim](~/xamarin-forms/user-interface/listview/interactivity.md).

<a name="Customizing_the_appearance_of_a_cell" />

### <a name="customizing-the-appearance-of-a-cell"></a>Bir hücrenin görünümünü özelleştirme

Hücre görünümü sınıflara göre özelleştirilebilir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) sınıfı ve ayar türü için bu sınıfın [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) özelliği [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).

Aşağıdaki ekran görüntüsünde gösterilen hücre birini oluşan [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) ve iki [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetimleri:

 ![](introduction-to-xamarin-forms-images/image14.png "ListView özel hücre görünümü")

Bu özel bir düzen oluşturmak için [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) sınıf Sınıflandırma, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
class EmployeeCell : ViewCell
{
    public EmployeeCell()
    {
        var image = new Image
        {
            HorizontalOptions = LayoutOptions.Start
        };
        image.SetBinding(Image.SourceProperty, new Binding("ImageUri"));
        image.WidthRequest = image.HeightRequest = 40;

        var nameLayout = CreateNameLayout();
        var viewLayout = new StackLayout()
        {
           Orientation = StackOrientation.Horizontal,
           Children = { image, nameLayout }
        };
        View = viewLayout;
    }

    static StackLayout CreateNameLayout()
    {
        var nameLabel = new Label
        {
            HorizontalOptions= LayoutOptions.FillAndExpand
        };
        nameLabel.SetBinding(Label.TextProperty, "DisplayName");

        var twitterLabel = new Label
        {
           HorizontalOptions = LayoutOptions.FillAndExpand,
           Font = Fonts.Twitter
        };
        twitterLabel.SetBinding(Label.TextProperty, "Twitter");

        var nameLayout = new StackLayout()
        {
           HorizontalOptions = LayoutOptions.StartAndExpand,
           Orientation = StackOrientation.Vertical,
           Children = { nameLabel, twitterLabel }
        };
        return nameLayout;
    }
}
```

Kod aşağıdaki görevleri gerçekleştirir:

-  Ekler bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) denetlemek ve kendisine bağlar `ImageUri` özelliği `Employee` nesnesi. Veri bağlama hakkında daha fazla bilgi için bkz: [veri bağlama](#Data_Binding).
-  Oluşturduğu bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) iki tutmak için bir dikey yönlendirme ile [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrol eder. `Label` Denetimleri bağlı `DisplayName` ve `Twitter` özelliklerini `Employee` nesnesi.
-  Oluşturduğu bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) varolan barındıracak [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) ve `StackLayout`. Yatay yönlendirme kullanarak alt düzenlenir.

Özel hücre oluşturulduktan sonra onu tarafından kullanılabilecek bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) içinde kaydırma tarafından denetim bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

Bu kodu sağlayacak bir `List` , `Employee` için [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Her bir hücre kullanarak işlenir `EmployeeCell` sınıfı. `ListView` Geçer `Employee` nesnesini `EmployeeCell` olarak kendi [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Hücre görünümünü özelleştirme hakkında daha fazla bilgi için bkz: [hücre Görünüm](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="Using_XAML_to_Create_and_Customize_A_List" />

### <a name="using-xaml-to-create-and-customize-a-list"></a>XAML kullanarak oluşturma ve bir liste özelleştirme

XAML denk [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) aşağıdaki kod örneğinde önceki bölümde gösterilmiştir:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:XamarinFormsXamlSample;assembly=XamarinFormsXamlSample"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="listView" IsVisible="false" ItemsSource="{x:Static local:App.Employees}" ItemSelected="EmployeeListOnItemSelected">
    <ListView.ItemTemplate>
      <DataTemplate>
        <ViewCell>
          <ViewCell.View>
            <StackLayout Orientation="Horizontal">
              <Image Source="{Binding ImageUri}" WidthRequest="40" HeightRequest="40" />
              <StackLayout Orientation="Vertical" HorizontalOptions="StartAndExpand">
                <Label Text="{Binding DisplayName}" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Twitter}" Font="{x:Static constants:Fonts.Twitter}"/>
              </StackLayout>
            </StackLayout>
          </ViewCell.View>
        </ViewCell>
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Bu XAML tanımlayan bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) içeren bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Veri kaynağı `ListView` aracılığıyla ayarlanır [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) özniteliği. Her satırda düzenini `ItemsSource` içinde tanımlanan [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) öğesi.

<a name="Data_Binding" />

## <a name="data-binding"></a>Veri Bağlama

Veri bağlama bağlayan adlı iki nesne *kaynak* ve *hedef*. *Kaynak* nesnesi, verileri sağlar. *Hedef* nesne tüketebilir (ve çoğunlukla görüntüler) veri kaynağı nesnesi. Örneğin, bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) (*hedef* nesnesi) genellikle bağlayacaksınız kendi [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) ortak bir özelliğine `string` bir özelliğinde*kaynak* nesnesi. Aşağıdaki diyagramda bağlama ilişki gösterilmektedir:

![](introduction-to-xamarin-forms-images/data-binding.png "Veri bağlama")

Veri bağlama ana avantajı, artık veri görünümleri ve veri kaynağı arasındaki eşitleme hakkında endişelenmeye gerek olmasıdır. Değişiklikleri *kaynak* nesne için kalan otomatik olarak *hedef* bağlama çerçevesi tarafından Perde Arkası nesne ve hedef nesne değişiklikleri isteğe bağlı olarak gönderilen geri *kaynak* nesnesi.

Veri oluşturma bağlama iki aşamalı bir işlemdir:

- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Özelliği *hedef* nesne ayarlanmalıdır *kaynak*.
- Bir bağlama arasında kurulmalıdır *hedef* ve *kaynak*. XAML kullanarak bu elde edilen [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) biçimlendirme uzantısı. C# ' ta bu tarafından sağlanır [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) yöntemi.

Veri bağlama hakkında daha fazla bilgi için bkz: [veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

### <a name="xaml"></a>XAML

Aşağıdaki kod veri bağlama XAML'de gerçekleştirmeye dair bir örnek gösterilmektedir:

```xaml
<Entry Text="{Binding FirstName}" ... />
```

Arasında bir bağ [ `Entry.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) özelliği ve `FirstName` özelliği *kaynak* nesne kurulur. Yapılan değişiklikler `Entry` denetimi otomatik olarak yayılır için `employeeToDisplay` nesnesi. Benzer şekilde, değişiklikler için yapılırsa `employeeToDisplay.FirstName` özelliği Xamarin.Forms bağlama altyapısı da güncelleştirilecek içeriğini `Entry` denetim. Bu olarak bilinen bir *iki yönlü bağlama*. Model sınıfı çalışmak için iki yönlü bağlama için sırayla uygulamalıdır `INotifyPropertyChanged` arabirimi.

Ancak [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) özelliği `EmployeeDetailPage` sınıfı XAML'de ayarlanabilir, burada arka plan kodu örneği olarak ayarlanır bir `Employee` nesnesi:

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

Sırada [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) her özellik *hedef* nesne ayrı ayrı ayarlanabilir, bu gerekli değildir. `BindingContext` tüm alt gruplar tarafından devralınır özel bir özelliktir. Bu nedenle, `BindingContext` üzerinde [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) ayarlanmış bir `Employee` örneği, tüm alt öğelerinin `ContentPage` aynı olan `BindingContext`ve ortaközelliklerinebağlayabilirsiniz`Employee`nesnesi.

### <a name="c35"></a>C&#35;

Aşağıdaki kod örneği, veri bağlama C# ' ta gerçekleştirme gösterir:

```csharp
public EmployeeDetailPage(Employee employeeToDisplay)
{
    this.BindingContext = employeeToDisplay;
    var firstName = new Entry()
    {
        HorizontalOptions = LayoutOptions.FillAndExpand
    };
    firstName.SetBinding(Entry.TextProperty, "FirstName");
    ...
}
```

[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Oluşturucusu örneği geçirilen bir `Employee` nesne ve ayarlar [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) bağlamak için nesnesine. Bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetim örneği ve bağlama arasında [ `Entry.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) özelliği ve `FirstName` özelliği *kaynak* nesnesi ayarlanır. Yapılan değişiklikler `Entry` denetimi otomatik olarak yayılır için `employeeToDisplay` nesnesi. Benzer şekilde, değişiklikler için yapılırsa `employeeToDisplay.FirstName` özelliği Xamarin.Forms bağlama altyapısı da güncelleştirilecek içeriğini `Entry` denetim. Bu olarak bilinen bir *iki yönlü bağlama*. Model sınıfı çalışmak için iki yönlü bağlama için sırayla uygulamalıdır `INotifyPropertyChanged` arabirimi.

`SetBinding` Yöntemi iki parametre alır. İlk parametre bağlama türü hakkındaki bilgileri belirtir. İkinci parametre ne bağlamak veya nasıl bağlanacağını hakkında bilgi sağlamak için kullanılır. İkinci parametresi, çoğu durumda, özelliğin adını bulunduran yalnızca bir dizedir [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Aşağıdaki söz dizimini bağlamak için kullanılan `BindingContext` doğrudan:

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

Nokta sözdizimini kullanacak şekilde Xamarin.Forms söyler [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) bir özellik yerine veri kaynağı olarak `BindingContext`. Bu durumlarda kullanışlıdır `BindingContext` basit bir tür olduğu gibi bir `string` veya bir `int`.

<a name="INotifyPropertyChanged" />

### <a name="property-change-notification"></a>Özellik değişikliği bildirimi

Varsayılan olarak, *hedef* nesne yalnızca değerini alır *kaynak* bağlama oluşturulduğunda nesne. Veri kaynağı ile eşitlenmiş UI tutmak için bildirmek için bir yol olmalıdır *hedef* nesne zaman *kaynak* nesnesi değişti. Bu düzenek tarafından sağlanan `INotifyPropertyChanged` arabirimi. Temel alınan özellik değeri değiştiğinde bu arabirimini uygulayan tüm verilere bağlı denetimler için bildirimleri sağlar.

Nesneleri uygulayan `INotifyPropertyChanged` artırmalısınız `PropertyChanged` özelliklerini birini yeni bir değerle aşağıdaki kod örneğinde gösterildiği şekilde güncelleştirildiğinde olay:

```csharp
public class MyObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    string _firstName;
    public string FirstName
    {
        get { return _firstName; }
        set
        {
            if (value.Equals(_firstName, StringComparison.Ordinal))
            {
                // Nothing to do - the value hasn't changed;
                return;
            }
            _firstName = value;
            OnPropertyChanged();

        }
    }

    void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        var handler = PropertyChanged;
        if (handler != null)
        {
            handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Zaman `MyObject.FirstName` özellik değişikliklerini `OnPropertyChanged` yöntemi çağrıldığında, hangi oluşturacağı `PropertyChanged` olay. Tetikleme, gereksiz olayları önlemek için `PropertyChanged` değil olayı özellik değeri değiştirirseniz değil.

İçinde unutmayın `OnPropertyChanged` yöntemi `propertyName` parametresi donatılan ile `CallerMemberName` özniteliği. Bu olması durumunda sağlar `OnPropertyChanged` ile yöntemi çağrıldığında bir `null` değeri `CallerMemberName` özniteliğini çağrılan yöntemin adını sağlayın `OnPropertyChanged`.

<a name="Navigation" />

## <a name="navigation"></a>Gezinme

Xamarin.Forms sağlayan bir dizi farklı sayfa gezinti deneyimleri bağlı olarak, [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) kullanılan yazın. İçin [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örnekleri var olan iki Gezinti deneyimleri:

- [Hiyerarşik Gezinme](#Hierarchical_Navigation)
- [Kalıcı gezinme](#Modal_Navigation)

[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Ve [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) sınıfları alternatif gezinti deneyimleri sağlar. Daha fazla bilgi için bkz: [Gezinti](~/xamarin-forms/app-fundamentals/navigation/index.md).

<a name="Hierarchical_Navigation" />

### <a name="hierarchical-navigation"></a>Hiyerarşik gezinme

[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Sınıfı kullanıcının bulunduğu iletir ve geriye doğru istediğiniz gibi sayfalar arasında gezinmek için bir hiyerarşik gezinme deneyimi sağlar. Sınıf uygulayan Gezinti son giren ilk çıkar (LIFO) yığınını olarak [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) nesneleri.

Hiyerarşik gezintideki [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) sınıfı bir yığınından gitmek için kullanılan [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) nesneleri. Bir sayfadan diğerine taşımak için bir uygulama burada etkin sayfa olacak yeni bir sayfa gezinti yığına gönderir. Önceki sayfaya dönmek için uygulamanın geçerli sayfa gezinti yığından pop ve yeni en üstteki sayfa etkin sayfa haline gelir.

Bir gezinti yığını eklenen ilk sayfası olarak adlandırılır *kök* sayfasında uygulama ve aşağıdaki kod örneğinde gösterir bu nasıl gerçekleştirilir:

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

Gitmek için `LoginPage`, çağırmak gerekli olan [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) yöntemi [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) aşağıdaki kod örneğinde kanıtlanabilir olarak geçerli sayfanın özelliği:

```csharp
await Navigation.PushAsync(new LoginPage());
```

Bu yeni neden `LoginPage` Gezinti yığında burada etkin sayfa haline gelir edilmesini nesnesi.

Etkin sayfa gezinti yığınından tuşlarına basarak Sil'i *geri* düğmesini cihazda bağımsız olarak, bu aygıttaki fiziksel bir düğme olup olmadığını veya bir ekran düğmesini. Program aracılığıyla önceki sayfaya dönmek için `LoginPage` örneği gerekir çağırma [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
await Navigation.PopAsync();
```

Hiyerarşik Gezinti hakkında daha fazla bilgi için bkz: [hiyerarşik Gezinti](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Modal_Navigation" />

### <a name="modal-navigation"></a>Kalıcı gezinme

Xamarin.Forms kalıcı sayfalar için destek sağlar. Kalıcı bir sayfa, görev tamamlandı veya iptal kadar uzağa erişilemeyeceğini kendi içinde bulunan bir görevi tamamlamak için kullanıcıların önerir.

Kalıcı bir sayfa herhangi biri olabilir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Xamarin.Forms tarafından desteklenen türleri. Kalıcı bir sayfayı uygulamayı görüntülemek için Gezinti yığına burada etkin sayfa olacak gönderir. Önceki sayfaya dönmek için uygulamanın geçerli sayfa gezinti yığından pop ve yeni en üstteki sayfa etkin sayfa haline gelir.

Kalıcı Gezinti yöntemleri tarafından açığa [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) herhangi bir özellikte [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) türetilmiş tür. [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Özelliği de kullanıma sunan bir [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) özelliği kendisinden Gezinti yığını kalıcı sayfalarında elde edilebilir. Ancak, kalıcı yığını işleme gerçekleştirmek ya da kalıcı Gezinti kök sayfası pencerelerinin hiçbir kavramı yoktur. Bu işlemler temel platformlarda Evrensel desteklenmeyen olmasıdır.

> [!NOTE]
> A [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) örneği kalıcı sayfa gezintisi gerçekleştirmek için gerekli değildir.

Kalıcı olarak gitmek için `LoginPage` çağırmak gerekli olan [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) yöntemi [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) aşağıdaki kod örneğinde kanıtlanabilir olarak geçerli sayfanın özelliği :

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

Bu neden `LoginPage` Gezinti yığına burada etkin sayfa haline gelir edilmesini örneği.

Etkin sayfa gezinti yığınından tuşlarına basarak Sil'i *geri* düğmesini cihazda bağımsız olarak, bu aygıttaki fiziksel bir düğme olup olmadığını veya bir ekran düğmesini. Program aracılığıyla özgün sayfaya geri dönmek için `LoginPage` örneği gerekir çağırma [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
await Navigation.PopModalAsync();
```

Bu neden `LoginPage` etkin sayfa haline gelen yeni en üstteki sayfa gezinti yığınından kaldırılacak örneği.

Kalıcı gezinme hakkında daha fazla bilgi için bkz: [kalıcı sayfaları](~/xamarin-forms/app-fundamentals/navigation/modal.md).

<a name="Next_Steps" />

## <a name="next-steps"></a>Sonraki Adımlar

Bu giriş makalesi, Xamarin.Forms uygulamaları yazmaya başlamak etkinleştirmeniz gerekir. Önerilen sonraki adımlar aşağıdaki işlevleri hakkında bilgi içerir:

- Denetim şablonları, çalışma zamanında uygulama sayfaları kolayca tema ve yeniden tema yeteneği sağlar. Daha fazla bilgi için bkz: [denetim şablonları](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).
- Veri şablonları, desteklenen denetimlere veri sunumu tanımlama yeteneği sağlar. Daha fazla bilgi için bkz: [veri şablonları](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Paylaşılan kod aracılığıyla yerel işlevler erişebilir [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) sınıfı. Daha fazla bilgi için bkz: [DependencyService yerel özelliklerle erişme](~/xamarin-forms/app-fundamentals/dependency-service/index.md).
- Xamarin.Forms, bu nedenle sınıflar arasında bağ azaltma ileti gönderme ve alma için basit bir ileti sistemi hizmeti içerir. Daha fazla bilgi için bkz: [Yayımla ve abone ol MessagingCenter ile](~/xamarin-forms/app-fundamentals/messaging-center.md).
- Her sayfaya, Düzen ve denetim farklı kullanarak her platformunda işlenen bir `Renderer` sırayla yerel bir denetimi oluşturur sınıfı ekranda düzenler ve paylaşılan kodda belirtilen davranışı ekler. Geliştiriciler kendi özel uygulayabilirsiniz `Renderer` görünümünü ve/veya Denetim davranışını özelleştirmek için sınıflar. Daha fazla bilgi için bkz: [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).
- Efektler özelleştirilmek üzere her platformda yerel denetimleri de olanak sağlar. Etkileri platforma özgü projelerinde sınıflara tarafından oluşturulan [ `PlatformEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/) denetlemek ve uygun bir Xamarin.Forms Denetim ekleyerek tüketilen. Daha fazla bilgi için bkz: [efektler](~/xamarin-forms/app-fundamentals/effects/index.md).

Alternatif olarak, mobil uygulamalarla oluşturma Xamarin.Forms, kitap Charles Petzold'un Xamarin.Forms hakkında daha fazla bilgi edinmek için uygun bir yerdir. Daha fazla bilgi için bkz: [Xamarin.Forms ile Mobile Apps oluşturma](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="summary"></a>Özet

Bu makalede Xamarin.Forms ve bu uygulamalarla yazmaya başlamak nasıl bir giriş sağlanır. Xamarin.Forms bir platformlar arası geliştiricilerin kolayca Android, iOS ve evrensel Windows platformu arasında paylaşılan kullanıcı arabirimleri oluşturmanıza olanak tanır UI Araç Seti soyutlama yerel olarak yedeklenen ' dir. Kullanıcı arabirimleri, her platform için uygun görünüm ve kullanımında korumak Xamarin.Forms uygulamaların izin verecek şekilde hedef platformu yerel denetimleri kullanarak işlenir.


## <a name="related-links"></a>İlgili bağlantılar

- [HTML Temelleri](~/xamarin-forms/xaml/xaml-basics/index.md)
- [Denetimler Başvurusu](~/xamarin-forms/user-interface/controls/index.md)
- [Kullanıcı Arabirimi](~/xamarin-forms/user-interface/index.md)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Başlarken örneği](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
- [Ücretsiz Self-Guided öğrenme (video)](https://university.xamarin.com/self-guided)
- [Merhaba, Xamarin.Forms iOS çalışma kitabı](https://developer.xamarin.com/workbooks/xamarin-forms/getting-started/GettingStartedWithXamarinForms-ios.workbook)
