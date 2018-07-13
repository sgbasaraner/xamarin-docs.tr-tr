---
title: Bölüm 14 özeti. Mutlak Düzen
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 14 özeti. Mutlak Düzen'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 54238b46b759497bc6c6738673b98db833399752
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998367"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Bölüm 14 özeti. Mutlak Düzen

Gibi `StackLayout`, [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout) türetildiği `Layout<View>` ve devralan bir `Children` özelliği. `AbsoluteLayout` alt öğeleri ve isteğe bağlı olarak, bunların boyutu konumlarını belirtmek Programcı gerektiren bir düzen sistemi uygular. Konumu alt sol üst köşesine göre sol üst köşesinin tarafından belirtilen `AbsoluteLayout` CİHAZDAN bağımsız birimler. `AbsoluteLayout` Ayrıca bir orantılı konumlandırma ve boyutlandırma özelliği uygular.

`AbsoluteLayout` yalnızca Programcı alt öğeleri üzerinde bir boyut düşüşüne neden olabilir durumlarda kullanılacak bir özel amaçlı düzen sistemi olarak düşünülmelidir (örneğin, `BoxView` öğeleri) veya ne zaman öğenin boyutunu etkilemez diğer alt öğelerini konumlandırma. `HorizontalOptions` Ve `VerticalOptions` özelliklerine sahip alt öğeleri üzerinde hiçbir etkisi bir `AbsoluteLayout`.

Bu bölümde, önemli bir özelliği de tanıtır *bağlanabilir özellikler bağlı* izin veren bir sınıf içinde tanımlanan özellikler (Bu durumda `AbsoluteLayout`) için başka bir sınıf eklenmesi (alt `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>Kodda AbsoluteLayout

Alt öğesi olarak ekleyebileceğiniz `Children` koleksiyonunu bir `AbsoluteLayout` standardını kullanarak [ `Add` ](xref:System.Collections.Generic.ICollection`1.Add*) yöntemi, ancak `AbsoluteLayout` ayrıca genişletilmiş sağlar [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) belirtmenize izin veren bir yönteme bir [ `Rectangle` ](xref:Xamarin.Forms.Rectangle). Başka bir [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) gerektirdiğine yalnızca bir [ `Point` ](xref:Xamarin.Forms.Point), bu durumda alt sınırlandırılmamış ve kendisini boyutları.

Oluşturabileceğiniz bir `Rectangle` değerini bir [Oluşturucusu](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double)) dört değer gerektiren &mdash; üst öğesiyle ilişkili alt öğenin sol üst köşesinin konumunu gösteren ilk iki ve gösteren ikinci iki çocuk boyutu. Ya da bir [Oluşturucusu](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size)) gerektiren bir `Point` ve [ `Size` ](xref:Xamarin.Forms.Size) değeri.

Bu `Add` yöntemleri gösterilen içinde [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), hangi konumları `BoxView` kullanarak öğeleri `Rectangle` değerleri ve `Label` bir kullanaraköğe`Point` değeri.

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) örnek kullanımları 32 `BoxView` chessboard düzeni oluşturmak için öğeleri. Program sunar `BoxView` 35 birimleri kare öğeleri sabit olarak kodlanmış boyutu. `AbsoluteLayout` Sahip kendi `HorizontalOptions` ve `VerticalOptions` kümesine `LayoutOptions.Center`, hangi nedenleri `AbsoluteLayout` 280 birimleri kare toplam boyutu için.

## <a name="attached-bindable-properties"></a>Ekli bağlanabilir Özellikler

Konum ve isteğe bağlı olarak bir alt öğesi boyutunu ayarlamak da mümkündür bir `AbsoluteLayout` için eklendikten sonra `Children` statik yöntemini kullanarak koleksiyon [ `AbsoluteLayout.SetLayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle)). Alt ilk bağımsız değişken olan; İkinci bir `Rectangle` nesne. Alt kendisi yatay olarak ve/veya dikey olarak genişlik ve yükseklik değerleri ayarlayarak boyutları gerektiğini belirtebileceğiniz [ `AbsoluteLayout.AutoSize` ](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) sabit.

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) örnek puts `AbsoluteLayout` içinde bir `ContentView` ile bir `SizeChanged` çağrılacak işleyici `AbsoluteLayout.SetLayoutBounds` olabildiğince büyük olacak şekilde tüm alt öğeleri üzerinde.  

Ekli bağlanılabilir özellik, `AbsoluteLayout` tanımlar statik salt okunur alan türü `BindableProperty` adlı [ `AbsoluteLayout.LayoutBoundsProperty` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty). Statik `AbsoluteLayout.SetLayoutBounds` yöntemi çağrılarak gerçekleştirilir `SetValue` ile alt `AbsoluteLayout.LayoutBoundsProperty`. Alt ekli bağlanılabilir özellik ve değerinin depolandığı bir sözlük içerir. Yerleşim sırasında `AbsoluteLayout` değeri çağırarak elde edebileceğiniz [ `AbsoluteLayout.GetLayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject)), ile uygulanan bir `GetValue` çağırın.

## <a name="proportional-sizing-and-positioning"></a>Orantılı boyutlandırma ve konumlandırma

`AbsoluteLayout` orantılı boyutlandırma ve konumlandırma özelliğini uygular. Sınıfı bir ikinci iliştirilmiş bağlanabilir özelliği tanımlayan [ `LayoutFlagsProperty` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty), ilgili statik yöntemleriyle [ `AbsoluteLayout.SetLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags)) ve [ `AbsoluteLayout.GetLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject)).

Bağımsız değişkeni `AbsoluteLayout.SetLayoutFlags` ve dönüş değeri `AbsoluteLayout.GetLayoutFlags` türü değeri [ `AbsoluteLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayoutFlags), aşağıdaki üyeleri olan bir sabit listesi:

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None) (0 eşit)
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF)

C# bit düzeyinde OR işleci ile birleştirebilirsiniz.

Bu bayrakları ayarlanmış belirli özellikleri `Rectangle` getirin ve alt boyut için kullanılan Düzen sınırları yapısı orantılı olarak yorumlanır.

Zaman `WidthProportional` bayrağı ayarlandığında, bir `Width` değeri aynı genişlikte alt olan 1 anlamına gelir `AbsoluteLayout`. Yüksekliğini benzer bir yaklaşım kullanılır.

Orantılı konumlandırma boyutu dikkate alır. Zaman `XProportional` bayrağı ayarlandığında, `X` özelliği `Rectangle` Düzen sınırları orantılı. Bir alt kenarı sola 0 değeri anlamına gelir sol ucuna konumlandırılmış `AbsoluteLayout`, ancak 1 anlamına gelir alt öğenin sağ kenarı sağ ucuna konumlandırılmış bir konuma `AbsoluteLayout`, sağ kenarının ötesine değil `AbsoluteLayout` expec olabileceği gibi t. Bir `X` 0,5 özelliği merkezleri yatay içinde alt `AbsoluteLayout`.

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) örnek orantılı boyutlandırma ve konumlama kullanımını gösterir.

## <a name="working-with-proportional-coordinates"></a>Orantılı koordinatları ile çalışma

Bazı durumlarda, farklı bir şekilde nasıl öğesinde uygulanır orantılı konumlandırma düşünme daha kolay bir şekilde `AbsoluteLayout`. Orantılı koordinatları ile çalışmak tercih edebilirsiniz burada bir `X` 1 özelliğini alt öğenin sol kenarı (sağ kenarı yerine) karşı sağ kenarında konumlandırır `AbsoluteLayout`.

Bu alternatif konumlandırma Düzen "kesirli alt koordinat." çağrılabilir Gerekli yerleşimi sınırları kesirli alt koordinatlarından dönüştürebilirsiniz `AbsoluteLayout` aşağıdaki formülü kullanarak:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) örnek bunu gösterir.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout ve XAML

Kullanabileceğiniz bir `AbsoluteLayout` XAML içinde ve ekli bağlanabilir özellikler üzerinde alt bir `AbsoluteLayout` öznitelik değerleri kullanarak `AbsoluteLayout.LayoutBounds` ve `AbsoluteLayout.LayoutFlags`. Bu gösterilmiştir [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) ve [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) örnekleri. İkinci program 32 içeren `BoxView` öğeleri ancak kullanan örtük `Style` içeren `AbsoluteLayout.LayoutFlags` en az aşağı biçimlendirme tutmak özelliği.

Bir öznitelik, bir sınıf adı, bir nokta ve özellik adı oluşur XAML *her zaman* ekli bağlanabilir özelliği.

## <a name="overlays"></a>Yer paylaşımları

Kullanabileceğiniz `AbsoluteLayout` oluşturmak için bir *katmana*, kapsayan sayfanın diğer denetimlerle belki de kullanıcı sayfasında normal denetimler ile etkileşim korumaya.

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) örnek bu tekniği gösterir ve ayrıca [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar), bir program tamamlandı ölçüde görüntüleyen bir Görev.

## <a name="some-fun"></a>Bazı eğlence

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) örnek sanal 5 7 nokta vuruşlu ekranıyla geçerli zamanı görüntüler. Her nokta olan bir `BoxView` (bunların 228 yoktur) boyutlandırıldığından ve konumlandırılıp `AbsoluteLayout`.

[![Üç nokta vuruşlu saat görüntüsü](images/ch14fg08-small.png "nokta vuruşlu saat")](images/ch14fg08-large.png#lightbox "nokta vuruşlu saati")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) program iki canlandırır `Label` Yatayda ve Dikeyde ekranda gelmesine nesneleri.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 14 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Bölüm 14 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
