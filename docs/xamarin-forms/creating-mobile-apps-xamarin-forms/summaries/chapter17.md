---
title: Bölüm 17 özeti. Kılavuzda Uzmanlaşma
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 17 özeti. Kılavuzda Uzmanlaşma'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b71859d0848d7bf790b3cc4beddc67a5ea86d340
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935484"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Bölüm 17 özeti. Kılavuzda Uzmanlaşma

[ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Alt satırlar ve sütunlar hücre halinde düzenler güçlü Düzen mekanizmadır. Benzer HTML aksine `table` öğesi `Grid` yalnızca sunu yerine Düzen amaçlıdır.

## <a name="the-basic-grid"></a>Temel kılavuz

`Grid` öğesinden türetilen [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), tanımlayan bir [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) özelliği, `Grid` devralır. Bu koleksiyonda XAML veya kod doldurabilirsiniz.

### <a name="the-grid-in-xaml"></a>XAML kılavuz

Tanımı bir `Grid` XAML içinde genellikle doldurma ile başlar [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) ve [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) koleksiyonları `Grid` ile [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) ve [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) nesneleri. Bu, satır sayısı ve sütunlarını kurmak nasıl `Grid`ve özellikleri.

`RowDefinition` sahip bir [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) özelliği ve `ColumnDefinition` sahip bir [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) özelliği, her iki tür [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), bir yapı.

XAML içinde [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) basit metin dizelerine dönüştürür `GridLength` değerleri. Planda, [ `GridLength` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) oluşturur `GridLength` değerine göre bir sayı ve bir değer türü [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), üç üyesi olan bir sabit listesi:

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; CİHAZDAN bağımsız birimler (XAML içinde bir sayı) genişliği veya yüksekliği belirtilen
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; Genişlik ve yükseklik autosized hücre içeriğini (XAML içinde "Auto") dayalı olan
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; Kalan enlerini veya boylarını ayrılır orantılı olarak (bir sayı ile "\*" adlı *yıldız*, XAML içinde)

Her alt `Grid` satır ve sütun (açıkça veya dolaylı olarak) atanmış olmalısınız. Yayılan satır ve sütun yayılma isteğe bağlıdır. Bu tüm ekli bağlanabilir özellikler kullanılarak belirtilir &mdash; tarafından tanımlanan özellikler `Grid` alt ayarlanmış ancak `Grid`. `Grid` dört adet statik ekli bağlanabilir özellikler tanımlar:

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) &mdash; sıfır tabanlı satır; Varsayılan 0'dır
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) &mdash; sıfır tabanlı sütunu; Varsayılan 0'dır
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) &mdash; sayı satırlarını alt yayılır; Varsayılan 1'dir
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) &mdash; sayı sütunlarının alt yayılır; Varsayılan 1'dir

Kod içinde bir program ayarlamak ve bu değerleri almak için sekiz statik yöntemleri kullanın:

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) ve [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) ve [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) ve [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) ve [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

XAML içinde şu öznitelikler, bu değerleri ayarlamak için kullanın:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) oluşturma ve başlatma örneği gösterir bir `Grid` XAML içinde.

`Grid` Devralan [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) özelliğinden `Layout` ve satırları ve sütunları arasındaki boşluğu sağlayan iki ek özelliklerini tanımlar:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) 6'ın varsayılan bir değeri yok
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) 6'ın varsayılan bir değeri yok

`RowDefinitions` Ve `ColumnDefinitions` koleksiyonları kati şekilde gerekli değildir. Çalıştırıyorsa, `Grid` satırları ve sütunları oluşturur `Grid` alt öğeleri ve tüm bunları varsayılan verir `GridLength` , "\*" (star).

### <a name="the-grid-in-code"></a>Kod kılavuz

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) örnek nasıl oluşturulacağını ve doldurulacağını gösterir bir `Grid` kod. Her alt doğrudan veya dolaylı olarak ek çağırarak ekli özelliklerini ayarlayabileceğiniz `Add` gibi yöntemler [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) tarafından tanımlanan [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) arabirim.

### <a name="the-grid-bar-chart"></a>Kılavuz çubuk grafik

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) örnek, birden çok ekleme gösterir `BoxView` öğelerine bir `Grid` toplu [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) yöntemi. Varsayılan olarak, bunlar `BoxView` öğelere sahip eşit genişlik. Her birinin yüksekliği `BoxView` çubuk grafik benzeyecek şekilde denetlenebilir.

`Grid` İçinde **GridBarChart** örnek paylaşımları bir `AbsoluteLayout` üst başlangıçta görünmez `Frame`. Program ayrıca ayarlar bir `TapGestureRecognizer` her `BoxView` kullanılacak `Frame` dokunulan çubuğu hakkında bilgi görüntülemek için.

### <a name="alignment-in-the-grid"></a>Kılavuzdaki hizalama

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) örnek nasıl kullanılacağını gösterir `VerticalOptions` ve `HorizontalOptions` çocuklarını hizalamak için özellikleri bir `Grid` hücre.

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) örnek eşit boşluk `Button` öğeleri ortalanmış olarak `Grid` hücreleri.

### <a name="cell-dividers-and-borders"></a>Hücre Bölücü ve Kenarlıklar

`Grid` Hücre Bölücü veya kenarlık çizen bir özellik içermiyor. Ancak, kendi uygulamanızı oluşturabilirsiniz.

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) ek satır ve sütun için özel olarak ince nasıl tanımlanacağını gösterir `BoxView` bölme çizgileri taklit etmek için öğeleri.

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) programı herhangi bir ek hücre oluşturmaz, ancak bunun yerine hizalar `BoxView` hücre kenarlığı taklit etmek için her bir hücresinde öğeleri.

## <a name="almost-real-life-grid-examples"></a>Neredeyse gerçek zamanlı konuşmaların Grid örnekleri

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) kullanan örnek bir `Grid` bir tuş görüntülemek için:

[![Üç ekran tuş kılavuzunun](images/ch17fg12-small.png "tuş kılavuz")](images/ch17fg12-large.png#lightbox "tuş kılavuz")

### <a name="responding-to-orientation-changes"></a>Yönlendirme değişikliklere yanıt verme

`Grid` Yönlendirme değişikliklerine yanıt verme bir program yapısı yardımcı olabilir. [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) örnek ikinci sütunda yatay odaklı bir telefonun dikey odaklı bir telefon ikinci satırı arasında bir öğe taşıyan bir yöntemi gösterir.

Program başlatır `Slider` öğeleri 0 ile 255 arasında ve kullandığı veri bağlamaları onaltılık kaydırıcıları değerini görüntülemek için bir dizi. Çünkü `Slider` değerleri kayan nokta ve onaltılık yalnızca tamsayı ile çalıştığı için biçimlendirme dizesi .NET bir [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı yardımcı olduğunu.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 17 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Bölüm 17 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Kılavuz](~/xamarin-forms/user-interface/layouts/grid.md)
