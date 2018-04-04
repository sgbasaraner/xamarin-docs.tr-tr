---
title: EventKit
description: Bu kılavuz, erişmek ve EventKit gösterilen Takvim veritabanında depolanan takvimleri, CalendarEvents ve anımsatıcıları verilerle çalışma hakkında genel bir bakış sağlar. Ana sınıflar ve onların rol EventKit framework ile ilişkili ortak görevleri bir dizi yanı sıra EventKit programlama'kapsar.
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a8439586ac92f8139cf9341611125352c85706e5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="eventkit"></a>EventKit

_Bu kılavuz, erişmek ve EventKit gösterilen Takvim veritabanında depolanan takvimleri, CalendarEvents ve anımsatıcıları verilerle çalışma hakkında genel bir bakış sağlar. Ana sınıflar ve onların rol EventKit framework ile ilişkili ortak görevleri bir dizi yanı sıra EventKit programlama'kapsar._

iOS sahip iki Takvim ilgili uygulamalar yerleşik: Takvim uygulama ve anımsatıcıları uygulama. Takvim uygulama Takvim verileri nasıl yönettiğini anlamanıza basit ancak anımsatıcıları uygulama daha az açıktır. Anımsatıcıları gerçekte ne zaman tamamladıkları vadesi geçmiş, olduğunuzda açısından, kendileriyle ilişkili tarihleri sahip vs. Bu nedenle, iOS takvim olayları veya anımsatıcı olarak adlandırılan, tek bir konumda olması gerekmediğini tüm takvim verilerini depolar *Takvim veritabanını*.

EventKit framework erişmek için bir yol sağlar *takvimleri*, *takvim olayları*, ve *anımsatıcıları* Takvim veritabanı depolar veri. Erişim takvimler ve takvim olayları süredir kullanılabilir 4 iOS bu yana, ancak anımsatıcıları erişimi iOS 6 yenidir.

Bu kılavuzda ele oluşturacağız:

-   **EventKit Temelleri** – bu EventKit temel parçalar ana sınıflar aracılığıyla getirir ve bunların kullanım bir anlayış sağlar. Bu bölümde, belgenin bir sonraki bölümü tackling önce okuma gereklidir. 
-   **Ortak Görevler** – ortak görevler bölümü ortak gibi şeyler konusunda hızlı başvuru olması amaçlanmıştır; takvimleri numaralandırma, oluşturma, kaydetme ve alma takvim olayları ve anımsatıcıları, yanı sıra yerleşik denetleyicileri için kullanma oluşturma ve takvim olayları değiştirme. Belirli görevler için bir başvuru olma amacını gibi ön arkaya, bu bölümde okuyamadı. 


Bu kılavuzdaki tüm görevler Yardımcısı örnek uygulama kullanılabilir:

 [![](eventkit-images/01.png "Yardımcı örnek uygulama ekranlar")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>Gereksinimler

EventKit iOS 4.0 kullanılmıştır, ancak anımsatıcıları verilere erişim iOS 6.0 sunulmuştur. Bu nedenle, genel EventKit geliştirme yapmak için en az hedef gerekir sürüm 4.0 ve 6.0 için anımsatıcılar.

Ayrıca, anımsatıcıları uygulaması ilk eklemediğiniz sürece anımsatıcıları veri Ayrıca, kullanılamaz, yani simulator'da kullanılabilir değil. Ayrıca, erişim istekleri, yalnızca gerçek cihazdaki kullanıcı için gösterilir. Bu nedenle, EventKit geliştirme cihazda en iyi test edilir.

## <a name="event-kit-basics"></a>Olay Seti temelleri

EventKit ile çalışırken, kavrayın ortak sınıfları ve bunların kullanım olması önemlidir. Tüm bu sınıfların bulunabilir `EventKit` ve `EventKitUI` (için `EKEventEditController`).

### <a name="eventstore"></a>EventStore

*EventStore* sınıftır en önemli EventKit içinde EventKit içinde herhangi bir işlem gerçekleştirmek için gerekli olduğundan. Bu, kalıcı depolama ya da tüm EventKit verileri için veritabanı altyapısı olarak düşünülebilir. Gelen `EventStore` takvimleri hem takvim olayları Takvim uygulamada yanı sıra, anımsatıcıları uygulamada anımsatıcıları erişebilirsiniz.

Çünkü `EventStore` olan bir veritabanı motoru gibi oluşturulabilir ve uygulama örneğini ömrü boyunca mümkün olduğunca az yok, yani uzun süreli, olmalıdır. Tek bir örneğini oluşturduktan sonra aslında, önerilir bir `EventStore` olmaz ihtiyacınız onu yeniden emin değilseniz bir uygulamada, bu başvuru geçici uygulama tüm ömrü boyunca tutmalısınız. Ayrıca, tüm çağrıları tek bir gitmesi gereken `EventStore` örneği. Bu nedenle, kullanılabilir tek bir örnek tutmak için Singleton deseni önerilir.

#### <a name="creating-an-event-store"></a>Bir olay deposu oluşturma

Aşağıdaki kod tek bir örneğini oluşturmak için etkili bir yol gösterir `EventStore` sınıf ve statik olarak içinde kullanılabilir bir uygulama yapın:

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

Yukarıdaki kod bir örneği için Singleton deseni kullanır `EventStore` uygulama yüklediğinde. `EventStore` Sonra genel uygulama içinde aşağıdaki gibi erişilebilir:

```csharp
App.Current.EventStore;
```

Burada tüm örneklerde oldukları için bu deseni kullanın Not `EventStore` aracılığıyla `App.Current.EventStore`.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>Takvim ve anımsatıcı verilere erişim isteyen

Herhangi bir veri EventStore aracılığıyla erişmesine izin verilmeden önce uygulamanın ilk takvim olayları verileri veya hangisinin bağlı olarak gereksinim duyduğunuz anımsatıcıları veri erişimi istemeniz gerekir. Bu, kolaylaştırmak için `EventStore` adlı bir yöntemi gösterir `RequestAccess` hangi — çağrıldığında — uygulama Takvim veri ya da bağlı olarak hangi Anımsatıcıverierişimiistediğinibelirtenkullanıcıiçinbiruyarıgörünümügöstermeyecektir`EKEntityType`kendisine geçirilen. Bir uyarı görünümü başlatır çünkü arama uyumsuzdur ve olarak geçirilen tamamlama işleyiciyi çağırır bir `NSAction` (veya Lambda) iki alacak ona parametreleri; desteklemediğini erişim verildi, bir Boole değeri ve bir `NSError`, not null gerçekleştirir İstekteki hata bilgilerini içerir. Örneğin, aşağıdaki kodlanmış bir uyarı istek verilmedi görüntülerseniz Takvim olay verileri ve Göster erişimi ister.

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

İstek verildiğinde, uygulama cihazda yüklü ve kullanıcı için bir uyarı oluşturan pop değil sürece anımsanır.
Ancak, erişimi yalnızca kaynak, takvim olayları veya verilen anımsatıcıları türüne verilir. Bir uygulama hem erişmesi gerekirse, her ikisi de istemeniz gerekir.

İzni hatırlanan gerektiğinden bir işlem gerçekleştirmeden önce her zaman erişim istemek için iyi bir fikirdir için her zaman istekte görece ucuz alır.

Tamamlama işleyici ayrı (kullanıcı Arabirimi olmayan) iş parçacığı üzerinde çağrıldığı için ek olarak, tamamlama işleyici Arabiriminde herhangi bir güncelleştirme aracılığıyla çağrılmalıdır `InvokeOnMainThread`, bir özel durum aksi ve yakalanan değil, uygulama kilitleniyor.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` türünü tanımlayan bir numaralandırma `EventKit` öğesi veya veri. İki değer vardır: `Event` ve anımsatıcı. Çeşitli yöntemler, dahil olmak üzere kullanılan `EventStore.RequestAccess` bildirmek için `EventKit` ne tür erişmek veya almak için verileri.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* takvim olayları bir grubu içeren bir takvim temsil eder. Takvimler depolanabilir çok farklı yerde gibi yerel olarak içinde *iCloud*, 3 taraf sağlayıcı yeri gibi bir *Exchange Server* veya *Google*, vb. Birçok kez `EKCalendar` bildirmek için kullanılan `EventKit` nerede olaylarını arayın veya bunları kaydedileceği konumu.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* bulunabilir `EventKitUI` ad alanı ve düzenlemek veya takvim olayları oluşturmak için kullanılan bir yerleşik denetleyicisidir. Çok benzer şekilde yerleşik kamera denetleyicileri `EKEventEditController` ağır lifting UI görüntüleme ve kaydetme işleme sizin için yapar.

### <a name="ekevent"></a>EKEvent

 *EKEvent* takvim olayı temsil eder. Her ikisi de `EKEvent` ve `EKReminder` devralınmalıdır `EKCalendarItem` ve alanları gibi `Title`, `Notes`ve benzeri.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* anımsatıcı öğeyi temsil eder.

### <a name="ekspan"></a>EKSpan

*EKSpan* yineleme ve iki değer olan olayları değiştirirken olayları aralığını açıklayan bir numaralandırma: *ThisEvent* ve *FutureEvents*. `ThisEvent` herhangi bir değişiklik, yalnızca belirli bir olaya başvuruluyor, serideki ancak meydana gelir anlamına gelir `FutureEvents` olay ve tüm gelecekteki tekrarları etkiler.

## <a name="tasks"></a>Görevler

Kullanım kolaylığı için EventKit kullanım aşağıdaki bölümlerde açıklanan ortak görevleri bölünür.

### <a name="enumerate-calendars"></a>Takvimler listeleme

Kullanıcı cihazda yapılandırmış takvimleri Numaralandırılacak çağrısı `GetCalendars` üzerinde `EventStore` ve almak istediğiniz takvimleri (anımsatıcıları ya da olaylar) türünü geçirin:

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>Ekleme veya yerleşik denetleyicisi kullanarak bir olay değiştirme

*EKEventEditViewController* oluşturmak veya takvim uygulamasını kullanırken kullanıcıya sunulan kullanıcı Arabirimi ile bir olayı düzenlemek istiyorsanız, çok sayıda ağır lifting sizin için yapar:

 [![](eventkit-images/02.png "Takvim uygulamasını kullanırken kullanıcıya sunulan kullanıcı Arabirimi")](eventkit-images/02.png#lightbox)

Kullanmak için bir yöntem içinde bildirilirse çöpünün toplanma açılmıyor böylece sınıf düzeyinde bir değişkeni bildirme isteyeceksiniz:

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

Daha sonra bunu başlatmak için: Bu örneği, bir başvuru vermek `EventStore`, Yukarı wire bir *EKEventEditViewDelegate* için temsilci seçme ve kullanarak görüntüleme `PresentViewController`:

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

İsteğe bağlı olarak, olay önceden doldurmak istiyorsanız (aşağıda gösterildiği gibi) yepyeni bir olay oluşturabilir ya da veya kaydedilmiş bir olay alabilirsiniz:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

Kullanıcı arabirimini önceden doldurmak isterseniz denetleyicisinde olay özelliği ayarladığınızdan emin olun:

```csharp
eventController.Event = newEvent;
```

Var olan bir olaya kullanmak için bkz: *Kimliğine göre bir olay almak* daha sonra bölüm.

Temsilci kılmalıdır `Completed` kullanıcı iletişim kutusuyla sona erdiğinde denetleyici tarafından çağrılan yöntemi:

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

İsteğe bağlı olarak, temsilci denetleyebilirsiniz *eylem* içinde `Completed` olay ve kaydetmeniz değiştirmek veya iptal edildi, başka işlemler yapmak için bir yöntem, etcetera:

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>Bir olay program aracılığıyla oluşturma

Kodda bir olay oluşturmak için kullanın *FromStore* Üreteç yöntemi `EKEvent` sınıfı ve herhangi bir veri ayarlayabilirsiniz:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

Kaydedilen olay istediğiniz takvimi ayarlamanız gerekir, ancak hiçbir tercih varsa, varsayılan kullanabilirsiniz:

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

Olay kaydetmek için arama *SaveEvent* yöntemi `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

Kaydedildikten sonra *EventIdentifier* özelliği daha sonra olay almak için kullanılabilecek benzersiz bir tanımlayıcı ile güncelleştirilecek:

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` GUID biçimlendirilmiş bir dize değil.

### <a name="create-a-reminder-programmatically"></a>Bir anımsatıcı program aracılığıyla oluşturma

Kodda bir anımsatıcı oluşturma çok takvim olay oluşturma ile aynıdır:

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

Kaydetmek için arama *SaveReminder* yöntemi `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>Bir olay Kimliğine göre alma

Bir olay tarafından almak için kimliği, kullanın *EventFromIdentifier* yöntemi `EventStore` ve bu geçirin `EventIdentifier` olaydan çekilen:

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

Olaylar için var olan diğer iki tanımlayıcı özellik ancak `EventIdentifier` bu için çalışır, tek.

### <a name="retrieving-a-reminder-by-id"></a>Kimliğe göre anımsatıcısı alınıyor

Bir anımsatıcı almak kullanın *GetCalendarItem* yöntemi `EventStore` ve bu geçirin *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

Çünkü `GetCalendarItem` döndüren bir `EKCalendarItem`, için dönüştürülmelidir `EKReminder` anımsatıcı veri erişim veya örneği olarak kullanmak gereken bir `EKReminder` daha sonra.

Kullanmayan `GetCalendarItem` takvim olayları için zaman yazma gibi çalışmıyor.

### <a name="deleting-an-event"></a>Bir olay silme

Takvim olayı silmek için arama *RemoveEvent* üzerinde `EventStore` ve olay ve uygun bir başvuru geçirin `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

Not ancak, bir olay silindikten sonra olay başvuru olacaktır `null`.

### <a name="deleting-a-reminder"></a>Bir anımsatıcı silme

Bir anımsatıcı silmek için arama *RemoveReminder* üzerinde `EventStore` ve anımsatıcı başvuru geçirin:

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

Yukarıdaki kod içinde bir cast yoktur `EKReminder`, çünkü `GetCalendarItem` bunu almak için kullanıldı

### <a name="searching-for-events"></a>Olayları arama

Takvim olayları aramak için oluşturmalısınız bir *NSPredicate* aracılığıyla nesne *PredicateForEvents* yöntemi `EventStore`. Bir `NSPredicate` bir sorgu veri nesnesi eşleşmeleri bulmak için iOS kullanır:

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

Oluşturduktan sonra `NSPredicate`, kullanın *EventsMatching* yöntemi `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

Sorgu zaman uyumlu (engelleme) olduğunu ve dolayısıyla yeni iş parçacığı veya görev yapmak için Döndür isteyebilirsiniz sorgu bağlı olarak uzun bir süre devam edebilir unutmayın.

### <a name="searching-for-reminders"></a>Anımsatıcıları için arama

Anımsatıcıları arama olaylarına benzer; bir koşul gerektirir, ancak iş parçacığı engelleme hakkında endişelenmeniz gerekmez şekilde çağrı zaten zaman uyumsuz:

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>Özet

Bu belge bakış EventKit framework'ün önemli parçaları ve en yaygın görevleri bir dizi döndürdü. Ancak, EventKit framework çok büyük ve güçlü ve burada gibi sunulmuş henüz özellikleri içerir: toplu güncelleştirmeler, uyarılar, kaydetme ve değişiklikleri Takvim veritabanı üzerinde dinleme olaylarına yineleme yapılandırma yapılandırma Bölge sınırlarını ve daha fazlasını ayarlama.  Daha fazla bilgi için Apple'nın bkz [takvim ve anımsatıcıları Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html).


## <a name="related-links"></a>İlgili bağlantılar

- [Takvimler (örnek)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [iOS 6’ya Giriş](~/ios/platform/introduction-to-ios6/index.md)
- [Takvimler ve anımsatıcıları giriş](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
