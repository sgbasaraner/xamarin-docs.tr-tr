---
title: Xamarin.Forms'a giriş
description: Bu makalede Xamarin.Forms ve onunla uygulamaları yazmaya başlamak nasıl bir giriş sağlar.
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2018
ms.openlocfilehash: c5d2f93c8cb97c50f9d35d9ad91adf4c6437a3db
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38999042"
---
# <a name="an-introduction-to-xamarinforms"></a>Xamarin.Forms'a giriş

_Xamarin.Forms, Android, iOS ve Windows için platformlar arası uygulamalar oluşturmalarını sağlayan bir çerçevedir. Kod ve kullanıcı arabirimi tanımları platformlar arasında paylaşılabilir, ancak yerel denetimleriyle oluşturulur. Bu makalede Xamarin.Forms ve Visual Studio'da C# ve XAML uygulamaları yazmaya başlamak nasıl bir giriş sağlar._

Xamarin.Forms uygulamaları [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) projelerin paylaşılan kod ve paylaşılan kod kullanma ve her platform için gerekli çıktı oluşturmak için ayrı uygulama projeleri içerir. Yeni Xamarin.Forms uygulaması oluşturduğunuzda, çözüm (C# ve XAML dosyalarını içeren) paylaşılan kod projesine yanı sıra platforma özgü projeleri bu ekran görüntüsünde gösterildiği gibi içerir:

![Visual Studio'da Xamarin.Forms şablonu çözümü](introduction-to-xamarin-forms-images/solution-both.png)

Xamarin.Forms uygulamaları yazarken, kod ve kullanıcı arabirimi, Android, iOS ve UWP projeleri tarafından başvurulan üst, .NET Standard projesine eklenir. Derleme ve Android, iOS ve UWP projeleri test etme ve uygulamanızı dağıtmak için çalıştırın.

## <a name="examining-a-xamarinforms-application"></a>Xamarin.Forms uygulaması İnceleme

Visual Studio'da varsayılan Xamarin.Forms uygulaması şablonu, tek bir metin etiketi görüntüler. Uygulamayı çalıştırdığınızda, aşağıdaki ekran görüntüleri için benzer görünmelidir:

[![](introduction-to-xamarin-forms-images/image05-sml.png "Varsayılan Xamarin.Forms uygulaması")](introduction-to-xamarin-forms-images/image05.png#lightbox)

Ekran görüntüleri her ekranda karşılık gelen bir *sayfa* Xamarin.Forms içinde. A [ `Page` ](xref:Xamarin.Forms.Page) temsil eder bir *etkinlik* Android, bir *görünüm denetleyicisi* ios'ta, veya bir *sayfası* içinde Windows Evrensel Platformu (UWP). Yukarıdaki ekran görüntüleri örnek örnekleyen bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) nesne ve görüntülemek için kullanan bir [ `Label` ](xref:Xamarin.Forms.Label).

Yeniden başlatma kodu kullanılmasını en üst düzeye çıkarmak için Xamarin.Forms uygulamaları adlı tek bir sınıf sahip `App` ilk örnekleme için sorumlu [ `Page` ](xref:Xamarin.Forms.Page) , görüntülenir. Örneği `App` sınıfı aşağıdaki kodda görülebilir (içinde **App.xaml.cs**):

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent();
    MainPage = new MainPage(); // sets the App.MainPage property to an instance of the MainPage class
  }
}
```

Bu kod yeni bir örneğini oluşturur [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) çağrılan nesne `MainPage` tek bir görüntülenir [ `Label` ](xref:Xamarin.Forms.Label) sayfada her ikisi de dikey ve yatay orta. XAML içinde **MainPage.xaml** dosya şu şekilde görünür:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:AwesomeApp" x:Class="AwesomeApp.MainPage">
    <StackLayout>
        <Label Text="Hello Xamarin.Forms"
           HorizontalOptions="Center"
           VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>Her platformda ilk Xamarin.Forms sayfasında başlatılıyor

> [!TIP]
> Xamarin.Forms nasıl çalıştığını anlamanıza yardımcı olması için bu bölümdeki platforma özgü bilgiler sağlanmaktadır.
> Proje şablonları, zaten bu sınıflar içerir. kendinize kod gerekmez.
>
> Atlayabilirsiniz [kullanıcı arabirimi](#user-interface) bölümünde ve daha sonra bu bölümü okuyun.

Bir sayfayı kullanmak için (gibi **MainPage** Yukarıdaki örnekteki) içindeki bir uygulama, her platform uygulaması gerekir Xamarin.Forms framework başlatılamıyor ve, başlangıç sayfasının bir örneğini sağlar. Bu başlatma adımına platformdan platforma değişir ve aşağıdaki bölümlerde ele alınmıştır.

#### <a name="ios"></a>iOS

İos'ta ilk Xamarin.Forms sayfasını başlatmak için platform projesi içeren `AppDelegate` öğesinden devralınan sınıf `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

`FinishedLaunching` Geçersiz kılma çağırarak Xamarin.Forms framework başlatır `Init` yöntemi. Bu kök görünüm denetleyicisi çağrısıyla ayarlanmadan önce uygulamanın yüklenmesi için Xamarin.Forms iOS özel uygulanışı neden `LoadApplication` yöntemi.

#### <a name="android"></a>Android

Android ilk Xamarin.Forms sayfasını başlatmak için platform projesi oluşturan kodu içeren bir `Activity` ile `MainLauncher` özniteliğiyle etkinlik inherting `FormsAppCompatActivity` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", Theme = "@style/MainTheme", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : FormsAppCompatActivity
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

`OnCreate` Geçersiz kılma çağırarak Xamarin.Forms framework başlatır `Init` yöntemi. Bu uygulamada Xamarin.Forms uygulama yüklenmeden önce yüklenmesi için Xamarin.Forms Android özel uygulanışı neden olur.

#### <a name="universal-windows-platform-uwp"></a>Evrensel Windows Platformu (UWP)

Evrensel Windows Platformu (UWP) uygulamaları `Init` Xamarin.Forms framework başlatan yöntem ınvoked from `App` sınıfı:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Bu, uygulamanın yüklenmesi için Xamarin.Forms UWP özel uygulanışı neden olur. İlk Xamarin.Forms sayfası tarafından başlatılan `MainPage` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

Xamarin.Forms uygulaması ile yüklenen `LoadApplication` yöntemi. Yeni bir Xamarin.Forms projesi oluşturduğunuzda, visual Studio yukarıdaki tüm kodu ekler.

## <a name="user-interface"></a>Kullanıcı arabirimi

Xamarin.Forms içinde kullanıcı arabirimleri oluşturmak için iki teknik vardır:

- Kullanıcı arabirimi, tamamen C# kaynak kodu oluşturun.
- *Extensible Application Markup Language* kullanıcı açıklamak için kullanılan bir bildirim temelli bir biçimlendirme dili (XAML) arabirimleri.

Kullandığınız yöntemden bağımsız olarak aynı sonuçları elde edilebilecek (ve her ikisi de aşağıda açıklanmıştır). Xamarin.Forms XAML hakkında daha fazla bilgi için bkz: [XAML Temelleri](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="views-and-layouts"></a>Görünümleri ve düzenleri

Xamarin.Forms uygulaması kullanıcı arabirimi oluşturmak için kullanılan dört ana denetim Grup vardır.

- **Sayfaları** – Xamarin.Forms sayfaları, platformlar arası mobil uygulama ekranları temsil eder. Sayfaları hakkında daha fazla bilgi için bkz. [Xamarin.Forms sayfaları](~/xamarin-forms/user-interface/controls/pages.md).
- **Düzenleri** – Xamarin.Forms düzenleri, mantıksal yapılarda görünümleri oluşturmak için kullanılan kapsayıcılardır. Düzenleri hakkında daha fazla bilgi için bkz: [Xamarin.Forms Layouts](~/xamarin-forms/user-interface/controls/layouts.md).
- **Görünümleri** – Xamarin.Forms görünümleri etiketleri, düğme ve metin girişi kutusu gibi kullanıcı arabiriminde görüntülenen denetimleri vardır. Görünümler hakkında daha fazla bilgi için bkz. [Xamarin.Forms görünümleri](~/xamarin-forms/user-interface/controls/views.md).
- **Hücreleri** – Xamarin.Forms hücre bir listedeki öğeler için kullanılan özel öğe ve listedeki her öğeye nasıl çizileceğini açıklanmaktadır. Hücreleri hakkında daha fazla bilgi için bkz: [Xamarin.Forms hücreleri](~/xamarin-forms/user-interface/controls/cells.md).

Çalışma zamanında, her denetimi ekranında işlenmeden yerel onun dengi eşleştirilir.

Denetimleri bir düzen içinde barındırılır. [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Sınıfı &ndash; yaygın olarak kullanılan bir düzen &ndash; aşağıda açıklanmıştır.

### <a name="stacklayout"></a>StackLayout

[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Otomatik olarak ekran boyutu ne olursa olsun ekrandaki denetimleri düzenleyerek platformlar arası uygulama geliştirmeyi basitleştirir. Her alt öğenin konumlandırılmış bir birbiri ardına, ya da yatay ya da dikey olarak sırayla eklendikleri. Ne kadar alan `StackLayout` kullanım şekline bağlıdır [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) ve [ `VerticalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) özellikleri ayarlanır, ancak varsayılan olarak `StackLayout` ekranın tamamını kullanmayı dener.

Aşağıdaki XAML kodu kullanarak bir örnek gösterilmektedir bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) üç düzenlemek için [ `Label` ](xref:Xamarin.Forms.Label) denetimler:

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

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

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

Varsayılan olarak [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) aşağıdaki ekran görüntülerinde gösterildiği dikey varsayar:

[![](introduction-to-xamarin-forms-images/image09-sml.png "Dikey StackLayout")](introduction-to-xamarin-forms-images/image09.png#lightbox "dikey StackLayout")

Yönünü [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) aşağıdaki XAML kod örneğinde gösterildiği gibi bir yatay yönlendirme için değiştirilebilir:

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

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

```csharp
public class StackLayoutExample: ContentPage
{
    public StackLayoutExample()
    {
        // Code that creates red, yellow, green labels removed for clarity (see above)
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

Aşağıdaki ekran görüntüleri, sonuçta elde edilen düzenini göster:

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

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

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

Aşağıdaki ekran görüntüleri, sonuçta elde edilen düzenini göster:

[![](introduction-to-xamarin-forms-images/image11-sml.png "Yatay StackLayout LayoutOptions ile")](introduction-to-xamarin-forms-images/image11.png#lightbox)

Hakkında daha fazla bilgi için [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) sınıfı [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

## <a name="lists-in-xamarinforms"></a>Xamarin.Forms içinde listeler

[ `ListView` ](xref:Xamarin.Forms.ListView) Denetim ekranda – her öğe, öğe koleksiyonu görüntülemek için sorumlu `ListView` tek bir hücrede yer alır. Varsayılan olarak, bir `ListView` yerleşik kullanacağı [ `TextCell` ](xref:Xamarin.Forms.TextCell) şablonu ve işleme tek satırlık bir metin.

Aşağıdaki kod örneği basit gösterir [ `ListView` ](xref:Xamarin.Forms.ListView) örneği:

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

Sonuç aşağıdaki ekran görüntüsünde gösterilmektedir [ `ListView` ](xref:Xamarin.Forms.ListView):

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

Hakkında daha fazla bilgi için [ `ListView` ](xref:Xamarin.Forms.ListView) denetlemek için bkz: [ListView](~/xamarin-forms/user-interface/listview/index.md).

### <a name="binding-to-a-custom-class"></a>Özel bir sınıf bağlama

[ `ListView` ](xref:Xamarin.Forms.ListView) Denetimi varsayılan kullanarak özel nesneler de görüntüleyebilir [ `TextCell` ](xref:Xamarin.Forms.TextCell) şablonu.

Aşağıdaki örnekte gösterildiği kod `TodoItem` sınıfı:

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

[ `ListView` ](xref:Xamarin.Forms.ListView) Denetimi, aşağıdaki kod örneğinde gösterildiği şekilde doldurulabilir:

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

Bağlama, ayarlanacak oluşturulabilir `TodoItem` özelliği tarafından görüntülenen [ `ListView` ](xref:Xamarin.Forms.ListView), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

Bu yolu belirten bir bağlama oluşturur `TodoItem.Name` özelliği ve daha önce görüntülenen ekran görüntüsünde neden olur.

Özel bir sınıf bağlama hakkında daha fazla bilgi için bkz. [ListView veri kaynakları](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

### <a name="selecting-an-item-in-a-listview"></a>Bir ListView içinde bir öğe seçme

Bir kullanıcı bir hücreye temas yanıt vermek için bir [ `ListView` ](xref:Xamarin.Forms.ListView), [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) olay işlenecek, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

İçinde bağımsız bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) yöntemi, yeni bir sayfa ile yerleşik geri gezinme açmak için kullanılabilir. [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Olay bir hücreyle ilişkili nesne erişim [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) özelliği, yeni bir sayfaya bağlama ve yeni sayfa kullanarak görüntüler `PushAsync`, olarak Aşağıdaki kod örneğinde gösterilmiştir:

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

Her platform yerleşik geri gezinme, kendi şekilde uygular. Daha fazla bilgi için [Gezinti](#Navigation).

Hakkında daha fazla bilgi için [ `ListView` ](xref:Xamarin.Forms.ListView) seçimi bkz [ListView etkileşim](~/xamarin-forms/user-interface/listview/interactivity.md).

### <a name="customizing-the-appearance-of-a-cell"></a>Bir hücreyi görünümünü özelleştirme

Hücre görünümü sınıflara göre özelleştirilebilir [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) sınıfı ve ayar türü için bu sınıfın [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) özelliği [ `ListView` ](xref:Xamarin.Forms.ListView).

Aşağıdaki ekran görüntüsünde gösterilen hücre birini oluşur [ `Image` ](xref:Xamarin.Forms.Image) ve iki [ `Label` ](xref:Xamarin.Forms.Label) denetimler:

 ![](introduction-to-xamarin-forms-images/image14.png "ListView özel hücre görünümü")

Bu özel bir düzen oluşturmak için [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) sınıfı Sınıflandırma, aşağıdaki kod örneğinde gösterildiği gibi:

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

-  Ekler bir [ `Image` ](xref:Xamarin.Forms.Image) denetlemek ve bağlar `ImageUri` özelliği `Employee` nesne. Veri bağlama hakkında daha fazla bilgi için bkz. [veri bağlama](#Data_Binding).
-  Oluşturur bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) iki tutacak dikey yönlendirme kullanıldığında [ `Label` ](xref:Xamarin.Forms.Label) kontrol eder. `Label` Denetimleri için ilişkili `DisplayName` ve `Twitter` özelliklerini `Employee` nesne.
-  Oluşturur bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) varolan barındıracak [ `Image` ](xref:Xamarin.Forms.Image) ve `StackLayout`. Bu, alt öğelerini yatay yönlendirme kullanarak kapatılmasını düzenler.

Özel hücre oluşturulduktan sonra tarafından kullanılabilen bir [ `ListView` ](xref:Xamarin.Forms.ListView) içinde kaydırma denetim bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

Bu kodu sağlayacak bir `List` , `Employee` için [ `ListView` ](xref:Xamarin.Forms.ListView). Her bir hücresinde kullanarak işlenir `EmployeeCell` sınıfı. `ListView` Geçer `Employee` nesnesini `EmployeeCell` olarak kendi [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext).

Hücre görünümü özelleştirme hakkında daha fazla bilgi için bkz. [hücre görünümü](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

### <a name="using-xaml-to-create-and-customize-a-list"></a>XAML kullanarak oluşturma ve bir listesini özelleştirme

XAML denk [ `ListView` ](xref:Xamarin.Forms.ListView) önceki bölümde aşağıdaki kod örneğinde gösterilmiştir:

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

Bu XAML tanımlayan bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) içeren bir [ `ListView` ](xref:Xamarin.Forms.ListView). Veri kaynağını `ListView` aracılığıyla ayarlanan [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) özniteliği. Her bir satırın düzenini `ItemsSource` içinde tanımlanan [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) öğesi.

## <a name="data-binding"></a>Veri bağlama

Veri bağlama bağlayan adlı iki nesne *kaynak* ve *hedef*. *Kaynak* veri nesnesi sağlar. *Hedef* nesne kullanma (ve çoğunlukla görüntüler) kaynak nesne verilerden. Örneğin, bir [ `Label` ](xref:Xamarin.Forms.Label) (*hedef* nesnesi) yaygın olarak bağlanır, [ `Text` ](xref:Xamarin.Forms.Label.Text) genel bir özelliğini `string` bir özelliğinde*kaynak* nesne. Aşağıdaki diyagramda, bağlama ilişkiyi göstermektedir:

![](introduction-to-xamarin-forms-images/data-binding.png "Veri bağlama")

Veri bağlama ana yararı, artık görünümlerinizi ve veri kaynağı arasında veri eşitleme hakkında endişelenmeye gerek olmasıdır. Değişiklikleri *kaynak* nesne için otomatik olarak itilir *hedef* bağlama çerçevesi tarafından sunulan Sahne Arkası nesne ve hedef nesnede değişiklik isteğe bağlı olarak gönderdiniz geri için *kaynak* nesne.

Veri oluşturma bağlama bir iki adımlı bir işlemdir:

- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Özelliği *hedef* nesne ayarlanmalıdır *kaynak*.
- Bir bağlama arasında kurulması gerekir *hedef* ve *kaynak*. XAML içinde bu yararlanılır [ `Binding` ](xref:Xamarin.Forms.Xaml.BindingExtension) işaretleme uzantısı. C# ' ta bu tarafından sağlanır [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) yöntemi.

Veri bağlama hakkında daha fazla bilgi için bkz. [temel veri bağlama bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

### <a name="xaml"></a>XAML

Aşağıdaki kod, XAML içinde veri bağlamayı gerçekleştiren bir örnek gösterilmektedir:

```xaml
<Entry Text="{Binding FirstName}" ... />
```

Arasında bir bağ [ `Entry.Text` ](xref:Xamarin.Forms.Entry.Text) özelliği ve `FirstName` özelliği *kaynak* nesne kurulur. Yapılan değişiklikler `Entry` denetimi otomatik olarak yayılır için `employeeToDisplay` nesne. Benzer şekilde, değişiklikler yapılırsa `employeeToDisplay.FirstName` özelliği Xamarin.Forms bağlama altyapısı ayrıca güncelleştirecek içeriği `Entry` denetimi. Bu olarak bilinen bir *iki yönlü bağlama*. Model sınıfı çalışmak için iki yönlü bağlamaya sırayla uygulamalıdır `INotifyPropertyChanged` arabirimi.

Ancak [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) özelliği `EmployeeDetailPage` sınıfı XAML içinde ayarlanabilir, burada arka plan kod örneğine ayarlanır bir `Employee` nesnesi:

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

Sırada [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) her özellik *hedef* nesne tek tek ayarlanabilir, bu gerekli değildir. `BindingContext` tüm alt gruplar tarafından devralınır özel bir özelliktir. Bu nedenle, `BindingContext` üzerinde [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) ayarlanmış bir `Employee` örneği, tüm alt öğelerinin `ContentPage` aynı `BindingContext`ve Genelözelliklerinibağlayabilirsiniz`Employee`nesne.

### <a name="c35"></a>C&#35;

Aşağıdaki kod, veri bağlama C# dilinde gerçekleştirmenin bir örnek göstermektedir:

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

[ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Oluşturucu örneği geçirilen bir `Employee` nesne ve ayarlar [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) bağlamak için nesne. Bir [ `Entry` ](xref:Xamarin.Forms.Entry) denetimi oluşturulana ve bağlama arasında [ `Entry.Text` ](xref:Xamarin.Forms.Entry.Text) özelliği ve `FirstName` özelliği *kaynak* nesnesi ayarlanır. Yapılan değişiklikler `Entry` denetimi otomatik olarak yayılır için `employeeToDisplay` nesne. Benzer şekilde, değişiklikler yapılırsa `employeeToDisplay.FirstName` özelliği Xamarin.Forms bağlama altyapısı ayrıca güncelleştirecek içeriği `Entry` denetimi. Bu olarak bilinen bir *iki yönlü bağlama*. Model sınıfı çalışmak için iki yönlü bağlamaya sırayla uygulamalıdır `INotifyPropertyChanged` arabirimi.

`SetBinding` Yöntem iki parametre alır. İlk parametre bağlama türü hakkındaki bilgileri belirtir. İkinci parametre ne bağlamak veya nasıl bağlanacağı hakkında bilgi sağlamak için kullanılır. İkinci parametre, çoğu durumda, özelliğin adını tutan yalnızca bir dizedir [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Aşağıdaki söz dizimini bağlamak için kullanılan `BindingContext` doğrudan:

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

Nokta sözdizimini kullanmak için Xamarin.Forms söyler [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) veri kaynağı yerine bir özellik olarak `BindingContext`. Bu durumlarda yararlı olur `BindingContext` basit bir tür olduğu gibi bir `string` veya `int`.

### <a name="property-change-notification"></a>Özellik değişikliği bildirimi

Varsayılan olarak, *hedef* nesnesi yalnızca bir değeri aldığında *kaynak* bağlama oluşturulduğunda nesne. Veri kaynağı ile eşitlenmiş kullanıcı Arabirimi tutmak bildirmek için bir yol olmalıdır *hedef* nesne zaman *kaynak* nesnesi değişti. Bu mekanizma tarafından sağlanan `INotifyPropertyChanged` arabirimi. Temel alınan özellik değeri değiştiğinde bu arabirimi uygulayan herhangi bir veriye bağlı denetim bildirimleri sağlar.

Nesneleri uygulayan `INotifyPropertyChanged` oluşturmalıdır `PropertyChanged` özelliklerini birini yeni bir değerle aşağıdaki kod örneğinde gösterildiği gibi güncelleştirildiğinde olay:

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

Zaman `MyObject.FirstName` özellik değişiklikleri `OnPropertyChanged` yöntemi çağrılır, hangi oluşturacağı `PropertyChanged` olay. Gereksiz olayları tetikleme, önlemek için `PropertyChanged` değil olayı özellik değerini değiştirirseniz değil.

Unutmayın, `OnPropertyChanged` yöntemi `propertyName` parametresi ile donatılmış `CallerMemberName` özniteliği. Bu durumlarda sağlar `OnPropertyChanged` ile yöntemi çağrıldığında bir `null` değeri `CallerMemberName` özniteliğini çağrılan yöntemin adını sağlayın `OnPropertyChanged`.

## <a name="navigation"></a>Gezinti

Xamarin.Forms sağlar bağlı olarak farklı sayfa gezinti deneyimleri sayısı [ `Page` ](xref:Xamarin.Forms.Page) kullanılan yazın. İçin [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) olan iki Gezinti deneyimleri örnekleri vardır:

- [Hiyerarşik Gezinme](#Hierarchical_Navigation)
- [Kalıcı Gezinti](#Modal_Navigation)

[ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) Ve [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) sınıfları başka gezinme deneyimler sağlar. Daha fazla bilgi için [Gezinti](~/xamarin-forms/app-fundamentals/navigation/index.md).

### <a name="hierarchical-navigation"></a>Hiyerarşik gezinme

[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Sınıfı kullanıcının olduğu iletir ve geriye doğru istediğiniz şekilde, sayfada gezinme için bir hiyerarşik Gezinti deneyimi sağlar. Sınıfın uyguladığı son giren ilk çıkar (LIFO) yığını olarak Gezinti [ `Page` ](xref:Xamarin.Forms.Page) nesneleri.

Hiyerarşik Gezinti [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) yığını gezinmek için kullanılan sınıfı [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) nesneleri. Bir sayfadan diğerine taşımak için bir uygulama burada etkin sayfa olur gezinme yığınında yeni bir sayfaya gönderir. Önceki sayfaya geri dönmek için gezinme yığınında geçerli sayfadaki uygulama açılır ve yeni en üstte sayfa etkin sayfa olur.

Bir gezinme yığınına eklenen ilk sayfa olarak adlandırılır *kök* sayfasında uygulama ve aşağıdaki kod örneği gösterir bu nasıl yapılır:

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

Gitmek için `LoginPage`, çağırmak gerekli olan [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) metodunda [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) geçerli sayfasında, aşağıdaki kod örneğinde kanıtlanabilir olarak özelliği:

```csharp
await Navigation.PushAsync(new LoginPage());
```

Bu yeni neden `LoginPage` burada etkin sayfa olur Gezinti yığına itilecek nesne.

Etkin sayfa gezinti yığından tuşlarına basarak POP *geri* cihazda düğme bağımsız olarak, bu cihaz üzerinde fiziksel bir düğme olup olmadığını veya bir ekrandaki bir düğme. Program aracılığıyla önceki sayfaya dönmek için `LoginPage` örneği çağırmalıdır [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
await Navigation.PopAsync();
```

Hiyerarşik gezinme hakkında daha fazla bilgi için bkz. [hiyerarşik gezinme](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

### <a name="modal-navigation"></a>Kalıcı Gezinti

Xamarin.Forms kalıcı sayfalar için destek sağlar. Kalıcı bir sayfa, görev tamamlandı veya iptal kadar UZAĞINIZDA erişilemeyeceğini kendi içinde bir görevi tamamlamak için kullanıcıların önerir.

Kalıcı bir sayfa herhangi biri olabilir [ `Page` ](xref:Xamarin.Forms.Page) Xamarin.Forms tarafından desteklenen türleri. Kalıcı bir sayfaya uygulaması görüntülemek için Gezinti yığın üstüne burada etkin sayfa olur gönderir. Önceki sayfaya dönmek için gezinme yığınında geçerli sayfadaki uygulama açılır ve yeni en üstte sayfa etkin sayfa olur.

Kalıcı Gezinti yöntemleri tarafından sunulur [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) herhangi bir özellikte [ `Page` ](xref:Xamarin.Forms.Page) türetilmiş türler. [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Özelliğini de sunan bir [ `ModalStack` ](xref:Xamarin.Forms.INavigation.ModalStack) özelliği kendisinden gezinme yığınında kalıcı sayfalar elde edilebilir. Ancak, kalıcı yığın düzenleme gerçekleştirme ya da kalıcı Gezinti kök sayfasına pencerelerinin konsepti yoktur. Bu işlemler temel platformlarda Evrensel desteklenmeyen olmasıdır.

> [!NOTE]
> A [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) örneği kalıcı sayfa gezintisi gerçekleştirmek için gerekli değildir.

Kalıcı olarak gitmek için `LoginPage` çağırmak gerekli olan [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) metodunda [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) geçerli sayfasında, aşağıdaki kod örneğinde kanıtlanabilir olarak özelliği :

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

Bu neden `LoginPage` gezinme yığınına, burada etkin sayfa olur itilecek örneği.

Etkin sayfa gezinti yığından tuşlarına basarak POP *geri* cihazda düğme bağımsız olarak, bu cihaz üzerinde fiziksel bir düğme olup olmadığını veya bir ekrandaki bir düğme. Program aracılığıyla geldikleri sayfaya geri dönmek için `LoginPage` örneği çağırmalıdır [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
await Navigation.PopModalAsync();
```

Bu neden `LoginPage` örneği etkin sayfa olma yeni en üstte sayfa gezinti yığından kaldırılacak.

Kalıcı gezintisi hakkında daha fazla bilgi için bkz. [kalıcı sayfalar](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="next-steps"></a>Sonraki adımlar

Giriş niteliğindeki bu makalede, Xamarin.Forms uygulamaları yazmaya başlamak etkinleştirmeniz gerekir. Önerilen sonraki adımları aşağıdaki işlevleri okumayı içerir:

- Denetim şablonları, çalışma zamanında uygulama sayfaları kolayca teması ve re-theme olanağı sağlar. Daha fazla bilgi için [denetim şablonları](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).
- Veri şablonları desteklenen denetimlere verilerini sunumu tanımlama yeteneği sağlar. Daha fazla bilgi için [veri şablonları](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Paylaşılan kod aracılığıyla yerel işlevler erişebilir [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) sınıfı. Daha fazla bilgi için [DependencyService yerel özelliklerle erişme](~/xamarin-forms/app-fundamentals/dependency-service/index.md).
- Xamarin.Forms, bu nedenle sınıflar arasında bağ azaltma, ileti göndermek ve almak için basit bir ileti sistemi hizmeti içerir. Daha fazla bilgi için [Yayımla ve abone ol MessagingCenter ile](~/xamarin-forms/app-fundamentals/messaging-center.md).
- Her sayfa, Düzen ve denetimi farklı kullanarak her platformunda işlenen bir `Renderer` sırayla yerel bir denetim oluşturan sınıf ekranda düzenler ve paylaşılan kodu belirtilen davranışı ekler. Geliştiriciler, kendi özel uygulayabilirsiniz `Renderer` sınıflar görünümünü ve davranışını özelleştirin. Daha fazla bilgi için [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).
- Etkileri, özelleştirilmek üzere her platformda yerel denetimler için de izin verir. Etkileri platforma özgü projelerinde sınıflara tarafından oluşturulan [ `PlatformEffect` ](xref:Xamarin.Forms.PlatformEffect`2) denetlemek ve uygun bir Xamarin.Forms denetimine ekleyerek tüketilir. Daha fazla bilgi için [etkileri](~/xamarin-forms/app-fundamentals/effects/index.md).

Alternatif olarak, [ _Xamarin.Forms ile Mobile Apps oluşturma_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md), bir kitap Charles Petzold ile Xamarin.Forms hakkında daha fazla bilgi edinmek için iyi bir yerdir. Kitap bir PDF olarak veya çeşitli e-kitap biçimlerde kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [HTML Temelleri](~/xamarin-forms/xaml/xaml-basics/index.md)
- [Denetimler Başvurusu](~/xamarin-forms/user-interface/controls/index.md)
- [Kullanıcı Arabirimi](~/xamarin-forms/user-interface/index.md)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Kullanmaya başlama örnekleri](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms API Başvurusu](xref:Xamarin.Forms)
- [Ücretsiz kendi öğrenme (video)](https://university.xamarin.com/self-guided)