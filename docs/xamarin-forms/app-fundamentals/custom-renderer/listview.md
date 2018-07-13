---
title: Bir ListView özelleştirme
description: Xamarin.Forms ListView dikey bir liste olarak verilerinin bir koleksiyonunu görüntüleyen bir görünümüdür. Bu makalede, yerel liste denetimi performans hakkında daha fazla denetime izin vererek platforma özel liste denetimleri ve yerel hücre düzenleri kapsülleyen bir özel Oluşturucu oluşturma işlemini gösterir.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b3b73d542faebdb8ab85c989d7812368f4f3ffac
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997497"
---
# <a name="customizing-a-listview"></a>Bir ListView özelleştirme

_Xamarin.Forms ListView dikey bir liste olarak verilerinin bir koleksiyonunu görüntüleyen bir görünümüdür. Bu makalede, yerel liste denetimi performans hakkında daha fazla denetime izin vererek platforma özel liste denetimleri ve yerel hücre düzenleri kapsülleyen bir özel Oluşturucu oluşturma işlemini gösterir._

Her bir Xamarin.Forms görünüm yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici içerir. Olduğunda bir [ `ListView` ](xref:Xamarin.Forms.ListView) bir Xamarin.Forms uygulaması iOS tarafından işlenen `ListViewRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `UITableView` denetimi. Android platformunda `ListViewRenderer` sınıfın örneğini oluşturur, bir yerel `ListView` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `ListViewRenderer` sınıfın örneğini oluşturur, bir yerel `ListView` denetimi. Oluşturucu ve Xamarin.Forms denetimleri eşleyen yerel denetim sınıfları hakkında daha fazla bilgi için bkz. [oluşturucu temel sınıfları ve yerel denetimleri](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagramda arasındaki ilişkiyi gösterir [ `ListView` ](xref:Xamarin.Forms.ListView) ve uygulamaya karşılık gelen yerel denetimi:

![](listview-images/listview-classes.png "ListView ve uygulama yerel denetimi arasındaki ilişki")

İşleme sürecini avantajlarından platforma özgü özelleştirmeler için özel Oluşturucu oluşturarak uygulamak için gerçekleştirilebilecek bir [ `ListView` ](xref:Xamarin.Forms.ListView) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_ListView_Control) Xamarin.Forms özel bir denetim.
1. [Tüketen](#Consuming_the_Custom_Control) Xamarin.Forms özel denetimden.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) her platformda denetimi için özel Oluşturucu.

Her öğe artık sırayla uygulanacağı açıklanmıştır bir `NativeListView` platforma özel liste denetimleri ve yerel hücre düzenleri yararlanır Oluşturucu. Bu senaryo, bir listesini içeren mevcut yerel uygulama ve yeniden kullanılabilir bir hücre kodla taşırken kullanışlıdır. Ayrıca, ayrıntılı özelleştirme gibi veri sanallaştırma performansını etkileyebilir listesi denetimi özellikleri sağlar.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>Özel ListView denetimi oluşturma

Özel bir [ `ListView` ](xref:Xamarin.Forms.ListView) denetimi tarafından sınıflara oluşturulabilir `ListView` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public class NativeListView : ListView
{
  public static readonly BindableProperty ItemsProperty =
    BindableProperty.Create ("Items", typeof(IEnumerable<DataSource>), typeof(NativeListView), new List<DataSource> ());

  public IEnumerable<DataSource> Items {
    get { return (IEnumerable<DataSource>)GetValue (ItemsProperty); }
    set { SetValue (ItemsProperty, value); }
  }

  public event EventHandler<SelectedItemChangedEventArgs> ItemSelected;

  public void NotifyItemSelected (object item)
  {
    if (ItemSelected != null) {
      ItemSelected (this, new SelectedItemChangedEventArgs (item));
    }
  }
}
```

`NativeListView` .NET Standard kitaplığı projesinde oluşturulur ve özel denetim için API tanımlar. Bu denetimi sunan bir `Items` doldurmak için kullanılan özellik `ListView` verileri ve hangi veri için bağımlı nedeniyle görüntüler. Ayrıca kullanıma sunduğu bir `ItemSelected` bir platforma özgü yerel liste denetiminde bir öğe seçildiğinde, harekete geçirilecek olay. Veri bağlama hakkında daha fazla bilgi için bkz. [temel veri bağlama bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Özel denetim kullanma

`NativeListView` Özel denetim başvurulabilir Xaml içinde .NET Standard kitaplığı projesinde konumu için bir ad alanı bildirmek ve denetimi ad alanı öneki kullanarak. Aşağıdaki kod örnekte gösterildiği nasıl `NativeListView` özel denetimi bir XAML sayfası tarafından kullanılabilir:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*" />
            </Grid.RowDefinitions>
          <Label Text="{x:Static local:App.Description}" HorizontalTextAlignment="Center" />
            <local:NativeListView Grid.Row="1" x:Name="nativeListView" ItemSelected="OnItemSelected" VerticalOptions="FillAndExpand" />
          </Grid>
      </ContentPage.Content>
</ContentPage>
```

`local` Ad alanı ön eki adı herhangi bir şey. Ancak, `clr-namespace` ve `assembly` değerleri, özel denetimin ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde, ön ek özel denetim başvurusu yapmak için kullanılır.

Aşağıdaki kod örnekte gösterildiği nasıl `NativeListView` özel denetimin bir C# sayfası tarafından kullanılabilir:

```csharp
public class MainPageCS : ContentPage
{
    NativeListView nativeListView;

    public MainPageCS()
    {
        nativeListView = new NativeListView
        {
            Items = DataSource.GetList(),
            VerticalOptions = LayoutOptions.FillAndExpand
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

        Content = new Grid
        {
            RowDefinitions = {
                new RowDefinition { Height = GridLength.Auto },
                new RowDefinition { Height = new GridLength (1, GridUnitType.Star) }
            },
            Children = {
                new Label { Text = App.Description, HorizontalTextAlignment = TextAlignment.Center },
                nativeListView
            }
        };
        nativeListView.ItemSelected += OnItemSelected;
    }
    ...
}
```

`NativeListView` Özel denetimi veri ile doldurulan bir listesini görüntülemek için platforma özgü özel Oluşturucu kullanan `Items` özelliği. Listedeki her bir satır veri – bir ad, kategori ve bir görüntü dosya adı üç öğelerini içerir. Listedeki her bir satırın düzenini platforma özgü özel Oluşturucu tarafından tanımlanır.

> [!NOTE]
> Çünkü `NativeListView` özel denetim özelliği kaydırma dahil platforma özel liste denetimleri kullanarak işlenecek, özel denetimin kaydırılabilir Düzen denetimlerinde gibi barındırılmalıdır değil [ `ScrollView` ](xref:Xamarin.Forms.ScrollView).

Özel oluşturucu artık platforma özel liste denetimleri ve yerel hücre düzenleri oluşturmak için her uygulama projesine eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir öğesinin `ListViewRenderer` özel kontrolünü icra eden sınıf.
1. Geçersiz kılma `OnElementChanged` özelleştirmek için özel denetim ve Yaz mantığı işleyen yöntemi. Bu yöntem olduğunda çağrılır karşılık gelen Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) oluşturulur.
1. Ekleme bir `ExportRenderer` bunu Xamarin.Forms özel denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı özniteliği. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Her platform projesinde özel bir oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse hücrenin taban sınıfı için varsayılan oluşturucu kullanılır.

Örnek uygulamada, onlar arasındaki ilişkileri yanı sıra her bir proje sorumluluklarını Aşağıdaki diyagramda gösterilmektedir:

![](listview-images/solution-structure.png "NativeListView özel Oluşturucu Proje Sorumlulukları")

`NativeListView` Öğesinden türetilen tüm hangi platforma özel Oluşturucu sınıflar tarafından işlenen özel denetim `ListViewRenderer` her platform için sınıf. Bu her sonuçları `NativeListView` aşağıdaki ekran görüntülerinde gösterildiği gibi platforma özgü liste denetimleri ve yerel hücre düzenlerle işlenen özel denetimi:

![](listview-images/screenshots.png "Her platformda NativeListView")

`ListViewRenderer` Sınıfı kullanıma sunan `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için Xamarin.Forms özel denetim oluşturulurken çağrılan yöntemi. Bu yöntem bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikler Xamarin.Forms öğesini temsil, oluşturucu *olduğu* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olduğu* bağlı olarak, sırasıyla. Örnek uygulamada, `OldElement` özelliği `null` ve `NewElement` özelliği, bir başvuru içerecek `NativeListView` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, yerel denetim özelleştirme gerçekleştirmek için yerdir. Belirlenmiş bir başvuru platformunda kullanılan yerel denetlemek için erişilebilir `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetime başvuru aracılığıyla alınabilir `Element` özelliği.

Olay işleyicileri için abone olurken dikkatli'nin alınması gereken `OnElementChanged` yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Yalnızca yerel denetimi yapılandırılmalıdır ve özel Oluşturucu yeni bir Xamarin.Forms öğe eklendiğinde olay işleyicileri abone. Benzer şekilde, ancak ' % s'öğesi işleyici değişiklikleri iliştirildiğinde abone tüm olay işleyicileri gelen aboneliği olması gerekir. Bu yaklaşımı benimsemeyi bellek sızıntılardan etkilese değil özel bir oluşturucu oluşturmaya yardımcı olur.

Geçersiz kılınan bir sürümünü `OnElementPropertyChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, Xamarin.Forms özel denetim üzerinde bağlanılabilir özellik değişikliklerine yanıt verme için yerdir. Bu geçersiz kılma birden çok kez çağrılabilir olarak değişen bir özellik için bir onay her zaman yapılmalıdır.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` özniteliği işleyici Xamarin.Forms ile kaydeder. Öznitelik, iki parametre – işlenen Xamarin.Forms özel denetimin tür adı ve özel Oluşturucu türü adını alır. `assembly` Özniteliğiyle önek öznitelik tüm derleme için geçerli olduğunu belirtir.

Aşağıdaki bölümlerde her platforma özgü özel Oluşturucu sınıfın uygulaması açıklanmaktadır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, iOS platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeiOSListViewRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSListViewRenderer : ListViewRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null) {
                // Unsubscribe
            }

            if (e.NewElement != null) {
                Control.Source = new NativeiOSListViewSource (e.NewElement as NativeListView);
            }
        }
    }
}
```

`UITableView` Denetim yapılandırılmış bir örneğini oluşturarak `NativeiOSListViewSource` koşuluyla yeni bir Xamarin.Forms öğesi için özel Oluşturucu bağlı olduğu sınıf. Bu sınıf için veri sağlar `UITableView` kılarak denetim `RowsInSection` ve `GetCell` yöntemlerinden `UITableViewSource` sınıfı ve olarak da ifşa eden bir `Items` görüntülenecek veri listesini içeren bir özellik. Sınıfı sağlar bir `RowSelected` çağıran bir yöntemi geçersiz kılma `ItemSelected` tarafından sağlanan olay `NativeListView` özel denetim. Geçersiz kılmalar yöntemi hakkında daha fazla bilgi için bkz: [sınıflara UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Yöntemi döndürür bir `UITableCellView` listesindeki her satır için verilerle doldurulur ve aşağıdaki kod örneğinde gösterilir:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
  // request a recycled cell to save memory
  NativeiOSListViewCell cell = tableView.DequeueReusableCell (cellIdentifier) as NativeiOSListViewCell;

  // if there are no cells to reuse, create a new one
  if (cell == null) {
    cell = new NativeiOSListViewCell (cellIdentifier);
  }

  if (String.IsNullOrWhiteSpace (tableItems [indexPath.Row].ImageFilename)) {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , null);
  } else {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , UIImage.FromFile ("Images/" + tableItems [indexPath.Row].ImageFilename + ".jpg"));
  }

  return cell;
}
```

Bu yöntem, oluşturur bir `NativeiOSListViewCell` ekranda görüntülenen verileri her satır için örneği. `NativeiOSCell` Örneği her hücre ve veri hücre düzenini tanımlar. Bir hücreyi kaydırma nedeniyle ekranından kaybolduğunda, hücre için yeniden kullanılabilir hale getirilir. Bu, yalnızca sağlayarak İsraf bellek önler `NativeiOSCell` örnekleri ekran yerine tüm verileri listesinde görüntülenen veriler için. Hücre yeniden hakkında daha fazla bilgi için bkz: [hücre yeniden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Yöntemi de okur `ImageFilename` özelliği yoksa, görüntünün okur ve olarak depolar, sağlanan verileri, her satır bir `UIImage` güncelleştirmeden önce örneği `NativeiOSListViewCell` için örnek verileri (ad, kategori ve görüntü) Satır.

`NativeiOSListViewCell` Sınıfı her hücre düzenini tanımlar ve aşağıdaki kod örneğinde gösterilir:

```csharp
public class NativeiOSListViewCell : UITableViewCell
{
  UILabel headingLabel, subheadingLabel;
  UIImageView imageView;

  public NativeiOSListViewCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
  {
    SelectionStyle = UITableViewCellSelectionStyle.Gray;

    ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);

    imageView = new UIImageView ();

    headingLabel = new UILabel () {
      Font = UIFont.FromName ("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB (127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    subheadingLabel = new UILabel () {
      Font = UIFont.FromName ("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB (38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add (headingLabel);
    ContentView.Add (subheadingLabel);
    ContentView.Add (imageView);
  }

  public void UpdateCell (string caption, string subtitle, UIImage image)
  {
    headingLabel.Text = caption;
    subheadingLabel.Text = subtitle;
    imageView.Image = image;
  }

  public override void LayoutSubviews ()
  {
    base.LayoutSubviews ();

    headingLabel.Frame = new CoreGraphics.CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
    subheadingLabel.Frame = new CoreGraphics.CGRect (100, 18, 100, 20);
    imageView.Frame = new CoreGraphics.CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Bu sınıf, hücrenin içeriğini ve düzenini işlemek için kullanılan denetimleri tanımlar. `NativeiOSListViewCell` Oluşturucusu örneklerini oluşturur `UILabel` ve `UIImageView` denetler ve görünümlerini başlatır. Bu denetimlerin her bir satırın verileri görüntülemek için kullanılan `UpdateCell` üzerinde bu veri kümesi için kullanılan yöntem `UILabel` ve `UIImageView` örnekleri. Bu örneklerin konumu ayarlayın tarafından geçersiz kılınan `LayoutSubviews` hücresi içinde kendi koordinat belirterek yöntemi.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Özel denetim üzerinde bir özellik değişiminin yanıtlama

Varsa `NativeListView.Items` özellik, eklenen öğeleri nedeniyle değişiklikler veya listeden kaldırıldı, özel Oluşturucu değişiklikleri görüntüleyerek yanıt vermesi gerekir. Bu geçersiz kılma tarafından gerçekleştirilebilir `OnElementPropertyChanged` aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Yöntem yeni bir örneğini oluşturur `NativeiOSListViewSource` için veri sağlar sınıfını `UITableView` denetlemek, bağlanabilir koşuluyla `NativeListView.Items` özelliği değişti.

### <a name="creating-the-custom-renderer-on-android"></a>Android'de özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, Android platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeAndroidListViewRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidListViewRenderer : ListViewRenderer
    {
        Context _context;

        public NativeAndroidListViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // unsubscribe
                Control.ItemClick -= OnItemClick;
            }

            if (e.NewElement != null)
            {
                // subscribe
                Control.Adapter = new NativeAndroidListViewAdapter(_context as Android.App.Activity, e.NewElement as NativeListView);
                Control.ItemClick += OnItemClick;
            }
        }
        ...

        void OnItemClick(object sender, Android.Widget.AdapterView.ItemClickEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(((NativeListView)Element).Items.ToList()[e.Position - 1]);
        }
    }
}
```

Yerel `ListView` denetimi özel Oluşturucu, yeni bir Xamarin.Forms öğesine bağlı olması koşuluyla yapılandırılır. Bu yapılandırma örneği oluşturmayı içerir `NativeAndroidListViewAdapter` yerel verileri sağlar sınıfını `ListView` denetlemek ve işlemek için bir olay işleyicisi kaydedilirken `ItemClick` olay. Sırayla Bu işleyici çağırır `ItemSelected` tarafından sağlanan olay `NativeListView` özel denetim. `ItemClick` Olay gönderilmedi gelen değişiklikleri Xamarin.Forms öğesi işleyici bağlıysa.

`NativeAndroidListViewAdapter` Türetildiği `BaseAdapter` sınıfı ve kullanıma sunan bir `Items` görüntülenecek veri listesini içeren özellik yanı sıra geçersiz kılma `Count`, `GetView`, `GetItemId`, ve `this[int]` yöntemleri. Bu yöntemi geçersiz kılma hakkında daha fazla bilgi için bkz. [bir ListAdapter uygulama](~/android/user-interface/layouts/list-view/populating.md). `GetView` Yöntemi verilerle doldurulmuş, her satır için bir görünüm verir ve aşağıdaki kod örneğinde gösterilir:

```csharp
public override View GetView (int position, View convertView, ViewGroup parent)
{
  var item = tableItems [position];

  var view = convertView;
  if (view == null) {
    // no view to re-use, create new
    view = context.LayoutInflater.Inflate (Resource.Layout.NativeAndroidListViewCell, null);
  }
  view.FindViewById<TextView> (Resource.Id.Text1).Text = item.Name;
  view.FindViewById<TextView> (Resource.Id.Text2).Text = item.Category;

  // grab the old image and dispose of it
  if (view.FindViewById<ImageView> (Resource.Id.Image).Drawable != null) {
    using (var image = view.FindViewById<ImageView> (Resource.Id.Image).Drawable as BitmapDrawable) {
      if (image != null) {
        if (image.Bitmap != null) {
          //image.Bitmap.Recycle ();
          image.Bitmap.Dispose ();
        }
      }
    }
  }

  // If a new image is required, display it
  if (!String.IsNullOrWhiteSpace (item.ImageFilename)) {
    context.Resources.GetBitmapAsync (item.ImageFilename).ContinueWith ((t) => {
      var bitmap = t.Result;
      if (bitmap != null) {
        view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (bitmap);
        bitmap.Dispose ();
      }
    }, TaskScheduler.FromCurrentSynchronizationContext ());
  } else {
    // clear the image
    view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (null);
  }

  return view;
}
```

`GetView` İşlenmek üzere bir hücreye dönmek için yöntemi çağrıldığında olarak bir `View`, veri listesindeki her satır için. Oluşturur bir `View` örnek görünümünü birlikte, ekranda görüntülenen verileri her satır için `View` bir düzen dosyasında tanımlanan örneği. Bir hücreyi kaydırma nedeniyle ekranından kaybolduğunda, hücre için yeniden kullanılabilir hale getirilir. Bu, yalnızca sağlayarak İsraf bellek önler `View` örnekleri ekran yerine tüm verileri listesinde görüntülenen veriler için. Görünümü yeniden kullanma hakkında daha fazla bilgi için bkz. [satır görünümü yeniden kullanma](~/android/user-interface/layouts/list-view/populating.md).

`GetView` Yöntemi de doldurur `View` görüntü verileri belirtilen dosya okuma gibi veriler örneğiyle `ImageFilename` özelliği.

Yerel olarak her hücre dispayed düzenini `ListView` tanımlanan `NativeAndroidListViewCell.axml` tarafından şişirileceğini Düzen dosyası, `LayoutInflater.Inflate` yöntemi. Aşağıdaki kod örneği, düzen tanımı gösterilmektedir:

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
            android:id="@+id/Text1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/Text2"
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

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Özel denetim üzerinde bir özellik değişiminin yanıtlama

Varsa `NativeListView.Items` özellik, eklenen öğeleri nedeniyle değişiklikler veya listeden kaldırıldı, özel Oluşturucu değişiklikleri görüntüleyerek yanıt vermesi gerekir. Bu geçersiz kılma tarafından gerçekleştirilebilir `OnElementPropertyChanged` aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

Yöntem yeni bir örneğini oluşturur `NativeAndroidListViewAdapter` yerel verileri sağlar sınıfını `ListView` denetlemek, bağlanabilir koşuluyla `NativeListView.Items` özelliği değişti.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP üzerinde özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, UWP için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeUWPListViewRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPListViewRenderer : ListViewRenderer
    {
        ListView listView;

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            listView = Control as ListView;

            if (e.OldElement != null)
            {
                // Unsubscribe
                listView.SelectionChanged -= OnSelectedItemChanged;
            }

            if (e.NewElement != null)
            {
                listView.SelectionMode = ListViewSelectionMode.Single;
                listView.IsItemClickEnabled = false;
                listView.ItemsSource = ((NativeListView)e.NewElement).Items;             
                listView.ItemTemplate = App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
                // Subscribe
                listView.SelectionChanged += OnSelectedItemChanged;
            }  
        }

        void OnSelectedItemChanged(object sender, SelectionChangedEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(listView.SelectedItem);
        }
    }
}
```

Yerel `ListView` denetimi özel Oluşturucu, yeni bir Xamarin.Forms öğesine bağlı olması koşuluyla yapılandırılır. Bu yapılandırma ayarı içerir nasıl yerel `ListView` denetimi yanıt seçilen öğeleri görünümünü ve her hücrenin içeriğini tanımlama ve işlemekiçinbirolayişleyicisikaydedilirkendenetimitarafındangörüntülenenverilerdolduruluyor`SelectionChanged` olay. Sırayla Bu işleyici çağırır `ItemSelected` tarafından sağlanan olay `NativeListView` özel denetim. `SelectionChanged` Olay gönderilmedi gelen değişiklikleri Xamarin.Forms öğesi işleyici bağlıysa.

Görünümünü ve her yerel içeriğini `ListView` hücre tarafından tanımlanan bir `DataTemplate` adlı `ListViewItemTemplate`. Bu `DataTemplate` uygulama düzeyinde kaynak sözlüğünde depolanır ve aşağıdaki kod örneğinde gösterilir:

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="#DAFF7F">
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

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Özel denetim üzerinde bir özellik değişiminin yanıtlama

Varsa `NativeListView.Items` özellik, eklenen öğeleri nedeniyle değişiklikler veya listeden kaldırıldı, özel Oluşturucu değişiklikleri görüntüleyerek yanıt vermesi gerekir. Bu geçersiz kılma tarafından gerçekleştirilebilir `OnElementPropertyChanged` aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
protected override void OnElementPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
    base.OnElementPropertyChanged(sender, e);

    if (e.PropertyName == NativeListView.ItemsProperty.PropertyName)
    {
        listView.ItemsSource = ((NativeListView)Element).Items;
    }
}
```

Yöntem yerel yeniden doldurur `ListView` denetim bağlanabilir değiştirilen verileri ile sağlanan `NativeListView.Items` özelliği değişti.

## <a name="summary"></a>Özet

Bu makalede, yerel liste denetimi performans hakkında daha fazla denetime izin vererek platforma özel liste denetimleri ve yerel hücre düzenleri kapsülleyen bir özel Oluşturucu Oluşturma gösterilmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererListView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
