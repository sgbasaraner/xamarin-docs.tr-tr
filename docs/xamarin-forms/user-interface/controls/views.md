---
title: Xamarin.Forms görünümleri
description: Xamarin.Forms platformlar arası mobil kullanıcı arabirimleri yapı taşları görünümleridir. Xamarin.Forms içinde bulunan görünümler bu makalede listelenmektedir.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 52d8d5f6eb38e5cb501d6284d08f7317981e0dcf
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998979"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms görünümleri

_Xamarin.Forms platformlar arası mobil kullanıcı arabirimleri yapı taşları görünümleridir._

Etiketler, düğmeler ve yaygın olarak bilinen kaydırıcılar gibi kullanıcı arabirimi nesneleri görünümleridir *denetimleri* veya *pencere öğeleri* diğer grafik programlama ortamlarında. Tüm türetilen Xamarin.Forms tarafından desteklenen görünümler [ `View` ](xref:Xamarin.Forms.View) sınıfı. Birden fazla kategoriye ayrılabilir:

## <a name="views-for-presentation"></a>Sunum görünümleri

### <a name="label"></a>Etiketle

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) tek satırlı metin dizelerini veya çok satırlı bloklar metin, sabit veya değişken biçimlendirme ile birlikte görüntüler. Ayarlama [ `Text` ](xref:Xamarin.Forms.Label.Text) sabit biçimlendirme veya kümesi için bir dize özelliğini [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) özelliğini bir [ `FormattedString` ](xref:Xamarin.Forms.FormattedString) nesne değişkeni biçimlendirme.<br /><br />[API belgeleri](xref:Xamarin.Forms.Label) / [Kılavuzu](~/xamarin-forms/user-interface/text/label.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Etiket örnek](views-images/Label.png "etiket örnek")](views-images/Label-Large.png#lightbox "etiket örneği")<br /> [Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Görüntü

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) bir bit eşlem görüntüler. Bit eşlemler Ortak proje veya platform projelerde kaynak olarak gömülü veya bir .NET kullanılarak oluşturulan Web üzerinden indirilebilir `Stream` nesne.<br /><br />[API belgeleri](xref:Xamarin.Forms.Image) / [Kılavuzu](~/xamarin-forms/user-interface/images.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Görüntü örnek](views-images/Image.png "görüntü örnek")](views-images/Image-Large.png#lightbox "görüntü örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) dolu bir dikdörtgen göre renklendirilmiş görüntüler [ `Color` ](xref:Xamarin.Forms.BoxView.Color) özelliği. `BoxView` Varsayılan boyut isteği 40 x 40 sahiptir. Diğer boyutları için Ata [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) ve [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) özellikleri.<br /><br />[API belgeleri](xref:Xamarin.Forms.BoxView) / [Kılavuzu](~/xamarin-forms/user-interface/boxview.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), ve [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![BoxView örnek](views-images/BoxView.png "BoxView örnek")](views-images/BoxView-Large.png#lightbox "BoxView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>Web görünümü

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) Web sayfalarını veya HTML içeriğini, bağlı görüntüler [ `Source` ](xref:Xamarin.Forms.WebView.Source) özelliği bir [ `UriWebViewSource` ](xref:Xamarin.Forms.UrlWebViewSource) veya [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource) nesne.<br /><br />[API belgeleri](xref:Xamarin.Forms.WebView) / [Kılavuzu](~/xamarin-forms/user-interface/webview.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) ve [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![WebView örnek](views-images/WebView.png "WebView örnek")](views-images/WebView-Large.png#lightbox "WebView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) iOS ve Android projeleri OpenGL grafik görüntüler. Evrensel Windows platformu için desteği yoktur. İOS ve Android projeleri için bir başvuru gerektirir **OpenTK 1.0** derleme veya **OpenTK** sürüm 1.0.0.0 derleme. `OpenGLView` Paylaşılan bir projede kullanmak kolaydır; .NET Standard kitaplığa kullandıysanız, ardından bir bağımlılık hizmeti de (örnek kodda gösterildiği gibi) gerekli olacaktır.<br /><br />Xamarin.Forms içinde yerleşik yalnızca grafik tesis budur ancak grafikler kullanarak bir Xamarin.Forms uygulaması da işleyebilirsiniz [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), veya [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[API belgeleri](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![OpenGLView örnek](views-images/OpenGLView.png "OpenGLView örnek")](views-images/OpenGLView-Large.png#lightbox "OpenGLView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) ile [arka plan kod](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Harita

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) bir harita görüntülenir. **Xamarin.Forms.Maps** Nuget paketi yüklü olmalıdır. Android ve evrensel Windows platformu eşlemesi yetkilendirme anahtarı gerektirir.<br /><br />[API belgeleri](xref:Xamarin.Forms.Maps.Map) / [Kılavuzu](~/xamarin-forms/user-interface/map.md) / [örnek](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Örnek eşleme](views-images/Map.png "harita örneği")](views-images/Map-Large.png#lightbox "harita örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Komutları başlatmak görünümleri

### <a name="button"></a>Düğme

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) metin görüntüleyen bir dikdörtgen nesnesi ve tetiklenen bir [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) basılı olay.<br /><br />[API belgeleri](xref:Xamarin.Forms.Button) / [Kılavuzu](~/xamarin-forms/user-interface/button.md) / [örnek](https://developer.xamarin.com/samples/UserInterface/ButtonDemos/) | [![Düğme örnek](views-images/Button.png "düğmesini örnek")](views-images/Button-Large.png#lightbox "düğmesi örneği")<br /> [Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) ile [arka plan kod](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) bir alan için bir arama yapmak için uygulama sinyalleri kullanıcı türü bir metin dizesi ve bir düğme (veya bir klavye anahtarı) görüntüler. [ `Text` ](xref:Xamarin.Forms.SearchBar.Text) Özelliği metin erişim sağlar ve [ `SearchButtonPressed` ](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) olay düğmeye basıldığını belirtir.<br /><br />[API belgeleri](xref:Xamarin.Forms.SearchBar) | [![SearchBar örnek](views-images/SearchBar.png "SearchBar örnek")](views-images/SearchBar-Large.png#lightbox "SearchBar örneği")<br /> [Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) ile [arka plan kod](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Görünümleri, değerleri ayarlamak için

### <a name="slider"></a>Kaydırıcı

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) Kullanıcının seçmesine olanak sağlayan bir `double` değeri ile belirtilen sürekli bir aralıktan [ `Minimum` ](xref:Xamarin.Forms.Slider.Minimum) ve [ `Maximum` ](xref:Xamarin.Forms.Slider.Maximum) özellikleri.<br /><br />[API belgeleri](xref:Xamarin.Forms.Slider) / [Kılavuzu](~/xamarin-forms/user-interface/slider.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) | [![Kaydırıcı örnek](views-images/Slider.png "kaydırıcı örnek")](views-images/Slider-Large.png#lightbox "kaydırıcı örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Adımlayıcıdaki

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) Kullanıcının seçmesine olanak sağlayan bir `double` belirtilen artımlı değerleri aralığı değerinden [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum), [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum), ve [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) özellikleri.<br /><br />[API belgeleri](xref:Xamarin.Forms.Stepper) | [![Adımlayıcıdaki örnek](views-images/Stepper.png "adımlayıcıdaki örnek")](views-images/Stepper-Large.png#lightbox "adımlayıcıdaki örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Anahtar

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) bir Boolean değer seçmek izin vermek için bir açma/kapatma düğmesi biçimini alır. [ `IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) Anahtar durumu bir özelliktir ve [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) durumu değiştiğinde olay harekete geçirilir.<br /><br />[API belgeleri](xref:Xamarin.Forms.Switch) | [![Örnek geçiş](views-images/Switch.png "geçiş örnek")](views-images/Switch-Large.png#lightbox "geçiş örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) platform tarih seçici ile bir tarih seçmek üzere kullanıcının sağlar. İle verilen tarih aralığı ayarlayın [ `MinimumDate` ](xref:Xamarin.Forms.DatePicker.MinimumDate) ve [ `MaximumDate` ](xref:Xamarin.Forms.DatePicker.MaximumDate) özellikleri. [ `Date` ](xref:Xamarin.Forms.DatePicker.Date) Seçilen tarihten özelliğidir ve [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) bu özellik değiştiğinde olay harekete geçirilir.<br /><br />[API belgeleri](xref:Xamarin.Forms.DatePicker) / [Kılavuzu](~/xamarin-forms/user-interface/datepicker.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![DatePicker örnek](views-images/DatePicker.png "DatePicker örnek")](views-images/DatePicker-Large.png#lightbox "DatePicker örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) bir saat ile platform Saat Seçici seçmesini sağlar. [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) Seçilen zamanda bir özelliktir. Uygulamanın değişiklikleri izleyebilirsiniz `Time` özelliği için bir işleyici yükleyerek [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) olay.<br /><br />[API belgeleri](xref:Xamarin.Forms.TimePicker) | [![TimePicker örnek](views-images/TimePicker.png "TimePicker örnek")](views-images/TimePicker-Large.png#lightbox "TimePicker örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Metin düzenleme görünümleri

Bu iki sınıfların türetilmesi [ `InputView` ](xref:Xamarin.Forms.InputView) tanımlayan sınıf [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) özelliği.

<a name="entry" />

### <a name="entry"></a>Giriş

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) Kullanıcının tek satırlık metin girmenizi ve düzenlemenizi sağlar. Metin olarak kullanılabilir [ `Text` ](xref:Xamarin.Forms.Entry.Text) özelliği ve [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) ve [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) olayları harekete metin değişikliklerini veya kullanıcı tamamlandığında, enter tuşuna dokunarak bildirir.<br /><br />Kullanım bir [ `Editor` ](#editor) girmeyi ve düzenlemeyi birden çok metin satırı için.<br /><br />[API belgeleri](xref:Xamarin.Forms.Entry) / [Kılavuzu](~/xamarin-forms/user-interface/text/entry.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Giriş örnek](views-images/Entry.png "giriş örnek")](views-images/Entry-Large.png#lightbox "giriş örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Düzenleyici

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) Kullanıcının birden çok metin satırı girmenizi ve düzenlemenizi sağlar. Metin olarak kullanılabilir [ `Text` ](xref:Xamarin.Forms.Editor.Text) özelliği ve [ `TextChanged` ](xref:Xamarin.Forms.Editor.TextChanged) ve [ `Completed` ](xref:Xamarin.Forms.Editor.Completed) olayları harekete metin değişikliklerini veya kullanıcı Tamamlama bildirir.<br /><br />Kullanım bir [ `Entry` ](#entry) girme ve tek satırlık metin düzenleme görünümü.<br /><br />[API belgeleri](xref:Xamarin.Forms.Editor) / [Kılavuzu](~/xamarin-forms/user-interface/text/editor.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Giriş örnek](views-images/Editor.png "Düzenleyicisi örnek")](views-images/Editor-Large.png#lightbox "Düzenleyici örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Etkinlik belirtmek için görünümleri

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) ilerlemenin inceleyip vermeden uygulama uzun etkinliğinde ilgilendiğini göstermek için bir animasyon kullanır. [ `IsRunning` ](xref:Xamarin.Forms.ActivityIndicator.IsRunning) Özellik animasyon denetler.<br /><br />Etkinliğin ilerleme biliniyorsa kullanmak bir [ `ProgressBar` ](#progressbar) yerine.<br /><br />[API belgeleri](xref:Xamarin.Forms.ActivityIndicator) | [![ActivityIndicator örnek](views-images/ActivityIndicator.png "ActivityIndicator örnek")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) Uygulama uzun bir etkinlik İlerliyor göstermek için bir animasyon kullanır. Ayarlama [ `Progress` ](xref:Xamarin.Forms.ProgressBar.Progress) özelliğini 0 ve ilerlemeyi göstermek için 1 arasındaki değerleri.<br /><br />Etkinliğin ilerleme durumu bilinmiyor kullanırsanız bir [ `ActivityIndicator` ](#activityindicator) yerine.<br /><br />[API belgeleri](xref:Xamarin.Forms.ProgressBar) | [![ProgressBar örnek](views-images/ProgressBar.png "ProgressBar örnek")](views-images/ProgressBar-Large.png#lightbox "ProgressBar örnek")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) ile [arka plan kod](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Koleksiyonları görüntüleyen görüşnümleri

### <a name="picker"></a>Seçici

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) Listeden seçilen bir öğeyi metin dizesi görüntüleyen ve görünümü dokunulduğunda bu öğe seçilmesine olanak sağlar. Ayarlama [ `Items` ](xref:Xamarin.Forms.Picker.Items) dizelerden oluşan bir liste özelliğini veya [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) özelliğini nesneleri koleksiyonu. [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Bir öğe seçildiğinde olay harekete geçirilir.<br /><br />`Picker` Yalnızca seçildiğinde öğelerin listesini görüntüler. Kullanım bir [ `ListView` ](#listView) veya [ `TableView` ](#tableView) sayfada kalır kaydırılabilir bir listesi.<br /><br />[API belgeleri](xref:Xamarin.Forms.Picker) / [Kılavuzu](~/xamarin-forms/user-interface/picker/index.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Seçici örneği](views-images/Picker.png "Seçici örneği")](views-images/Picker-Large.png#lightbox "Seçici örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) ile [arka plan kod](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) öğesinden türetilen [ `ItemsView[Cell]` ](xref:Xamarin.Forms.ItemsView`1) ve kaydırılabilir seçilebilir veri öğeleri listesini görüntüler. Ayarlayın [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) nesne kümesi ve bir koleksiyon özelliğine [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) özelliğini bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) öğeleri şeklini tanımlayan nesne. biçimlendirilecek. [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Olay sinyalini verir seçimi, olarak kullanılabildiği yapılmadığını [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) özelliği.<br /><br />[API belgeleri](xref:Xamarin.Forms.ListView) / [Kılavuzu](~/xamarin-forms/user-interface/listview/index.md) / [örnek](https://developer.xamarin.com/samples/WorkingWithListview) | [![ListView örnek](views-images/ListView.png "ListView örnek")](views-images/ListView-Large.png#lightbox "ListView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>Tablo görünümü

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) Satır türü içeren bir liste görüntüler [ `Cell` ](xref:Xamarin.Forms.Cell) isteğe bağlı üst bilgiler ve alt başlıkları eklemek. Ayarlama [ `Root` ](xref:Xamarin.Forms.TableView.Root) türünde bir nesne özelliğini [ `TableRoot` ](xref:Xamarin.Forms.TableRoot)ve ekleme [ `TableSection` ](xref:Xamarin.Forms.TableSection) , nesnelere `TableRoot`. Her `TableSection` koleksiyonudur `Cell` nesneleri.<br /><br />[API belgeleri](xref:Xamarin.Forms.TableView) / [Kılavuzu](~/xamarin-forms/user-interface/tableview.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Tablo görünümü örnek](views-images/TableView.png "Tablo görünümü örnek")](views-images/TableView-Large.png#lightbox "Tablo görünümü örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms'a giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery örnek](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API belgeleri](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
