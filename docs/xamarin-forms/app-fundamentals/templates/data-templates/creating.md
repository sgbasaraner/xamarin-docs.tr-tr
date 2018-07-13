---
title: Bir Xamarin.Forms DataTemplate'ı oluşturma
description: Veri şablonları ResourceDictionary veya özel bir tür ya da uygun Xamarin.Forms hücresi türü satır içi olarak oluşturulabilir. Bu makalede her yöntem incelenir.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 63f9bf82bc8e637aced1afa5d5699ac1e8dc3f8c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994621"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Bir Xamarin.Forms DataTemplate'ı oluşturma

_Veri şablonları ResourceDictionary veya özel bir tür ya da uygun Xamarin.Forms hücresi türü satır içi olarak oluşturulabilir. Bu makalede her yöntem incelenir._

Genel bir kullanım senaryosu bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) nesnelerin bir koleksiyondaki verileri görüntüleyen bir [ `ListView` ](xref:Xamarin.Forms.ListView). Her hücreye veri görünümünü [ `ListView` ](xref:Xamarin.Forms.ListView) ayarlanarak yönetilebilir [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) özelliğini bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Bunu gerçekleştirmek için kullanılır teknikler vardır:

- [Satır içi DataTemplate oluşturma](#inline).
- [DataTemplate ile bir tür oluşturarak](#type).
- [Bir kaynak olarak bir DataTemplate'ı oluşturma](#resource).

Kullanılan yöntem ne olursa olsun, her bir hücresinde görünümünü sonucudur [ `ListView` ](xref:Xamarin.Forms.ListView) tarafından tanımlanan bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](creating-images/data-template-appearance.png "ListView DataTemplate ile")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Satır içi DataTemplate oluşturma

[ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) Özelliği için bir satır içi ayarlanabilir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Veri şablonu başka bir yerde yeniden gerek varsa uygun denetimi özelliği doğrudan alt yerleştirilen biri olan bir satır içi şablonu kullanılmalıdır. Belirtilen öğelerin `DataTemplate` her hücre görünümü aşağıdaki XAML kod örneğinde gösterildiği gibi tanımlayın:

```xaml
<ListView Margin="0,20,0,0">
    <ListView.ItemsSource>
        <x:Array Type="{x:Type local:Person}">
            <local:Person Name="Steve" Age="21" Location="USA" />
            <local:Person Name="John" Age="37" Location="USA" />
            <local:Person Name="Tom" Age="42" Location="UK" />
            <local:Person Name="Lucas" Age="29" Location="Germany" />
            <local:Person Name="Tariq" Age="39" Location="UK" />
            <local:Person Name="Jane" Age="30" Location="USA" />
        </x:Array>
    </ListView.ItemsSource>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Label Text="{Binding Name}" FontAttributes="Bold" />
                    <Label Grid.Column="1" Text="{Binding Age}" />
                    <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Bir satır içi alt [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) gerekir, olması veya öğesinden türetilen, tip [ `ViewCell` ](xref:Xamarin.Forms.ViewCell). Düzen içinde `ViewCell` burada tarafından yönetilen bir [ `Grid` ](xref:Xamarin.Forms.Grid). `Grid` Üçünü içeren [ `Label` ](xref:Xamarin.Forms.Label) örnekler, BIND kendi [ `Text` ](xref:Xamarin.Forms.Label.Text) her uygun özelliklerini özelliklerine `Person` koleksiyondaki nesne.

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
    public WithDataTemplatePageCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        var personDataTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            ...
            var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
            var ageLabel = new Label();
            var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

            nameLabel.SetBinding(Label.TextProperty, "Name");
            ageLabel.SetBinding(Label.TextProperty, "Age");
            locationLabel.SetBinding(Label.TextProperty, "Location");

            grid.Children.Add(nameLabel);
            grid.Children.Add(ageLabel, 1, 0);
            grid.Children.Add(locationLabel, 2, 0);

            return new ViewCell { View = grid };
        });

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemsSource = people, ItemTemplate = personDataTemplate, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

C#, satır içi [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) belirten bir yapıcı yeniden yüklemesi kullanarak oluşturulan bir `Func` bağımsız değişken.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>DataTemplate ile bir tür oluşturma

[ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) Özelliği de ayarlanabilir bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) hücre türünden oluşturulur. Bu yaklaşımın avantajı, uygulama boyunca birden çok veri şablonları tarafından hücresi türü tarafından tanımlanan görünüm yeniden kullanılabilen olmasıdır. Bu yaklaşımın bir örneği olarak aşağıdaki XAML kodu gösterir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <local:PersonCell />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Burada, [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) özelliği bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) hücre görünümü tanımlayan özel bir türden oluşturulur. Özel tür türünden türetilmesi gerekir [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ViewCell xmlns="http://xamarin.com/schemas/2014/forms"
          xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
          x:Class="DataTemplates.PersonCell">
     <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.2*" />
            <ColumnDefinition Width="0.3*" />
        </Grid.ColumnDefinitions>
        <Label Text="{Binding Name}" FontAttributes="Bold" />
        <Label Grid.Column="1" Text="{Binding Age}" />
        <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
    </Grid>
</ViewCell>
```

İçinde [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), Düzen yönetilir burada bir [ `Grid` ](xref:Xamarin.Forms.Grid). `Grid` Üçünü içeren [ `Label` ](xref:Xamarin.Forms.Label) örnekler, BIND kendi [ `Text` ](xref:Xamarin.Forms.Label.Text) her uygun özelliklerini özelliklerine `Person` koleksiyondaki nesne.

Eşdeğer C# kodu, aşağıdaki örnekte gösterilmiştir:

```csharp
public class WithDataTemplatePageFromTypeCS : ContentPage
{
    public WithDataTemplatePageFromTypeCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemTemplate = new DataTemplate(typeof(PersonCellCS)), ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

C# ' ta, [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) hücresi türü bağımsız değişken olarak belirten bir yapıcı yeniden yüklemesi kullanılarak oluşturulur. Hücresi türü türünden türetilmesi gerekir [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public class PersonCellCS : ViewCell
{
    public PersonCellCS()
    {
        var grid = new Grid();
        ...
        var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        var ageLabel = new Label();
        var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

        nameLabel.SetBinding(Label.TextProperty, "Name");
        ageLabel.SetBinding(Label.TextProperty, "Age");
        locationLabel.SetBinding(Label.TextProperty, "Location");

        grid.Children.Add(nameLabel);
        grid.Children.Add(ageLabel, 1, 0);
        grid.Children.Add(locationLabel, 2, 0);

        View = grid;
    }
}
```

> [!NOTE]
> Xamarin.Forms basit verileri görüntülemek için kullanılan hücresi türlerini içerdiğine dikkat edin [ `ListView` ](xref:Xamarin.Forms.ListView) hücreleri. Daha fazla bilgi için [hücre görünümü](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Bir kaynak olarak bir DataTemplate'ı oluşturma

Veri şablonları da oluşturulabilir yeniden kullanılabilir nesneleri olarak bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Bu bildirimi her bir benzersiz sağlayarak gerçekleştirilir `x:Key` açıklayıcı bir anahtarla sağlayan öznitelik `ResourceDictionary`aşağıdaki XAML kod örneğinde gösterildiği gibi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="personTemplate">
                 <ViewCell>
                    <Grid>
                        ...
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        ...
        <ListView ItemTemplate="{StaticResource personTemplate}" Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Atandığı [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) kullanarak özellik `StaticResource` işaretleme uzantısı. Unutmayın, while `DataTemplate` sayfanın içinde tanımlanan [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), denetim düzeyi veya uygulama düzeyinde de tanımlanabilir.

Eşdeğer sayfaya aşağıdaki kod örneği C# dilinde gösterir:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
  public WithDataTemplatePageCS ()
  {
    ...
    var personDataTemplate = new DataTemplate (() => {
      var grid = new Grid ();
      ...
      return new ViewCell { View = grid };
    });

    Resources = new ResourceDictionary ();
    Resources.Add ("personTemplate", personDataTemplate);

    Content = new StackLayout {
      Margin = new Thickness(20),
      Children = {
        ...
        new ListView { ItemTemplate = (DataTemplate)Resources ["personTemplate"], ItemsSource = people };
      }
    };
  }
}
```

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Eklenir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) kullanarak [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) belirten yöntemi bir `Key` için kullanılan dize başvuru `DataTemplate` bunu alınırken.

## <a name="summary"></a>Özet

Veri şablonları, satır içi olarak veya özel bir türden oluşturmak nasıl bu makalede açıklanan bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Veri şablonu başka bir yerde yeniden gerek yoksa, bir satır içi şablonu kullanılmalıdır. Alternatif olarak, bir veri şablonu bir denetim düzeyi, sayfa düzeyi veya uygulama düzeyinde kaynak veya özel bir tür olarak tanımlayarak yeniden kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Hücre Görünümü](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Veri şablonları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
