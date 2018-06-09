---
title: Bölüm 22 özeti. Animasyon
description: 'Xamarin.Forms ile mobil uygulamaları oluşturma: Bölüm 22 özeti. Animasyon'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b25fed9a86b82e56cb3b2bf5e3276c8ff63f4e35
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241975"
---
# <a name="summary-of-chapter-22-animation"></a>Bölüm 22 özeti. Animasyon

Xamarin.Forms Zamanlayıcı kullanarak kendi animasyonları oluşturabileceğiniz gördüğünüz veya `Task.Delay`, ancak Xamarin.Forms tarafından sağlanan animasyon özellikleri kullanarak genellikle daha kolay olur. Üç sınıfları animasyonlarına uygulayın:

- [`ViewExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/), üst düzey yaklaşımı
- [`Animation`](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/), daha zor ancak daha çok yönlü
- [`AnimationExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/), en çok yönlü, düşük düzeyli yaklaşımı

Genellikle, animasyonları bağlanabilir özellikleri tarafından desteklenen özellikler hedefleyin. Bu zorunlu değildir, ancak dinamik olarak yapılan değişiklikler tepki yalnızca özellikleri şunlardır.

Animasyonlarına için XAML arabirimi yoktur, ancak içinde açıklanan teknikleri kullanarak XAML içine animasyonları tümleştirebilirsiniz [ **Bölüm 23. Tetikleyiciler ve davranışları**](chapter23.md).

## <a name="exploring-basic-animations"></a>Temel animasyonları keşfetme

Genişletme yöntemleri bulunan temel animasyon işlevlerdir [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) sınıfı. Türetilen herhangi bir nesne bu yöntemleri uygulamak `VisualElement`. En basit animasyonları özellikler ele alınmıştır dönüşümler hedef [ `Chapter 21. Transforms` ](chapter21.md).

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) gösteren nasıl `Clicked` olay işleyicisi için bir `Button` çağırabilirsiniz [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) dönmesi için genişletme yöntemi bir daire içinde düğmesi.

`RotateTo` Yöntemi değişiklikleri `Rotation` özelliği `Button` 0-360 (varsayılan) bir üç aylık dönem ikinci seyri içinde. Varsa `Button` dokunduğunuz yeniden, ancak hiçbir şey yapmayacak çünkü `Rotation` özelliği 360 derece bulunmakta.

### <a name="setting-the-animation-duration"></a>Animasyon süresi ayarlama

İkinci bağımsız değişkeni `RotateTo` milisaniye cinsinden süre. Büyük bir değere ayarlayın dokunarak `Button` sırasında bir animasyon geçerli açı yeni bir animasyon başında başlar.

### <a name="relative-animations"></a>Göreli animasyonları

`RelRotateTo` Yöntemi mevcut değeri için belirtilen değer ekleyerek göreli bir döndürme gerçekleştirir. Bu yöntem tanır `Button` birden çok kez ve her dokunduğunuz için zaman artar `Rotation` 360 derece özelliği.

### <a name="awaiting-animations"></a>Animasyon bekleniyor

Animasyon yöntemleri `ViewExtensions` dönmek `Task<bool>` nesneleri. Bu, bir dizi kullanarak sıralı animasyonları tanımlayabilirsiniz anlamına gelir `ContinueWith` veya `await`. `bool` Tamamlama dönüş değeri: `false` animasyon kesinti olmadan tamamladığınızda veya `true` tarafından iptal edildi, [ `CancelAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) tarafından başlatılan tüm animasyonları iptal yöntemi başka bir yöntem `ViewExtensions` aynı öğede ayarlayın.

### <a name="composite-animations"></a>Bileşik animasyonları

Bileşik animasyon oluşturmak için beklemenin ve beklemenin olmayan animasyonları karıştırabilirsiniz. Animasyonları bunlar `ViewExtensions` hedefleyen `TranslatonX`, `TranslationY`, ve `Scale` dönüştürme özellikleri:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`ScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.ScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)

Dikkat `TranslateTo` her ikisi de olası olarak etkilediğini `TranslationX` ve `TranslationY` özellikleri.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll ve Task.WhenAny

Eş zamanlı animasyonları kullanarak yönetmek mümkündür [ `Task.WhenAll` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAll%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), birden çok görev tüm yürüttükten zaman sinyalleri ve [ `Task.WhenAny` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAny%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), hangi sinyalleri ne zaman ilk birkaç Görevler sonuçlarına göre.

### <a name="rotation-and-anchors"></a>Döndürme ve tutturucular

Çağrılırken `ScaleTo`, `RelScaleTo`, `RotateTo`, ve `RelRotateTo` ayarlayabileceğiniz yöntemleri [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) ve [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) göstermek için özellikleri Merkezi ölçekleme ve döndürme.

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) uzayda tarafından bu tekniği gösteren bir `Button` sayfası Merkez etrafında.

### <a name="easing-functions"></a>İşlevler kolaylaştırma

Genellikle animasyonları bir bitiş değeri başlangıç değerinden doğrusal. İşlevler kolaylaştırma hızlandırma veya kendi süresince yavaşlaması animasyonları neden olabilir. Animasyon yöntemleri son isteğe bağlı bağımsız değişkeni türünde [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/), 11 statik salt okunur türü alanları tanımlayan bir sınıf `Easing`:

- [`Linear`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/), varsayılan
- [`SinIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/), [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/), ve [`SinInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/)
- [`CubicIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/), [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/), ve [`CubicInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/)
- [`BounceIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) Ve [`BounceOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/)
- [`SpringIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) Ve [`SpringOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)

`In` Soneki etkisi animasyon başlayarak olduğunu gösterir `Out` sonunda anlamına gelir ve `InOut` animasyon başında ve sonunda olduğu anlamına gelir.

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) örnek işlevleri kolaylaştırma kullanımını gösterir.

### <a name="your-own-easing-functions"></a>Hareket hızı işlevlerinizi

Ayrıca, kendi hareket hızı işlevleri aktararak tanımlayabilirsiniz bir [ `Func<double, double>` ](https://developer.xamarin.com/api/type/System.Func%3CT,TResult%3E/) için [ `Easing` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Easing.Easing/p/System.Func%7BSystem.Double,System.Double%7D/). `Easing` Ayrıca örtük bir dönüştürme tanımlar `Func<double, double>` için `Easing`. Animasyonun başından sonuna doğrusal olarak devam eder hareket hızı işlevine bağımsız değişken her zaman 0 ve 1 aralığında aynıdır. İşlev *genellikle* 0 ve 1 aralığında bir değer döndürür, ancak kısaca negatif veya 1'den büyük olabilir (ile olduğu gibi `SpringIn` ve `SpringOut` işlevleri) veya gerçekleştirmekte olduğunuz biliyorsanız kuralları bozar.

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) örneği, özel bir hareket hızı işlev gösterir ve [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) başka gösterir.

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) örnek ayrıca gösteren özel bir hareket hızı işlev ve değiştirmenin bir teknik `AnchorX` ve `AnchorY` özellikleri döndürme animasyon dizisi içinde.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplık sahip bir [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) özel kolaylaştırma kullanan sınıfı işlev bir düğme tıklatıldığında jiggle için. [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) örnek gösterilmektedir.

### <a name="entrance-animations"></a>Giriş animasyonları

Bir sayfa ilk görüntülendiğinde animasyon bir popüler tür oluşur. Bu tür bir animasyon başlatılabilir [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) sayfasında geçersiz kılar. XAML sayfası istediğiniz görünmesi ayarlamak için en iyi animasyonlarına için *sonra* animasyon ve ardından başlatmak ve kod düzeninden animasyon ekleme.

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) örnek kullanır [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) sayfanın içeriğini silinerek için genişletme yöntemi.

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) örnek kullanır [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) genişletme yöntemi taraftan sayfanın içeriğini kaydırın.

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) örnek kullanır [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) animasyon için genişletme yöntemi `RotationY` özelliği. A [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) yöntemdir de kullanılabilir.

### <a name="forever-animations"></a>Devamlı animasyonları

Program sonlanana kadar diğer uçta animasyonları "sonsuza kadar" çalıştırın. Bunlar genellikle tanıtım amacıyla tasarlanmıştır.

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) örnek kullanır [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) iki parça metin belirerek ve silinerek animasyon.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) bir görüntüler ve tüm görüntülemediğini bulunmaları sonra sırayla tek tek harfler 180 derece döndürür. Ardından tüm dizeyi 180 derece özgün dizeye aynı okumak için çevrilmiş.

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) örnek döndüğü basit bir `BoxView` Helikopter ekran merkezi etrafında uzayda oluştu.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) döner `BoxView` Tekerlek Parmağı ekran merkezi etrafında ve her spoke ilginç kalıpları oluşturmak için kendisini döndürür:

[![Üçlü ekran döndürme Tekerlek Parmağı görüntüsü](images/ch22fg21-small.png "döndürme Tekerlek Parmağı")](images/ch22fg21-large.png#lightbox "döndürme Tekerlek Parmağı")

Ancak, aşamalı olarak artırmak `Rotation` özelliği, bir öğenin çalışmayabilir uzun vadede olarak [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) örnek gösterilmektedir.

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) örnek kullanır [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), ve [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) yapmak için ona bir bit eşlem 3B alanda döndürme gibi görünüyor.

### <a name="animating-the-bounds-property"></a>Sınırların özelliğe animasyon ekleme

Yalnızca genişletme yönteminde `ViewExtensions` henüz gösterilen olan [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), hangi etkili bir şekilde canlandırır salt okunur [ `Bounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) çağırarak özelliği [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) yöntemi. Bu yöntem normalde tarafından çağrılır `Layout` de anlatıldığı gibi türevleri [ **bölüm 26. CustomLayouts**](chapter26.md).

`LayoutTo` Yöntemi özel amaçlar için kısıtlanmış. [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) program sıkıştırmak ve genişletmek için bunu kullanan bir `BoxView` sayfanın tarafına geri dönmeler gibi.

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) örnek kullanır `LayoutTo` taşımak için bir Klasik uygulamasında numaralı döşeme yerine karıştırılmış görüntü görüntüler 15-16 Bulmacanın kutucuklar:

[![Üçlü ekran görüntüsü Xamarin Xuzzle](images/ch22fg26-small.png "Xuzzle Bulmaca oyun")](images/ch22fg26-large.png#lightbox "Xuzzle Bulmaca oyun")

### <a name="your-own-awaitable-animations"></a>Kendi bildirdiğimize animasyonları

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) örnek bildirdiğimize animasyon oluşturur. Döndürebilir önemli sınıfı bir `Task` nesne yöntemi ve animasyon tamamlandığında sinyal [ `TaskCompletionSource` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskCompletionSource%3CTResult%3E/).

## <a name="deeper-into-animations"></a>Daha derin animasyonları içine

Xamarin.Forms animasyon sistemi biraz kafa karıştırıcı olabilir. Ek olarak `Easing` sınıfı, animasyon sistemi oluşur `ViewExtensions`, `Animation`, ve `AnimationExtension` classses.

### <a name="viewextensions-class"></a>ViewExtensions sınıfı

Gördünüz [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/). Dönüş dokuz yöntemleri tanımlar `Task<bool>` ve [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/). Yedi dokuz yöntemleri hedef özellikleri dönüştürün. Diğer iki olan [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), hangi hedefleri [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) özelliği ve [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), çağıran [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) yöntemi.

### <a name="animation-class"></a>Animasyon sınıfı

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Sınıfına sahip bir [Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Animation.Animation/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Action/) geri çağırma ve tamamlanmış yöntemleri ve animasyonun parametrelerini tanımlamak için beş bağımsız değişkenlere sahip.

Alt animasyonları ile eklenebilir [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/), ve ve, aşırı [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Double/System.Double/).

Animasyon nesne çağrısıyla başlatılır [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BSystem.Double,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) yöntemi.

### <a name="animationextensions-class"></a>AnimationExtensions sınıfı

[ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Sınıfı, çoğunlukla genişletme yöntemleri içerir. Birçok sürümü vardır bir `Animate` yöntemi ve genel [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) yöntemidir, gerçekten size gereken yalnızca animasyon işlevi olduğunu kadar çok yönlü.

## <a name="working-with-the-animation-class"></a>Animasyon sınıfı ile çalışma

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) örnek gösterilmektedir [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) birkaç farklı animasyon sınıfıyla.

### <a name="child-animations"></a>Alt animasyonları

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) örnek ayrıca olun animasyonları kullanın (çok benzer şekilde) alt gösterir [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) ve [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/) yöntemleri.

### <a name="beyond-the-high-level-animation-methods"></a>Üst düzey animasyon yöntemleri

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) örnek ayrıca özellikleri basitleþtirmektir sunmamızı animasyonları gerçekleştirme gösterir `ViewExtensions` yöntemleri. Bir örnekte, bir dizi nokta almak uzun; başka bir örnekte, bir `BackgroundColor` özelliği animasyonlu.

### <a name="more-of-your-own-awaitable-methods"></a>Daha fazla bildirdiğimize yöntemlerinizi

[ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Yöntemi `ViewExtensions` ile işe yaramazsa [ `Easing.SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) işlevi. 1 hareket hızı çıktısını alır, durdurur.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığını içeren bir [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) ile sınıf [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) ve [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) Bu sorun yok genişletme yöntemleri yanı [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) ve [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) olanlar iptal yöntemleri animasyonları.

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) gösteren `TranslateXTo` yöntemi.

`MoreExtensions` Sınıfı da içeren bir [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) X ve Y çeviri birleştirir genişletme yöntemi ve [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) yöntemi.

### <a name="implementing-a-bezier-animation"></a>Bezier animasyon uygulama

Bir öğenin bir Bezier eğrisi yol boyunca taşır animasyonun geliştirmek mümkündür. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığını içeren bir [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) Bezier eğrisi yalıtır yapısı ve [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) denetim yönüne numaralandırması.

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) Sınıfı içeren bir [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) genişletme yöntemi ve [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) yöntemi.

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) örneği, bir Beizer yol boyunca bir öğe animasyonu gösterir.

## <a name="working-with-animationextensions"></a>AnimationExtensions ile çalışma

Bir animasyon standart koleksiyondan eksik bir renk animasyon türüdür. İki arasında kesinti için doğru yolu yoktur sorunudur `Color` değerleri. Tek tek RGB değerleri kesinti mümkündür, ancak yalnızca geçerli olarak enterpolasyonla HSL değerleri.

Bu nedenle, [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı içeren iki `Color` animasyon yöntemleri: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)ve [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Ayrıca iki iptal yöntem vardır: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) ve [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Her iki yöntem olun kullanımı [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), kapsamlı genel çağırarak animasyonun gerçekleştirir [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) yönteminde [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/).

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) örneği, renk animasyonları bu iki tür kullanarak gösterir.

## <a name="structuring-your-animations"></a>Animasyonları yapılandırma

Bazen, XAML animasyonları express ve bunları MVVM ile birlikte kullanmak yararlıdır. Bu sonraki bölümde ele alınmıştır [ **Bölüm 23. Tetikleyiciler ve davranışları**](chapter23.md).



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 22 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Bölüm 22 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animasyon](~/xamarin-forms/user-interface/animation/index.md)
