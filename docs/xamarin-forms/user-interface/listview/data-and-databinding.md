---
title: ListView veri kaynakları
description: Bu makalede, bir Xamarin.Forms ListView verilerle doldurmak nasıl ve veri bağlama ile bir ListView kullanma açıklanmaktadır.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 17c353844a7ddc808e5d9f0632434472913170a4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995212"
---
# <a name="listview-data-sources"></a>ListView veri kaynakları

ListView veri listesini görüntülemek için kullanılır. Bir ListView veri ve nasıl biz seçili öğeye bağlayabilirsiniz doldurma hakkında bilgi edineceksiniz.

- **[ItemsSource ayarlama](#ItemsSource)**  &ndash; basit bir liste veya dizi kullanır.
- **[Veri bağlama](#Data_Binding)**  &ndash; ListView ve bir modeli arasında bir ilişki kurar. Bağlama MVVM desen için idealdir.

## <a name="itemssource"></a>ItemsSource
ListView, verileriniz ile doldurulur `ItemsSource` koleksiyon uygulama kabul edebilir özelliği `IEnumerable`. Doldurmak için en kolay yolu bir `ListView` dizelerden oluşan bir dizi kullanmayı içerir:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};

//monochrome will not appear in the list because it was added
//after the list was populated.
listView.ItemsSource.Add("monochrome");
```

![](data-and-databinding-images/itemssource-simple.png "ListView görüntüleme dize listesi")

Yukarıdaki yaklaşım doldurur `ListView` içeren bir dize listesi. Varsayılan olarak, `ListView` çağıracak `ToString` ve sonucu görüntüler bir `TextCell` her satır için. Verilerin nasıl görüntüleneceğini özelleştirmek için bkz: [hücre görünümü](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Çünkü `ItemsSource` gönderilmiştir diziye bir içerik, temel alınan liste veya dizi değiştikçe güncelleştirmez. Öğeleri eklenmiş, kaldırılmış ve alttaki listede değiştirilmiş uygulandıkça otomatik güncelleştirilecek şekilde ListView istiyorsanız, kullanılacak gerekir bir `ObservableCollection`. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) tanımlanan `System.Collections.ObjectModel` ve tıpkı `List`bildirimde bulunabilir, ancak `ListView` değişiklikler:

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Veri Bağlama
Veri bağlama "bir sınıfta, ViewModel gibi bazı CLR nesnesinin özellikleri bir kullanıcı arabirimi nesnenin özelliklerini bağlar Yapıştırıcı" dir. Veri bağlama yararlıdır, çünkü çok sıkıcı Demirbaş kod değiştirerek kullanıcı arabirimlerinin geliştirilmesini kolaylaştırır.

Veri bağlama, ilişkili değerleri değiştikçe nesneleri eşitlenmiş durumda kalmasını sağlayarak çalışır. Bir denetimin değerini her değiştiğinde için olay işleyicileri yazmak zorunda kalmadan, bağlama kurmak ve bağlama, ViewModel içinde etkinleştir.

Veri bağlama hakkında daha fazla bilgi için bkz. [temel veri bağlama bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) olduğu bölümü dört [Xamarin.Forms XAML temelleri makale serisi](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Hücreleri bağlama
Hücreleri (ve alt öğeleri hücre) özelliklerini, nesnelerin özelliklerine bağlanabilir `ItemsSource`. Örneğin, bir ListView görüntülerle çalışanların bir listesini göstermek için kullanılabilir.

Çalışan sınıfı:

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` oluşturulur ve yap `ListView`'s `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

Liste verilerle doldurulur:

```csharp
public EmployeeListPage()
{
  ...
  employees.Add(new Employee{ DisplayName="Rob Finnerty"});
  employees.Add(new Employee{ DisplayName="Bill Wrestler"});
  employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
  employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
  employees.Add(new Employee{ DisplayName="Sheri Spruce"});
  employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

Aşağıdaki kod parçacığı gösteren bir `ListView` bağlı çalışanların listesi için:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
Title="Employee List">
  <ListView x:Name="EmployeeView">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Bu XAML içinde bağımlı ancak bağlama kod kolaylık olması için Kurulum olduğunu unutmayın.

Önceki bit XAML birini tanımlayan bir `ContentPage` içeren bir `ListView`. Veri kaynağını `ListView` aracılığıyla ayarlanan `ItemsSource` özniteliği. Her bir satırın düzenini `ItemsSource`içinde tanımlanan `ListView.ItemTemplate` öğesi.

Bu sonucu.

![](data-and-databinding-images/bound-data.png "Veri bağlama kullanarak ListView")

### <a name="binding-selecteditem"></a>SelectedItem bağlama

Genellikle seçili öğeye bağlamak isteyeceksiniz bir `ListView`yerine yerine bir olay işleyicisi değişikliklerine yanıt verme kullanabilir. XAML içinde bunun için bağlama `SelectedItem` özelliği:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

Varsayılarak `listView`'s `ItemsSource` dizeleri listesi `SomeLabel` ilişkili metin özelliğini olacaktır `SelectedItem`.



## <a name="related-links"></a>İlgili bağlantılar

- [İki yolla (örnek) bağlama](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [1.4 sürüm notları](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 sürüm notları](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
