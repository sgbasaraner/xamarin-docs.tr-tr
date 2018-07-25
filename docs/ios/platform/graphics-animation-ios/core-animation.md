---
title: Xamarin.iOS temel animasyon
description: Bu makalede nasıl doğrudan alt düzey animasyon denetimi kullanmak için yüksek performanslı, yanı sıra, Uıkit içinde akıcı animasyon tanıdığını gösteren çekirdek animasyon çerçevesi inceler.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3d26e58822385c20f3c08d0b75ba468467c2c9b1
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242138"
---
# <a name="core-animation-in-xamarinios"></a>Xamarin.iOS temel animasyon

_Bu makalede nasıl doğrudan alt düzey animasyon denetimi kullanmak için yüksek performanslı, yanı sıra, Uıkit içinde akıcı animasyon tanıdığını gösteren çekirdek animasyon çerçevesi inceler._

iOS içerir [ *çekirdek animasyon* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) uygulamanızı görünümlerde animasyon desteği sağlamak için.
Tüm iOS gibi tablodan kaydırma ve farklı görünümleri arasında çekerek yedekliliğe kesintisiz animasyonlarda yanı sıra çekirdek animasyonu dahili olarak bağımlı olduğundan yaptıkları gerçekleştirin.

Temel animasyon ve temel grafikler çerçeveleri göz alıcı, oluşturmaya birlikte çalışabilir animasyonlu 2B grafikler. Aslında çekirdek animasyon harika, sinematik deneyimler oluşturma 3B alanda 2B grafikler bile da dönüştürebilirsiniz. 3B bu makalenin kapsamı dışında olsa da ancak gerçek 3B grafik oluşturmak için bir şey MonoGame gibi bir API için oyunlar etkinleştirin ya da OpenGL ES gibi kullanmanız gerekir.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Temel animasyon

iOS temel animasyon çerçeve görünümler arasında geçiş, menüler kayan ve dizileri etkileri kaydırma gibi animasyon efektleri oluşturmak için kullanılır. Animasyon ile çalışmak için iki yolu vardır:

- [Uıkit aracılığıyla](#Using_UIKit_Animation), görünüm tabanlı animasyonlar yanı sıra denetleyiciler arasındaki animasyonlu geçişlerini içerir.
- [Temel animasyon aracılığıyla](#Using_Core_Animation), doğrudan daha ayrıntılı denetim için izin verme katmanları.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>Uıkit animasyon kullanma

Uıkit animasyon bir uygulamaya eklemek kolaylaştıran birçok özellik sunar. Temel animasyon dahili olarak kullanılıyor olsa da, yalnızca görünümleri ve denetleyicileri ile çalışmak için onu dengelediği.

Bu bölümde dahil olmak üzere Uıkit animasyon özellikleri açıklanmaktadır:

-  Denetleyiciler arasındaki geçişleri
-  Görünümler arasında geçiş
-  Özellik animasyon görüntüle


### <a name="view-controller-transitions"></a>Görünüm geçişlerini görüntüleme

 `UIViewController` Görünüm denetleyicileri üzerinden arasında geçiş için yerleşik destek sağlar `PresentViewController` yöntemi. Kullanırken `PresentViewController`, ikinci denetleyici geçiş isteğe bağlı olarak animasyonu oluşturulabilen.

Örneğin, iki denetleyicileriyle burada ilk denetleyicisi de bir düğme temas çağıran bir uygulama düşünün `PresentViewController` ikinci bir denetleyici görüntülenecek. İkinci denetleyici göstermek için kullanılan hangi geçiş animasyon denetlemek için ayarlamanız yeterlidir, [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) aşağıda gösterildiği gibi özelliği:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

Bu durumda bir `PartialCurl` animasyon kullanılır, diğerleri birçok dahil olmak üzere, kullanılabilir, ancak:

-  `CoverVertical` – Slaytları ekranın alt
-  `CrossDissolve` – Eski görünüm yavaşça & Yeni Görünüm belirerek girer
-  `FlipHorizontal` -Sağdan sola Yatay Çevir. İşten çıkarma üzerinde soldan sağa geçiş çevirir.


Geçiş animasyon eklemek gibi geçirmek `true` ikinci bağımsız değişkeni olarak `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

Geçişi nasıl göründüğünü aşağıdaki ekran görüntüsünde gösterilmektedir `PartialCurl` çalışması:

 ![](core-animation-images/06-view-transitions.png "Bu ekran PartialCurl geçiş gösterir")

### <a name="view-transitions"></a>Görünüm geçişlerini

Denetleyiciler arasındaki geçişler yanı sıra Uıkit takas etmek için başka bir görünüm için görünüm arasında hareketlendirme geçişleri de destekler.

Denetleyicili deyin olduğu gibi `UIImageView`, ikinci bir görüntüye dokunma burada görüntülenmelidir `UIImageView`. Görüntü animasyon uygulamak için ikinci resim görünümüne geçiş değerlendirmesi görünümünün çağırmak kadar basittir `UIView.Transition`, iletmeden `toView` ve `fromView` aşağıda gösterildiği gibi:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` Ayrıca alır bir `duration` animasyonun ne kadar süre çalıştığında, denetimleri parametre olarak [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) animasyon gibi şeyleri ve Hızlandırma işlevini belirtmek için. Ayrıca animasyon tamamlandığında çağrılacak bir tamamlama işleyicisi da belirtebilirsiniz.

Görüntü arasındaki animasyonlu geçiş ne zaman show aşağıdaki ekran görüntüsünde görünümleri `TransitionFlipFromTop` kullanılır:

 ![](core-animation-images/07-animated-transition.png "Bu ekran animasyonlu TransitionFlipFromTop kullanıldığında görüntü görünüm arasında geçiş gösterir")

### <a name="view-property-animations"></a>Görünüm özellik animasyonları

Uıkit destekleyen çeşitli özellikler üzerinde animasyon `UIView` ücretsiz olarak dahil olmak üzere sınıfı:

-  Çerçeve
-  Sınırları
-  Merkez
-  alfa
-  Dönüştürme
-  Renk


Özellik değişiklikleri belirterek animasyonlarına örtük olarak gerçekleşecek bir `NSAction` temsilci statik olarak geçirilen `UIView.Animate` yöntemi. Örneğin, aşağıdaki kod merkez noktasını canlandırır bir `UIImageView`:

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

Bu bir görüntüyü geri ve İleri ekranın üst kısmında animasyon ekleme aşağıda gösterildiği gibi olur:

 ![](core-animation-images/08-animate-center.png "Bir görüntüyü geri ve İleri ekranın üst kısmında çıktısı olarak animasyon ekleme")

Olduğu gibi `Transition` yöntemi `Animate` birlikte hızlandırma işlevini ayarlamak süre sağlar. Bu örnek ayrıca kullanılan `UIViewAnimationOptions.Autoreverse` seçeneği, ilk bir değerden canlandırmak animasyon neden olur. Ancak, bu kod ayrıca ayarlar `Center` ilk değeriyle bir tamamlama işleyicisi dönün. Animasyonun özellik değerlerini zamanla ilişkilendirme, ancak gerçek model özelliğinin her zaman ayarlanan son değeri değerdir. Bu örnekte, bir noktasını sağ tarafına yakın değerlendirmesi bir değerdir. Ayarlamadan `Center` başlangıç noktasına olduğu yere nedeniyle animasyonun tamamlandığı `Autoreverse` animasyon tamamlandıktan sonra ayarlanan, görüntünün geri sağ aşağıda gösterildiği gibi ek bileşen:

 ![](core-animation-images/09-animation-complete.png "Animasyon tamamlandıktan sonra başlangıç noktasına merkezi ayarlamadan görüntü geri sağ Yasla")

## <a name="using-core-animation"></a>Temel animasyon kullanma

 `UIView` animasyonları çok sayıda özellik izin ve mümkünse kolay uygulama nedeniyle kullanılmalıdır. Daha önce bahsedildiği gibi Uıview animasyonları çekirdek animasyon çerçevesi kullanın. Ancak, bazı şeyler ile yapılamaz `UIView` animasyonları olan bir görünümü hareketlendirilemeyebilir ek özellikleri veya doğrusal olmayan bir yol boyunca ilişkilendirme gibi. Böyle durumlarda, daha hassas bir denetim gereken çekirdek animasyon doğrudan de kullanılabilir.

### <a name="layers"></a>Katmanları

Temel animasyon ile çalışırken, animasyon aracılığıyla gerçekleşir *katmanları*, türü olan `CALayer`. Yoktur, bir katman hiyerarşisi çok hiyerarşisini görüntüle gibi bir katman görünümüne kavramsal olarak benzer. Aslında, görünümler, kullanıcı etkileşimi için destek ekleme görünümü katmanları yedekleyin. Herhangi bir görünümü görünümün aracılığıyla katmanı erişebileceğiniz `Layer` özelliği. Bağlam, kullanılan `Draw` yöntemi `UIView` katmandan gerçekten oluşturulur. Dahili olarak, yedekleme katmanı bir `UIView` sahip ne çağrıları görünüm için kendisini ayarlamak, temsilci `Draw`. Çizim için bunu bir `UIView`, kendi katmanına gerçekten çizim.

Katman animasyonları, örtük veya açık olabilir. Örtük animasyonları tanımlayıcı. Yalnızca katman özelliklerini değiştirmeniz gerekir bildirmek ve animasyon yalnızca çalışır. Açık animasyonları, diğer yandan bir katmana eklenen bir animasyon sınıfı aracılığıyla oluşturulur. Açık animasyonları animasyon nasıl oluşturulduğunu üzerinde ek denetim sağlar. Örtük ve açık animasyonları daha derinlemesine içine aşağıdaki bölümleri inceleyin.

### <a name="implicit-animations"></a>Örtük animasyonları

Bir katman özelliklerine animasyon ekleme yollarından biri örtük bir animasyon ' dir. `UIView` animasyonları örtük animasyonlar oluşturun. Ancak, doğrudan bir katman de karşı örtük animasyonlar oluşturabilirsiniz.

Örneğin, aşağıdaki kod bir katmanın ayarlar `Contents` bir görüntüden bir kenarlık genişliği ve rengine ayarlar ve bir görünüm katmanı alt katman katmanı ekler:

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

Katman için örtük bir animasyon eklemek için basitçe özellik değişiklikleri kaydırma bir `CATransaction`. Bu gibi bir görünüm animasyonla canlandırılabilir olmazdı özellikleri sağlayan `BorderWidth` ve `BorderColor` aşağıda gösterildiği gibi:

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

Bu kod ayrıca katmanın canlandırır `Position`, katmanın demir atma noktası ölçülen superlayer'ın koordinatları üst soldan konumu olduğu. Bir katmanı bağlantı noktasını, katmanın koordinat sistemi içinde normalleştirilmiş bir noktasıdır.

Konum ve bağlantı noktasını aşağıdaki şekilde gösterilmiştir:

 ![](core-animation-images/10-postion-anchorpt.png "Bu şekilde konum ve bağlantı noktası gösterilmektedir.")

Örneği çalıştırdığınızda `Position`, `BorderWidth` ve `BorderColor` aşağıdaki ekran görüntülerinde gösterildiği gibi animasyon ekleme:

 ![](core-animation-images/11-implicit-animation.png "Örnek çalıştırıldığında, konum, BorderWidth ve BorderColor gösterildiği gibi animasyon ekleme")

### <a name="explicit-animations"></a>Açık animasyonları

Örtük animasyonları ek olarak, devralınan sınıflar çeşitli temel animasyon içeren `CAAnimation` sağlayan ardından katmana açıkça eklenen animasyonları kapsüller. Bu, animasyon başlangıç değerini değiştirerek, animasyonları gruplandırma ve ana kareleri doğrusal olmayan yollara izin verecek şekilde belirtme gibi animasyon üzerinde daha ayrıntılı denetim sağlar.

Aşağıdaki kod örneği kullanarak bir açık animasyon gösterir bir `CAKeyframeAnimation` daha önce (örtük animasyon bölümünde) gösterilen katman için:

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

Bu kod değişikliklerini `Position` sonra bir ana kare animasyon tanımlamak için kullanılan bir yol oluşturarak katmanın. Dikkat katmanın `Position` son değeri olarak ayarlanır `Position` animasyon öğesinden. Bu, katman aniden geri döner, `Position` animasyon önce animasyon sunu değer ve gerçek model değerinden yalnızca değiştiğinden. Animasyon model değeri son değerine ayarlayarak katmanı kalın animasyon sonunda bir yerde.

Aşağıdaki ekran görüntüleri içeren görüntü ile belirtilen yol animasyonu katmanı göster:

 ![](core-animation-images/12-explicit-animation.png "Bu ekran görüntüsü ile belirtilen yol animasyon içeren bir katman gösterir")
 
## <a name="summary"></a>Özet

Bu makalede aracılığıyla sağlanan animasyon özellikler inceledik *çekirdek animasyon* çerçeveleri. Biz Uıkit içinde animasyon nasıl güç katan hem doğrudan alt düzey animasyon denetimi için nasıl kullanılabileceğini gösteren temel animasyon incelenir.

## <a name="related-links"></a>İlgili bağlantılar

- [Temel animasyon örneği](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Temel Grafikler](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Grafikler ve animasyon için izlenecek yol](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Temel Animasyon](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
