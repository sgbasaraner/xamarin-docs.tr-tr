---
title: "Bölüm 14 özeti. Mutlak düzeni"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ac4c41ebd70b58e95a3fa4fa7a391a473361b1db
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Bölüm 14 özeti. Mutlak düzeni

Gibi `StackLayout`, [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) türetilen `Layout<View>` ve devralan bir `Children` özelliği. `AbsoluteLayout` alt öğelerini ve isteğe bağlı olarak boyutlarına konumlarını belirtmek Programcı gerektiren bir Düzen sistemine uygular. Konumu alt sol üst köşesindeki göre sol üst köşesindeki tarafından belirtilen `AbsoluteLayout` CİHAZDAN bağımsız birimler. `AbsoluteLayout` Ayrıca bir orantılı konumlandırma ve boyutlandırma özelliğini uygular.

`AbsoluteLayout` Programcı alt boyutuna yalnızca uygulayabilir yapılırken kullanılacak özel amaçlı düzen sistemi olarak düşünülmelidir (örneğin, `BoxView` öğeleri) veya ne zaman öğenin boyutunu etkilemez diğer alt öğeleri konumlandırma. `HorizontalOptions` Ve `VerticalOptions` özelliklerine sahip alt üzerinde hiçbir etkisi bir `AbsoluteLayout`.

Bu bölümde Ayrıca, önemli özelliklerinden tanıtır *iliştirilmiş bağlanabilir Özellikler* bir sınıf tarafından tanımlanan özellikler izin ver (Bu durumda `AbsoluteLayout`) başka bir sınıfa bağlı olması için (bir alt `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>Kodda AbsoluteLayout

Alt öğesi olarak ekleyebileceğiniz `Children` koleksiyonu bir `AbsoluteLayout` standart kullanarak [ `Add` ](https://developer.xamarin.com/api/member/System.Collections.Generic.ICollection%3CT%3E.Add/p/T/) yöntemi, ancak `AbsoluteLayout` ayrıca genişletilmiş sağlar [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) belirtmenize olanak tanır yöntemi bir [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/). Başka bir [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) gerektirdiğine yalnızca bir [ `Point` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Point/), bu durumda alt Kısıtlanmamış ve kendisini boyutları.

Oluşturabileceğiniz bir `Rectangle` değerini bir [Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/System.Double/System.Double/System.Double/System.Double/) dört #x 2014; & değerleri gerektiren üst göre alt sol üst köşesindeki konumunu belirten ilk iki ve belirten ikinci iki Çocuğunuzun boyutu. Veya kullanabileceğiniz bir [Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/Xamarin.Forms.Point/Xamarin.Forms.Size/) gerektiren bir `Point` ve [ `Size` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/) değeri.

Bunlar `Add` yöntemleri gösterilen içinde [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), hangi konumları `BoxView` kullanarak öğeleri `Rectangle` değerleri ve bir `Label` yalnızca bir kullanaraköğe`Point` değeri.

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) örnek kullanan 32 `BoxView` chessboard düzeni oluşturmak için öğeleri. Program sunar `BoxView` 35 birimleri kare öğeleri bir sabit kodlanmış boyutu. `AbsoluteLayout` Sahip kendi `HorizontalOptions` ve `VerticalOptions` kümesine `LayoutOptions.Center`, hangi nedenler `AbsoluteLayout` 280 birimleri kare toplam boyutunu sağlamak için.

## <a name="attached-bindable-properties"></a>Ekli bağlanabilir Özellikler

Konum ve isteğe bağlı olarak, bir alt öğesi boyutunu ayarlamak mümkündür bir `AbsoluteLayout` için eklendikten sonra `Children` statik yöntemini kullanarak koleksiyonu [ `AbsoluteLayout.SetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutBounds/p/Xamarin.Forms.BindableObject/Xamarin.Forms.Rectangle/). İlk bağımsız değişken alt olan; İkinci bir `Rectangle` nesnesi. Alt kendisini yatay ve/veya dikey genişlik ve yükseklik değerleri ayarlayarak boyutları olduğunu belirtebilirsiniz [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) sabit.

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) örnek yerleştirmelerin `AbsoluteLayout` içinde bir `ContentView` ile bir `SizeChanged` çağrılacak işleyici `AbsoluteLayout.SetLayoutBounds` mümkün olduğu kadar büyük olmak üzere tüm alt öğeleri üzerinde.  

Ekli bağlanabilirse özellik, `AbsoluteLayout` tanımlayan statik salt okunur alan türü `BindableProperty` adlı [ `AbsoluteLayout.LayoutBoundsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/). Statik `AbsoluteLayout.SetLayoutBounds` yöntemi çağrılarak uygulanır `SetValue` ile alt `AbsoluteLayout.LayoutBoundsProperty`. Alt iliştirilmiş bağlanabilirse özelliği ve değerini depolandığı bir sözlük içerir. Yerleşim sırasında `AbsoluteLayout` çağırarak bu değeri elde edebileceğiniz [ `AbsoluteLayout.GetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutBounds/p/Xamarin.Forms.BindableObject/), ile uygulanan bir `GetValue` çağırın.

## <a name="proportional-sizing-and-positioning"></a>Orantılı boyutlandırma ve konumlandırma

`AbsoluteLayout` orantılı boyutlandırma ve özellik yerleştirme uygular. Sınıf bir ikinci iliştirilmiş bağlanabilirse özelliği tanımlayan [ `LayoutFlagsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/), ilgili statik yöntemleriyle [ `AbsoluteLayout.SetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutFlags/p/Xamarin.Forms.BindableObject/Xamarin.Forms.AbsoluteLayoutFlags/) ve [ `AbsoluteLayout.GetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutFlags/p/Xamarin.Forms.BindableObject/).

Bağımsız değişkeni `AbsoluteLayout.SetLayoutFlags` ve dönüş değerini `AbsoluteLayout.GetLayoutFlags` türünde bir değer [ `AbsoluteLayoutFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/), aşağıdaki üyeleri olan bir numaralandırma:

- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.None/) (0 değerine eşit)
- [`XProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.XProportional/) (1)
- [`YProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.YProportional/) (2)
- [`PositionProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional/) (3)
- [`WidthProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional/) (4)
- [`HeightProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional/) (8)
- [`SizeProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional/) (12)
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.All/) (\xFFFFFFFF)

C# bit düzeyinde OR işleci bunlarla birleştirebilirsiniz.

Bu bayrak kümesi belirli özelliklerini `Rectangle` konumu ve alt boyutu için kullanılan düzeni sınırları yapısı orantılı olarak yorumlanır.

Zaman `WidthProportional` bayrağı ayarlandığında, bir `Width` alt aynı genişlikte 1 anlamına gelir değerini `AbsoluteLayout`. Benzer bir yaklaşım yüksekliği için kullanılır.

Orantılı konumlandırma ve boyutunu dikkate alır. Zaman `XProportional` bayrağı ayarlandığında, `X` özelliği `Rectangle` düzeni sınırları doğru orantılıdır. Bir alt kenarı sol 0 değeri anlamına gelir sol kenarı konumlandırılmış `AbsoluteLayout`, ancak bir konum 1 anlamına gelir çocuk sağ kenarı sağ kenarına konumlandırıldı `AbsoluteLayout`, sağ kenarının ötesine değil `AbsoluteLayout` expec olabileceği gibi t. Bir `X` 0,5 özelliğinin merkezleri yatay içinde alt `AbsoluteLayout`.

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) örnek orantılı boyutlandırma ve konumlama kullanımını gösterir.

## <a name="working-with-proportional-coordinates"></a>Orantılı koordinatları ile çalışma

Bazı durumlarda, farklı bir şekilde nasıl şu uygulanan orantılı konumlandırma düşünmek daha kolay `AbsoluteLayout`. Orantılı koordinatları ile çalışmayı tercih burada bir `X` özelliği 1'in sağ köşesine karşı çocuğun sol kenarı (sağ kenarı yerine) konumlandırır `AbsoluteLayout`.

Bu alternatif konumlandırma düzeni "kesirli alt koordinatları." çağrılabilir Kesirli alt koordinatları için gerekli düzeni sınırları dönüştürebilir `AbsoluteLayout` aşağıdaki formülü kullanarak:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) örnek bu gösterir.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout ve XAML

Kullanabileceğiniz bir `AbsoluteLayout` XAML'de ekli bağlanabilir özelliklerini ve alt öğelerinin bir `AbsoluteLayout` öznitelik değerleri kullanılarak `AbsoluteLayout.LayoutBounds` ve `AbsoluteLayout.LayoutFlags`. Bu, gösterilmiştir [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) ve [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) örnekleri. İkinci programı 32 içeren `BoxView` öğeleri ancak kullanan bir örtük `Style` içeren `AbsoluteLayout.LayoutFlags` aşağıya doğru en az biçimlendirme tutmak için özellik.

XAML'de bir sınıf adı, bir nokta ve bir özellik adı oluşan bir özniteliği olan *her zaman* iliştirilmiş bağlanabilirse özellik.

## <a name="overlays"></a>Yer paylaşımları

Kullanabileceğiniz `AbsoluteLayout` oluşturmak için bir *katmana*, hangi kapsayan diğer denetimleri sayfasıyla belki de kullanıcı sayfasında normal denetimleri ile etkileşim korumak için. 

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) örnek bu tekniği gösterir ve ayrıca gösterir [ `ProgressBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/), bir program tamamlandı ölçüde görüntüleyen bir Görev.

## <a name="some-fun"></a>Bazı eğlenceli

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) örnek benzetimli 5 x 7 iğneli ekranıyla geçerli saati görüntüler. Her nokta bir `BoxView` (bunların 228 vardır) boyutlandırılmış ve üzerinde konumlandırılmış `AbsoluteLayout`.

[![Üçlü ekran iğneli saatinin](images/ch14fg08-small.png "iğneli saati")](images/ch14fg08-large.png "iğneli saati")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) program iki canlandırır `Label` yatay ve dikey olarak ekranda Sıçrama nesnelere.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 14 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Bölüm 14 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
