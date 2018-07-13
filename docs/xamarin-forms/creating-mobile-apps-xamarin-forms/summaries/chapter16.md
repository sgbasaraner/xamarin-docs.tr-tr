---
title: Bölüm 16 özeti. Veri bağlama
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 16 özeti. Veri bağlama'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c4ad067778203759a54ed8141db0b82602e40f6c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997458"
---
# <a name="summary-of-chapter-16-data-binding"></a>Bölüm 16 özeti. Veri bağlama

Programcıların genellikle kendilerini bir nesnenin bir özellik değiştiğinde olan algılama olay işleyicileri yazarak bulun ve, başka bir nesneye bir özellik değerini değiştirmek için kullanın. Bu işlem teknik ile otomatik olarak *veri bağlama*. Veri bağlamaları, genellikle XAML içinde tanımlanır ve tanımı kullanıcı arabiriminin bir parçası haline gelir.

Çok sık, bu veri bağlamaları kullanıcı arabirimi nesneleri temel alınan verilere bağlanın. Bu, daha, araştırılan bir tekniktir [ **Bölüm 18. MVVM**](chapter18.md). Ancak, veri bağlamaları iki veya daha fazla kullanıcı arabirimi öğelerini da bağlanabilirsiniz. Bu bölümde veri bağlamanın erken örneklerin çoğu bu teknik gösterilmektedir.

## <a name="binding-basics"></a>Temel bağlama

Veri bağlamasında, çeşitli özellikleri, yöntemleri ve sınıfları ilgilidir:

- [ `Binding` ](xref:Xamarin.Forms.Binding) Sınıf türetilir [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) ve veri bağlama birçok özelliklerini kapsar
- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Özelliği tarafından tanımlanan [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) sınıfı
- [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Yöntemi tarafından tanımlanan ayrıca [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) sınıfı
- [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) Sınıfı tanımlayan üç ek `SetBinding` yöntemleri

Aşağıdaki iki sınıf bağlamaları için XAML biçimlendirme uzantıları destekler:

- [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) destekleyen `Binding` işaretleme uzantısı
- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) destekleyen `x:Reference` işaretleme uzantısı

Veri bağlama iki arabirim ilgilidir:

- [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) içinde `System.ComponentModel` ad alanı olan bir özellik değiştiğinde bildirim uygulamak için
- [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) değerleri veri bağlamaları alanındaki başka bir türden diğerine dönüştürme küçük sınıflarını tanımlamak için kullanılır

Veri bağlama iki özellik aynı nesneye veya (daha sık) iki farklı nesne bağlanır. Bu iki özellik olarak ifade edilir *kaynak* ve *hedef*. Genel olarak, kaynak özellik değişikliği hedef özelliği gerçekleşmesi bir değişiklik neden olur, ancak bazen yönünü ters çevrilir. Bakılmaksızın:

- *hedef* özelliği yedeklenen, tarafından bir [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)
- *kaynak* özelliği genellikle uygulayan bir sınıf üyesi. [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged)

Uygulayan bir sınıf `INotifyPropertyChanged` ateşlenir bir [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) bir özellik değeri değiştiğinde bir olay. `BindableObject` uygulayan `INotifyPropertyChanged` ve otomatik olarak tetiklenen bir `PropertyChanged` bir özellik tarafından yedeklendiğinde olay bir `BindableProperty` değişiklikleri değerler, ancak yazabilirsiniz, kendi sınıfları uygulayan `INotifyPropertyChanged` türetme olmadan `BindableObject`.

## <a name="code-and-xaml"></a>Kod ve XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) örnek kodda bir veri bağlamayı ayarlama işlemini gösterir:

- Kaynak `Value` özelliği bir `Slider`
- Hedef `Opacity` özelliği bir `Label`

İki nesnenin ayarlayarak bağlı `BindingContext` , `Label` nesnesini `Slider` nesnesi. Çağırarak iki özelliğe bağlı bir [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) genişletme yöntemini `Label` başvuran `OpacityProperty` bağlanılabilir özellik ve `Value` özelliği `Slider` olarak ifade edilen bir dize.

Düzenleme `Slider` ardından neden `Label` soluklaştırılacak içine ve dışına görünümü.

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) XAML içinde ayarlanmış veri bağlama ile aynı bir programdır. `BindingContext` , `Label` Ayarlanmış bir `x:Reference` işaretleme uzantısı başvuran `Slider`ve `Opacity` özelliği `Label` ayarlanır `Binding` işaretleme uzantısı ile kendi [ `Path` ](xref:Xamarin.Forms.Binding.Path) özelliğe başvurma `Value` özelliği `Slider`.

## <a name="source-and-bindingcontext"></a>Kaynak ve BindingContext

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) örnek kodda alternatif bir yaklaşım gösterilmektedir. A `Binding` ayarlayarak nesnesi oluşturulur [ `Source` ](xref:Xamarin.Forms.Binding.Source) özelliğini `Slider` nesne ve [ `Path` ](xref:Xamarin.Forms.Binding.Path) özelliğini "Value". [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Yöntemi `BindableObject` üzerinde çağrılır `Label` nesne.

[ `Binding` Oluşturucusu](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) ayrıca tanımlamak için kullanılmış `Binding` nesne.

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) örnek karşılaştırılabilir teknik XAML içinde gösterir. `Opacity` Özelliği `Label` ayarlanmış bir `Binding` işaretleme uzantısı ile [ `Path` ](xref:Xamarin.Forms.Binding.Path) kümesine `Value` özelliği ve [ `Source` ](xref:Xamarin.Forms.Binding.Source) kümesine bir katıştırılmış `x:Reference` işaretleme uzantısı.

Özet olarak, bağlama kaynak nesneye başvurmak için iki yolu vardır:

- Aracılığıyla `BindingContext` hedef özelliği
- Aracılığıyla `Source` özelliği `Binding` nesnenin kendisini

Her ikisi de belirtilirse, ikinci önceliklidir. Avantajı `BindingContext` olan görsel ağacı dağıtılır. Bu *çok* kullanışlı birden çok hedef özellikleri aynı kaynak nesnesine bağlıdır.

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) program bu teknikte gösterir [ `WebView` ](xref:Xamarin.Forms.WebView) öğesi. İki `Button` öğeleri gezinme için İleri ve devralınan bir `BindingContext` başvuran üst öğesinden `WebView`. `IsEnabled` İki düğmenin özelliklerini ardından sahip basit `Binding` düğme hedef biçimlendirme uzantıları `IsEnabled` özelliklerine bağlı ayarlarını [ `CanGoBack` ](xref:Xamarin.Forms.WebView.CanGoBack) ve [ `CanGoForward` ](xref:Xamarin.Forms.WebView.CanGoForward) salt okunur özelliklerini `WebView`.

## <a name="the-binding-mode"></a>Bağlama modu

Ayarlama [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) özelliği `Binding` üyesinin [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) sabit listesi:

- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) Hedef kaynak özellik değişiklikleri etkilememesi
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) hedef özelliği değişiklikleri kaynak etkilememesi
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) böylece kaynak ve hedef değişiklikleri birbirlerini etkileyen
- [`Default`](xref:Xamarin.Forms.BindingMode.Default) kullanılacak [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) belirtilen hedef `BindableProperty` oluşturuldu. Hiçbiri belirtilmemişse, varsayılan değer `OneWay` normal bağlanabilir özellikler için ve `OneWayToSource` salt okunur bağlanabilir özellikler için.

MVVM senaryolarda veri bağlamaları hedefleri genellikle olması olasılığı özelliklere sahip bir `DefaultBindingMode` , `TwoWay`. Bunlar:

- `Value` özelliği `Slider` ve `Stepper`
- `IsToggled` özelliği `Switch`
- `Text` özelliği `Entry`, `Editor`, ve `SearchBar`
- `Date` özelliği `DatePicker`
- `Time` özelliği `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) örnek, bir veri bağlama hedefi olduğu dört bağlama modlarını gösterir `FontSize` özelliği bir `Label` ve kaynağı `Value` özelliği bir `Slider`. Böylece her `Slider` karşılık gelen yazı tipi boyutunu denetlemek için `Label`. Ancak `Slider` öğeleri olduğundan başlatılmadığından `DefaultBindingMode` , `FontSize` özelliği `OneWay`.

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) örnek üzerinde bağlamaları ayarlar `Value` özelliği `Slider` başvuran `FontSize` her özellik `Label`. Bu geriye dönük olarak görünür, ancak daha iyi initialzing içinde çalıştığını `Slider` öğeleri olduğundan `Value` özelliği `Slider` sahip bir `DefaultBindingMode` , `TwoWay`.

[![Üçlü ters bağlama görüntüsü](images/ch16fg06-small.png "ters bağlama")](images/ch16fg06-large.png#lightbox "ters bağlama")

Bu bağlamaları içinde MVVM nasıl tanımlandığı için analojiktir ve bu tür sık bağlama kullanırsınız.

## <a name="string-formatting"></a>Dize biçimlendirmesi

Hedef özelliği türünde olduğunda `string`, kullanabileceğiniz [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat) özelliği tarafından tanımlanan `BindingBase` kaynağına dönüştürmek için bir `string`. Ayarlama `StringFormat` biçimlendirme dizesi ile statik kullanacağı bir .NET özelliğini [ `String.Format` ](xref:System.String.Format(System.String,System.Object)) nesne görüntülenecek biçimi. Küme ayraçları için bir katıştırılmış işaretleme uzantısı mistaken olmaz, dolayısıyla bu biçimlendirme dizesi içinde bir işaretleme uzantısı kullanırken, tek tırnak işaretleri ile çevreleyin.

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) örnek nasıl kullanılacağını gösterir `StringFormat` XAML içinde.

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) örnek gösterir bağlamaları ile sayfanın boyutunun görüntüleme `Width` ve `Height` özelliklerini `ContentPage`.

## <a name="why-is-it-called-path"></a>Neden "Path" adı verilir?

[ `Path` ](xref:Xamarin.Forms.Binding.Path) Özelliği `Binding` özellikler ve dizin oluşturucular noktalarla ayrılmış bir dizi olabileceğinden şekilde çağrılır. [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) örnek, çeşitli örnekleri gösterir.

## <a name="binding-value-converters"></a>Bağlama değeri dönüştürücüleri

Bir bağlamanın kaynak ve hedef özellikleri farklı olduğunda, bir bağlama dönüştürücü kullanarak türleri arasında dönüştürme yapabilirsiniz. Uygulayan bir sınıf budur [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) arabirim ve iki yöntem içerir: [ `Convert` ](xref:Xamarin.Forms.IValueConverter.Convert(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) hedef, kaynak dönüştürmek ve [ `ConvertBack` ](xref:Xamarin.Forms.IValueConverter.ConvertBack(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) Hedef kaynak dönüştürmek için.

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplıktır dönüştürmek için örnek bir `int` için bir `bool`. Tarafından gösterilen [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) yalnızca sağlayan örnek `Button` en az bir karakter ile yazılan, bir `Entry`.

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) Sınıfı dönüştürür bir `bool` için bir `string` ve tanımlar için metni döndürülmesi gerektiğini belirlemek için iki özellik `false` ve `true` değerleri.
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) Benzerdir. [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) örnek gösterir farklı metinleri farklı renkler göre görüntülemek için bu iki dönüştürücüler kullanarak bir `Switch` ayarı.

Genel [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) değiştirebilirsiniz `BoolToStringConverter` ve `BoolToColorConverter` genelleştirilmiş işlev görür `bool`-için-herhangi bir türde nesne Dönüştürücüsü.

## <a name="bindings-and-custom-views"></a>Bağlamaları ve özel görünümler

Özel denetimler kullanarak veri bağlamaları basitleştirebilir. [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) Kod dosyası tanımlar `Text`, `TextColor`, `FontSize`, `FontAttributes`, ve `IsChecked` özellikleri, ancak hiçbir mantıksal denetimin hiç görsellerinin sahiptir.
Bunun yerine [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) dosya üzerinde veri bağlamaları ile denetimin görseller için tüm biçimlendirme içeren `Label` arka plan kod dosyasında tanımlanan özelliklerini temel öğeleri.

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) örnek gösterir `NewCheckBox` özel denetim.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 16 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Bölüm 16 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
