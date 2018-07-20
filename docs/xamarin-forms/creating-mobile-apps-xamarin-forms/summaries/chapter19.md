---
title: Bölüm 19 özeti. Koleksiyon görünümleri
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 19 özeti. Koleksiyon görünümleri'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: 01985cf253c0f33c52128386b36c11af50381ee1
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156697"
---
# <a name="summary-of-chapter-19-collection-views"></a>Bölüm 19 özeti. Koleksiyon görünümleri

> [!NOTE] 
> Bu sayfadaki notları kitapta tanıtılan malzeme gelen Xamarin.Forms nerede ayrıldığını alanları gösterir.

Xamarin.Forms koleksiyonları korumak ve öğeleri görüntüleyen üç görünüm tanımlar:

- [`Picker`](xref:Xamarin.Forms.Picker) kullanıcının birini seçmesine izin veren bir kısa dize öğelerin listesidir
- [`ListView`](xref:Xamarin.Forms.ListView) genellikle uzun genellikle aynı türdeki öğelerinin listesini içerir ve biçimlendirme, ayrıca kullanıcının birini seçmesine izin verme
- [`TableView`](xref:Xamarin.Forms.TableView) oluşan bir koleksiyondur *hücreleri* (genellikle, çeşitli türler ve görünümler) verileri görüntülemek veya kullanıcı girişi yönetmek için

MVVM uygulamaların kullanması için ortak olan `ListView` seçilebilir nesneler koleksiyonunu görüntülemek için.

## <a name="program-options-with-picker"></a>Program seçenekleri Seçici

[ `Picker` ](xref:Xamarin.Forms.Picker) Görece kısa bir listesi arasından bir seçim yapmalarına izin vermek istediğinizde iyi bir seçimdir `string` öğeleri.

### <a name="the-picker-and-event-handling"></a>Olay işleme ve Seçici

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) örnek XAML ayarlamak için nasıl kullanılacağını gösterir `Picker` [ `Title` ](xref:Xamarin.Forms.Picker.Title) özelliği ve ekleme `string` öğelerine[ `Items` ](xref:Xamarin.Forms.Picker.Items) koleksiyonu. Kullanıcı seçtiğinde `Picker`, panosundaki `Items` platforma bağımlı bir şekilde koleksiyonu.

[ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Olayı, kullanıcı bir öğenin seçili olduğunda gösterir. Sıfır tabanlı [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) özelliği, ardından seçilen öğeyi gösterir. Hiçbir öğe seçili değilse `SelectedIndex` eşittir &ndash;1.

Ayrıca `SelectedIndex` seçilen öğe, ancak başlatma sonrasında ayarlanmalıdır `Items` koleksiyonu doldurulur. XAML içinde bu, büyük olasılıkla bir özellik öğesi ayarlamak için kullanacağınız anlamına gelir `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Veri bağlama Seçici

`SelectedIndex` Özelliği, bir bağlanılabilir özellik tarafından desteklenir, ancak `Items` kullanarak veri bağlama ile değil, bir `Picker` zordur. Bir çözüm ise kullanılacak `Picker` birlikte bir [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) gibi bir [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) bunun nasıl çalıştığı gösterilmektedir.

> [!NOTE] 
> Xamarin.Forms `Picker` artık `ItemsSource` ve `SelectedItem` veri bağlamayı destekleyen özellikleri. Bkz: [Seçici](~/xamarin-forms/user-interface/picker/index.md).

## <a name="rendering-data-with-listview"></a>ListView ile veri işleme

[ `ListView` ](xref:Xamarin.Forms.ListView) Türetildiği tek sınıftır [ `ItemsView<TVisual>` ](xref:Xamarin.Forms.ItemsView`1) devralır, gelen [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) ve [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) özellikleri.

`ItemsSource` tür `IEnumerable` ancak `null` varsayılan olarak ve açıkça başlatılır veya gereken bir koleksiyona aracılığıyla veri bağlama (daha sık) ayarlayın. Bu koleksiyondaki öğelerin herhangi bir türde olabilir.

`ListView` tanımlayan bir [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) ya da özellik kümesi öğeleri birine `ItemsSource` koleksiyonu veya `null` hiçbir öğe seçili değilse. `ListView` ateşlenir [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) yeni bir öğe seçildiğinde olay.

### <a name="collections-and-selections"></a>Koleksiyonlar ve seçimleri

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) örnek dolduran bir `ListView` 17 `Color` değerler bir `List<Color>` koleksiyonu. Seçilebilir öğeleri ancak varsayılan olarak, paragrafta görüntülendikleri `ToString` beyanda bulunmamaktadır. Bu bölümde birkaç örnek görüntüleyen düzeltin ve istediğiniz gibi cazip hale işlemini göstermektedir.

### <a name="the-row-separator"></a>Satır ayırıcı

İOS ve Android görüntüler üzerinde ince bir çizgi satırları ayırır. Bu denetim [ `SeparatorVisibiliy` ](xref:Xamarin.Forms.ListView.SeparatorVisibility) ve [ `SeparatorColor` ](xref:Xamarin.Forms.ListView.SeparatorColor) özellikleri. `SeparatorVisibility` özelliği türünü [ `SeparatorVisbility` ](xref:Xamarin.Forms.SeparatorVisibility), iki üyesi olan bir sabit listesi:

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default), varsayılan ayar
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>Veri seçili öğe bağlama

`SelectedItem` Özelliği, bir bağlanılabilir özellik tarafından desteklenir, kaynak veya hedef veri bağlamanın olabilir. Varsayılan `BindingMode` olduğu `OneWayToSource`, ancak genellikle MVVM senaryolarda özellikle bir iki yönlü veri bağlama hedefi. [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) örnek bu bağlama türünü gösterir.

### <a name="the-observablecollection-difference"></a>ObservableCollection farkı

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) örnek kümeleri `ItemsSource` özelliği bir `ListView` için bir `List<DateTime>` koleksiyonu ve ardından aşamalı olarak yeni bir ekler `DateTime` nesnesini koleksiyonuna Her bir zamanlayıcı kullanarak ikinci.

Ancak, `ListView` otomatik olarak kendisini çünkü güncelleştirmez `List<T>` koleksiyon öğeleri eklenecek veya koleksiyondan kaldırılır zaman belirtmek için bir bildirim mekanizması yok.

Böyle senaryolarda kullanmak için çok daha iyi sınıfıdır [ `ObservableCollection<T>` ](xref:System.Collections.ObjectModel.ObservableCollection`1) tanımlanan `System.Collections.ObjectModel` ad alanı. Bu sınıfın uyguladığı [ `INotifyCollectionChanged` ](xref:System.Collections.Specialized.INotifyCollectionChanged) arabirimi ve sonuç olarak etkinleşir bir [ `CollectionChanged` ](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) olay öğeleri eklendiğinde veya değiştirilen veya içinde taşınan koleksiyondan veya kaldırıldı koleksiyonu. Zaman `ListView` dahili olarak olduğunu algılarsa bir sınıf uygulamak `INotifyCollectionChanged` olarak ayarlanan kendi `ItemsSource` özelliği için bir işleyici ekler `CollectionChanged` olay ve koleksiyonu değiştiğinde görünümünü güncelleştirir.

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) örnek, kullanımını gösterir `ObservableCollection`.

### <a name="templates-and-cells"></a>Şablonları ve hücreler

Varsayılan olarak, bir `ListView` her öğenin kullanarak, bir koleksiyondaki öğeleri görüntüler `ToString` yöntemi. Öğeleri görüntülemek için bir şablon tanımlama daha iyi bir yaklaşım gerektirir.

Bu özellikle ilgili denemeler için kullanabileceğiniz [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. Bu sınıfın statik tanımlar `All` türünün özelliği `IList<NamedColor>` 141 içeren `NamedColor` genel alanlarına karşılık gelen nesneleri `Color` yapısı.

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) örnek kümeleri `ItemsSource` , bir `ListView` bu `NamedColor.All` özelliği, ancak yalnızca tam sınıf adını `NamedColor` nesneler görüntülenir.

`ListView` Bu öğeleri görüntülemek için bir şablon gerekir. Kod içinde ayarlayabilirsiniz [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) özelliği tarafından tanımlanan `ItemsView<TVisual>` için bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) kullanarak nesne [ `DataTemplate` Oluşturucusu](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) , bir türevi başvuran [ `Cell` ](xref:Xamarin.Forms.Cell) sınıfı. `Cell` beş türevleri sahiptir:

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; iki tane `Label` görünümleri (kavramsal olarak konuşma)
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; ekler bir `Image` görüntüleyin `TextCell`
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; içeren bir `Entry` görünümüyle bir `Label`
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; içeren bir `Switch` ile bir `Label`
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; herhangi biri `View` (alt öğeleri olan büyük olasılıkla)

Ardından çağırın [ `SetValue` ](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) ve [ `SetBinding` ](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) üzerinde `DataTemplate` değerleri ile ilişkilendirilecek bir nesne `Cell` özellikleri veya veri bağlamaları ayarlamak için `Cell` Özellikleri'ndeki öğelerin kullanımını başvuran özellikleri `ItemsSource` koleksiyonu. Bu gösterilmiştir [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) örnek.

Her öğesi tarafından görüntülenen `ListView`, küçük bir görsel ağacı şablondan oluşturulmuş olan ve veri bağlamalarını bu görsel ağaçta belirlenir öğelerin özelliklerini öğe arasında. İşleyicileri yükleyerek bu işlemin bir fikir edinebilirsiniz [ `ItemAppearing` ](xref:Xamarin.Forms.ListView.ItemAppearing) ve [ `ItemDisappearing` ](xref:Xamarin.Forms.ListView.ItemDisappearing) olayları `ListView`, veya alternatif kullanarak [ `DataTemplate`Oluşturucusu](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})) görsel ağaçta bir öğenin oluşturulması her zaman çağrılan bir işlev kullanır.

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) işlevsel olarak eşdeğer bir program tamamen XAML içinde gösterir. A `DataTemplate` etiket ayarlandığında `ItemTemplate` özelliği `ListView`, ardından `TextCell` ayarlanır `DataTemplate`. Koleksiyondaki öğelerin özelliklerini bağlamaları doğrudan ayarlanır [ `Text` ](xref:Xamarin.Forms.TextCell.Text) ve [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) özelliklerini `TextCell`.

### <a name="custom-cells"></a>Özel hücreleri

XAML içinde ayarlamak olası bir [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) için `DataTemplate` ve bir özel görsel ağaç olarak tanımlayabilirsiniz [ `View` ](xref:Xamarin.Forms.ViewCell.View) özelliği `ViewCell`. (`View` içerik özelliği `ViewCell` böylece `ViewCell.View` etiketleri gerekli değildir.) [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) örnek, bu teknik gösterir:

[![Üçlü listesinin ekran görüntüsü özel adlı renk](images/ch19fg11-small.png "özel adlı renk liste")](images/ch19fg11-large.png#lightbox "özel adlı renk listesi")

Tüm platformlar için doğru boyutlandırma alma zor olabilir. [ `RowHeight` ](xref:Xamarin.Forms.ListView.RowHeight) Özellik kullanışlıdır ancak bazı durumlarda için başvurmadan isteyeceksiniz [ `HasUnevenRows` ](xref:Xamarin.Forms.ListView.HasUnevenRows) özelliği daha az verimlidir, ancak zorlar `ListView` satırları boyutlandırmak için. İOS ve Android için uygun satır boyutlandırma almak için iki bu özelliklerden birini kullanmalısınız.

### <a name="grouping-the-listview-items"></a>ListView öğeleri gruplandırma

`ListView` öğeleri gruplandırması ve bu grupları arasında gezinme destekler. `ItemsSource` Özelliği değişkeninin bir koleksiyonlar koleksiyonu için ayarlanmalıdır: nesne, `ItemsSource` için gereken ayarlanır uygulamak `IEnumerable`, ve koleksiyondaki her öğe de uygulamalıdır `IEnumerable`. Her grup, iki özellik içermelidir: Grup, bir üç harfli kısaltması metin açıklaması.

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı oluşturur yedi grupları `NamedColor` nesneleri. [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) örnek ile bu grupların nasıl kullanılacağını göstermektedir [ `IsGroupingEnabled` ](xref:Xamarin.Forms.ListView.IsGroupingEnabled) özelliği `ListView` kümesine `true`ve [ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding) ve [ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding) her grubu özelliklerinde ilişkili özellikler.

### <a name="custom-group-headers"></a>Özel grup üstbilgileri

Özel üst bilgiler için oluşturmak mümkündür `ListView` değiştirerek grupları `GroupDisplayBinding` özelliğiyle [ `GroupHeaderTemplate` ](xref:Xamarin.Forms.ListView.GroupHeaderTemplate) üstbilgileri için bir şablon tanımlama.

### <a name="listview-and-interactivity"></a>ListView ve etkileşim

Genellikle bir uygulama ile kullanıcı etkileşimi alır bir `ListView` bir işleyici ekleyerek `ItemSelected` veya [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) olay veya veri bağlamada ayarlayarak `SelectedItem` özelliği. Ancak bazı hücre türleri (`EntryCell` ve `SwitchCell`) kullanıcı etkileşimine izin ver ve ayrıca özel hücrelerin, kendisini oluşturmak için olası kullanıcıyla etkileşim. [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) 100 örneklerini oluşturur [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) ve bir dizin Üçlüsü kullanarak her rengini değiştirmek kullanıcının `Slider` öğeleri. Program yapar ayrıca kullanım [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) içinde [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView ve MVVM

`ListView` MVVM senaryolarda büyük bir rol oynar. Her bir `IEnumerable` koleksiyonu içinde bir ViewModel var, genellikle bağlı bir `ListView`. Ayrıca, genellikle koleksiyondaki öğelerin uygulamak `INotifyPropertyChanged` şablon özelliklerinde ile bağlamak için.

### <a name="a-collection-of-viewmodels"></a>Bir koleksiyonu Viewmodel'lar

Bu, keşfetmek için [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) göre birkaç sınıf kitaplığı oluşturur bir [XML veri dosyası](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) ve bu kurgusal Okul kurgusal öğrencileri görüntülerini.

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) Sınıf türetilir [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) Sınıftır koleksiyonu `Student` nesneleri ve ayrıca türetildiği `ViewModelBase`. [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) XML dosyasını yükler ve tüm nesneleri birleştirir.

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) program kullanan bir `ImageCell` Öğrenciler ve bunların resimleri görüntülemek için bir `ListView`:

[![Üçlü listesinin ekran görüntüsü Öğrenci](images/ch19fg18-small.png "Öğrenci listesi")](images/ch19fg18-large.png#lightbox "Öğrenci listesi")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) örnek ekler bir [ `Header` ](xref:Xamarin.Forms.ListView.Header) özelliği ancak yalnızca gösterilir Android'de.

### <a name="selection-and-the-binding-context"></a>Seçim ve bağlama bağlamı

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) program bağlamalar `BindingContext` , bir `StackLayout` için `SelectedItem` özelliği `ListView`. Bu, seçilen Öğrenci hakkında ayrıntılı bilgileri görüntülemek program sağlar.

### <a name="context-menus"></a>Bağlam menüleri

Hücre bir platforma özgü şekilde uygulanan bir bağlam menüsü tanımlayabilirsiniz. Bu menü oluşturmak için Ekle [ `MenuItem` ](xref:Xamarin.Forms.MenuItem) nesneleri için [ `ContextActions` ](xref:Xamarin.Forms.Cell.ContextActions) özelliği `Cell`.

`MenuItem` beş özelliklerini tanımlar:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) türü `string`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) türü `FileImageSource`
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) türü `bool`
- [`Command`](xref:Xamarin.Forms.MenuItem.Command) türü `ICommand`
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) türü `object`

`Command` Ve `CommandParameter` özellikleri yaptığından ViewModel her öğe için istenen menü komutları yürütmek için yöntemler içerir. MVVM olmayan senaryolarda `MenuItem` ayrıca tanımlayan bir [ `Clicked` ](xref:Xamarin.Forms.MenuItem.Clicked) olay.

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) bu tekniği gösterir. `Command` Her özellik `MenuItem` türünde bir özelliğe bağlı `ICommand` içinde `Student` sınıfı. Ayarlama `IsDestructive` özelliğini `true` için bir `MenuItem` kaldırır veya seçili nesneyi siler.

### <a name="varying-the-visuals"></a>Görsellerin Çeşitleme uygulanıyor

Görsellerdeki küçük farklılıklar'ndeki öğelerin kullanımını bazen isteyeceksiniz `ListView` özelliğini temel alarak. Örneğin, ne zaman bir öğrenci sınıf noktası ortalama düşer 2.0 altında [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) örnek kırmızı renkte, öğrencinin adını görüntüler.
Bu bağlama değer dönüştürücü kullanılmasıyla gerçekleştirilir [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs), [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı.

### <a name="refreshing-the-content"></a>İçerik yenileniyor

`ListView` Verileri yenilemek için aşağı açılır hareket destekler. Program ayarlamalısınız [ `IsPullToRefresh` ](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) özelliğini `true` bunu etkinleştirmek için. `ListView` Ayarlayarak için aşağı açılır hareket yanıt kendi [ `IsRefreshing` ](xref:Xamarin.Forms.ListView.IsRefreshing) özelliğini `true`ve oluşturma tarafından [ `Refreshing` ](xref:Xamarin.Forms.ListView.Refreshing) olay ve (MVVM senaryoları için) çağırma `Execute` yöntemi kendi [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) özelliği.

Kod işleme `Refresh` olay veya `RefreshCommand` tarafından görüntülenen veriler büyük olasılıkla güncelleştirmeleri `ListView` ve ayarlar `IsRefreshing` geri `false`.

[ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) örnek gösterir kullanarak bir [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) uygulayan `RefreshCommand` ve `IsRefreshing` veri bağlama özellikleri.

## <a name="the-tableview-and-its-intents"></a>Tablo görünümü ve kendi amaçları

Sırada `ListView` genellikle aynı türde birden çok örneğini görüntüler [ `TableView` ](xref:Xamarin.Forms.TableView) genellikle bir kullanıcı arabirimi için çeşitli türlerde birden çok özellik sağlamaya odaklanır. Her bir öğe kendi ile ilişkilendirilen [ `Cell` ](xref:Xamarin.Forms.Cell) türetme özelliği görüntülemek veya ona bir kullanıcı arayüzü sağlama.

### <a name="properties-and-hierarchies"></a>Özellikler ve hiyerarşiler

`TableView` yalnızca dört özelliklerini tanımlar:

- [`Intent`](xref:Xamarin.Forms.TableView.Intent) tür [ `TableIntent` ](xref:Xamarin.Forms.TableIntent), bir sabit listesi
- [`Root`](xref:Xamarin.Forms.TableView.Root) tür [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), içerik özelliği `TableView`
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) türü `int`
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) türü `bool`

`TableIntent` Numaralandırma gösterir nasıl kullanmak istediğinize `TableView`:

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

Bu üyeleri ayrıca bazı kullanımları Öner `TableView`.

Diğer sınıfları tanımlarken bir tablo ilgilidir:

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) türetilen bir soyut sınıfı `BindableObject` ve tanımlayan bir [ `Title` ](xref:Xamarin.Forms.TableSectionBase.Title) özelliği

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) türetilen bir soyut sınıfı `TableSectionBase` ve uygulayan `IList<T>` ve `INotifyCollectionChanged`

- [`TableSection`](xref:Xamarin.Forms.TableSection) öğesinden türetilen `TableSectionBase<Cell>`

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) öğesinden türetilen `TableSectionBase<TableSection>`

Kısacası, `TableView` sahip bir `Root` ayarlamak için özellik bir `TableRoot` bir koleksiyon nesne, `TableSection` nesneleri, her biri, koleksiyonudur `Cell` nesneleri. Bir tabloda birden çok bölümü vardır ve her bölümde birden çok hücre vardır. Tablo, bir başlık olabilir ve her bölümde bir başlık olabilir. Ancak `TableView` kullanır `Cell` türevleri bunu yapmaz kullanım `DataTemplate`.

### <a name="a-prosaic-form"></a>Prosaic form

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) örnek tanımlayan bir [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) görünüm modeli, bir örneği haline gelir `BindingContext` , `TableView`. Her `Cell` türetme içinde kendi `TableSection` ardından özelliklerine bağlamaları olabilir `PersonalInformation` sınıfı.

### <a name="custom-cells"></a>Özel hücreleri

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) örnek genişletir üzerinde **EntryForm**. [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Sınıfı iki ek özellikler uygulanabilirliğini yöneten bir Boolean özelliği içerir. Bu iki ek özellikler için özel bir programın kullandığı `PickerCell` dayalı bir [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) ve [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) içinde [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı.

Ancak `IsEnabled` iki özelliklerini `PickerCell` Boole özelliğine bağlı öğeleri `ProgrammerInformation`, bu tekniği iş, sonraki örnek, ister görünmüyor.

### <a name="conditional-sections"></a>Koşullu bölümler

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) örnek koyar ayrı bir Boolean öğesi seçime koşullu iki öğeyi `TableSection`. Arka plan kod dosyası bu bölümünden kaldırır `TableView` veya Boolean özelliği geri alarak ekler.

### <a name="a-tableview-menu"></a>Tablo görünümü menüsü

Başka bir kullanımını bir `TableView` bir menü. [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) örnek gösterir biraz geçiş yapmanıza izin veren bir menü `BoxView` ekranın etrafında.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 19 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Bölüm 19 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [Seçici](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
