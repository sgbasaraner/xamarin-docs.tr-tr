---
title: Xamarin.Forms DataTemplate oluşturma
description: Veri şablonları ResourceDictionary veya bir özel tür veya uygun Xamarin.Forms hücre türü satır içi de oluşturulabilir. Bu makalede her teknik araştırır.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 8aa0ad693fd1a7f086492f93f18c1e33871dee0e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240518"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Xamarin.Forms DataTemplate oluşturma

_Veri şablonları ResourceDictionary veya bir özel tür veya uygun Xamarin.Forms hücre türü satır içi de oluşturulabilir. Bu makalede her teknik araştırır._

Genel bir kullanım senaryosu bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) nesneleri koleksiyonu verileri görüntüleyen bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Her hücre için verilerin görünümünü [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ayarlanarak yönetilebilir [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) özelliğine bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Bunu gerçekleştirmek için kullanılan teknikleri vardır:

- [Satır içi DataTemplate oluşturma](#inline).
- [Bir türü ile bir DataTemplate oluşturma](#type).
- [Bir kaynak olarak bir DataTemplate oluşturma](#resource).

Kullanılan bağımsız olarak, sonuç, her hücre görünümünü tekniktir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) tarafından tanımlanan bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](creating-images/data-template-appearance.png "ListView DataTemplate ile")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Satır içi DataTemplate oluşturma

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Özelliği için bir satır içi ayarlanabilir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Başka bir yerde veri şablonu yeniden gerekmiyorsa uygun denetim özelliği doğrudan alt yerleştirilen biridir, bir satır içi şablon kullanılmalıdır. Belirtilen öğe `DataTemplate` her hücre görünümünü aşağıdaki XAML kod örneğinde gösterildiği gibi tanımlayın:

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

Satır içi alt [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) gerekir, olması veya öğesinden türetilen, yazın [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/). Düzen içinde `ViewCell` burada tarafından yönetilen bir [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). `Grid` Üç içeren [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekler, BIND kendi [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) her uygun özelliklerini özelliklerine `Person` koleksiyon nesnesinde.

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

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

C#, satır içi [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) belirten bir oluşturucu aşırı kullanılarak oluşturulan bir `Func` bağımsız değişkeni.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Bir DataTemplate türü ile oluşturma

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Özelliği de ayarlanabilir bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) bir hücre türünden oluşturulur. Bu yaklaşımın avantajı, hücre türü tarafından tanımlanan görünüm uygulama boyunca birden çok veri şablonları tarafından kullanılabilme ' dir. Aşağıdaki XAML kodu, bu yaklaşımın bir örnektir:

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

Burada, [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) özelliği ayarlanmış bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) hücre görünümünü tanımlayan bir özel tür oluşturulur. Özel tür türünden türetilmelidir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), aşağıdaki kod örneğinde gösterildiği gibi:

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

İçinde [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), Düzen yönetilir burada bir [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). `Grid` Üç içeren [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekler, BIND kendi [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) her uygun özelliklerini özelliklerine `Person` koleksiyon nesnesinde.

Eşdeğer C# kod, aşağıdaki örnekte gösterilmiştir:

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

C# ' ta, [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) bağımsız değişken olarak hücre türünü belirten bir oluşturucu aşırı kullanılarak oluşturulur. Hücre türü türünden türetilmelidir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), aşağıdaki kod örneğinde gösterildiği gibi:

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
> Xamarin.Forms de basit verileri görüntülemek için kullanılan hücre türleri içerdiğine dikkat [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) hücreleri. Daha fazla bilgi için bkz: [hücre Görünüm](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Bir kaynak olarak bir DataTemplate oluşturma

Veri şablonları da oluşturulabilir yeniden kullanılabilir nesneler olarak bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Her bildirim benzersiz bir vererek elde edilen `x:Key` açıklayıcı bir anahtarla sağlar özniteliği `ResourceDictionary`aşağıdaki XAML kod örneğinde gösterildiği gibi:

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

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Atandığı [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) kullanarak özellik `StaticResource` biçimlendirme uzantısı. Unutmayın, while `DataTemplate` sayfanın içinde tanımlanan [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), Denetim düzeyinde veya uygulama düzeyinde de tanımlanabilir.

Aşağıdaki kod örneğinde eşdeğer sayfaya C# dilinde gösterir:

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

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Eklenen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) kullanarak [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) belirtir yöntemi bir `Key` için kullanılan dize başvuru `DataTemplate` onu alınırken.

## <a name="summary"></a>Özet

Bu makalede veri şablonlarının, satır içi, özel bir tür veya içinde nasıl oluşturulacağı açıklanmıştır bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Başka bir yerde veri şablonu yeniden gerekmiyorsa bir satır içi şablon kullanılmalıdır. Alternatif olarak, veri şablonu özel bir tür olarak ya da denetim düzeyi sayfa düzeyinde veya uygulama düzeyinde kaynak olarak tanımlayarak yeniden kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Hücre Görünümü](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Veri şablonları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
