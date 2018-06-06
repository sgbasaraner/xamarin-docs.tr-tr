---
title: Xamarin.iOS çekirdek animasyonda
description: Bu makalede nasıl doğrudan alt düzey animasyon denetimi kullanmak için yüksek performanslı, Uıkit, akıcı animasyonları yanı sıra sağladığını gösteren çekirdek animasyon çerçevesi inceler.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5cc6019ed148b870e38659eb30ac7f2738481a50
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786826"
---
# <a name="core-animation-in-xamarinios"></a>Xamarin.iOS çekirdek animasyonda

_Bu makalede nasıl doğrudan alt düzey animasyon denetimi kullanmak için yüksek performanslı, Uıkit, akıcı animasyonları yanı sıra sağladığını gösteren çekirdek animasyon çerçevesi inceler._

iOS içeren [ *çekirdek animasyon* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) uygulamanızda görünümler için animasyon desteği sağlamak için.
Tüm tabloları kaydırma ve farklı görünümleri arasında geçirmeyi gibi iOS son derece kesintisiz animasyonları yanı sıra bunların çekirdek animasyonu dahili dayandığından yaparlar gerçekleştirin.

Çekirdek animasyon ve çekirdek grafikleri çerçeveleri güzel, oluşturmak için birlikte çalışabilir animasyonlu 2B grafik. Aslında çekirdek animasyon inanılmaz, sinematik deneyimlerini oluşturmada 2B grafik 3B uzaydaki bile da dönüştürebilirsiniz. Bu makalenin kapsamı dışındadır 3B olmasına rağmen ancak gerçek 3B grafik oluşturmak için bir şey OpenGL ES gibi ya da oyunlar Aç MonoGame gibi bir API için kullanmanız gerekir.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Çekirdek animasyon

iOS çekirdek animasyon çerçeve görünümleri arasında geçiş, menüler kayan ve birkaçıdır etkileri kaydırma gibi animasyon efektlerini oluşturmak için kullanılır. Animasyon ile çalışmak için iki yolu vardır:

- [Uıkit aracılığıyla](#Using_UIKit_Animation), görünüm tabanlı animasyonları ve bunun yanı sıra denetleyicileri arasında animasyonlu geçişler içerir.
- [Çekirdek animasyon aracılığıyla](#Using_Core_Animation), doğrudan geçirmiş denetimi için izin verme katmanları.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>Uıkit animasyon kullanma

Uıkit animasyon bir uygulama eklemek kolaylaştıran çeşitli özellikler sunar. Çekirdek animasyon dahili kullansa da, yalnızca görünümleri ve denetleyicilerini çalışması için onu hemen soyutlar.

Bu bölümde dahil olmak üzere Uıkit animasyon özelliklerini ele alınmıştır:

-  Denetleyicileri arasındaki geçişleri
-  Görünümler arasında geçişler
-  Görünüm özelliği animasyon


### <a name="view-controller-transitions"></a>Görünüm denetleyicisini geçişleri

 `UIViewController` Görünüm denetleyicileri üzerinden arasında geçiş için yerleşik destek sağlar `PresentViewController` yöntemi. Kullanırken `PresentViewController`, ikinci denetleyici geçişi isteğe bağlı olarak animasyon uygulanabilir.

Örneğin, burada bir düğme ilk denetleyicideki temas çağırır iki denetleyicileri uygulamayla göz önünde bulundurun `PresentViewController` ikinci bir denetleyici görüntülemek için. Hangi geçiş animasyon ikinci denetleyici göstermek için kullanılan denetlemek için yalnızca ayarlamak kendi [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) aşağıda gösterildiği gibi özelliği:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

Bu durumda bir `PartialCurl` animasyon kullanılır, birkaç diğerleri de dahil olmak üzere, kullanılabilir, ancak:

-  `CoverVertical` – Slayt yukarı ekranın en altındaki
-  `CrossDissolve` – Eski görünümü yavaşça & Yeni Görünüm belirerek girer
-  `FlipHorizontal` -Sağdan sola Yatay Çevir. İşten çıkarma üzerinde soldan sağa geçiş çevirir.


Geçiş animasyon geçmesini `true` ikinci bağımsız değişkeni olarak `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

Geçişin nasıl göründüğünü aşağıdaki ekran gösterilir `PartialCurl` durumu:

 ![](core-animation-images/06-view-transitions.png "Bu ekran PartialCurl geçiş gösterir")

### <a name="view-transitions"></a>Görünüm geçişleri

Denetleyiciler arasındaki geçişleri yanı sıra Uıkit hareketlendirme geçişler için başka bir görünüm takas görünümler arasında da destekler.

Örneğin, bir denetleyicisiyle yaşadığınız deyin `UIImageView`, resimdeki dokunma ikinci bir yere görüntülenmelidir `UIImageView`. Görüntü animasyon için görünümün değerlendirmesi ikinci görüntü görünümüne geçiş çağırmak kadar kolaydır `UIView.Transition`, onu `toView` ve `fromView` aşağıda gösterildiği gibi:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` Ayrıca alır bir `duration` ne kadar süreyle animasyon çalışır, denetimleri parametre yanı [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) ve hareket hızı işlevi animasyon gibi şeyler belirtmek için. Buna ek olarak, animasyon tamamlandığında çağrılacak bir tamamlanma işleyicisini belirtebilirsiniz.

Ekran görüntüsünün altında göster görüntü arasında animasyonlu geçiş ne zaman görünümleri `TransitionFlipFromTop` kullanılır:

 ![](core-animation-images/07-animated-transition.png "Bu ekran TransitionFlipFromTop kullanıldığında görüntü görünümler arasında animasyonlu geçiş gösterir")

### <a name="view-property-animations"></a>Görünüm özellik animasyonları

Uıkit destekleyen üzerinde çeşitli özellikler animasyon `UIView` ücretsiz olarak dahil olmak üzere sınıfı:

-  Çerçeve
-  Sınırları
-  Merkez
-  Alfa
-  Dönüştürme
-  Renk


Özellik değişiklikleri belirterek animasyonlarına örtük olarak gerçekleşecek bir `NSAction` temsilci statik olarak geçirilen `UIView.Animate` yöntemi. Örneğin, aşağıdaki kodu orta noktası canlandırır bir `UIImageView`:

```csharp
pt = imgView.Center;

UIView.Animate (
    duration: 2, 
    delay: 0, 
    options: UIViewAnimationOptions.CurveEaseInOut | 
        UIViewAnimationOptions.Autoreverse,
    animation: () => {
        imgView.Center = new CGPoint (View.Bounds.GetMaxX () 
            - imgView.Frame.Width / 2, pt.Y);},
    completion: () => {
        imgView.Center = pt; }
);
```

Bu bir görüntünün geri ve İleri ekran üstte ortasının içinde aşağıda gösterildiği gibi olur:

 ![](core-animation-images/08-animate-center.png "Bir görüntü ve geriye ekranın üst kısmında çıktısı olarak animasyon ekleme")

İle `Transition` yöntemi, `Animate` birlikte hareket hızı işlevi ayarlamak süre sağlar. Bu örnek ayrıca kullanılan `UIViewAnimationOptions.Autoreverse` değerinden ilk birini animasyon animasyonun seçeneği. Ancak, bu kod ayrıca ayarlar `Center` ilk değerini tamamlama işleyicisinde dön. Animasyonun özellik değerlerini zamanla enterpolasyonla olsa da, gerçek model özelliğinin her zaman ayarlanmış son değer değeridir. Bu örnekte, değerin bir sağ tarafında yakın değerlendirmesi noktasıdır. Ayarlamadan `Center` başlangıç noktasına olduğu yere nedeniyle animasyon tamamlandıktan `Autoreverse` animasyon tamamlandıktan sonra ayarlanan, görüntünün geri sağ tarafında aşağıda gösterildiği gibi ek:

 ![](core-animation-images/09-animation-complete.png "Animasyon tamamlandıktan sonra ilk noktasına merkezi ayarlamadan görüntünün geri sağ tarafında Daya")

## <a name="using-core-animation"></a>Çekirdek animasyon kullanma

 `UIView` animasyon yeteneğinin çok izin ve uygulama kolaylığı nedeniyle mümkünse kullanılmalıdır. Daha önce belirtildiği gibi UIView animasyonları çekirdek animasyon çerçevesi kullanın. Ancak, bazı şeyleri ile yapılamaz `UIView` bir görünümle animasyonlu ek özellikler animasyonu veya doğrusal olmayan bir yol boyunca enterpolasyonla gibi animasyonları. Daha hassas denetim ihtiyaç duyacağınız bu gibi durumlarda çekirdek animasyon doğrudan de kullanılabilir.

### <a name="layers"></a>Katmanları

Çekirdek animasyon ile çalışırken, animasyon aracılığıyla gerçekleşir *katmanları*, türü olan `CALayer`. Yoktur, bir katman hiyerarşisi çok hiyerarşisini görüntüleme gibi bir katman bir görünüme kavramsal olarak benzerdir. Aslında, görünümler, kullanıcı etkileşimi için destek ekleyen görünümüyle Katmanlar yedekleyin. Görünümün aracılığıyla herhangi bir görünüm katmanı erişebilirsiniz `Layer` özelliği. Bağlam aslında, kullanılan `Draw` yöntemi `UIView` katmandan gerçekte oluşturulur. Dahili olarak, yedekleme katman bir `UIView` sahip ne çağırır olan görünüme kendisi, kendi temsilci `Draw`. İçin çizim sırasında bunu bir `UIView`, aslında kendi katmana çizim.

Katman animasyonları örtük veya açık olabilir. Örtük animasyonları tanımlayıcı. Yalnızca katman özelliklerini değiştirmeniz gerekir bildirme ve animasyon yalnızca çalışır. Açık bir animasyon, diğer yandan bir katmana eklenen bir animasyon sınıfı aracılığıyla oluşturulur. Açık bir animasyon animasyonun nasıl oluşturulduğunu üzerinde ek denetim sağlar. Örtük ve açık animasyonları daha derinlemesine içine aşağıdaki bölümleri inceleyin.

### <a name="implicit-animations"></a>Örtük animasyonları

Bir katman özelliklerini animasyon bir örtük animasyonun yoludur. `UIView` animasyon örtük animasyonları oluşturun. Ancak, bir katman de karşı doğrudan örtük animasyonları oluşturabilirsiniz.

Örneğin, aşağıdaki kod bir katmanın ayarlar `Contents` bir görüntüden kenarlık genişliğini ve rengini ayarlar ve bir görünümün katman alt katman katmanı ekler:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    layer = new CALayer ();
    layer.Bounds = new CGRect (0, 0, 50, 50);
    layer.Position = new CGPoint (50, 50);
    layer.Contents = UIImage.FromFile ("monkey2.png").CGImage;
    layer.ContentsGravity = CALayer.GravityResize;
    layer.BorderWidth = 1.5f;
    layer.BorderColor = UIColor.Green.CGColor;

    View.Layer.AddSublayer (layer);
}
```

Katman için örtük bir animasyon eklemek için yalnızca özelliği değişiklikleri sarmalamak bir `CATransaction`. Bu gibi bir görünüm animasyon ile canlandırılabilir olmayacaktır özellikleri sağlar `BorderWidth` ve `BorderColor` aşağıda gösterildiği gibi:

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    CATransaction.Begin ();
    CATransaction.AnimationDuration = 10;
    layer.Position = new CGPoint (50, 400);
    layer.BorderWidth = 5.0f;
    layer.BorderColor = UIColor.Red.CGColor;
    CATransaction.Commit ();
}
```

Bu kod ayrıca katmanın canlandırır `Position`, superlayer's koordinatları üst soldan ölçülen katmanın bağlantı noktasının konumunu olduğu. Bir katman bağlantı noktasını katmanın koordinat sistemi içinde normalleştirilmiş bir noktasıdır.

Aşağıdaki şekilde, konum ve bağlantı noktası gösterilmektedir:

 ![](core-animation-images/10-postion-anchorpt.png "Bu şekil, konum ve bağlantı noktası gösterir.")

Örnek çalıştırdığınızda `Position`, `BorderWidth` ve `BorderColor` aşağıdaki ekran görüntülerinde gösterildiği gibi animasyon ekleme:

 ![](core-animation-images/11-implicit-animation.png "Örnek çalıştırdığınızda, konum, BorderWidth ve BorderColor gösterildiği gibi animasyon ekleme")

### <a name="explicit-animations"></a>Açık bir animasyon

Örtük animasyonları ek olarak, çekirdek animasyon çeşitli devralınmalıdır sınıfları içerir. `CAAnimation` imkan sağlayan daha sonra bir katmana açıkça eklenen animasyonları yalıtma. Bunlar, bir animasyon başlangıç değerini değiştirerek, animasyonları gruplandırma ve ana kare doğrusal olmayan yollara izin verecek şekilde belirtme gibi animasyonları geçirmiş denetime izin verir.

Aşağıdaki kod kullanarak bir açık animasyon örneği gösterir bir `CAKeyframeAnimation` daha önce (örtük animasyon bölümünde) gösterilen katman için:

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);
    
    // get the initial value to start the animation from
    CGPoint fromPt = layer.Position;
    
    /* set the position to coincide with the final animation value
    to prevent it from snapping back to the starting position
    after the animation completes*/
    layer.Position = new CGPoint (200, 300);
    
    // create a path for the animation to follow
    CGPath path = new CGPath ();
    path.AddLines (new CGPoint[] { fromPt, new CGPoint (50, 300), new CGPoint (200, 50), new CGPoint (200, 300) });
    
    // create a keyframe animation for the position using the path
    CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
    animPosition.Path = path;
    animPosition.Duration = 2;
    
    // add the animation to the layer.
    /* the "position" key is used to overwrite the implicit animation created
    when the layer positino is set above*/
    layer.AddAnimation (animPosition, "position");
}
```

Bu kod değişiklikleri `Position` sonra ana kare animasyonunun tanımlamak için kullanılan bir yol oluşturarak katmanın. Dikkat katmanın `Position` son değerine ayarlanmış `Position` animasyon gelen. Bu, katman aniden için döndürecekti kendi `Position` animasyon önce animasyonun sunu ve gerçek model değerleri yalnızca değiştiğinden. Animasyon model değeri son değerine ayarlayarak katman kalmak animasyonun sonunda yerinde.

Aşağıdaki ekran görüntüleri ile belirtilen yol görüntünün ortasının içeren katmanını göster:

 ![](core-animation-images/12-explicit-animation.png "Bu ekran görüntüsünü belirtilen yol animasyon içeren katmanı gösterir")
 
## <a name="summary"></a>Özet

Bu makalede aracılığıyla sağlanan animasyon özellikleri inceledik *çekirdek animasyon* çerçeveleri. Çekirdek animasyon, hem nasıl Uıkit animasyonları destekler ve doğrudan alt düzey animasyon denetimi için nasıl kullanılabileceğini gösteren bileşen başvuru incelenir.

## <a name="related-links"></a>İlgili bağlantılar

- [Çekirdek animasyon örneği](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Temel Grafikler](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Grafikler ve animasyon gözden geçirme](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Temel Animasyon](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
