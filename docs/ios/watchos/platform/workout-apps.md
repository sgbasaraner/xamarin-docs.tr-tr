---
title: Etkinlik uygulamalar
description: "Bu makalede yenilikleri kapsayan Apple watchOS 3 ve Xamarin uygulamak nasıl etkinlik uygulamalarında yaptı."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 2282a340811d9932f9df3a1343b22ffc35247e54
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="workout-apps"></a>Etkinlik uygulamalar

_Bu makalede yenilikleri kapsayan Apple watchOS 3 ve Xamarin uygulamak nasıl etkinlik uygulamalarında yaptı._


Yeni watchOS 3 için etkinlik uygulamalar üzerinde Apple Watch arka planda çalışan ve HealthKit verilerine erişim olanağı ilişkili. Kendi üst iOS 10 tabanlı uygulama da kullanıcı müdahalesi olmadan dayalı watchOS 3 uygulamasını başlatmak için özelliğine sahiptir.

Aşağıdaki konularda ayrıntılı olarak ele alınacaktır:

## <a name="about-workout-apps"></a>Etkinlik uygulamaları hakkında

Sistem durumu ve uygunluk hedeflerine doğru günün birkaç saat devoting uygunluk ve etkinlik uygulamalar kullanıcılarının yüksek oranda ayrılmış olabilir. Sonuç olarak, doğru olarak toplamak ve verileri görüntülemek ve Apple Health ile sorunsuz tümleştirme esnek, kullanımı kolay uygulamalar bekler.

İyi tasarlanmış bir uygunluk veya etkinlik uygulamasının uygunluk hedeflerine ulaşmak için bunların etkinliklerini grafik kullanıcıların yardımcı olur. Apple Watch kullanarak uygunluk ve etkinlik uygulamalar anlık Kalp oranı erişimi kalori yazma ve etkinlik algılama.

[![](workout-apps-images/workout01.png "Uygunluk ve etkinlik uygulama örneği")](workout-apps-images/workout01.png#lightbox)

WatchOS 3, yeni _arka çalışan_ verir etkinlik ilgili uygulamalar üzerinde Apple Watch arka planda çalışan ve HealthKit verilerine erişim olanağı.

Bu belgenin arka çalışan özelliği tanıtmak, etkinlik uygulama yaşam döngüsü kapsar ve etkinlik uygulama kullanıcının nasıl katkıda bulunabilirsiniz Göster _etkinlik çalma_ Apple Watch üzerinde.

## <a name="about-workout-sessions"></a>Etkinlik oturumları hakkında

Her etkinlik uygulamanın Kalp olan bir _etkinlik oturum_ (`HKWorkoutSession`), kullanıcı Başlat ve durdur. Etkinlik oturum API uygulanması kolaydır ve gibi bir etkinlik uygulaması için çeşitli avantajları sağlar:

- Hareket ve kalori etkinlik türüne göre algılama yazma.
- Kullanıcının etkinlik çalma otomatik katkısı.
- Kullanıcı aygıt (ya da kendi bileğe kadar oluşturma veya Apple Watch ile etkileşim) uyku modundan her bir oturumu sırasında uygulama otomatik olarak görüntülenir.

## <a name="about-background-running"></a>Arka plan çalışmasını hakkında

Yukarıda belirtildiği gibi watchOS 3 ile arka planda çalıştırmak için bir etkinlik uygulaması ayarlanabilir. Arka planda çalışan bir etkinlik uygulaması kullanarak, Apple Watch ın algılayıcı verilerinden arka planda çalıştığı sırada işleyebilir. Örneğin, artık ekranda görüntülenen olsa bile bir uygulama kullanıcının Kalp oranı, izlemek devam edebilirsiniz.

Arka plan çalışmasını etkin etkinlik kendi geçerli ilerleme kullanıcıyı bilgilendirmek üzere haptic bir uyarı göndermek gibi oturumu sırasında herhangi bir zamanda kullanıcıya Canlı geri bildirim sunmak üzere özelliği de sağlar.

Ayrıca, arka plan çalıştıran kullanıcı bunlar kendi Apple Watch Hızlı bakış en son verileri varken, kullanıcı arabirimi hızlı bir şekilde güncelleştirin yazmasına izin verir.

Apple Watch yüksek performansı korumak için arka plan çalıştıran kullanarak bir izleme uygulaması pil korumak için arka plan iş miktarını sınırlamanız gerekir. Bir uygulama sırasında arka planda aşırı CPU kullanması durumunda watchOS tarafından askıya.

### <a name="enabling-background-running"></a>Çalıştıran arka plan etkinleştirme

Arka plan çalışmasını etkinleştirmek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, izleme uzantının Yardımcısı iPhone uygulamanın çift `Info.plist` dosyayı düzenlemek için açın.
2. Geçiş **kaynak** görünümü: 

    [![](workout-apps-images/plist01.png "Kaynak Görünümü")](workout-apps-images/plist01.png#lightbox)
3. Adlı yeni bir anahtar ekleyin `WKBackgroundModes` ve **türü** için `Array`: 

    [![](workout-apps-images/plist02.png "WKBackgroundModes adlı yeni bir anahtar ekleyin")](workout-apps-images/plist02.png#lightbox)
4. Dizi ile yeni bir öğe eklemek **türü** , `String` ve değerini `workout-processing`: 

    [![](workout-apps-images/plist03.png "Dizi dize türünde ve etkinlik işleme değeri ile yeni bir öğe ekleyin")](workout-apps-images/plist03.png#lightbox)
5. Değişiklikleri dosyaya kaydedin.

## <a name="starting-a-workout-session"></a>Bir etkinlik oturumu başlatılıyor

Bir etkinlik oturum açması için üç ana adım vardır:

[![](workout-apps-images/workout02.png "Bir etkinlik oturum açması için üç ana adım")](workout-apps-images/workout02.png#lightbox)

1. Uygulama yetkilendirme HealthKit erişim verilerde istemeniz gerekir.
2. Başlatılmakta etkinlik türü için bir etkinlik yapılandırma nesnesi oluşturun.
3. Oluşturun ve yeni oluşturulan etkinlik Yapılandırması'nı kullanarak bir etkinlik oturumu başlatın.

### <a name="requesting-authorization"></a>İstekte bulunan Yetkilendirme

Bir uygulama kullanıcının HealthKit verilere erişmeden önce istemek ve kullanıcıdan yetkilendirme almak gerekir. Etkinlik uygulama yapısına bağlı olarak aşağıdaki istek türlerini yapabilir:

- Veri yazmak için yetkilendirme:
    - Egzersizleriniz
- Verileri okumak için yetkilendirme:
    - Yakılan enerji
    - uzaklık
    - Kalp oranı  

Uygulama yetkilendirme isteyebilmesi HealthKit erişmek için yapılandırılması gerekiyor.

Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Entitlements.plist` dosyayı düzenlemek için açın.
2. Alt kısmına kaydırın ve denetleme **etkinleştirmek HealthKit**: 

    [![](workout-apps-images/auth01.png "Denetimi etkinleştir HealthKit")](workout-apps-images/auth01.png#lightbox)
3. Değişiklikleri dosyaya kaydedin.
4. ' Ndaki yönergeleri izleyin [açık uygulama kimliği ve sağlama profili](~/ios/platform/healthkit.md) ve [uygulama kimliği ve sağlama profili ile Xamarin.iOS uygulamanızı ilişkilendirme](~/ios/platform/healthkit.md) bölümlerini [giriş HealthKit](~/ios/platform/healthkit.md) doğru uygulama sağlamak için makale.
5. Son olarak, konusundaki yönergeleri kullanın [programlama durumu Seti](~/ios/platform/healthkit.md) ve [kullanıcının izin isteyen](~/ios/platform/healthkit.md) bölümlerini [HealthKit giriş](~/ios/platform/healthkit.md) makalesi isteği kullanıcının HealthKit veri deposuna erişmek için yetkilendirme.

### <a name="setting-the-workout-configuration"></a>Etkinlik yapılandırma ayarı

Etkinlik oturumları, bir etkinlik yapılandırma nesnesi kullanarak oluşturulur (`HKWorkoutConfiguration`) etkinlik türünü belirtir (gibi `HKWorkoutActivityType.Running`) ve etkinlik konumu (gibi `HKWorkoutSessionLocationType.Outdoor`):

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Bir etkinlik oturum temsilci oluşturma 

Bir etkinlik oturumu sırasında oluşabilecek olayları işlemek için bir etkinlik oturum temsilci örneği oluşturmak uygulama gerekir. Projeye yeni bir sınıf ekleyin ve onu kapatarak temel `HKWorkoutSessionDelegate` sınıfı. Bir dış çalışma örneği için aşağıdaki gibi görünebilir:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }
        #endregion
    }
}
```

Bu sınıf, etkinlik oturum değişiklikleri durumu olarak gerçekleştirilecektir çeşitli olaylarını oluşturur (`DidChangeToState`) ve etkinlik oturum başarısız olursa (`DidFail`). 

### <a name="creating-a-workout-session"></a>Etkinlik oturum oluşturma

Etkinlik yapılandırması ve etkinlik oturum temsilci kullanarak yukarıda yeni bir etkinlik oturum oluşturun ve kullanıcının varsayılan HealthKit depo başlatmak için oluşturduğunuz:

```csharp
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

private void StartOutdoorRun ()
{
    // Create a workout configuration
    var configuration = new HKWorkoutConfiguration () {
        ActivityType = HKWorkoutActivityType.Running,
        LocationType = HKWorkoutSessionLocationType.Outdoor
    };

    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (configuration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Uygulama bu etkinlik oturumu başlatır ve bunların izleme yüze kullanıcı anahtarları, bir küçük yeşil "adam çalışır" simge karşılaştığı görüntülenecek:

[![](workout-apps-images/workout03.png "Nominal bir küçük yeşil çalışan adam simgesi görüntülenir")](workout-apps-images/workout03.png#lightbox)

Kullanıcı bu simgeyi dokunur, uygulamasına geri götürülürsünüz.

## <a name="data-collection-and-control"></a>Veri toplama ve Denetim

Bir etkinlik oturumu yapılandırılmış ve başlatılan sonra oturumu (örneğin, kullanıcının Kalp hızı) hakkında veri toplamak ve oturum durumunu denetlemek uygulama gerekir:

[![](workout-apps-images/workout04.png "Veri toplama ve Denetim Diyagramı")](workout-apps-images/workout04.png#lightbox)

1. **Örnekleri Gözlemleme** -uygulama, üzerinde işlem ve kullanıcıya görüntülenen HealthKit bilgi almak gerekir.
2. **Olayları Gözlemleme** -uygulama HealthKit veya uygulamanın kullanıcı Arabirimi (örneğin, etkinlik duraklatma kullanıcı) oluşturulan olaylar yanıt vermesi gerekir.
3. **Çalışma durumu girin** -oturum başlatıldı ve şu anda çalışıyor.
4. **Duraklatıldı durumuna girmesini** -kullanıcı geçerli etkinlik oturum duraklatıldı ve daha sonraki bir tarihte yeniden başlatabilirsiniz. Kullanıcı birkaç kez tek bir etkinlik oturumu içinde çalışan ve duraklatılmış durumları arasında geçiş yapabilirsiniz.
5. **Son Etkinlik oturum** - herhangi bir noktada kullanıcı etkinlik oturumu sonlandırabilir veya sona ve kendi başına ölçülen bir etkinlik (örneğin, iki mil Çalıştır) ise bitiş.

Son kullanıcının HealthKit veri deposuna etkinlik oturumunun sonuçlarını kaydetmek için adımdır.

### <a name="observing-healthkit-samples"></a>HealthKit örnekleri Gözlemleme

Uygulama açmanız gerekecek bir _sorgu bağlantı nesnesi_ her HealthKit veri noktaları için Kalp oranı veya etkin enerji Yakılan gibi bunu, ilgilendiği. Gözlemlenen her veri noktası için bir güncelleştirme işleyici uygulamaya gönderilen gibi yeni verilerini yakalamak için oluşturulması gerekir.

Uygulama bu veri noktalarından toplamlar (örneğin, toplam çalışma uzaklığı) biriktirmesi ve kendi kullanıcı arabirimi gerektiği gibi güncelleştirin. Ayrıca, belirli hedef veya bir çalışma sonraki mil Tamamlanıyor gibi başarı ulaştığınızda uygulama kullanıcıların bildirebilir.

Aşağıdaki örnek kod göz atın:

```csharp
private void ObserveHealthKitSamples ()
{
    // Get the starting date of the required samples
    var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

    // Get data from the local device
    var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
    var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

    // Assemble compound predicate
    var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

    // Get ActiveEnergyBurned
    var queryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
        // Valid?
        if (error == null) {
            // Yes, process all returned samples
            foreach (HKSample sample in addedObjects) {
                var quantitySample = sample as HKQuantitySample;
                ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);
            }
            
            // Update User Interface
            ...
        }
    });

    // Start Query
    HealthStore.ExecuteQuery (queryActiveEnergyBurned);
                                          
}
```

Kullanmak için veri almak istediği başlangıç tarihini ayarlamak için bir koşul oluşturur `GetPredicateForSamples` yöntemi. HealthKit bilgilerini kullanarak cihaz kümesini oluşturur `GetPredicateForObjectsFromDevices` bu durumda yalnızca yerel Apple Watch yöntemi (`HKDevice.LocalDevice`). İki koşulları bir bileşik koşul birleştirilir (`NSCompoundPredicate`) kullanarak `CreateAndPredicate` yöntemi.

Yeni bir `HKAnchoredObjectQuery` istenen veri noktası için oluşturulan (Bu durumda `HKQuantityTypeIdentifier.ActiveEnergyBurned` etkin enerji Yakılan veri noktası için), döndürülen veri miktarı hiçbir sınır uygulanmaz (`HKSampleQuery.NoLimit`) ve veri olma uygulamaya dönüş işlemek için bir güncelleştirme işleyici tanımlanır HealthKit. 

Güncelleştirme işleyicisi, uygulamaya verilen veri noktası için yeni veriler teslim dilediğiniz zaman çağrılır. Herhangi bir hata döndürülürse, uygulama güvenle verileri okuyabilir, tüm gerekli hesaplama ve kendi kullanıcı Arabirimi gerektiği gibi güncelleştirin.

Kod döngü tüm örnekleri (`HKSample`) döndürülen `addedObjects` dizi ve bir miktar örnek çevirir (`HKQuantitySample`). Sonra da bir joule olarak örnek çift değerini alır (`HKUnit.Joule`) ve etkin enerji etkinlik için yazılmış toplamı içine toplanır ve kullanıcı arabirimini güncelleştirir.

### <a name="achieved-goal-notification"></a>Arşivlenmiş hedef bildirim

Bir hedef (örneğin, bir çalışma ilk mil tamamladıktan), etkinlik uygulamasında kullanıcı başarır zaman olarak yukarıda açıklanan, onu haptic geri bildirim Taptic altyapısı aracılığıyla kullanıcı gönderebilirsiniz. Kullanıcı geri bildirim istemek için olay görmek için kendi bileğe kadar büyük olasılıkla oluşturacağı bu yana uygulama aynı zamanda, UI bu noktada, güncelleştirmeniz gerekir.

Haptic geri bildirim yürütmek için aşağıdaki kodu kullanın:

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>Olayları Gözlemleme

Uygulama kullanıcının etkinlik sırasında belirli noktaları vurgulamak kullanabilirsiniz zaman damgaları olaylardır. Bazı olayları doğrudan uygulama tarafından oluşturulan ve etkinlik kaydedilir ve bazı olaylar HealthKit tarafından otomatik olarak oluşturulur.

HealthKit tarafından oluşturulan olayları gözlemektir için uygulama geçersiz kılar `DidGenerateEvent` yöntemi `HKWorkoutSessionDelegate`:

```csharp
using System.Collections.Generic;
...

public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Save HealthKit generated event
    WorkoutEvents.Add (@event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Lap:
        break;
    case HKWorkoutEventType.Marker:
        break;
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

Apple aşağıdaki yeni olay türlerini watchOS 3 ekledi:

- `HKWorkoutEventType.Lap` -Etkinlik eşit uzaklıkta bölümleri kesme olayları içindir. Örneğin, bir parça çalışırken geçici bir diz işaretlemek için.
- `HKWorkoutEventType.Marker` -Rasgele ilgi için etkinlik içinde noktalarıdır. Örneğin, belirli bir rota noktasında bir dış çalışma ulaşmasını.

Bu yeni türleri uygulama tarafından oluşturulan ve grafikler ve İstatistikler oluştururken daha sonra kullanmak için etkinlik depolanır.

Bir imleç olayını oluşturmak için aşağıdakileri yapın:

```csharp
using System.Collections.Generic;
...

public float MilesRun { get; set; }
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public void ReachedNextMile ()
{
    // Create and save marker event
    var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
    WorkoutEvents.Add (markerEvent);

    // Notify user
    NotifyUserOfReachedMileGoal (++MilesRun);
}
```

Bu kod bir imleç olayını yeni bir örneğini oluşturur (`HKWorkoutEvent`) ve olayların (daha sonra etkinlik oturumuna yazılır) özel bir koleksiyona kaydeder ve olay haptics yoluyla kullanıcıyı uyarır.

### <a name="pausing-and-resuming-workouts"></a>Duraklatma ve sürdürme Egzersizleriniz

Bir etkinlik oturumunda herhangi bir noktada, kullanıcı geçici olarak etkinlik durdurabilir ve daha sonraki bir zamanda sürdürün. Örneğin, önemli bir çağrı alabilir ve çağrı tamamlandıktan sonra Çalıştır sürdürmek için bir iç Çalıştır duraklatın.

Uygulamanın kullanıcı Arabirimi, duraklatma ve böylece kullanıcı etkinliklerini askıya alınmış durumdayken Apple Watch güç ve veri alanından tasarruf edebilirsiniz (çağırarak HealthKit) etkinlik sürdürmek için bir yol sağlamanız gerekir. Ayrıca, uygulama etkinlik oturum duraklatılmış bir durumda olduğunda, alınan yeni veri noktalarına yok saymanız gerekir.

HealthKit duraklatıp çağrıları duraklatma ve sürdürme olaylar oluşturarak yanıtlar. Etkinlik oturum duraklatıldığında oturum sürdürülene kadar hiçbir yeni olayları ya da veri uygulamaya HealthKit tarafından gönderilir.

Bir etkinlik oturumu duraklatıp için aşağıdaki kodu kullanın:

```csharp
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public HKWorkoutSession WorkoutSession { get; set;}
...

public void PauseWorkout ()
{
    // Pause the current workout
    HealthStore.PauseWorkoutSession (WorkoutSession);
}

public void ResumeWorkout ()
{
    // Pause the current workout
    HealthStore.ResumeWorkoutSession (WorkoutSession);
}
```

Geçersiz kılarak HealthKit oluşturulan duraklatma ve sürdürme olayları işlenebilir `DidGenerateEvent` yöntemi `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);

    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

### <a name="motion-events"></a>Hareket olayları

Ayrıca yeni hareket duraklatıldı watchOS 3 için olan (`HKWorkoutEventType.MotionPaused`) ve hareket sürdürüldü (`HKWorkoutEventType.MotionResumed`) olayları. Kullanıcı başlatır ve taşıma durdurduğunda bu olaylar sırasında çalışan bir etkinlik HealthKit tarafından otomatik olarak oluşturulur.

Uygulama hareket duraklatılmış bir olayı aldığında, kullanıcı hareket sürdürür ve hareket sürdürür olayı alındığında kadar veri toplamayı durdurmanız gerekir. Uygulama hareket duraklatılmış bir olaya yanıt olarak etkinlik oturumunda duraklatın değil.

> [!IMPORTANT]
> Hareket duraklatıldı ve hareket sürdürme olayları RunningWorkout etkinlik türü için yalnızca desteklenir (`HKWorkoutActivityType.Running`).

Geçersiz kılarak bu olayları yeniden işlenebilir `DidGenerateEvent` yöntemi `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    }
}

```

## <a name="ending-and-saving-the-workout-session"></a>Bitiş ve etkinlik oturum kaydetme

Kullanıcı kendi etkinlik tamamlandığında, uygulamanın geçerli etkinlik oturumunu sona erdirmek ve HealthKit veritabanına kaydetmek gerekir. HealthKit için kaydedilen egzersizleriniz otomatik olarak etkinlik etkinlik listesinde görüntülenir.

Yeni iOS 10, bu kullanıcının iPhone etkinlik etkinlik listesi listesini içerir. Bu nedenle Apple Watch yanınızda olmasa bile etkinlik telefonda sunulur.

3 taraf uygulamalar artık kullanıcının günlük taşıma hedefleri katkıda bulunabilirsiniz şekilde enerji örnekleri dahil egzersizleriniz kullanıcının taşıma halka etkinlikleri uygulama güncelleştirin.

Bitirmek ve etkinlik oturumu kaydetmek için aşağıdaki adımları gerekir:

[![](workout-apps-images/workout05.png "Bitiş ve etkinlik oturum diyagramı kaydetme")](workout-apps-images/workout05.png#lightbox)

1. İlk olarak, uygulama etkinlik oturumunu sona erdirmek gerekir.
2. Etkinlik oturum HealthKit için kaydedilir.
3. Tüm örnekleri (örneğin, Yakılan enerji veya uzaklığı) kaydedilen etkinlik oturumuna ekleyin.

### <a name="ending-the-session"></a>Oturumu sona erdirme

Etkinlik oturumunu sona erdirmek için çağrı `EndWorkoutSession` yöntemi `HKHealthStore` tümleştirilmesidir `HKWorkoutSession`:

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
...

public void EndOutdoorRun ()
{
    // End the current workout session
    HealthStore.EndWorkoutSession (WorkoutSession);
}
```

Bu aygıtlar algılayıcılar kendi normal moda sıfırlar. Etkinlik bitiş HealthKit sona erdiğinde, bir geri çağırma alacak `DidChangeToState` yöntemi `HKWorkoutSessionDelegate`:

```csharp
public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
{
    // Take action based on the change in state
    switch (toState) {
    ...
    case HKWorkoutSessionState.Ended:
        StopObservingHealthKitSamples ();
        RaiseEnded ();
        break;
    }

}
```

### <a name="saving-the-session"></a>Oturum kaydetme

Uygulama etkinlik oturumu bittikten sonra bir etkinlik oluşturmak gerekir (`HKWorkout`) ve (birlikte bir olay) HealthKit veri deposuna kaydedin (`HKHealthStore`):

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
public float MilesRun { get; set; }
public double ActiveEnergyBurned { get; set;}
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

private void SaveWorkoutSession ()
{
    // Build required workout quantities 
    var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
    var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

    // Create any required metadata
    var metadata = new NSMutableDictionary ();
    metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

    // Create workout
    var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                    WorkoutSession.StartDate, 
                                    NSDate.Now, 
                                    WorkoutEvents.ToArray (), 
                                    energyBurned, 
                                    distance, 
                                    metadata);

    // Save to HealthKit
    HealthStore.SaveObject (workout, (successful, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (successful) {

            }
        } else {
            // Report error
        }
    });

}
```

Bu kod oluşturur etkinlik yazılmış enerji toplam miktarı ve uzaklık iste `HKQuantity` nesneleri. Etkinlik tanımlama meta veri sözlüğü oluşturulur ve etkinlik konumunu belirtilir:

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Yeni bir `HKWorkout` nesnesi ile aynı oluşturulur `HKWorkoutActivityType` olarak `HKWorkoutSession`, başlangıç ve bitiş tarihleri (Yukarıdaki bölümlerden birikmiş) olaylar, yazılmış, enerji listesi toplam uzaklığı ve meta veri sözlüğü. Bu nesne sistem durumu deposuna kaydedilir ve hatalarını ele.  

### <a name="adding-samples"></a>Örnekleri ekleme

Uygulama için bir etkinlik örnekler kümesi kaydettiğinde, uygulamanın belirli bir etkinlik ile ilişkili tüm örnekleri için daha sonraki bir tarihte HealthKit sorgulayabilmesi HealthKit örnekler ve etkinlik arasında bir bağlantı oluşturur. Uygulama, bu bilgileri kullanarak etkinlik verilerini grafikleri oluşturmak ve etkinlik zaman çizelgesi karşı çizmek.

Etkinlik uygulamanın taşıma halkası katkıda bulunmak bir uygulama için kaydedilmiş etkinlik enerji örnekleriyle içermesi gerekir. Ayrıca, uzaklık ve enerji toplamlarını uygulama ile kaydedilen bir etkinlik ilişkilendirir örnekleri toplamını eşleşmesi gerekir.

Örnekleri için kaydedilmiş bir etkinlik eklemek için aşağıdakileri yapın:

```csharp
using System.Collections.Generic;
using WatchKit;
using HealthKit;
...

public HKHealthStore HealthStore { get; private set; }
public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
...


private void SaveWorkoutSamples (HKWorkout workout)
{
    // Add samples to saved workout
    HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (success) {

            }
        } else {
            // Report error
        }
    });
}
```

İsteğe bağlı olarak, uygulama hesaplamak ve daha küçük bir alt kümesini örnekleri veya kaydedilmiş etkinlik ile ilişkili alır (etkinlik tüm aralığını kapsayan) tek mega örnek oluşturun.

## <a name="workouts-and-ios-10"></a>Egzersizleriniz ve iOS 10

Bir üst iOS 10 tabanlı etkinlik uygulaması her watchOS 3 etkinlik uygulamanın var ve, 10, iOS için yeni bu iOS uygulamasının Apple Watch etkinlik modda (kullanıcı müdahalesi olmadan) koyun ve watchOS uygulama çalıştıran arka plan modunda çalışacak bir etkinlik başlatmak için kullanılabilir ( bkz [hakkında arka plan çalıştıran](#About-Background-Running) üzerinde daha fazla ayrıntı için).

WatchOS uygulama çalışırken, Mesajlaşma ve üst iOS uygulaması ile iletişim için WatchConnectivity kullanabilirsiniz.

Bu işleminin nasıl çalıştığı bir göz atalım:

[![](workout-apps-images/workout06.png "iPhone ve Apple Watch iletişim diyagramı")](workout-apps-images/workout06.png#lightbox)

1. İPhone uygulamasını oluşturur bir `HKWorkoutConfiguration` nesne ve etkinlik türü ve konumu ayarlar.
2. `HKWorkoutConfiguration` Nesne uygulamasının Apple Watch sürümü gönderilir ve zaten çalışmıyorsa sistemi tarafından başlatılır.
3. Geçirilen etkinlik yapılandırmasında kullanarak, watchOS 3 uygulama yeni bir etkinlik oturumu başlatır (`HKWorkoutSession`).

> [!IMPORTANT]
> Bir etkinlik üzerinde Apple Watch başlatmak üst iPhone uygulama, watchOS 3 uygulama etkin arka plan çalıştıran olması gerekir. Lütfen bakın [etkinleştirme arka çalışan](#Enabling-Background-Running) üzerinde daha fazla ayrıntı için.

Bu işlem bir etkinlik oturumu watchOS 3 uygulamada doğrudan başlatma işlemini için çok benzer. İPhone üzerinde aşağıdaki kodu kullanın:

```csharp
using System;
using HealthKit;
using WatchConnectivity;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
#endregion
...

private void StartOutdoorRun ()
{
    // Can the app communicate with the watchOS version of the app?
    if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
        // Create a workout configuration
        var configuration = new HKWorkoutConfiguration () {
            ActivityType = HKWorkoutActivityType.Running,
            LocationType = HKWorkoutSessionLocationType.Outdoor
        };

        // Start watch app
        HealthStore.StartWatchApp (configuration, (success, error) => {
            // Handle any errors
            if (error == null) {
                // Was the save successful
                if (success) {
                    ...
                }
            } else {
                // Report error
                ...
            }
        });
    }
}
```

Bu kod, uygulamanın watchOS sürümü yüklü olduğundan ve iPhone sürüm ilk bağlanabilmesi sağlar:

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

Oluşturur sonra bir `HKWorkoutConfiguration` her zamanki gibi ve kullandığı `StartWatchApp` yöntemi `HKHealthStore` Apple Watch gönderin ve uygulama ve etkinlik oturumu başlatmak için.

Ve izleme OS uygulamayı üzerinde aşağıdaki kodda `WKExtensionDelegate`:

```csharp
using WatchKit;
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...


public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
{
    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Sürdüğünü `HKWorkoutConfiguration` ve yeni bir `HKWorkoutSession` ve özel bir örneğini ekler `HKWorkoutSessionDelegate`. Etkinlik oturum kullanıcının HealthKit sistem durumu deposunda karşı başlatılır.

## <a name="bringing-all-the-pieces-together"></a>Tüm parçaları araya getirme

Bu belgede belirtilen bilgiler tümünün alma, watchOS 3 göre etkinlik uygulama ve onun üst iOS 10 tabanlı etkinlik uygulaması, aşağıdaki bölümleri içerebilir:

1. **iOS 10 `ViewController.cs`**  -izleme bağlantısı oturumu ve Apple Watch üzerinde bir etkinlik başlangıç işler.
2. **watchOS 3 `ExtensionDelegate.cs`**  -etkinlik uygulamanın watchOS 3 sürümünü işler.
3. **watchOS 3 `OutdoorRunDelegate.cs`**  -özel `HKWorkoutSessionDelegate` için etkinlik olayları işlemek için.

> [!IMPORTANT]
> Aşağıdaki bölümlerde gösterilen kodu yalnızca watchOS 3 etkinlik uygulamalarında sağlanan yeni, Gelişmiş özellikleri uygulamak için gereken bölümleri içerir. Tüm destekleyici kodu ve sunmak ve kullanıcı arabirimini güncelleştirmek için kodu dahil değildir ancak diğer watchOS Belgelerimizi izleyerek kolayca oluşturulabilir.<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs` Etkinlik uygulamanın üst iOS 10 sürümünü dosyasına aşağıdaki kodu ekleyin:

```csharp
using System;
using HealthKit;
using UIKit;
using WatchConnectivity;

namespace MonkeyWorkout
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
        public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Private Methods
        private void InitializeWatchConnectivity ()
        {
            // Is Watch Connectivity supported?
            if (!WCSession.IsSupported) {
                // No, abort
                return;
            }

            // Is the session already active?
            if (ConnectivitySession.ActivationState != WCSessionActivationState.Activated) {
                // No, start session
                ConnectivitySession.ActivateSession ();
            }
        }

        private void StartOutdoorRun ()
        {
            // Can the app communicate with the watchOS version of the app?
            if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
                // Create a workout configuration
                var configuration = new HKWorkoutConfiguration () {
                    ActivityType = HKWorkoutActivityType.Running,
                    LocationType = HKWorkoutSessionLocationType.Outdoor
                };

                // Start watch app
                HealthStore.StartWatchApp (configuration, (success, error) => {
                    // Handle any errors
                    if (error == null) {
                        // Was the save successful
                        if (success) {
                            ...
                        }
                    } else {
                        // Report error
                        ...
                    }
                });
            }
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Start Watch Connectivity
            InitializeWatchConnectivity ();
        }
        #endregion
    }
}
```

### <a name="extensiondelegatecs"></a>ExtensionDelegate.cs

`ExtensionDelegate.cs` Etkinlik uygulamanın watchOS 3 sürümünü dosyasına aşağıdaki kodu ekleyin:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
        public OutdoorRunDelegate RunDelegate { get; set; }
        #endregion

        #region Constructors
        public ExtensionDelegate ()
        {
            
        }
        #endregion

        #region Private Methods
        private void StartWorkoutSession (HKWorkoutConfiguration workoutConfiguration)
        {
            // Create workout session
            // Start workout session
            NSError error = null;
            var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

            // Successful?
            if (error != null) {
                // Report error to user and return
                return;
            }

            // Create workout session delegate and wire-up events
            RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

            RunDelegate.Failed += () => {
                // Handle the session failing
                ...
            };

            RunDelegate.Paused += () => {
                // Handle the session being paused
                ...
            };

            RunDelegate.Running += () => {
                // Handle the session running
                ...
            };

            RunDelegate.Ended += () => {
                // Handle the session ending
                ...
            };
            
            RunDelegate.ReachedMileGoal += (miles) => {
                // Handle the reaching a session goal
                ...
            };

            RunDelegate.HealthKitSamplesUpdated += () => {
                // Update UI as required
                ...
            };

            // Start session
            HealthStore.StartWorkoutSession (workoutSession);
        }

        private void StartOutdoorRun ()
        {
            // Create a workout configuration
            var workoutConfiguration = new HKWorkoutConfiguration () {
                ActivityType = HKWorkoutActivityType.Running,
                LocationType = HKWorkoutSessionLocationType.Outdoor
            };

            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion

        #region Override Methods
        public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
        {
            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion
    }
}
```

### <a name="outdoorrundelegatecs"></a>OutdoorRunDelegate.cs

`OutdoorRunDelegate.cs` Etkinlik uygulamanın watchOS 3 sürümünü dosyasına aşağıdaki kodu ekleyin:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Private Variables
        private HKAnchoredObjectQuery QueryActiveEnergyBurned;
        #endregion

        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        public float MilesRun { get; set; }
        public double ActiveEnergyBurned { get; set;}
        public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
        public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;

        }
        #endregion

        #region Private Methods
        private void ObserveHealthKitSamples ()
        {
            // Get the starting date of the required samples
            var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

            // Get data from the local device
            var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
            var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

            // Assemble compound predicate
            var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

            // Get ActiveEnergyBurned
            QueryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
                // Valid?
                if (error == null) {
                    // Yes, process all returned samples
                    foreach (HKSample sample in addedObjects) {
                        // Accumulate totals
                        var quantitySample = sample as HKQuantitySample;
                        ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);

                        // Save samples
                        WorkoutSamples.Add (sample);
                    }

                    // Inform caller
                    RaiseHealthKitSamplesUpdated ();
                }
            });

            // Start Query
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
                                                  
        }

        private void StopObservingHealthKitSamples ()
        {
            // Stop query
            HealthStore.StopQuery (QueryActiveEnergyBurned);
        }

        private void ResumeObservingHealthkitSamples ()
        {
            // Resume current queries 
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
        }

        private void NotifyUserOfReachedMileGoal (float miles)
        {
            // Play haptic feedback
            WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);

            // Raise event
            RaiseReachedMileGoal (miles);
        }

        private void SaveWorkoutSession ()
        {
            // Build required workout quantities
            var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
            var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

            // Create any required metadata
            var metadata = new NSMutableDictionary ();
            metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

            // Create workout
            var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                            WorkoutSession.StartDate, 
                                            NSDate.Now, 
                                            WorkoutEvents.ToArray (), 
                                            energyBurned, 
                                            distance, 
                                            metadata);

            // Save to HealthKit
            HealthStore.SaveObject (workout, (successful, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (successful) {
                        // Add samples to workout
                        SaveWorkoutSamples (workout);
                    }
                } else {
                    // Report error
                    ...
                }
            });

        }

        private void SaveWorkoutSamples (HKWorkout workout)
        {
            // Add samples to saved workout
            HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (success) {
                        ...
                    }
                } else {
                    // Report error
                    ...
                }
            });
        }
        #endregion

        #region Public Methods
        public void PauseWorkout ()
        {
            // Pause the current workout
            HealthStore.PauseWorkoutSession (WorkoutSession);
        }

        public void ResumeWorkout ()
        {
            // Pause the current workout
            HealthStore.ResumeWorkoutSession (WorkoutSession);
        }

        public void ReachedNextMile ()
        {
            // Create and save marker event
            var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
            WorkoutEvents.Add (markerEvent);

            // Notify user
            NotifyUserOfReachedMileGoal (++MilesRun);
        }

        public void EndOutdoorRun ()
        {
            // End the current workout session
            HealthStore.EndWorkoutSession (WorkoutSession);
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                StopObservingHealthKitSamples ();
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                if (fromState == HKWorkoutSessionState.Paused) {
                    ResumeObservingHealthkitSamples ();
                } else {
                    ObserveHealthKitSamples ();
                }
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                StopObservingHealthKitSamples ();
                SaveWorkoutSession ();
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);

            // Save HealthKit generated event
            WorkoutEvents.Add (@event);

            // Take action based on the type of event
            switch (@event.Type) {
            case HKWorkoutEventType.Lap:
                ...
                break;
            case HKWorkoutEventType.Marker:
                ...
                break;
            case HKWorkoutEventType.MotionPaused:
                ...
                break;
            case HKWorkoutEventType.MotionResumed:
                ...
                break;
            case HKWorkoutEventType.Pause:
                ...
                break;
            case HKWorkoutEventType.Resume:
                ...
                break;
            }
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();
        public delegate void OutdoorRunMileGoalDelegate (float miles);

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }

        public event OutdoorRunMileGoalDelegate ReachedMileGoal;
        internal void RaiseReachedMileGoal (float miles)
        {
            if (this.ReachedMileGoal != null) this.ReachedMileGoal (miles);
        }

        public event OutdoorRunEventDelegate HealthKitSamplesUpdated;
        internal void RaiseHealthKitSamplesUpdated ()
        {
            if (this.HealthKitSamplesUpdated != null) this.HealthKitSamplesUpdated ();
        }
        #endregion
    }
}
```

## <a name="best-practices"></a>En İyi Yöntemler

Apple tasarlarken ve etkinlik uygulamaları watchOS 3 ve iOS 10 uygulama aşağıdaki en iyi yöntemleri kullanarak önerir:

- İPhone ve uygulamayı iOS 10 sürümünü bağlanamıyor olsa bile watchOS 3 etkinlik uygulama hala çalıştığından emin olun.
- Uzaklık örnekleri GPS olmadan oluşturmak mümkün olduğundan GPS kullanılamadığında HealthKit uzaklığı kullanın.
- Kullanıcının Apple Watch veya iPhone etkinlik başlangıç izin verin.
- Geçmiş verileri görünümlerde (3 diğer taraf uygulamaları gibi) diğer kaynaklardan egzersizleriniz görüntülemek uygulama izin verir.
- Uygulama değil silinmiş görüntü egzersizleriniz geçmiş verilerdeki olmadığından emin olun.

## <a name="summary"></a>Özet

Bu makalede ele alınan geliştirmeleri Apple watchOS 3 ve Xamarin uygulamak nasıl etkinlik uygulamalarında yaptı.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
- [HealthKit giriş](~/ios/platform/healthkit.md)
