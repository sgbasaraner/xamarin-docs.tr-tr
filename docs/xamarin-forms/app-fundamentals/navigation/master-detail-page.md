---
title: Xamarin.Forms ana öğe-ayrıntı sayfası
description: Xamarin.Forms MasterDetailPage iki ilgili verinin bilgi sayfaları – öğeleri sunan bir ana sayfa ve ana sayfada öğe ayrıntılarını sunan bir ayrıntı sayfası yöneten bir sayfadır. Bu makalede, bir MasterDetailPage kullanın ve kendi bilgi sayfaları arasında gezinmek açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: a3d0edbd933339ee8b8a0a277a4f2493cc8dc70e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997471"
---
# <a name="xamarinforms-master-detail-page"></a>Xamarin.Forms ana öğe-ayrıntı sayfası

_Xamarin.Forms MasterDetailPage iki ilgili verinin bilgi sayfaları – öğeleri sunan bir ana sayfa ve ana sayfada öğe ayrıntılarını sunan bir ayrıntı sayfası yöneten bir sayfadır. Bu makalede, bir MasterDetailPage kullanın ve kendi bilgi sayfaları arasında gezinmek açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Ana sayfaya, genellikle aşağıdaki ekran görüntülerinde gösterildiği gibi öğeleri listesini görüntüler:

[![](master-detail-page-images/masterpage-components.png "Ana sayfa bileşenleri")](master-detail-page-images/masterpage-components-large.png#lightbox "ana sayfa bileşenleri")

Her platformda öğelerin listesini konumunu aynıdır ve öğelerden birini seçerek ilgili ayrıntıları sayfasına gider. Ayrıca, ana sayfa aynı zamanda active ayrıntıları sayfasına gitmek için kullanılan bir düğme içeren bir gezinti çubuğu özellikleri:

- İOS, gezinti çubuğunda, sayfanın en üstünde bulunur ve ayrıntıları sayfasına gider bir düğmeye sahip. Ayrıca, etkin ayrıntı sayfası için ana sayfanın solundaki geçirilerek gezinilebilir.
- Android, gezinti çubuğunda, sayfanın en üstünde bulunur ve ayrıntıları sayfasına gider bir düğme bir başlık ve bir simge görüntüler. Simgenin tanımlandığı `[Activity]` düzenler özniteliği `MainActivity` Android platforma özgü projede sınıfı. Ayrıca, etkin ayrıntı sayfası için ana sayfanın solundaki çekerek, ekranın sağ uçta ayrıntı sayfası dokunarak ve dokunarak gezinilebilir *geri* ekranın alt kısmındaki düğmesi.
- Üzerindeki Evrensel Windows Platformu (UWP), gezinti çubuğunda, sayfanın en üstünde bulunur ve ayrıntıları sayfasına gider bir düğmeye sahip.

Öğesine karşılık gelen bir ayrıntı sayfası görüntüler veri ana sayfasında seçili ve aşağıdaki ekran görüntülerinde ayrıntı sayfası ana bileşenleri gösterilmektedir:

![](master-detail-page-images/detailpage-components.png "Ayrıntı Sayfası bileşenleri")

Ayrıntı Sayfası platforma bağımlı olan bir gezinti çubuğu içerir:

- İOS, gezinti çubuğunda sayfanın en üstünde mevcut bir başlık görüntüler ve ayrıntı sayfası örneği içine sarmalanır koşuluyla ana sayfaya döndüren bir düğmeye sahip [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) örneği. Ayrıca, ana sayfa için ayrıntı sayfası sağ tarafından çekerek döndürülebilir.
- Android, gezinti çubuğunda sayfanın en üstünde bulunur ve başlık, simge ve ana sayfaya döndüren bir düğme görüntülenir. Simgenin tanımlandığı `[Activity]` düzenler özniteliği `MainActivity` Android platforma özgü projede sınıfı.
- UWP, gezinti çubuğunda sayfasının en üstünde mevcut olduğundan ve bir başlık görüntüler ve ana sayfaya döndüren bir düğmeye sahip.

### <a name="navigation-behavior"></a>Gezinti davranışları

Ana ve ayrıntı sayfaları arasında gezinti deneyimi, davranışı platform bağımlı olur:

- İOS, ayrıntı sayfası *slaytları* ana sayfa slaytları olarak sağa sola ve sol bölümü ayrıntı sayfası hala görülebilir.
- Android, ayrıntı ve ana sayfalar olan *yayılan* birbirlerine.
- UWP üzerinde ayrıntılı ve ana sayfalar olan *takas*.

Daha fazla ayrıntı sayfası görünür olacak şekilde ana sayfaya iOS ve Android'de benzer bir genişlik ana sayfa dikey modda olarak olan benzer bir davranış yatay modda izlenir.

Gezinme davranışını denetleme hakkında daha fazla bilgi için bkz: [ayrıntı sayfasında görünen davranışını denetleme](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Bir MasterDetailPage oluşturma

A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) içeren [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) ve [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) türü her ikisi de özellikleri [ `Page` ](xref:Xamarin.Forms.Page), almak ve ana ve ayrıntı sayfaları sırasıyla ayarlamak için kullanılır.

> [!IMPORTANT]
> A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) kök sayfa olacak şekilde tasarlanmıştır ve bir alt sayfası diğer sayfa türlerinde beklenmeyen ve tutarsız davranışa neden, kullanarak. Ayrıca, bu ana sayfanın önerilir bir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) her zaman olmalıdır bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) örneği ve ayrıntı sayfası ile yalnızca doldurulmalıdır [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), ve `ContentPage` örnekleri. Bu, tüm platformlar arasında tutarlı bir kullanıcı deneyimi sağlamaya yardımcı olur.

Aşağıdaki XAML kod örnekte gösterildiği bir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) ayarlayan [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) ve [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) özellikleri:

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

Aşağıdaki kod örneği, eşdeğer gösterir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) C# içinde oluşturulan:

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

[ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) Özelliği bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) örneği. [ `MasterDetailPage.Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) Özelliği bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) içeren bir `ContentPage` örneği.

### <a name="creating-the-master-page"></a>Ana sayfa oluşturma

Aşağıdaki XAML kod örneği bildirimi gösterir `MasterPage` aracılığıyla başvurulan nesne [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) özelliği:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
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

Sayfa oluşan bir [ `ListView` ](xref:Xamarin.Forms.ListView) , doldurulur XAML verilerle ayarlayarak onun [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) bir dizi özelliği `MasterPageItem` örnekleri. Her `MasterPageItem` tanımlar `Title`, `IconSource`, ve `TargetType` özellikleri.

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) atandığı [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) her görüntülenecek özelliği `MasterPageItem`. `DataTemplate` İçeren bir [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) , oluşur bir [ `Image` ](xref:Xamarin.Forms.Image) ve [ `Label` ](xref:Xamarin.Forms.Label). [ `Image` ](xref:Xamarin.Forms.Image) Görüntüler `IconSource` özellik değeri ve [ `Label` ](xref:Xamarin.Forms.Label) görüntüler `Title` her özellik değerini `MasterPageItem`.

Sayfanın alt [ `Title` ](xref:Xamarin.Forms.Page.Title) ve [ `Icon` ](xref:Xamarin.Forms.Page.Icon) özellikler kümesi. Başlık çubuğu ayrıntı sayfası sahip olması koşuluyla simgesi ayrıntı sayfasında görünür. İos'ta bu etkinleştirilmelidir ayrıntı sayfası örneğinde sarmalama tarafından bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) örneği.

> [!NOTE]
> [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) Sayfası olması gerekir, [ `Title` ](xref:Xamarin.Forms.Page.Title) özelliğini ayarlayın ya da bir özel durum oluşur.

Aşağıdaki kod örneği, oluşturulan C# ' de eşdeğer sayfaya gösterir:

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

Aşağıdaki ekran görüntüleri her platformda ana sayfaya göster:

![](master-detail-page-images/masterpage.png "Ana sayfası örneği")

### <a name="creating-and-displaying-the-detail-page"></a>Oluşturma ve ayrıntı sayfası görüntüleme

`MasterPage` Örneği içeren bir `ListView` kullanıma sunduğu özellik kendi [ `ListView` ](xref:Xamarin.Forms.ListView) örneği böylece `MainPage` [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) örneği kaydedebilir bir İşlenecek Olay işleyicisi [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) olay. Böylece `MainPage` ayarlamak için örnek [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) özelliği seçili temsil eden sayfasına `ListView` öğesi. Aşağıdaki kod örneği, olay işleyicisi gösterir:

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected` Yöntemi aşağıdaki eylemleri gerçekleştirir:

- Alır [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) gelen [ `ListView` ](xref:Xamarin.Forms.ListView) örneği ve etkinleştirilmediğini sağlanan `null`, ayrıntı sayfası içindedepolanansayfatürüyenibirörneğiniayarlar`TargetType`özelliğini `MasterPageItem`. Sayfa türü içinde sarmalanmış bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) aracılığıyla başvurulan simgesi emin olmak için örnek [ `Icon` ](xref:Xamarin.Forms.Page.Icon) özelliği `MasterPage` iOS ayrıntı sayfalarında gösterilir.
- Seçili öğenin [ `ListView` ](xref:Xamarin.Forms.ListView) ayarlanır `null` hiçbiri emin olmak için `ListView` öğeleri, sonraki açışınızda seçilir `MasterPage` sunulur.
- Ayrıntı Sayfası ayarlayarak kullanıcıya sunulan [ `MasterDetailPage.IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) özelliğini `false`. Bu özellik, ana veya ayrıntı sayfasında sunulan olup olmadığını denetler. Kümesine `true` ana sayfasını görüntüleyin ve `false` ayrıntı sayfasında görüntülenecek.

Aşağıdaki ekran görüntüleri Göster `ContactPage` ana sayfada seçilen sonra gösterilen Ayrıntı Sayfası:

![](master-detail-page-images/detailpage.png "Ayrıntı Sayfası örneği")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Ayrıntı sayfasında görünen davranışını denetleme

Nasıl [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) ana ve ayrıntı sayfaları yönetir olup uygulamasını çalıştıran telefon veya tablet, cihaz yönünü ve değerini üzerinde bağlıdır [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) özellik. Bu özellik, ayrıntı sayfası nasıl görüntüleneceğini belirler. Onun olası değerler şunlardır:

- **Varsayılan** – sayfaları platform varsayılan kullanılarak görüntülenir.
- **Popover** – ayrıntı sayfası kapsayan veya kısmen ana sayfaya kapsar.
- **Bölünmüş** : sol taraftaki ana sayfa görüntülenir ve sağ tarafta ayrıntıları sayfasıdır.
- **SplitOnLandscape** – cihaz yatay yönde olduğunda bölünmüş ekran kullanılır.
- **SplitOnPortrait** – cihaz dikey yöndeyken olduğunda bölünmüş ekran kullanılır.

Aşağıdaki XAML kod örneğinde nasıl ayarlanacağı gösterilmektedir [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) özelliği bir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

Aşağıdaki kod örneği, eşdeğer gösterir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) C# içinde oluşturulan:

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

Ancak, değerini [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) özelliği yalnızca satış noktası tabletleri veya masaüstünde çalışan uygulamaları etkiler. Her zaman telefonlarında çalışan uygulamalar *Popover* davranışı.

## <a name="summary"></a>Özet

Bu makalede nasıl kullanılacağını gösteren bir [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) ve kendi bilgi sayfaları arasında gezinebilirsiniz. Xamarin.Forms `MasterDetailPage` iki sayfa ilgili bilgilerin – öğeleri sunan bir ana sayfa ve ana sayfada öğe ayrıntılarını sunan bir ayrıntı sayfası yöneten bir sayfadır.


## <a name="related-links"></a>İlgili bağlantılar

- [Sayfa çeşitleri](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
