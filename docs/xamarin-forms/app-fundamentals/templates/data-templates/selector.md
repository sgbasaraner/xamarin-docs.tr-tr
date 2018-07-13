---
title: Xamarin.Forms DataTemplateSelector oluşturma
description: Bu makalede, çalışma zamanında bir verilere bağlı özellik değerine göre bir DataTemplate'ı seçmek için kullanılan bir DataTemplateSelector oluşturup gösterilmektedir.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: a72777c7e51e96a8e123ecd85ad0aa24fc60fc6c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994523"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Xamarin.Forms DataTemplateSelector oluşturma

_Bir DataTemplateSelector verilere bağlı özellik değerine göre çalışma zamanında bir DataTemplate'ı seçmek için kullanılabilir. Bu, aynı belirli nesnelerin görünümünü özelleştirmek için nesne türüne uygulanacak birden çok DataTemplates sağlar. Bu makalede, bir DataTemplateSelector oluşturup gösterilmektedir._

Veri şablon seçiciyi gibi senaryolarına olanak sağlayan bir [ `ListView` ](xref:Xamarin.Forms.ListView) nesnelerden oluşan bir koleksiyon için bağlama burada her bir nesnenin görünümünü `ListView` çalışma zamanında döndüren veri şablon seçiciyi seçilebilir bir belirli [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="creating-a-datatemplateselector"></a>Bir DataTemplateSelector oluşturma

Devralınan bir sınıfı oluşturarak bir veri şablon seçiciyi uygulanan [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). `OnSelectTemplate` Yöntemi belirli bir döndürmek için geçersiz ardından [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public class PersonDataTemplateSelector : DataTemplateSelector
{
  public DataTemplate ValidTemplate { get; set; }
  public DataTemplate InvalidTemplate { get; set; }

  protected override DataTemplate OnSelectTemplate (object item, BindableObject container)
  {
    return ((Person)item).DateOfBirth.Year >= 1980 ? ValidTemplate : InvalidTemplate;
  }
}
```

`OnSelectTemplate` Yöntemi döndürür değerine göre uygun şablonu `DateOfBirth` özelliği. Şablon döndürülecek değeri `ValidTemplate` özelliği veya `InvalidTemplate` tüketildiğinde ayarlanan özellik `PersonDataTemplateSelector`.

Veri şablonu Seçici sınıfının bir örneğini sonra Xamarin.Forms Denetim özellikleri gibi atanabilir [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1). Geçerli özelliklerin bir listesi için bkz. [DataTemplate oluşturma](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Sınırlamalar

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) örnekleri aşağıdaki sınırlamalara sahiptir:

- `DataTemplateSelector` Alt her zaman döndürmelidir aynı verileri için aynı şablonu birden çok kez sorgularsa.
- `DataTemplateSelector` Öğesinin alt sınıfı değil döndürmelidir başka `DataTemplateSelector` öğesinin alt sınıfı.
- `DataTemplateSelector` Öğesinin alt sınıfı değil, yeni örneklerini döndürmelidir bir `DataTemplate` her çağrıda. Bunun yerine, aynı örneğini iade edilmesi gerekir. Bunun yapılmaması, bellek sızıntısı oluşturur ve sanallaştırma devre dışı bırakır.
- Android'de olabilir başına en fazla 20 farklı veri şablonları `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>XAML içinde bir DataTemplateSelector kullanma

XAML içinde `PersonDataTemplateSelector` bir kaynak olarak bildirerek aşağıdaki kod örneğinde gösterildiği gibi oluşturulabilir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Selector;assembly=Selector" x:Class="Selector.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="validPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <DataTemplate x:Key="invalidPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <local:PersonDataTemplateSelector x:Key="personDataTemplateSelector"
                ValidTemplate="{StaticResource validPersonTemplate}"
                InvalidTemplate="{StaticResource invalidPersonTemplate}" />
        </ResourceDictionary>
    </ContentPage.Resources>
  ...
</ContentPage>
```

Bu sayfa düzeyi [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) iki tanımlar [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) örnekleri ve `PersonDataTemplateSelector` örneği. `PersonDataTemplateSelector` Örnek kümeleri kendi `ValidTemplate` ve `InvalidTemplate` uygun özellikler `DataTemplate` kullanarak örnekleri `StaticResource` işaretleme uzantısı. Kaynakları çalışırken, sayfanın içinde tanımlanan Not [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), bunlar da denetim düzeyi veya uygulama düzeyinde tanımlanabilir.

`PersonDataTemplateSelector` Örneği atayarak o kullanılır [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) aşağıdaki kod örneğinde gösterildiği gibi özelliği:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Çalışma zamanında, [ `ListView` ](xref:Xamarin.Forms.ListView) çağrıları `PersonDataTemplateSelector.OnSelectTemplate` yöntemi olarak veri nesnesini geçirerek çağrısı ile temel alınan koleksiyondaki öğelerin her biri için `item` parametresi. [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Tarafından döndürülen yöntemi daha sonra bu nesneye uygulanır.

Aşağıdaki ekran görüntüleri sonucu göster [ `ListView` ](xref:Xamarin.Forms.ListView) uygulama `PersonDataTemplateSelector` altta yatan seçki her nesne için:

![](selector-images/data-template-selector.png "ListView veri şablon seçiciyi ile")

Tüm `Person` nesnesi bir `DateOfBirth` 1980 eşit veya büyük özellik değer kırmızı renkte görüntülenmesini kalan nesneleri ile yeşil renkte görüntülenir.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>C dilinde bir DataTemplateSelector kullanma&num;

C# ' ta, `PersonDataTemplateSelector` örneği ve atanan [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) aşağıdaki kod örneğinde gösterildiği gibi özelliği:

```csharp
public class HomePageCS : ContentPage
{
  DataTemplate validTemplate;
  DataTemplate invalidTemplate;

  public HomePageCS ()
  {
    ...
    SetupDataTemplates ();
    var listView = new ListView {
      ItemsSource = people,
      ItemTemplate = new PersonDataTemplateSelector {
        ValidTemplate = validTemplate,
        InvalidTemplate = invalidTemplate }
    };

    Content = new StackLayout {
      Margin = new Thickness (20),
      Children = {
        ...
        listView
      }
    };
  }
  ...  
}
```

`PersonDataTemplateSelector` Örnek kümeleri kendi `ValidTemplate` ve `InvalidTemplate` uygun özellikler [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) tarafından oluşturulan örnekleri `SetupDataTemplates` yöntemi. Çalışma zamanında, [ `ListView` ](xref:Xamarin.Forms.ListView) çağrıları `PersonDataTemplateSelector.OnSelectTemplate` yöntemi olarak veri nesnesini geçirerek çağrısı ile temel alınan koleksiyondaki öğelerin her biri için `item` parametresi. `DataTemplate` Tarafından döndürülen yöntemi daha sonra bu nesneye uygulanır.

## <a name="summary"></a>Özet

Bu makalede, oluşturma ve kullanma işlemleri gösterilmiştir bir [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). A `DataTemplateSelector` seçmek için kullanılan bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) verilere bağlı özellik değerine göre çalışma zamanında. Birden çok böylece `DataTemplate` örnekleri aynı belirli nesnelerin görünümünü özelleştirmek için nesne türüne uygulanacak.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri şablon seçiciyi (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
