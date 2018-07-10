---
title: Bölüm 15 özeti. Etkileşimli arabirimi
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 15 özeti. Etkileşimli arabirimi'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 6d26e3b9a82917ec3f70190e5e90c59d274de990
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935192"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Bölüm 15 özeti. Etkileşimli arabirimi

Bu bölümde sekiz keşfediyor `View` kullanıcının etkileşime izin vermek türevleri.

## <a name="view-overview"></a>Görünüm genel bakış

Xamarin.Forms öğesinden türetilen 20 instantiable sınıfları içeren `View` ama `Layout`. Bu altı önceki bölümlerde ele alınmış:

- `Label`: [ **Bölüm 2. Bir uygulamanın anatomisi**](chapter02.md)
- `BoxView`: [ **Bölüm 3. Yığını kaydırma**](chapter03.md)
- `Button`: [ **Bölüm 6. Düğme tıklamaları**](chapter06.md)
- `Image`: [ **13. bölüm. Bit eşlemler**](chapter13.md)
- `ActivityIndicator`: [ **13. bölüm. Bit eşlemler**](chapter13.md)
- `ProgressBar`: [ **Bölüm 14. AbsoluteLayout**](chapter14.md)

Bu bölümde sekiz görünümlerinde etkili bir şekilde temel .NET veri türleri ile etkileşim kurmak kullanıcı izin ver:

|Veri türü|Görünümler|
|--- |--- |
|`Double`|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/), [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|
|`Boolean`|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|
|`String`|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/), [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|
|`DateTime`|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/), [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|

Bu görünümler temel alınan veri türleri etkileşimli görsel gösterimi düşünebilirsiniz. Bu kavramı daha sonraki bölümde, araştırılan [ **Bölüm 16. Veri bağlama**](chapter16.md).

Kalan altı görünümler, aşağıdaki bölümlerde ele alınmaktadır:

- `WebView`: [ **Bölüm 16. Veri bağlama**](chapter16.md)
- `Picker`: [ **Bölüm 19. Koleksiyon görünümleri**](chapter19.md)
- `ListView`: [ **Bölüm 19. Koleksiyon görünümleri**](chapter19.md)
- `TableView`: [ **Bölüm 19. Koleksiyon görünümleri**](chapter19.md)
- `Map`: [ **Bölüm 28. Konum ve haritalar**](chapter28.md)
- `OpenGLView`: Bu kitap (ve Windows platformları için destek yok) kapsamında değil

## <a name="slider-and-stepper"></a>Kaydırıcı ve adımlayıcıdaki

Her ikisi de [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) ve [ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) sayısal bir değer arasından seçim izin verin. `Slider` Sırasında sürekli bir aralık `Stepper` ayrık değerler içerir.

### <a name="slider-basics"></a>Kaydırıcı temelleri

[ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) En soldaki değer aralığının en sağdaki gösteren çubuk yatay olan. Bu üç ortak özelliklerini tanımlar:

- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) tür `double`, varsayılan değer 0
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) tür `double`, varsayılan değer 0
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) tür `double`, varsayılan değer olan 1

Bu özellikleri yeniden bağlanabilir özellikler, tutarlı olduğundan emin olun:

- Üç tüm özellikler için [ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) bağlanılabilir özellik sağlar için belirtilen yöntemi `Value` arasında `Minimum` ve `Maximum`.
- [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/) Metodunda `MinimumProperty` döndürür `false` varsa `Minimum` değerinden büyük veya ona eşit bir değere ayarlayın `Maximum`ve benzer `MaximumProperty`. Döndüren `false` gelen `validateValue` yöntemi neden bir `ArgumentException` oluşturulur.

`Slider` ateşlenir [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) olay ile bir [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) bağımsız değişken olduğunda `Value` özellik değişiklikleri programlama yoluyla veya ne zaman kullanıcı yöneten `Slider`.

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) örnek basit kullanımını gösterir `Slider`.

### <a name="common-pitfalls"></a>Yaygın görülen tehlikeleri

Hem kod ve XAML, `Minimum` ve `Maximum` özellikleri, belirttiğiniz sıraya göre ayarlanır. Bu özellikler başlatmak mutlaka böylece `Maximum` her zaman büyük olup `Minimum`. Aksi takdirde bir özel durum oluşturulur.

Başlatma `Slider` özellikleri neden olabilecek `Value` değiştirmek için özellik ve `ValueChanged` olay başlatılmasına izin. Emin olmanız `Slider` olay işleyicisi, sayfa başlatma sırasında henüz oluşturmadıysanız görünümleri erişim değil.

`ValueChanged` Değil olayını ateşle sırasında `Slider` başlatma sürece `Value` özellik değişiklikleri. Çağırabilirsiniz `ValueChanged` doğrudan koddan işleyici.

### <a name="slider-color-selection"></a>Kaydırıcı renk seçimi

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) programını içeren üç `Slider` RGB değerleri belirterek bir renk etkileşimli olarak seçmenize olanak tanıyan öğeleri:

[![Üç ekran görüntüsü R G B kaydırıcıları](images/ch15fg03-small.png "RGB kaydırıcıları")](images/ch15fg03-large.png#lightbox "RGB kaydırıcılar")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) örnek kullanan iki `Slider` iki taşımak için öğeleri `Label` öğeler arasında bir `AbsoluteLayout` ve diğer birine Soldurma.

### <a name="the-stepper-difference"></a>Adımlayıcıdaki farkı

[ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Aynı özellikleri ve olayları olarak tanımlar `Slider` ancak `Maximum` özelliği, 100'e başlatılır ve `Stepper` dördüncü bir özelliğini tanımlar:

- [`Increment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) tür `double`, 1 başlatıldı

Görsel `Stepper` etiketli iki düğmeden oluşur **&ndash;** ve **+**. Tuşuna basarak **&ndash;** azaltır `Value` tarafından `Increment` en az `Minimum`. Tuşuna basarak **+** artırır `Value` tarafından `Increment` maksimum `Maximum`.

Bu tarafından gösterilmiştir [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) örnek.

## <a name="switch-and-checkbox"></a>Anahtar ve onay kutusu

[ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) Bir Boole değeri belirtmesini sağlar.

### <a name="switch-basics"></a>Anahtar temelleri

Görsel `Switch` açılıp kapatılabilir bir geçiş oluşur. Sınıfı, bir özellik tanımlar:

- [`IsToggled`](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) türü `bool`

`Switch` bir olayı tanımlar:

- [`Toggled`](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) eşlik bir [ `ToggledEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToggledEventArgs/) olduğunda harekete geçirilen nesne `IsToggled` özellik değişiklikleri.

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) programı gösterir `Switch`.

### <a name="a-traditional-checkbox"></a>Geleneksel bir onay kutusu

Bazı geliştiriciler daha geleneksel tercih `CheckBox` için `Switch`. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı içeren bir `CheckBox` türetilen sınıf `ContentView`. `CheckBox` tarafından uygulanan [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) ve [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) dosyaları. `CheckBox` üç özelliklerini tanımlar (`Text`, `FontSize`, ve `IsChecked`) ve bir `CheckedChanged` olay.

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) örnek bunu gösterir `CheckBox`.

## <a name="typing-text"></a>Metin yazma

Xamarin.Forms ve metin düzenlemesi kullanıcının sağlayan üç görünüm tanımlar:

- [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) tek satırlık bir metin
- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) için birden çok metin satırı
- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) tek satırlık metin arama amacıyla için.

`Entry` ve `Editor` öğesinden türetilen [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/), öğesinden türetildiğini `View`. `SearchBar` doğrudan türetilen `View`.

### <a name="keyboard-and-focus"></a>Klavye ve odağı

Telefon ve tabletlerden fiziksel klavyeler olmadan `Entry`, `Editor`, ve `SearchBar` öğelerin tümü, kullanıcın dosya hiyerarşisinden bir sanal klavye neden. Bu klavye ekranında varlığını giriş odağı ilgilidir. Bir görünüm sahip olmalıdır, [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) ve [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) özelliklerini ayarlamak `true` giriş odağını almak için.

İki yöntem, bir salt okunur özelliği ve iki olay giriş odağını ile ilgilidir. Bu tüm tanımlanır `VisualElement`:

- [ `Focus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Focus()/) Yöntem giriş odağını bir öğe için ayarlanacak ve döndürür `true` başarılıysa
- [ `Unfocus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Unfocus()/) Yöntemi, bir öğeyi Girintiyi kaldırır
- [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) Salt okunur özelliği, öğe odak giriş varsa gösterir
- [ `Focused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Focused/) Olay gösterir, giriş odağını bir öğeyi alır
- [ `Unfocused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Unfocused/) Olayı, ne zaman bir öğe giriş odağını kaybediyor gösterir

### <a name="choosing-the-keyboard"></a>Klavye seçme

[ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) Sınıfı `Entry` ve `Editor` türetilen yalnızca bir özelliğini tanımlar:

- [`Keyboard`](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) türü [`Keyboard`](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)

Bu, görüntülenen klavye türünü belirtir. Bazı klavyeler, URI veya sayılar için iyileştirilmiştir.

`Keyboard` Sınıfı sağlayan statik klavyeyle tanımlama [ `Keyboard.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Keyboard.Create/p/Xamarin.Forms.KeyboardFlags/) türünde bir bağımsız değişken yöntemiyle [ `KeyboardFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardFlags/), aşağıdaki bit bayrakları olan bir sabit listesi:

- `None` 0 olarak ayarlayın
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) 1 olarak ayarlayın
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) 2 olarak ayarlayın
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) 4'e ayarlayın
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) \xFFFFFFFF için ayarlayın

Çok satırlı kullanırken [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) bir paragraf ya da daha fazla metin beklendiğinde çağırma `Keyboard.Create` bir klavye seçimi için iyi bir yaklaşımdır. Tek satırlı için [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), aşağıdaki statik salt okunur özelliklerini `Keyboard` yararlıdır:

- [`Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Default/)
- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Text/)
- [`Chat`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Chat/)
- [`Url`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Url/)
- [`Email`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Email/)
- [`Telephone`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Telephone/)
- [`Numeric`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Numeric/) içeren veya içermeyen bir ondalık noktası pozitif sayılar için.

[ `KeyboardTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardTypeConverter/) Tarafından gösterildiği gibi XAML içinde bu özellikleri belirtmeye izin verir [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) program.

### <a name="entry-properties-and-events"></a>Giriş özellikleri ve olayları

Tek satırlı [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) aşağıdaki özellikleri tanımlar:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) tür `string`, görünen metni `Entry`
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) türü `Color`
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontFamily/) türü `string`
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontSize/) türü `double`
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontAttributes/) türü `FontAttributes`
- [`IsPassword`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.IsPassword/) tür `bool`, neden olan karakter maskelenmiş olamaz
- [`Placeholder`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) tür `string`, görünen dimly renkli metinler için `Entry` herhangi bir şey yazılmadan önce
- [`PlaceholderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.PlaceholderColor/) türü `Color`

`Entry` Ayrıca iki olay tanımlar:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) ile bir [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) olduğunda harekete geçirilen nesne `Text` özellik değişiklikleri
- [`Completed`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/), kullanıcının tamamlandıktan ve klavye kapatılır harekete geçirilir. Kullanıcı tamamlama bir platforma özgü şekilde gösterir.

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) örnek, bu iki olayları gösterir.

### <a name="the-editor-difference"></a>Düzenleyici farkı

Çok satırlı [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) aynı tanımlar `Text` ve `Font` özellikleri olarak `Entry` ancak diğer özellikleri. `Editor` Ayrıca aynı iki özellikleri tanımlar `Entry`.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) kaydeder ve içeriğini geri yükleyen bir serbest biçimli notlar alma programı `Editor`.

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Türünden türemez `InputView`, sahip olmadığı bir `Keyboard` özelliği. Ancak, tüm sahip `Text`, `Font`, ve `Placeholder` özellikleri, `Entry` tanımlar. Ayrıca, `SearchBar` üç ek özelliklerini tanımlar:

- [`CancelButtonColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.CancelButtonColor/) türü `Color`
- [`SearchCommand`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) tür [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) veri bağlamaları ve MVVM ile kullanmak için
- [`SearchCommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) tür `Object`, ile kullanmak için `SearchCommand`

Metin platforma özgü iptal düğmesi siler. `SearchBar` Ayrıca bir platforma özgü arama düğmesi bulunur. Bu düğmeler birini tuşuna basarak, başlatır iki olaylardan biri, `SearchBar` tanımlar:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.TextChanged/) eşlik bir [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) nesnesi
- [`SearchButtonPressed`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) örnek gösterir `SearchBar`.

## <a name="date-and-time-selection"></a>Tarih ve saat seçimi

[ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) Ve [ `TimePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) görünümleri kullanıcının bir tarih veya saat belirlemesine olanak tanıyan platforma özgü denetimleri uygulayın.

### <a name="the-datepicker"></a>Tarih Seçici

[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) dört özelliklerini tanımlar:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) tür `DateTime`, 1 Ocak 1900 için başlatılamadı
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) tür `DateTime`, 31 Aralık 2100 için başlatılamadı
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) tür `DateTime`, için başlatılamadı `DateTime.Today`
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) tür `string`, bir tarih görüntüleme "7/20/1969" ABD gibi bunun sonucunda "d", kısa tarih deseni başlatılmadı dizesi biçimlendirme .NET.

Ayarlayabileceğiniz `DateTime` özelliği öğeleri olarak özelliklerini belirtme ve kültür sabit kısa tarih kullanarak XAML özelliklerinde biçimlendirmek ("7/20/1969").   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) örnek kullanıcı tarafından seçilen iki tarih arasındaki gün sayısını hesaplar.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (veya bir TimeSpanPicker?)

[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) iki özellik ve olay tanımlar:

- [`Time`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) tür `TimeSpan` yerine `DateTime`, gece yarısından beri geçen süreyi gösteren
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Format/) tür `string`, .NET ABD'de biçimlendirme dizesi "t", "1:45 PM" gibi bir saati görüntüleme výsledek kısa bir süre deseni olarak başlatılır.

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) program nasıl kullanılacağını gösteren `TimePicker` Zamanlayıcı için bir zaman belirtmek için. Program, yalnızca ön planda tutarsanız çalışır.

**SetTimer** de kullanmayı gösterir [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) yöntemi `Page` bir uyarı kutusu görüntülenecek.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 15 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Bölüm 15 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Girdi](~/xamarin-forms/user-interface/text/entry.md)
- [Düzenleyici](~/xamarin-forms/user-interface/text/editor.md)
