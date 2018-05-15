---
title: İzlenecek yol - Android dokunma kullanma
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/09/2018
ms.openlocfilehash: 625ba800ce498f80c0344c67e26bd79360de4002
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="walkthrough---using-touch-in-android"></a>İzlenecek yol - Android dokunma kullanma

Bize kavramları çalışan bir uygulama önceki bölümde kullanmak bkz. Dört etkinlikleri ile bir uygulama oluşturacağız. İlk etkinlik menü veya çeşitli API'ler göstermek için diğer etkinlikleri başlatacak bir denetim panosu olacaktır. Aşağıdaki ekran görüntüsü ana etkinlik gösterir:

[![Örnek ekran Touch bana düğmesi](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

Touch örnek, ilk etkinliğin görünümleri dokunmak için olay işleyicileri kullanmayı gösterir. Hareketi tanıyıcı etkinlik gösteren alt nasıl `Android.View.Views` ve olayları işleme yanı sıra tutarak hareketleri nasıl ele alınacağını gösterir. Üçüncü ve son etkinlik **özel hareketi**, Göster nasıl özel hareketler kullanır. İzleyin ve almak işleri kolaylaştırmak için Biz bu kılavuzda etkinliklerden birini odaklanan her bölüm içeren bölümlere ayırmak.

## <a name="touch-sample-activity"></a>Touch örnek etkinlik

-   Projeyi açın **TouchWalkthrough\_Başlat**. **MainActivity** gitmek için tüm kümesi &ndash; bize dokunma davranışa etkinliğin uygulamak için en fazla olduğu. Uygulamayı çalıştırın ve'ı tıklatırsanız **Touch örnek**, aşağıdaki etkinlik başlamanız gerekir:

    [![Ekran görüntüsü etkinlik Touch görüntülenen başlar](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Biz, etkinlik başlar onayladıktan, dosyayı açma **TouchActivity.cs** ve için bir işleyici ekleyin `Touch` olayı `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Sonra aşağıdaki yöntemi ekleyin **TouchActivity.cs**:

    ```csharp
    private void TouchMeImageViewOnTouch(object sender, View.TouchEventArgs touchEventArgs)
    {
        string message;
        switch (touchEventArgs.Event.Action & MotionEventActions.Mask)
        {
            case MotionEventActions.Down:
            case MotionEventActions.Move:
            message = "Touch Begins";
            break;

            case MotionEventActions.Up:
            message = "Touch Ends";
            break;

            default:
            message = string.Empty;
            break;
        }

        _touchInfoTextView.Text = message;
    }
    ```

Yukarıdaki kod biz kabul dikkat edin `Move` ve `Down` eylem olarak aynı. Rağmen kullanıcı kendi parmak Yükselt değil olmasıdır `ImageView`, onu dolaşmak veya kullanıcı tarafından exerted baskısı değişebilir. Bu tip değişiklikler oluşturacak bir `Move` eylem.

Her zaman kullanıcı rötuşları `ImageView`, `Touch` olay harekete geçirilen ve bizim işleyici iletisini görüntüler **Touch başlar** aşağıdaki ekran görüntüsünde gösterildiği gibi ekranında:

[![Ekran görüntüsü etkinlik Touch başlar](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Kullanıcı temas sürece `ImageView`, **Touch başlar** görüntülenmesi `TextView`. Ne zaman kullanıcı artık temas `ImageView`, ileti **Touch sona erer** görüntülenmesi `TextView`, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Ekran görüntüsü etkinlik Touch sona erer](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Hareketi tanıyıcı etkinliği

Şimdi hareketi tanıyıcı etkinlik uygulayan olanak sağlar. Bu etkinliğin nasıl ekran geçici bir görünüm sürükleyin ve tutarak yakınlaştırma uygulamak için bir yol göstermeye gösterilmektedir.

-   Adlı uygulama yeni bir etkinlik eklemek `GestureRecognizer`.
    Bu etkinlik için kod, aşağıdaki kodu benzer şekilde düzenleyin:

    ```csharp
    public class GestureRecognizerActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            View v = new GestureRecognizerView(this);
            SetContentView(v);
        }
    }
    ```

-   Yeni bir Android eklemek projeye görüntülemek ve adlandırın `GestureRecognizerView`. Bu sınıf için aşağıdaki değişkenleri ekleyin:

    ```csharp
    private static readonly int InvalidPointerId = -1;

    private readonly Drawable _icon;
    private readonly ScaleGestureDetector _scaleDetector;

    private int _activePointerId = InvalidPointerId;
    private float _lastTouchX;
    private float _lastTouchY;
    private float _posX;
    private float _posY;
    private float _scaleFactor = 1.0f;
    ```

-   Aşağıdaki oluşturucuyu ekleyin `GestureRecognizerView`. Bu oluşturucu ekleyecek bir `ImageView` bizim etkinlik. Bu noktada kodu hala derlenmez &ndash; sınıfı oluşturmak ihtiyacımız `MyScaleListener` yeniden boyutlandırma ile yardımcı olacak `ImageView` zaman kullanıcı pinches onu:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Bizim faaliyete resim çizmek için geçersiz kılmak ihtiyacımız `OnDraw` yöntemi aşağıdaki kod parçacığında gösterildiği gibi görünüm sınıfı. Bu kodu taşır `ImageView` tarafından belirtilen konuma `_posX` ve `_posY` resize görüntünün ölçekleme faktörü göre:

    ```csharp
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        canvas.Save();
        canvas.Translate(_posX, _posY);
        canvas.Scale(_scaleFactor, _scaleFactor);
        _icon.Draw(canvas);
        canvas.Restore();
    }
    ```

-   Sonraki örnek değişkeni güncelleştirmek ihtiyacımız `_scaleFactor` kullanıcı olarak pinches `ImageView`. Adlı bir sınıf ekleyeceğiz `MyScaleListener`. Bu sınıf, kullanıcı pinches zaman Android tarafından gerçekleştirilecektir ölçek olaylarını dinler `ImageView`.
    Aşağıdaki iç sınıfına ekleyin `GestureRecognizerView`. Bu sınıf, bir `ScaleGesture.SimpleOnScaleGestureListener`. Bu sınıf hareketleri bir kısmı ilgilendiğiniz zaman dinleyicileri bir alt kümesi için kolaylık sınıftır:

    ```csharp
    private class MyScaleListener : ScaleGestureDetector.SimpleOnScaleGestureListener
    {
        private readonly GestureRecognizerView _view;

        public MyScaleListener(GestureRecognizerView view)
        {
            _view = view;
        }

        public override bool OnScale(ScaleGestureDetector detector)
        {
            _view._scaleFactor *= detector.ScaleFactor;

            // put a limit on how small or big the image can get.
            if (_view._scaleFactor > 5.0f)
            {
                _view._scaleFactor = 5.0f;
            }
            if (_view._scaleFactor < 0.1f)
            {
                _view._scaleFactor = 0.1f;
            }

            _iconview.Invalidate();
            return true;
        }
    }
    ```

-   İhtiyacımız içinde geçersiz kılmak için sonraki yöntemi `GestureRecognizerView` olan `OnTouchEvent`. Aşağıdaki kod, bu yöntem tam uygulamasını listeler. Çok fazla kod yoktur Burada, bu nedenle bir dakikanızı ayırın ve burada neler bakın sağlar. Gerekirse simgesi ölçeklendirmek bu yöntem yapacağı ilk şey olduğunu &ndash; bu çağırarak işlenir `_scaleDetector.OnTouchEvent`. Sonraki Biz bu yöntem ne çağrılan şekil deneyin:

    - Kullanıcı ekranla işlemdeki, X ve Y konumlarını ve ekran işlemdeki ilk işaretçi Kimliğini kaydeder.

    - Ardından kullanıcının ekranda kendi dokunma taşındıysa, kullanıcı işaretçiyi ne kadar taşınmış şekil.

    - Kullanıcı kendi parmak ekranın kaldırılmış değilse, ardından biz hareketleri izlemeyi durdurur.

    ```csharp
    public override bool OnTouchEvent(MotionEvent ev)
    {
        _scaleDetector.OnTouchEvent(ev);

        MotionEventActions action = ev.Action & MotionEventActions.Mask;
        int pointerIndex;

        switch (action)
        {
            case MotionEventActions.Down:
            _lastTouchX = ev.GetX();
            _lastTouchY = ev.GetY();
            _activePointerId = ev.GetPointerId(0);
            break;

            case MotionEventActions.Move:
            pointerIndex = ev.FindPointerIndex(_activePointerId);
            float x = ev.GetX(pointerIndex);
            float y = ev.GetY(pointerIndex);
            if (!_scaleDetector.IsInProgress)
            {
                // Only move the ScaleGestureDetector isn't already processing a gesture.
                float deltaX = x - _lastTouchX;
                float deltaY = y - _lastTouchY;
                _posX += deltaX;
                _posY += deltaY;
                Invalidate();
            }

            _lastTouchX = x;
            _lastTouchY = y;
            break;

            case MotionEventActions.Up:
            case MotionEventActions.Cancel:
            // We no longer need to keep track of the active pointer.
            _activePointerId = InvalidPointerId;
            break;

            case MotionEventActions.PointerUp:
            // check to make sure that the pointer that went up is for the gesture we're tracking.
            pointerIndex = (int) (ev.Action & MotionEventActions.PointerIndexMask) >> (int) MotionEventActions.PointerIndexShift;
            int pointerId = ev.GetPointerId(pointerIndex);
            if (pointerId == _activePointerId)
            {
                // This was our active pointer going up. Choose a new
                // action pointer and adjust accordingly
                int newPointerIndex = pointerIndex == 0 ? 1 : 0;
                _lastTouchX = ev.GetX(newPointerIndex);
                _lastTouchY = ev.GetY(newPointerIndex);
                _activePointerId = ev.GetPointerId(newPointerIndex);
            }
            break;

        }
        return true;
    }
    ```

-   Şimdi uygulamayı çalıştırın ve hareketi tanıyıcı etkinliği başlatın.
    Başladığında Ekran aşağıdaki ekran görüntüsüne benzer görünmelidir:

    [![Hareketi tanıyıcı ekran Android simgesiyle başlangıç](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Şimdi simgesi dokunma ve ekran sürükleyin. Tutarak yakınlaştırma hareketi deneyin. Belirli bir noktada ekranınızda aşağıdaki ekran şöyle görünebilir:

    [![Ekran hareketlerini taşıma simgesi](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

Bu noktada, kendinize bir pat arkasında vermeniz gerekir: bir Android uygulamasını tutarak yakınlaştırma yalnızca uygulanmadı! Bir hızlı sonu alabilir ve bu kılavuzda üçüncü ve son etkinlik taşıma sağlar &ndash; özel hareketler kullanma.

## <a name="custom-gesture-activity"></a>Özel hareketi etkinliği

Bu kılavuzda son ekran özel hareketler kullanır.

Bu kılavuzda amaçları doğrultusunda, hareketleri kitaplığı zaten hareketi aracını kullanarak oluşturulduğundan ve dosyayı'nde projeye eklenen **kaynakları/ham/hareketleri**. İzlenecek yol son etkinlik almak göz önünden housekeeping bu bit ile sağlar.

-   Adlı bir düzen dosya eklemek **özel\_hareketi\_layout.axml** projesine aşağıdaki içeriğe sahip. Proje tüm görüntüleri zaten **kaynakları** klasörü:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
        <ImageView
            android:src="@drawable/check_me"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="3"
            android:id="@+id/imageView1"
            android:layout_gravity="center_vertical" />
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
    </LinearLayout>
    ```

-   Sonraki projeye yeni bir etkinlik ekleyin ve adını `CustomGestureRecognizerActivity.cs`. İki örneği değişken sınıfına aşağıdaki iki kod satırı gösteren olarak ekleyin:

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Düzen `OnCreate` bu yöntemi aşağıdaki kod, BT'nin benzer şekilde etkinliği. Bu kodda neler olduğunu açıklamak için bir dakikanızı ayırın olanak sağlar. Bunu yapmadan ilk örneği şeydir bir `GestureOverlayView` ve etkinlik kök görünümü olarak ayarlayın.
    Biz de bir olay işleyicisi atamak `GesturePerformed` olayı `GestureOverlayView`. Sonraki biz daha önce oluşturulan düzeni dosyasını Şişir ve bir alt görünüm olarak eklemek `GestureOverlayView`. Değişkeni başlatmak için son adımdır `_gestureLibrary` hareketleri dosyasını ve uygulama kaynaklarından yükleyin. Hareketleri dosyanın herhangi bir nedenden dolayı yüklenemezse yok çok bu etkinliği yapabilirsiniz, kapatma gelir:

    ```csharp
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
        SetContentView(gestureOverlayView);
        gestureOverlayView.GesturePerformed += GestureOverlayViewOnGesturePerformed;

        View view = LayoutInflater.Inflate(Resource.Layout.custom_gesture_layout, null);
        _imageView = view.FindViewById<ImageView>(Resource.Id.imageView1);
        gestureOverlayView.AddView(view);

        _gestureLibrary = GestureLibraries.FromRawResource(this, Resource.Raw.gestures);
        if (!_gestureLibrary.Load())
        {
            Log.Wtf(GetType().FullName, "There was a problem loading the gesture library.");
            Finish();
        }
    }
    ```

-   İhtiyacımız yöntemi uygulamak için en son şey `GestureOverlayViewOnGesturePerformed` aşağıdaki kod parçacığında gösterildiği gibi. Zaman `GestureOverlayView` bir hareketi algılar geri için bu yöntemi çağırır. Sizi çalışırız almak için ilk şey bir `IList<Prediction>` çağırarak hareketi eşleşen nesneleri `_gestureLibrary.Recognize()`. Bir bit LINQ almak için kullanırız `Prediction` hareketi yüksek puanını sahip.

    Eşleşme varsa yeterli puan ile yüksek hareket sonra herhangi bir şey yapmadan olay işleyicisi çıkar. Aksi durumda biz tahmin adını denetleyin ve görüntülenen görüntünün hareketi adına göre değiştirin:

    ```csharp
    private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
    {
        IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
        orderby p.Score descending
        where p.Score > 1.0
        select p;
        Prediction prediction = predictions.FirstOrDefault();

        if (prediction == null)
        {
            Log.Debug(GetType().FullName, "Nothing seemed to match the user's gesture, so don't do anything.");
            return;
        }

        Log.Debug(GetType().FullName, "Using the prediction named {0} with a score of {1}.", prediction.Name, prediction.Score);

        if (prediction.Name.StartsWith("checkmark"))
        {
            _imageView.SetImageResource(Resource.Drawable.checked_me);
        }
        else if (prediction.Name.StartsWith("erase", StringComparison.OrdinalIgnoreCase))
        {
            // Match one of our "erase" gestures
            _imageView.SetImageResource(Resource.Drawable.check_me);
        }
    }
    ```

-   Uygulamayı çalıştırın ve özel hareketi tanıyıcı faaliyeti başlatın. Aşağıdaki ekran görüntüsüne benzer görünmelidir:

    [![Ekran bana görüntü denetleyin](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Şimdi bir onay işareti ekranda çizme ve görüntülenmesini bit eşlemi, sonraki ekran görüntülerinde gösterildiği gibi görünmelidir:

    [![Çizilen onay, onay işaretine tanınmıyor](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    Son olarak, bir karalama ekranda çizin. Bu ekran görüntülerinde gösterildiği gibi onay kutusunu özgün görüntüye değiştirmeniz gerekir:

    [![Karalama ekranında, özgün resim görüntülenir](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Artık bir anlayış dokunma ve hareket uygulamada Xamarin.Android kullanarak Android tümleştirme vardır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android Touch başlatın (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch son (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
