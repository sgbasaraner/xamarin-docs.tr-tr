---
title: Sekmeli sayfası
description: Xamarin.Forms TabbedPage sekmeler ve daha büyük bir ayrıntı alanı listesini ayrıntı alanına içeriği yüklerken her sekmesi oluşur. Bu makalede bir TabbedPage sayfaları toplulukta gezinmek için nasıl kullanılacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 7af9248e706e615ea3e693a58a5f7664e8dc4daa
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847818"
---
# <a name="tabbed-page"></a>Sekmeli sayfası

_Xamarin.Forms TabbedPage sekmeler ve daha büyük bir ayrıntı alanı listesini ayrıntı alanına içeriği yüklerken her sekmesi oluşur. Bu makalede bir TabbedPage sayfaları toplulukta gezinmek için nasıl kullanılacağı gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Aşağıdaki ekran görüntüleri Göster bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) her platformda:

![](tabbed-page-images/tab1.png "TabbedPage örneği")

Aşağıdaki ekran görüntüleri her platformda sekme biçimi odaklanır:

![](tabbed-page-images/tabbedpage-components.png "TabbedPage sekmesini bileşenleri")

Düzenini bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)ve kendi sekmeleri platformuna bağlıdır:

- İOS, ekranın alt kısmında sekmeler listesi görüntülenir ve yukarıdaki ayrıntı alandır. Her sekme de normal çözümlemesi için Saydamlığı olan 30 PNG x 30, yüksek çözünürlüklü için 60 x 60 ve 90 x 90 iPhone 6 olması gereken bir simge görüntüsü sahip çözümleme artı. Beşten fazla sekme varsa bir *daha fazla* sekmesi görüntülenir, kullanılabilecek ek sekmeler erişmek için. Xamarin.Forms uygulaması görüntülerinde yükleme hakkında daha fazla bilgi için bkz: [görüntülerle çalışma](~/xamarin-forms/user-interface/images.md). Simge gereksinimleri hakkında daha fazla bilgi için bkz: [sekmeli uygulamaları oluşturma](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Unutmayın `TabbedRenderer` iOS bir geçersiz kılınabilir için `GetIcon` sekmesini simgeler belirtilen bir kaynaktan yüklemek için kullanılan yöntem. Bu geçersiz kılma üzerinde simgelerle SVG görüntüleri kullanmayı mümkün kılar bir `TabbedPage`. Ayrıca, seçili ve seçili sürümler simge sağlanabilir.

- Android, ekranın en üstünde sekmeler listesi görüntülenir ve ayrıntı alanı aşağıdadır. Sekme adlarını otomatik olarak büyük harfe ve varsa bir ekranda sığmayacak kadar çok sayıda kullanıcı sekmeler koleksiyonunu gezinebilirsiniz.

    > [!NOTE]
  > Uygulama Android kullanırken, her sekme ayrıca bir simge görüntülemek unutmayın. Ayrıca, `TabbedPageRenderer` Android uygulama bir overridable için `SetTabIcon` bir özel akıştan sekmesini simgeler yüklemek için kullanılan yöntem `Drawable`. Bu geçersiz kılma üzerinde simgelerle SVG görüntüleri kullanmayı mümkün kılar bir `TabbedPage`.

- Windows tablet form-faktörleri, sekmelerin her zaman görünmez ve kullanıcılar sağdan aşağı gerekir (veya bağlı fare varsa sağ tıklatın,) sekmeleri görüntülemek için bir `TabbedPage` (aşağıda gösterildiği gibi).

![](tabbed-page-images/windows-tabs.png "Windows TabbedPage sekmeleri")

## <a name="creating-a-tabbedpage"></a>Bir TabbedPage oluşturma

İki yaklaşım, oluşturmak için kullanılabilecek bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

- [Doldurmak](#Populating_a_TabbedPage_with_a_Page_Collection) [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) alt koleksiyonuyla [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) koleksiyonu gibi nesneler [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örnekleri.
- [Ata](#Populating_a_TabbedPage_with_a_Template) bir koleksiyona [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) özelliği ve ata bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) için [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) sayfalar için döndürülecek özelliği koleksiyonundaki nesneleri.

Her iki yaklaşımın ile [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) her sekme kullanıcının seçtiği gibi her bir sayfa görüntülenir.

> [!NOTE]
> Önerilen bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) ile doldurulması gerekir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) ve [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)yalnızca örnekleri. Bu tüm platformlarda tutarlı bir kullanıcı deneyimi sağlamak için yardımcı olur.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>TabbedPage sayfa koleksiyonu ile doldurma

Aşağıdaki XAML kodu örnekte gösterildiği bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) alt koleksiyonuyla doldurarak oluşturulan [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) nesneler:

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

Aşağıdaki kod örneğinde eşdeğer gösterir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) C# ' ta oluşturuldu:

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) İle iki alt doldurulur [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) nesneleri. İlk alt olan bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örneği ve ikinci sekmesinde bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) içeren bir `ContentPage` örneği.

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) UI sanallaştırma desteklemez. Bu nedenle, performansı etkilenebilir `TabbedPage` çok fazla sayıda alt öğeleri içerir.

Aşağıdaki ekran görüntüleri Göster `TodayPage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) üzerinde gösterilen örnek *Bugün* sekmesi:

![](tabbed-page-images/today-page.png "Bir TabbedPage ContentPage")

Seçme *zamanlama* sekmesinde görüntüler `SchedulePage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) içinde kaydırılan örneği bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) örneği ve gösterilir Aşağıdaki ekran görüntüsü:

![](tabbed-page-images/schedule-page.png "Bir TabbedPage NavigationPage")

Düzenini hakkında bilgi için bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), bkz: [gerçekleştirme Gezinti](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Yerleştirmek için kabul edilebilir olmakla birlikte bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) içine bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), onu yerleştirmek için önerilmez bir `TabbedPage` içine bir `NavigationPage`. Bunun nedeni, iOS, bir `UITabBarController` her zaman için sarmalayıcı görevi gören `UINavigationController`. Daha fazla bilgi için bkz: [birleştirilmiş görünümü denetleyicisi arabirimleri](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) iOS Geliştirici Kitaplığı'nda.

#### <a name="navigation-inside-a-tab"></a>Sekme içinde gezinme

Gezinti yapılabilir ikinci sekmesinden çağırarak [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) yöntemi [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) özelliği [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örneği Aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

Bu neden `UpcomingAppointmentsPage` Gezinti yığına burada etkin sayfa haline gelir edilmesini örneği. Bu, aşağıdaki ekran görüntülerinde gösterilir:

![](tabbed-page-images/navigationpage.png "Sekme içinde gezinme")

Gezinti kullanarak gerçekleştirme hakkında daha fazla bilgi için [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) sınıfı için bkz: [hiyerarşik Gezinti](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Bir şablonla TabbedPage doldurma

Aşağıdaki XAML kodu örnekte gösterildiği bir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) atama tarafından oluşturulan bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) için [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) sayfalar için döndürülecek özelliği derlemedeki nesneler:

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Ayarlayarak verilerle doldurmuş [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) arka plan kodu dosyanın oluşturucusu özelliğinde:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

Aşağıdaki kod örneğinde eşdeğer gösterir [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) C# ' ta oluşturuldu:

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

Her sekme görüntüler bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) bir dizi kullanan [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ve [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) sekme için verileri görüntülemek için örnekleri. Aşağıdaki ekran görüntüleri içeriğini göster *Tamarin* sekmesi:

![](tabbed-page-images/tab3.png "Bir şablonla TabbedPage doldurma")

Başka bir sekmeye sonra seçerek içerik için sekmeyi görüntüler.

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) UI sanallaştırma desteklemez. Bu nedenle, performansı etkilenebilir `TabbedPage` çok fazla sayıda alt öğeleri içerir.

Hakkında daha fazla bilgi için [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), bkz: [bölüm 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold'un'ın Xamarin.Forms defteri.

## <a name="summary"></a>Özet

Bu makalede bir TabbedPage sayfaları toplulukta gezinmek için nasıl kullanılacağı gösterilmektedir. Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) her sekmesi ayrıntısı alanına içeriği yüklerken bir sekmeler ve daha büyük bir ayrıntı alanı listesinden oluşur.


## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa çeşit](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)
