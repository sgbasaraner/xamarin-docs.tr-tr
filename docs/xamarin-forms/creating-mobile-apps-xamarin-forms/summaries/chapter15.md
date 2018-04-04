---
title: Bölüm 15 özeti. Etkileşimli arabirimi
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c5b2bc00c4337969322193966f26ce0e151f426e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Bölüm 15 özeti. Etkileşimli arabirimi

Bu bölümde sekiz araştırır `View` kullanıcıyla etkileşime izin türevleri.

## <a name="view-overview"></a>Görünüm genel bakış

Xamarin.Forms öğesinden türetilen 20 instantiable sınıfları içerir `View` ama `Layout`. Altı bu önceki bölümlerde ele alınmış:

- `Label`: [ **Bölüm 2. Bir uygulama anatomisi**](chapter02.md)
- `BoxView`: [ **Bölüm 3. Yığın kaydırma**](chapter03.md)
- `Button`: [ **Bölüm 6. Düğme tıklama**](chapter06.md)
- `Image`: [ **Bölüm 13. Bitmaps**](chapter13.md)
- `ActivityIndicator`: [ **Bölüm 13. Bitmaps**](chapter13.md)
- `ProgressBar`: [ **Bölüm 14. AbsoluteLayout**](chapter14.md)

Bu bölümde sekiz görünümleri etkin temel .NET veri türleri ile etkileşim kurmak kullanıcı izin ver:

|Veri türü|Görünümler|
|--- |--- |
|`Double`|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/), [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|
|`Boolean`|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|
|`String`|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/), [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|
|`DateTime`|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/), [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|

Bu görünümler temel alınan veri türleri etkileşimli görsel gösterimi düşünebilirsiniz. Bu kavram daha sonraki bölümde, keşfedilen [ **Bölüm 16. Veri bağlama**](chapter16.md).

Kalan altı görünümleri aşağıdaki bölümlerde ele alınmıştır:

- `WebView`: [ **Bölüm 16. Veri bağlama**](chapter16.md)
- `Picker`: [ **Bölüm 19. Koleksiyon görünümleri**](chapter19.md)
- `ListView`: [ **Bölüm 19. Koleksiyon görünümleri**](chapter19.md)
- `TableView`: [ **Bölüm 19. Koleksiyon görünümleri**](chapter19.md)
- `Map`: [ **Bölüm 28. Konum ve eşlemeleri**](chapter28.md)
- `OpenGLView`: Bu kitap (ve Windows platformları için destek yok) kapsamında değil

## <a name="slider-and-stepper"></a>Kaydırıcı ve Adımlayıcı

Her ikisi de [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) ve [ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) sayısal bir değer arasında bir aralık seçin izin verin. `Slider` Sırasında sürekli bir aralığı `Stepper` ayrık değerler içerir.

### <a name="slider-basics"></a>Kaydırıcı temelleri

[ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) Değerleri aralığı en soldaki en sağdaki temsil eden çubuk yatay değil. Üç ortak özelliklerini tanımlar:

- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) tür `double`, varsayılan değer 0
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) tür `double`, varsayılan değer 0
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) tür `double`, varsayılan değer 1

Bu özellikleri geri bağlanabilir özellikleri tutarlı olduğundan emin olun:

- Tüm üç özelliklerinin [ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) sayılmasını bağlanabilirse özelliği için belirtilen yöntemi `Value` arasında `Minimum` ve `Maximum`.
- [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/) Yöntemi `MinimumProperty` döndürür `false` varsa `Minimum` eşit veya daha büyük bir değere ayarlayın `Maximum`ve benzer `MaximumProperty`. Döndürme `false` gelen `validateValue` yöntemi neden bir `ArgumentException` oluşturulması için.

`Slider` ateşlenir [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) olay ile bir [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) bağımsız değişkeni olduğunda `Value` özellik değişikliklerini program aracılığıyla veya ne zaman kullanıcı yöneten `Slider`.

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) basit kullanımını gösteren örnek `Slider`.

### <a name="common-pitfalls"></a>Ortak Tuzaklar

Hem kod hem de XAML'de `Minimum` ve `Maximum` özellikleri belirttiğiniz sırayla ayarlayın. Bu özellikleri başlatmak mutlaka böylece `Maximum` her zaman değerinden daha büyük `Minimum`. Aksi takdirde bir özel durum oluşturulur.

Başlatma `Slider` özellikleri neden olabilecek `Value` değiştirmek için özellik ve `ValueChanged` olay harekete. Emin olmanız `Slider` olay işleyicisi sayfa başlatma sırasında henüz oluşturmadınız görünümleri erişim değil.

`ValueChanged` Değil olayını ateşle sırasında `Slider` başlatma sürece `Value` özellik değişikliği. Çağırabilirsiniz `ValueChanged` doğrudan koddan işleyici.

### <a name="slider-color-selection"></a>Kaydırıcı renk seçimi

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) programı içeren üç `Slider` RGB değerleri belirterek bir renk etkileşimli olarak seçmenize olanak öğeleri:

[![Üçlü ekran görüntüsü R G B kaydırıcılar](images/ch15fg03-small.png "RGB kaydırıcılar")](images/ch15fg03-large.png#lightbox "RGB kaydırıcılar")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) örnek kullanan iki `Slider` iki taşımak için öğeleri `Label` öğeler arasında bir `AbsoluteLayout` ve diğer birine silinerek.

### <a name="the-stepper-difference"></a>Adımlayıcı fark

[ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Aynı özellikleri ve olayları olarak tanımlar `Slider` ancak `Maximum` özelliği, 100 başlatılır ve `Stepper` dördüncü bir özelliğini tanımlar:

- [`Increment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) tür `double`, 1 başlatıldı

Görsel `Stepper` etiketli iki düğmeden oluşur **&ndash;** ve **+**. Tuşuna basarak **&ndash;** azaltır `Value` tarafından `Increment` minimum olarak `Minimum`. Tuşuna basarak **+** artırır `Value` tarafından `Increment` maksimum `Maximum`.

Bu tarafından gösterilen [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) örnek.

## <a name="switch-and-checkbox"></a>Anahtar ve onay kutusu

[ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) Bir Boole değeri belirtmesini sağlar.

### <a name="switch-basics"></a>Anahtar temelleri

Görsel `Switch` kapatıp açık bir geçiş oluşur. Sınıfı, bir özelliğini tanımlar:

- [`IsToggled`](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) türü `bool`

`Switch` bir olay tanımlar:

- [`Toggled`](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) eşlik bir [ `ToggledEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToggledEventArgs/) nesne, zaman harekete `IsToggled` özellik değişikliği.

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) program gösteren `Switch`.

### <a name="a-traditional-checkbox"></a>Geleneksel bir onay kutusu

Bazı geliştiriciler daha geleneksel tercih `CheckBox` için `Switch`. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığını içeren bir `CheckBox` öğesinden türetilen sınıf `ContentView`. `CheckBox` tarafından uygulanan [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) ve [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) dosyaları. `CheckBox` üç özelliklerini tanımlar (`Text`, `FontSize`, ve `IsChecked`) ve bir `CheckedChanged` olay.

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) örnek gösterilmektedir bu `CheckBox`.

## <a name="typing-text"></a>Metin yazma

Xamarin.Forms girin ve metin düzenleme kullanıcı sağlayan üç görünüm tanımlar:

- [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) tek satırlık bir metin
- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) birkaç satırlık metin
- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) metin arama amacıyla tek bir satır için.

`Entry` ve `Editor` öğesinden türetilen [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/), den türetilen `View`. `SearchBar` doğrudan türetilen `View`.

### <a name="keyboard-and-focus"></a>Klavye ve odak

Telefon ve tabletlerden fiziksel klavyeler olmadan `Entry`, `Editor`, ve `SearchBar` öğelerin tümü neden açılır sanal bir klavye. Bu klavye ekranında varlığını odak giriş ilgilidir. Bir görünüm sahip olmalıdır, [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) ve [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) özelliklerini ayarlamak `true` giriş odağını alınamıyor.

İki yöntem, bir salt okunur özellik ve iki olay giriş odağını ile ilgilidir. Bunlar tüm tarafından tanımlanan `VisualElement`:

- [ `Focus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Focus()/) Yöntemi giriş odağını bir öğe olarak ayarlamaya çalışır ve döndürür `true` başarılı olursa
- [ `Unfocus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Unfocus()/) Yöntemi giriş odağını bir öğeyi kaldırır
- [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) Salt okunur özelliği, öğe odak giriş varsa gösterir
- [ `Focused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Focused/) Olay gösterir zaman giriş odağını bir öğeyi alır
- [ `Unfocused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Unfocused/) Olayı, ne zaman bir öğe giriş odağı kaybettiğinde gösterir

### <a name="choosing-the-keyboard"></a>Klavye seçme

[ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) Sınıfı `Entry` ve `Editor` türetilen yalnızca bir özelliğini tanımlar:

- [`Keyboard`](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) türü [`Keyboard`](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)

Bu, görüntülenen klavye türünü belirtir. Bazı klavyeler URI'ler veya sayıları getirilmiştir.

`Keyboard` Sınıfı verir statik klavyeyle tanımlama [ `Keyboard.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Keyboard.Create/p/Xamarin.Forms.KeyboardFlags/) türünde bir bağımsız değişken yöntemiyle [ `KeyboardFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardFlags/), aşağıdaki bit bayrakları olan bir numaralandırma:

- `None` 0 olarak ayarlayın
- [`CapitalizeSentence`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.CapitalizeSentence/) 1 olarak ayarlayın
- [`Spellcheck`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Spellcheck/) 2 olarak ayarlanmış
- [`Suggestions`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Suggestions/) 4'e ayarlayın
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.All/) \xFFFFFFFF için ayarlama

Çok satırlı kullanırken [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) bir paragraf ya da daha fazla metin beklenirken çağırma `Keyboard.Create` klavye seçmek için iyi bir yaklaşımdır. Tek satırlı için [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), aşağıdaki statik salt okunur özelliklerini `Keyboard` yararlıdır:

- [`Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Default/)
- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Text/)
- [`Chat`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Chat/)
- [`Url`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Url/)
- [`Email`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Email/)
- [`Telephone`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Telephone/)
- [`Numeric`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Numeric/) ile veya olmadan ondalık pozitif sayılar için.

[ `KeyboardTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardTypeConverter/) Tarafından gösterildiği gibi bu özellikleri XAML'de belirleme sağlar [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) program.

### <a name="entry-properties-and-events"></a>Giriş özellikleri ve olayları

Tek satırlı [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) aşağıdaki özellikleri tanımlar:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) tür `string`, görüntülenen metin `Entry`
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) türü `Color`
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontFamily/) türü `string`
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontSize/) türü `double`
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontAttributes/) türü `FontAttributes`
- [`IsPassword`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.IsPassword/) tür `bool`, maskelenecek karakterler neden olur
- [`Placeholder`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) tür `string`, görünür dimly renkli metin için `Entry` herhangi bir şey yazılmadan önce
- [`PlaceholderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.PlaceholderColor/) türü `Color`

`Entry` Ayrıca iki olayları tanımlar:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) ile bir [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) nesnesi, her harekete `Text` özellik değişiklikleri
- [`Completed`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/), kullanıcı tamamlandı ve klavye kapatılır. Kullanıcı tamamlama bir platforma özgü şekilde gösterir.

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) örneği, bu iki olayları gösterir.

### <a name="the-editor-difference"></a>Düzenleyici fark

Çok satırlı [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) aynı tanımlar `Text` ve `Font` özellikleri olarak `Entry` ancak diğer özellikleri. `Editor` Ayrıca aynı iki özellikleri tanımlar `Entry`.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) kaydeder ve içeriğini geri yükleyen bir serbest biçimli notları alma program `Editor`.

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Türünden türemez `InputView`, sahip olmayan bir `Keyboard` özelliği. Ancak, tüm sahip `Text`, `Font`, ve `Placeholder` özellikleri, `Entry` tanımlar. Ayrıca, `SearchBar` üç ek özelliklerini tanımlar:

- [`CancelButtonColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.CancelButtonColor/) türü `Color`
- [`SearchCommand`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) tür [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) veri bağlamaları ve MVVM ile kullanmak için
- [`SearchCommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) tür `Object`, ile kullanmak için `SearchCommand`

Metin platforma özgü iptal düğmesi siler. `SearchBar` Ayrıca bir platforma özgü arama düğmesi bulunur. Bu düğme birini tuşuna basarak, başlatır iki olaylardan biri, `SearchBar` tanımlar:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.TextChanged/) eşlik bir [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) nesnesi
- [`SearchButtonPressed`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) örnek gösterilmektedir `SearchBar`.

## <a name="date-and-time-selection"></a>Tarih ve saat seçimi

[ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) Ve [ `TimePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) görünümleri bir tarih veya saat belirtmek kullanıcının tanıyan platforma özgü denetimleri uygulayın.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) dört özelliklerini tanımlar:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) tür `DateTime`, 1 Ocak 1900 başlatıldı
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) tür `DateTime`, 31 Aralık 2100 başlatıldı
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) tür `DateTime`, için başlatıldı `DateTime.Today`
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) tür `string`, tarih görüntüleme "7/20/1969" ABD gibi sonuçta biçimlendirme dizesi "d", kısa tarih düzeni başlatılmış .NET.

Ayarlayabileceğiniz `DateTime` özellikleri özelliği öğeleri olarak ifade etmek ve değişmeyen kültür kısa tarih kullanarak XAML'de özellikleri biçimlendirin ("7/20/1969").   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) örnek kullanıcı tarafından seçilen iki tarih arasındaki gün sayısını hesaplar.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (veya bir TimeSpanPicker olan?)

[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) iki özellikleri ve hiçbir olayları tanımlar:

- [`Time`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) tür `TimeSpan` yerine `DateTime`, gece yarısından beri geçen süreyi belirten
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Format/) tür `string`, ABD'de biçimlendirme dizesi "t", "1:45 PM" gibi bir saat görüntüsündeki kaynaklanan kısa bir süre düzeni başlatılmış .NET.

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) program nasıl kullanılacağını gösteren `TimePicker` bir süre için bir zamanlayıcı belirtmek için. Program, yalnızca ön planda tutarsanız çalışır.

**SetTimer** kullanarak ayrıca gösterir [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) yöntemi `Page` bir uyarı kutusu görüntülemek için.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 15 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Bölüm 15 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Girdi](~/xamarin-forms/user-interface/text/entry.md)
- [Düzenleyici](~/xamarin-forms/user-interface/text/editor.md)
