---
title: İOS dokunma
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 9ed90a9c4ddcd398d834cb8c91553a57e7bd5ad8
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689547"
---
# <a name="touch-in-ios"></a>İOS dokunma

Tüm fiziksel aygıt etkileşim için merkezi olarak dokunma olaylarına anlamak ve bir iOS uygulaması API'leri touch önemlidir. Tüm dokunmalı etkileşimler içeren bir `UITouch` nesnesi. Bu makalede biz nasıl kullanılacağını öğreneceksiniz `UITouch` sınıfı ve API'lerini dokunma desteklemek için. Daha sonra biz hareketleri desteği hakkında bilgi edinmek için bizim bilgi genişletilir.

## <a name="enabling-touch"></a>Dokunma etkinleştirme

Denetimlerini `UIKit` – bu altsınıflanmış UI denetim gelen – bunu Uıkit için yerleşik hareketleri sahip oldukları kullanıcı etkileşimi bağlıdır ve bu nedenle dokunma etkinleştirmek gerekli değildir. Zaten etkinleştirilmiş.

Ancak, görünümleri birçoğu `UIKit` varsayılan olarak etkin dokunma sahip değil. Dokunma bir denetimi etkinleştirmek için iki yolu vardır. İlk yolu kullanıcı etkileşimi etkin Tasarımcısı, iOS özelliği panelinde onay aşağıdaki ekran görüntüsünde gösterildiği gibi şudur:

 [![](touch-in-ios-images/image1.png "İOS Tasarımcısı özelliği panelinde kullanıcı etkileşimi Etkin onay kutusunu işaretleyin")](touch-in-ios-images/image1.png#lightbox)

Biz de bir denetleyici ayarlamak için kullanabileceğiniz `UserInteractionEnabled` özelliğinin true üzerinde bir `UIView` sınıfı. Kullanıcı arabirimini kodda oluşturduysanız, bu gereklidir.

Aşağıdaki kod satırını bir örnek verilmiştir:

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>Dokunma olayları

Dokunma, kullanıcının ekrana dokunur, kendi parmak taşıdığında veya kendi parmak kaldırır oluşan üç aşama vardır. Bu yöntemler tanımlanan `UIResponder`, UIView için temel sınıfı olan. iOS üzerinde ilişkili yöntemleri kılar `UIView` ve `UIViewController` dokunma işlemek için:

-  `TouchesBegan` – Ekran ilk işlemdeki bu denir.
-  `TouchesMoved` – Dokunma değişiklikleri konumu kullanıcı olarak kendi parmakları ekran kayan bu denir.
-  `TouchesEnded` veya `TouchesCancelled` – `TouchesEnded` kullanıcının parmakları ekranı kaldırılmış olduğunda çağrılır.  `TouchesCancelled` Örneğin, iOS – dokunma iptal ederse, bir kullanıcı bir tuşuna iptal etmek için bir düğmeye çıktığınızda bilgilendirilmesine parmak slayt varsa çağrılır.


Dokunma olay bir görünüm nesnesi sınırları içinde olup olmadığını denetlemek için UIViews yığınını aracılığıyla olayları seyahat yinelemeli touch. Bu genellikle adlandırılır _isabet testi_. Bunlar ilk üstteki üzerinde çağrılacağı `UIView` veya `UIViewController` ve ardından üzerinde çağrılacağı `UIView` ve `UIViewControllers` bunları görünüm hiyerarşide aşağıda.

A `UITouch` nesne kullanıcı ekrana dokunur her zaman oluşturulur. `UITouch` Nesnesi dokunma olduysa geçirme, vb. gibi dokunma, nerede oluştuğunu gerçekleştiği touch hakkındaki verileri içerir. Dokunma olaylarına rötuşları özelliği – geçen bir `NSSet` bir veya daha fazla rötuşları içeren. Biz bir touch başvuru sağlamak için bu özelliği kullanın ve uygulamanın yanıt belirleyin.

Dokunma olaylardan biri geçersiz kılma sınıfları ilk temel uygulamayı çağırması ve daha sonra `UITouch` olayla ilişkili nesne. İlk dokunma başvuru almak için arama `AnyObject` özelliği ve olarak cast bir `UITouch` aşağıdaki örnekte göster:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        //code here to handle touch
    }
}
```

iOS otomatik olarak tanır art arda hızlı ekranda dokunur ve bunları tek bir dokunun olarak toplayacak `UITouch` nesnesi. Bu denetimi olarak kadar kolay bir çift dokunun denetleniyor kılar `TapCount` aşağıdaki kodda gösterildiği şekilde özelliği:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        if (touch.TapCount == 2)
        {
            // do something with the double touch.
        }
    }
}
```

## <a name="multi-touch"></a>Çok dokunma

Çok dokunma denetimlerinde varsayılan olarak etkin değil. Çok dokunma iOS tasarımcısı tarafından aşağıdaki ekran görüntüsünde gösterildiği gibi etkinleştirilebilir:

 [![](touch-in-ios-images/image2.png "İOS Tasarımcısı etkin çok dokunma")](touch-in-ios-images/image2.png#lightbox)

Çok dokunma programlı olarak ayarlamak da mümkündür `MultipleTouchEnabled` aşağıdaki kod satırını gösterildiği gibi özelliği:

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

Kaç tane parmakları ekran işlemdeki belirlemek için `Count` özellikte `UITouch` özelliği:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>Dokunma konumunu belirleme

Yöntem `UITouch.LocationInView` belirli bir görünüm içindeki dokunma koordinatlarını tutan bir CGPoint nesnesi döndürür. Ayrıca, biz yöntemini çağırarak bu konuma denetimin içinde olup olmadığını sınayabilirsiniz `Frame.Contains`. Aşağıdaki kod parçacığını Bu örneği gösterilmektedir:

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

Biz iOS dokunma olayların bir anlama sahip olduğunuza göre hareketi tanıyıcıları hakkında öğrenelim.

## <a name="gesture-recognizers"></a>Hareketi tanıyıcıları

Hareketi tanıyıcıları büyük ölçüde basitleştirebilir ve dokunma uygulamayı desteklemek için programlama çabasını azaltmış olursunuz. iOS hareketi tanıyıcıları tek dokunma olay bir dizi dokunma olay toplama.

Xamarin.iOS sınıfı sağlar `UIGestureRecognizer` aşağıdaki yerleşik hareketi tanıyıcıları için temel sınıf olarak:

-  *UITapGestureRecognizer* – için bir veya daha fazla Tap budur.
-  *UIPinchGestureRecognizer* – Pinching ve birbirinden parmakları yayılmak.
-  *UIPanGestureRecognizer* – kaydırma veya sürükleme.
-  *UISwipeGestureRecognizer* – herhangi bir yönde geçirme.
-  *UIRotationGestureRecognizer* – saat yönünde veya yönünün hareketli iki parmakları döndürme.
-  *UILongPressGestureRecognizer* – basılı tutun, bazen bir uzun basın veya uzun tıklatma olarak adlandırılır.


Hareketi tanıyıcıyı için temel desen aşağıdaki gibidir:

1.  **Hareketi tanıyıcı örneği** – ilk olarak, örneği bir `UIGestureRecognizer` bir alt kümesi. Örneği nesnesi tarafından bir görünümü ilişkilendirilir ve görünümü, çıkarıldığından çöpünün toplanma. Bu görünüm bir sınıf düzeyi değişkeni olarak oluşturmak gerekli değildir.
1.  **Herhangi bir hareketi ayarı yapılandırmak** – hareketi tanıyıcı yapılandırmak için sonraki adımdır. Üzerinde Xamarin'ın belgelerine `UIGestureRecognizer` ve alt sınıfları davranışını denetlemek için ayarlanabilir özelliklerinin bir listesi için bir `UIGestureRecognizer` örneği.
1.  **Hedef yapılandırma** – kendi Objective-C miras nedeniyle Xamarin.iOS hareketi tanıyıcı hareket eşleştiğinde olayları yükseltmek değil.  `UIGestureRecognizer` has yöntemi – `AddTarget` – anonim bir temsilci veya kod Objective-C seçicisiyle hareketi tanıyıcı bir eşleşme bulunduğunda yürütmek için kabul edebilir.
1.  **Hareketi tanıyıcı etkinleştirmek** – dokunmalı etkileşimler etkinleştirildiğinde yalnızca dokunma olaylarla hareketleri yalnızca tanınır gibi.
1.  **Hareketi tanıyıcı görünümüne eklemek** – çağırarak hareketi bir görünüme eklemek için son adımdır `View.AddGestureRecognizer` ve onu bir hareketi tanıyıcı nesnesi.

Başvurmak [tanıyıcı örnekleri hareket](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) koda uygulanması hakkında daha fazla bilgi için.

Hareketi 's hedef çağrıldığında oluştu hareketi başvuru geçirilir. Bu oluştu hareketi hakkında bilgi edinmek hareketi hedef sağlar. Kullanılabilir bilgi kapsamını kullanılan hareketi tanıyıcı türüne bağlıdır. Lütfen her bulunan veriler hakkında bilgi için Xamarin'ın belgelerine bakın `UIGestureRecognizer` bir alt kümesi.

Hareketi tanıyıcı bir görünüme eklendikten sonra Görünüm (ve altındaki herhangi bir görünüm) herhangi bir touch olayı almazlar olduğunu unutmamak önemlidir. Aynı anda hareketlerle dokunma olayları izin veren `CancelsTouchesInView` özelliği aşağıdaki kodda gösterildiği şekilde false olarak ayarlanmalıdır:

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

Her `UIGestureRecognizer` hareketi tanıyıcı durumu hakkında önemli bilgiler sağlayan bir durum özelliğe sahiptir. Bu özelliğin değeri her değiştiğinde iOS bir güncelleştirme vermiş abone yöntemi çağırır. Özel hareketi tanıyıcı State özelliği hiçbir zaman güncelleştirmeleri olursa abone hiçbir zaman, hareketi tanıyıcı gereksiz işleme adı verilir.

Hareketleri iki türden biri olarak özetlenebilir:

1.  *Ayrık* – tanınan bu hareketleri yalnızca yangın ilk zaman.
1.  *Sürekli* – bu hareketleri tanınan sürece harekete geçin.


Hareketi tanıyıcıları şu durumlardan birinde bulunur:

-  *Olası* – bu tüm hareketi tanıyıcıları başlangıçtaki durumdur. State özelliği varsayılan değer budur.
-  *Began* – sürekli hareketi ilk tanınan, durum Began için ayarlanır. Böylece abone olan ne zaman değiştirildiğini ve hareketi tanıma başladığında arasında ayırt etmek için.
-  *Değiştirilen* – sürekli hareketi başladı, ancak tamamlanmadı sonra bir touch taşır veya değişiklikler, her zaman hareketi beklenen parametreleri içinde hala olduğu sürece durumu değiştirme hedefi ayarlanır.
-  *İptal* – bu durum tanıyıcı Began değiştirme hedefi oluştu ve sonra bu tür bir şekilde olarak artık değiştirilmiş rötuşları hareketi desenini sığacak ayarlanır.
-  *Tanınan* – hareketi tanıyıcı rötuşları kümesi ile eşleşen ve abone hareketi tamamlandığını bildirir durumu ayarlanır.
-  *Sona erdi* – Recognized durumu için bir diğer ad budur.
-  *Başarısız* – hareketi tanıyıcı artık dinleme durumu başarısız olarak değiştirildi için rötuşları eşleştirebilirsiniz.


Xamarin.iOS bu değerleri temsil eden `UIGestureRecognizerState` numaralandırması.

## <a name="working-with-multiple-gestures"></a>Birden çok hareketleri ile çalışma

Varsayılan olarak, iOS varsayılan hareketleri aynı anda çalıştırılmasına izin vermiyor. Bunun yerine, her hareketi tanıyıcı belirleyici olmayan bir sırada dokunma olayları alır. Aşağıdaki kod parçacığını aynı anda çalışacak bir hareketi tanıyıcısı yapma gösterilmiştir:

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

İOS içinde hareketi devre dışı bırakmak mümkündür. Bir uygulama ve geçerli dokunma olayları nasıl kararlarında ve hareket tanınan hale getirmek için durumunu incelemek hareketi tanıyıcı izin iki temsilci özellikleri vardır. İki olay şunlardır:

1.  *ShouldReceiveTouch* – sağ hareketi tanıyıcı dokunma olay geçirilir ve rötuşları inceleyin ve hangi rötuşları hareketi tanıyıcı tarafından işlenecek karar vermek için bir fırsat sunar önce bu temsilci çağrılır.
1.  *ShouldBegin* – bir tanıyıcı durumu başka bir duruma olası değiştirme girişiminde bulunduğunda çağrılır. False değerinin döndürülmesi durumu başarısız olarak değiştirilecek hareketi tanıyıcı zorlar.


Bu yöntemlerle kesin türü belirtilmiş kılabilirsiniz `UIGestureRecognizerDelegate`, zayıf temsilci veya bağlama aracılığıyla tarafından aşağıdaki kod parçacığında gösterildiği gibi olay işleyici sözdizimi:

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

Son olarak, başka bir hareketi tanıyıcı başarısız olursa, yalnızca başarılı olur hareketi tanıyıcı kuyruğuna mümkündür. Örneğin, bir çift dokunun hareketi tanıyıcısı başarısız olduğunda tek dokunun hareketi tanıyıcı yalnızca başarılı olmalıdır. Aşağıdaki kod parçacığını bunun bir örneğini sağlar:

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>Özel bir hareketi oluşturma

İOS bazı varsayılan hareketi tanıyıcıları sağlasa da, bazı durumlarda özel hareketi tanıyıcıları oluşturmak gerekli olabilir. Bir özel hareketi tanıyıcısı oluşturma, aşağıdaki adımları içerir:

1.  Bir alt kümesi `UIGestureRecognizer` .
1.  Uygun dokunma olay yöntemleri geçersiz kılın.
1.  Taban sınıfı durumu özelliği aracılığıyla tanıma durumu Kabarcık.


İçinde bu pratik bir örneği ele alınacaktır [kullanarak Touch iOS içinde](ios-touch-walkthrough.md) gözden geçirme.
