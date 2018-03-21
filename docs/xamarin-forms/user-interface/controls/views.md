---
title: "Xamarin.Forms görünümleri"
description: "Xamarin.Forms görünümleri platformlar arası mobil kullanıcı arabirimleri yapı taşlarıdır."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ef4de2d544f3bcfb661b29dd90de738ae0442373
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms görünümleri

_Xamarin.Forms görünümleri platformlar arası mobil kullanıcı arabirimleri yapı taşlarıdır._

Etiketler, düğmeler ve yaygın olarak da bilinir kaydırıcılar gibi kullanıcı arabirimi nesneleri görünümlerdir *denetimleri* veya *pencere öğeleri* diğer grafik programlama ortamlarda. Tüm türetilen Xamarin.Forms tarafından desteklenen görünümler [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) sınıfı. Birden fazla kategoriye ayrılabilir:

## <a name="views-for-presentation"></a>Sunu Görünümleri

### <a name="label"></a>Etiketle

|     |     |
| --- | --- |
| [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) tek satırlı metin dizesi veya metin, sabit veya değişken biçimlendirmeye sahip ya da çok satırlı bloklarını görüntüler. Ayarlama [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) sabit biçimlendirme veya kümesi için bir dize özelliği [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) özelliğine bir [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/) nesne değişkeni için biçimlendirme.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) / [Kılavuzu](~/xamarin-forms/user-interface/text/label.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Etiket örnek](views-images/Label.png "etiket örnek")](views-images/Label-Large.png#lightbox "etiket örneği")<br /> [Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Görüntü

|     |     |
| --- | --- |
| [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) bir bit eşlem görüntüler. Bit eşlemler Ortak proje veya platform projeleri kaynaklar olarak katıştırılmış veya bir .NET kullanılarak oluşturulan Web üzerinden indirilebilir `Stream` nesnesi.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) / [Kılavuzu](~/xamarin-forms/user-interface/images.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Görüntü örnek](views-images/Image.png "görüntü örnek")](views-images/Image-Large.png#lightbox "görüntü örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) tarafından renkli düz bir dikdörtgen görüntüler [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) özelliği. `BoxView` Varsayılan boyut istek 40 x 40 sahiptir. Diğer boyutlara için Ata [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) ve [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) özellikleri.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) / [Kılavuzu](~/xamarin-forms/user-interface/boxview.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), ve [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![BoxView örnek](views-images/BoxView.png "BoxView örnek")](views-images/BoxView-Large.png#lightbox "BoxView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>Web görünümü

|     |     |
| --- | --- |
| [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Web sayfaları veya HTML içeriği olup olmadığına göre dayanarak görüntüler [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) özelliği ayarlanmış bir [ `UriWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UrlWebViewSource/) veya bir [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/) nesnesi.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) / [Kılavuzu](~/xamarin-forms/user-interface/webview.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) ve [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![Web görünümü örnek](views-images/WebView.png "WebView örnek")](views-images/WebView-Large.png#lightbox "Web görünümü örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/) OpenGL grafikler, iOS ve Android projeleri görüntüler. Evrensel Windows platformu için desteği yoktur. İOS ve Android projelerine başvuru gerektiren **OpenTK 1.0** derleme veya **OpenTK** sürüm 1.0.0.0 derleme. `OpenGLView` Paylaşılan bir projede kullanmak daha kolay olur; bir PCL ya da .NET standart kitaplığında kullandıysanız, ardından bir bağımlılık hizmeti de (örnek kodda gösterildiği gibi) gerekir.<br /><br />Bu Xamarin.Forms yerleşik yalnızca grafik tesis olmakla birlikte, bir Xamarin.Forms uygulaması, ayrıca grafikleri kullanma işleyebilen [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), veya [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/)<br /><br /> | [![OpenGLView örnek](views-images/OpenGLView.png "OpenGLView örnek")](views-images/OpenGLView-Large.png#lightbox "OpenGLView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) ile [arka plan kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>eşleme

|     |     |
| --- | --- |
| [`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) bir harita görüntüler. **Xamarin.Forms.Maps** Nuget paketi yüklü olmalıdır. Android ve evrensel Windows platformu bir harita yetkilendirme anahtarı gerektirir.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) / [Kılavuzu](~/xamarin-forms/user-interface/map.md) / [örnek](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Örnek eşleme](views-images/Map.png "eşleme örnek")](views-images/Map-Large.png#lightbox "eşleme örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Komutları başlatmak görünümleri

### <a name="button"></a>Düğme

|     |     |
| --- | --- |
| [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) metin, görüntüler dikdörtgen bir nesnedir ve hangi ateşlenir bir [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) tuşuna bastığınızda, olay.<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) | [![Düğme örnek](views-images/Button.png "düğmesini örnek")](views-images/Button-Large.png#lightbox "düğmesi örneği")<br /> [Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) ile [arka plan kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) bir alan için bir arama yapmak için uygulama sinyalleri türü bir metin dizesi ve bir düğme (veya bir klavye anahtarı) kullanıcıya görüntüler. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.Text/) Özellik metin erişim sağlar ve [ `SearchButtonPressed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/) olayı, düğmesine basıldığında olduğunu gösterir.<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) | [![SearchBar örnek](views-images/SearchBar.png "SearchBar örnek")](views-images/SearchBar-Large.png#lightbox "SearchBar örneği")<br /> [Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) ile [arka plan kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Ayar değerleri için görünümleri 

### <a name="slider"></a>Kaydırıcı

|     |     |
| --- | --- |
| [`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) Kullanıcının seçmesine olanak sağlayan bir `double` ile belirtilen sürekli bir aralık değerinden [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) ve [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) özellikleri.<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) | [![Kaydırıcı örnek](views-images/Slider.png "kaydırıcı örnek")](views-images/Slider-Large.png#lightbox "kaydırıcı örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Adımlayıcı

|     |     |
| --- | --- |
| [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Kullanıcının seçmesine olanak sağlayan bir `double` ile belirtilen artımlı değerleri aralığı değerinden [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Minimum/), [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Maximum/), ve [ `Increment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) özellikleri.<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) | [![Adımlayıcı örnek](views-images/Stepper.png "Adımlayıcı örnek")](views-images/Stepper-Large.png#lightbox "Adımlayıcı örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Anahtar 

|     |     |
| --- | --- |
| [`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) kullanıcının bir Boole değeri seçmesine izin veren bir açık/kapalı anahtar biçimini alır. [ `IsToggled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) Anahtarının durumunu bir özelliktir ve [ `Toggled` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) durumu değiştiğinde olay tetiklenir.<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) | [![Geçiş örnek](views-images/Switch.png "geçiş örnek")](views-images/Stepper-Large.png#lightbox "geçiş örnek")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) bir tarih ile platform tarih seçici seçmesini sağlar. İle izin verilen tarih aralığını ayarlamak [ `MinimumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) ve [ `MaximumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) özellikleri. [ `Date` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) Seçilen tarihten bir özelliktir ve [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) bu özelliği değiştiğinde olay tetiklenir.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) / [Kılavuzu](~/xamarin-forms/user-interface/datepicker.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![DatePicker örnek](views-images/DatePicker.png "DatePicker örnek")](views-images/DatePicker-Large.png#lightbox "DatePicker örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) bir saat ile platform Saat Seçici seçmesini sağlar. [ `Time` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) Özelliği, seçilen süre. Bir uygulama değişiklikleri izleyebilirsiniz `Time` özelliği için bir işleyici yükleyerek [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) olay.<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) | [![TimePicker örnek](views-images/TimePicker.png "TimePicker örnek")](views-images/TimePicker-Large.png#lightbox "TimePicker örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Metin düzenleme için görünümleri

Bu iki sınıf türetin [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) tanımlar sınıfı [ `Keyboard` ](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) özelliği.

<a name="entry" />

### <a name="entry"></a>Giriş

|     |     |
| --- | --- |
| [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kullanıcının girin ve tek satırlık metin düzenleme olanak sağlar. Metin olarak kullanılabilir [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) özelliği ve [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) ve [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) olaylar ne zaman metin değişikliklerini veya kullanıcı Tamamlama enter tuşuna dokunarak işaret eder.<br /><br />Kullanım bir [ `Editor` ](#editor) girme ve birkaç satırlık metin düzenleme için.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) / [Kılavuzu](~/xamarin-forms/user-interface/text/entry.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Giriş örnek](views-images/Entry.png "girişi örnek")](views-images/Entry-Large.png#lightbox "giriş örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Düzenleyici

|     |     |
| --- | --- |
| [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) kullanıcının girin ve birkaç satırlık metin düzenleme olanak sağlar. Metin olarak kullanılabilir [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Editor.Text/) özelliği ve [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) ve [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) olaylar ne zaman metin değişikliklerini veya kullanıcı Tamamlama işaret eder.<br /><br />Kullanım bir [ `Entry` ](#entry) girme ve tek satırlık metin düzenleme görünümü.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) / [Kılavuzu](~/xamarin-forms/user-interface/text/editor.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Giriş örnek](views-images/Editor.png "Düzenleyicisi örnek")](views-images/Editor-Large.png#lightbox "Düzenleyici örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Etkinlik belirtmek için görünümleri

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) devam eden herhangi bir gösterge vermeden uygulama uzun bir etkinlikte ilgilendiğini göstermek için bir animasyon kullanır. [ `IsRunning` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) Özellik, animasyon denetler.<br /><br />Etkinliğin ilerleme bilinen kullanırsanız bir [ `ProgressBar` ](#progressbar) yerine.<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) | [![ActivityIndicator örnek](views-images/ActivityIndicator.png "ActivityIndicator örnek")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) bir animasyon uygulama uzun bir etkinlik işlendiğini göstermek için kullanır. Ayarlama [ `Progress` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ProgressBar.Progress/) değerleri 0 ile 1 ilerlemeyi göstermek için arasında özelliğine.<br /><br />Etkinliğin ilerleme durumu bilinmiyor kullanırsanız bir [ `ActivityIndicator` ](#activityindicator) yerine.<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) | [![ProgressBar örnek](views-images/ProgressBar.png "ProgressBar örnek")](views-images/ProgressBar-Large.png#lightbox "ProgressBar örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) ile [arka plan kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Koleksiyonları görüntüleyen veri görünümleri

### <a name="picker"></a>Picker

|     |     |
| --- | --- |
| [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) metin dizesinin bir listeden seçili bir öğenin görüntüler ve görünüm dokunduğunuz olduğunda bu öğeyi seçerek verir. Ayarlama [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) dizeleri listesini özelliğine veya [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) nesneler koleksiyonunu özelliğine. [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Bir öğe seçildiğinde olay tetiklenir.<br /><br />`Picker` Yalnızca seçildiğinde öğeleri listesini görüntüler. Kullanım bir [ `ListView` ](#listView) veya [ `TableView` ](#tableView) sayfada kalır kaydırılabilir bir listesi.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) / [Kılavuzu](~/xamarin-forms/user-interface/picker/index.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Seçici örneği](views-images/Picker.png "Seçici örneği")](views-images/Picker-Large.png#lightbox "Seçici örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) ile [arka plan kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) türetilen [ `ItemsView[Cell]` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) ve kaydırılabilir seçilebilir veri öğeleri listesini görüntüler. Ayarlama [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) nesneleri ve kümesi koleksiyonunu özelliğine [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) özelliğine bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) nasıl öğeleri açıklayan nesnesi biçimlendirilecek. [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Olay sinyalleri seçimi, olarak kullanılabilir olduğu yapılmadığını [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) özelliği.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) / [Kılavuzu](~/xamarin-forms/user-interface/listview/index.md) / [örnek](https://developer.xamarin.com/samples/WorkingWithListview) | [![ListView örnek](views-images/ListView.png "ListView örnek")](views-images/ListView-Large.png#lightbox "ListView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>Tablo görünümü

|     |     |
| --- | --- |
| [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) Satır türü görüntüler [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) isteğe bağlı üstbilgi ve subheaders ile. Ayarlama [ `Root` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) türünde bir nesne için özellik [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)ve ekleme [ `TableSection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) , nesnelere `TableRoot`. Her `TableSection` koleksiyonudur `Cell` nesneleri.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) / [Kılavuzu](~/xamarin-forms/user-interface/tableview.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Tablo görünümü örnek](views-images/TableView.png "Tablo görünümü örnek")](views-images/TableView-Large.png#lightbox "Tablo görünümü örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery örnek](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API belgeleri](https://developer.xamarin.com/api/root/Xamarin.Forms/)
