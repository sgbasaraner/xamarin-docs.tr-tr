---
title: ListView hücresi görünümünü özelleştirme
description: Bu makalede ListView denetimi kolaylık yararlanarak çalışırken Xamarin.Forms uygulamalarında verileri sunmak için seçenekler ele alınmıştır.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 7a0f55b6d8a61f52f4ef137d83c56d86149bc3c9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996262"
---
# <a name="customizing-listview-cell-appearance"></a>ListView hücresi görünümünü özelleştirme

ListView sunan kullanımının özelleştirilebilir kaydırılabilir listeleri `ViewCell`s. `ViewCells` metin ve resim görüntüleme, doğru/yanlış durumunu gösteren ve kullanıcı girişi almak için kullanılabilir.

ListView hücrelerden istediğiniz görüntüyü almak için iki yaklaşım vardır:

- **[Yerleşik hücreleri özelleştirme](#Built_in_Cells)**  &ndash; daha kolay ve daha iyi performans küçültülür konularında kendini gösterir.
- **[Özel hücreleri oluşturma](#customcells)**  &ndash; daha sonuç üzerinde denetim, ancak doğru uygulanmadı olası performans sorunları vardır.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Yerleşik hücreleri
Xamarin.Forms, pek çok basit uygulamalar için çalışma yerleşik hücre ile birlikte gelir:

- **TextCell** &ndash; metin görüntüleme
- **ImageCell** &ndash; metin içeren bir resim görüntülemek için.

İki ek hücre [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) ve [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) bunlar ile yaygın olarak kullanılmayan ancak kullanılabilir `ListView`. Bkz: [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) bu hücreleri hakkında daha fazla bilgi.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) İsteğe bağlı olarak ayrıntılı metin olarak ikinci bir satır içeren bir metin görüntülemek için bir hücre var.

TextCells işlenir yerel denetimlerini çalışma zamanında, bir özel kıyasla performans çok iyi, bu nedenle `ViewCell`. TextCells özelleştirilebilir ayarlamanızı izin verme:

- `Text` &ndash; ilk satırında, büyük yazı tipiyle gösterilen metin.
- `Detail` &ndash; ilk satırı, küçük bir yazı tipi altında gösterilen metin.
- `TextColor` &ndash; metin rengi.
- `DetailColor` &ndash; Ayrıntı metin rengi

![](customizing-cell-appearance-images/text-cell-default.png "Varsayılan TextCell örneği")

![](customizing-cell-appearance-images/text-cell-custom.png "Özelleştirilmiş TextCell örneği")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell), gibi `TextCell`ikincil ayrıntı metin görüntülemek için kullanılabilir ve her platformun yerel denetimlerini kullanarak mükemmel performans sunar. `ImageCell` farklıdır `TextCell` , metnin solunda bir resim görüntüler.

`ImageCell` verilerle ilgili kişiler veya filmler listesi gibi görsel bir özelliği bir listesini görüntülemek, ihtiyacınız olduğunda yararlıdır. ImageCells özelleştirilebilir ayarlamanızı izin verme:

- `Text` &ndash; ilk satırında, büyük yazı tipiyle gösterilen metin
- `Detail` &ndash; ilk satırı, küçük bir yazı tipi altında gösterilen metin
- `TextColor` &ndash; metin rengi
- `DetailColor` &ndash; Ayrıntı metin rengi
- `ImageSource` &ndash; metnin yanında gösterilecek resmi

![](customizing-cell-appearance-images/image-cell-default.png "Varsayılan ImageCell örneği")

![](customizing-cell-appearance-images/image-cell-custom.png "Özelleştirilmiş ImageCell örneği")

<a name="customcells" />

## <a name="custom-cells"></a>Özel hücreleri
Yerleşik hücreleri gerekli düzeni gerektiğinde, özel hücreleri gerekli düzeni uygulanır. Örneğin, bir hücre eşit ağırlık sahip iki etiket ile sunmak isteyebilirsiniz. A `TextCell` yetersiz olabilir çünkü `TextCell` daha küçük olan bir etiketi vardır. Çoğu hücre özelleştirmeleri ek salt okunur verileri (örneğin, ilave etiketler, görüntüleri veya başka görüntü bilgilerini) ekleyin.

Tüm özel hücreleri öğesinden türetilmelidir [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), aynı temel sınıf tüm yerleşik hücresi türlerini kullanın.

Xamarin.Forms 2 tanıtılan yeni bir [önbelleğe alma davranışını](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) üzerinde `ListView` özel hücre bazı türleri için kayma performansını artırma için ayarlanan denetimi.

Bu, özel bir hücre örneğidir:

![](customizing-cell-appearance-images/custom-cell.png "Özel hücre örneği")

### <a name="xaml"></a>XAML
Yukarıdaki bir düzen oluşturmak için XAML aşağıda verilmiştir:

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

Yukarıdaki XAML çok yapıyor. Bu konuyu biraz açalım:

- Özel hücresi içinde iç içe bir `DataTemplate`, içinde olduğu `ListView.ItemTemplate`. Bu, herhangi bir hücreyi kullanmakla aynı işlemdir.
- `ViewCell` Özel hücre türüdür. Alt `DataTemplate` öğesi olması veya türünden türetilmesi gerekir `ViewCell`.
- Bu iç fark `ViewCell`, düzen tarafından yönetilen bir `StackLayout`. Bu düzen arka plan rengini özelleştirebilirsiniz olanak sağlıyor. Unutmayın, herhangi bir özelliği `StackLayout` diğer bir deyişle bağlanabilir can bağımlı içinde özel bir hücre, ancak burada gösterilmez.

### <a name="cnum"></a>C&num;

Özel bir hücre C# dilinde belirtme, XAML eşdeğer biraz daha ayrıntılıdır. Bir göz atalım:

İle ilk olarak, bir özel hücre sınıfı tanımlamanız `ViewCell` temel sınıf olarak:

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

Sayfası için bir Oluşturucuda `ListView`, ListView'ın ayarlamak `ItemTemplate` yeni bir özellik `DataTemplate`:

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

Bir özel hücre türün için bağlama sırasında [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) görüntüleme UI denetimleri örneği `BindableProperty` değerleri kullanması gereken [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) görüntülenecek veri kümesi için geçersiz kılma Her bir hücre, aşağıdaki kod örneğinde gösterildiği gibi hücre oluşturucusu yerine:

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

[ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) Geçersiz kılma ne zaman çağrılacağı [ `BindingContextChanged` ](xref:Xamarin.Forms.BindableObject.BindingContextChanged) yanıt değeri olarak olay harekete geçirilir [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) özelliğini değiştirme. Bu nedenle, `BindingContext` değiştirir, UI denetimleri görüntüleme [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) değerleri, kendi veri ayarlamalıdır. Unutmayın `BindingContext` için denetlenmesi bir `null` değer sırayla sonuçlanır çöp toplama için Xamarin.Forms tarafından bu ayarlanabilir gibi `OnBindingContextChanged` çağrılan geçersiz kılar.

Alternatif olarak, kullanıcı Arabirimi denetimleri adlarınıza bağlayabileceğiniz [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) geçersiz kılmak için ihtiyacını ortadan kaldırır, değerleri görüntülemek için örnekleri `OnBindingContextChanged` yöntemi.

> [!NOTE]
> Geçersiz kılarken `OnBindingContextChanged`, temel sınıfın emin `OnBindingContextChanged` kayıtlı temsilcilerin alması için bunları yöntemi çağrıldığında `BindingContextChanged` olay.

XAML içinde özel hücresi türü için veri bağlama aşağıdaki kod örneğinde gösterildiği gibi gerçekleştirilebilir:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Bu bağlar `Name`, `Age`, ve `Location` bağlanabilir özelliklerinde `CustomCell` için örnek `Name`, `Age`, ve `Location` temel alınan bir koleksiyondaki her bir nesnenin özellikleri.

C# ' de eşdeğer bağlama aşağıdaki kod örneğinde gösterilmiştir:

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

İOS ve Android üzerinde olursa [ `ListView` ](xref:Xamarin.Forms.ListView) öğeleri geri dönüştürme ve özel oluşturucu özel hücre kullanır, özel Oluşturucu, özellik değişikliği bildirimi doğru uygulamalıdır. Hücreleri yeniden yapılırken özellik değerlerine bağlama bağlamı, kullanılabilir bir hücre ile güncelleştirildiğinde değişir `PropertyChanged` gerçekleştirilen olaylarının. Daha fazla bilgi için [bir Viewcell'i özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Hücre geri dönüştürme hakkında daha fazla bilgi için bkz. [önbelleğe alma stratejisi](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

## <a name="related-links"></a>İlgili bağlantılar

- [Yerleşik hücreleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Özel hücreleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Bağlama bağlamı değiştirildi (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
