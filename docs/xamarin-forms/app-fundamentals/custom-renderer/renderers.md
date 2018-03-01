---
title: "Oluşturucu temel sınıflar ve yerel denetimleri"
description: "Yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu her Xamarin.Forms denetleyebilir. Bu makalede oluşturucuyu ve her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulamak yerel denetim sınıfları listeler."
ms.topic: article
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 95518d9b23db68cc972549c730deeea968512444
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="renderer-base-classes-and-native-controls"></a>Oluşturucu temel sınıflar ve yerel denetimleri

_Yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu her Xamarin.Forms denetleyebilir. Bu makalede oluşturucuyu ve her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulamak yerel denetim sınıfları listeler._

Dışında `MapRenderer` sınıfı, platforma özgü Oluşturucu aşağıdaki ad alanlarında bulunabilir:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (uygulama)** – Xamarin.Forms.Platform.Android.AppCompat
- **Windows Phone 8** – Xamarin.Forms.Platform.WinPhone
- **WinRT** – Xamarin.Forms.Platform.WinRT
- **Evrensel Windows Platformu (UWP)** – Xamarin.Forms.Platform.UWP

`MapRenderer` Sınıfı aşağıdaki ad alanlarında bulunabilir:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Windows Phone 8** – Xamarin.Forms.Maps.WP8
- **WinRT** – Xamarin.Forms.Maps.WinRT
- **Evrensel Windows Platformu (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Sayfaları

Aşağıdaki tabloda her Xamarin.Forms uygulaması yerel denetim sınıfları ve oluşturucu listeler [sayfa](~/xamarin-forms/user-interface/controls/pages.md) türü:

<table>
 <thead>
   <tr>
     <td><strong>Sayfa</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (uygulama)</strong></td>
     <td><strong>Windows Phone 8</strong</td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md">PageRenderer</a></td>
     <td>UIViewController</td>
     <td>ViewGroup</td>
     <td></td>
     <td>Panel</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a></td>
     <td><p>PhoneMasterDetailRenderer (iOS – telefon)</p><p>TabletMasterDetailPageRenderer (iOS – Tablet)</p><p>MasterDetailRenderer (Android)</p><p>MasterDetailPageRenderer (Android AppCompat)</p><p>MasterDetailRenderer (Windows Phone)</p><p>MasterDetailPageRenderer (WinRT)</p></td>
     <td><p>UIViewController (telefon)</p><p>UISplitViewController (Tablet)</p></td>
     <td>DrawerLayout (v4)</td>
     <td>DrawerLayout (v4)</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (özel denetimi)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a></td>
     <td><p>NavigationRenderer (iOS ve Android)</p><p>NavigationPageRenderer (Android AppCompat)</p><p>NavigationPageRenderer (Windows Phone 8 and WinRT)</p></td>
     <td>UIToolbar</td>
     <td>ViewGroup</td>
     <td>ViewGroup</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (özel denetimi)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a></td>
     <td><p>TabbedRenderer (iOS ve Android)</p><p>TabbedPageRenderer (Android AppCompat)</p><p>TabbedPageRenderer (Windows Phone 8 ve WinRT)</p></td>
     <td>UIView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>UIElement (Özet)</td>
     <td>FrameworkElement (Özet)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a></td>
     <td>PageRenderer</td>
     <td>UIViewController</td>
     <td>ViewGroup</td>
     <td></td>
     <td>Panel</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage<a/></td>
     <td>CarouselPageRenderer</td>
     <td>UIScrollView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>UIElement (Panorama)</td>
     <td>FrameworkElement (FlipView)</td>
   </tr>
 </tbody>
</table>

## <a name="layouts"></a>Düzenleri

Aşağıdaki tabloda her Xamarin.Forms uygulaması yerel denetim sınıfları ve oluşturucu listeler [düzeni](~/xamarin-forms/user-interface/controls/layouts.md) türü:

<table>
 <thead>
   <tr>
     <td><strong>Düzen</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Görüntüle</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Görüntüle</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Çerçeve</a></td>
     <td>FrameRenderer</td>
     <td>UIView</td>
     <td>ViewGroup</td>
     <td>Kenarlık</td>
     <td>Kenarlık</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a></td>
     <td>ScrollViewRenderer</td>
     <td>UIScrollView</td>
     <td>ScrollView</td>
     <td>ScrollViewer</td>
     <td>ScrollViewer</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Görüntüle</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Görüntüle</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Kılavuz</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Görüntüle</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Görüntüle</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Görüntüle</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
 </tbody>
</table>

## <a name="views"></a>Görünümler

Aşağıdaki tabloda her Xamarin.Forms uygulaması yerel denetim sınıfları ve oluşturucu listeler [Görünüm](~/xamarin-forms/user-interface/controls/views.md) türü:

<table>
 <thead>
   <tr>
     <td><strong>Görünümler</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (uygulama)</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a></td>
     <td>ActivityIndicatorRenderer</td>
     <td>UIActivityIndicator</td>
     <td>ProgressBar</td>
     <td></td>
     <td>ProgressBar</td>
     <td>ProgressBar</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a></td>
     <td><p>BoxRenderer (iOS ve Android)</p><p>BoxViewRenderer (Windows Phone ve WinRT)</p></td>
     <td>UIView</td>
     <td>ViewGroup</td>
     <td></td>
     <td>Dikdörtgen</td>
     <td>Dikdörtgen</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Düğme</a></td>
     <td>ButtonRenderer</td>
     <td>UIButton</td>
     <td>Düğme</td>
     <td>AppCompatButton</td>
     <td>Düğme</td>
     <td>Düğme</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/">CarouselView</a></td>
     <td>CarouselViewRenderer</td>
     <td>UIScrollView</td>
     <td>RecyclerView</td>
     <td></td>
     <td>FlipView</td>
     <td>FlipView</td>
   </tr>   
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a></td>
     <td>DatePickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>DatePicker</td>
     <td>DatePicker</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Düzenleyicisi</a></td>
     <td>EditorRenderer</td>
     <td>UITextView</td>
     <td>EditText</td>
     <td></td>
     <td>TextBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Giriş</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/entry.md">EntryRenderer</a></td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>PhoneTextBox/PasswordBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Görüntü</a></td>
     <td>ImageRenderer</td>
     <td>UIImageView</td>
     <td>ImageView</td>
     <td></td>
     <td>Görüntü</td>
     <td>Görüntü</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Etiket</a></td>
     <td>LabelRenderer</td>
     <td>UILabel</td>
     <td>Kutusu TextView</td>
     <td></td>
     <td>TextBlock</td>
     <td>TextBlock</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/listview.md">ListViewRenderer</a></td>
     <td>UITableView</td>
     <td>ListView</td>
     <td></td>
     <td>LongListSelector</td>
     <td>ListView</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/">eşleme</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md">MapRenderer</a></td>
     <td>MKMapView</td>
     <td>MapView</td>
     <td></td>
     <td>eşleme</td>
     <td>MapControl</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Picker</a></td>
     <td>PickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td>EditText</td>
     <td>FrameworkElement</td>
     <td>ComboBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a></td>
     <td>ProgressBarRenderer</td>
     <td>UIProgressView</td>
     <td>ProgressBar</td>
     <td></td>
     <td>ProgressBar</td>
     <td>ProgressBar</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a></td>
     <td>SearchBarRenderer</td>
     <td>UISearchBar</td>
     <td>SearchView</td>
     <td></td>
     <td>PhoneTextBox</td>
     <td><p>SearchBox (WinRT)</p><p>AutoSuggestBox (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Kaydırıcı</a></td>
     <td>SliderRenderer</td>
     <td>UISlider</td>
     <td>SeekBar</td>
     <td></td>
     <td>Kaydırıcı</td>
     <td>Kaydırıcı</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Adımlayıcı</a></td>
     <td>StepperRenderer</td>
     <td>UIStepper</td>
     <td>LinearLayout</td>
     <td></td>
     <td>StackPanel kenarlık ve iki düğmeleri</td>
     <td><p>UserControl (WinRT)</p><p>Denetim (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">geçiş</a></td>
     <td>SwitchRenderer</td>
     <td>UISwitch</td>
     <td>Anahtar</td>
     <td>SwitchCompat</td>
     <td>Bir ToggleSwitchButton kenarlık</td>
     <td>ToggleSwitch</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">Tablo görünümü</a></td>
     <td>TableViewRenderer</td>
     <td>UITableView</td>
     <td>ListView</td>
     <td></td>
     <td>Kılavuz TextBlock ve ListBox ile</td>
     <td>ListView</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a></td>
     <td>TimePickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>TimePicker</td>
     <td>TimePicker</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">Web görünümü</a></td>
     <td>WebViewRenderer</td>
     <td>UIWebView</td>
     <td>Web görünümü</td>
     <td></td>
     <td>WebBrowser</td>
     <td>Web görünümü</td>
   </tr>
 </tbody>
</table>

## <a name="cells"></a>Hücreleri

Aşağıdaki tabloda her Xamarin.Forms uygulaması yerel denetim sınıfları ve oluşturucu listeler [hücre](~/xamarin-forms/user-interface/controls/cells.md) türü:

<table>
 <thead>
   <tr>
     <td><strong>Hücreleri</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a></td>
   <td>EntryCellRenderer</td>
   <td>Bir UITextField ile UITableViewCell</td>
   <td>LinearLayout kutusu TextView ve EDITTEXT</td>
   <td>Bir TextBlock ve bir PhoneTextBox içeren bir kılavuz ile DataTemplate</td>
   <td>Metin kutusu ile DataTemplate</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a></td>
   <td>SwitchCellRenderer</td>
   <td>Bir UISwitch ile UITableViewCell</td>
   <td>Anahtar</td>
   <td>Bir ToggleSwitch ile DataTemplate</td>
   <td>DataTemplate TextBlock ve ToggleSwitch içeren bir Kılavuzu</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a></td>
   <td>TextCellRenderer</td>
   <td>UITableViewCell</td>
   <td>İki TextViews ile LinearLayout</td>
   <td>DataTemplate iki TextBlock'lar ile StackPanel içeren bir düğmesi</td>
   <td>İki TextBlock'lar içeren StackPanel ile DataTemplate</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a></td>
   <td>ImageCellRenderer</td>
   <td>Bir UIImage ile UITableViewCell</td>
   <td>İki TextViews ve bir ImageView LinearLayout</td>
   <td>Görüntüyü ve iki TextBlock'lar içeren StackPanel içeren bir kılavuz içeren bir düğme ile DataTemplate</td>
   <td>Görüntüyü ve iki TextBlock'lar içeren bir kılavuz ile DataTemplate</td>  
   </td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/">ViewCell</a></td>
   <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md">ViewCellRenderer</a></td>
   <td>UITableViewCell</td>
   <td>Görüntüle</td>
   <td>Bir ContentPresenter ile DataTemplate</td>
   <td>Bir ContentPresenter ile DataTemplate</td>  
   </td>
 </tr>
 </tbody>
</table>

## <a name="summary"></a>Özet

Bu makalede listelenen oluşturucuyu ve her Xamarin.Forms sayfa düzeni, Görünüm ve hücre uygulamak yerel denetim sınıfları. Yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu her Xamarin.Forms denetleyebilir.



## <a name="related-links"></a>İlgili bağlantılar

- [Özel oluşturucu (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
