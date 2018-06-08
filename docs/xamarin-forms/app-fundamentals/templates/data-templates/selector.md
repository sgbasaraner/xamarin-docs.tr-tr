---
title: Bir DataTemplateSelector oluşturma
description: Bir DataTemplateSelector veri bağlama özelliğinin değeri temel alınarak çalışma zamanında bir DataTemplate seçmek için kullanılabilir. Bu, aynı türdeki bir belirli nesnelerin görünümünü özelleştirmek için nesne için uygulanan birden çok DataTemplates sağlar. Bu makalede nasıl oluşturulacağı ve bir DataTemplateSelector tüketen gösterilmektedir.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: ad8cce32cb9cfe2ea5e78f11b1250440371a9851
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847198"
---
# <a name="creating-a-datatemplateselector"></a>Bir DataTemplateSelector oluşturma

_Bir DataTemplateSelector veri bağlama özelliğinin değeri temel alınarak çalışma zamanında bir DataTemplate seçmek için kullanılabilir. Bu, aynı türdeki bir belirli nesnelerin görünümünü özelleştirmek için nesne için uygulanan birden çok DataTemplates sağlar. Bu makalede nasıl oluşturulacağı ve bir DataTemplateSelector tüketen gösterilmektedir._

Bir veri şablonu Seçici gibi senaryolara olanak sağlar bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) nesneleri, bir koleksiyona bağlama burada her nesne görünümünü `ListView` çalışma zamanında döndürme veri şablonu Seçici seçilebilir bir belirli [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/).

## <a name="creating-a-datatemplateselector"></a>Bir DataTemplateSelector oluşturma

Öğesinden devralınan bir sınıf oluşturarak veri şablonu Seçici uygulanan [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). `OnSelectTemplate` Yöntem belirli bir döndürmek için geçersiz sonra [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), aşağıdaki kod örneğinde gösterildiği gibi:

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

`OnSelectTemplate` Yöntemi döndürür değerine göre uygun şablon `DateOfBirth` özelliği. Döndürülecek şablonu değeri `ValidTemplate` özelliği veya `InvalidTemplate` kullanırken ayarlama özelliği `PersonDataTemplateSelector`.

Veri şablonu Seçici sınıfının bir örneği sonra Xamarin.Forms Denetim özellikleri gibi atanabilir [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/). Geçerli özelliklerinin listesi için bkz: [DataTemplate oluşturma](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Sınırlamalar

[`DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) örnekleri aşağıdaki sınırlamalara sahiptir:

- `DataTemplateSelector` Alt her zaman döndürmelidir aynı verileri için aynı şablonu birden çok kez sorgularsa.
- `DataTemplateSelector` Alt sınıfı değil döndürmelidir başka `DataTemplateSelector` bir alt kümesi.
- `DataTemplateSelector` Alt sınıfı değil yeni örneklerini döndürmelidir bir `DataTemplate` her çağrıda. Bunun yerine, aynı örneği döndürdü gerekir. Bunun Sağlanamaması bellek sızıntısı oluşturacak ve sanallaştırma devre dışı bırakır.
- Android, olabilir başına en fazla 20 farklı veri şablonları `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>XAML'de DataTemplateSelector kullanma

XAML'de `PersonDataTemplateSelector` bir kaynak olarak bildirerek aşağıdaki kod örneğinde gösterildiği gibi oluşturulabilir:

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

Bu sayfa düzeyi [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) iki tanımlar [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) örnekleri ve `PersonDataTemplateSelector` örneği. `PersonDataTemplateSelector` Örneği kümeleri kendi `ValidTemplate` ve `InvalidTemplate` uygun özellikleri `DataTemplate` kullanarak örnekleri `StaticResource` biçimlendirme uzantısı. Kaynakları sırasında sayfa içinde tanımlanan unutmayın [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), bunlar da denetim düzeyi veya uygulama düzeyinde tanımlanabilir.

`PersonDataTemplateSelector` Örneği tüketilen kendisine atayarak [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) özelliği, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Çalışma zamanında [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) çağrıları `PersonDataTemplateSelector.OnSelectTemplate` yöntemi her bir veri nesnesi olarak geçirme çağrısı ile temel alınan koleksiyondaki öğe için `item` parametresi. [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Tarafından döndürülen yöntemi sonra bu nesneye uygulanır.

Aşağıdaki ekran görüntüleri sonucu göster [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) uygulama `PersonDataTemplateSelector` temel koleksiyon her nesne için:

![](selector-images/data-template-selector.png "ListView veri şablonu Seçici ile")

Tüm `Person` olan nesneyi bir `DateOfBirth` özellik değeri sıfırdan büyük veya eşit 1980 kırmızı olarak görüntülenmesini kalan nesneler yeşil renkte görüntülenir.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>C DataTemplateSelector kullanma&num;

C# ' ta, `PersonDataTemplateSelector` örneği ve atanan [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) özelliği, aşağıdaki kod örneğinde gösterildiği gibi:

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

`PersonDataTemplateSelector` Örneği kümeleri kendi `ValidTemplate` ve `InvalidTemplate` uygun özellikleri [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) tarafından oluşturulan örnekleri `SetupDataTemplates` yöntemi. Çalışma zamanında [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) çağrıları `PersonDataTemplateSelector.OnSelectTemplate` yöntemi her bir veri nesnesi olarak geçirme çağrısı ile temel alınan koleksiyondaki öğe için `item` parametresi. `DataTemplate` Tarafından döndürülen yöntemi sonra bu nesneye uygulanır.

## <a name="summary"></a>Özet

Bu makalede, oluşturma ve kullanma gösterilmiştir bir [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). A `DataTemplateSelector` seçmek için kullanılan bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) veri bağlama özelliğinin değeri temel alınarak çalışma zamanında. Bu, birden çok sağlar `DataTemplate` nesnesinin belirli nesnelerin görünümünü özelleştirmek için aynı türü için uygulanacak örnekleri.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri şablonu Seçici (örnek)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)
