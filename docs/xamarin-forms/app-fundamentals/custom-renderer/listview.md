---
title: ListView özelleştirme
description: Xamarin.Forms ListView dikey bir liste olarak verilerinin bir koleksiyonunu görüntüleyen bir görünüm değil. Bu makalede, yerel liste denetimi performansı hakkında daha fazla denetime izin vererek platforma özgü liste denetimleri ve yerel hücre düzenleri yalıtan özel Oluşturucu Oluşturma gösterilir.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 9d822444196479dabd19f43f45f289117f64c05e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-a-listview"></a>ListView özelleştirme

_Xamarin.Forms ListView dikey bir liste olarak verilerinin bir koleksiyonunu görüntüleyen bir görünüm değil. Bu makalede, yerel liste denetimi performansı hakkında daha fazla denetime izin vererek platforma özgü liste denetimleri ve yerel hücre düzenleri yalıtan özel Oluşturucu Oluşturma gösterilir._

Her Xamarin.Forms görünüm yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu yok. Zaman bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) iOS içinde bir Xamarin.Forms uygulaması tarafından işlenen `ListViewRenderer` sınıf örneği, hangi sırayla yerel başlatır `UITableView` denetim. Android platformunda `ListViewRenderer` sınıfı başlatır yerel `ListView` denetim. Windows Phone ve evrensel Windows Platformu (UWP) `ListViewRenderer` sınıfı başlatır yerel `ListView` denetim. Oluşturucu ve Xamarin.Forms denetimleri Eşle yerel denetim sınıfları hakkında daha fazla bilgi için bkz: [Oluşturucu taban sınıfları ve yerel denetimlere](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagram arasındaki ilişkiyi gösterir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ve uyguladıktan karşılık gelen yerel denetimi:

![](listview-images/listview-classes.png "ListView denetimi ve uygulama yerel denetimleri arasındaki ilişki")

Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için avantajlarından alınabilir bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_ListView_Control) Xamarin.Forms özel bir denetim.
1. [Tüketen](#Consuming_the_Custom_Control) Xamarin.Forms özel denetim.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) her platformda denetimi için özel Oluşturucu.

Her öğe artık sırasıyla uygulamak için incelenecektir bir `NativeListView` platforma özgü liste denetimleri ve yerel hücre düzenleri yararlanan Oluşturucu. Bu senaryo, bir listesini içeren var olan bir yerel uygulamayı ve yeniden kullanılabilir hücre kodu taşıma durumlarda faydalıdır. Ayrıca, verileri sanallaştırma gibi performansını etkileyebilecek listesi denetimi özellikleri ayrıntılı özelleştirmesini sağlar.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>Özel ListView denetimi oluşturma

Özel bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetim sınıflara tarafından oluşturulabilir `ListView` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

`NativeListView` Taşınabilir sınıf kitaplığı (PCL) projesinde oluşturulur ve özel denetim için API tanımlar. Bu denetim kullanıma sunan bir `Items` doldurmak için kullanılan özellik `ListView` veri ve olabilen amaçları için bağlı verileri görüntülemek. Ayrıca kullanıma sunan bir `ItemSelected` bir platforma özgü yerel liste denetiminde bir öğe seçildiğinde, tetiklenmez olay. Veri bağlama hakkında daha fazla bilgi için bkz: [veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Özel denetim kullanma

`NativeListView` Özel denetim başvurulabilir XAML'de PCL projesinde konumu için bir ad alanı bildirme ve denetimi ad alanı öneki kullanarak. Aşağıdaki örnekte gösterildiği kod nasıl `NativeListView` özel denetim XAML sayfası tarafından tüketilen:

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

`local` Ad alanı öneki adlı bir şey. Ancak, `clr-namespace` ve `assembly` değerlerin özel denetim ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde öneki özel denetim başvurmak için kullanılır.

Aşağıdaki örnekte gösterildiği kod nasıl `NativeListView` özel denetim bir C# sayfası tarafından tüketilen:

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

`NativeListView` Özel denetim aracılığıyla doldurulmuş veri listesini görüntülemek için platforma özgü özel Oluşturucu kullanır `Items` özelliği. Listedeki her bir satır veri – bir ad, kategori ve resim dosya adı üç öğeleri içerir. Listedeki her bir satır düzeni platforma özgü özel Oluşturucu tarafından tanımlanır.

> [!NOTE]
> Çünkü `NativeListView` özel denetim çizilir özelliği kaydırma dahil platforma özgü liste denetimleri kullanarak, özel denetimin kaydırılabilir Düzen denetimlerinde gibi barındırılmalıdır değil [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/).

Özel oluşturucu artık platforma özgü liste denetimleri ve yerel hücre düzenler oluşturmak için her uygulama projesi eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Öğesinin bir alt kümesi oluşturmak `ListViewRenderer` özel denetim işler sınıfı.
1. Geçersiz kılma `OnElementChanged` özelleştirmek için özel denetim ve yazma mantık işleyen yöntemi. Bu yöntem aldığında çağrılan karşılık gelen Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) oluşturulur.
1. Ekleme bir `ExportRenderer` özniteliği, Xamarin.Forms özel denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Her platform projesinde özel Oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse, varsayılan oluşturucu hücrenin taban sınıfı için kullanılır.

Aşağıdaki diyagram, her proje örnek uygulamasında, aralarındaki ilişkilerin birlikte sorumlulukları gösterir:

![](listview-images/solution-structure.png "NativeListView özel Oluşturucu Proje Sorumlulukları")

`NativeListView` Öğesinden türetilen tüm hangi platforma özgü Oluşturucu sınıflar tarafından işlenen özel denetim `ListViewRenderer` her platform için sınıf. Bu her sonuçları `NativeListView` platforma özgü liste denetimleri ve yerel hücre düzenleri, aşağıdaki ekran görüntülerinde gösterildiği gibi işlenen özel denetim:

![](listview-images/screenshots.png "Her platformda NativeListView")

`ListViewRenderer` Sınıf çıkarır `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için Xamarin.Forms özel denetim oluşturulduğunda çağrılan yöntemi. Bu yöntem alır bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikleri Xamarin.Forms öğesini temsil, oluşturucu *olan* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olan* bağlı olarak, sırasıyla. Örnek uygulamasında `OldElement` özelliği `null` ve `NewElement` özelliği bir başvuru içerecek `NativeListView` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, yerel denetim özelleştirme gerçekleştirmek için yerdir. Platformda kullanılan yerel denetlemek için belirlenmiş bir başvuru üzerinden erişilen `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetlemek için bir başvuru aracılığıyla elde edilebilir `Element` özelliği.

Olay işleyicileri için abone olurken dikkatli'nin alınması gereken `OnElementChanged` aşağıdaki kod örneğinde gösterildiği şekilde yöntemi:

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

Yalnızca yerel bir denetimi yapılandırılmalıdır ve olay işleyicileri özel Oluşturucu yeni bir Xamarin.Forms öğesi eklendiğinde abone. Benzer şekilde, yalnızca Oluşturucu öğesi değişiklikleri iliştirildiğinde abone tüm olay işleyicileri gelen aboneliği olması gerekir. Bu yaklaşım benimsenmesi bellek sızıntılarını yaşar olmayan özel bir oluşturucu oluşturmak için yardımcı olur.

Geçersiz kılınan bir sürümünü `OnElementPropertyChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, Xamarin.Forms özel denetim bağlanabilirse özelliği değişiklikleri yanıt verecek şekilde yerdir. Bu geçersiz kılma birçok kez çağrılabilir olarak değiştirildiğinde bir özellik için bir onay her zaman yapılmalıdır.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` Xamarin.Forms ile oluşturucuyu kaydeder özniteliği. Öznitelik iki parametre – işlenen Xamarin.Forms özel denetim türü adı ve özel Oluşturucu Tür adını alır. `assembly` Özniteliğine öneki belirtir. öznitelik tüm derlemesi için geçerlidir.

Aşağıdaki bölümlerde her platforma özgü özel Oluşturucu sınıfı uyarlamasını açıklanmaktadır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde iOS platformu için özel Oluşturucu gösterir:

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

`UITableView` Denetim yapılandırılmış bir örneğini oluşturarak `NativeiOSListViewSource` sınıfının yeni bir Xamarin.Forms öğesi özel Oluşturucu bağlı olması koşuluyla. Bu sınıf veri sağlayan `UITableView` kılarak denetim `RowsInSection` ve `GetCell` yöntemleri `UITableViewSource` sınıfı ve göre gösterme bir `Items` görüntülenecek veri listesi içeren özellik. Bu sınıf ayrıca sağladığı bir `RowSelected` çağırır yöntemi geçersiz kılma `ItemSelected` tarafından sağlanan olay `NativeListView` özel denetim. Geçersiz kılmaları yöntemi hakkında daha fazla bilgi için bkz: [sınıflara UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Yöntemi döndürür bir `UITableCellView` , listedeki her satır için verilerle doldurulur ve aşağıdaki kod örneğinde gösterilir:

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

Bu yöntem oluşturur bir `NativeiOSListViewCell` ekranda görüntülenen verileri her satır için örneği. `NativeiOSCell` Örnek her hücre ve hücrenin veri düzenini tanımlar. Bir hücrenin kaydırma nedeniyle ekranından kaybolduğunda, hücre için yeniden kullanıma sunulacaktır. Bu yalnızca sağlayarak İsraf bellek önler `NativeiOSCell` örnekleri ekran, yerine tüm verileri listesinde görüntülenen veriler için. Hücre yeniden hakkında daha fazla bilgi için bkz: [hücre yeniden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Yöntemi de okur `ImageFilename` özelliği her bir satır var, görüntünün okur ve olarak depolar, sağlanan verilerin bir `UIImage` güncelleştirmeden önce örneği `NativeiOSListViewCell` için örnek verileri (ad, kategori ve görüntü) Satır.

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

Bu sınıf hücrenin içeriğinin ve bunların düzeni işlemek için kullanılan denetimleri tanımlar. `NativeiOSListViewCell` Oluşturucusu oluşturur örneklerini `UILabel` ve `UIImageView` denetler ve görünümlerini başlatır. Bu denetimler ile her satırın verileri görüntülemek için kullanılan `UpdateCell` üzerinde bu veri kümesi için kullanılan yöntem `UILabel` ve `UIImageView` örnekleri. Bu örnekler konumunu ayarlama tarafından geçersiz kılınmış `LayoutSubviews` hücre içinde kendi koordinat belirterek yöntemi.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Özel denetim özelliği değişiminde yanıtlama

Varsa `NativeListView.Items` özelliği eklenmesini öğeleri nedeniyle değiştirir veya listesinden kaldırıldı, özel Oluşturucu değişiklikleri görüntüleyerek yanıt vermesi. Bu geçersiz kılma tarafından gerçekleştirilebilir `OnElementPropertyChanged` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Yöntemi, yeni bir örneğini oluşturur `NativeiOSListViewSource` veri sağlayan sınıf `UITableView` denetlemek, bağlanabilirse sağlanan `NativeListView.Items` özelliği değişti.

### <a name="creating-the-custom-renderer-on-android"></a>Android özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde, Android platformu için özel Oluşturucu gösterir:

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

Yerel `ListView` denetim özel Oluşturucu yeni bir Xamarin.Forms öğesi bağlı olması koşuluyla yapılandırılır. Bu yapılandırma örneği oluşturmayı içerir `NativeAndroidListViewAdapter` yerel veri sağlayan sınıf `ListView` denetlemek ve işlemek için bir olay işleyicisi kaydetme `ItemClick` olay. Bu işleyici sırayla çağıracağı `ItemSelected` tarafından sağlanan olay `NativeListView` özel denetim. `ItemClick` Olay aboneliği kaldırılan gelen değişiklikler Xamarin.Forms öğesi Oluşturucu bağlıysa.

`NativeAndroidListViewAdapter` Türetilen `BaseAdapter` sınıfı ve düzenlemenizi sağlayan bir `Items` görüntülenecek veri listesi içeren özellik yanı sıra geçersiz kılma `Count`, `GetView`, `GetItemId`, ve `this[int]` yöntemleri. Bu yöntemi geçersiz kılmalar hakkında daha fazla bilgi için bkz: [bir ListAdapter uygulama](~/android/user-interface/layouts/list-view/populating.md). `GetView` Yöntemi, verilerle doldurmuş her satır için bir görünüm verir ve aşağıdaki kod örneğinde gösterilir:

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

`GetView` Yöntemi işlenmek üzere hücre geri dönmek için çağrılır olarak bir `View`, listedeki verileri her satır için. Oluşturduğu bir `View` örnek görünümünü birlikte ekranda görüntülenen verileri her satır için `View` bir düzen dosyasında tanımlanan örneği. Bir hücrenin kaydırma nedeniyle ekranından kaybolduğunda, hücre için yeniden kullanıma sunulacaktır. Bu yalnızca sağlayarak İsraf bellek önler `View` örnekleri ekran, yerine tüm verileri listesinde görüntülenen veriler için. Görünümü yeniden hakkında daha fazla bilgi için bkz: [satır görünümü yeniden kullanma](~/android/user-interface/layouts/list-view/populating.md).

`GetView` Yöntemi de doldurur `View` görüntü verilerini belirtilen filename okuma dahil verileri örneğiyle `ImageFilename` özelliği.

Yerel olarak her hücre dispayed düzenini `ListView` tanımlanan `NativeAndroidListViewCell.axml` tarafından şişirileceğini düzeni dosyasını `LayoutInflater.Inflate` yöntemi. Aşağıdaki kod örneğinde Düzen tanımını gösterir:

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

Bu iki bu düzeni belirtir `TextView` kontrol eder ve bir `ImageView` denetim hücrenin içeriği görüntülemek için kullanılır. İki `TextView` denetimleri içinde dikey yönelimli bir `LinearLayout` denetimiyle içinde bulunan tüm denetimler bir `RelativeLayout`.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Özel denetim özelliği değişiminde yanıtlama

Varsa `NativeListView.Items` özelliği eklenmesini öğeleri nedeniyle değiştirir veya listesinden kaldırıldı, özel Oluşturucu değişiklikleri görüntüleyerek yanıt vermesi. Bu geçersiz kılma tarafından gerçekleştirilebilir `OnElementPropertyChanged` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

Yöntemi, yeni bir örneğini oluşturur `NativeAndroidListViewAdapter` yerel veri sağlayan sınıf `ListView` denetlemek, bağlanabilirse sağlanan `NativeListView.Items` özelliği değişti.

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Windows Phone üzerinde özel Oluşturucu Oluşturma ve UWP

Aşağıdaki kod örneği, Windows Phone ve UWP için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeWinPhoneListViewRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class NativeWinPhoneListViewRenderer : ListViewRenderer
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

Yerel `ListView` denetim özel Oluşturucu yeni bir Xamarin.Forms öğesi bağlı olması koşuluyla yapılandırılır. Bu yapılandırma ayarı içerir nasıl yerel `ListView` denetim yanıt verecektir seçili öğeleri için Görünüm ve her bir hücre içeriğini tanımlama ve işlemekiçinbirolayişleyicisikaydetmedenetimitarafındangörüntülenenverileridoldurma`SelectionChanged` olay. Bu işleyici sırayla çağıracağı `ItemSelected` tarafından sağlanan olay `NativeListView` özel denetim. `SelectionChanged` Olay aboneliği kaldırılan gelen değişiklikler Xamarin.Forms öğesi Oluşturucu bağlıysa.

Görünüm ve her yerel içeriğini `ListView` hücre tarafından tanımlanan bir `DataTemplate` adlı `ListViewItemTemplate`. Bu `DataTemplate` uygulama düzeyi kaynak sözlükte depolanır ve aşağıdaki kod örneğinde gösterilir:

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

`DataTemplate` Hücre ve düzeni ve görünüm içeriğini görüntülemek için kullanılan denetimleri belirtir. İki `TextBlock` kontrol eder ve bir `Image` denetimi, veri bağlama aracılığıyla hücrenin içeriği görüntülemek için kullanılır. Ayrıca, bir örneğini `ConcatImageExtensionConverter` birleştirmek için kullanılan `.jpg` dosya her görüntü dosya adı uzantısı. Bu sağlar `Image` denetim yüklemek ve olduğunda görüntü işleme `Source` özelliği ayarlanmış.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Özel denetim özelliği değişiminde yanıtlama

Varsa `NativeListView.Items` özelliği eklenmesini öğeleri nedeniyle değiştirir veya listesinden kaldırıldı, özel Oluşturucu değişiklikleri görüntüleyerek yanıt vermesi. Bu geçersiz kılma tarafından gerçekleştirilebilir `OnElementPropertyChanged` aşağıdaki kod örneğinde gösterildiği yöntemi:

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

Yöntem yerel yeniden doldurur `ListView` denetim bağlanabilirse değişen verilerle sağlanan `NativeListView.Items` özelliği değişti.

## <a name="summary"></a>Özet

Bu makalede, yerel liste denetimi performansı hakkında daha fazla denetime izin vererek platforma özgü liste denetimleri ve yerel hücre düzenleri yalıtan özel Oluşturucu Oluşturma gösterilmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererListView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
