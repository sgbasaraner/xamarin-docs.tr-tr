---
title: "Ana-Ayrıntı Sayfası"
description: "Xamarin.Forms MasterDetailPage iki ilgili sayfaları – bilgilerinin öğeleri sunan bir ana sayfa ve ana sayfada öğeleri hakkında ayrıntılar sunar ayrıntı sayfası yöneten bir sayfadır. Bu makalede bir MasterDetailPage ve kendi bilgi sayfaları arasında gezinmek nasıl kullanılacağı açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: f7e949902e2f960a9aa68c600514b7fefc8ae30d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="master-detail-page"></a>Ana-Ayrıntı Sayfası

_Xamarin.Forms MasterDetailPage iki ilgili sayfaları – bilgilerinin öğeleri sunan bir ana sayfa ve ana sayfada öğeleri hakkında ayrıntılar sunar ayrıntı sayfası yöneten bir sayfadır. Bu makalede bir MasterDetailPage ve kendi bilgi sayfaları arasında gezinmek nasıl kullanılacağı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Bir ana sayfa, genellikle aşağıdaki ekran görüntülerinde gösterildiği gibi öğeleri listesini görüntüler:

[![](master-detail-page-images/masterpage-components.png "Ana sayfa bileşenleri")](master-detail-page-images/masterpage-components-large.png "ana sayfa bileşenleri")

Öğe listesinin konumunu her platformda aynıdır ve öğelerden birini seçerek karşılık gelen Ayrıntı Sayfası'na gidin. Ayrıca, ana sayfa etkin Ayrıntı Sayfası'na gitmek için kullanılan bir düğmeyi içeren bir gezinti çubuğu özellikleri:

- İOS, gezinti çubuğunda sayfanın en üstünde bulunduğundan ve ayrıntı sayfasına giden bir düğme vardır. Ayrıca, etkin ayrıntı sayfası için ana sayfa sol geçirilerek gittiğinizde.
- Android'de, gezinti çubuğunda sayfanın en üstünde bulunduğundan ve bir başlık, simge ve giden bir düğme Ayrıntı Sayfası'na görüntüler. Simge tanımlanan `[Activity]` süsler özniteliği `MainActivity` Android platforma özgü projesinde sınıfı. Ayrıca, etkin ayrıntı sayfası için sol ana sayfaya geçirmeyi tarafından ekranın sağ uçta ayrıntı sayfası dokunarak şirket uygulamalarına ve e dokunabilirsiniz gezinilebilir *geri* ekranın altındaki düğmesini.
- Üzerinde Evrensel Windows Platformu (UWP), gezinti çubuğunda sayfanın en üstünde bulunduğundan ve ayrıntı sayfasına giden bir düğme vardır.

Öğesine karşılık gelen bir ayrıntı sayfa görüntüler verileri ana sayfa seçili ve ayrıntı sayfası ana bileşenlerini aşağıdaki ekran görüntülerinde gösterilir:

![](master-detail-page-images/detailpage-components.png "Ayrıntı Sayfası bileşenleri")

Ayrıntı Sayfası içeriği platforma bağımlı bir gezinti çubuğu içerir:

- İOS, gezinti çubuğunda sayfanın en üstünde bulunduğundan ve bir başlık görüntüler ve ayrıntı sayfası örneği olarak paketlenir koşuluyla ana sayfaya döndüren bir düğme [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) örneği. Ayrıca, ana sayfa için sağa ayrıntı sayfası geçirilerek döndürülebilir.
- Android, gezinti çubuğunda sayfanın en üstünde bulunduğundan ve bir başlık, simge ve ana sayfaya döndüren bir düğme görüntüler. Simge tanımlanan `[Activity]` süsler özniteliği `MainActivity` Android platforma özgü projesinde sınıfı.
- UWP, gezinti çubuğunda sayfanın en üstünde bulunduğundan ve bir başlık görüntüler ve ana sayfaya döndüren bir düğme vardır.

### <a name="navigation-behavior"></a>Gezinti davranışı

Ana ve ayrıntı sayfaları arasında gezinme deneyimi platform bağımlı davranıştır:

- İOS, ayrıntı sayfası *slayt* ana sayfa slayt olarak sağdan sola ve ayrıntı sol bölümü için hala görünür sayfasıdır.
- Android, ayrıntı ve ana sayfalarıdır *üzerini kaplamış şekilde* birbirlerine.
- UWP üzerinde ayrıntı ve ana sayfalarıdır *takas*.

Daha fazla ayrıntı sayfasının görünür olması için iOS ve Android ana sayfada benzer genişliği dikey modunda, ana sayfa olarak olan benzer davranış yatay modunda izlenir.

Gezinti davranışını denetleme hakkında daha fazla bilgi için bkz: [ayrıntı sayfası görüntü davranışını denetleme](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Bir MasterDetailPage oluşturma

A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) içeren [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) ve [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) türü her ikisi de özellikleri [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), hangi almak ve ana ve ayrıntı sayfaları sırasıyla ayarlamak için kullanılır.

> [!IMPORTANT]
> A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) kök sayfa olacak şekilde tasarlanmıştır ve bir alt sayfası diğer sayfa türlerinde beklenmeyen ve tutarsız davranışlara neden olarak kullanma. Ayrıca, ana sayfasının önerilir bir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) her zaman olması gereken bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örneği ve ayrıntı sayfası ile yalnızca doldurulmalıdır [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), ve `ContentPage` örnekleri. Bu tüm platformlarda tutarlı bir kullanıcı deneyimi sağlamak için yardımcı olur.

Aşağıdaki XAML kodu örnekte gösterildiği bir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) ayarlayan [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) ve [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) özellikleri:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  xmlns:local="clr-namespace:MasterDetailPageNavigation;assembly=MasterDetailPageNavigation"
                  x:Class="MasterDetailPageNavigation.MainPage">
    <MasterDetailPage.Master>
        <local:MasterPage x:Name="masterPage" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Aşağıdaki kod örneğinde eşdeğer gösterir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) C# ' ta oluşturuldu:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        masterPage = new MasterPageCS ();
        Master = masterPage;
        Detail = new NavigationPage (new ContactsPageCS ());
        ...
    }
    ...
}
```

[ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) Özelliği ayarlanmış bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örneği. [ `MasterDetailPage.Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) Özelliği ayarlanmış bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) içeren bir `ContentPage` örneği.

### <a name="creating-the-master-page"></a>Ana sayfa oluşturma

Aşağıdaki XAML kod örneği bildirimi gösterir `MasterPage` aracılığıyla başvurulan nesne [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) özelliği:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:MasterPageItem}">
                    <local:MasterPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:MasterPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:MasterPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="5,10">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30"/>
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding IconSource}" />
                            <Label Grid.Column="1" Text="{Binding Title}" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Sayfa oluşan bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) , doldurulur XAML'de verilerle ayarlayarak kendi [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) bir dizi özelliğine `MasterPageItem` örnekleri. Her `MasterPageItem` tanımlar `Title`, `IconSource`, ve `TargetType` özellikleri.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) atandığı [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) her görüntülenecek özelliği, `MasterPageItem`. `DataTemplate` İçeren bir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) , oluşur bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) ve [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/). [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Görüntüler `IconSource` özellik değeri ve [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) görüntüler `Title` her özellik değerini `MasterPageItem`.

Sayfanın kendi [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) ve [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) özelliklerini ayarla. Başlık çubuğu ayrıntı sayfası sahip olması koşuluyla simgesi ayrıntı sayfasında görünür. İos'ta bu etkinleştirilmelidir ayrıntı sayfası örneğinde kaydırma tarafından bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) örneği.

> [!NOTE]
> [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) Sayfa olmalıdır, [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) özelliğini ayarlayın ya da bir özel durum meydana gelir.

Aşağıdaki kod örneği, C# ' de oluşturulan eşdeğer sayfaya gösterir:

```csharp
public class MasterPageCS : ContentPage
{
  public ListView ListView { get { return listView; } }

  ListView listView;

  public MasterPageCS ()
  {
    var masterPageItems = new List<MasterPageItem> ();
    masterPageItems.Add (new MasterPageItem {
      Title = "Contacts",
      IconSource = "contacts.png",
      TargetType = typeof(ContactsPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "TodoList",
      IconSource = "todo.png",
      TargetType = typeof(TodoListPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "Reminders",
      IconSource = "reminders.png",
      TargetType = typeof(ReminderPageCS)
    });

    listView = new ListView {
      ItemsSource = masterPageItems,
      ItemTemplate = new DataTemplate (() => {
        var grid = new Grid { Padding = new Thickness(5, 10) };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(30) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

        var image = new Image();
        image.SetBinding(Image.SourceProperty, "IconSource");
        var label = new Label { VerticalOptions = LayoutOptions.FillAndExpand };
        label.SetBinding(Label.TextProperty, "Title");

        grid.Children.Add(image);
        grid.Children.Add(label, 1, 0);

        return new ViewCell { View = grid };
      }),
      SeparatorVisibility = SeparatorVisibility.None
    };

    Icon = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

Aşağıdaki ekran görüntüleri her platformda ana sayfayı göster:

![](master-detail-page-images/masterpage.png "Ana sayfası örneği")

### <a name="creating-and-displaying-the-detail-page"></a>Oluşturma ve ayrıntı sayfası görüntüleme

`MasterPage` Örneğini içeren bir `ListView` gösteren özelliği, [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) örneği böylece `MainPage` [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) örneği kaydolabilir bir İşlenecek Olay işleyicisi [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) olay. Böylece `MainPage` ayarlamak için örnek [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) seçili temsil eden sayfa özelliğine `ListView` öğesi. Aşağıdaki kod örneği olay işleyicisini gösterir:

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.ListView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.ListView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected` Yöntemi aşağıdaki eylemleri gerçekleştirir:

- Alır [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) gelen [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) örneği ve, olmadığını sağlanan `null`, ayrıntı sayfası depolanansayfatürüyenibirörneğiniayarlar`TargetType`özelliği `MasterPageItem`. Sayfa türü içinde kaydırılan bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) aracılığıyla başvurulan simgesi emin olmak için örnek [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) özelliği `MasterPage` iOS ayrıntı sayfalarında gösterilir.
- Seçilen öğeyi [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ayarlanır `null` hiçbiri emin olmak için `ListView` öğeleri sonraki seçilecektir `MasterPage` sunulur.
- Ayrıntı Sayfası ayarlayarak kullanıcıya sunulan [ `MasterDetailPage.IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) özelliğine `false`. Bu özellik, ana veya ayrıntı sayfası sunulan olup olmadığını denetler. Kümesine `true` ana sayfayı görüntülemek için ve `false` ayrıntı sayfasını görüntüleyin.

Aşağıdaki ekran görüntüleri Göster `ContactPage` ana sayfada seçilen sonra gösterilen Ayrıntı Sayfası:

![](master-detail-page-images/detailpage.png "Ayrıntı Sayfası örneği")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Ayrıntı sayfa görüntüleme davranışını denetleme

Nasıl [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) ana ve ayrıntı sayfaları yönetir uygulama çalışıp çalışmadığını bir telefon veya tablet, cihaz yönünü ve değerini üzerinde bağımlı [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) özellik. Bu özellik, ayrıntı sayfası nasıl görüntüleneceğini belirler. Olası değerler şunlardır:

- **Varsayılan** – sayfaları platform varsayılan kullanılarak görüntülenir.
- **Popover** – ayrıntı sayfası kapsayan veya kısmen ana sayfa kapsar.
- **Bölünmüş** – ana sayfanın sol tarafta görüntülenir ve sağ tarafta ayrıntı sayfasıdır.
- **SplitOnLandscape** – bölünmüş ekran cihaz yatay yönde olduğunda kullanılır.
- **SplitOnPortrait** – bölünmüş ekran cihaz dikey yönde olduğunda kullanılır.

Aşağıdaki XAML kod örneğinde nasıl ayarlanacağını gösterir [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) özelliği bir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

Aşağıdaki kod örneğinde eşdeğer gösterir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) C# ' ta oluşturuldu:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        MasterBehavior = MasterBehavior.Popover;
        ...
    }
}
```

Ancak, değeri [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) özelliği, tabletler veya Masaüstü üzerinde çalışan uygulamalar yalnızca etkiler. Telefonlarda her zaman çalışan uygulamaların olması *Popover* davranışı.

## <a name="summary"></a>Özet

Bu makalede nasıl kullanılacağı gösterilmiştir bir [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) ve kendi bilgi sayfaları arasında gidin. Xamarin.Forms `MasterDetailPage` iki sayfayla ilgili bilgileri – öğeleri sunan bir ana sayfa ve ana sayfada öğeleri hakkında ayrıntılar sunar ayrıntı sayfası yöneten bir sayfadır.


## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa çeşit](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)
