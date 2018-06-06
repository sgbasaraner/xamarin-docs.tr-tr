---
title: 'İzlenecek yol: Dokunma Xamarin.iOS içinde kullanma'
description: Bu belge touch örnek dokunmalı etkileşimler, hareketi tanıyıcıları ve özel hareketi tanıyıcıları ele Xamarin.iOS uygulamalarında nasıl ele alınacağını açıklar.
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: fff49599d3843bb09d407316d6964ca54b6a1004
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784796"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>İzlenecek yol: Dokunma Xamarin.iOS içinde kullanma

Bu kılavuz çeşitli dokunma olaylarına yanıt kodunun nasıl yazılacağını gösterir. Her örneğin ayrı bir ekranda yer alır:

- [Örnekleri touch](#Touch_Samples) – nasıl dokunma olayları yanıt.
- [Tanıyıcı örnekleri hareket](#Gesture_Recognizer_Samples) – yerleşik hareketi tanıyıcıları kullanma.
- [Özel hareketi tanıyıcı örnek](#Custom_Gesture_Recognizer) – özel hareketi tanıyıcı oluşturma.

Her bölüm sıfırdan kod yazmaya yönergeleri içerir.
[Örnek kod başlangıç](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) zaten tam film şeridi ve menü ekran içerir:

 [![](ios-touch-walkthrough-images/image3.png "Örnek menü ekran içerir")](ios-touch-walkthrough-images/image3.png#lightbox)

Film şeridi için kodu ekleyin ve iOS kullanılabilir dokunma olayları farklı türleri hakkında bilgi almak için aşağıdaki yönergeleri izleyin. Bunun yerine, [tamamlanmış örnek](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final) çalışan her şeyi öğrenin.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>Dokunma örnekleri

Bu örnekte, biz API'leri dokunma bazıları gösterilmektedir. Dokunma olayları uygulamak için gerekli kodu eklemek için aşağıdaki adımları izleyin:


1. Projeyi açın **Touch_Start**. Her şeyi uygun olduğundan emin olmak için ilk projeyi çalıştırın ve dokunma **Touch örnekleri** düğmesi. (Hiçbiri düğmelerin çalışır halde) aşağıdakine benzer bir ekran görmeniz gerekir:
    
    [![](ios-touch-walkthrough-images/image4.png "Örnek uygulaması çalışma dışı düğmeleriyle çalıştırın")](ios-touch-walkthrough-images/image4.png#lightbox)


1. Dosyayı düzenlemek **TouchViewController.cs** ve sınıfa aşağıdaki iki örnek değişkenleri eklemek `TouchViewController`:

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```


1. Uygulama `TouchesBegan` aşağıdaki kodda gösterildiği gibi yöntemi:

    ```csharp 
    public override void TouchesBegan(NSSet touches, UIEvent evt)
    {
        base.TouchesBegan(touches, evt);
    
        // If Multitouch is enabled, report the number of fingers down
        TouchStatus.Text = string.Format ("Number of fingers {0}", touches.Count);
    
        // Get the current touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            // Check to see if any of the images have been touched
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Fist image touched
                TouchImage.Image = UIImage.FromBundle("TouchMe_Touched.png");
                TouchStatus.Text = "Touches Began";
            } else if (touch.TapCount == 2 && DoubleTouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Second image double-tapped, toggle bitmap
                if (imageHighlighted)
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
                    TouchStatus.Text = "Double-Tapped Off";
                }
                else
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
                    TouchStatus.Text = "Double-Tapped On";
                }
                imageHighlighted = !imageHighlighted;
            } else if (DragImage.Frame.Contains(touch.LocationInView(View)))
            {
                // Third image touched, prepare to drag
                touchStartedInside = true;
            }
        }
    }
    ```
    
    Bu yöntem denetleyerek çalışır bir `UITouch` nesne ve varsa dokunma nerede oluştuğunu temel bazı eylemleri gerçekleştirin:

    * _TouchImage içinde_ – metni görüntülemek `Touches Began` etiket ve değişiklik görüntü.
    * _DoubleTouchImage içinde_ – hareketi çift dokunmayla olduysa görüntülenen resmi değiştirin.
    * _DragImage içinde_ – dokunma başlatıldığını belirten bir bayrak ayarlayın. Yöntemi `TouchesMoved` Bu bayrak belirlemek için kullanacağı `DragImage` veya değil, ekran taşınması gereken biz sonraki adımda göreceksiniz gibi.

    Yukarıdaki kod yalnızca tek tek rötuşları ile ilgilenir olduğunda, yine davranışı yok kullanıcının ekranda kendi parmak taşınsa. Taşımayı yanıtlamasını uygulamak `TouchesMoved` aşağıdaki kodda gösterildiği gibi:

    ```csharp 
    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchStatus.Text = "Touches Moved";
            }

            //==== IMAGE DRAG
            // check to see if the touch started in the drag me image
            if (touchStartedInside)
            {
                // move the shape
                float offsetX = touch.PreviousLocationInView(View).X - touch.LocationInView(View).X;
                float offsetY = touch.PreviousLocationInView(View).Y - touch.LocationInView(View).Y;
                DragImage.Frame = new RectangleF(new PointF(DragImage.Frame.X - offsetX, DragImage.Frame.Y - offsetY), DragImage.Frame.Size);
            }
        }
    }
    ```

    Bu yöntem alır bir `UITouch` nesne ve dokunma oluştuğu olup olmadığını kontrol eder. Dokunma oluştuysa `TouchImage`, ardından rötuşları taşınmış ekranda görüntülenen metin. 

    Varsa `touchStartedInside` kullanıcı kendi parmak sahip olduğunu biliyoruz sonra doğrudur `DragImage` ve onu hareket etmek. Kodu taşır `DragImage` kullanıcı kendi parmak ekran taşınırken.

1. Kullanıcı kendi parmak ekranın kaldırır ya da touch olay iOS iptal durumu işlemek gerekir. Bunun için biz gerçekleştireceksiniz `TouchesEnded` ve `TouchesCancelled` aşağıda gösterildiği gibi:

    ```csharp
    public override void TouchesCancelled(NSSet touches, UIEvent evt)
    {
        base.TouchesCancelled(touches, evt);
    
        // reset our tracking flags
        touchStartedInside = false;
        TouchImage.Image = UIImage.FromBundle("TouchMe.png");
        TouchStatus.Text = "";
    }
    
    public override void TouchesEnded(NSSet touches, UIEvent evt)
    {
        base.TouchesEnded(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchImage.Image = UIImage.FromBundle("TouchMe.png");
                TouchStatus.Text = "Touches Ended";
            }
        }
        // reset our tracking flags
        touchStartedInside = false;
    }
    ```
    
    Bu yöntemlerin her ikisi de sıfırlayacak `touchStartedInside` bayrağı false olarak ayarlayın. `TouchesEnded` Görüntü kazandırır `TouchesEnded` ekranında.

1. Bu noktada örnekleri dokunmatik ekran tamamlanmıştır. Her görüntü ile etkileşimli olarak nasıl ekran aşağıdaki ekran görüntüsünde gösterildiği gibi değiştiğine dikkat edin:
        
    [![](ios-touch-walkthrough-images/image4.png "Başlangıç uygulaması ekranı")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "Bir düğme kullanıcının sürüklediği sonra ekranı")](ios-touch-walkthrough-images/image5.png#lightbox)
 

<a name="Gesture_Recognizer_Samples" />

##  <a name="gesture-recognizer-samples"></a>Hareketi tanıyıcı örnekleri

[Önceki bölümde](#Touch_Samples) dokunma olayları kullanarak bir nesneyi ekran çevresinde sürüklemek nasıl gösterilmektedir.
Bu bölümde dokunma olaylarını kurtulun ve aşağıdaki hareketi tanıyıcıları kullanmayı göstermektedir:

-  `UIPanGestureRecognizer` Görüntünün çevresine ekran sürükleme.
-  `UITapGestureRecognizer` Çift dokunma ekranında yanıtlamak için.

Çalıştırırsanız [örnek kod başlangıç](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) ve tıklayın **hareketi tanıyıcı örnekleri** düğme, aşağıdaki ekranı görürsünüz:

 [![](ios-touch-walkthrough-images/image6.png "Bu ekran hareketi tanıyıcı örnekleri düğmesini tıklatarak gösterir")](ios-touch-walkthrough-images/image6.png#lightbox)

Hareketi tanıyıcıları uygulamak için aşağıdaki adımları izleyin:


1. Dosyayı düzenlemek **GestureViewController.cs** ve aşağıdaki örnek değişkeni ekleyin:

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Resmin önceki konumunu izlemek için bu örnek değişkeni ihtiyacımız var.
Pan hareketi tanıyıcı kullanacağı `originalImageFrame` görüntüyü ekranda yeniden çizmek için gereken uzaklığı hesaplamak için değer.

1. Denetleyiciye aşağıdaki yöntemi ekleyin:

    ```csharp
    private void WireUpDragGestureRecognizer()
    {
        // Create a new tap gesture
        UIPanGestureRecognizer gesture = new UIPanGestureRecognizer();
    
        // Wire up the event handler (have to use a selector)
        gesture.AddTarget(() => HandleDrag(gesture));  // to be defined
    
        // Add the gesture recognizer to the view
        DragImage.AddGestureRecognizer(gesture);
    }
    ```

    Bu kod başlatır bir `UIPanGestureRecognizer` örneği ve bir görünümüne ekler.
Biz yöntemi biçiminde hareketi için bir hedef Ata fark `HandleDrag` – bu yöntem sonraki adımda sağlanır.

1. HandleDrag uygulamak için denetleyiciye aşağıdaki kodu ekleyin:

    ```csharp
    private void HandleDrag(UIPanGestureRecognizer recognizer)
    {
        // If it's just began, cache the location of the image
        if (recognizer.State == UIGestureRecognizerState.Began)
        {
            originalImageFrame = DragImage.Frame;
        }
    
        // Move the image if the gesture is valid
        if (recognizer.State != (UIGestureRecognizerState.Cancelled | UIGestureRecognizerState.Failed
            | UIGestureRecognizerState.Possible))
        {
            // Move the image by adding the offset to the object's frame
            PointF offset = recognizer.TranslationInView(DragImage);
            RectangleF newFrame = originalImageFrame;
            newFrame.Offset(offset.X, offset.Y);
            DragImage.Frame = newFrame;
        }
    }
    ```

    Yukarıdaki kod önce hareketi tanıyıcı durumunu kontrol edin ve ardından ekran görüntüsü taşıyın. Yerinde şu kodla denetleyicisi artık ekran geçici bir görüntü sürükleme destekler.


1. Ekleme bir `UITapGestureRecognizer` DoubleTouchImage içinde görüntülenmesini görüntü değiştirir. Aşağıdaki yöntemi ekleyin `GestureViewController` denetleyicisi:

    ```csharp
    private void WireUpTapGestureRecognizer()
    {
        // Create a new tap gesture
        UITapGestureRecognizer tapGesture = null;
    
        // Report touch
        Action action = () => {
            TouchStatus.Text = string.Format("Image touched at: {0}",tapGesture.LocationOfTouch(0, DoubleTouchImage));
    
            // Toggle the image
            if (imageHighlighted)
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
            }
            else
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
            }
            imageHighlighted = !imageHighlighted;
        };
    
        tapGesture = new UITapGestureRecognizer(action);
    
        // Configure it
        tapGesture.NumberOfTapsRequired = 2;
    
        // Add the gesture recognizer to the view
        DoubleTouchImage.AddGestureRecognizer(tapGesture);
    }
    ```

    Bu kod için kod çok benzer `UIPanGestureRecognizer` ancak bir temsilci için kullanarak hedef kullanmak yerine bir `Action`. 

1. Değişiklik yapmak için ihtiyacımız son şey. `ViewDidLoad` böylece yalnızca eklediğimiz yöntemleri çağırır. Aşağıdaki kod benzer şekilde ViewDidLoad değiştirin:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        Title = "Gesture Recognizers";
    
        // Save initial state
        originalImageFrame = DragImage.Frame;
    
        WireUpTapGestureRecognizer();
        WireUpDragGestureRecognizer();
    }
    ```

    Biz değerini başlatma de fark `originalImageFrame`.


1. Uygulamayı çalıştırın ve iki görüntü ile etkileşim.
Aşağıdaki ekran görüntüsü bu etkileşimleri örneğidir:
    
    [![](ios-touch-walkthrough-images/image7.png "Bu ekran görüntüsü, bir Sürükle etkileşimi gösterir.")](ios-touch-walkthrough-images/image7.png#lightbox)



<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>Özel hareketi tanıyıcı

Bu bölümde özel hareketi tanıyıcı oluşturmak için önceki bölümlerde kavramları uygulayacağız. Özel hareketi tanıyıcı olacak alt sınıfların `UIGestureRecognizer`, ekranda bir "V" kullanıcı çizer oluştururken tanımak sonra ve bir bit eşlem Değiştir. Aşağıdaki ekran görüntüsünde, bu ekranı örneğidir:

 [![](ios-touch-walkthrough-images/image8.png "Kullanıcı 'V' ekranda çizer olduğunda uygulama algılar")](ios-touch-walkthrough-images/image8.png#lightbox)

Özel hareketi tanıyıcınızı oluşturmak için aşağıdaki adımları izleyin:


1. Adlı projeye yeni bir sınıf ekleyin `CheckmarkGestureRecognizer`ve aşağıdaki kod gibi görünmesi:

    ```csharp
    using System;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace Touch
    {
        public class CheckmarkGestureRecognizer : UIGestureRecognizer
        {
            #region Private Variables
            private CGPoint midpoint = CGPoint.Empty;
            private bool strokeUp = false;
            #endregion
    
            #region Override Methods
            /// <summary>
            ///   Called when the touches end or the recognizer state fails
            /// </summary>
            public override void Reset()
            {
                base.Reset();
    
                strokeUp = false;
                midpoint = CGPoint.Empty;
            }
    
            /// <summary>
            ///   Is called when the fingers touch the screen.
            /// </summary>
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                // we want one and only one finger
                if (touches.Count != 1)
                {
                    base.State = UIGestureRecognizerState.Failed;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the touches are cancelled due to a phone call, etc.
            /// </summary>
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                // we fail the recognizer so that there isn't unexpected behavior
                // if the application comes back into view
                base.State = UIGestureRecognizerState.Failed;
            }
    
            /// <summary>
            ///   Called when the fingers lift off the screen
            /// </summary>
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                //
                if (base.State == UIGestureRecognizerState.Possible && strokeUp)
                {
                    base.State = UIGestureRecognizerState.Recognized;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the fingers move
            /// </summary>
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                // if we haven't already failed
                if (base.State != UIGestureRecognizerState.Failed)
                {
                    // get the current and previous touch point
                    CGPoint newPoint = (touches.AnyObject as UITouch).LocationInView(View);
                    CGPoint previousPoint = (touches.AnyObject as UITouch).PreviousLocationInView(View);
    
                    // if we're not already on the upstroke
                    if (!strokeUp)
                    {
                        // if we're moving down, just continue to set the midpoint at
                        // whatever point we're at. when we start to stroke up, it'll stick
                        // as the last point before we upticked
                        if (newPoint.X >= previousPoint.X && newPoint.Y >= previousPoint.Y)
                        {
                            midpoint = newPoint;
                        }
                        // if we're stroking up (moving right x and up y [y axis is flipped])
                        else if (newPoint.X >= previousPoint.X && newPoint.Y <= previousPoint.Y)
                        {
                            strokeUp = true;
                        }
                        // otherwise, we fail the recognizer
                        else
                        {
                            base.State = UIGestureRecognizerState.Failed;
                        }
                    }
                }
    
                Console.WriteLine(base.State.ToString());
            }
            #endregion
        }
    }
    ```

    Reset yöntemi aldığında çağrılan `State` özelliğini değiştirir ya da `Recognized` veya `Ended`. Bu özel hareketi tanıyıcıyla ayarlamak herhangi bir iç durumu sıfırlamaya zamandır.
Şimdi sınıf, kullanıcı uygulama ile etkileşim yeni başlatıldığında ve hareketi algılamayı yeniden denemek hazır.



1. Size özel hareketi tanıyıcı tanımladığınız göre (`CheckmarkGestureRecognizer`) Düzenle **CustomGestureViewController.cs** dosya ve aşağıdaki iki örnek değişkenleri ekleyin:

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. Örneği ve bizim hareketi tanıyıcı yapılandırmak için denetleyiciye aşağıdaki yöntemi ekleyin:

    ```csharp
    private void WireUpCheckmarkGestureRecognizer()
    {
        // Create the recognizer
        checkmarkGesture = new CheckmarkGestureRecognizer();
    
        // Wire up the event handler
        checkmarkGesture.AddTarget(() => {
            if (checkmarkGesture.State == (UIGestureRecognizerState.Recognized | UIGestureRecognizerState.Ended))
            {
                if (isChecked)
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Unchecked.png");
                }
                else
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Checked.png");
                }
                isChecked = !isChecked;
            }
        });
    
        // Add the gesture recognizer to the view
        View.AddGestureRecognizer(checkmarkGesture);
    }
    ```

1. Düzen `ViewDidLoad` çağırır böylece `WireUpCheckmarkGestureRecognizer`aşağıdaki kod parçacığında gösterildiği gibi:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Uygulamayı çalıştırın ve ekranda bir "V" Çizim deneyin. Görüntü olan değişiklik, görüntülenen aşağıdaki ekran görüntülerinde gösterildiği gibi görmeniz gerekir:
    
    [![](ios-touch-walkthrough-images/image9.png "İşaretli düğmesi")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "Düğmenin işaretli")](ios-touch-walkthrough-images/image10.png#lightbox)



Yukarıdaki üç iOS olayları touch yanıtlamaları kanıtlanabilir farklı yolları bölümler: dokunma olayları, yerleşik hareketi tanıyıcıları kullanarak veya bir özel hareketi tanıyıcı ile.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS Touch başlatın (örnek)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS son Touch (örnek)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
