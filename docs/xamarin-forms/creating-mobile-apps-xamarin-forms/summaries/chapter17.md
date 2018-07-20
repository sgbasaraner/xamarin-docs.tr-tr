---
title: Bölüm 17 özeti. Kılavuzda Uzmanlaşma
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 17 özeti. Kılavuzda Uzmanlaşma'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 4ce734f629c13b1419611c0a8fea1dfe4bbaa4ef
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156749"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Bölüm 17 özeti. Kılavuzda Uzmanlaşma

[ `Grid` ](xref:Xamarin.Forms.Grid) Alt satırlar ve sütunlar hücre halinde düzenler güçlü Düzen mekanizmadır. Benzer HTML aksine `table` öğesi `Grid` yalnızca sunu yerine Düzen amaçlıdır.

## <a name="the-basic-grid"></a>Temel kılavuz

`Grid` öğesinden türetilen [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1), tanımlayan bir [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) özelliği, `Grid` devralır. Bu koleksiyonda XAML veya kod doldurabilirsiniz.

### <a name="the-grid-in-xaml"></a>XAML kılavuz

Tanımı bir `Grid` XAML içinde genellikle doldurma ile başlar [ `RowDefinitions` ](xref:Xamarin.Forms.Grid.RowDefinitions) ve [ `ColumnDefinitions` ](xref:Xamarin.Forms.Grid.ColumnDefinitions) koleksiyonları `Grid` ile [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) ve [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition) nesneleri. Bu, satır sayısı ve sütunlarını kurmak nasıl `Grid`ve özellikleri.

`RowDefinition` sahip bir [ `Height` ](xref:Xamarin.Forms.RowDefinition.Height) özelliği ve `ColumnDefinition` sahip bir [ `Width` ](xref:Xamarin.Forms.ColumnDefinition.Width) özelliği, her iki tür [ `GridLength` ](xref:Xamarin.Forms.GridLength), bir yapı.

XAML içinde [ `GridLengthTypeConverter` ](xref:Xamarin.Forms.GridLengthTypeConverter) basit metin dizelerine dönüştürür `GridLength` değerleri. Planda, [ `GridLength` Oluşturucusu](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType)) oluşturur `GridLength` değerine göre bir sayı ve bir değer türü [ `GridUnitType` ](xref:Xamarin.Forms.GridUnitType), üç üyesi olan bir sabit listesi:

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; CİHAZDAN bağımsız birimler (XAML içinde bir sayı) genişliği veya yüksekliği belirtilen
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; Genişlik ve yükseklik autosized hücre içeriğini (XAML içinde "Auto") dayalı olan
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; Kalan enlerini veya boylarını ayrılır orantılı olarak (bir sayı ile "\*" adlı *yıldız*, XAML içinde)

Her alt `Grid` satır ve sütun (açıkça veya dolaylı olarak) atanmış olmalısınız. Yayılan satır ve sütun yayılma isteğe bağlıdır. Bu tüm ekli bağlanabilir özellikler kullanılarak belirtilir &mdash; tarafından tanımlanan özellikler `Grid` alt ayarlanmış ancak `Grid`. `Grid` dört adet statik ekli bağlanabilir özellikler tanımlar:

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; sıfır tabanlı satır; Varsayılan 0'dır
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; sıfır tabanlı sütunu; Varsayılan 0'dır
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; sayı satırlarını alt yayılır; Varsayılan 1'dir
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; sayı sütunlarının alt yayılır; Varsayılan 1'dir

Kod içinde bir program ayarlamak ve bu değerleri almak için sekiz statik yöntemleri kullanın:

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) ve [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) ve [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) ve [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) ve [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

XAML içinde şu öznitelikler, bu değerleri ayarlamak için kullanın:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) oluşturma ve başlatma örneği gösterir bir `Grid` XAML içinde.

`Grid` Devralan [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) özelliğinden `Layout` ve satırları ve sütunları arasındaki boşluğu sağlayan iki ek özelliklerini tanımlar:

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) 6'ın varsayılan bir değeri yok
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) 6'ın varsayılan bir değeri yok

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
