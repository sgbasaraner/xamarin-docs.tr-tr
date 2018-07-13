---
title: Xamarin.Forms veri şablonları giriş
description: Xamarin.Forms veri şablonları desteklenen denetimlere verilerini sunumu tanımlama yeteneği sağlar. Bu makalede konakadı neden İnceleme veri şablonları, bir giriş sağlar.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 129ce7a04b93bfb3cb1b9a1639aee61cd56d09d5
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998927"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Xamarin.Forms veri şablonları giriş

_Xamarin.Forms veri şablonları desteklenen denetimlere verilerini sunumu tanımlama yeteneği sağlar. Bu makalede konakadı neden İnceleme veri şablonları, bir giriş sağlar._

Göz önünde bir [ `ListView` ](xref:Xamarin.Forms.ListView) koleksiyonunu görüntüleyen `Person` nesneleri. Aşağıdaki kod örneği tanımı gösterilmektedir `Person` sınıfı:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` Sınıfı tanımlar `Name`, `Age`, ve `Location` olduğunda ayarlanabilir özellikleri bir `Person` nesnesi oluşturulur. [ `ListView` ](xref:Xamarin.Forms.ListView) Koleksiyonunu görüntülemek için kullanılan `Person` aşağıdaki XAML kod örneğinde gösterildiği gibi nesneler:

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
                    <local:Person Name="John" Age="37" Location="USA" />
                    <local:Person Name="Tom" Age="42" Location="UK" />
                    <local:Person Name="Lucas" Age="29" Location="Germany" />
                    <local:Person Name="Tariq" Age="39" Location="UK" />
                    <local:Person Name="Jane" Age="30" Location="USA" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Öğeleri eklenir [ `ListView` ](xref:Xamarin.Forms.ListView) başlatma tarafından XAML içinde [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) bir dizi özellikten `Person` örnekleri.

> [!NOTE]
> Unutmayın `x:Array` öğesi gerektiriyor bir `Type` dizideki öğelerin türünü belirten özniteliği.

Eşdeğer C# sayfaya başlatır aşağıdaki kod örneğinde gösterilen [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) özelliğini bir `List` , `Person` örnekleri:

```csharp
public WithoutDataTemplatePageCS()
{
    ...
    var people = new List<Person>
    {
        new Person { Name = "Steve", Age = 21, Location = "USA" },
        new Person { Name = "John", Age = 37, Location = "USA" },
        new Person { Name = "Tom", Age = 42, Location = "UK" },
        new Person { Name = "Lucas", Age = 29, Location = "Germany" },
        new Person { Name = "Tariq", Age = 39, Location = "UK" },
        new Person { Name = "Jane", Age = 30, Location = "USA" }
    };

    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children = {
            ...
            new ListView { ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
        }
    };
}
```

[ `ListView` ](xref:Xamarin.Forms.ListView) Çağrıları `ToString` nesneleri koleksiyonda görüntülenirken. Olmadığından hiçbir `Person.ToString` geçersiz kılmak, `ToString` aşağıdaki ekran görüntülerinde gösterildiği gibi her bir nesnenin türü adı döndürür:

![](introduction-images/no-data-template.png "ListView olmadan veri şablonu")

`Person` Nesnesi geçersiz kılma `ToString` yöntemini aşağıdaki kod örneğinde gösterildiği gibi anlamlı veri görüntülemek için:

```csharp
public class Person
{
  ...
  public override string ToString ()
  {
    return Name;
  }
}
```

Sonuçlanır [ `ListView` ](xref:Xamarin.Forms.ListView) görüntüleme `Person.Name` aşağıdaki ekran görüntülerinde gösterildiği gibi koleksiyondaki her nesne için özellik değeri:

![](introduction-images/override-tostring.png "ListView veri şablonu ile")

`Person.ToString` Geçersiz kılma oluşan bir biçimlendirilmiş dize döndürebilir `Name`, `Age`, ve `Location` özellikleri. Ancak, bu yaklaşım yalnızca sınırlı bir denetime her bir veri öğesi görünümünü sunar. Daha fazla esneklik için bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) oluşturulabilir, verilerin görünümünü tanımlar.

## <a name="creating-a-datatemplate"></a>Bir DataTemplate'ı oluşturma

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) veri görünümünü belirtmek için kullanılır ve genellikle verileri görüntülemek için veri bağlama kullanır. Nesnelerin bir koleksiyondaki verileri görüntülerken, ortak kullanım senaryosuna olduğu bir [ `ListView` ](xref:Xamarin.Forms.ListView). Örneğin, bir `ListView` koleksiyonuna bağlı `Person` nesneleri `ListView.ItemTemplate` özellik ayarlanacak bir `DataTemplate` her görünümünü tanımlayan `Person` nesnesine `ListView`. `DataTemplate` Her özellik değerlerine bağlama öğeleri içerecek `Person` nesne. Veri bağlama hakkında daha fazla bilgi için bkz. [temel veri bağlama bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) bir değer olarak, aşağıdakiler için kullanılabilir:

- [`ListView.HeaderTemplate`](xref:Xamarin.Forms.ListView.HeaderTemplate)
- [`ListView.FooterTemplate`](xref:Xamarin.Forms.ListView.FooterTemplate)
- [`ListView.GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)
- [`ItemsView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1), tarafından devralınan [ `ListView` ](xref:Xamarin.Forms.ListView).
- [`MultiPage.ItemTemplate`](xref:Xamarin.Forms.MultiPage`1), tarafından devralınan [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), ve [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage).

> [!NOTE]
> Olsa da unutmayın [ `TableView` ](xref:Xamarin.Forms.TableView) kullanımları yapar [ `Cell` ](xref:Xamarin.Forms.Cell) nesneleri, kullanmayan bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Veri bağlamaları doğrudan her zaman ayarlanan olmasıdır `Cell` nesneleri.

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) yukarıda listelenen özellik öğesinin doğrudan alt öğesi olarak bilinen yerleştirilen bir *satır içi şablon*. Alternatif olarak, bir `DataTemplate` denetim düzeyi, sayfa düzeyinde veya uygulama düzeyinde bir kaynak olarak tanımlanabilir. Tanımlama yeri seçme bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) burada kullanılabileceği etkiler:

- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) tanımlı denetim düzeyi yalnızca denetime uygulanabilir.
- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) tanımlanan sayfa düzeyi sayfasında birden fazla geçerli denetimleri uygulanabilir.
- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) uygulama düzeyinde tanımlanan uygulama boyunca uygulanan geçerli denetimler olabilir.

Veri şablonları görünümü hiyerarşide daha düşük önceliklidir tanımlanan bunlar paylaştığınızda daha yukarı `x:Key` öznitelikleri. Örneğin, bir uygulama düzeyinde veri şablonu bir sayfa düzeyinde veri şablonu tarafından geçersiz kılınır ve sayfa düzeyinde veri şablonu bir denetim düzeyi veri şablonu veya bir satır içi veri şablonu tarafından geçersiz kılınır.


## <a name="related-links"></a>İlgili bağlantılar

- [Hücre Görünümü](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Veri şablonları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
