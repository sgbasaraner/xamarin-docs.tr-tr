---
title: ListView veri kaynakları
description: Bu makalede, bir Xamarin.Forms ListView verilerle doldurmak nasıl ve veri bağlama ile bir ListView kullanma açıklanmaktadır.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2018
ms.openlocfilehash: 71e1655b6bc05c621ee97fcf826ce8b468f0dd48
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351502"
---
# <a name="listview-data-sources"></a>ListView veri kaynakları

A [ `ListView` ](xref:Xamarin.Forms.ListView) veri listesini görüntülemek için kullanılır. Bir ListView veri ve nasıl biz seçili öğeye bağlayabilirsiniz doldurma hakkında bilgi edineceksiniz.

- **[ItemsSource ayarlama](#ItemsSource)**  &ndash; basit bir liste veya dizi kullanır.
- **[Veri bağlama](#Data_Binding)**  &ndash; ListView ve bir modeli arasında bir ilişki kurar. Bağlama MVVM desen için idealdir.

## <a name="itemssource"></a>ItemsSource

A [ `ListView` ](xref:Xamarin.Forms.ListView) kullanarak veri ile doldurulan [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) koleksiyon uygulama kabul edebilir özelliği `IEnumerable`. Doldurmak için en kolay yolu bir `ListView` dizelerden oluşan bir dizi kullanmayı içerir:

```xaml
<ListView>
      <ListView.ItemsSource>
          <x:Array Type="{x:Type x:String}">
            <x:String>mono</x:String>
            <x:String>monodroid</x:String>
            <x:String>monotouch</x:String>
            <x:String>monorail</x:String>
            <x:String>monodevelop</x:String>
            <x:String>monotone</x:String>
            <x:String>monopoly</x:String>
            <x:String>monomodal</x:String>
            <x:String>mononucleosis</x:String>
          </x:Array>
      </ListView.ItemsSource>
</ListView>
```

Eşdeğer C# kodu verilmiştir:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]
{
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
