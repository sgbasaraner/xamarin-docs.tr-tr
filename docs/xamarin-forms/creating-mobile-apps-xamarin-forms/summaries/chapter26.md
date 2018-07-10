---
title: Bölüm 26 özeti. Özel düzenler
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: bölüm 26 özeti. Özel düzenler'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b6ef23364cac0dd1459681aa92c7a7db58bc81f0
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935647"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Bölüm 26 özeti. Özel düzenler

Xamarin.Forms ile türetilmiş birkaç sınıflarını içerir [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout`, ve
* `RelativeLayout`.

Bu bölümde, türetilen kendi sınıflarınızı nasıl oluşturabileceğiniz açıklanmaktadır `Layout<View>`.

## <a name="an-overview-of-layout"></a>Düzen genel bakış

Xamarin.Forms düzenini işleyen merkezi sistemi yoktur. Her öğe, kendi boyutu ne olmalı ve kendisi içinde belirli bir alandaki işlemek nasıl belirlemekten sorumludur.

### <a name="parents-and-children"></a>Üst ve alt

Alt öğelere sahip her öğe için kendi içinde alt konumlandırma sorumludur. Sonuçta ne alt boyutunu belirler. üst dayalı olan boyutu kullanılabilir vardır ve alt boyut olmak istemez.

### <a name="sizing-and-positioning"></a>Boyutlandırma ve konumlandırma

Düzen sayfa görsel ağacın üstüne başlar ve tüm dalları devam eder. En önemli genel yöntem düzeninde [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) tarafından tanımlanan `VisualElement`. Bir üst öğe diğer öğeleri çağrıları için her öğenin `Layout` her alt öğelerinin boyutunu ve kendisine göre konumunun biçiminde alt vermek için bir [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) değeri. Bunlar `Layout` çağrıları yaymak görsel ağaç.

Bir çağrı `Layout` ekranında görüntülenecek bir öğe için gereklidir ve ayarlamak aşağıdaki salt okunur özellikler neden olur. Bunlar tutarlı `Rectangle` yönteme:

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) türü `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) türü `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) türü `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) türü `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) türü `double`

Öncesinde `Layout` çağrısı, `Height` ve `Width` sahte değerleri &ndash;1.

Bir çağrı `Layout` aşağıdaki korumalı yöntemlere yapılan çağrılar da tetikleyen:

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/), hangi çağrıları
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/), hangi geçersiz kılınabilir.

Son olarak, aşağıdaki olay harekete geçirilir:

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

`OnSizeAllocated` Yöntemi tarafından geçersiz kılınmıştır `Page` ve `Layout`, yalnızca iki alt öğeleri olan Xamarin.Forms sınıflardaki olduğu. Geçersiz kılınan yöntem çağrıları

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) için `Page` türevleri ve [ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/) için `Layout` form veya türevleri çağırır
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) için `Page` türevleri ve [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) için `Layout` türevleri.

`LayoutChildren` Daha sonra çağırır `Layout` her öğenin alt öğeleri. Bir yeni en az bir alt varsa `Bounds` ayarlama sonra aşağıdaki olay harekete geçirilir:

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) için `Page` türevleri ve [ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/) için `Layout` türevleri

### <a name="constraints-and-size-requests"></a>Sınırlamalar ve boyutu istekleri

İçin `LayoutChildren` akıllıca çağrılacak `Layout` alt öğeleri üzerinde bilmeniz gerekir bir *tercih edilen* veya *istenen* alt öğe boyutu. Bu nedenle çağrıları `Layout` her alt öğeleri için genellikle yapılan çağrılar tarafından öncelenen

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

Kitap yayımlanan sonra `GetSizeRequest` metodu kullanım dışı ve yerini şu alacak

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

`Measure` Yöntemi karşılar [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) özelliği ve türünde bir bağımsız değişken içeren [ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/), iki üyesi olan:

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) Kenar boşlukları içermeyecek şekilde

Birçok öğe için `GetSizeRequest` veya `Measure` kendi işleyicisi bağlantısı yerel öğenin boyutunu alır. Parametreleri genişliği ve yüksekliği için iki yöntem de sahip *kısıtlamaları*. Örneğin, bir `Label` genişlik birden çok metin satırı sarmalamak nasıl kullanılacağını belirlemek için kullanır.

Her ikisi de `GetSizeRequest`ve `Measure` türünde bir değer döndürmesi [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/), iki özelliği vardır:

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) türü `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) türü `Size`

Bu iki değerleri aynıysa, çok sık ve `Minimum` değeri genellikle dikkate.

`VisualElement` aynı zamanda benzeyen bir korumalı yöntem tanımlar `GetSizeRequest` gelen adlı `GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) döndürür bir `SizeRequest` değeri

Bu yöntem artık kullanım dışı ve yerini şu alacak:

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

Öğesinden türetilen her sınıf `Layout` veya `Layout<T>` kılmalı `OnSizeRequest` veya `OnMeasure`. Burada bir düzen sınıf genellikle çağırarak alır alt öğelerinin boyutunu temel aldığı kendi boyutunu belirler. Bu, `GetSizeRequest` veya `Measure` alt öğeleri üzerinde. Önce ve sonra arama `OnSizeRequest` veya `OnMeasure`, `GetSizeRequest` veya `Measure` aşağıdaki özelliğe göre ayarlamalar yapar:

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)tür `double`, etkiler `Request` özelliği `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) tür `double`, etkiler `Request` özelliği `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/) tür `double`, etkiler `Minimum` özelliği `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/) tür `double`, etkiler `Minimum` özelliği `SizeRequest`

### <a name="infinite-constraints"></a>Sonsuz kısıtlamaları

Kısıtlama bağımsız değişkenlerini geçirilen `GetSizeRequest` (veya `Measure`) ve `OnSizeRequest` (veya `OnMeasure`) sonsuz olabilir (yani, değerleri `Double.PositiveInfinity`). Ancak, `SizeRequest` bunlardan döndürülen yöntemleri sonsuz boyutları içeremez.

İstenen boyut öğenin doğal boyutu yansıtmalıdır sonsuz kısıtlamaları gösteriyor. Dikey `StackLayout` çağrıları `GetSizeRequest` (veya `Measure`) ile bir sonsuz yükseklik alt üzerinde. Yatay yığın düzeni çağırır `GetSizeRequest` (veya `Measure`) ile sonsuz bir genişlik alt üzerinde. Bir `AbsoluteLayout` çağrıları `GetSizeRequest` (veya `Measure`) sonsuz kısıtlamalarıyla genişlik ve yükseklik alt öğeleri üzerinde.

### <a name="peeking-inside-the-process"></a>İçindeki işlemi gözatma

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) bilgi basit düzen için istek görüntüler kısıtlaması ve boyutu.

## <a name="deriving-from-layoutview"></a>Düzenden türetme<View>

Özel düzen sınıfın türetildiği `Layout<View>`. Bu iki sorumluluklara sahiptir:

- Geçersiz kılma `OnMeasure` çağrılacak `Measure` Düzen ait alt öğeleri üzerinde. Düzeni için istenen bir boyutunu döndürür
- Geçersiz kılma `LayoutChildren` çağrılacak `Layout` Düzen ait alt öğeleri üzerinde

`for` Veya `foreach` bu geçersiz kılmaları döngüde tüm alt atlayın, `IsVisible` özelliği `false`.

Bir çağrı `OnMeasure` garanti edilmez. `OnMeasure` düzeninin üst Düzen'ın boyutu (örneğin, bir sayfayı dolduran bir düzen) imkanına sahiptiniz olursa çağrılacak değil. Bu nedenle, `LayoutChildren` sırasında elde edilen alt boyutları güvenemezsiniz `OnMeasure` çağırın. Çok sık, `LayoutChildren` kendisini çağırmalıdır `Measure` düzeni'nın alt üzerinde veya mantığı (ileride tartışılmayı) önbellek boyutu tür uygulayabilirsiniz.

### <a name="an-easy-example"></a>Bir kolayca örnek

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) örnek içeren bir Basitleştirilmiş [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) sınıfı ve kullanımını Tanıtımı.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Dikey ve yatay Basitleştirilmiş konumlandırma

İşlerden biri, `VerticalStack` gerçekleştirmelidir sistemdeki `LayoutChildren` geçersiz kılar. Çocuğun kullanmaktadır `HorizontalOptions` kendi yuvasında içinde alt konumlandırmak nasıl belirlemek için özellik `VerticalStack`. Bunun yerine statik yöntem çağırabilirsiniz [ `Layout.LayoutChildIntoBoundingRect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/). Bu yöntemin çağırdığı `Measure` kullanır ve alt kendi `HorizontalOptions` ve `VerticalOptions` belirtilen dikdörtgenin içindeki alt konumlandırmak için özellikleri.

### <a name="invalidation"></a>Geçersiz kılma

Genellikle bir öğenin özellik değişikliği o öğenin düzeninde nasıl göründüğünü etkiler. Düzen yeni bir düzen tetiklemek için geçersiz olması gerekir.

`VisualElement` korumalı bir yöntem tanımlar [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/), öğenin boyutunu etkiler, genellikle değişikliğin tüm bağlanılabilir özellik özellik değişti işleyici tarafından çağrılır. `InvalidateMeasure` Yöntemi ateşlenir bir [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) olay.

`Layout` Sınıf adlı benzer korumalı bir yöntem tanımlar [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/), bir `Layout` Türev nasıl yerleştirir ve alt öğelerindeki boyutları etkileyen herhangi bir değişiklik için çağırmalıdır.

### <a name="some-rules-for-coding-layouts"></a>Bazı düzenleri kodlama kuralları

1. Tarafından tanımlanan özellikler `Layout<T>` türevleri bağlanabilir özellikler tarafından desteklenen ve özellik değişti işleyicileri çağırmalıdır `InvalidateLayout`.

2. A `Layout<T>` ekli bağlanabilir özellikler tanımlar Türev geçersiz kılmalıdır [ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/) alt özelliği değiştirilmiş bir işleyici eklemek ve [ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/) , kaldırmak için işleyici. İşleyici bağlanabilir özellikler bu değişiklikleri bağlı olup olmadığını denetleyin ve çağırarak yanıt `InvalidateLayout`.

3. A `Layout<T>` alt boyutları önbelleğini uygulayan Türev geçersiz kılmalıdır `InvalidateLayout` ve [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) ve bu yöntemler çağrıldığında önbelleğini temizleyin.

### <a name="a-layout-with-properties"></a>Özelliklere sahip bir düzen

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) tüm alt öğelerini aynı boyut ve alt öğeleri için bir satır (veya sütun) sarmalar olduğunu varsayar sonraki. Tanımladığı bir `Orientation` özelliği gibi `StackLayout`, ve `ColumnSpacing` ve `RowSpacing` gibi özellikleri `Grid`, ve alt boyutları önbelleğe alır.

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) örnek yerleştiren bir `WrapLayout` içinde bir `ScrollView` stok fotoğrafları görüntülemek için.

### <a name="no-unconstrained-dimensions-allowed"></a>Sınırlandırılmamış boyutlara izin!

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) İçinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı, kendi içindeki tüm alt öğelerini görüntülemek için tasarlanmıştır. Bu nedenle, sınırlandırılmamış boyutlarla ilgili olamaz ve bir karşılaşılırsa özel durum harekete.

[ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) örnek gösterir `UniformGridLayout`:

[![Fotoğraf kılavuzunun üç ekran](images/ch26fg08-small.png "tek düzen Kılavuz düzeni")](images/ch26fg08-large.png#lightbox "tek düzen Kılavuz düzeni")

### <a name="overlapping-children"></a>Çakışan alt öğeleri

A `Layout<T>` Türev alt çakışma. Ancak, alt, sırayla işlenir `Children` toplama ve sırasını değil, kendi `Layout` yöntemleri çağrıldığında.

`Layout` Sınıfı, koleksiyon içindeki bir alt taşımanıza olanak sağlayan iki yöntem tanımlar:

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) alt koleksiyon başına taşımak için
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) alt koleksiyonun sonuna taşımak için

Çakışan alt öğeleri için koleksiyon başına alt öğeleri üzerinde alt koleksiyonun sonuna görsel olarak görünür.

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı işleme sırayı gösterir ve bu nedenle biri izin vermek için ekli özelliği tanımlar, Başkalarının en üstünde görüntülenecek alt öğeleri. [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) örnek bunu gösterir:

[![Öğrenci kart dosya kılavuzunun üç ekran](images/ch26fg10-small.png "çakışan Düzen alt")](images/ch26fg10-large.png#lightbox "çakışan Düzen alt öğeleri")

### <a name="more-attached-bindable-properties"></a>Daha fazla ekli bağlanabilir Özellikler

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı tanımlayan iki belirtmek için ekli bağlanabilir Özellikler `Point` değerleri ve kalınlığı değeri ve yöneten `BoxView` satırları benzeyecek şekilde öğeleri.

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) örnek 3D küp çizmek için bu kullanır.

### <a name="layout-and-layoutto"></a>Düzen ve LayoutTo

A `Layout<T>` Türev çağırabilir `LayoutTo` yerine `Layout` düzenini animasyon uygulamak için. [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) Sınıfı bunu, yapar ve [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) örnek gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 26 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Bölüm 26 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Özel düzenleri oluşturma](~/xamarin-forms/user-interface/layouts/custom.md)
