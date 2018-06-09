---
title: Xamarin.Forms veri şablonları giriş
description: Xamarin.Forms veri şablonları, desteklenen denetimlere veri sunumu tanımlama yeteneği sağlar. Bu makalede veri şablonları, gerekli neden inceleniyor tanıtılmaktadır.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: e54c7d3ea01c59a20561b69c6e790747567d92f0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240183"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Xamarin.Forms veri şablonları giriş

_Xamarin.Forms veri şablonları, desteklenen denetimlere veri sunumu tanımlama yeteneği sağlar. Bu makalede veri şablonları, gerekli neden inceleniyor tanıtılmaktadır._

Göz önünde bulundurun bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) koleksiyonunu görüntüleyen `Person` nesneleri. Aşağıdaki kod örneğinde tanımını gösterir `Person` sınıfı:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` Sınıfı tanımlayan `Name`, `Age`, ve `Location` olduğunda ayarlanabilir özellikleri bir `Person` nesnesi oluşturulur. [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Koleksiyonunu görüntülemek için kullanılan `Person` aşağıdaki XAML kod örneğinde gösterildiği gibi nesneler:

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

Öğeleri eklenir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) başlatma tarafından XAML'de [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) bir dizi özelliğinden `Person` örnekleri.

> [!NOTE]
> Unutmayın `x:Array` öğesi gerektiriyor bir `Type` dizisindeki öğelerin türünü belirten özniteliği.

Eşdeğer C# sayfaya başlatır aşağıdaki kod örneğinde gösterildiği [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) özelliğine bir `List` , `Person` örnekleri:

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

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Çağrıları `ToString` nesneleri koleksiyonda görüntülenirken. Olduğundan hiçbir `Person.ToString` geçersiz kılmak, `ToString` aşağıdaki ekran görüntülerinde gösterildiği gibi her nesne türü adını döndürür:

![](introduction-images/no-data-template.png "ListView veri şablonu olmadan")

`Person` Nesnesi geçersiz kılabilirsiniz `ToString` yöntemi aşağıdaki kod örneğinde gösterildiği gibi anlamlı verileri görüntülemek için:

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

Bu, sonuçlanır [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) görüntüleme `Person.Name` aşağıdaki ekran görüntülerinde gösterildiği gibi koleksiyondaki her nesne için özellik değeri:

![](introduction-images/override-tostring.png "ListView veri şablonu ile")

`Person.ToString` Geçersiz kılma oluşan biçimlendirilmiş bir dize döndürme olasılığından `Name`, `Age`, ve `Location` özellikleri. Ancak, bu yaklaşım sınırlı denetime her bir veri öğesi görünümünü sunar. Daha fazla esneklik için bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) oluşturulabilir, verilerin görünümünü tanımlar.

## <a name="creating-a-datatemplate"></a>Bir DataTemplate oluşturma

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) verilerin görünümünü belirtmek için kullanılır ve genellikle verileri görüntülemek için veri bağlama kullanır. Nesneleri topluluğu verileri görüntülerken, kendi ortak kullanım senaryosu olduğu bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Örneğin, bir `ListView` bir koleksiyona bağlı `Person` nesneleri `ListView.ItemTemplate` özellik ayarlanacak bir `DataTemplate` her görünümünü tanımlayan `Person` nesnesinde `ListView`. `DataTemplate` Her özellik değerlerine bağlama öğeleri içerecek olan `Person` nesnesi. Veri bağlama hakkında daha fazla bilgi için bkz: [veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) bir değer olarak aşağıdaki özellikler için kullanılabilir:

- [`ListView.HeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HeaderTemplate/)
- [`ListView.FooterTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.FooterTemplate/)
- [`ListView.GroupHeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)
- [`ItemsView.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/), tarafından devralınan [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).
- [`MultiPage.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), tarafından devralınan [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), ve [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/).

> [!NOTE]
> Rağmen unutmayın [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) kullanımlarını yapar [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) nesneleri, onu kullanmayan bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Veri bağlamaları her zaman doğrudan ayarlanan olmasıdır `Cell` nesneleri.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) yukarıda listelenen özelliklerinin doğrudan alt öğesi olarak bilinen yerleştirilen bir *satır içi şablon*. Alternatif olarak, bir `DataTemplate` denetim düzeyi, sayfa düzeyinde veya uygulama düzeyinde bir kaynak olarak tanımlanabilir. Nerede tanımlamalı seçerek bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) nerede kullanılabilir etkiler:

- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) tanımlanan denetim düzeyi yalnızca denetime uygulanabilir.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) tanımlanan sayfanın düzeyi sayfasında birden fazla geçerli denetimleri uygulanabilir.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) uygulama düzeyinde tanımlanan uygulama boyunca uygulanan geçerli denetimler olabilir.

Görünüm hiyerarşisinde daha düşük veri şablonları önceliklidir tanımlananlar paylaştıkları zaman daha yüksek yukarı `x:Key` öznitelikleri. Örneğin, bir uygulama düzeyi veri şablonu sayfa düzeyinde veri şablon tarafından geçersiz kılınır ve sayfa düzeyinde veri şablonu denetim düzeyi veri şablonu veya bir satır içi veri şablonu tarafından geçersiz kılınır.


## <a name="related-links"></a>İlgili bağlantılar

- [Hücre Görünümü](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Veri şablonları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
