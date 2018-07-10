---
title: Bir Viewcell'i özelleştirme
description: Xamarin.Forms Viewcell'i ListView ya da bir geliştirici tarafından tanımlanan görünüm içeren tablo görünümü eklenebilir bir hücre ' dir. Bu makalede, bir Xamarin.Forms ListView denetimi içinde barındırılan bir Viewcell'i için özel Oluşturucu oluşturma işlemini gösterir.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 3cb4d7f152e0f9540275f12f0ade568cd0552784
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935582"
---
# <a name="customizing-a-viewcell"></a>Bir Viewcell'i özelleştirme

_Xamarin.Forms Viewcell'i ListView ya da bir geliştirici tarafından tanımlanan görünüm içeren tablo görünümü eklenebilir bir hücre ' dir. Bu makalede, bir Xamarin.Forms ListView denetimi içinde barındırılan bir Viewcell'i için özel Oluşturucu oluşturma işlemini gösterir. ListView kaydırma sırasında sürekli olarak adlandırılan bu engeller Xamarin.Forms Düzen hesaplamalar durdurur._

Tüm Xamarin.Forms hücrenin yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici yok. Olduğunda bir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) bir Xamarin.Forms uygulaması iOS tarafından işlenen `ViewCellRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `UITableViewCell` denetimi. Android platformunda `ViewCellRenderer` sınıfın örneğini oluşturur, bir yerel `View` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `ViewCellRenderer` sınıfın örneğini oluşturur, bir yerel `DataTemplate`. Oluşturucu ve Xamarin.Forms denetimleri eşleyen yerel denetim sınıfları hakkında daha fazla bilgi için bkz. [oluşturucu temel sınıfları ve yerel denetimleri](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagramda arasındaki ilişkiyi gösterir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) ve onu uygulayan karşılık gelen yerel denetimler:

![](viewcell-images/viewcell-classes.png "Viewcell'i ve uygulama yerel denetimi arasındaki ilişki")

İşleme sürecini avantajlarından platforma özgü özelleştirmeler için özel Oluşturucu oluşturarak uygulamak için gerçekleştirilebilecek bir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Cell) Xamarin.Forms özel hücre.
1. [Tüketen](#Consuming_the_Custom_Cell) Xamarin.Forms özel hücreden.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) her platformda hücre için özel Oluşturucu.

Her öğe artık sırayla uygulanacağı açıklanmıştır bir `NativeCell` bir Xamarin.Forms içinde barındırılan her hücre için bir platforma özel düzen yararlanır Oluşturucu [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetimi. Bu Xamarin.Forms Düzen hesaplama sırasında sürekli çağrılmasını durdurur `ListView` kaydırma.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Özel hücresi oluşturmak

Sınıflara göre özel hücre denetimi oluşturulabilir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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
`NativeCell` Sınıfı .NET Standard kitaplığı projesinde oluşturulur ve API'si için özel hücre tanımlar. Özel hücresi `Name`, `Category`, ve `ImageFilename` özellikleri, veri bağlama aracılığıyla görüntülenebilir. Veri bağlama hakkında daha fazla bilgi için bkz. [temel veri bağlama bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Özel hücre kullanma

`NativeCell` Özel hücre başvurulabilir Xaml içinde .NET Standard kitaplığı projesinde konumu için bir ad alanı bildirmek ve özel hücre öğesinde ad alanı öneki kullanarak. Aşağıdaki kod örnekte gösterildiği nasıl `NativeCell` özel hücre bir XAML sayfası tarafından kullanılabilir:

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

`local` Ad alanı ön eki adı herhangi bir şey. Ancak, `clr-namespace` ve `assembly` değerleri, özel denetimin ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde, ön ek özel hücre başvurusu için kullanılır.

Aşağıdaki kod örnekte gösterildiği nasıl `NativeCell` özel hücre bir C# sayfası tarafından kullanılabilir:

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

Bir Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) denetimi ile doldurulmuş veri listesini görüntülemek için kullanılan [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/) özelliği. [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) En aza indirmek önbelleğe alma stratejisi çalışır `ListView` bellek Ayak izi ve yürütme hızını liste hücreleri dönüştürerek. Daha fazla bilgi için [önbelleğe alma stratejisi](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Listedeki her bir satır veri – bir ad, kategori ve bir görüntü dosya adı üç öğelerini içerir. Listedeki her bir satırın düzenini tanımlayan `DataTemplate` aracılığıyla başvurulan [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/) bağlanılabilir özellik. `DataTemplate` Listesinde veri satırlarının olacağını tanımlayan bir `NativeCell` görüntüleyen kendi `Name`, `Category`, ve `ImageFilename` veri bağlama aracılığıyla özellikleri. Hakkında daha fazla bilgi için `ListView` denetlemek için bkz: [ListView](~/xamarin-forms/user-interface/listview/index.md).

Özel oluşturucu, artık her hücre için platforma özel düzeni özelleştirildiği her bir uygulama projesine eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir öğesinin `ViewCellRenderer` özel hücre işleyen sınıfı.
1. Özel hücre işleyen platforma özgü yöntemi geçersiz kılın ve özelleştirmek için mantıksal yazma.
1. Ekleme bir `ExportRenderer` bunu Xamarin.Forms özel hücre işlemek için kullanılacak belirtmek için özel Oluşturucu sınıfı özniteliği. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Xamarin.Forms öğelerin çoğu, her platform projesinde özel bir oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse denetimin taban sınıfı için varsayılan oluşturucu kullanılır. Ancak, özel Oluşturucu her platform projesinde işlenirken gereken bir [Viewcell'i](xref:Xamarin.Forms.ViewCell) öğesi.

Örnek uygulamada, onlar arasındaki ilişkileri yanı sıra her bir proje sorumluluklarını Aşağıdaki diyagramda gösterilmektedir:

![](viewcell-images/solution-structure.png "NativeCell özel Oluşturucu Proje Sorumlulukları")

`NativeCell` Öğesinden türetilen tüm hangi platforma özel Oluşturucu sınıflar tarafından işlenen özel hücre `ViewCellRenderer` her platform için sınıf. Bu her sonuçları `NativeCell` platforma özel düzeni kullanarak, aşağıdaki ekran görüntülerinde gösterildiği işlenen özel hücre:

![](viewcell-images/screenshots.png "Her platformda NativeCell")

`ViewCellRenderer` Sınıfı özel hücre işlemek için platforma özgü yöntemleri gösterir. Bu `GetCell` yöntemi iOS platformunda `GetCellCore` Android platformunda yöntemi ve `GetTemplate` UWP yöntemi.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` özniteliği işleyici Xamarin.Forms ile kaydeder. Öznitelik, iki parametre – işlenen Xamarin.Forms hücrenin tür adı ve özel Oluşturucu türü adını alır. `assembly` Özniteliğiyle önek öznitelik tüm derleme için geçerli olduğunu belirtir.

Aşağıdaki bölümlerde her platforma özgü özel Oluşturucu sınıfın uygulaması açıklanmaktadır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, iOS platformu için özel Oluşturucu gösterir:

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

`GetCell` Yöntemi, her bir hücresinde görüntülenecek oluşturmak için çağrılır. Her hücre bir `NativeiOSCell` örneği hücre ve verilerini düzenini tanımlar. İşlemi `GetCell` yöntemdir bağımlı [ `ListView` ](xref:Xamarin.Forms.ListView) önbelleğe alma stratejisi:

- Zaman [ `ListView` ](xref:Xamarin.Forms.ListView) önbelleğe alma stratejisi [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), `GetCell` yöntemi her hücre için çağrılacak. A `NativeiOSCell` örneği her biri için oluşturulacak `NativeCell` ilk ekranda görüntülenen örnek. Aracılığıyla kullanıcı kaydırma yaparken `ListView`, `NativeiOSCell` örnekleri yeniden kullanılan olacaktır. İOS hücre yeniden kullanma hakkında daha fazla bilgi için bkz: [hücre yeniden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Bu özel Oluşturucu kodu bazı hücre yeniden kullanımı gerçekleştirecek bile [ `ListView` ](xref:Xamarin.Forms.ListView) hücreleri saklanacak şekilde ayarlanmıştır.

  Her tarafından görüntülenen verileri `NativeiOSCell` örneği yeni oluşturulmuş ya da yeniden kullanılır, güncelleştirilecek her verilerle `NativeCell` tarafından örnek `UpdateCell` yöntemi.

  > [!NOTE]
  > `OnNativeCellPropertyChanged` Yöntemi hiçbir zaman olur olduğunda çağrılan [ `ListView` ](xref:Xamarin.Forms.ListView) önbelleğe alma stratejisi hücreleri korumak için ayarlanır.

- Zaman [ `ListView` ](xref:Xamarin.Forms.ListView) önbelleğe alma stratejisi [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), `GetCell` yöntemi, ilk ekranda görüntülenen her hücre için çağrılacak. A `NativeiOSCell` örneği her biri için oluşturulacak `NativeCell` ilk ekranda görüntülenen örnek. Her tarafından görüntülenen verileri `NativeiOSCell` örnek verilerle güncelleştirilir `NativeCell` tarafından örnek `UpdateCell` yöntemi. Ancak, `GetCell` yöntemi ve kullanıcı sonuçlarda gezinirken olarak çağrılacak olmaz `ListView`. Bunun yerine, `NativeiOSCell` örnekleri yeniden kullanılan olacaktır. `PropertyChanged` olayları yükseltilir `NativeCell` örnek verilerini değiştiğinde ve `OnNativeCellPropertyChanged` olay işleyicisi, her yeniden kullanılan verileri güncelleştirir `NativeiOSCell` örneği.

Aşağıdaki örnekte gösterildiği kod `OnNativeCellPropertyChanged` ne zaman çağrılan yöntemi bir `PropertyChanged` olayı oluşturulur:

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

Bu yöntem güncelleştirmeleri tarafından görüntülenen verileri yeniden kullanılan `NativeiOSCell` örnekleri. Yöntemi, birden çok kez çağrılabilir olarak değişen bir özellik için bir onay yapılır.

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

Bu sınıf, hücrenin içeriğini ve düzenini işlemek için kullanılan denetimleri tanımlar. Sınıfının Implements [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) gerekli olduğunda arabirimi [ `ListView` ](xref:Xamarin.Forms.ListView) kullanan [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) önbelleğe alma stratejisi. Bu arabirim, sınıf uygulamalıdır belirtir [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) geri hücreler için özel hücre veri döndürmesi gereken özelliği.

`NativeiOSCell` Oluşturucu başlatır görünümünü `HeadingLabel`, `SubheadingLabel`, ve `CellImageView` özellikleri. Bu özellikler, depolanan verileri görüntülemek için kullanılan `NativeCell` örneği ile `UpdateCell` her bir özellik değerini ayarlamak için çağrılan yöntem. Buna ek olarak, [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kullanan [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) stratejisi, tarafından görüntülenen verileri önbelleğe alma `HeadingLabel`, `SubheadingLabel`, ve `CellImageView` özellikleri olabilir güncelleştiren `OnNativeCellPropertyChanged` özel Oluşturucu yöntemi.

Hücre düzenini tarafından gerçekleştirilir `LayoutSubviews` koordinatlarını ayarlar geçersiz kılma `HeadingLabel`, `SubheadingLabel`, ve `CellImageView` hücresi içinde.

### <a name="creating-the-custom-renderer-on-android"></a>Android'de özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, Android platformu için özel Oluşturucu gösterir:

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

`GetCellCore` Yöntemi, her bir hücresinde görüntülenecek oluşturmak için çağrılır. Her hücre bir `NativeAndroidCell` örneği hücre ve verilerini düzenini tanımlar. İşlemi `GetCellCore` yöntemdir bağımlı [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) önbelleğe alma stratejisi:

- Zaman [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) önbelleğe alma stratejisi [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), `GetCellCore` yöntemi her hücre için çağrılacak. A `NativeAndroidCell` her biri için oluşturulacak `NativeCell` ilk ekranda görüntülenen örnek. Aracılığıyla kullanıcı kaydırma yaparken `ListView`, `NativeAndroidCell` örnekleri yeniden kullanılan olacaktır. Android hücre yeniden kullanma hakkında daha fazla bilgi için bkz. [satır görünümü yeniden kullanma](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Bu özel Oluşturucu kodu bazı hücre yeniden kullanımı gerçekleştireceğini Not bile [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) hücreleri saklanacak şekilde ayarlanmıştır.

  Her tarafından görüntülenen verileri `NativeAndroidCell` örneği yeni oluşturulmuş ya da yeniden kullanılır, güncelleştirilecek her verilerle `NativeCell` tarafından örnek `UpdateCell` yöntemi.

  > [!NOTE]
  > Not da `OnNativeCellPropertyChanged` yöntemi olacaktır ne zaman çağrılır [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) olan hücreleri korumak için bu seçeneği ayarlanırsa, bunu değil güncelleştirir `NativeAndroidCell` özellik değerleri.

- Zaman [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) önbelleğe alma stratejisi [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), `GetCellCore` yöntemi, ilk ekranda görüntülenen her hücre için çağrılacak. A `NativeAndroidCell` örneği her biri için oluşturulacak `NativeCell` ilk ekranda görüntülenen örnek. Her tarafından görüntülenen verileri `NativeAndroidCell` örnek verilerle güncelleştirilir `NativeCell` tarafından örnek `UpdateCell` yöntemi. Ancak, `GetCellCore` yöntemi ve kullanıcı sonuçlarda gezinirken olarak çağrılacak olmaz `ListView`. Bunun yerine, `NativeAndroidCell` örnekleri yeniden kullanılan olacaktır.  `PropertyChanged` olayları yükseltilir `NativeCell` örnek verilerini değiştiğinde ve `OnNativeCellPropertyChanged` olay işleyicisi, her yeniden kullanılan verileri güncelleştirir `NativeAndroidCell` örneği.

Aşağıdaki örnekte gösterildiği kod `OnNativeCellPropertyChanged` ne zaman çağrılan yöntemi bir `PropertyChanged` olayı oluşturulur:

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

Bu yöntem güncelleştirmeleri tarafından görüntülenen verileri yeniden kullanılan `NativeAndroidCell` örnekleri. Yöntemi, birden çok kez çağrılabilir olarak değişen bir özellik için bir onay yapılır.

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

Bu sınıf, hücrenin içeriğini ve düzenini işlemek için kullanılan denetimleri tanımlar. Sınıfının Implements [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) gerekli olduğunda arabirimi [ `ListView` ](xref:Xamarin.Forms.ListView) kullanan [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) önbelleğe alma stratejisi. Bu arabirim, sınıf uygulamalıdır belirtir [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) geri hücreler için özel hücre veri döndürmesi gereken özelliği.

`NativeAndroidCell` Oluşturucusu Şişir `NativeAndroidCell` düzeni ve başlatır `HeadingTextView`, `SubheadingTextView`, ve `ImageView` denetimlere inflated düzen özellikleri. Bu özellikler, depolanan verileri görüntülemek için kullanılan `NativeCell` örneği ile `UpdateCell` her bir özellik değerini ayarlamak için çağrılan yöntem. Buna ek olarak, [ `ListView` ](xref:Xamarin.Forms.ListView) kullanan [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) stratejisi, tarafından görüntülenen verileri önbelleğe alma `HeadingTextView`, `SubheadingTextView`, ve `ImageView` özellikleri olabilir güncelleştiren `OnNativeCellPropertyChanged` özel Oluşturucu yöntemi.

Düzen tanımını aşağıdaki kod örneği gösterir `NativeAndroidCell.axml` Düzen dosyası:

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

Bu düzen, iki belirtir `TextView` denetimleri ve `ImageView` denetim hücrenin içeriğini görüntülemek için kullanılır. İki `TextView` denetimleri içinde dikey yönlendirilmiş bir `LinearLayout` denetimiyle içinde bulunan tüm denetimlerin bir `RelativeLayout`.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP üzerinde özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, UWP için özel Oluşturucu gösterir:

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

`GetTemplate` Veri listesindeki her satır için işlenecek hücrenin döndürülecek yöntemi çağrılır. Oluşturur bir `DataTemplate` her `NativeCell` ile ekranda görüntülenecek örneği `DataTemplate` görünümünü ve hücrenin içeriğini tanımlama.

`DataTemplate` Uygulama düzeyinde kaynak sözlüğünde depolanır ve aşağıdaki kod örneğinde gösterilir:

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

`DataTemplate` Hücre ve düzeninin ve görünümünün içeriklerini görüntülemek için kullanılan denetimleri belirtir. İki `TextBlock` denetimleri ve `Image` denetimine veri bağlama aracılığıyla hücrenin içeriğini görüntülemek için kullanılır. Ayrıca, bir örneğini `ConcatImageExtensionConverter` birleştirmek için kullanılan `.jpg` dosya her görüntü dosya adı uzantısı. Bu, sağlar `Image` denetimi, yüklemek ve olduğunda görüntüsünü işlemek `Source` özelliği ayarlanmış.

## <a name="summary"></a>Özet

Bu makalede için özel Oluşturucu Oluşturma gösterilmiştir bir [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) bir Xamarin.Forms içinde barındırılan [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetimi. Bu Xamarin.Forms Düzen hesaplama sırasında sürekli çağrılmasını durdurur `ListView` kaydırma.


## <a name="related-links"></a>İlgili bağlantılar

- [ListView performans](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
