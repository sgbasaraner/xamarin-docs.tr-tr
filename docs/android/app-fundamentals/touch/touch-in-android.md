---
title: Android dokunma
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: f1ccc86a20cb441bfdda864b8c7e263a691f5ab7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767498"
---
# <a name="touch-in-android"></a>Android dokunma

Çok gibi iOS, Android kullanıcının fiziksel ekran etkileşim hakkındaki verileri tutan bir nesne oluşturur &ndash; bir `Android.View.MotionEvent` nesnesi. Bu nesne ne gerçekleştirilir gibi verileri tutar, dokunma sürdü burada yerleştirin, ne kadar baskısı uygulandı, vb. A `MotionEvent` nesnesi, aşağıdaki değerleri taşımayı böler:

-  İlk dokunma, ekran ya da touch bitiş arasında taşıma dokunma gibi hareket türünü tanımlayan bir eylem kodu.

-  Bir konumu açıklayan eksen değerleri kümesi `MotionEvent` ve diğer taşıma özellikleri dokunma yer aldığı Burada, dokunma gerçekleşen ne zaman ve ne kadar baskısı kullanıldı.
   Önceki listede tüm eksen değerleri tanımlamaz şekilde eksen değerleri cihaza bağlı olarak farklı olabilir.


`MotionEvent` Nesnesi bir uygulamada uygun bir yöntem geçirilir. Bir touch olayına yanıt olarak bir Xamarin.Android uygulaması için üç yolu vardır:

-  *Bir olay işleyicisi atamak `View.Touch`*  - `Android.Views.View` sınıfına sahip bir `EventHandler<View.TouchEventArgs>` hangi uygulamalar için bir işleyici atayabilirsiniz. Tipik .NET davranış budur.

-  *Uygulama `View.IOnTouchListener`*  -bu arabirimi örneklerini görünümünü kullanarak bir görünüm nesnesine atanmış. `SetOnListener` yöntem. Bu bir olay işleyicisi atama için işlevsel olarak eşdeğerdir `View.Touch` olay. Touched olduklarında, birçok farklı görünümleri gereken bazı ortak veya paylaşılan mantığı ise, bir sınıf oluşturun ve daha her görünümün kendi olay işleyicisi atamak için bu yöntemi uygulaması daha verimli olacaktır.

-  *Geçersiz kılma `View.OnTouchEvent`*  -Android alt tüm görünümlerde `Android.Views.View`. Bir görünüm işlemdeki, Android çağıracak `OnTouchEvent` ve onu bir `MotionEvent` nesnesini parametre olarak.


> [!NOTE]
> Tüm Android aygıtlar dokunmatik ekranlar desteklemez. 

Bildirim dosyanızı şu etiket ekleme Google Play için yalnızca görünen dokunma etkinleştirilmiş olan aygıtları uygulamanıza neden olur:

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>Hareketleri

Bir hareketi dokunmatik ekran çizilmiş şekli ' dir. Hareket, bir veya daha fazla vuruşları bir dizi farklı bir ekrana sahip iletişim noktası tarafından oluşturulan noktaları oluşan her vuruşun olabilir. Android birçok farklı türlerinden hareketleri, ekran boyunca basit fling çok dokunma içeren karmaşık hareketleri için destekleyebilir.

Android sağlar `Android.Gestures` özellikle yönetme ve hareketleri için yanıt için ad alanı. AT tüm hareketleri Kalp olduğu adlı özel bir sınıf `Android.Gestures.GestureDetector`. Adından da anlaşılacağı gibi bu sınıfın hareketleri ve temel olayları dinler `MotionEvents` işletim sistemi tarafından sağlanan.

Hareketi algılayıcısı uygulamak için bir etkinlik oluşturmalıdır bir `GestureDetector` sınıfı ve örneği sağlayan `IOnGestureListener`tarafından aşağıdaki kod parçacığında gösterildiği gibi:

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

Bir etkinlik de OnTouchEvent uygulamak ve MotionEvent hareketi algılayıcısı geçmesi gerekir. Aşağıdaki kod parçacığını Bu örneği gösterilmektedir:

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

Bir örneği olduğunda `GestureDetector` bir hareketi tanımlayan ilgi, bu etkinlik veya uygulama bir olay oluşturma veya tarafından sağlanan bir geri çağırma yoluyla bildirir `GestureDetector.IOnGestureListener`.
Bu arabirim için çeşitli hareketleri altı yöntemleri sağlar:

-  *OnDown* -bir dokunun oluşur ancak serbest bırakılmaz olduğunda çağrılır.

-  *OnFling* -bir fling oluşur ve veri olayı tetikleyen başlangıç ve bitiş dokunma sağlayan olduğunda çağrılır.

-  *OnLongPress* -uzun tuşuna oluştuğunda çağrılır.

-  *OnScroll* -kaydırma olay oluştuğunda çağrılır.

-  *OnShowPress* - bir OnDown gerçekleştikten sonra ve taşıma adlı veya olayı gerçekleştirilemiyor.

-  *OnSingleTapUp* -tek bir dokunun oluştuğunda çağrılır.


Çoğu durumda uygulamalar yalnızca hareketleri bir kısmı ilgisini. Bu durumda, uygulamaları GestureDetector.SimpleOnGestureListener sınıfını genişletir ve gerekir olaylara karşılık gelen yöntemleri geçersiz kılmak, ilgi duyan.

## <a name="custom-gestures"></a>Özel hareketler

Hareketleri, bir uygulama ile etkileşim kullanıcılar için harika bir yoludur. Anlatıldığı kadarki API'leri için basit hareketler yeterli, ancak daha karmaşık hareketleri için biraz onerous kanıtlamak. İle daha karmaşık hareketleri yardımcı olmak için başka bir API'nin kümesinde, bazı özel hareketler ile ilişkili yük kolaylaştıracak Android.Gestures ad Android sağlar.

### <a name="creating-custom-gestures"></a>Özel hareketler oluşturma

Android 1.6 itibaren Android SDK hareketleri Oluşturucu adlı öykünücüsünde önceden yüklenmiş bir uygulama ile birlikte gelir. Bu uygulamayı bir uygulamaya katıştırılabilir önceden tanımlanmış hareketleri oluşturmak için geliştirici sağlar. Aşağıdaki ekran görüntüsünde hareketleri oluşturucu örneği gösterilmektedir:

[![Örnek hareketlerle hareketleri Oluşturucu ekran görüntüsü](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

Google Play hareketi aracı adlı bu uygulamanın geliştirilmiş bir sürümü bulunamadı. Oluşturulduktan sonra hareketleri test etmenizi sağlar ancak bu hareketi hareketleri Oluşturucu gibi çok aracıdır. Bu sonraki ekran hareketlerini Oluşturucu gösterir:

[![Örnek hareketlerle hareketi aracı ekran görüntüsü](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

Hareketi aracı bunlar oluşturuldukça sınanacak hareketleri verdiğinden özel hareketler oluşturmak için biraz daha yararlı olur ve Google Play kolayca yüklenebilir.

Ekranda çizim ve bir ad atama hareket oluşturma hareketi araç sağlar. Hareketleri oluşturduktan sonra cihazın SD kart ikili dosya kaydedildi. Bu dosya, aygıttan alınan ve klasör /Resources/raw bir uygulamayla paketlenmiş gerekiyor. Bu dosya Android hata ayıklama köprüsü kullanarak öykünücüsünden alınabilir. Aşağıdaki örnek, bir uygulamanın kaynak dizinine Galaxy Nexus dosya kopyalama gösterir:

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

Dosyayı aldıktan sonra onu olmalıdır uygulamanızla dizin /Resources içinde paketlenmiş/ham. Aşağıdaki kod parçacığında gösterildiği gibi bir GestureLibrary dosyasını yüklemek için bu hareketi dosyayı kullanmak için en kolay yolu olur:

```csharp
GestureLibary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>Özel hareketler kullanma

Özel hareketler bir etkinlikte tanımak için kendi düzenine eklenen bir Android.Gesture.GestureOverlay nesnesi olmalıdır. Aşağıdaki kod parçacığını programlı olarak bir GestureOverlayView bir etkinliğe nasıl ekleneceğini gösterir:

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

Aşağıdaki XML parçacığını bir GestureOverlayView bildirimli olarak eklemeyi gösterir:

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

`GestureOverlayView` Hareket çizim işlemi sırasında gerçekleştirilecektir çeşitli olayları vardır. En ilginç olay `GesturePeformed`. Kullanıcı kendi hareketi çizim tamamlandığında bu olay tetiklenir.

Bu olay oluştuğunda, etkinlik soran bir `GestureLibrary` deneyin ve kullanıcı hareketlerini biri ile oluşturulan hareketi hareketi aracı tarafından eşleştirmek için. `GestureLibrary` Tahmin nesnelerin bir listesini döndürür.

Her tahmin nesnesi bir puan ve hareketleri birinin adını tutar `GestureLibrary`. Daha yüksek puanı, büyük olasılıkla tahmin adlı hareketi kullanıcı tarafından çizilmiş hareketi eşleşir.
Genel olarak bakıldığında, puanları 1.0 düşük, düşük eşleşme olarak kabul edilir.

Aşağıdaki kod bir hareketi eşleşen bir örnek gösterilmektedir:

```csharp
private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
{
    // In this example _gestureLibrary was instantiated in OnCreate
    IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
    orderby p.Score descending
    where p.Score > 1.0
    select p;
    Prediction prediction = predictions.FirstOrDefault();

    if (prediction == null)
    {
        Log.Debug(GetType().FullName, "Nothing matched the user's gesture.");
        return;
    }

    Toast.MakeText(this, prediction.Name, ToastLength.Short).Show();
}
```

Bu ile yapılır, bir Xamarin.Android uygulaması dokunma ve hareket kullanmak nasıl bir anlamış olmanız gerekir. Bize şimdi bir kılavuz taşıyın ve tüm çalışan bir örnek uygulama açıklanan kavramlar bakın.



## <a name="related-links"></a>İlgili bağlantılar

- [Android Touch başlatın (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch son (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
