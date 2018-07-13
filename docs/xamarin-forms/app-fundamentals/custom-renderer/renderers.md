---
title: Oluşturucu temel sınıfları ve yerel denetimleri
description: Yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici her Xamarin.Forms denetleyebilir. Bu makalede her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulayan yerel denetim sınıfları ve işleyici listeler.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 01610581a0fd72401cb6daa4d5c8c2cb99fe6a2f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995114"
---
# <a name="renderer-base-classes-and-native-controls"></a>Oluşturucu temel sınıfları ve yerel denetimleri

_Yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici her Xamarin.Forms denetleyebilir. Bu makalede her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulayan yerel denetim sınıfları ve işleyici listeler._

Dışında `MapRenderer` sınıfı, platforma özel Oluşturucu aşağıdaki alanlarında bulunabilir:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Evrensel Windows Platformu (UWP)** – Xamarin.Forms.Platform.UWP

`MapRenderer` Sınıf şu ad alanlarında bulunabilir:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Evrensel Windows Platformu (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Sayfaları

Aşağıdaki tabloda her Xamarin.Forms uygulayan yerel denetim sınıfları ve işleyici listeler [sayfa](~/xamarin-forms/user-interface/controls/pages.md) türü:

|Sayfa|Oluşturucu|iOS|Android|Android (uygulama)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|Uıviewcontroller|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS – telefon), TabletMasterDetailPageRenderer (iOS – Tablet), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android AppCompat) MasterDetailPageRenderer (UWP)|Uıviewcontroller'ı (Phone) UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (özel denetimi)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|(İOS ve Android) NavigationRenderer NavigationPageRenderer (Android AppCompat), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (özel denetimi)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|(İOS ve Android) TabbedRenderer TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UWP)|Uıview|ViewPager|ViewPager|FrameworkElement (Özet)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|Uıviewcontroller|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>Düzenleri

Aşağıdaki tabloda her Xamarin.Forms uygulayan yerel denetim sınıfları ve işleyici listeler [Düzen](~/xamarin-forms/user-interface/controls/layouts.md) türü:

|Düzen|Oluşturucu|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|Uıview|Görüntüle|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|Uıview|Görüntüle|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|Uıview|ViewGroup|Kenarlık|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|Uıview|Görüntüle|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|Uıview|Görüntüle|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|Uıview|Görüntüle|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|Uıview|Görüntüle|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|Uıview|Görüntüle|FrameworkElement|

## <a name="views"></a>Görünümler

Aşağıdaki tabloda her Xamarin.Forms uygulayan yerel denetim sınıfları ve işleyici listeler [görünümü](~/xamarin-forms/user-interface/controls/views.md) türü:

|Görünümler|Oluşturucu|iOS|Android|Android (uygulama)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS ve Android), BoxViewRenderer (UWP)|Uıview|ViewGroup||Dikdörtgen|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|Düğme|AppCompatButton|Düğme|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Görüntü|
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Kaydırıcı|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Denetim|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Anahtar|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WebViewRenderer|UIWebView|Web görünümü||Web görünümü|

## <a name="cells"></a>Hücreleri

Aşağıdaki tabloda her Xamarin.Forms uygulayan yerel denetim sınıfları ve işleyici listeler [hücre](~/xamarin-forms/user-interface/controls/cells.md) türü:

|Hücreleri|Oluşturucu|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|Bir UITextField ile UITableViewCell|Bir TextView ve EDITTEXT LinearLayout|DataTemplate ile bir metin kutusu|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|Bir UISwitch ile UITableViewCell|Anahtar|DataTemplate ile bir TextBlock ve ToggleSwitch içeren kılavuz|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|İki TextViews ile LinearLayout|DataTemplate ile iki TextBlock'lar içeren StackPanel|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|Bir UIImage ile UITableViewCell|İki TextViews ve bir ImageView LinearLayout|DataTemplate ile resim ve iki TextBlock'lar içeren kılavuz|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Görüntüle|DataTemplate ile bir ContentPresenter|

## <a name="summary"></a>Özet

Bu makalede listelenen oluşturucusu ve her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulayan yerel denetim sınıfları. Yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici her Xamarin.Forms denetleyebilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Özel oluşturucular (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
