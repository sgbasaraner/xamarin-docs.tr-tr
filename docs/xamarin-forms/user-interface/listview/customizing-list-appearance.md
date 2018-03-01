---
title: "Liste Görünümü"
description: "Üstbilgiler, altbilgiler, gruplar ve değişken yükseklikli hücreleri kullanarak ListViews özelleştirin."
ms.topic: article
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: c828a48342fc1b387dab2884dbb4aa5d82faebdd
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="list-appearance"></a>Liste Görünümü

`ListView` arka plandaki yanı sıra genel listesinin sunu denetleme seçenekleri sahip `ViewCell`s. Seçenekler şunlardır:

- [**Gruplandırma** ](#Grouping) &ndash; Grup ListView öğeleri daha kolay gezinme ve geliştirilmiş kuruluş için.
- [**Üstbilgiler ve altbilgiler** ](#Headers_and_Footers) &ndash; başında ve diğer öğeleri ile birlikte kayar görünümü sonuna bilgileri görüntüler.
- [**Satır ayırıcı** ](#Row_Separators) &ndash; öğeleri arasında ayırıcı çizgileri göstermek veya gizlemek.
- [**Değişken yükseklik satırları** ](#Row_Heights) &ndash; varsayılan olarak tüm satırların aynı yükseklikte olsa da bu görüntülenecek farklı yükseklikte satırlarla izin verecek şekilde değiştirilebilir.

<a name="Grouping" />

## <a name="grouping"></a>Gruplandırma
Genellikle, büyük veri kümelerine yönelik bir sürekli kaydırma listesinde sunulduğunda yönetilmeleri zorlaşabilir. Etkinleştirme gruplandırma bu durumda kullanıcı deneyimini daha iyi içeriği düzenleme ve gezinme veri kolaylaştırmak platforma özgü denetimleri etkinleştirme artırabilir.

Ne zaman gruplandırma etkinleştirilirse için bir `ListView`, her grup için bir başlık satırı eklenir.

Gruplandırma etkinleştirmek için:

- Liste (gruplarının bir listesini, bir öğe listesi olan her bir grubu) listesini oluşturun.
- Ayarlama `ListView`'s `ItemsSource` bu listeye.
- Ayarlama `IsGroupingEnabled` true.
- Ayarlama [ `GroupDisplayBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) grup başlığı olarak kullanılan özellik gruplarının bağlamak için.
- [İsteğe bağlı] Ayarlama [ `GroupShortNameBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) grubu için kısa bir ad olarak kullanılan özellik gruplarının bağlamak için. Kısa ad bağlantı listeleri (iOS, Windows Phone üzerinde döşeme rigt tarafı sütun) için kullanılır.

Grupları için bir sınıf oluşturarak başlayın:

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

Yukarıdaki kod `All` bizim ListView bağlama kaynağı olarak için verilen listesidir. `Title` ve `ShortName` Grup başlıkları için kullanılacak olan özelliklerdir.

Bu aşamada `All` boş bir listedir. Böylece listenin program başlangıcında doldurulur statik bir oluşturucu ekleyin:

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

Yukarıdaki kod biz de çağırabilirsiniz `Add` öğeleri üzerinde `groups`, türü örnekleri olan `PageTypeGroup`. Bu olasıdır çünkü `PageTypeGroup` devraldığı `List<PageModel>`. Bu, yukarıda belirtilen listelerinde desen listesi örneğidir.

Gruplandırılmış listesini görüntülemek için XAML şöyledir:

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

Bizim Not:

- Ayarlama `GroupShortNameBinding` için `ShortName` bizim Grup sınıfında tanımlanan özelliği
- Ayarlama `GroupDisplayBinding` için `Title` bizim Grup sınıfında tanımlanan özelliği
- Ayarlama `IsGroupingEnabled` true
- Değiştirilen `ListView`'s `ItemsSource` gruplanmış listesi

### <a name="customizing-grouping"></a>Gruplandırma özelleştirme
ListView içinde gruplandırma temel uygulamak nasıl gördük, grup üstbilgileri görüntüsünü özelleştirmek nasıl görelim.

Benzer şekilde nasıl `ListView` sahip bir `ItemTemplate` satırları görüntülenme tanımlamak için `ListView` sahip bir `GroupHeaderTemplate`. Bu, yukarıda, özel bir grup üstbilgisi şablonuyla ListView örneğidir:

![](customizing-list-appearance-images/grouping-depth.png "ListView özelleştirilmiş GroupHeaderTemplate ile")


Bu tasarım XAML'de nasıl şöyledir:

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
            <!-- End Group Header Customization
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>Üstbilgiler ve Altbilgiler
Bir kaydırma üstbilgi ve altbilgi listenin öğelerini sunmak ListView mümkündür. Üstbilgi ve altbilgi dizeler metin veya daha karmaşık bir düzen olabilir. Bu ayrı olduğuna dikkat edin [bölüm grupları](#Grouping).

Ayarlayabileceğiniz `Header` ve/veya `Footer` basit bir dize değeri veya bunları daha karmaşık bir düzene ayarlayabilirsiniz.
Ayrıca `HeaderTemplate` ve `FooterTemplate` olanak tanıyan özellikler bu destek veri bağlama üstbilgi ve altbilgi için daha karmaşık düzenleri oluşturma.

Basit bir üstbilgi/altbilgi oluşturmak için yalnızca görüntülemek istediğiniz metin üstbilgisinde veya altbilgisinde özellikleri ayarlayın. Kod:

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

XAML'de:

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
Ayırıcı satırlar arasında görüntülenir `ListView` öğeleri varsayılan olarak iOS ve Android. Windows Phone bu platformları UX yönergeleri başına ayırıcı satırları desteklemez. İOS ve Android ayırıcı satırlarında gizlemek tercih ediyorsanız, ayarlamak `SeparatorVisibility` , ListView özelliği. Seçeneklerini `SeparatorVisibility` şunlardır:

* **Varsayılan** -iOS ve Android cihazlarda bir ayırıcı satır gösterir.
* **Hiçbiri** -tüm platformlarda ayırıcı gizler.

Varsayılan görünürlük:

C# ' TA:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "Varsayılan satır Ayırıcılı ListView")

Yok:

C# ' TA:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView satır ayırıcı olmadan")

Ayırıcı çizginin rengini de ayarlayabilirsiniz `SeparatorColor` özelliği:

C# ' TA:

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView yeşil satır Ayırıcılı")

**Not**: Bu özelliklerden herhangi birini yüklemeden sonra Android ayarı `ListView` büyük performans cezası doğurur.

<a name="Row_Heights" />

## <a name="row-heights"></a>Satır yükseklik
ListView tüm satırlarda, varsayılan olarak aynı yüksekliğe sahip. ListView bu davranışı değiştirmek için kullanılan iki özelliklere sahiptir:

- `HasUnevenRows` &ndash; `true`/`false` değer, satır varsa değişen yükseklikte kümesine `true`. Varsayılan olarak `false`.
- `RowHeight` &ndash; ayarlar her satır yüksekliğini ne zaman `HasUnevenRows` olan `false`.

Tüm satırların yüksekliğini ayarlayarak ayarlayabileceğiniz `RowHeight` özelliği `ListView`.

### <a name="custom-fixed-row-height"></a>Özel sabit satır yüksekliği

C# ' TA:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView sabit satır yüksekliği ile")


### <a name="uneven-rows"></a>Düzensiz Satırları

Tek tek satır farklı yükseklikte olmasını istiyorsanız, ayarlayabileceğiniz `HasUnevenRows` özelliğine `true`.
Satır yükseklikte el ile ayarlanmasına sahip olmadığına dikkat edin `HasUnevenRows` ayarlandığından `true`, yükseklik Xamarin.Forms tarafından otomatik olarak hesaplanır.


C# ' TA:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView düzensiz satırlarla")

### <a name="runtime-resizing-of-rows"></a>Çalışma zamanı satırları yeniden boyutlandırma

Tek tek `ListView` satır, sağlanan çalışma zamanında program aracılığıyla yeniden boyutlandırılabilir `HasUnevenRows` özelliği ayarlanmış `true`. [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) Bile, aşağıdaki kod örneğinde gösterildiği gibi şu anda görünür değilse yöntemi güncelleştirmeleri bir hücrenin boyutu:

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

`OnImageTapped` Olay işleyicisi yanıt olarak yürütüldüğünde bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) hücrede dokunduğunuz ve boyutu artar `Image` hücrede görüntülenmesi kolayca görüntülenebilir.

![](customizing-list-appearance-images/dynamic-row-resizing.png "Çalışma zamanı satır yeniden boyutlandırma ile ListView")

Bu özellik aşırı kullanılmasına, güçlü bir performans düşüşü olasılığını olduğunu unutmayın.



## <a name="related-links"></a>İlgili bağlantılar

- [Gruplandırma (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Özel oluşturucu görünümü (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Dinamik yeniden boyutlandırma, satır (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [1.4 sürüm notları](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 sürüm notları](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
