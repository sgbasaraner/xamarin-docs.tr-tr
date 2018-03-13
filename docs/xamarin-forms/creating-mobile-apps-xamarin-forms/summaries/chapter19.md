---
title: "Bölüm 19 özeti. Koleksiyon görünümleri"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 73b3ec3e60a8fca5c48f515eab2cbb8359618dbb
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-19-collection-views"></a>Bölüm 19 özeti. Koleksiyon görünümleri

Xamarin.Forms koleksiyonları korumak ve bunların öğelerini görüntüleyen üç görünüm tanımlar:

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) kullanıcının bir seçmesine izin verir görece kısa dize öğeleri listesi
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) genellikle uzun öğeleri aynı türde genellikle listesidir ve biçimlendirme, ayrıca bir seçmesine izin verme
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) koleksiyonudur *hücreleri* (genellikle, çeşitli türleri ve görünümler) verileri görüntülemek veya kullanıcı girişi yönetmek için

Kullanılacak MVVM uygulamalar için ortak olan `ListView` nesneler seçilebilir koleksiyonunu görüntülemek için.

## <a name="program-options-with-picker"></a>Seçici programı seçenekleri

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Bir seçenek daha kısa bir listesini kümelerini seçmesine izin vermek gerektiğinde iyi bir seçimdir `string` öğeleri.

### <a name="the-picker-and-event-handling"></a>Seçici ve olay işleme

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) örnek XAML ayarlamak için nasıl kullanılacağını gösteren `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) özelliği ve ekleme `string` öğelerine[ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) koleksiyonu. Kullanıcı seçtiğinde `Picker`, öğeleri görüntüler `Items` bir platforma bağımlı şekilde koleksiyonu.

[ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Olayı, kullanıcı bir öğeyi seçili olduğunda gösterir. Sıfır tabanlı [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) özelliği daha sonra seçilen öğeyi gösterir. Öğe seçiliyse `SelectedIndex` eşittir &#x2013;1.

Aynı zamanda `SelectedIndex` seçilen öğe, ancak başlatma sonrasında ayarlanmalıdır `Items` koleksiyonu doldurulur. XAML'de, bu büyük olasılıkla bir özellik öğesi ayarlamak için kullanacağınız olduğunu anlamına gelir `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Veri Seçici bağlama

`SelectedIndex` Özelliği bağlanabilirse özelliği tarafından yedeklenen ancak `Items` bunu veri bağlama ile kullanarak değil, bir `Picker` zordur. Bir çözüm kullanmaktır `Picker` ile birlikte bir [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) birinde gibi [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) bunun nasıl çalıştığı gösterilmektedir.

## <a name="rendering-data-with-listview"></a>ListView verilerle işleme

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Türetilen tek sınıftır [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) hangi devralır gelen [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) ve [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) özellikleri.

`ItemsSource` tür `IEnumerable` ancak `null` varsayılan ve açıkça başlatılmadı veya gerekir (daha sık) veri bağlama aracılığıyla bir koleksiyona ayarlayın. Bu koleksiyondaki öğelerin herhangi bir türde olabilir.

`ListView` tanımlayan bir [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) da özellik kümesine öğelerden birini `ItemsSource` koleksiyonu veya `null` öğe seçili değilse. `ListView` ateşlenir [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) yeni bir öğe seçildiğinde olay.

### <a name="collections-and-selections"></a>Koleksiyonlar ve seçimleri

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) örnek dolgular bir `ListView` 17 `Color` değerler bir `List<Color>` koleksiyonu. Öğeleri seçilebilir ancak varsayılan olarak kendi paragrafta görüntülendikleri `ToString` temsili. Bu bölümde çeşitli örnekler, görüntülenen düzeltin ve istediğiniz gibi çekici olun gösterilmektedir.

### <a name="the-row-separator"></a>Satır ayırıcı

İOS ve Android görüntüler, ince bir çizgi satırları ayırır. Bu denetim [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/) ve [ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/) özellikleri. `SeparatorVisibility` özellik türüdür [ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/), iki üyesi olan bir numaralandırma:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.Default/), varsayılan ayar
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.None/)

### <a name="data-binding-the-selected-item"></a>Veri seçili öğe bağlama

`SelectedItem` Özelliği kaynak veya hedef veri bağlaması olabilir bağlanabilir bir özelliği tarafından yedeklenir. Varsayılan `BindingMode` olan `OneWayToSource`, ancak genellikle bu özellikle MVVM senaryolarda bir iki yönlü veri bağlama hedefidir. [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) örneği, bu tür bir bağlama gösterir.

### <a name="the-observablecollection-difference"></a>ObservableCollection fark

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) örnek kümeleri `ItemsSource` özelliği bir `ListView` için bir `List<DateTime>` koleksiyonu ve ardından aşamalı olarak yeni bir ekler `DateTime` nesnesini koleksiyona Her bir süreölçer ikinci.

Ancak, `ListView` otomatik olarak kendisini çünkü güncelleştirmez `List<T>` koleksiyon öğeleri için eklenemez veya koleksiyondan kaldırıldı zaman belirtmek için bir bildirim mekanizması yok.

Böyle senaryolarda kullanmak için bir çok daha iyi sınıf [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) tanımlanan `System.Collections.ObjectModel` ad alanı. Bu sınıf uygulayan [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/) arabirimi ve sonuç olarak ateşlenir bir [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/) öğeleri eklenen veya değiştirilen veya içinde taşındığında koleksiyondan veya kaldırıldığında olay koleksiyonu. Zaman `ListView` dahili olduğunu algılarsa bir sınıf uygulama `INotifyCollectionChanged` ayarlanmış kendi `ItemsSource` özelliği, bir işleyici iliştirir `CollectionChanged` olay ve koleksiyon değiştiğinde görünümünü güncelleştirir.

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) kullanımını gösteren örnek `ObservableCollection`.

### <a name="templates-and-cells"></a>Şablonlar ve hücreler

Varsayılan olarak, bir `ListView` her öğenin kullanarak kendi koleksiyondaki öğeler görüntüler `ToString` yöntemi. Daha iyi bir yaklaşım öğeleri görüntülemek için bir şablon tanımlama içerir.

Bu özellik ile denemek için kullanabileceğiniz [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. Bu sınıfın statik tanımlar `All` türündeki özelliği `IList<NamedColor>` 141 içeren `NamedColor` genel alanlarına karşılık gelen nesneleri `Color` yapısı.

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) örnek kümeleri `ItemsSource` , bir `ListView` bu `NamedColor.All` özelliği, ancak yalnızca tam sınıf adını `NamedColor` nesneler görüntülenir.

`ListView` Bu öğeleri görüntülemek için bir şablon gerekir. Kodda, ayarladığınız [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) özelliği tarafından tanımlanan `ItemsView<TVisual>` için bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) kullanarak nesne [ `DataTemplate` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) , bir türevi başvuran [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) sınıfı. `Cell` beş türevleri sahiptir:

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) &#x2014; iki içeren `Label` görünümler (kavramsal konuşulur)
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &#x2014; ekler bir `Image` için görüntüleyin `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &#x2014; içeren bir `Entry` ile görüntülemek bir `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &#x2014; içeren bir `Switch` ile bir `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &#x2014; herhangi `View` (olan büyük olasılıkla)

' I çağırın [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) ve [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) üzerinde `DataTemplate` değerleri ile ilişkilendirmek üzere nesnesine `Cell` özelliklerini veya veri bağlamaları ayarlamak için `Cell` öğelerin özelliklerini başvuran özellikleri `ItemsSource` koleksiyonu. Bu, gösterilmiştir [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) örnek.

Her bir öğe tarafından görüntülenen `ListView`, küçük bir görsel ağaç şablondan oluşturulan ve veri bağlamaları kurulmuş öğe ve öğelerin özellikleri arasında bu görsel ağaçta. İçin işleyiciler yükleyerek bu işlem hakkında bir fikir edinmek [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/) ve [ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/) olayları `ListView`, veya alternatif kullanarak [ `DataTemplate`Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/) öğenin görsel ağaç oluşturulmalıdır her zaman adlı bir işlev kullanır.

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) XAML'de tamamen işlevsel olarak aynı program gösterir. A `DataTemplate` etiketi ayarlanmış `ItemTemplate` özelliği `ListView`ve ardından `TextCell` ayarlanır `DataTemplate`. Koleksiyondaki öğelerin özelliklerini bağlamalar ayarlandığında doğrudan [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) ve [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) özelliklerini `TextCell`.

### <a name="custom-cells"></a>Özel hücreler

XAML'de ayarlamak olası bir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) için `DataTemplate` ve özel bir görsel ağaç olarak tanımlamak [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/) özelliği `ViewCell`. (`View` içerik özellik `ViewCell` böylece `ViewCell.View` etiketleri gerekli değildir.) [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) örneği, bu teknik gösterir:

[![Üçlü ekran görüntüsü özel adlı renk liste](images/ch19fg11-small.png "özel adlı renk liste")](images/ch19fg11-large.png#lightbox "özel adlı renk listesi")

Tüm platformlar için sağa boyutlandırma alma zor olabilir. [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/) Özelliği yararlıdır, ancak bazı durumlarda için çözümlemelere istersiniz [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/) daha az verimlidir özelliği, ancak zorlar `ListView` satır boyutu için. İOS ve Android için bu iki özellik birini uygun satır boyutlandırma almak için kullanmanız gerekir.

### <a name="grouping-the-listview-items"></a>ListView öğeleri gruplandırma

`ListView` öğeleri gruplandırma ve bu grupları arasında gezinme destekler. `ItemsSource` Özelliği değişkeninin bir koleksiyonlar koleksiyonu için ayarlanmalıdır: nesne, `ItemsSource` için gereken ayarlamak uygulamak `IEnumerable`, ve koleksiyondaki her öğe ayrıca uygulamalıdır `IEnumerable`. Her grup iki özellik içermelidir: Grup ve üç harfli kısaltması metin açıklaması.

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı yedi grupları oluşturur `NamedColor` nesneleri. [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) örnek ile bu grupların nasıl kullanılacağını göstermektedir [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/) özelliği `ListView` kümesine `true`ve [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) ve [ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) her grubu özelliklerinde özellikleri bağlı.

### <a name="custom-group-headers"></a>Özel grup üstbilgileri

Özel Üstbilgileri oluşturmak mümkündür `ListView` değiştirerek grupları `GroupDisplayBinding` özelliğiyle [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/) üstbilgileri için bir şablon tanımlama.

### <a name="listview-and-interactivity"></a>ListView ve etkileşim

Genellikle bir uygulama ile kullanıcı etkileşimi alacağı bir `ListView` bir işleyiciye ekleyerek `ItemSelected` veya [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) olayı veya veri bağlamada ayarlayarak `SelectedItem` özelliği. Ancak bazı hücre türleri (`EntryCell` ve `SwitchCell`) kullanıcı etkileşimine izin ver ve ayrıca özel hücrelerin, kendisini oluşturmak için olası kullanıcıyla etkileşim. [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) 100 örneklerini oluşturur [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) ve sorularının cevabını birini kullanarak her rengini değiştirmek kullanıcının sağlar `Slider` öğeleri. Program yapar kullanımını da [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) içinde [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView ve MVVM

`ListView` MVVM senaryolarda büyük bir rol oynar. Her bir `IEnumerable` koleksiyonu var bir ViewModel, genellikle bağlı bir `ListView`. Ayrıca, genellikle koleksiyondaki öğelerin uygulamak `INotifyPropertyChanged` şablondaki özelliklerle bağlamak için.

### <a name="a-collection-of-viewmodels"></a>ViewModels koleksiyonu

Bu, keşfetmek için [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) kitaplığı göre birkaç sınıfları oluşturur bir [XML veri dosyası](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) ve bu kurgusal okuldan kurgusal Öğrenciler görüntüler.

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) Sınıfı türer [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) Sınıftır koleksiyonu `Student` nesneleri ve ayrıca türeyen `ViewModelBase`. [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) XML dosyası indirilir ve tüm nesneleri derler.

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) program kullanan bir `ImageCell` Öğrenciler ve kendi görüntülerini görüntülemek için bir `ListView`:

[![Üçlü ekran görüntüsü Öğrenci listesi](images/ch19fg18-small.png "Öğrenci listesi")](images/ch19fg18-large.png#lightbox "Öğrenci listesi")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) örnek ekler bir [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/) özellik, ancak yalnızca görünür Android.

### <a name="selection-and-the-binding-context"></a>Seçim ve bağlama bağlamı

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) program bağlamalar `BindingContext` , bir `StackLayout` için `SelectedItem` özelliği `ListView`. Bu, seçilen Öğrenci hakkında ayrıntılı bilgileri görüntülemek program sağlar.

### <a name="context-menus"></a>Bağlam menüleri

Bir hücrenin bir platforma özgü şekilde uygulanan bir bağlam menüsü tanımlayabilirsiniz. Bu menü oluşturmak için Ekle [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) nesneleri [ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) özelliği `Cell`.

`MenuItem` beş özelliklerini tanımlar:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) türü `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) türü `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) türü `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) türü `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) türü `object`

`Command` Ve `CommandParameter` özellikleri kapsıyor ViewModel her öğe için istenen menü komutları yürütmek için yöntemler içerir. MVVM olmayan senaryolarda `MenuItem` de tanımlayan bir [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) olay.

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) bu tekniği gösterir. `Command` Her özellik `MenuItem` türünde bir özelliğe bağlı `ICommand` içinde `Student` sınıfı. Ayarlama `IsDestructive` özelliğine `true` için bir `MenuItem` , kaldırır veya seçilen nesneyi siler.

### <a name="varying-the-visuals"></a>Görsel değişen

Görsel hafif Çeşitlemeler öğelerinin bazen isteyeceksiniz `ListView` bir özelliğe dayalı. Örneğin, ne zaman öğrencinin düzeyde noktası ortalama düşer 2.0 [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) örnek kırmızıyla bu öğrencinin adını görüntüler.
Bu bağlama değer dönüştürücüsü kullanarak gerçekleştirilir [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs), [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı.

### <a name="refreshing-the-content"></a>İçerik yenileniyor

`ListView` Verileri yenilemek için aşağı açılır hareketi destekler. Program ayarlamalısınız [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/) özelliğine `true` bunu etkinleştirmek için. `ListView` Ayarlayarak aşağı açılır hareketi yanıt kendi [ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/) özelliğine `true`ve yükseltme tarafından [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/) olay ve (MVVM senaryoları) çağırma `Execute` yöntemi kendi [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) özelliği.

Kod işleme `Refresh` olay veya `RefreshCommand` tarafından görüntülenen veriler büyük olasılıkla güncelleştirmeleri `ListView` ve ayarlar `IsRefreshing` geri `false`.

[ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) örnek gösterilmektedir kullanarak bir [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) uygulayan `RefreshCommand` ve `IsRefreshing` veri bağlama özellikleri.

## <a name="the-tableview-and-its-intents"></a>Tablo görünümü ve kendi amaçları

Sırada `ListView` genellikle aynı türde birden çok örneğini görüntüler [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) genellikle birden çok çeşitli özellikler için bir kullanıcı arabirimi sağlamaya odaklanmıştır. Her öğe kendi ile ilişkilendirilen [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) özelliği görüntüleme veya bir kullanıcı arabirimi sağlayan türetilmiş.

### <a name="properties-and-hierarchies"></a>Özellikleri ve hiyerarşileri

`TableView` yalnızca dört özelliklerini tanımlar:

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) tür [ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/), numaralandırması
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) tür [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/), içerik özelliği `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) türü `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) türü `bool`

`TableIntent` Numaralandırma gösterir nasıl kullanmak istediğinize `TableView`:

- [`Data`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Data/)
- [`Form`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Form/)
- [`Settings`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Settings/)
- [`Menu`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Menu/)

Bu üyeler ayrıca bazı kullanımları önermek `TableView`.

Diğer birçok sınıf bir tablo tanımlama oynayan:

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) türetilen bir Özet sınıf `BindableObject` ve tanımlayan bir [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/) özelliği

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) türetilen bir Özet sınıf `TableSectionBase` ve uygulayan `IList<T>` ve `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) türetilen `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) türetilen `TableSectionBase<TableSection>`

Kısacası, `TableView` sahip bir `Root` ayarlamak için özellik bir `TableRoot` bir nesne, `TableSection` nesneleri, her biri koleksiyonudur `Cell` nesneleri. Bir tabloda birden çok bölüm ve birden çok hücreyi her bölüm varsa. Tabloda bir başlık olabilir ve her bölüm başlığı olabilir. Ancak `TableView` kullanır `Cell` türevleri, onu yapmaz kullanımı `DataTemplate`.

### <a name="a-prosaic-form"></a>Prosaic formu

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) örnek tanımlayan bir [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) görünüm modeli, bir örneği haline gelir `BindingContext` , `TableView`. Her `Cell` türetilmiş içinde kendi `TableSection` özelliklerine bağlamaları sonra olabilir `PersonalInformation` sınıfı.

### <a name="custom-cells"></a>Özel hücreler

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) örnek genişletir **EntryForm**. [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Sınıfı, iki ek özellikler uygulanabilirliğini yöneten bir Boolean özelliği içerir. Bu iki ek özellik için özel bir program kullanan `PickerCell` dayalı bir [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) ve [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) içinde [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı.

Ancak `IsEnabled` iki özelliklerini `PickerCell` öğeleri Boolean özelliğinde bağlı `ProgrammerInformation`, bu teknik çalışması için sonraki örnek ister görünmüyor.

### <a name="conditional-sections"></a>Koşullu bölümleri

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) örnek koyar ayrı bir Boolean öğesinde seçimini koşullu iki öğeyi `TableSection`. Arka plan kod dosyası bu bölümünden kaldırır `TableView` veya Boolean özelliği geri temel ekler.

### <a name="a-tableview-menu"></a>Bir tablo görünümü menüsü

Başka bir kullanımını bir `TableView` bir menüsü. [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) örnek gösterilmektedir biraz taşımanıza olanak sağlayan bir menü `BoxView` ekran çevresinde.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 19 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Bölüm 19 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
