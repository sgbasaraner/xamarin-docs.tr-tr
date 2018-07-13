---
title: ListView görünümünü özelleştirme
description: Bu makalede, üstbilgiler, altbilgiler, grupları ve değişken yükseklik hücreleri kullanarak Xamarin.Forms uygulamalarında ListViews özelleştirileceği açıklanmaktadır.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 1326a1326b4a88459e4e0a01ef590e770e3a88c0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997354"
---
# <a name="customizing-listview-appearance"></a>ListView görünümünü özelleştirme

`ListView` arka plandaki yanı sıra genel listenin sunu denetleme seçenekleri sahip `ViewCell`s. Seçenekler şunlardır:

- [**Gruplandırma** ](#Grouping) &ndash; daha kolay gezinme ve geliştirilmiş bir kuruluş için ListView içinde öğeleri gruplandırma.
- [**Üstbilgiler ve altbilgiler** ](#Headers_and_Footers) &ndash; başlangıcına ve sonuna kadar diğer öğelerle kayan görünümünü bilgileri görüntüler.
- [**Satır ayırıcı** ](#Row_Separators) &ndash; göster veya gizle öğeleri arasındaki ayırıcı satırlar.
- [**Değişken yükseklik satırları** ](#Row_Heights) &ndash; varsayılan olarak tüm satırların aynı olan, ancak bu, görüntülenecek farklı yüksekliklerini satırlarla izin verecek şekilde değiştirilebilir.

<a name="Grouping" />

## <a name="grouping"></a>Gruplandırma
Genellikle, büyük veri kümelerine yönelik bir sürekli kaydırma listesinde yansıtılırken zahmetli hale gelebilir. Etkinleştirme gruplandırma, içeriği daha iyi düzenlemek ve gezinme veri kolaylaştırmak platforma özel denetimleri etkinleştirme bu gibi durumlarda kullanıcı deneyimini geliştirebilir.

Ne zaman gruplandırma etkin olduğu için bir `ListView`, her grup için bir üst bilgi satırı eklenir.

Gruplandırma etkinleştirmek için:

- Bir liste (gruplarının bir listesini, olan öğelerin listesini her bir grubu) listesini oluşturun.
- Ayarlama `ListView`'s `ItemsSource` bu listeye.
- Ayarlama `IsGroupingEnabled` true.
- Ayarlama [ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding) grup başlığı olarak kullanılan özellik gruplarının bağlamak için.
- [İsteğe bağlı] Ayarlama [ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding) grubu için kısa ad olarak kullanılan özellik gruplarının bağlamak için. Kısa adı, bağlantı listeleri (iOS üzerinde sağ tarafında sütun) için kullanılır.

Gruplar için bir sınıf oluşturarak başlayın:

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

Yukarıdaki kodda, `All` bağlama kaynağı olarak bizim ListView için verilen listesidir. `Title` ve `ShortName` grup başlıklarını için kullanılacak olan özelliklerdir.

Bu aşamada, `All` boş bir listedir. Liste, program başlangıcında doldurulacak bir statik oluşturucu ekleyin:

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alfa", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        }
        All = Groups; //set the publicly accessible list
}
```

Yukarıdaki kodda biz de çağırabilirsiniz `Add` öğeleri üzerinde `groups`, türün örneklerinin olduğu `PageTypeGroup`. Bu mümkün olur çünkü `PageTypeGroup` devraldığı `List<PageModel>`. Bu, yukarıda belirtilen listeleri Desen listesinin bir örnektir.

XAML gruplandırılmış listesini görüntülemek için şu şekildedir:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
        GroupDisplayBinding="{Binding Title}"
        GroupShortNameBinding="{Binding ShortName}"
        IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                     Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Bu durum şunlara sebep olur:

![](customizing-list-appearance-images/grouping-depth.png "ListView gruplama örneği")

Sahip olduğumuz unutmayın:

- Ayarlama `GroupShortNameBinding` için `ShortName` bizim Grup sınıfı içinde tanımlanan bir özellik
- Ayarlama `GroupDisplayBinding` için `Title` bizim Grup sınıfı içinde tanımlanan bir özellik
- Ayarlama `IsGroupingEnabled` true
- Değiştirilen `ListView`'s `ItemsSource` gruplanmış listesi

### <a name="customizing-grouping"></a>Gruplandırma özelleştirme

Gruplandırma listesinde etkinleştirildiyse, Grup üstbilgisi da özelleştirilebilir.

Benzer şekilde nasıl `ListView` sahip bir `ItemTemplate` satırları görüntülenme tanımlama `ListView` sahip bir `GroupHeaderTemplate`.

XAML Grup üstbilgisinde özelleştirilmesine bir örnek aşağıda gösterilmiştir:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
         GroupDisplayBinding="{Binding Title}"
         GroupShortNameBinding="{Binding ShortName}"
         IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding Subtitle}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding ShortName}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization -->
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>Üstbilgiler ve Altbilgiler
Bir ListView üstbilgi ve altbilgi, kaydırma listesi öğeleriyle sunmak mümkündür. Üstbilgi ve altbilgi dizeler metin veya daha karmaşık bir düzen olabilir. Bu ayrı olduğunu unutmayın [bölüm grupları](#Grouping).

Ayarlayabileceğiniz `Header` ve/veya `Footer` için basit bir dize değeri veya bunları daha karmaşık bir düzene ayarlayabilirsiniz.
Ayrıca `HeaderTemplate` ve `FooterTemplate` olanak tanıyan özellikler üstbilgi ve altbilgi için daha karmaşık düzenler, destek veri bağlama oluşturun.

Basit bir üstbilgi/altbilgi oluşturmak için üstbilgisi veya altbilgisi özelliklerini görüntülemek istediğiniz metni ayarlamanız yeterlidir. Kod:

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

XAML içinde:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView üstbilgi ve altbilgi")

Özelleştirilmiş üstbilgi ve altbilgi oluşturmak için üstbilgi ve altbilgi görünümleri tanımlayın:

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
        TextColor="Olive"
        BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
        TextColor="Gray"
        BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView özelleştirilmiş üstbilgi ve altbilgi")

<a name="Row_Separators" />

## <a name="row-separators"></a>Satır ayırıcı
Ayırıcı satırlar arasında görüntülenir `ListView` öğeleri varsayılan olarak, iOS ve Android. İOS ve Android'de ayırıcı satırları gizle tercih verilirse `SeparatorVisibility` , ListView özelliği. Seçeneklerini `SeparatorVisibility` şunlardır:

* **Varsayılan** -iOS ve Android üzerinde bir ayırıcı çizginin gösterir.
* **Hiçbiri** -tüm platformlarda ayırıcı gizler.

Varsayılan görünürlüğü:

C# İÇİN:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "Varsayılan satır ayırıcı ile ListView")

Hiçbiri:

C# İÇİN:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView satır ayırıcı olmadan")

Bir ayırıcı çizginin rengini ayarlayabilirsiniz `SeparatorColor` özelliği:

C# İÇİN:

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView yeşil satır ayırıcı ile")

> [!NOTE]
> Bu özelliklerin herhangi birini yüklemeden sonra Android'de ayarlama `ListView` büyük performans cezasına sebep olur.

<a name="Row_Heights" />

## <a name="row-heights"></a>Satır yüksekliklerini
Bir ListView tüm satırlarda, varsayılan olarak aynı yüksekliğe sahiptir. ListView bu davranışı değiştirmek için kullanılan iki özelliğe sahiptir:

- `HasUnevenRows` &ndash; `true`/`false` değer, satır varsa değişen yüksekliklerini kümesine `true`. Varsayılan olarak `false`.
- `RowHeight` &ndash; ayarlar her satır yüksekliğini ne zaman `HasUnevenRows` olduğu `false`.

Tüm satırların yüksekliğini ayarlayarak ayarlayabileceğiniz `RowHeight` özellikte `ListView`.

### <a name="custom-fixed-row-height"></a>Özel sabit satır yüksekliği

C# İÇİN:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView sabit satır yüksekliği ile")


### <a name="uneven-rows"></a>Düzensiz satırlar

Ayrı satırlara farklı yüksekliklerini sahip olmasını isterseniz, ayarlayabileceğiniz `HasUnevenRows` özelliğini `true`.
Satır yüksekliklerini elle ayarlanmasına yüklü olmadığını unutmayın `HasUnevenRows` ayarlanmış `true`, yüksekliklerini Xamarin.Forms tarafından otomatik olarak hesaplanır.


C# İÇİN:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView düzensiz satırlarla")

### <a name="runtime-resizing-of-rows"></a>Çalışma zamanı satırlarını yeniden boyutlandırma

Tek tek `ListView` satırları sağlanan, çalışma zamanında programlı bir şekilde yeniden boyutlandırılabilir `HasUnevenRows` özelliği `true`. [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) Yöntemi bile, aşağıdaki kod örneğinde gösterildiği gibi şu anda görünür olmadığı durumlarda bir hücrenin boyutu güncelleştirir:

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

`OnImageTapped` Olay işleyicisi, yanıt olarak yürütülür bir [ `Image` ](xref:Xamarin.Forms.Image) hücrede dokunulduğunda ve boyutunu artırır `Image` hücrede görüntülenmesi kolayca görüntülenebilir.

![](customizing-list-appearance-images/dynamic-row-resizing.png "Çalışma zamanı satır yeniden boyutlandırılarak ListView")

Bu özellik aşırı kullanılmasına, güçlü bir performans düşüşü olasılığını olduğunu unutmayın.



## <a name="related-links"></a>İlgili bağlantılar

- [Gruplandırma (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Özel oluşturucu Görünüm (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Dinamik yeniden boyutlandırma, satır (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [1.4 sürüm notları](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 sürüm notları](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
