---
title: "Bölüm 4 özeti. Yığın kaydırma"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 5ad53d7bc8c4ee54a47c4b327fb6f07bc1906ab9
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Bölüm 4 özeti. Yığın kaydırma

Bu bölümde kavramı tanıtımı için öncelikle ayrılan *düzeni*, birden çok görünüm sayfasında görsel görünümünü düzenlemek için Xamarin.Forms kullanır teknikleri ve sınıfları için genel terim olduğu.

Düzen öğesinden türetilen birden fazla sınıf içerir [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) ve [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/). Bu bölümde odaklanır [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

Ayrıca bu bölümde sunulan olan [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/), [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/), ve [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) sınıfları.

## <a name="stacks-of-views"></a>Görünüm yığınları

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) türetilen `Layout<View>` ve devralan bir [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) türündeki özelliği `IList<View>`. Bu koleksiyona, birden çok görünüm öğe ekleyin ve `StackLayout` yatay veya dikey yığında görüntüler.

Ayarlama [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) özelliği `StackLayout` üyesi için [ `StackOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackOrientation/) numaralandırma, ya da [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Vertical/) veya [ `Horizontal`](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Horizontal/). Varsayılan, `Vertical` değeridir.

Ayarlama [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) özelliği `StackLayout` için bir `double` alt aralığını belirtmek için değer. Varsayılan değer 6'dır.

Kod içinde öğeleri ekleyebilirsiniz `Children` koleksiyonunu `StackLayout` içinde bir `for` veya `foreach` döngü örnekte gösterildiği şekilde [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) örnek veya başlatma `Children` tek bir görünüm içindeki kanıtlanabilir olarak listesiyle koleksiyonunu [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Alt öğesinden türetilmelidir `View` ancak diğer içerebilir `StackLayout` nesneleri.

## <a name="scrolling-content"></a>İçerik kaydırma

Varsa bir `StackLayout` bir sayfasında görüntülenecek çok fazla sayıda alt öğeler içerdiğinden, koyabilirsiniz `StackLayout` içinde bir [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) kaydırma izin vermek için.

Ayarlama [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) özelliği `ScrollView` kaydırma istediğiniz görünümü için. Bu genellikle olur bir `StackLayout`, ancak herhangi bir görünüm olabilir.

Ayarlama [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) özelliği `ScrollView` üyesi için [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/) özelliği, [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Vertical/), [ `Horizontal` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Horizontal/), veya [ `Both` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Both/). Varsayılan, `Vertical` değeridir. Varsa içeriğini bir `ScrollView` olan bir `StackLayout`, iki yönler tutarlı olmalıdır.

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) kullanımını gösteren örnek `ScrollView` ve `StackLayout` kullanılabilen renkleri görüntülemek için. Örnek ayrıca .NET yansıma tüm ortak statik özellikleri ve alanları almak için nasıl kullanılacağını gösterir `Color` açıkça bunları listesinde gerek kalmadan yapısı.

## <a name="the-expands-option"></a>Genişler seçeneği

Zaman bir `StackLayout` yığınları, alt öğelerini, her alt toplam yüksekliği içinde belirli bir yuva kapladığı `StackLayout` çocuğun boyutu ve ayarlarına bağlıdır, `HorizontalOptions` ve `VerticalOptions` özellikleri. Bu özellikleri türü değerleri atanan [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

`LayoutOptions` Yapısı iki özellik tanımlar:

- [`Alignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) Numaralandırma türünün [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/) dört üyeleriyle [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), ve [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)
- [`Expands`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) türü `bool`

Size kolaylık sağlamak için `LayoutOptions` yapısı ayrıca türünde sekiz statik salt okunur alanlar tanımlar `LayoutOptions` tüm bileşimleri iki örnek özelliklerini kapsar:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

Aşağıdaki tartışma içerir bir `StackLayout` varsayılan dikey yönlendirme ile. Yatay `StackLayout` benzerdir.

Dikey için `StackLayout`, `HorizontalOptions` ayarı, bir alt genişliğini içinde yatay olarak nasıl konumlandırılacağını belirler `StackLayout`. Bir `Alignment` ayarıyla `Start`, `Center`, veya `End` alt yatay Kısıtlanmamış olması neden olur. Alt kendi genişliğini belirler ve sola, merkezi veya sağına yerleştirilir `StackLayout`. `Fill` Seçeneği yatay kısıtlı alt neden olur ve genişliğini doldurur `StackLayout`.

Dikey için `StackLayout`, her alt dikey Kısıtlanmamış ve alır dikey yuva çocuğun yükseklik bağlı olarak, bu durumda `VerticalOptions` ayardır ilgisiz.

Varsa dikey `StackLayout` kendisini Kısıtlanmamış & olan #x 2014; varsa, `VerticalOptions` ayar `Start`, `Center`, veya `End`, ardından yüksekliğini `StackLayout` alt toplam yüksekliği.

Ancak, dikey `StackLayout` dikey kısıtlı & #x 2014 if kendi `VerticalOptions` ayar `Fill`& #x 2014; ardından yüksekliğini `StackLayout` toplam yüksekliği büyük olabilir, kapsayıcı yüksekliğini olacaktır alt. Bu durumda ve en az bir alt öğe varsa bir `VerticalOptions` ayarı bir `Expands` işareti `true`, ardından ek alanı `StackLayout` ile bu alt öğeleri arasında eşit olarak ayrılan bir `Expands` işareti `true`. Alt toplam yüksekliği yüksekliğini sonra eşit `StackLayout`ve `Alignment` parçası `VerticalOptions` ayarı, alt kendi yuvasında dikey olarak nasıl konumlandırılacağını belirler.

Bu, gösterilmiştir [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) örnek.

## <a name="frame-and-boxview"></a>Çerçeve ve BoxView

Bu iki dikdörtgen görünümleri genellikle sunu amaçlar için kullanılır.

[ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) Görüntüleyen bir düzen gibi olabilir başka bir görünüm etrafında dikdörtgen kare `StackLayout`. `Frame` devralınan bir [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) özelliğinden [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) içinde görüntülenecek görünüm ayarlanan `Frame`. `Frame` Varsayılan olarak saydamdır. Çerçevenin görünümünü özelleştirmek için aşağıdaki üç özellikleri ayarlayın:

- [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/) Görünür hale getirmek için özellik. Ayarlamak için ortak olan `OutlineColor` için `Color.Accent` zaman bilmediğiniz temel renk düzenini.
- [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/) Özelliği ayarlanabilir `true` siyah gölge iOS cihazlarda görüntülenecek.
- Ayarlama [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) özelliğine bir `Thickness` çerçeve ve çerçeve arasında boşluk bırakmak için değer içerik. Varsayılan değer 20 tüm yüze birimidir.

`Frame` Varsayılan `HorizontalOptions` ve `VerticalOptions` değerlerini `LayoutOptions.Fill`, anlamına `Frame` kapsayıcısı doldurur. Diğer ayarlarla boyutunu `Frame` içeriğini boyutuna göre.

`Frame` Örneklerde gösterildiği [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) örnek.

[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Dikdörtgen tarafından belirtilen renk görüntüler kendi [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) özelliği.

Varsa `BoxView` sınırlı değildir (kendi `HorizontalOptions` ve `VerticalOptions` özelliklerine sahip kullanıcıların varsayılan ayarlarını `LayoutOptions.Fill`), `BoxView` kullanılabilir alanı için doldurur. Varsa `BoxView` Kısıtlanmamış olduğundan (ile `HorizontalOptions` ve `LayoutOptions` ayarlarını `Start`, `Center`, veya `End`), bir varsayılan boyut 40 birimleri kare sahiptir. A `BoxView` bir boyuttaki kısıtlı ve diğer Kısıtlanmamış.

Genellikle, ayarlarız [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) ve [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) özelliklerini `BoxView` belirli bir boyuta vermek için. Bu tarafından gösterilmiştir [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) örnek.

Birden çok örneği kullanabilirsiniz `StackLayout` birleştirmek için bir `BoxView` ve birkaç `Label` içinde örnekler bir `Frame` belirli bir renk görüntülemek ve bu görünümlerin her biri yerleştirmek için bir `StackLayout` içinde bir `ScrollView` çekici oluşturmak için gösterilen renk listesi [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) örnek:

[![Üçlü ekran rengi bloklarının](images/ch04fg11-small.png "listesi, renkler")](images/ch04fg11-large.png#lightbox "listesi, renkler")

## <a name="a-scrollview-in-a-stacklayout"></a>Bir StackLayout içinde ScrollView?

Koyma bir `StackLayout` içinde bir `ScrollView` ortak ancak koyma bir `ScrollView` içinde bir `StackLayout` ayrıca bazı durumlarda kullanışlıdır. Teorik olarak bu mümkün olmaması gerekir çünkü dikey alt `StackLayout` dikey Kısıtlanmamış şunlardır. Ancak bir `ScrollView` dikey olarak kısıtlanmalıdır. Kaydırma için kendi alt boyutunu sonra belirleyebilmesi belirli bir yükseklikte verilmelidir.

Eli vermektir `ScrollView` alt `StackLayout` bir `VerticalOptions` ayarıyla `FillAndExpand`. Bu, gösterilmiştir [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) örnek.

**BlackCat** örnek ayrıca tanımlayın ve taşınabilir sınıf kitaplığı (PCL) katıştırılmış program kaynaklara erişmek nasıl gösterir. Bu da paylaşılan varlık projeleriyle (SAP) elde ancak işlem olarak biraz daha [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) örnek gösterilmektedir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 4 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Bölüm 4 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Bölüm 4 F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
