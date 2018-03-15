---
title: "Bölüm 26 özeti. Özel düzenler"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b17924ee2ee7c944b3924ec79568f07feb4449a7
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Bölüm 26 özeti. Özel düzenler

Xamarin.Forms türetilen birden fazla sınıf içerir [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout`, ve
* `RelativeLayout`.

Bu bölümde öğesinden türetilen kendi sınıflarınızı oluşturmayı açıklar `Layout<View>`.

## <a name="an-overview-of-layout"></a>Düzen genel bakış

Xamarin.Forms Düzen işleme hiçbir merkezi sistemi yoktur. Her öğe, ne kendi boyutu olmalıdır ve kendisini içinde belirli bir alandaki işlemesini nasıl belirlemek için sorumludur.

### <a name="parents-and-children"></a>Üst ve alt öğeleri

Alt öğesi yok her öğe, bu Çocuklar kendi içinde konumlandırma için sorumludur. Sonuçta ne alt boyutunu belirleyen üst bağlı olduğundan boyutuna kullanılabilir varsa ve boyutu alt olmak ister.

### <a name="sizing-and-positioning"></a>Boyutlandırma ve konumlandırma

Düzen sayfası görsel ağaç üstündeki başlar ve tüm dalları devam eder. En önemli ortak yerleşim yöntemidir [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) tarafından tanımlanan `VisualElement`. Bir üst öğe diğer öğeleri çağrıları için her öğenin `Layout` her boyut ve kendisini göreli bir konum biçiminde alt vermek için alt öğelerinin bir [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) değeri. Bunlar `Layout` çağrıları aracılığıyla görsel ağacın yayılması.

Çağrı `Layout` ekranda görüntülenen bir öğe için gereklidir ve ayarlanması aşağıdaki salt okunur özellikler neden olur. İle tutarlı `Rectangle` yönteme geçirilen:

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) türü `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) türü `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) türü `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) türü `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) türü `double`

Öncesinde `Layout` çağrısı, `Height` ve `Width` sahte değerleri &ndash;1.

Çağrı `Layout` ayrıca aşağıdaki korumalı yöntemler çağrıları tetikler:

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/), hangi çağrıları
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/), hangi geçersiz kılındı.

Son olarak, aşağıdaki olay tetiklenir:

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

`OnSizeAllocated` Yöntemi geçersiz kılındı `Page` ve `Layout`, yalnızca iki alt bulunabilir Xamarin.Forms sınıflarda olduğu. Geçersiz kılınan yöntem çağrıları

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) için `Page` türevleri ve [ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/) için `Layout` çağırır türevleri
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) için `Page` türevleri ve [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) için `Layout` türevleri.

`LayoutChildren` Daha sonra çağırır `Layout` her öğenin alt öğelerinin. En az bir alt yeni varsa `Bounds` ayarı aşağıdaki olay tetiklenir sonra:

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) için `Page` türevleri ve [ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/) için `Layout` türevleri

### <a name="constraints-and-size-requests"></a>Kısıtlamalar ve boyutu istekleri

İçin `LayoutChildren` akıllıca çağırmak için `Layout` tüm alt öğelerini hakkında bilmeniz gerekir bir *tercih edilen* veya *istenen* çocuklar için boyut. Bu nedenle çağrıları `Layout` her alt genellikle yapılan çağrılar tarafından öncesinde olur

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

Kitap yayımladıktan sonra `GetSizeRequest` yöntemi kullanım ve değiştirilecek

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

`Measure` Yöntemi düzenler [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) özelliği ve türünde bir bağımsız değişken içeriyor [ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/), iki üye vardır:

- [`IncludeMargins`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.IncludeMargins/)
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.None/) Kenar boşlukları içermeyecek şekilde

Birçok öğelerin `GetSizeRequest` veya `Measure` kendi Oluşturucu öğe yerel boyutunu alır. Her iki yöntem genişlik ve yükseklik parametrelere sahip *kısıtlamaları*. Örneğin, bir `Label` genişliği kısıtlaması birkaç satırlık metin kaydırma nasıl kullanılacağını belirlemek için kullanır.

Her ikisi de `GetSizeRequest`ve `Measure` türünde bir değer döndürmesi [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/), iki özellik vardır:

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) türü `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) türü `Size`

Çok sık bu iki değer aynıdır ve `Minimum` değeri genellikle yoksayıldı.

`VisualElement` Ayrıca benzer bir korumalı yöntemi tanımlar `GetSizeRequest` gelen adlı `GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) döndüren bir `SizeRequest` değeri

Bu yöntem artık kullanım dışı ve değiştirilecek:

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

Öğesinden türetilen her sınıf `Layout` veya `Layout<T>` geçersiz kılmanız gerekir `OnSizeRequest` veya `OnMeasure`. Burada bir düzen sınıf genellikle çağırarak alır, alt öğelerini boyutuna göre kendi boyutunu belirler budur `GetSizeRequest` veya `Measure` alt üzerinde. Önce ve sonra arama `OnSizeRequest` veya `OnMeasure`, `GetSizeRequest` veya `Measure` aşağıdaki özelliklerine bağlı düzeltmeleri yapar:

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)tür `double`, etkiler `Request` özelliği `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) tür `double`, etkiler `Request` özelliği `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/) tür `double`, etkiler `Minimum` özelliği `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/) tür `double`, etkiler `Minimum` özelliği `SizeRequest`

### <a name="infinite-constraints"></a>Sonsuz kısıtlamaları

Kısıtlama bağımsız değişkenlerini geçirilen `GetSizeRequest` (veya `Measure`) ve `OnSizeRequest` (veya `OnMeasure`) sonsuz olabilir (yani, değerleri `Double.PositiveInfinity`). Ancak, `SizeRequest` bunlardan döndürülen yöntemleri sonsuz boyutları içeremez.

İstenen boyuta öğenin doğal boyutu yansıtmalıdır sonsuz kısıtlamaları gösteriyor. Dikey `StackLayout` çağrıları `GetSizeRequest` (veya `Measure`) alt sonsuz yükseklik sınırlamalı üzerinde. Yatay yığın düzeni çağırır `GetSizeRequest` (veya `Measure`) alt sonsuz genişliği sınırlamalı üzerinde. Bir `AbsoluteLayout` çağrıları `GetSizeRequest` (veya `Measure`) alt sonsuz genişlik ve yükseklik kısıtlamalarına sahip üzerinde.

### <a name="peeking-inside-the-process"></a>İçindeki işlemi gözatma

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) bilgi basit bir düzen için istek görüntüler kısıtlama ve boyutu.

## <a name="deriving-from-layoutview"></a>Düzeninden türetme<View>

Bir özel düzen sınıfın türetildiği `Layout<View>`. İki sorumlulukları vardır:

- Geçersiz kılma `OnMeasure` çağırmak için `Measure` düzeni ait alt üzerinde. İstenen boyuta düzeni için Döndür
- Geçersiz kılma `LayoutChildren` çağırmak için `Layout` düzeni ait alt öğeleri hakkında

`for` Veya `foreach` bu geçersiz kılmaları döngüde tüm alt Atla, `IsVisible` özelliği ayarlanmış `false`.

Çağrı `OnMeasure` garanti edilmez. `OnMeasure` Düzen üst düzeni 's boyutu (örneğin, bir sayfayı dolduran bir düzen) yöneten varsa çağrılmaz. Bu nedenle, `LayoutChildren` sırasında elde edilen alt boyutları Bel olamaz `OnMeasure` çağırın. Çok sık `LayoutChildren` kendisini çağırmalısınız `Measure` düzeni ait alt üzerinde veya bir tür (daha sonra ele alınması için) mantığını önbellek boyutu uygulayabilirsiniz.

### <a name="an-easy-example"></a>Kolay örneği

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) örnek içeren basitleştirilmiş bir [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) sınıfı ve kullanımını Tanıtımı.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Basitleştirilmiş yatay ve dikey konumlandırma

İşlerden biri, `VerticalStack` gerçekleştirmelisiniz sırasında ortaya çıkan `LayoutChildren` geçersiz kılar. Yöntemi çocuk kullanır `HorizontalOptions` nasıl kendi yuvasında içinde alt konumunu belirlemek için özellik `VerticalStack`. Bunun yerine statik yöntemini çağırabilir [ `Layout.LayoutChildIntoBoundingRect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/). Bu yöntemi çağırır `Measure` kullanır ve alt kendi `HorizontalOptions` ve `VerticalOptions` belirtilen dikdörtgenin içindeki alt konumlandırmak için özellikler.

### <a name="invalidation"></a>Geçersiz kılma

Genellikle bir öğenin özellik değişikliği o öğeye düzeninde nasıl göründüğünü etkiler. Düzen yeni bir düzen tetiklemek için geçersiz kılınan olması gerekir.

`VisualElement` korumalı yöntem tanımlar [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/), genellikle herhangi bir bağlanabilirse özelliği özelliği değiştirildi işleyici tarafından değişiklik çağırıldığı öğenin boyutunu etkiler. `InvalidateMeasure` Yöntemi ateşlenir bir [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) olay.

`Layout` Sınıfı tanımlayan adlı benzer bir korumalı yöntem [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/), hangi bir `Layout` türevi nasıl yerleştirir ve alt boyutları etkileyen herhangi bir değişiklik için çağrı.

### <a name="some-rules-for-coding-layouts"></a>Düzenleri kodlamak için bazı kurallar

1. Tarafından tanımlanan özellikler `Layout<T>` türevleri bağlanabilir özellikleri tarafından yedeklenen ve özelliği değiştirildi işleyicileri çağırmalıdır `InvalidateLayout`.

2. A `Layout<T>` ekli bağlanabilir özelliklerini tanımlar türevi geçersiz kılma [ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/) bir özelliği değiştirildi işleyici, alt öğelerini eklemek için ve [ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/) , kaldırmak için işleyici. İşleyici bu değişiklikleri ekli bağlanabilir özelliklerini kontrol edin ve çağırarak yanıt `InvalidateLayout`.

3. A `Layout<T>` alt boyutları önbelleği uygulayan türevi geçersiz kılma `InvalidateLayout` ve [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) ve bu yöntemler çağrıldığında Önbelleği Temizle.

### <a name="a-layout-with-properties"></a>Bir düzen özellikleri

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) tüm alt gruplar aynı boyut ve alt öğeleri için bir satır (veya sütun) sarmalar olduğunu varsayar sonraki. Bunu tanımlayan bir `Orientation` gibi özelliği `StackLayout`, ve `ColumnSpacing` ve `RowSpacing` gibi özellikleri `Grid`, ve alt boyutları önbelleğe alır.

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) örnek yerleştiren bir `WrapLayout` içinde bir `ScrollView` stok fotoğraf görüntüleme.

### <a name="no-unconstrained-dimensions-allowed"></a>Kısıtlanmamış boyutları izin!

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) İçinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı, kendi içinde tüm alt öğelerini görüntülemek için tasarlanmıştır. Bu nedenle, Kısıtlanmamış boyutlarda Dağıt olamaz ve bir karşılaşılırsa, bir özel durum oluşturur.

[ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) örnek gösterilmektedir `UniformGridLayout`:

[![Fotoğraf kılavuzunun Üçlü ekran](images/ch26fg08-small.png "Tekdüzen Kılavuz düzeni")](images/ch26fg08-large.png#lightbox "Tekdüzen Kılavuz düzeni")

### <a name="overlapping-children"></a>Çakışan alt öğeleri

A `Layout<T>` türevi alt çakışıyor. Ancak, alt kendi sırada işlenir `Children` toplama ve sırasını değil, kendi `Layout` yöntemleri çağrılmadan.

`Layout` Sınıfı koleksiyonun içindeki bir alt taşımanızı sağlayan iki yöntem tanımlar:

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) alt koleksiyon başına taşımak için
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) alt koleksiyonun sonuna taşımak için

Çakışan çocuklar için alt topluluğun sonunda koleksiyonunun başına alt öğeler üzerinde görsel olarak görünür.

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı işleme sırayı gösterir ve bu nedenle biri izin vermek için iliştirilmiş bir özellik tanımlar, Başkalarının üstünde görüntülenecek alt öğeleri. [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) örnek bu gösterilmektedir:

[![Üçlü ekran Öğrenci kartı dosya kılavuzunun](images/ch26fg10-small.png "çakışan düzeni alt")](images/ch26fg10-large.png#lightbox "çakışan düzeni alt")

### <a name="more-attached-bindable-properties"></a>Daha fazla iliştirilmiş bağlanabilir Özellikler

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı tanımlayan iki belirtmek için iliştirilmiş bağlanabilir Özellikler `Point` değerleri ve bir kalınlığı değeri ve yöneten `BoxView` satırları benzeyecek şekilde öğeleri.

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) örnek bir 3B küp çizmek için kullanır.

### <a name="layout-and-layoutto"></a>Düzen ve LayoutTo

A `Layout<T>` türevi çağırabilir `LayoutTo` yerine `Layout` düzeni animasyon uygulamak için. [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) Sınıfı bunu yapar ve [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) örnek gösterilmektedir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 26 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Bölüm 26 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Özel düzenler oluşturma](~/xamarin-forms/user-interface/layouts/custom.md)
