---
title: "İOS 7 giriş"
description: "Bu makalede, iOS 7 View Controller geçişleri UIView animasyonları, Uıkit Dynamics ve metin Seti geliştirmeler dahil, sunulan önemli yeni API'leri yer almaktadır. Ayrıca, bazı değişiklikler kullanıcı arabirimi ve yeni enchanced çoklu özellikleri kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a7bebc2b73ecb564028a92340c726bd5c1f1c54b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-7"></a>İOS 7 giriş

_Bu makalede, iOS 7 View Controller geçişleri UIView animasyonları, Uıkit Dynamics ve metin Seti geliştirmeler dahil, sunulan önemli yeni API'leri yer almaktadır. Ayrıca, bazı değişiklikler kullanıcı arabirimi ve yeni enchanced çoklu özellikleri kapsar._

iOS 7, iOS için önemli bir güncelleştirmedir. Üzerinde içerik yerine uygulama chrome odak koyar tamamen yeni bir kullanıcı arabirimi tasarımı sunar. Görsel değişiklikler iOS 7 daha zengin etkileşimleri ve deneyimleri oluşturmak için yeni API sayısız ekler. Bu belge anketler yeni teknolojilerden iOS 7 ile sunulan ve daha fazla araştırması için bir başlangıç noktası olarak hizmet verir.

## <a name="uiview-animation-enhancements"></a>UIView animasyon geliştirmeleri

iOS 7 Uıkit, daha önce çekirdek animasyon framework doğrudan bırakma gerekli işlemleri uygulamaların izin verecek şekilde animasyon desteği artırmaktadır. Örneğin, `UIView` ana kare animasyonların yanı sıra yay animasyonları artık gerçekleştirebilirsiniz, daha önce bir `CAKeyframeAnimation` uygulanan bir `CALayer`.

### <a name="spring-animations"></a>Yay animasyonları

 `UIView` artık bir yay efekti hareketlendirme özelliği değişikliklerle destekler. Bu eklemek için ya da çağrısı `AnimateNotify` veya `AnimateNotifyAsync` yöntemi, aşağıda açıklandığı gibi değerler yay düzeltme oranı ve ilk yay hız için geçirme:

-  `springWithDampingRatio` – Bir değer arasında 0 ve 1, burada oscillation için daha küçük değeri artırır.
-  `initialSpringVelocity` – İlk yay hız Saniyede toplam animasyon uzaklığı yüzdesi.


Görüntü görünümün merkezi değiştiğinde aşağıdaki kodu yay efekti oluşturur:

```csharp
void AnimateWithSpring ()
{ 
    float springDampingRatio = 0.25f;
    float initialSpringVelocity = 1.0f;
    
    UIView.AnimateNotify (3.0, 0.0, springDampingRatio, initialSpringVelocity, 0, () => {
    
        imageView.Center = new CGPoint (imageView.Center.X, 400);   
            
    }, null);
}
```

Bu yay etkiyi animasyonunun yeni bir merkezi konuma tamamladıkça aşağıda gösterildiği gibi Sıçrama görünmesine resim görünümü neden olur:

 ![](images/spring-animation.png "Bu yay etkiyi animasyonunun yeni bir merkezi konuma tamamladıkça Sıçrama görünmesine resim görünümü neden olur.")

### <a name="keyframe-animations"></a>Animasyon ana kare

`UIView` Sınıfı şimdi içerir `AnimateWithKeyframes` ana kare animasyonları oluşturma yöntemi bir `UIView`. Bu yöntem için diğer benzer `UIView` animasyon yöntemleri dışındaki bir ek `NSAction` ana kare dahil etmek için bir parametre geçirilir. İçinde `NSAction`, ana kare çağrılarak eklenir `UIView.AddKeyframeWithRelativeStartTime`.

Örneğin, aşağıdaki kod parçacığını bir ana kare animasyon görünümü döndürmek için de bir görünümün merkezi hale getirmeyi oluşturur:

```csharp
void AnimateViewWithKeyframes ()
{
    var initialTransform = imageView.Transform;
    var initialCeneter = imageView.Center;

    // can now use keyframes directly on UIView without needing to drop directly into Core Animation

    UIView.AnimateKeyframes (2.0, 0, UIViewKeyframeAnimationOptions.Autoreverse, () => {
        UIView.AddKeyframeWithRelativeStartTime (0.0, 0.5, () => { 
            imageView.Center = new CGPoint (200, 200);
        });

        UIView.AddKeyframeWithRelativeStartTime (0.5, 0.5, () => { 
            imageView.Transform = CGAffineTransform.MakeRotation ((float)Math.PI / 2);
        });
    }, (finished) => {
        imageView.Center = initialCeneter;
        imageView.Transform = initialTransform;

        AnimateWithSpring ();
    });
}
```

İlk iki parametre `AddKeyframeWithRelativeStartTime` yöntemini belirtin başlangıç saatini ve süresini ana kare sırasıyla animasyon gerçekleştirilmesini yüzdesi. Görüntünün görünümü yeni merkezine ilk ikinci ortasının sonuçları Yukarıdaki örnek ardından sonraki saniye içinde 90 derece döndürerek. Animasyonun belirtir beri `UIViewKeyframeAnimationOptions.Autoreverse` bir seçenek olarak, her iki ana kare de geriye doğru animasyon. Son olarak, son değerleri tamamlama işleyicisinde ilk durumuna ayarlanır.

Aşağıdaki ekran görüntüleri birleşik animasyon ana kare aracılığıyla gösterilmektedir:

 ![](images/keyframes.png "Bu ekran görüntüleri birleşik animasyon ana kare aracılığıyla gösterir")

## <a name="uikit-dynamics"></a>Uıkit Dynamics

Uıkit Dynamics uygulamaların fizik üzerinde temel animasyonlu etkileşimlerine izin ver Uıkit API'lerinde yeni kümesidir. Uıkit Dynamics buna olanak sağlamak için bir 2B fizik altyapısı yalıtır.

Doğası gereği bildirim temelli bir API'dir. Adlı nesneleri - oluşturarak fizik etkileşimleri nasıl davranacağını bildirme *davranışları* - hızlı fizik kavramlarına yer çekimi, çakışmaları, yaylar vb. gibi. Adlı başka bir nesneye behavior(s) attach sonra bir *dinamik Animator'da*, bir görünüm yalıtır. Dinamik Animator'da bildirilen fizik davranışları uygulama cares geçen *dinamik öğeler* -öğelerini uygulayan `IUIDynamicItem`, gibi bir `UIView`.

Birkaç farklı ilkel davranışları dahil olmak üzere karmaşık etkileşimler tetiklemek kullanılabilir vardır:

-  `UIAttachmentBehavior` – Sağlayacak şekilde birlikte hareket iki dinamik öğe ekler veya ek noktanız dinamik bir öğe ekler.
-  `UICollisionBehavior` – İçinde çakışma katılmayı dinamik öğeleri sağlar.
-  `UIDynamicItemBehavior` – Genel bir esneklik, yoğunluğu ve uyuşmazlık gibi dinamik öğelerine uygulamak için özellikler kümesini belirtir.
-  `UIGravityBehavior` -Yer çekimi gravitational yönde hızlandırmak için öğeleri neden dinamik bir öğe için geçerlidir.
-  `UIPushBehavior` – Force dinamik bir öğeye uygular.
-  `UISnapBehavior` – Bir dinamik öğesinin yay etkisi olmadan bir konuma Daya olanak sağlar.


Birçok temelleri olsa da, fizik tabanlı etkileşimleri Uıkit Dynamics kullanarak bir görünüm eklemek için genel davranışları tutarlı bir işlemdir:

1.  Dinamik Animator'da oluşturun.
1.  Behavior(s) oluşturun.
1.  Davranışları dinamik Animator'da ekleyin.


### <a name="dynamics-example"></a>Dynamics örneği

Yer çekimi ve bir çakışma sınır ekler bir örneğe bakalım bir `UIView`.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

Yer çekimi bir görüntü görünümüne ekleme yukarıda özetlenen 3 adımları izler.

Biz karşılaşmayacağınızı `ViewDidLoad` Bu örnek için yöntem. İlk olarak, ekleyin bir `UIImageView` gibi örneği:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

Ekranın üst sınırında ortalanmış bir resim görünümü oluşturur. Yer çekimi ile "sonbaharda" görüntüsü oluşturmak için bir örneğini oluşturmak bir `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

`UIDynamicAnimator` Başvuru örneği alır `UIView` veya `UICollectionViewLayout`, ekli behavior(s) animasyonlu öğeleri içerir.

Ardından, oluşturun bir `UIGravityBehavior` örneği. Uygulama bir veya daha fazla nesnelerini geçirebilirsiniz `IUIDynamicItem`gibi bir `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

Davranış dizisi olarak geçirilir `IUIDynamicItem`, bu durumda içeren tek `UIImageView` animasyon örneği.

Son olarak, dinamik Animator'da davranışı ekleyin:

```csharp
dynAnimator.AddBehavior (gravity);
```

Bu, aşağıda Resimli olarak yer çekimi ile aşağı animasyon görüntü sonuçlanır:

![](images/gravity2.png "Başlangıç görüntü konumu") 
![](images/gravity3.png "bitiş görüntü konumu")

Olduğundan ekran sınırlarının sınırlama hiçbir şey, resim görünümü sadece alt döner. Böylece görüntü ekran kenarlar ile çakışıyor görünümü sınırlamak için ekleyebiliriz bir `UICollisionBehavior`. Biz bu sonraki bölümde ele alacağız.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

Biz oluşturarak başlarsınız bir `UICollisionBehavior` ve komutlarında yaptığımız gibi dinamik Animator'da için ekleyerek `UIGravityBehavior`.

Dahil etmek için kodu değiştirin `UICollisionBehavior`:

```csharp
using (image = UIImage.FromFile ("monkeys.jpg")) {

    imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
        Image =  image
    };

    View.AddSubview (imageView);

    // 1. create the dynamic animator
    dynAnimator = new UIDynamicAnimator (this.View);

    // 2. create behavior(s)
    var gravity = new UIGravityBehavior (imageView);
    var collision = new UICollisionBehavior (imageView) {
        TranslatesReferenceBoundsIntoBoundary = true
    };

    // 3. add behaviors(s) to the dynamic animator
    dynAnimator.AddBehaviors (gravity, collision);
}
```

`UICollisionBehavior` Adlı bir özelliği vardır `TranslatesReferenceBoundsIntoBoundry`. Bu ayar `true` başvuru çakışma sınır olarak kullanılacak görünümün sınırları neden olur.

Görüntü aşağı yer çekimi ile canlandırır, şimdi onu biraz ekranın devre dışı var. kalanına şekilde önce geri dönmeler.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

Biz, daha fazla ek davranışları ile düşer halde resim görünümü davranışını kontrol edebilirsiniz. Örneğin, eklediğimiz bir `UIDynamicItemBehavior` görüntü ekranın ile çakışıyor olduğunda daha fazla sıçrama görünüme neden esneklik artırmak için.

Ekleme bir `UIDynamicItemBehavior` davranışları gibi ile aynı adımları izlemektedir. İlk davranışı oluşturun:

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

Daha sonra dinamik Animator'da davranışı ekleyin:

 `dynAnimator.AddBehavior (dynBehavior);`

Sınırlarıyla çakışıyor olduğunda bu davranış yerinde resim görünümü daha geri dönmeler.

## <a name="general-user-interface-changes"></a>Genel kullanıcı arabirimi değişiklikleri

Uıkit Dynamics, denetleyici geçişleri ve Gelişmiş UIView animasyonları yukarıda açıklandığı gibi yeni Uıkit API'leri yanı sıra çeşitli UI visual değişiklikleri ve ilişkili API değişiklikleri çeşitli görünümler ve denetimler için iOS 7 tanıtır. Daha fazla bilgi için bkz: [iOS 7 kullanıcı arabirimine genel bakış](~/ios/platform/introduction-to-ios7/ios7-ui.md).

## <a name="text-kit"></a>Metin Seti

Metin, güçlü metin düzeni ve işleme özellikleri sunan yeni bir API setidir. Düşük düzey çekirdek metin çerçevesi üzerine inşa ancak çekirdek metin kullanmak çok daha kolaydır.

Daha fazla bilgi için lütfen bkz bizim [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>Çoklu

iOS 7 ne zaman ve arka plan iş nasıl gerçekleştirildiğini değiştirir. İOS 7 görev tamamlamada artık uygulamalar uyanık arka planda çalışan görevleri ve uygulamalar için arka plan işlemleri bitişik olmayan bir biçimde woken tutar. iOS 7 arka planda yeni içerikle uygulamaları güncelleştirmek için üç yeni API'ler de ekler:

-  Arka planda getirmeye – düzenli aralıklarla arka planda içeriği güncelleştirmek için uygulamaları sağlar.
-  Uzak bildirimler - bir anında iletme bildirimi alırken içeriği güncelleştirmek uygulamaları sağlar. Bildirimler ya da olabilir Sessiz veya bir başlık kilit ekranı görüntüleyebilirsiniz.
-  Arka plan Aktarım Hizmeti – karşıya yükleme ve sabit bir zaman sınırı olmadan büyük dosyaları gibi veri indirme sağlar.


Yeni çoklu özellikleri hakkında daha fazla ayrıntı için Xamarin iOS bölümlerine bakın [Backgrounding Kılavuzu](~/ios/app-fundamentals/backgrounding/index.md).

## <a name="summary"></a>Özet

Bu makalede, birkaç önemli kümeye yeni eklenecekleri iOS yer almaktadır. İlk olarak, görünüm denetleyicileri için özel geçişler ekleme gösterir. Ardından, görünümlerde koleksiyon, hem de bir gezinti denetleyicisi içinde etkileşimli olarak arasında ve koleksiyon görünümlerini geçişleri kullanmayı gösterir. Ardından, Uıkit uygulamaların önceden çekirdek animasyon karşı doğrudan programlama gereken noktalar için nasıl kullanılacağını gösteren UIView animasyonları yapılan birkaç geliştirme sunmaktadır. Son olarak, hangi fizik altyapısı için Uıkit getirir, yeni Uıkit Dynamics API, metin Seti çerçevede şimdi kullanılabilir zengin metin desteği yanında sunulmuştur.

## <a name="related-links"></a>İlgili bağlantılar

- [İOS 7 (örnek) giriş](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 kullanıcı arabirimine genel bakış](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)
