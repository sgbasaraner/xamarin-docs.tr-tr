---
title: Bölüm 16 özeti. Veri bağlama
description: 'Xamarin.Forms ile mobil uygulamaları oluşturma: Bölüm 16 özeti. Veri bağlama'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 520da1518c7b795bd1ad17cc3cfaa8d37815de53
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241516"
---
# <a name="summary-of-chapter-16-data-binding"></a>Bölüm 16 özeti. Veri bağlama

Programcıları genellikle kendilerini bir nesnenin bir özelliğini değiştiğinde olmadığını algılayan olay işleyicileri yazma bulun ve, başka bir nesnenin bir özelliğin değerini değiştirmek için kullanın. Bu işlem teknik ile otomatik olarak *veri bağlama*. Veri bağlama genellikle XAML içinde tanımlanan ve kullanıcı arabirimi tanımının bir parçası haline gelir.

Sıklıkla, bu veri bağlamaları kullanıcı arabirimi nesneleri arka plandaki verilere bağlanın. Bu daha olarak keşfedilen bir tekniği olan [ **Bölüm 18. MVVM**](chapter18.md). Ancak, veri bağlamaları iki veya daha fazla kullanıcı arabirimi öğeleri da bağlanabilirsiniz. Bu bölümde veri bağlama erken örnekleri çoğu bu teknik gösterilmiştir.

## <a name="binding-basics"></a>Temel bağlama

Veri bağlama, çeşitli özellikleri, yöntemleri ve sınıfları oynayan:

- [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) Sınıfı türer [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) ve veri bağlamanın birçok özelliği kapsar
- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Özelliği tarafından tanımlanan [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) sınıfı
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Yöntemi tarafından tanımlanan de [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) sınıfı
- [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) Sınıfı tanımlayan üç ek `SetBinding` yöntemleri

Aşağıdaki iki sınıfları için bağlamaları XAML işaretleme uzantılarına destekler:

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) destekleyen `Binding` biçimlendirme uzantısı
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) destekleyen `x:Reference` biçimlendirme uzantısı

Veri bağlama iki arabirim oynayan:

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) içinde `System.ComponentModel` ad alanı olan bir özellik değiştiğinde bildirim uygulamak için
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) değerleri veri bağlamaları diğerine bir türden diğerine dönüştürün küçük sınıflarını tanımlamak için kullanılır

Veri bağlama aynı nesne ya da (daha sık) iki farklı nesneleri iki özelliklerini bağlanır. Bu iki özellik denir *kaynak* ve *hedef*. Genellikle, source özelliği değişikliği bir değişiklik hedef özelliğinde oluşmasına neden olur, ancak bazen yönü ters çevrilir. Ne olursa olsun:

- *hedef* özelliği yedeklenen, tarafından bir [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- *kaynak* özelliği, genellikle uygulayan bir sınıf üyesi olduğu [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

Uygulayan bir sınıf `INotifyPropertyChanged` ateşlenir bir [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/) bir özellik değeri değiştiğinde olay. `BindableObject` uygulayan `INotifyPropertyChanged` ve otomatik olarak başlatılır bir `PropertyChanged` olay bir özellik tarafından yedeklenen bir `BindableProperty` değişiklikleri değerleri, ancak yazabilirler kendi sınıflarınızı uygulayan `INotifyPropertyChanged` türetme olmadan `BindableObject`.

## <a name="code-and-xaml"></a>Kod ve XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) örnek kodda bir veri bağlaması yapma gösterir:

- Kaynak `Value` özelliğinin bir `Slider`
- Hedef `Opacity` özelliğinin bir `Label`

İki nesne ayarlayarak bağlı `BindingContext` , `Label` nesnesini `Slider` nesne. İki özellik çağırarak bağlı bir [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) genişletme yöntemi `Label` başvuran `OpacityProperty` bağlanabilirse özelliği ve `Value` özelliği `Slider` olarak ifade edilen bir dize.

Düzenleme `Slider` sonra neden `Label` Görünüm ve bu moddan silinerek için.

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) XAML'de ayarlamak veri bağlama ile aynı programdır. `BindingContext` , `Label` Ayarlanmış bir `x:Reference` biçimlendirme uzantısı başvuran `Slider`ve `Opacity` özelliği `Label` ayarlanır `Binding` biçimlendirme uzantısı ile kendi [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) özelliğe başvurma `Value` özelliği `Slider`.

## <a name="source-and-bindingcontext"></a>Kaynak ve Bindingparameters'te

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) örnek kodda alternatif bir yaklaşım gösterir. A `Binding` nesne ayarlanarak oluşturulur [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) özelliğine `Slider` nesne ve [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) "Value" özelliğine. [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Yöntemi `BindableObject` üzerinde daha sonra çağrılır `Label` nesnesi.

[ `Binding` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) ayrıca tanımlamak için kullanılmış `Binding` nesnesi.

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) örnek XAML'de karşılaştırılabilir tekniği gösterir. `Opacity` Özelliği `Label` ayarlanmış bir `Binding` biçimlendirme uzantısı ile [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) kümesine `Value` özelliği ve [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) kümesine bir katıştırılmış `x:Reference` biçimlendirme uzantısı.

Özet olarak, bağlama kaynak nesneye başvurmak için iki yolu vardır:

- Aracılığıyla `BindingContext` hedef özelliği
- Aracılığıyla `Source` özelliği `Binding` nesnenin kendisini

Her ikisi de belirtilirse, ikinci önceliklidir. Avantajı `BindingContext` aracılığıyla görsel ağacın yayılır değil. Bu *çok* kullanışlı aynı kaynak nesneyle ilişkili birden çok hedef özellikleri.

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) program gösteren Bu teknik ile [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) öğesi. İki `Button` İleri ve geri girmek için gezinme öğeleri devralan bir `BindingContext` başvuruyor kendi üst öğesinden `WebView`. `IsEnabled` İki düğme özelliklerini sonra sahip basit `Binding` düğmesi hedef biçimlendirme uzantıları `IsEnabled` özelliklerine bağlı ayarlarını üzerinde [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/) ve [ `CanGoForward` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/) salt okunur özelliklerini `WebView`.

## <a name="the-binding-mode"></a>Bağlama modu

Ayarlama [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) özelliği `Binding` üyesi için [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) numaralandırma:

- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) Hedef kaynak özelliğindeki değişiklikleri etkilememesi
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) Böylece hedef özelliğindeki değişiklikler kaynak etkiler
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) Kaynak ve hedef değişiklikleri birbirine etkilememesi
- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) kullanılacak [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) belirtilen hedef `BindableProperty` oluşturuldu. Hiçbiri belirtilmemişse, varsayılan değer `OneWay` normal bağlanabilir özellikleri ve `OneWayToSource` salt okunur bağlanabilir özellikleri.

Veri bağlamaları MVVM senaryolarda hedeflerini genellikle büyük olasılıkla özelliklere sahip bir `DefaultBindingMode` , `TwoWay`. Bunlar:

- `Value` özelliği `Slider` ve `Stepper`
- `IsToggled` özelliği `Switch`
- `Text` özelliği `Entry`, `Editor`, ve `SearchBar`
- `Date` özelliği `DatePicker`
- `Time` özelliği `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) örnek gösteren dört bağlama modları olduğu veri bağlama ile `FontSize` özelliği bir `Label` ve kaynağı `Value` özelliği bir `Slider`. Her böylece `Slider` ilgili yazı tipi boyutunu denetlemek için `Label`. Ancak `Slider` öğeleri başlatılmadı çünkü `DefaultBindingMode` , `FontSize` özelliği `OneWay`.

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) örnek üzerinde bağlamaları ayarlar `Value` özelliği `Slider` başvuran `FontSize` her özellik `Label`. Bu geriye doğru görünüyor, ancak daha iyi initialzing içinde çalışır `Slider` öğeleri çünkü `Value` özelliği `Slider` sahip bir `DefaultBindingMode` , `TwoWay`.

[![Ters bağlama Üçlü ekran](images/ch16fg06-small.png "ters bağlama")](images/ch16fg06-large.png#lightbox "ters bağlama")

Bu bağlamaları nasıl MVVM içinde tanımlanan için benzerdir ve bu tür sık bağlama kullanacaksınız.

## <a name="string-formatting"></a>Biçimlendirme dizesi

Target özelliği türü olduğunda `string`, kullanabileceğiniz [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) özelliği tarafından tanımlanan `BindingBase` kaynağına dönüştürmek için bir `string`. Ayarlama `StringFormat` biçimlendirme statik ile kullanacağınız dizesi bir .NET özelliğine [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) nesneyi görüntülemek için biçimi. Süslü ayraçlar katıştırılmış biçimlendirme uzantısı için mistaken olmaz şekilde bu biçimlendirme dizesi biçimlendirme uzantısı içinde kullanırken, tek tırnak işaretiyle koyun.

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) örnek nasıl kullanılacağını gösteren `StringFormat` XAML'de.

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) örnek gösterilmektedir bağlamalar ile sayfa boyutunu görüntüleme `Width` ve `Height` özelliklerini `ContentPage`.

## <a name="why-is-it-called-path"></a>Neden "Path" adı verilir?

[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) Özelliği `Binding` özellikler ve dizin oluşturucular noktalarla ayrılmış bir dizi olabileceğinden şekilde çağrılır. [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) örnek çeşitli örnekler göstermektedir.

## <a name="binding-value-converters"></a>Değer dönüştürücüler bağlama

Bir bağlama kaynak ve hedef özellikleri farklı türleri olduğunda bağlama dönüştürücü ile türleri arasında dönüştürebilirsiniz. Uygulayan bir sınıf budur [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) arabirim ve iki yöntem içerir: [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) hedef, kaynak dönüştürmek ve [ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) Hedef kaynak dönüştürmek için.

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığıdır dönüştürmek için örnek bir `int` için bir `bool`. Tarafından gösterilen [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) yalnızca sağlayan örnek `Button` en az bir karakter içine yazdığınız durumda bir `Entry`.

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) Sınıf dönüştürür bir `bool` için bir `string` ve hangi metin için döndürülmelidir belirtmek için iki özelliklerini tanımlayan `false` ve `true` değerleri.
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) Benzer. [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) örnek gösterilmektedir göre farklı renkler farklı metinleri görüntülemek için bu iki dönüştürücüleri kullanarak bir `Switch` ayarı.

Genel [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) değiştirebilirsiniz `BoolToStringConverter` ve `BoolToColorConverter` genelleştirilmiş derler ve `bool`-için-nesne dönüştürücü herhangi bir türde.

## <a name="bindings-and-custom-views"></a>Bağlamalar ve özel görünümler

Özel denetimler veri bağlamaları kullanarak basitleştirebilirsiniz. [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) Kod dosyası tanımlar `Text`, `TextColor`, `FontSize`, `FontAttributes`, ve `IsChecked` özellikleri, ancak hiç denetiminin görsellerinin hiçbir mantığı vardır.
Bunun yerine [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) dosyası üzerinde veri bağlamaları denetimin görsellerinin tüm biçimlendirme içeren `Label` öğeleri temel alarak arka plan kodu dosyasında tanımlanan özellikler.

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) örnek gösterilmektedir `NewCheckBox` özel denetim.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 16 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Bölüm 16 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
