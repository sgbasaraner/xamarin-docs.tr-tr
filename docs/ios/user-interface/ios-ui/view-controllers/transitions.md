---
title: "Görünüm denetleyicisini geçişleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 88849d3007c007a5ac8820ca84083aa01459c4ce
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="view-controller-transitions"></a>Görünüm denetleyicisini geçişleri

Uıkit görünüm denetleyicileri sunan oluşur animasyonlu geçiş özelleştirmek için destek ekler. Bu destek doğrudan devralınan özel denetleyicileri yanı sıra yerleşik denetleyicileri birlikte `UIViewController`. Ayrıca, `UICollectionViewController` koleksiyon görünümü düzenleri içinde animasyonlu geçişler yararlanmak için denetleyici geçiş özelleştirme yararlanır.

## <a name="custom-transitions"></a>Özel geçişler

İOS 7 görünüm denetleyicileri arasında animasyonlu geçiş tamamen özelleştirilebilir. `UIViewController` Şimdi içeren bir `TransitioningDelegate` özel Animator'da sınıfı sistem için bir geçiş oluştuğunda sağlayan özelliği.

Özel bir geçiş ile kullanmak için `PresentViewController`:

1.  Ayarlama `ModalPresentationStyle` için `UIModalPresentationStyle.Custom` sunulması için denetleyici üzerindeki.
2.  Uygulama `UIViewControllerTransitioningDelegate` Animator'da sınıfı oluşturmak için bir örneği olan `UIViewControllerAnimatedTransitioning` .
3.  Ayarlama `TransitioningDelegate` örneği özelliğine `UIViewControllerTransitioningDelegate` , ayrıca sunulması için denetleyici üzerindeki.
4.  Görünüm denetleyicisi sunar.


Örneğin, aşağıdaki kodu türünde bir görünüm denetleyicisi sunar `ControllerTwo` - a `UIViewController` bir alt kümesi:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Uygulama çalışırken ve düğmesine içinde alt kısmından hale getirmeyi ikinci denetleyicinin görünümünün varsayılan animasyon aşağıda gösterildiği gibi neden olur:

 ![](transitions-images/no-custom-transition.png "Uygulama çalışırken ve düğmesine ikinci denetleyicileri görünümünün alt kısmından hareketli hale getirmeyi varsayılan animasyon neden olur.")

Ancak, ayarı `ModalPresentationStyle` ve `TransitioningDelegate` geçişe yönelik özel animasyon sonuçlanıyor:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom;
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

`TransitioningDelegate` Bir örneğini oluşturmaktan sorumlu `UIViewControllerAnimatedTransitioning` adlı bir alt - `CustomAnimator` aşağıdaki örnekte:

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning PresentingController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

Geçişi gerçekleştiğinde sistem örneği oluşturur `IUIViewControllerContextTransitioning`, Animator'da'nın yöntemlere iletilen. `IUIViewControllerContextTransitioning` içeren `ContainerView` animasyon oluştuğu, geçişi başlatan görünümü denetleyici ve için geçişi görünümü denetleyici yanı sıra.

`UIViewControllerAnimatedTransitioning` Sınıfı gerçek animasyon işler. İki yöntem uygulanması gerekir:

1.  `TransitionDuration` – animasyonun süresini saniye cinsinden döndürür.
1.  `AnimateTransition` – Gerçek animasyon gerçekleştirir.


Örneğin, aşağıdaki uygulayan sınıf `UIViewControllerAnimatedTransitioning` denetleyicinin görünüm çerçeve animasyon eklemek için:

```csharp
public class CustomTransitionAnimator : UIViewControllerAnimatedTransitioning
{
    public CustomTransitionAnimator ()
    {
    }

    public override double TransitionDuration (IUIViewControllerContextTransitioning transitionContext)
    {
        return 1.0;
    }

    public override void AnimateTransition (IUIViewControllerContextTransitioning transitionContext)
        {
            var inView = transitionContext.ContainerView;
            var toVC = transitionContext.GetViewControllerForKey (UITransitionContext.ToViewControllerKey);
            var toView = toVC.View;

            inView.AddSubview (toView);

            var frame = toView.Frame;
            toView.Frame = CGRect.Empty;

            UIView.Animate (TransitionDuration (transitionContext), () => {
                toView.Frame = new CGRect (20, 20, frame.Width - 40, frame.Height - 40);
            }, () => {
                transitionContext.CompleteTransition (true);
            });
        }
}
```

Düğme dokunduğunuz, artık, animasyonun uygulanan `UIViewControllerAnimatedTransitioning` sınıfı kullanılır:

 ![](transitions-images/custom-transition.png "Geçerli çalışan yakınlaştırma örneği")

## <a name="collection-view-transitions"></a>Koleksiyon görünümü geçişleri

Koleksiyon görünümlerini animasyonlu geçişler oluşturmak için yerleşik destek vardır:

-  **Gezinti denetleyicileri** – animasyonlu iki arasında geçiş `UICollectionViewController` örneklerini isteğe bağlı olarak işlenebilir otomatik olarak ne zaman bir `UINavigationController` bunları yönetir.
-  **Geçiş düzeni** – yeni bir `UICollectionViewTransitionLayout` sınıfı düzenleri arasında geçiş etkileşimli verir.


### <a name="navigation-controller-transitions"></a>Gezinti denetleyicisi geçişleri

Bir gezinme denetleyiciyle kullanıldığında bir `UICollectionViewController` denetleyicileri arasında animasyonlu geçişler için destek içerir. Bu destek, yerleşik olarak bulunur ve uygulamak için yalnızca birkaç basit adımı gerektirir:

1.  Ayarlama `UseLayoutToLayoutNavigationTransitions` için `false` üzerinde bir `UICollectionViewController` .
1.  Bir örnek ekler `UICollectionViewController` Gezinti denetleyicinin yığınının kök.
1.  İkinci bir oluşturma `UICollectionViewController` ve kendi `UseLayoutToLayoutNavigtionTransitions` özelliğine `true` .
1.  İkinci anında `UICollectionViewController` Gezinti denetleyicinin yığını üzerine.


Aşağıdaki kod ekler bir `UICollectionViewController` adlı bir alt `ImagesCollectionViewController` bir gezinti denetleyicisinin yığınının kök ile `UseLayoutToLayoutNavigationTransitions` özelliğini `false`:

```csharp
UIWindow window;
ImagesCollectionViewController viewController;
UICollectionViewFlowLayout layout;
UINavigationController navController;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    // create and initialize a UICollectionViewFlowLayout
    layout = new UICollectionViewFlowLayout (){
        SectionInset = new UIEdgeInsets (10,5,10,5),
        MinimumInteritemSpacing = 5,
        MinimumLineSpacing = 5,
        ItemSize = new CGSize (100, 100)
    };

    viewController = new ImagesCollectionViewController (layout) {
            UseLayoutToLayoutNavigationTransitions = false;
        }

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();
    
    return true;
}
```

Bir öğe seçildiğinde, ikinci bir örneğini `ImagesController` yalnızca bu kez farklı düzen sınıfı kullanılarak oluşturulur. Bu denetleyici için `UseLayoutToLayoutNavigtionTransitions` ayarlanır `true`, aşağıda gösterildiği gibi:

```csharp
CircleLayout circleLayout;
ImagesCollectionViewController controller2;

...

public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // UseLayoutToLayoutNavigationTransitions when item is selected
        circleLayout = new CircleLayout (Monkeys.Instance.Count){
                ItemSize = new CGSize (100, 100)
            };
            
    controller2 = new ImagesCollectionViewController (circleLayout) {
        UseLayoutToLayoutNavigationTransitions = true;
        }

    NavigationController.PushViewController (controller2, true);
}
```

`UseLayoutToLayoutNavigationTransitions` Gezinti yığını denetleyicisi eklenmeden önce özelliği ayarlanmalıdır. Bu özellik kümesiyle normal yatay kayan geçiş aşağıda gösterildiği gibi iki denetleyicileri düzenleri arasında animasyonlu bir geçiş değiştirilir:

![](transitions-images/nav2.png "Animasyonlu geçişe iki denetleyicileri düzenleri arasında")

### <a name="transition-layout"></a>Geçiş düzeni

Gezinti denetleyicileri içinde düzeni geçiş desteği ek olarak, yeni bir düzen olarak adlandırılan `UICollectionViewTransitionLayout` kullanıma sunulmuştur. Bu düzen sınıfı etkileşimli denetim düzeni geçiş işlemi sırasında vererek verir `TransitionProgress` koddan ayarlanacak. `UICollectionViewTransitionLayout` farklı - ve yenisini değil - `SetCollectionViewLayout` animasyonlu düzeni geçişe oluşmasına neden iOS 6 yönteminden. Bu yöntem, animasyonlu geçişin ilerleme durumunu denetlemek için yerleşik destek sağlamadı.

 `UICollectionViewTransitionLayout` , örneğin, kullanıcı etkileşimi, geçiş için hedeflenen düzeni yanı sıra özgün düzeni yönetme tarafından yanıta düzenleri arasında geçiş denetlemek için yapılandırılacak hareketi tanıyıcı sağlar.

Etkileşimli geçişe kullanarak hareketi tanıyıcı içinde uygulamaya yönelik adımlar `UICollectionViewTransitionLayout` aşağıdaki gibidir:

1.  Hareketi tanıyıcı oluşturun.
1.  Çağrı `StartInteractiveTransition` yöntemi `UICollectionView` , hedef düzeni ve tamamlanma işleyici geçirme.
1.  Ayarlama `TransitionProgress` özelliği `UICollectionViewTransitionLayout` döndürülen örneği `StartInteractiveTransition` yöntemi.
1.  Düzen geçersiz.
1.  Çağrı `FinishInteractiveTransition` yöntemi `UICollectionView` Geçişi tamamlamak için veya `CancelInteractiveTransition` iptal etmek yöntemi.  `FinishInteractiveTransition` ancak hedef düzeni, geçişi tamamlamak animasyon neden `CancelInteractiveTransition` özgün düzene döndürme animasyon sonuçlanıyor.
1.  Geçiş tamamlandığında tamamlama işleyicisinde işlemek `StartInteractiveTransition` yöntemi.
1.  Hareketi tanıyıcı koleksiyonu görünümüne ekleyin.


Aşağıdaki kod etkileşimli düzeni geçişe tutarak hareketi tanıyıcı içinde uygular:

```csharp
imagesController = new ImagesCollectionViewController (flowLayout);

nfloat sf = 0.4f;
UICollectionViewTransitionLayout trLayout = null;
UICollectionViewLayout nextLayout;

pinch = new UIPinchGestureRecognizer (g => {

    var progress = Math.Abs(1.0f -  g.Scale)/sf;

    if(trLayout == null){
        if(imagesController.CollectionView.CollectionViewLayout is CircleLayout)
            nextLayout = flowLayout;
        else
            nextLayout = circleLayout;

        trLayout = imagesController.CollectionView.StartInteractiveTransition (nextLayout, (completed, finished) => {   
            Console.WriteLine ("transition completed");
            trLayout = null;
        });
    }

    trLayout.TransitionProgress = (nfloat)progress;

    imagesController.CollectionView.CollectionViewLayout.InvalidateLayout ();

    if(g.State == UIGestureRecognizerState.Ended){
        if (trLayout.TransitionProgress > 0.5f)
            imagesController.CollectionView.FinishInteractiveTransition ();
        else
            imagesController.CollectionView.CancelInteractiveTransition ();
    }

});

imagesController.CollectionView.AddGestureRecognizer (pinch);

```

Koleksiyon görünümü kullanıcı pinches gibi `TransitionProgress` tutarak ölçeğe bağlı ayarlanır. Geçiş % 50 tamamlandı, önce kullanıcı tutarak ererse bu uygulama, geçiş işlemi iptal edildi. Aksi takdirde, geçişi tamamlanmıştır.




## <a name="related-links"></a>İlgili bağlantılar

- [İOS 7 (örnek) giriş](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 Kullanıcı Arabirimine Genel Bakış](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Arka Planda İşleme](~/ios/app-fundamentals/backgrounding/index.md)
