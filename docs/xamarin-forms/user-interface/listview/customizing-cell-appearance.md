---
title: Hücre görünümü
description: ListView kolaylık yararlanarak sırasında verileri sunmak için seçeneklerini araştırın.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: df0e113f0c76ea9bde58da7a7ceccd50edd5b227
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="cell-appearance"></a>Hücre görünümü

ListView sunar kullanılarak özelleştirilebilir kaydırılabilir listeleri `ViewCell`s. `ViewCells` metin ve resimler görüntülemek, bir doğru/yanlış durumunu gösteren ve kullanıcı girişi almak için kullanılabilir.

ListView hücrelerden istediğiniz görünümü almak için iki yaklaşım vardır:

- **[Yerleşik hücreleri özelleştirme](#Built_in_Cells)**  &ndash; daha kolay ve daha iyi performans özelleştirilebilirliğini ödün verme pahasına.
- **[Özel hücreleri oluşturma](#customcells)**  &ndash; daha kontrol Nihai sonuç, ancak doğru uygulanmadı olası performans sorunları varsa.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Yerleşik hücreleri
Birçok basit uygulamalar için çalışma yerleşik hücrelerle Xamarin.Forms gelir:

- **TextCell** &ndash; metin görüntüleme
- **ImageCell** &ndash; metinle birlikte bir görüntü görüntüleme.

İki ek hücreleri [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) ve [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) bunlar ile yaygın olarak kullanılmayan ancak kullanılabilir `ListView`. Bkz: [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) bu hücreler hakkında daha fazla bilgi.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) İkinci satır ayrıntı metin olarak isteğe bağlı olarak metin görüntüleme için bir hücre olduğunda.

TextCells işlenir yerel denetimlerini çalışma zamanında olarak özel bir kıyasla performans yararlı olacak şekilde `ViewCell`. TextCells ayarlamanıza olanak veren özelleştirilebilir şunlardır:

- `Text` &ndash; İlk satırdaki büyük yazı tipiyle gösterilen metin.
- `Detail` &ndash; ilk satırda daha küçük bir yazı tipi altında görüntülenen metin.
- `TextColor` &ndash; metin rengi.
- `DetailColor` &ndash; Ayrıntı metin rengi

![](customizing-cell-appearance-images/text-cell-default.png "Varsayılan TextCell örneği")

![](customizing-cell-appearance-images/text-cell-custom.png "Özelleştirilmiş TextCell örneği")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), gibi `TextCell`ve ikincil ayrıntı metin görüntülemek için kullanılabilir ve her platformun yerel denetimlerini kullanarak iyi performans sunar. `ImageCell` farklı `TextCell` içeren bir görüntü metni sola görüntüler.

`ImageCell` Kişiler veya filmler listesi gibi bir görsel yönü verilerle listesini görüntülemek gerektiğinde kullanışlıdır. ImageCells ayarlamanıza olanak veren özelleştirilebilir şunlardır:

- `Text` &ndash; İlk satırdaki büyük yazı tipiyle gösterilen metni
- `Detail` &ndash; ilk satırda daha küçük bir yazı tipi altında görüntülenen metin
- `TextColor` &ndash; metin rengi
- `DetailColor` &ndash; Ayrıntı metin rengi
- `ImageSource` &ndash; metnin yanında görüntülenecek resmi

Windows Phone 8.1, hedeflerken unutmayın `ImageCell` görüntüleri varsayılan olarak ölçekleme sağlamaz. Ayrıca, Windows Phone 8.1 hangi ayrıntılı metin sunulan tek platformdur aynı renk ve yazı tipi varsayılan olarak birincil metin olarak olduğuna dikkat edin. Windows Phone 8.0 işler `ImageCell` aşağıda görüldüğü gibi:

![](customizing-cell-appearance-images/image-cell-default.png "Varsayılan ImageCell örneği")

![](customizing-cell-appearance-images/image-cell-custom.png "Özelleştirilmiş ImageCell örneği")

<a name="customcells" />

## <a name="custom-cells"></a>Özel hücreler
Yerleşik hücreleri gerekli düzeni sağlamıyorsa, özel hücreleri gerekli düzeni uygulanır. Örneğin, bir hücre eşit ağırlığa sahip iki etiketlerle sunmak isteyebilirsiniz. A `LabelCell` yetersiz olabilir çünkü `LabelCell` küçük olan bir etiketi yok. Çoğu hücre özelleştirmeleri ek salt okunur verileri (örneğin, ek etiketleri, görüntüleri veya başka görüntü bilgilerini) ekleyin.

Tüm özel hücreleri öğesinden türetilmelidir [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), tüm yerleşik hücrenin yazdığı kullanım aynı temel sınıfı.

Xamarin.Forms 2 sunulan yeni bir [önbelleğe alma davranışı](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) üzerinde `ListView` bazı türleri için özel bir hücre kaydırma performansını geliştirmek için ayarlayabileceğiniz denetim.

Bu, özel bir hücre örneğidir:

![](customizing-cell-appearance-images/custom-cell.png "Özel hücre örneği")

### <a name="xaml"></a>XAML
Yukarıdaki düzeni oluşturmak için XAML aşağıdadır:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Yukarıdaki XAML çok yapıyor. Şimdi onu Bölünme:

- Özel hücre içinde iç içe bir `DataTemplate`, içinde olduğu `ListView.ItemTemplate`. Bu, herhangi bir hücreyi kullanmakla aynı işlemdir.
- `ViewCell` Özel hücre türüdür. Alt `DataTemplate` öğesi olması veya türden türetilmiş `ViewCell`.
- Bu iç fark `ViewCell`, düzeni tarafından yönetilen bir `StackLayout`. Bu düzen bize arka plan rengini özelleştirmenizi sağlar. Unutmayın herhangi bir özelliği `StackLayout` diğer bir deyişle bağlanabilirse can bağlı özel bir hücreyi burada gösterilmese de.

### <a name="cnum"></a>C&num;

Özel bir hücre C# ' ta belirtme, XAML eşdeğer biraz daha ayrıntılıdır. Bir göz atalım:

İlk olarak, bir özel hücre sınıf ile tanımlama `ViewCell` temel sınıf olarak:

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

Sayfa ile birlikte, Oluşturucuda `ListView`, ListView's ayarlamak `ItemTemplate` yeni bir özellik `DataTemplate`:

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

Unutmayın Oluşturucusu `DataTemplate` bir türü alır. Typeof işleci için CLR türünü alır `CustomCell`.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>Bağlam değişiklikleri bağlama

Bir özel hücre türüne ait bağlama sırasında [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) görüntüleme UI denetimleri örneği `BindableProperty` değerleri kullanması gereken [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) görüntülenecek veri kümesi için geçersiz kılın Her bir hücre yerine aşağıdaki kod örneğinde gösterildiği gibi hücre oluşturucusu:

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null) {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

[ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) Geçersiz kılma ne zaman çağrılır [ `BindingContextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.BindingContextChanged/) yanıt değeri olarak olay ateşlenir [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) özelliğini değiştirmek. Bu nedenle, `BindingContext` değişiklikler, kullanıcı Arabirimi denetimlerini görüntüleme [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) değerleri verilerini ayarlanmış olmalıdır. Unutmayın `BindingContext` için denetlenmesi bir `null` Xamarin.Forms tarafından bu sırayla sonuçlanır çöp toplama için ayarlanabilir, değer `OnBindingContextChanged` çağrılan geçersiz kılar.

Alternatif olarak, kullanıcı Arabirimi denetimlerini bağlayabilirsiniz [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) geçersiz kılmak için gereksinimini ortadan kaldırır değerlerini görüntülemek için örnekler `OnBindingContextChanged` yöntemi.

> [!NOTE]
> Geçersiz kılarken `OnBindingContextChanged`, temel sınıfın emin `OnBindingContextChanged` kayıtlı temsilciler alması için bunları yöntemi çağrıldığında `BindingContextChanged` olay.

XAML içinde veri bağlama özel hücre türü aşağıdaki kod örneğinde gösterildiği gibi elde edilebilir:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Bu bağlar `Name`, `Age`, ve `Location` bağlanabilirse özelliklerinde `CustomCell` örneği, çok `Name`, `Age`, ve `Location` temel koleksiyondaki her nesnenin özelliklerini.

C# eşdeğer bağlama aşağıdaki kod örneğinde gösterilir:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView {
    ItemsSource = people,
    ItemTemplate = customCell
};
```

İOS ve Android, varsa [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) öğeleri geri dönüştürme ve özel oluşturucu özel hücre kullanır, özel Oluşturucu doğru özellik değişikliği bildirimi uygulamalıdır. Hücreleri yeniden yapıldığında bağlama bağlamı, kullanılabilir bir hücre ile güncelleştirildiğinde özellik değerlerine değişir `PropertyChanged` gerçekleştirilen olaylarının. Daha fazla bilgi için bkz: [bir ViewCell özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Hücre geri dönüştürme hakkında daha fazla bilgi için bkz: [önbelleğe alma stratejisi](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

## <a name="related-links"></a>İlgili bağlantılar

- [Hücreleri (örnek) oluşturulmuş](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Özel hücreleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Bağlama bağlam değiştirildi (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
