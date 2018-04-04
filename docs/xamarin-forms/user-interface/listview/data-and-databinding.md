---
title: ListView veri kaynakları
description: ListView verilerle doldurmak öğrenin.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6f0553f2887d4acb1015da71395be3f316e9acee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="listview-data-sources"></a>ListView veri kaynakları

ListView veri listesini görüntülemek için kullanılır. ListView veriler ve nasıl biz seçilen öğeye bağlayabilirsiniz ile doldurma hakkında bilgi edineceksiniz.

- **[ItemsSource ayarı](#ItemsSource)**  &ndash; basit bir liste veya dizi kullanır.
- **[Veri bağlama](#Data_Binding)**  &ndash; model ve ListView arasında bir ilişki kurar. Bağlama MVVM düzeni için idealdir.

## <a name="itemssource"></a>ItemsSource
ListView verileri kullanarak ile doldurulur `ItemsSource` tüm koleksiyon uygulama kabul edebilir özelliği `IEnumerable`. En basit yolu doldurmak için bir `ListView` bir dizeler dizisi kullanmayı içerir:

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

Yukarıdaki yaklaşım dolduracaktır `ListView` dizelerin bir listesi. Varsayılan olarak, `ListView` çağıracak `ToString` ve sonuçta görüntülemek bir `TextCell` her satır için. Verinin nasıl görüntüleneceğini özelleştirmek için bkz: [hücre Görünüm](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Çünkü `ItemsSource` gönderildikten bir diziye, içeriği arka plandaki liste veya dizi değiştikçe güncelleştirmez. ListView öğeleri eklenmiş, kaldırılmış ve arka plandaki listesinde değiştirilen gibi otomatik olarak güncelleştirmek istiyorsanız kullanmanız gerekecektir bir `ObservableCollection`. [`ObservableCollection`](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) tanımlanan `System.Collections.ObjectModel` ve tıpkı `List`, bildirimde bulunabilir ancak bu `ListView` değişiklikler:

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Veri Bağlama
Veri bağlama ", ViewModel sınıfında gibi bazı CLR nesnesinin özellikleri için bir kullanıcı arabirimi nesnenin özelliklerini bağlar Birleştirici" dir. Veri bağlama yararlı nedeni çok sayıda sıkıcı Demirbaş kod değiştirerek kullanıcı arabirimleri geliştirilmesini kolaylaştırır.

Veri bağlama, ilişkili değerlerini değiştirirken nesneleri eşitlenmiş tutarak çalışır. Bir denetimin değeri her değiştiğinde olay işleyicileri için yazmak zorunda kalmadan, bağlama kurmak ve bağlama, ViewModel etkinleştirin.

Veri bağlama hakkında daha fazla bilgi için bkz: [veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) olduğu bölümü dört [Xamarin.Forms XAML temelleri makalesi serisi](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Hücreleri bağlama
Hücreleri (ve alt hücre) özelliklerini, nesnelerin özelliklerini bağlanabilir `ItemsSource`. Örneğin, bir ListView görüntülerle çalışanların bir listesini sunmak için kullanılabilir.

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

Aşağıdaki kod parçacığında gösteren bir `ListView` çalışanlar bir listeye bağlı:

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

XAML'de bağlı ancak bağlama kuruldu kod kolaylık unutmayın.

XAML önceki bit tanımlayan bir `ContentPage` içeren bir `ListView`. Veri kaynağı `ListView` aracılığıyla ayarlanır `ItemsSource` özniteliği. Her satırda düzenini `ItemsSource`içinde tanımlanan `ListView.ItemTemplate` öğesi.

Bu sonuç.

![](data-and-databinding-images/bound-data.png "Veri bağlama kullanarak ListView")

### <a name="binding-selecteditem"></a>SelectedItem bağlama

Genellikle seçilen öğeye bağlamak istersiniz bir `ListView`, bunun yerine bir olay işleyicisi değişikliklere çıkabilir. XAML'de bunu bağlamak `SelectedItem` özelliği:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

Varsayılarak `listView`'s `ItemsSource` dizeleri listesidir `SomeLabel` bağlı kendi text özelliğine sahip `SelectedItem`.



## <a name="related-links"></a>İlgili bağlantılar

- [İki yolla (örnek) bağlama](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [1.4 sürüm notları](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 sürüm notları](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
