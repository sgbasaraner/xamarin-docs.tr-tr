---
title: İzlenecek yol - Android'de dokunma kullanma
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/09/2018
ms.openlocfilehash: d379630e3b7fa2b42bd9530e1dccd75e9634dd2f
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935533"
---
# <a name="walkthrough---using-touch-in-android"></a>İzlenecek yol - Android'de dokunma kullanma

Bize kavram çalışan bir uygulama önceki bölümde kullanmak konusuna bakın. Dört etkinliği ile bir uygulama oluşturacağız. İlk etkinlik, bir menü veya bir geçiş çeşitli API'leri göstermek için diğer etkinlikleri başlatacak Panosu olacaktır. Ana etkinlik aşağıdaki ekran görüntüsünde gösterilmektedir:

[![Örnek ekran Touch bana düğmesi](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

Touch örnek, ilk etkinliğin görünümleri temas için olay işleyicileri kullanmayı gösterir. Hareket tanıyıcı etkinlik gösterilecektir nasıl alt Sınıflama `Android.View.Views` ve olaylarını işleme yanı sıra nasıl işleyeceğinizi tabletinizde hareketlerini gösterir. Üçüncü ve son etkinlik **özel hareket**, show nasıl özel hareketler kullanır. İşleri izleyin ve etkisini azaltmak kolaylaştırmak için Biz bu izlenecek yol etkinliklerden birini odaklanarak her bölüm içeren bölümlere ayırmak.

## <a name="touch-sample-activity"></a>Örnek etkinlik dokunma

-   Projeyi açmak **TouchWalkthrough\_Başlat**. **MainActivity** gitmek için tüm kümesi &ndash; bize etkinliğinde touch davranışı uygulamak için en fazla olan. Uygulamayı çalıştırmak ve'ı tıklatırsanız **Touch örnek**, aşağıdaki etkinlik başlamanız gerekir:

    [![Ekran görüntüsü etkinlik Touch görüntülenip başlar](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Biz, etkinlik başlar onayladıktan sonra dosyayı açmak **TouchActivity.cs** ve eklemek için bir işleyici `Touch` olayı `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Ardından, aşağıdaki yöntemi ekleyin **TouchActivity.cs**:

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

Yukarıdaki kodda fark kabul ediyoruz `Move` ve `Down` eylem olarak aynı. Olsa bile kullanıcı kendi parmağınızı kaldırın değil olmasıdır `ImageView`, onu yerleri veya tarafından kullanıcı sarf baskısı değişebilir. Bu tür değişiklikleri oluşturacak bir `Move` eylem.

Her zaman kullanıcı dokunmalar `ImageView`, `Touch` olay yükseltilir ve bizim işleyici iletisini görüntüler **Touch başlar** ekranında, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Ekran görüntüsü etkinlik Touch başlar](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Kullanıcı temas sürece `ImageView`, **Touch başlar** görüntülenmesi `TextView`. Ne zaman kullanıcı artık dokunma `ImageView`, ileti **Touch sona erer** görüntülenmesi `TextView`aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Ekran görüntüsü etkinlik Touch sona erer](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Hareket tanıyıcı etkinliği

Artık hareket tanıyıcı etkinlik uygulayan olanak tanır. Bu etkinlik ekranın etrafında bir görünüm sürükleyin ve tabletinizde sıkıştırarak yakınlaştırma hareketini kullanarak uygulamak için bir şekilde açıklamak nasıl sürdürebileceğiniz gösterilecek.

-   Yeni bir etkinlik olarak adlandırılan uygulamaya ekleme `GestureRecognizer`.
    Bu etkinlik için kod, aşağıdaki koda benzer şekilde düzenleyin:

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

-   Yeni bir Android ekleme görüntülemek için projeyi ve adlandırın `GestureRecognizerView`. Bu sınıfa aşağıdaki değişkenleri ekleyin:

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

-   Aşağıdaki oluşturucuyu ekleyin `GestureRecognizerView`. Bu oluşturucu ekleyecek bir `ImageView` bizim etkinlik. Bu noktada kod hala derlenmez &ndash; sınıfı oluşturmak ihtiyacımız `MyScaleListener` yeniden boyutlandırma ile yardımcı olacak `ImageView` zaman kullanıcı pinches onu:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Resim bizim faaliyete çizmek için geçersiz kılmak ihtiyacımız `OnDraw` yöntemini aşağıdaki kod parçacığında gösterildiği gibi görünüm sınıfı. Bu kod taşınır `ImageView` tarafından belirtilen konuma `_posX` ve `_posY` yeniden görüntü göre Ölçeklendirme çarpanı:

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

-   Sonraki örnek değişkeni güncelleştirmek ihtiyacımız `_scaleFactor` kullanıcı olarak pinches `ImageView`. Adlı bir sınıf ekleyeceğiz `MyScaleListener`. Bu sınıf kullanıcı pinches zaman Android tarafından gerçekleştirilecektir ölçek olayları dinleyecek `ImageView`.
    Aşağıdaki iç sınıfa eklemek `GestureRecognizerView`. Bu sınıf, bir `ScaleGesture.SimpleOnScaleGestureListener`. Bu sınıf hareket alt kümesinde ilgilendiğiniz zaman dinleyicileri öğesinin alt sınıfı için bir kolaylık sınıfı şöyledir:

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

            _view.Invalidate();
            return true;
        }
    }
    ```

-   Sonraki yöntem, geçersiz kılmak için ihtiyacımız `GestureRecognizerView` olduğu `OnTouchEvent`. Aşağıdaki kodu tam uygulamayı bu yöntemin listeler. Birçok kod burada, bu nedenle bir dakikanızı ayırın ve burada neler olduğuna bakın sağlar. Bu yöntem yapacağı ilk şey simgesi gerekirse ölçeği olan &ndash; bu çağırarak işlenir `_scaleDetector.OnTouchEvent`. Sonraki Biz bu yöntemin hangi eylemin çekilerek şekil deneyin:

    - Kullanıcı ekranı dokunulan, X ve Y konumları ve ekran dokunulan ilk işaretçi Kimliğini kaydedin.

    - Daha sonra kullanıcı, dokunmatik ekranda taşıdıysanız, kullanıcı işaretçiyi taşınmış ne kadar şekil.

    - Kullanıcının kendi parmak ekranın yükseltilmiş, ardından biz hareketlerini izlemeyi durdurur.

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

-   Şimdi uygulamayı çalıştırın ve hareket tanıyıcı etkinliği başlatın.
    Başladığında Ekran aşağıdaki ekran görüntüsüne benzer görünmelidir:

    [![Hareket tanıyıcı başlangıç ekranı ile Android simgesi](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Artık dokunma simgesi ve ekranın istediğiniz yere sürükleyin. Tabletinizde sıkıştırarak yakınlaştırma hareketini deneyin. Belirli bir noktada ekranınız aşağıdaki ekran görüntüsünde, şuna benzer:

    [![Ekran hareketlerini taşıma simgesi](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

Bu noktada, kendinize bir pat arkasında vermeniz gerekir: bir Android uygulamasında yalnızca tabletinizde sıkıştırarak yakınlaştırma hareketini kullanarak uyguladıysanız! Hızlı sonu olması ve bu kılavuzda yer alan üçüncü ve son etkinliğe geçin sağlar &ndash; özel hareketler kullanarak.

## <a name="custom-gesture-activity"></a>Özel hareket etkinliği

Bu kılavuzdaki son ekran özel hareketler kullanır.

Bu izlenecek yolda amacı doğrultusunda, hareketlerini kitaplığı zaten hareket aracını kullanarak oluşturulduğundan ve proje dosyasına ekleneceği **kaynakları/ham/hareketlerini**. Son gözden geçirme etkinlik alma göz önünden housekeeping bu bit ile sağlar.

-   Adlı bir düzen dosyası ekleme **özel\_hareket\_layout.axml** projeye aşağıdaki içeriğe sahip. Proje zaten tüm görüntüleri sahip **kaynakları** klasörü:

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

-   Ardından, projeye yeni bir etkinlik ekleyin ve adlandırın `CustomGestureRecognizerActivity.cs`. İki örnek değişkenleri sınıfına aşağıdaki iki kod satırlarını gösterildiği gibi ekleyin:

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Düzen `OnCreate` yöntemi bu onun aşağıdaki koda benzer şekilde etkinlik. Bu kodda neler olduğunu açıklamak için bir dakikanızı ayırın olanak tanır. Yapmamız gereken ilk şey örneği olan bir `GestureOverlayView` ve etkinliğin kök görünümü olarak ayarlayın.
    Biz de bir olay işleyicisi atama `GesturePerformed` olayı `GestureOverlayView`. Sonraki biz Şişir daha önce oluşturulan Düzen dosyası ve bir alt görünüm olarak eklemek `GestureOverlayView`. Değişkeni başlatmak için son adımdır `_gestureLibrary` hareketlerini dosyasını ve uygulama kaynaklarından yükleyin. Hareketlerini dosyası herhangi bir nedenden dolayı yüklenemiyor, yok çok bu etkinlik yapabilirsiniz, kapatma, bu nedenle:

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

-   Yöntemi uygulamak için ihtiyacımız olan son bir şey `GestureOverlayViewOnGesturePerformed` aşağıdaki kod parçacığında gösterildiği gibi. Zaman `GestureOverlayView` bir hareketi algılar geri bu yöntemi çağırır. Çalıştığımızda almak için ilk şey bir `IList<Prediction>` hareket çağırarak eşleşen nesneleri `_gestureLibrary.Recognize()`. LINQ biraz almak için kullandığımız `Prediction` hareketi için en yüksek puanı sahip.

    Eşleşme varsa yeterli puanı ile yüksek hareket ve olay işleyicisi, hiçbir şey yapmadan çıkar. Aksi takdirde biz tahmin adını kontrol edin ve hareket adını temel alarak görüntülenen resmi değiştirmek:

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

-   Uygulamayı çalıştırmak ve özel hareket tanıyıcı faaliyeti başlatın. Aşağıdaki ekran görüntüsü gibi görünmelidir:

    [![Ekran bana görüntü denetleyin](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Artık bir onay işareti ekranda çizim ve görüntülenmesini bit eşlemi, sonraki ekran görüntülerinde gösterildiği gibi görünmelidir:

    [![Çizilen onay işareti, onay işareti değerlendirilmiştir](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    Son olarak, bir karalama ekranda çizin. Bu ekran görüntülerinde gösterildiği gibi onay kutusunu özgün görüntüye değiştirmeniz gerekir:

    [![Ekranında, özgün resmin karalama görüntülenir](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Artık dokunma ve Xamarin.Android kullanarak bir Android uygulaması hareketleri tümleştirmek nasıl bir anlayışa sahipsiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Android Touch Başlat (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch son (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
