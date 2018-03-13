---
title: "Bölüm 17 özeti. Kılavuz katılımınıza"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 09f63dd418ea1fb523c028edb02c28c22bfdccd1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Bölüm 17 özeti. Kılavuz katılımınıza

[ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Alt satırlar ve sütunlar hücre halinde düzenler güçlü düzeni mekanizmadır. Benzer HTML aksine `table` öğesi, `Grid` yalnızca sunu yerine Düzen amaçlıdır.

## <a name="the-basic-grid"></a>Temel kılavuz

`Grid` türetilen [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), tanımlayan bir [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) özelliği, `Grid` devralır. Bu koleksiyonda XAML veya kod doldurabilirsiniz.

### <a name="the-grid-in-xaml"></a>XAML kılavuzunda

Tanımı bir `Grid` XAML'de genellikle doldurma ile başlar [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) ve [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) koleksiyonları `Grid` ile [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) ve [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) nesneleri. Satır sayısı ve sütunlarının kurmak nasıl budur `Grid`ve özellikleri.

`RowDefinition` sahip bir [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) özelliği ve `ColumnDefinition` sahip bir [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) özelliği, hem de türü [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), bir yapı.

XAML'de [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) dönüştürür basit metin dizeleri `GridLength` değerleri. Arka planda [ `GridLength` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) oluşturur `GridLength` değerine göre bir sayı ve türünde bir değer [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), üç üyesi olan bir numaralandırma:

- [`Absolute`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Absolute/) & #x 2014; CİHAZDAN bağımsız birimler (XAML numarasında) genişliği veya yüksekliği belirtilen
- [`Auto`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Auto/) & #x 2014; Genişlik ve yükseklik autosized hücre içeriğinin (XAML'de "Auto") dayalı olan
- [`Star`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) & #x 2014; Kalan yüksekliği veya genişliği ayrılır orantılı olarak (bir sayı ile "\*" adlı *yıldız*, XAML içinde)

Her alt `Grid` de satır ve sütun (açık veya örtülü olarak) atanması gerekir. Çok satıra yayılmış ve sütun yayılma isteğe bağlıdır. Bunlar tüm kullanarak bağlanabilir özellikleri & #x 2014 bağlı belirtilir; tarafından tanımlanan özellikler `Grid` alt ancak ayarlanmış `Grid`. `Grid` dört statik ekli bağlanabilir özelliklerini tanımlar:

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) & #x 2014; sıfır tabanlı satır; Varsayılan 0'dır
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) & #x 2014; sıfır tabanlı sütun; Varsayılan 0'dır
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) & #x 2014; sayı satırlarını alt yayılır; Varsayılan 1'dir
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) & #x 2014; sayı sütunlarını alt kapsayan; Varsayılan 1'dir

Kod içinde bir program ayarlamak ve bu değerleri almak için sekiz statik yöntemleri kullanın:

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) Ve [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) Ve [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) Ve [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) Ve [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

XAML'de bu değerleri ayarlamak için aşağıdaki öznitelikler kullanın:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) örnek oluşturma ve başlatma gösteren bir `Grid` XAML'de.

`Grid` Devralır [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) özelliğinden `Layout` ve satır ve sütunları arasındaki boşluğu sağlayan iki ek özellikler tanımlar:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) 6'ın varsayılan bir değeri yok
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) 6'ın varsayılan bir değeri yok

`RowDefinitions` Ve `ColumnDefinitions` koleksiyonları kesinlikle gerekli değildir. Eğer yoksa, `Grid` satırları ve sütunları için oluşturur `Grid` alt ve tümünü varsayılan verir `GridLength` , "\*" (yıldız).

### <a name="the-grid-in-code"></a>Kod kılavuzunda

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) örnek gösterilmektedir oluşturmak ve doldurmak nasıl bir `Grid` kod. Ekli özellikler her alt doğrudan veya dolaylı olarak ek çağırarak ayarlayabilirsiniz `Add` gibi yöntemler [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) tarafından tanımlanan [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) arabirim.

### <a name="the-grid-bar-chart"></a>Kılavuz çubuk grafiği

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) örnek göstermektedir birden çok ekleme `BoxView` öğelerine bir `Grid` toplu kullanarak [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) yöntemi. Varsayılan olarak, bunlar `BoxView` öğelere sahip eşit genişliği. Her birinin yüksekliği `BoxView` bir çubuk grafik benzeyecek şekilde denetlenebilir.

`Grid` İçinde **GridBarChart** örnek paylaşımları bir `AbsoluteLayout` başlangıçta görünmez ile üst `Frame`. Program ayrıca ayarlar bir `TapGestureRecognizer` her `BoxView` kullanmak için `Frame` tapped çubuğu hakkında bilgi görüntülemek için.

### <a name="alignment-in-the-grid"></a>Kılavuz içinde hizalama

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) örnek nasıl kullanılacağını gösteren `VerticalOptions` ve `HorizontalOptions` içinde alt hizalamak için özellikler bir `Grid` hücre.

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) örnek eşit alanları `Button` öğeleri ortalanmış `Grid` hücreleri.

### <a name="cell-dividers-and-borders"></a>Hücre Bölücü ve Kenarlıklar

`Grid` Hücre Bölücü veya kenarlık çizer bir özellik içermiyor. Ancak, kendi yapabilirsiniz.

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) ek satır ve sütun özellikle için ince nasıl tanımlanacağı gösterilmektedir `BoxView` bölme çizgileri taklit etmek üzere öğeleri.

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) program ek hücreler oluşturmaz, ancak bunun yerine hizalar `BoxView` hücre kenarlığı taklit etmek üzere her bir hücre öğeleri.

## <a name="almost-real-life-grid-examples"></a>Neredeyse gerçek hayatta kılavuz örnekleri

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) kullanan örnek bir `Grid` bir tuş görüntülemek için:

[![Tuş kılavuzunun Üçlü ekran](images/ch17fg12-small.png "tuş kılavuz")](images/ch17fg12-large.png#lightbox "tuş kılavuz")

### <a name="responding-to-orientation-changes"></a>Yönlendirme değişikliklere yanıt verme

`Grid` Yönlendirmesini değişikliklere için bir program yapısı yardımcı olabilir. [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) örneği, bir öğenin dikey odaklı Phone ikinci bir satır ve ikinci sütunun yatay yönelimli Phone arasında taşır bir teknik gösterir.

Program başlatır `Slider` 0-255 ve kullandığı veri bağlamaları onaltılık kaydırıcılar değerini görüntülemek için bir aralığı öğelerine. Çünkü `Slider` değerleri kayan nokta ve onaltılık yalnızca tamsayı ile çalıştığı için biçimlendirme dizesi .NET bir [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı yardımcı olduğunu.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 17 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Bölüm 17 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Kılavuz](~/xamarin-forms/user-interface/layouts/grid.md)
