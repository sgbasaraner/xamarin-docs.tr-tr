---
title: Xamarin.Forms sekmeli sayfa
description: Xamarin.Forms TabbedPage sekmeler ve daha büyük bir ayrıntı alanı listesini her sekmesi ayrıntısı alanına içerik yükleme oluşur. Bu makalede sayfaların koleksiyonda gitmek için bir TabbedPage kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 3eb978780222da2050fc91dfa41c68ef4bd3b6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996301"
---
# <a name="xamarinforms-tabbed-page"></a>Xamarin.Forms sekmeli sayfa

_Xamarin.Forms TabbedPage sekmeler ve daha büyük bir ayrıntı alanı listesini her sekmesi ayrıntısı alanına içerik yükleme oluşur. Bu makalede sayfaların koleksiyonda gitmek için bir TabbedPage kullanmayı gösterir._

## <a name="overview"></a>Genel Bakış

Aşağıdaki ekran görüntüleri Göster bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) her platformda:

![](tabbed-page-images/tab1.png "TabbedPage örneği")

Aşağıdaki ekran görüntüleri her platformda sekme biçimi odaklanır:

![](tabbed-page-images/tabbedpage-components.png "TabbedPage sekmesini bileşenleri")

Düzenini bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)ve sekmelerinin, seçtiğiniz platforma bağlıdır:

- İOS, ekranın altındaki sekme listesi görüntülenir ve yukarıda ayrıntılı alandır. Her sekme bir 30 x 30 PNG saydamlığına normal çözümlemesi için yüksek çözünürlüklü için 60 x 60 ve iPhone 6 90 x 90 olması gereken bir simge görüntüsü de sahiptir. Ayrıca çözüm. Beşten fazla sekme varsa bir *daha fazla* sekmesi görüntülenir, kullanılabilen ek sekmeleri erişmek için. Xamarin.Forms uygulaması görüntülerde yükleme hakkında daha fazla bilgi için bkz. [görüntülerle çalışma](~/xamarin-forms/user-interface/images.md). Simge gereksinimleri hakkında daha fazla bilgi için bkz. [sekmeli uygulamaları oluşturma](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Unutmayın `TabbedRenderer` için iOS geçersiz kılınabilir bir `GetIcon` sekme simgelerini belirli bir kaynaktan yüklemek için kullanılan yöntem. Bu geçersiz kılma üzerinde simgelerle SVG görüntüleri kullanmayı mümkün kılar bir `TabbedPage`. Ayrıca, bir simge seçili ve seçili olmayan sürümleri sağlanabilir.

- Android, sekmeler listesinde varsayılan olarak ekranın en üstünde görünür ve ayrıntı alanı aşağıda verilmiştir. Ancak, platforma özgü ile ekranın altındaki sekme listesi taşınabilir. Daha fazla bilgi için [ayarı TabbedPage araç çubuğu yerleştirme ve renk](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar).

    > [!NOTE]
  > Not AppCompat Android'de kullanırken her sekme ayrıca bir simge görüntüler. Ayrıca, `TabbedPageRenderer` için Android AppCompat geçersiz kılınabilir bir `SetTabIcon` bir özel sekme simgelerini yüklemek için kullanılan yöntem `Drawable`. Bu geçersiz kılma üzerinde simgelerle SVG görüntüleri kullanmayı mümkün kılar bir `TabbedPage`.

- Windows tablet form faktörleri üzerinde sekmeleri her zaman görünür durumda ve kullanıcıların ihtiyaç geçirme gitme (veya sağ tıklayın, bunlar bağlı bir fareniz varsa) sekmeleri görüntülemek için bir `TabbedPage` (aşağıda gösterildiği gibi).

![](tabbed-page-images/windows-tabs.png "Windows üzerinde TabbedPage sekmeleri")

## <a name="creating-a-tabbedpage"></a>Bir TabbedPage oluşturma

İki yaklaşım oluşturmak için kullanılabilir bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

- [Doldurma](#Populating_a_TabbedPage_with_a_Page_Collection) [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) alt koleksiyonuyla [ `Page` ](xref:Xamarin.Forms.Page) koleksiyonu gibi nesneleri [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) örnekleri.
- [Ata](#Populating_a_TabbedPage_with_a_Template) bir koleksiyona [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) özelliği ve ata bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) için [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) sayfaları için döndürülecek özelliği koleksiyondaki nesneleri.

Her iki yaklaşım ile [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) her sekme kullanıcının seçtiği gibi her bir sayfa görüntülenir.

> [!NOTE]
> Bırakmanız önerilir bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) ile doldurulmalıdır [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) ve [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)yalnızca örnekler. Bu, tüm platformlar arasında tutarlı bir kullanıcı deneyimi sağlamaya yardımcı olur.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Sayfa koleksiyonu ile bir TabbedPage doldurma

Aşağıdaki XAML kod örnekte gösterildiği bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) alt koleksiyonu ile doldurarak oluşturulmuş [ `Page` ](xref:Xamarin.Forms.Page) nesneler:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" Icon="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

Aşağıdaki kod örneği, eşdeğer gösterir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) C# içinde oluşturulan:

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.Icon = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) İki alt ile doldurulur [ `Page` ](xref:Xamarin.Forms.Page) nesneleri. İlk alt öğesi bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) ikinci sekme ise örneği bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) içeren bir `ContentPage` örneği.

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Kullanıcı Arabirimi sanallaştırma desteklemez. Bu nedenle, performans etkilenebilir `TabbedPage` çok fazla alt öğe içeriyor.

Aşağıdaki ekran görüntüleri Göster `TodayPage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) üzerinde gösterilen örnek *Bugün* sekmesinde:

![](tabbed-page-images/today-page.png "İçinde bir TabbedPage ContentPage")

Seçme *zamanlama* görüntüler sekmesine `SchedulePage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) içine sarmalanır örneği, bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) örneği ve gösterilir Aşağıdaki ekran görüntüsüne bakın:

![](tabbed-page-images/schedule-page.png "İçinde bir TabbedPage NavigationPage")

Düzeni hakkında daha fazla bilgi için bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), bkz: [gerçekleştirme Gezinti](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Yerleştirmek için kabul edilebilir olmakla birlikte bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) içine bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), yerleştirmek için önermedi bir `TabbedPage` içine bir `NavigationPage`. Bunun nedeni, iOS, bir `UITabBarController` her zaman için bir sarmalayıcı görevi gören `UINavigationController`. Daha fazla bilgi için [birleştirilmiş görünüm denetleyicisi arabirimleri](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) iOS Geliştirici Kitaplığı'nda.

#### <a name="navigation-inside-a-tab"></a>Bir sekme içinde gezinme

Gezinti gerçekleştirilebilir ikinci sekmesinden çağırarak [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) metodunda [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) özelliği [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) örneği Aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

Bu neden `UpcomingAppointmentsPage` gezinme yığınına, burada etkin sayfa olur itilecek örneği. Bu, aşağıdaki ekran görüntülerinde gösterilir:

![](tabbed-page-images/navigationpage.png "Bir sekme içinde gezinme")

Gezinti gerçekleştirme hakkında daha fazla bilgi için [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) sınıfı [hiyerarşik gezinme](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Bir şablonu ile bir TabbedPage doldurma

Aşağıdaki XAML kod örnekte gösterildiği bir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) atayarak oluşturulmuş bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) için [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) sayfaları için döndürülecek özelliği koleksiyondaki nesneleri:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage">
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" Icon="monkeyicon.png">
        <StackLayout Padding="5, 25">
          <Label Text="{Binding Name}" Font="Bold,Large" HorizontalOptions="Center" />
          <Image Source="{Binding PhotoUrl}" WidthRequest="200" HeightRequest="200" />
          <StackLayout Padding="50, 10">
            <StackLayout Orientation="Horizontal">
              <Label Text="Family:" HorizontalOptions="FillAndExpand" />
              <Label Text="{Binding Family}" Font="Bold,Medium" />
            </StackLayout>
            ...
          </StackLayout>
        </StackLayout>
      </ContentPage>
    </DataTemplate>
  </TabbedPage.ItemTemplate>
</TabbedPage>
```

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Ayarlayarak veri ile doldurulan [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) arka plan kod dosyası için oluşturucu özelliği:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

Aşağıdaki kod örneği, eşdeğer gösterir [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) C# içinde oluşturulan:

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() => {
      var nameLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage {
        Icon = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children = {
            nameLabel,
            image,
            new StackLayout {
              Padding = new Thickness (50, 10),
              Children = {
                new StackLayout {
                  Orientation = StackOrientation.Horizontal,
                  Children = {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                ...
              }
            }
          }
        }
      };
      contentPage.SetBinding (TitleProperty, "Name");
      return contentPage;
    });
    ItemsSource = MonkeyDataModel.All;
  }
}
```

Her sekme görüntüler bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) bir dizi kullanan [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) ve [ `Label` ](xref:Xamarin.Forms.Label) örnekleri için sekmesinde verileri görüntülemek için. Aşağıdaki ekran görüntüleri içeriğini göster *Tamarin* sekmesinde:

![](tabbed-page-images/tab3.png "Bir şablonu ile bir TabbedPage doldurma")

Başka bir sekmesini seçip bu sekme içeriğini görüntüler.

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Kullanıcı Arabirimi sanallaştırma desteklemez. Bu nedenle, performans etkilenebilir `TabbedPage` çok fazla alt öğe içeriyor.

Hakkında daha fazla bilgi için [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), bkz: [bölüm 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold'ın Xamarin.Forms rehberi.

## <a name="summary"></a>Özet

Bu makalede sayfaların koleksiyonda gitmek için bir TabbedPage kullanma gösterilmektedir. Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) her sekmesi ayrıntısı alanına içerik yükleme, sekmeler ve daha büyük bir ayrıntı alanı listesini oluşur.


## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa çeşitleri](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
