---
title: Bir ViewCell özelleştirme
description: Xamarin.Forms ViewCell ListView ya da bir geliştirici tarafından tanımlanan görünüm içeren tablo görünümü eklenebilir bir hücre olduğunda. Bu makalede, bir Xamarin.Forms ListView denetimi içinde barındırılan bir ViewCell için özel Oluşturucu Oluşturma gösterilir. Bu, engeller Xamarin.Forms düzeni hesaplamalar ListView kaydırma sırasında sürekli adlı durdurur.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 4d1d4323e42df6240fee7be42ae8fac70a2b3f1f
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="customizing-a-viewcell"></a>Bir ViewCell özelleştirme

_Xamarin.Forms ViewCell ListView ya da bir geliştirici tarafından tanımlanan görünüm içeren tablo görünümü eklenebilir bir hücre olduğunda. Bu makalede, bir Xamarin.Forms ListView denetimi içinde barındırılan bir ViewCell için özel Oluşturucu Oluşturma gösterilir. Bu, engeller Xamarin.Forms düzeni hesaplamalar ListView kaydırma sırasında sürekli adlı durdurur._

Her Xamarin.Forms hücrenin yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu yok. Zaman bir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) iOS içinde bir Xamarin.Forms uygulaması tarafından işlenen `ViewCellRenderer` sınıf örneği, hangi sırayla yerel başlatır `UITableViewCell` denetim. Android platformunda `ViewCellRenderer` sınıfı başlatır yerel `View` denetim. Üzerinde Evrensel Windows Platformu (UWP), `ViewCellRenderer` sınıfı başlatır yerel `DataTemplate`. Oluşturucu ve Xamarin.Forms denetimleri Eşle yerel denetim sınıfları hakkında daha fazla bilgi için bkz: [Oluşturucu taban sınıfları ve yerel denetimlere](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagram arasındaki ilişkiyi gösterir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) ve uyguladıktan karşılık gelen yerel denetimlere:

![](viewcell-images/viewcell-classes.png "ViewCell ve uygulama yerel denetimi arasındaki ilişki")

Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için avantajlarından alınabilir bir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Cell) Xamarin.Forms özel hücre.
1. [Tüketen](#Consuming_the_Custom_Cell) Xamarin.Forms özel hücreden.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) hücrede her platform için özel Oluşturucu.

Her öğe artık sırasıyla uygulamak için incelenecektir bir `NativeCell` Xamarin.Forms içinde barındırılan her bir hücre platforma özgü düzeni yararlanan Oluşturucu [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetim. Bu art arda sırasında çağrılan gelen Xamarin.Forms düzeni hesaplamalar durdurur `ListView` kaydırma.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Özel hücre oluşturma

Bir özel hücre denetimi sınıflara tarafından oluşturulabilir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public class NativeCell : ViewCell
{
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(NativeCell), "");

  public string Name {
    get { return (string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }

  public static readonly BindableProperty CategoryProperty =
    BindableProperty.Create ("Category", typeof(string), typeof(NativeCell), "");

  public string Category {
    get { return (string)GetValue (CategoryProperty); }
    set { SetValue (CategoryProperty, value); }
  }

  public static readonly BindableProperty ImageFilenameProperty =
    BindableProperty.Create ("ImageFilename", typeof(string), typeof(NativeCell), "");

  public string ImageFilename {
    get { return (string)GetValue (ImageFilenameProperty); }
    set { SetValue (ImageFilenameProperty, value); }
  }
}
```
`NativeCell` Sınıfı taşınabilir sınıf kitaplığı (PCL) projesinde oluşturulur ve özel hücre API'si tanımlar. Özel hücre sunan `Name`, `Category`, ve `ImageFilename` veri bağlama aracılığıyla görüntülenebilir özellikleri. Veri bağlama hakkında daha fazla bilgi için bkz: [veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Özel hücre kullanma

`NativeCell` Özel hücre başvurulabilir XAML'de PCL projesinde konumu için bir ad alanı bildirme ve özel hücre öğede ad alanı öneki kullanarak. Aşağıdaki örnekte gösterildiği kod nasıl `NativeCell` özel hücre XAML sayfası tarafından tüketilen:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Xamarin.Forms native cell" HorizontalTextAlignment="Center" />
            <ListView x:Name="listView" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <local:NativeCell Name="{Binding Name}" Category="{Binding Category}" ImageFilename="{Binding ImageFilename}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local` Ad alanı öneki adlı bir şey. Ancak, `clr-namespace` ve `assembly` değerlerin özel denetim ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde öneki özel hücre başvurusu için kullanılır.

Aşağıdaki örnekte gösterildiği kod nasıl `NativeCell` özel hücre bir C# sayfası tarafından tüketilen:

```csharp
public class NativeCellPageCS : ContentPage
{
    ListView listView;

    public NativeCellPageCS()
    {
        listView = new ListView(ListViewCachingStrategy.RecycleElement)
        {
            ItemsSource = DataSource.GetList(),
            ItemTemplate = new DataTemplate(() =>
            {
                var nativeCell = new NativeCell();
                nativeCell.SetBinding(NativeCell.NameProperty, "Name");
                nativeCell.SetBinding(NativeCell.CategoryProperty, "Category");
                nativeCell.SetBinding(NativeCell.ImageFilenameProperty, "ImageFilename");

                return nativeCell;
            })
        };

        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
                Padding = new Thickness(0, 20, 0, 0);
                break;
            case Device.Android:
            case Device.UWP:
                Padding = new Thickness(0);
                break;
        }

        Content = new StackLayout
        {
            Children = {
                new Label { Text = "Xamarin.Forms native cell", HorizontalTextAlignment = TextAlignment.Center },
                listView
            }
        };
        listView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Bir Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetim aracılığıyla doldurulmuş veri listesini görüntülemek için kullanılan [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/) özelliği. [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Stratejisi önbelleğe alma denemesi en aza indirmek `ListView` bellek alanını ve yürütme hızını liste hücreleri geri dönüştürülüyor. Daha fazla bilgi için bkz: [önbelleğe alma stratejisi](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Listedeki her bir satır veri – bir ad, kategori ve resim dosya adı üç öğeleri içerir. Listedeki her satırın düzenini tarafından tanımlanan `DataTemplate` aracılığıyla başvurulan [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/) bağlanabilirse özelliği. `DataTemplate` Her listesinde veri satırının olacağını tanımlayan bir `NativeCell` görüntüleyen kendi `Name`, `Category`, ve `ImageFilename` veri bağlama aracılığıyla özellikleri. Hakkında daha fazla bilgi için `ListView` denetlemek için bkz: [ListView](~/xamarin-forms/user-interface/listview/index.md).

Özel oluşturucu artık her hücre için platforma özel yerleşim özelleştirmek için her uygulama projesi eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Öğesinin bir alt kümesi oluşturmak `ViewCellRenderer` özel hücre işler sınıfı.
1. Özel hücre işleyen platforma özgü yöntemi geçersiz kılın ve özelleştirmek için mantığı yazma.
1. Ekleme bir `ExportRenderer` özniteliği, Xamarin.Forms özel hücre işlemek için kullanılacak belirtmek için özel Oluşturucu sınıfı. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Çoğu Xamarin.Forms öğeleri için her platform projesinde özel Oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse, varsayılan oluşturucu denetimin taban sınıfı için kullanılır. Ancak, özel Oluşturucu her platform projesinde işlenirken gereken bir [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) öğesi.

Aşağıdaki diyagram, her proje örnek uygulamasında, aralarındaki ilişkilerin birlikte sorumlulukları gösterir:

![](viewcell-images/solution-structure.png "NativeCell özel Oluşturucu Proje Sorumlulukları")

`NativeCell` Öğesinden türetilen tüm hangi platforma özgü Oluşturucu sınıflar tarafından işlenen özel hücre `ViewCellRenderer` her platform için sınıf. Bu her sonuçları `NativeCell` platforma özgü düzeniyle aşağıdaki ekran görüntülerinde gösterildiği gibi işlenen özel hücre:

![](viewcell-images/screenshots.png "Her platformda NativeCell")

`ViewCellRenderer` Sınıfı özel hücre işlemek için platforma özel yöntemler sunar. Bu `GetCell` iOS platformunda yöntemi `GetCellCore` Android platformunda yöntemi ve `GetTemplate` UWP yöntemi.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` Xamarin.Forms ile oluşturucuyu kaydeder özniteliği. Öznitelik iki parametre – işlenen Xamarin.Forms hücre tür adını ve özel Oluşturucu Tür adını alır. `assembly` Özniteliğine öneki belirtir. öznitelik tüm derlemesi için geçerlidir.

Aşağıdaki bölümlerde her platforma özgü özel Oluşturucu sınıfı uyarlamasını açıklanmaktadır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde iOS platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeiOSCellRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        NativeiOSCell cell;

        public override UITableViewCell GetCell(Cell item, UITableViewCell reusableCell, UITableView tv)
        {
            var nativeCell = (NativeCell)item;

            cell = reusableCell as NativeiOSCell;
            if (cell == null)
                cell = new NativeiOSCell(item.GetType().FullName, nativeCell);
            else
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;
            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCell` Yöntemi görüntülenecek her bir hücre oluşturmak için çağrılır. Her hücre bir `NativeiOSCell` hücre ve verilerini düzenini tanımlayan örnek. İşlemi `GetCell` yöntemdir bağımlı [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) stratejisi önbelleğe alma:

- Zaman [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) stratejisi önbelleğe alma [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCell` yöntemi her hücrenin çağrıldıktan. A `NativeiOSCell` örneği her biri için oluşturulacak `NativeCell` ilk ekranda görüntülenen örneği. Kullanıcı aracılığıyla kayarken `ListView`, `NativeiOSCell` örnekleri yeniden kullanılan olacaktır. İOS hücre yeniden kullanma hakkında daha fazla bilgi için bkz: [hücre yeniden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Bu özel Oluşturucu kodu bazı hücre yeniden kullanımını gerçekleştirecek bile [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) hücreleri korumak için ayarlanır.

  Her tarafından görüntülenen verileri `NativeiOSCell` örneği yeni oluşturulmuş ya da yeniden kullanılan güncelleştirilmeyecek her verilerle `NativeCell` tarafından örnek `UpdateCell` yöntemi.

  > [!NOTE]
  > `OnNativeCellPropertyChanged` Yöntemi hiçbir zaman olacaktır olduğunda çağrılan [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) stratejisi önbelleğe alma hücreleri korumak için ayarlanır.

- Zaman [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) stratejisi önbelleğe alma [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCell` yöntemi ilk ekranda görüntülenen her hücre için çağrılabilir. A `NativeiOSCell` örneği her biri için oluşturulacak `NativeCell` ilk ekranda görüntülenen örneği. Her tarafından görüntülenen verileri `NativeiOSCell` örnek verilerle güncelleştirilir `NativeCell` tarafından örnek `UpdateCell` yöntemi. Ancak, `GetCell` yöntemi kullanıcı kayarken olarak çağrılacak olmaz `ListView`. Bunun yerine, `NativeiOSCell` örnekleri yeniden kullanılan olacaktır. `PropertyChanged` olayları yükseltilmiş üzerinde `NativeCell` örnek verileri değiştiğinde ve `OnNativeCellPropertyChanged` olay işleyicisi verilerin güncelleştirilmesine neden olacak her yeniden kullanılan `NativeiOSCell` örneği.

Aşağıdaki örnekte gösterildiği kod `OnNativeCellPropertyChanged` ne zaman çağrılmış yöntemi bir `PropertyChanged` olayı oluşturulur:

```csharp
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingLabel.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingLabel.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.CellImageView.Image = cell.GetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Bu yöntem güncelleştirmeleri tarafından görüntülenen verileri yeniden kullanılan `NativeiOSCell` örnekleri. Birden çok kez yöntemi çağrılabilir olarak değiştirilir özelliği için bir denetimi yapılır.

`NativeiOSCell` Sınıfı her hücre düzenini tanımlar ve aşağıdaki kod örneğinde gösterilir:

```csharp
internal class NativeiOSCell : UITableViewCell, INativeElementView
{
  public UILabel HeadingLabel { get; set; }
  public UILabel SubheadingLabel { get; set; }
  public UIImageView CellImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeiOSCell(string cellId, NativeCell cell) : base(UITableViewCellStyle.Default, cellId)
  {
    NativeCell = cell;

    SelectionStyle = UITableViewCellSelectionStyle.Gray;
    ContentView.BackgroundColor = UIColor.FromRGB(255, 255, 224);
    CellImageView = new UIImageView();

    HeadingLabel = new UILabel()
    {
      Font = UIFont.FromName("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB(127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    SubheadingLabel = new UILabel()
    {
      Font = UIFont.FromName("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB(38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add(HeadingLabel);
    ContentView.Add(SubheadingLabel);
    ContentView.Add(CellImageView);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingLabel.Text = cell.Name;
    SubheadingLabel.Text = cell.Category;
    CellImageView.Image = GetImage(cell.ImageFilename);
  }

  public UIImage GetImage(string filename)
  {
    return (!string.IsNullOrWhiteSpace(filename)) ? UIImage.FromFile("Images/" + filename + ".jpg") : null;
  }

  public override void LayoutSubviews()
  {
    base.LayoutSubviews();

    HeadingLabel.Frame = new CGRect(5, 4, ContentView.Bounds.Width - 63, 25);
    SubheadingLabel.Frame = new CGRect(100, 18, 100, 20);
    CellImageView.Frame = new CGRect(ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Bu sınıf hücrenin içeriğinin ve bunların düzeni işlemek için kullanılan denetimleri tanımlar. Sınıf uygulayan [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) gerekli olduğunda arabirimi [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kullanan [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) stratejisi önbelleğe alma. Bu arabirim sınıfı uygulamalıdır belirtir [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) geri dönüştürüldüğünde hücreler için özel hücre veri döndürmelidir özelliği.

`NativeiOSCell` Oluşturucu görünümünü başlatır `HeadingLabel`, `SubheadingLabel`, ve `CellImageView` özellikleri. Bu özellikleri depolanan verileri görüntülemek için kullanılan `NativeCell` örneği ile `UpdateCell` her bir özellik değerini ayarlamak için çağrılan yöntem. Ayrıca, [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kullanan [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) stratejisi, tarafından görüntülenen verileri önbelleğe alma `HeadingLabel`, `SubheadingLabel`, ve `CellImageView` özellikleri olabilir güncelleyen `OnNativeCellPropertyChanged` özel Oluşturucu yöntemi.

Hücre düzeni tarafından gerçekleştirilir `LayoutSubviews` geçersiz kılmak, hangi koordinatlarını ayarlar `HeadingLabel`, `SubheadingLabel`, ve `CellImageView` hücre içinde.

### <a name="creating-the-custom-renderer-on-android"></a>Android özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde, Android platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeAndroidCellRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        NativeAndroidCell cell;

        protected override Android.Views.View GetCellCore(Cell item, Android.Views.View convertView, ViewGroup parent, Context context)
        {
            var nativeCell = (NativeCell)item;
            Console.WriteLine("\t\t" + nativeCell.Name);

            cell = convertView as NativeAndroidCell;
            if (cell == null)
            {
                cell = new NativeAndroidCell(context, nativeCell);
            }
            else
            {
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;
            }

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;

            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCellCore` Yöntemi görüntülenecek her bir hücre oluşturmak için çağrılır. Her hücre bir `NativeAndroidCell` hücre ve verilerini düzenini tanımlayan örnek. İşlemi `GetCellCore` yöntemdir bağımlı [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) stratejisi önbelleğe alma:

- Zaman [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) stratejisi önbelleğe alma [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCellCore` yöntemi her hücrenin çağrıldıktan. A `NativeAndroidCell` her biri için oluşturulan `NativeCell` ilk ekranda görüntülenen örneği. Kullanıcı aracılığıyla kayarken `ListView`, `NativeAndroidCell` örnekleri yeniden kullanılan olacaktır. Android hücre yeniden kullanma hakkında daha fazla bilgi için bkz: [satır görünümü yeniden kullanma](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Bu özel Oluşturucu kodu bazı hücre yeniden kullanımını gerçekleştirecek Not bile [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) hücreleri korumak için ayarlanır.

  Her tarafından görüntülenen verileri `NativeAndroidCell` örneği yeni oluşturulmuş ya da yeniden kullanılan güncelleştirilmeyecek her verilerle `NativeCell` tarafından örnek `UpdateCell` yöntemi.

  > [!NOTE]
  > Not Bu süre `OnNativeCellPropertyChanged` yöntemi olacaktır olduğunda çağrılan [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) olan değil güncelleştirmek hücreleri korumak için ayarlamak, `NativeAndroidCell` özellik değerleri.

- Zaman [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) stratejisi önbelleğe alma [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCellCore` yöntemi ilk ekranda görüntülenen her hücre için çağrılabilir. A `NativeAndroidCell` örneği her biri için oluşturulacak `NativeCell` ilk ekranda görüntülenen örneği. Her tarafından görüntülenen verileri `NativeAndroidCell` örnek verilerle güncelleştirilir `NativeCell` tarafından örnek `UpdateCell` yöntemi. Ancak, `GetCellCore` yöntemi kullanıcı kayarken olarak çağrılacak olmaz `ListView`. Bunun yerine, `NativeAndroidCell` örnekleri yeniden kullanılan olacaktır.  `PropertyChanged` olayları yükseltilmiş üzerinde `NativeCell` örnek verileri değiştiğinde ve `OnNativeCellPropertyChanged` olay işleyicisi verilerin güncelleştirilmesine neden olacak her yeniden kullanılan `NativeAndroidCell` örneği.

Aşağıdaki örnekte gösterildiği kod `OnNativeCellPropertyChanged` ne zaman çağrılmış yöntemi bir `PropertyChanged` olayı oluşturulur:

```csharp
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingTextView.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingTextView.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.SetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Bu yöntem güncelleştirmeleri tarafından görüntülenen verileri yeniden kullanılan `NativeAndroidCell` örnekleri. Birden çok kez yöntemi çağrılabilir olarak değiştirilir özelliği için bir denetimi yapılır.

`NativeAndroidCell` Sınıfı her hücre düzenini tanımlar ve aşağıdaki kod örneğinde gösterilir:

```csharp
internal class NativeAndroidCell : LinearLayout, INativeElementView
{
  public TextView HeadingTextView { get; set; }
  public TextView SubheadingTextView { get; set; }
  public ImageView ImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeAndroidCell(Context context, NativeCell cell) : base(context)
  {
    NativeCell = cell;

    var view = (context as Activity).LayoutInflater.Inflate(Resource.Layout.NativeAndroidCell, null);
    HeadingTextView = view.FindViewById<TextView>(Resource.Id.HeadingText);
    SubheadingTextView = view.FindViewById<TextView>(Resource.Id.SubheadingText);
    ImageView = view.FindViewById<ImageView>(Resource.Id.Image);

    AddView(view);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingTextView.Text = cell.Name;
    SubheadingTextView.Text = cell.Category;

    // Dispose of the old image
    if (ImageView.Drawable != null)
    {
      using (var image = ImageView.Drawable as BitmapDrawable)
      {
        if (image != null)
        {
          if (image.Bitmap != null)
          {
            image.Bitmap.Dispose();
          }
        }
      }
    }

    SetImage(cell.ImageFilename);
  }

  public void SetImage(string filename)
  {
    if (!string.IsNullOrWhiteSpace(filename))
    {
      // Display new image
      Context.Resources.GetBitmapAsync(filename).ContinueWith((t) =>
      {
        var bitmap = t.Result;
        if (bitmap != null)
        {
          ImageView.SetImageBitmap(bitmap);
          bitmap.Dispose();
        }
      }, TaskScheduler.FromCurrentSynchronizationContext());
    }
    else
    {
      // Clear the image
      ImageView.SetImageBitmap(null);
    }
  }
}
```

Bu sınıf hücrenin içeriğinin ve bunların düzeni işlemek için kullanılan denetimleri tanımlar. Sınıf uygulayan [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) gerekli olduğunda arabirimi [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kullanan [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) stratejisi önbelleğe alma. Bu arabirim sınıfı uygulamalıdır belirtir [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) geri dönüştürüldüğünde hücreler için özel hücre veri döndürmelidir özelliği.

`NativeAndroidCell` Oluşturucusu Şişir `NativeAndroidCell` düzeni ve başlatır `HeadingTextView`, `SubheadingTextView`, ve `ImageView` inflated Düzen denetimlerinde özellikleri. Bu özellikleri depolanan verileri görüntülemek için kullanılan `NativeCell` örneği ile `UpdateCell` her bir özellik değerini ayarlamak için çağrılan yöntem. Ayrıca, [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kullanan [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) stratejisi, tarafından görüntülenen verileri önbelleğe alma `HeadingTextView`, `SubheadingTextView`, ve `ImageView` özellikleri olabilir güncelleyen `OnNativeCellPropertyChanged` özel Oluşturucu yöntemi.

Aşağıdaki kod örneğinde düzeni tanımını gösterir `NativeAndroidCell.axml` Düzen dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="8dp"
    android:background="@drawable/CustomSelector">
    <LinearLayout
        android:id="@+id/Text"
        android:orientation="vertical"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingLeft="10dip">
        <TextView
            android:id="@+id/HeadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/SubheadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="14dip"
            android:textColor="#FF267F00"
            android:paddingLeft="100dip" />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout>
```

Bu iki bu düzeni belirtir `TextView` kontrol eder ve bir `ImageView` denetim hücrenin içeriği görüntülemek için kullanılır. İki `TextView` denetimleri içinde dikey yönelimli bir `LinearLayout` denetimiyle içinde bulunan tüm denetimler bir `RelativeLayout`.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP üzerinde özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde UWP için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeUWPCellRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

`GetTemplate` Yöntemi listesindeki verileri her satır için işlenmek üzere hücre geri dönmek için çağrılır. Oluşturduğu bir `DataTemplate` her `NativeCell` ekranda ile görüntülenen örnek `DataTemplate` hücre içeriğini ve görünümü tanımlama.

`DataTemplate` Uygulama düzeyi kaynak sözlükte depolanır ve aşağıdaki kod örneğinde gösterilir:

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="LightYellow">
        <Grid.Resources>
            <local:ConcatImageExtensionConverter x:Name="ConcatImageExtensionConverter" />
        </Grid.Resources>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.40*" />
            <ColumnDefinition Width="0.40*"/>
            <ColumnDefinition Width="0.20*" />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.ColumnSpan="2" Foreground="#7F3300" FontStyle="Italic" FontSize="22" VerticalAlignment="Top" Text="{Binding Name}" />
        <TextBlock Grid.RowSpan="2" Grid.Column="1" Foreground="#267F00" FontWeight="Bold" FontSize="12" VerticalAlignment="Bottom" Text="{Binding Category}" />
        <Image Grid.RowSpan="2" Grid.Column="2" HorizontalAlignment="Left" VerticalAlignment="Center" Source="{Binding ImageFilename, Converter={StaticResource ConcatImageExtensionConverter}}" Width="50" Height="50" />
        <Line Grid.Row="1" Grid.ColumnSpan="3" X1="0" X2="1" Margin="30,20,0,0" StrokeThickness="1" Stroke="LightGray" Stretch="Fill" VerticalAlignment="Bottom" />
    </Grid>
</DataTemplate>
```

`DataTemplate` Hücre ve düzeni ve görünüm içeriğini görüntülemek için kullanılan denetimleri belirtir. İki `TextBlock` kontrol eder ve bir `Image` denetimi, veri bağlama aracılığıyla hücrenin içeriği görüntülemek için kullanılır. Ayrıca, bir örneğini `ConcatImageExtensionConverter` birleştirmek için kullanılan `.jpg` dosya her görüntü dosya adı uzantısı. Bu sağlar `Image` denetim yüklemek ve olduğunda görüntü işleme `Source` özelliği ayarlanmış.

## <a name="summary"></a>Özet

Bu makalede için özel Oluşturucu Oluşturma gösterilmiştir bir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) Xamarin.Forms içinde barındırılan [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetim. Bu art arda sırasında çağrılan gelen Xamarin.Forms düzeni hesaplamalar durdurur `ListView` kaydırma.


## <a name="related-links"></a>İlgili bağlantılar

- [ListView performansı](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
