---
title: Oluşturucu temel sınıflar ve yerel denetimleri
description: Yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu her Xamarin.Forms denetleyebilir. Bu makalede oluşturucuyu ve her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulamak yerel denetim sınıfları listeler.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 9402bd53ab3bfb0b11182eb700aa560e8f962de3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="renderer-base-classes-and-native-controls"></a>Oluşturucu temel sınıflar ve yerel denetimleri

_Yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu her Xamarin.Forms denetleyebilir. Bu makalede oluşturucuyu ve her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulamak yerel denetim sınıfları listeler._

Dışında `MapRenderer` sınıfı, platforma özgü Oluşturucu aşağıdaki ad alanlarında bulunabilir:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (uygulama)** – Xamarin.Forms.Platform.Android.AppCompat
- **Evrensel Windows Platformu (UWP)** – Xamarin.Forms.Platform.UWP

`MapRenderer` Sınıfı aşağıdaki ad alanlarında bulunabilir:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Evrensel Windows Platformu (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Sayfaları

Aşağıdaki tabloda her Xamarin.Forms uygulaması yerel denetim sınıfları ve oluşturucu listeler [sayfa](~/xamarin-forms/user-interface/controls/pages.md) türü:

|Sayfa|Renderer|iOS|Android|Android (uygulama)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)|PhoneMasterDetailRenderer (iOS – telefon), TabletMasterDetailPageRenderer (iOS – Tablet), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android uygulama), MasterDetailPageRenderer (UWP)|UIViewController (telefon) UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (özel denetimi)|
|[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)|NavigationRenderer (iOS ve Android), NavigationPageRenderer (Android uygulama), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (özel denetimi)|
|[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)|TabbedRenderer (iOS ve Android), TabbedPageRenderer (Android uygulama), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Özet)|
|[`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>Düzenleri

Aşağıdaki tabloda her Xamarin.Forms uygulaması yerel denetim sınıfları ve oluşturucu listeler [düzeni](~/xamarin-forms/user-interface/controls/layouts.md) türü:

|Düzen|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)|ViewRenderer|UIView|Görüntüle|FrameworkElement|
|[`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)|ViewRenderer|UIView|Görüntüle|FrameworkElement|
|[`Frame`](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)|FrameRenderer|UIView|ViewGroup|Kenarlık|
|[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)|ViewRenderer|UIView|Görüntüle|FrameworkElement|
|[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)|ViewRenderer|UIView|Görüntüle|FrameworkElement|
|[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)|ViewRenderer|UIView|Görüntüle|FrameworkElement|
|[`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)|ViewRenderer|UIView|Görüntüle|FrameworkElement|
|[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)|ViewRenderer|UIView|Görüntüle|FrameworkElement|

## <a name="views"></a>Görünümler

Aşağıdaki tabloda her Xamarin.Forms uygulaması yerel denetim sınıfları ve oluşturucu listeler [Görünüm](~/xamarin-forms/user-interface/controls/views.md) türü:

|Görünümler|Renderer|iOS|Android|Android (uygulama)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)|BoxRenderer (iOS ve Android), BoxViewRenderer (Windows Phone ve WinRT)|UIView|ViewGroup||Dikdörtgen|
|[`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)|ButtonRenderer|UIButton|Düğme|AppCompatButton|Düğme|
|[`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/)|CarouselViewRenderer|UIScrollView|RecyclerView||FlipView|
|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)|ImageRenderer|UIImageView|ImageView||Görüntü|
|[`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)|LabelRenderer|UILabel|Kutusu TextView||TextBlock|
|[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)|SliderRenderer|UISlider|SeekBar||Kaydırıcı|
|[`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|StepperRenderer|UIStepper|LinearLayout||Denetim|
|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|SwitchRenderer|UISwitch|Anahtar|SwitchCompat|ToggleSwitch|
|[`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)|WebViewRenderer|UIWebView|Web görünümü||Web görünümü|

## <a name="cells"></a>Hücreleri

Aşağıdaki tabloda her Xamarin.Forms uygulaması yerel denetim sınıfları ve oluşturucu listeler [hücre](~/xamarin-forms/user-interface/controls/cells.md) türü:

|Hücreleri|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/)|EntryCellRenderer|Bir UITextField ile UITableViewCell|LinearLayout kutusu TextView ve EDITTEXT|Metin kutusu ile DataTemplate|
|[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/)|SwitchCellRenderer|Bir UISwitch ile UITableViewCell|Anahtar|DataTemplate TextBlock ve ToggleSwitch içeren bir Kılavuzu|
|[`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/)|TextCellRenderer|UITableViewCell|İki TextViews ile LinearLayout|İki TextBlock'lar içeren StackPanel ile DataTemplate|
|[`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)|ImageCellRenderer|Bir UIImage ile UITableViewCell|İki TextViews ve bir ImageView LinearLayout|Görüntüyü ve iki TextBlock'lar içeren bir kılavuz ile DataTemplate|
|[`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Görüntüle|Bir ContentPresenter ile DataTemplate|

## <a name="summary"></a>Özet

Bu makalede listelenen oluşturucuyu ve her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulamak yerel denetim sınıfları. Yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu her Xamarin.Forms denetleyebilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Özel oluşturucu (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
