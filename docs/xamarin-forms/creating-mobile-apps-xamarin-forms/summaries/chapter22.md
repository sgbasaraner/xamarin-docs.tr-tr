---
title: Bölüm 22 özeti. Animasyon
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 22 özeti. Animasyon'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 58a49b92b654e673eddd63da29cdb1866a217fca
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996405"
---
# <a name="summary-of-chapter-22-animation"></a>Bölüm 22 özeti. Animasyon

Xamarin.Forms Zamanlayıcı kullanarak kendi animasyonlar oluşturabilirsiniz gördüğünüz veya `Task.Delay`, ancak Xamarin.Forms tarafından sağlanan animasyon özellikleri kullanarak genellikle daha kolay. Üç sınıf animasyonlarına uygulayın:

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions), üst düzey bir yaklaşım
- [`Animation`](xref:Xamarin.Forms.Animation), daha verimli ancak daha zor
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions), en çok yönlü, en düşük düzey yaklaşımı

Genellikle, animasyonları bağlanabilir özellikler tarafından desteklenen özellikler hedefleyin. Bu bir gereksinim değildir, ancak dinamik olarak değişikliklerine tepki özellikler yalnızca şunlardır.

Bu animasyonları için XAML arabirimi yoktur, ancak XAML içinde açıklanan teknikleri kullanarak animasyonları tümleştirebileceğiniz [ **Bölüm 23. Tetikleyiciler ve davranışlar**](chapter23.md).

## <a name="exploring-basic-animations"></a>Temel animasyon keşfetme

Temel animasyon işlevlerdir bulunan uzantı yöntemleri [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) sınıfı. Öğesinden türetilen herhangi bir nesne bu yöntemleri uygulamak `VisualElement`. Basit animasyonlar özellikleri ele alınan dönüşümleri hedef [ `Chapter 21. Transforms` ](chapter21.md).

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) gösterir nasıl `Clicked` için olay işleyicisi bir `Button` çağırabilirsiniz [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) dönmesi için genişletme yöntemi bir daire içinde düğmesi.

`RotateTo` Yöntemi değişiklikleri `Rotation` özelliği `Button` 0 (varsayılan) üçte birine, dörtte saniye boyunca 360. Varsa `Button` dokunulduğunda yeniden, ancak hiçbir şey yapmasa çünkü `Rotation` özelliğini 360 derece haldedir.

### <a name="setting-the-animation-duration"></a>Animasyon süresi ayarlama

İkinci bağımsız değişkeni `RotateTo` milisaniye cinsinden süresidir. Büyük bir değere ayarlayın dokunarak `Button` animasyon sırasında geçerli açı yeni bir animasyonun başından başlar.

### <a name="relative-animations"></a>Göreli animasyonları

`RelRotateTo` Yöntemi için mevcut değeri belirtilen bir değer ekleyerek göreli dönüş gerçekleştirir. Bu yöntem tanır `Button` birden çok kez ve her birinin incelenmeyi süresi arttıkça `Rotation` 360 derece özelliği.

### <a name="awaiting-animations"></a>Animasyonları bekleniyor

Tüm animasyon yöntemlere `ViewExtensions` dönüş `Task<bool>` nesneleri. Bu sıralı animasyonları kullanarak bir dizi tanımlayabilirsiniz anlamına gelir `ContinueWith` veya `await`. `bool` Tamamlama dönüş değeri: `false` animasyon kesintisiz tamamlandıysa veya `true` tarafından iptal edildi durumunda [ `CancelAnimation` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) tarafından başlatılan tüm animasyonları iptal yöntemi başka bir yöntem `ViewExtensions` aynı öğede ayarlayın.

### <a name="composite-animations"></a>Bileşik animasyonları

Bileşik animasyon oluşturmak için beklenen ve bekleniyor animasyonları karıştırabilirsiniz. İçinde animasyon bunlar `ViewExtensions` hedefleyen `TranslatonX`, `TranslationY`, ve `Scale` dönüştürme özellikleri:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

Dikkat `TranslateTo` hem olası olarak etkilediğini `TranslationX` ve `TranslationY` özellikleri.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll ve Task.WhenAny

Eş zamanlı animasyon kullanarak yönetmek mümkündür [ `Task.WhenAll` ](xref:System.Threading.Tasks.Task.WhenAll*), birden çok görev tüm yürüttükten sonra bildirir ve [ `Task.WhenAny` ](xref:System.Threading.Tasks.Task.WhenAny*), hangi sinyalleri ne zaman ilk birkaç görevleri sonuçlandırdı.

### <a name="rotation-and-anchors"></a>Döndürme ve yer işaretleri

Çağrılırken `ScaleTo`, `RelScaleTo`, `RotateTo`, ve `RelRotateTo` yöntemleri ayarlayabilirsiniz [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) ve [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) göstermek için özellikleri ölçeklendirme ve döndürme Merkezi.

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) uzayda tarafından bu tekniği gösterir bir `Button` sayfanın kaydırır.

### <a name="easing-functions"></a>Kolaylaştırıcı İşlevler

Genellikle animasyonları için bir bitiş değeri başlangıç değerinden doğrusal olarak. Kolaylaştırıcı işlevler animasyonları hızını artırın ya da kendi kursunda yavaşlamasına neden olabilir. Animasyon yöntemlerine son isteğe bağlı bağımsız değişken türünde [ `Easing` ](xref:Xamarin.Forms.Easing), türünde 11 statik salt okunur alanlar tanımlayan bir sınıf `Easing`:

- [`Linear`](xref:Xamarin.Forms.Easing.Linear), varsayılan
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn), [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut), ve [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn), [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut), ve [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) ve [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) ve [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

`In` Soneki etkisi, animasyonun başlangıcında olduğunu gösterir `Out` sonunda anlamına gelir ve `InOut` animasyon başında ve sonunda olduğu anlamına gelir.

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) örnek kolaylaştırıcı işlevler kullanımını gösterir.

### <a name="your-own-easing-functions"></a>Kendi kolaylaştırıcı İşlevler

Ayrıca, kendi kolaylaştırıcı işlevler geçirerek tanımlayabilirsiniz bir [ `Func<double, double>` ](xref:System.Func`2) için [ `Easing` Oluşturucusu](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double})). `Easing` aynı zamanda örtük bir dönüştürme tanımlar `Func<double, double>` için `Easing`. Animasyonun başından sonuna doğrusal olarak geçer hızlandırma işlevini bağımsız değişkeni her zaman 0 ile 1 aralığında aynıdır. İşlev *genellikle* 0 ile 1 aralığında bir değer döndürür, ancak kısa bir süreliğine negatif veya 1'den büyük olabilir (olduğu gibi `SpringIn` ve `SpringOut` işlevler) veya ne yaptığınızı biliyorsanız kuralları uğratabilir.

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) örnek bir özel hızlandırma işlevini gösterir ve [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) başka gösterir.

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) örnek ayrıca gösterir bir özel hızlandırma işlevini ve bir yöntemi değiştirme `AnchorX` ve `AnchorY` bir dizisi döndürme animasyon içindeki özellikleri.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığın bir [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) özel hızlandırma kullanan sınıf işlev bir düğme tıklandığında jiggle için. [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) örnek gösterir.

### <a name="entrance-animations"></a>Giriş animasyonları

Bir sayfa ilk görüntülendiğinde animasyon popüler bir tür gerçekleşir. Böyle bir animasyon başlatılmadan [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) sayfanın geçersiz kılar. İçin en iyi şekilde görünmesi için sayfayı istediğiniz XAML ayarlamak için bu animasyonları *sonra* animasyonu başlatmak ve kod düzenden animasyon ekleme.

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) kullanan örnek [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) sayfanın içeriğini soluklaştırılacak genişletme yöntemi.

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) kullanan örnek [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) taraflarından sayfanın içeriğini slayt için genişletme yöntemi.

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) kullanan örnek [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) canlandırmak için genişletme yöntemi `RotationY` özelliği. A [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) yöntemdir de kullanılabilir.

### <a name="forever-animations"></a>Sonsuza kadar animasyonları

Program sonlanana kadar diğer uçta animasyonları "sonsuz" çalıştırın. Bunlar, genellikle tanıtım amacıyla tasarlanmıştır.

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) kullanan örnek [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) iki parça metin giriş ve çıkış soluklaştırılacak animasyon.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) bir görüntüler ve tüm baş aşağı bulunmaları ardından sırayla tek tek harf 180 derece döndürür. Tüm dizeyi orijinal dizeyi aynı okunacak 180 derece çevrilmiş sonra.

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) basit bir örnek döndürür `BoxView` Helikopter ekranın ortasında uzayda oluştu.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) döner `BoxView` Tekerlek Parmağı ekranın ortasında ve sonra her bir uçtaki ilginç kalıpları oluşturmak için kendisini döndürür:

[![Üç ekran döndürme Tekerlek Parmağı görüntüsü](images/ch22fg21-small.png "döndürme Tekerlek Parmağı")](images/ch22fg21-large.png#lightbox "Tekerlek Parmağı döndürme")

Ancak, aşamalı artan `Rotation` bir öğenin özelliği çalışmayabilir uzun vadede olarak [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) örnek gösterir.

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) kullanan örnek [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), ve [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) bir bit eşlem 3B alanda döndürme gibi görünüyor sağlamak için.

### <a name="animating-the-bounds-property"></a>Sınırları özelliğe animasyon ekleme

Yalnızca genişletme yönteminin `ViewExtensions` henüz gösterilen olan [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), hangi etkili bir şekilde canlandırın salt okunur [ `Bounds` ](xref:Xamarin.Forms.VisualElement.Bounds) çağırarak [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) yöntemi. Bu yöntem genellikle çağıran `Layout` bölümünde açıklandığı gibi türevleri [ **bölüm 26. CustomLayouts**](chapter26.md).

`LayoutTo` Yöntemi özel amaçlar için sınırlı olmalıdır. [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) programın sıkıştırmak ve açmak için kullandığı bir `BoxView` sayfanın tarafına, geri dönmeler gibi.

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) kullanan örnek `LayoutTo` taşımak için bir Klasik uygulamasında numaralı kutucukları yerine karıştırılmış bir resim görüntüler 15-16 Bulmacanın kutucukları:

[![Üç ekran görüntüsü Xamarin Xuzzle](images/ch22fg26-small.png "Xuzzle Game Bulmaca")](images/ch22fg26-large.png#lightbox "Xuzzle Bulmaca oyunu")

### <a name="your-own-awaitable-animations"></a>Beklenebilir animasyonlarınızı

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) örnek beklenebilir animasyon oluşturur. Döndürebilen önemli sınıfı bir `Task` yöntemi ve animasyon tamamlandığında sinyal nesnedir [ `TaskCompletionSource` ](xref:System.Threading.Tasks.TaskCompletionSource`1).

## <a name="deeper-into-animations"></a>Ayrıntılı incelemesi animasyonları

Xamarin.Forms animasyonu sistem biraz kafa karıştırıcı olabilir. Ek olarak `Easing` sınıfı, animasyon sistemi oluşur `ViewExtensions`, `Animation`, ve `AnimationExtension` classses.

### <a name="viewextensions-class"></a>ViewExtensions sınıfı

Önceden gördüğünüz [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions). Döndüren yöntemler dokuz tanımlar `Task<bool>` ve [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)). Yedi dokuz yöntemleri hedefin özelliklerini dönüştürün. Diğer iki olan [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), hangi hedeflerin [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) özelliği ve [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), çağıran [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) yöntemi.

### <a name="animation-class"></a>Animasyon sınıfı

[ `Animation` ](xref:Xamarin.Forms.AnimationExtensions) Sınıfında bir [Oluşturucusu](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action)) geri çağırma ve tamamlanmış yöntemleri ve animasyon parametrelerini tanımlamak için beş bağımsız değişken.

Alt animasyonları ile eklenebilir [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)), [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)), [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)), ve ve aşırı yükünü [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)).

Animasyon nesne bir çağrıyla başlatılır [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) yöntemi.

### <a name="animationextensions-class"></a>AnimationExtensions sınıfı

[ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) Sınıfı çoğunlukla genişletme yöntemleri içerir. Çeşitli sürümleri bir `Animate` yöntemi ve genel [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) yöntemidir, gerçekten ihtiyacınız yalnızca animasyon işlev, bu nedenle çok yönlü.

## <a name="working-with-the-animation-class"></a>Animasyon sınıfı ile çalışma

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) örnek gösterir [ `Animation` ](xref:Xamarin.Forms.Animation) birkaç farklı animasyonlarla sınıfı.

### <a name="child-animations"></a>Alt animasyonları

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) örnek ayrıca gösterir yapan animasyonları kullanın (çok benzer) alt [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) ve [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)) yöntemleri.

### <a name="beyond-the-high-level-animation-methods"></a>Üst düzey animasyon yöntemlerin

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) örnek ayrıca gösterir özellikler hedeflenen tarafından ötesine geçip animasyonları gerçekleştirme `ViewExtensions` yöntemleri. Bir örnekte, bir dizi nokta almak uzun; başka bir örnekte, bir `BackgroundColor` özelliği bir animasyon görünür.

### <a name="more-of-your-own-awaitable-methods"></a>Beklenebilir yöntemlerinizin daha fazla bilgi

[ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Yöntemi `ViewExtensions` ile işe yaramazsa [ `Easing.SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) işlevi. Yukarıdaki 1 kolaylaştırıcı çıktısını alır, durdurur.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı içeren bir [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) sınıfıyla [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) ve [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) Bu sorun yoksa genişletme yöntemleri ile birlikte [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) ve [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) bu iptal için yöntemleri animasyonları.

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) gösterir `TranslateXTo` yöntemi.

`MoreExtensions` Sınıfı da içeren bir [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) X ve Y çeviri birleştiren bir genişletme yöntemi ve bir [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) yöntemi.

### <a name="implementing-a-bezier-animation"></a>Bezier animasyon uygulama

Bezier eğrisi yol boyunca bir öğe taşıyan animasyon geliştirmek mümkündür. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı içeren bir [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) kapsülleyen bir Bezier eğrisi yapısı ve [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) denetim yönlendirmesi için sabit listesi.

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) Sınıfı içeren bir [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) genişletme yöntemi ve bir [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) yöntemi.

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) örnek, bir öğe Beizer yol animasyonu gösterir.

## <a name="working-with-animationextensions"></a>AnimationExtensions ile çalışma

Bir animasyon standart koleksiyondan eksik bir renk animasyon türüdür. İki arasında enterpolasyon için doğru bir yolu yoktur sorunudur `Color` değerleri. RGB değerleri tek tek enterpolasyon mümkündür, ancak yalnızca geçerli olarak ilişkilendirme HSL değerleri.

Bu nedenle, [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı içeren iki `Color` animasyon yöntemleri: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)ve [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Ayrıca iki iptal yöntem vardır: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) ve [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Her iki yöntem de olun kullanım [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), kapsamlı genel çağırarak animasyon gerçekleştiren [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) yönteminde [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions).

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) örnek, renk animasyonları bu iki tür kullanım gösterir.

## <a name="structuring-your-animations"></a>Animasyonlarınızı yapılandırma

Bazen, XAML içinde animasyon express ve bunları MVVM ile birlikte kullanmak kullanışlıdır. Bu sonraki bölümde ele alınmıştır [ **Bölüm 23. Tetikleyiciler ve davranışlar**](chapter23.md).



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 22 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Bölüm 22 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animasyon](~/xamarin-forms/user-interface/animation/index.md)
