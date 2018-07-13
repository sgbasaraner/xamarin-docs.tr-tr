---
title: Bölüm 4 özeti. Yığını kaydırma
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 4 özeti. Yığını kaydırma'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3571774ddec4182f35cac6f13d4582235e2ff31a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997432"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Bölüm 4 özeti. Yığını kaydırma

Bu bölümde, öncelikli olarak kavramını tanıtımı için ayrılmıştır *Düzen*, sınıflar ve Xamarin.Forms birden çok görünüm sayfasında görünümünü düzenlemek için kullanılır teknikler için genel terim olduğu.

Düzen öğesinden türetilen birkaç sınıfları içerir [ `Layout` ](xref:Xamarin.Forms.Layout) ve [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1). Bu bölümde odaklanır [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).

Ayrıca bu bölümde sunulan olan [ `ScrollView` ](xref:Xamarin.Forms.ScrollView), [ `Frame` ](xref:Xamarin.Forms.Frame), ve [ `BoxView` ](xref:Xamarin.Forms.BoxView) sınıfları.

## <a name="stacks-of-views"></a>Yığınlar görünümü

[`StackLayout`](xref:Xamarin.Forms.StackLayout) öğesinden türetilen `Layout<View>` ve devralan bir [ `Children` ](xref:Xamarin.Forms.Layout`1) türünün özelliği `IList<View>`. Bu koleksiyona, birden çok görünüm öğeleri eklemek ve `StackLayout` bunları bir yatay veya dikey yığın içinde görüntüler.

Ayarlama [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) özelliği `StackLayout` üyesinin [ `StackOrientation` ](xref:Xamarin.Forms.StackOrientation) numaralandırma ya da [ `Vertical` ](xref:Xamarin.Forms.StackOrientation.Vertical) veya [ `Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). Varsayılan, `Vertical` değeridir.

Ayarlama [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) özelliği `StackLayout` için bir `double` alt aralığını belirtmek için değer. Varsayılan değer 6'dır.

Kod içinde öğeleri ekleyebilirsiniz `Children` koleksiyonunu `StackLayout` içinde bir `for` veya `foreach` döngü gösterildiği şekilde [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) veya örnek Başlat `Children` tek bir görünüm olarak içinde kanıtlanabilir bir liste koleksiyonunu [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Alt öğesinden türetilmelidir `View` ancak diğer içerebilir `StackLayout` nesneleri.

## <a name="scrolling-content"></a>İçerik kaydırma

Varsa bir `StackLayout` bir sayfasında görüntülenecek çok fazla alt koyabilirsiniz `StackLayout` içinde bir [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) kaydırma izin vermek için.

Ayarlama [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) özelliği `ScrollView` kaydırma istediğiniz görünümü. Bu genellikle, bir `StackLayout`, ancak herhangi bir görünüm olabilir.

Ayarlama [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) özelliği `ScrollView` üyesinin [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) özelliği [ `Vertical` ](xref:Xamarin.Forms.ScrollOrientation.Vertical), [ `Horizontal` ](xref:Xamarin.Forms.ScrollOrientation.Horizontal), veya [ `Both` ](xref:Xamarin.Forms.ScrollOrientation.Both). Varsayılan, `Vertical` değeridir. İçeriği bir `ScrollView` olduğu bir `StackLayout`, iki yönler tutarlı olmalıdır.

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) örnek, kullanımını gösterir `ScrollView` ve `StackLayout` kullanılabilir renklerin görüntülenecek. Örnek ayrıca tüm ortak statik özellikler ve alanları almak için .NET, yansıtma kullanmayı gösteren `Color` doğrudan belirterek listelemek zorunda kalmadan yapısı.

## <a name="the-expands-option"></a>Genişler seçeneği

Olduğunda bir `StackLayout` yığınları alt öğelerinde harcanan, her alt toplam yüksekliği içinde belirli bir yuva kapladığı `StackLayout` çocuğun boyutunu ve ayarlarını bağlıdır, `HorizontalOptions` ve `VerticalOptions` özellikleri. Bu özellikler türündeki değerler atanır [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

`LayoutOptions` Yapısını tanımlayan iki özellikleri:

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) numaralandırma türü [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment) dört üyeleriyle [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center), [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), ve [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) türü `bool`

Size kolaylık sağlamak için `LayoutOptions` yapı türünde sekiz statik salt okunur alanlar da tanımlar `LayoutOptions` kombinasyonların tümünde iki örnek özelliklerini kapsar:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

Aşağıdaki tartışma içeren bir `StackLayout` varsayılan dikey yönlendirme kullanıldığında. Yatay `StackLayout` benzerdir.

Dikey için `StackLayout`, `HorizontalOptions` ayarı, bir alt içinde genişliğini yatay olarak nasıl konumlandırılacağını belirler `StackLayout`. Bir `Alignment` ayarıyla `Start`, `Center`, veya `End` alt yatay sınırlandırılmamış olması neden olur. Alt kendi genişliğini belirler ve sol, Orta veya sağ tarafında konumlandırılan `StackLayout`. `Fill` Seçeneği yatay kısıtlı alt neden olur ve genişliğini kaplayacak şekilde `StackLayout`.

Dikey için `StackLayout`, her alt dikey sınırlandırılmamış ve alır dikey bir yuva alt öğenin yükseklik bağlı olarak, bu durumda `VerticalOptions` ayarı bizim konumuzla ilgili değildir.

Varsa dikey `StackLayout` kendisini sınırlandırılmamış&mdash;diğer bir deyişle, kendi `VerticalOptions` ayardır `Start`, `Center`, veya `End`, ardından yüksekliğini `StackLayout` alt toplam yüksekliği.

Ancak, dikey `StackLayout` dikey kısıtlı&mdash;varsa kendi `VerticalOptions` ayardır `Fill` &mdash;ardından yüksekliğini `StackLayout` toplam daha büyük olabilir, kapsayıcı yüksekliği olur alt yüksekliği. Bu durumda ve en az bir alt varsa bir `VerticalOptions` ayarı bir `Expands` , bayrak `true`, sonra ek alana `StackLayout` ile tüm alt arasında eşit olarak ayrılan bir `Expands` , bayrak `true`. Alt toplam yüksekliği ardından yüksekliği eşit olacaktır `StackLayout`ve `Alignment` parçası `VerticalOptions` ayarı, alt kendi yuvasında dikey olarak nasıl konumlandırılacağını belirler.

Bu gösterilmiştir [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) örnek.

## <a name="frame-and-boxview"></a>Çerçeve ve BoxView

Bu iki dikdörtgen görünümleri genellikle sunu amaçlar için kullanılır.

[ `Frame` ](xref:Xamarin.Forms.Frame) Görünümü gibi bir düzen olabilir başka bir görünüme etrafında dikdörtgen bir çerçeve görüntüler `StackLayout`. `Frame` devralınan bir [ `Content` ](xref:Xamarin.Forms.ContentView.Content) özelliğinden [ `ContentView` ](xref:Xamarin.Forms.ContentView) içinde görüntülenecek görünüm ayarladığınız `Frame`. `Frame` Varsayılan saydamdır. Çerçeve görünümünü özelleştirmek için aşağıdaki üç özellikleri ayarlayın:

- [ `OutlineColor` ](xref:Xamarin.Forms.Frame.OutlineColor) Görünür yapmak için özellik. Ayarlanacak yaygındır `OutlineColor` için `Color.Accent` zaman bilmediğiniz temel renk şeması.
- [ `HasShadow` ](xref:Xamarin.Forms.Frame.HasShadow) Özelliği ayarlanabilir `true` siyah bir gölge iOS cihazlarda görüntülenecek.
- Ayarlama [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) özelliğini bir `Thickness` çerçeve ve çerçeve arasında bir boşluk bırakmayı değeri içerik. Varsayılan değer 20 tüm yüze birimidir.

`Frame` Varsayılan `HorizontalOptions` ve `VerticalOptions` değerlerini `LayoutOptions.Fill`, anlamına `Frame` kapsayıcısı doldurur. Diğer ayarlarla boyutunu `Frame` içeriği boyutu temel alınır.

`Frame` Gösterilmiştir [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) örnek.

[ `BoxView` ](xref:Xamarin.Forms.BoxView) Tarafından belirtilen renkli bir dikdörtgen alan görüntüler, [ `Color` ](xref:Xamarin.Forms.BoxView.Color) özelliği.

Varsa `BoxView` sınırlıdır (kendi `HorizontalOptions` ve `VerticalOptions` özelliklere sahip, varsayılan ayarlarına `LayoutOptions.Fill`), `BoxView` için mevcut olan alanı doldurur. Varsa `BoxView` sınırlandırılmamış olduğu (ile `HorizontalOptions` ve `LayoutOptions` ayarlarını `Start`, `Center`, veya `End`), varsayılan boyut 40 birimleri karenin sahiptir. A `BoxView` kısıtlı bir boyut ve diğerinde sınırlandırılmamış.

Genellikle, belirleyeceğim [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) ve [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) özelliklerini `BoxView` belirli bir boyutu vermek için. Bu gösterilmiştir [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) örnek.

Birden fazla örneğini kullanabilirsiniz `StackLayout` birleştirmek için bir `BoxView` ve birkaç `Label` içinde örnekler bir `Frame` belirli bir renk görüntülemek ve her biri bu görünümlerde koymak için bir `StackLayout` içinde bir `ScrollView` çekici oluşturmak için gösterilen renklerin listesi [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) örnek:

[![Üç ekran rengi bloğu](images/ch04fg11-small.png "liste, renkleri")](images/ch04fg11-large.png#lightbox "liste, renkleri")

## <a name="a-scrollview-in-a-stacklayout"></a>Bir StackLayout içinde bir ScrollView?

Yerleştirme bir `StackLayout` içinde bir `ScrollView` ortak ancak koyarak bir `ScrollView` içinde bir `StackLayout` da bazen kullanışlıdır. Teorik olarak bu mümkün olmaması gerekir çünkü dikey alt `StackLayout` dikey sınırlandırılmamış olan. Ancak bir `ScrollView` dikey olarak kısıtlanmalıdır. Kaydırma için kendi alt boyutu ardından belirleyebilirsiniz belirli bir yükseklikte verilmelidir.

Hile vermektir `ScrollView` alt `StackLayout` bir `VerticalOptions` ayarıyla `FillAndExpand`. Bu gösterilmiştir [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) örnek.

**BlackCat** örnek nasıl tanımlanacağını ve taşınabilir sınıf kitaplığı (PCL) katıştırılmış program kaynaklara erişim de gösterir. Bu paylaşılan varlık projeleriyle (SAP) gerçekleştirilebilir ancak olarak biraz zor bir işlemdir [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) örnek gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 4 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Bölüm 4 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Bölüm 4 F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
