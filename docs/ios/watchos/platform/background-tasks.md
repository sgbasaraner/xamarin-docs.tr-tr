---
title: "Arka plan görevleri"
description: "Yeni arka plan görevleri watchOS 3 izleme uygulama her zaman en son verileri ve stok anlık görüntüleri sağlamak için kullanın."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/13/2017
ms.openlocfilehash: 524d551a96dd1352d86671238a63c103cef9b0c5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="background-tasks"></a>Arka plan görevleri

_Yeni arka plan görevleri watchOS 3 izleme uygulama her zaman en son verileri ve stok anlık görüntüleri sağlamak için kullanın._

WatchOS 3 izleme uygulama bilgilerini güncel tutmak üç ana yolu vardır: 

- Birkaç yeni arka plan görevleri birini kullanarak. 
- Kendi zorluklar birini (çok zaman güncelleştirmek için vermiş) Saat yüzünü sahip. 
- Uygulama için kullanıcı PIN yeni yuva sahip (burada, bellekte depolanır ve sık sık güncelleştirilir). 

## <a name="keeping-an-app-up-to-date"></a>Bir uygulama güncel tutma

Tüm geliştirici watchOS uygulamanın verileri ve kullanıcı arabirimi geçerli ve güncelleştirilmiş tutabilirsiniz yolları ele almadan önce bu bölüm kullanım desenleri ve bir kullanıcı kendi iPhone ve bunların Apple Watch göre gün boyunca arasında nasıl geçiş tipik bir dizi göz atın  günün saati ve etkinlik şu anda (yürüten gibi) yaptıklarını.

Aşağıdaki örnek alın:

[ ![](background-tasks-images/update00.png "Bir kullanıcı kendi iPhone ve bunların Apple Watch gün boyunca arasında nasıl geçiş")](background-tasks-images/update00.png)

1. Bir kahve için satır beklenirken sabah, kullanıcının kendi iPhone güncel haberleri birkaç dakika gözatar.
2. Kafeterya ayrılmadan önce bunlar hızlı bir şekilde kendi izleme yüz üzerinde olası ile hava durumu denetleyin.
3. Yemeği önce eşlemeleri uygulama iPhone üzerinde yakındaki bir restoran bulmak ve bir istemci karşılamak için bir ayırma kitap kullanırlar.
4. Restoran seyahat sırasında bildirim kendi Apple Watch ve hızlı bir bakış ile almak için bunların Yemeği randevu geciktiğini bildikleri.
5. Akşam eşlemeleri uygulama iPhone üzerinde giriş yürüten önce trafiği denetlemek için kullanırlar.
6. Yol üzerinde bazı sütlü seçmek için bir iMessage bildirim yönergelerinin kendi Apple Watch aldıkları ev ve bunlar "Tamam" yanıtı göndermek için hızlı yanıt özelliğini kullanın.

"Hızlı Bakış" nedeniyle (üç saniyeden az) nasıl bir kullanıcı bir Apple Watch uygulama var. genellikle kullanmak isteyen, doğası uygulamanın istenen bilgileri getirebilir ve kullanıcıya görüntülemeden önce kullanıcı Arabiriminde güncelleştirmek için yeterli bir süre değil.

Yeni kullanarak API'leri Apple eklemiştir watchOS 3'de, uygulama için zamanlayabilirsiniz bir _arka plan yenileme_ ve kullanıcı istediği önce istenen bilgileri hazır bulundurun. Yukarıda açıklanan hava durumu olası örneği alın:

[ ![](background-tasks-images/update01.png "Hava durumu olası örneği")](background-tasks-images/update01.png)

1. Sistem tarafından belirli bir zamanda uyandırılabilir için uygulama zamanlama. 
2. Uygulama güncelleştirme oluşturmak için gerekli bilgileri getirir.
3. Uygulama kullanıcı arabirimi yeni verileri yansıtacak şekilde yeniden oluşturur.
4. Kullanıcının, uygulamanın olası glances, kullanıcının güncelleştirmesi beklemek zorunda kalmadan güncel bilgileri vardır.

Yukarıda görülen watchOS sistem çok sınırlı havuzu kullanılabilir olan bir veya daha fazla görevleri kullanarak uygulamayı uyku modundan:

[ ![](background-tasks-images/update02.png "Bir veya daha fazla görev kullanarak uygulamayı watchOS sistem uyandırır")](background-tasks-images/update02.png)

Apple öneririz (uygulamaya sınırlı kaynak olduğundan), bu görevin en yapmadan uygulama kendisini güncelleştirme işlemi tamamlanana kadar sürüklediğinizde tutarak.

Sistem bu görevleri yeni çağırarak teslim `HandleBackgroundTasks` yöntemi `WKExtensionDelegate` temsilci. Örneğin:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Constructors
        public ExtensionDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request here
            ...
        }
        #endregion
    }
}
```

Uygulamayı belirtilen görevi tamamlandığında, sisteme bu tamamlandı işaretleyerek döndürmeden:

[ ![](background-tasks-images/update03.png "Görev tamamlandı işaretleyerek sisteme döndürür.")](background-tasks-images/update03.png)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>Yeni arka plan görevleri

bir uygulama, uygulama gibi açmadan önce kullanıcının gereken içeriğe sahip sağlama bilgilerini güncelleştirmek için kullanabileceğiniz birkaç arka plan görevleri watchOS 3 sunar:

- **App Refresh arka plan** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) görev uygulamanın durumunu arka planda güncelleştirmesine izin verir. Genellikle bu kullanarak internet yeni içerik indirme gibi başka bir görev içerecektir bir [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Arka plan anlık görüntü yenileme** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) görev uygulamanın sistem yuva doldurmak için kullanılan bir anlık görüntüsünü alır önce kendi içeriği ve kullanıcı Arabirimi güncelleştirmesine izin verir.
- **Arka plan izleme bağlantı** - [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) görev eşleştirilmiş iPhone arka plan veri aldığında uygulama için başlatıldı.
- **Arka plan URL oturum** - [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) görev, uygulama için başlatılır, arka plan aktarım yetkilendirme gerektirir veya (başarıyla veya hata) tamamlar.

Bu görevleri, aşağıdaki bölümlerde ayrıntılı olarak ele alınacaktır.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

`WKApplicationRefreshBackgroundTask` İleriki bir tarihte woken uygulamanız için zamanlanan genel bir görevdir:

[ ![](background-tasks-images/update04.png "İleriki bir tarihte woken WKApplicationRefreshBackgroundTask")](background-tasks-images/update04.png)

Görev çalışma zamanı içinde uygulama yerel işleme güncelleştirme gibi her türlü olası zaman çizelgesi yapabilir veya bazı gerekli verilerle fetch bir `NSUrlSession`.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

Sistem göndereceğiniz bir `WKURLSessionRefreshBackgroundTask` verileri bittiği indiriliyor ve uygulama tarafından işlenmesi hazır:

[ ![](background-tasks-images/update05.png "Veri yükleme bittiğinde WKURLSessionRefreshBackgroundTask")](background-tasks-images/update05.png)

Bir uygulama veri arka planda yüklenirken çalışır durumda kalır değil. Bunun yerine, uygulama istek verileri için zamanlar sonra askıya alınmış ve sistem karşıdan yüklenmesi tamamlandığında uygulamayı yalnızca reawakening verileri işler.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

WatchOS 3'de, Apple, kullanıcıların sık kullanılan uygulamalarını sabitleyin ve hızlı bir şekilde erişmesine yuva ekledi. Kullanıcının Apple Watch yan düğmesine bastığında sabitlenmiş uygulama anlık görüntü Galerisi görüntülenir. Kullanıcı istediğiniz uygulamayı bulun ve ardından anlık görüntü çalışan uygulamanın arabirimi ile değiştirerek başlatmak için uygulama dokunun için sola veya sağa doğru çekin.

[ ![](background-tasks-images/update06.png "Anlık görüntü çalışan uygulamaları arabirimiyle değiştirme")](background-tasks-images/update06.png)

Sistem anlık görüntüleri, uygulamanın kullanıcı arabirimini düzenli aralıklarla alır (göndererek bir `WKSnapshotRefreshBackgroundTask`) ve bu anlık görüntülerin yuva doldurmak için kullanır. watchOS uygulama içeriği ve kullanıcı Arabirimi bu anlık görüntü oluşturulduğunda önce güncelleştirme olanağı sağlar.

Uygulama için Önizleme ve başlatma görüntüleri olarak çalıştıklarının beri anlık görüntüleri watchOS 3 çok önemlidir. Bir yuva uygulamasında kullanıcı kapatır değilse, tam ekrana genişletin, ön girin ve çalıştırma, anlık görüntünün güncel zorunludur şekilde başlatın:

[ ![](background-tasks-images/update07.png "Bir yuva uygulamasında kullanıcı kapatır, tam ekrana genişletecek")](background-tasks-images/update07.png)

Sistem yeniden verecek bir `WKSnapshotRefreshBackgroundTask` uygulama (verileri ve kullanıcı arabirimini güncelleştirerek) önce hazırlanabilmeniz adına, anlık görüntü alınır:

[ ![](background-tasks-images/update08.png "Uygulama verileri ve kullanıcı Arabirimi anlık görüntü alınırken önce güncelleştirerek hazırlama")](background-tasks-images/update08.png)

Uygulamanın ne zaman işaretler `WKSnapshotRefreshBackgroundTask` tamamlandı, sistemi otomatik olarak uygulamanın UI Snapshot götürecektir.

> [!IMPORTANT]
> **Not:** her zaman zamanlamak önemli bir ` WKSnapshotRefreshBackgroundTask` uygulamayı yeni veri aldı ve kendi kullanıcı arabirimi güncelleştirildi veya kullanıcı değiştirilmiş bilgileri göremez sonra.




Kullanıcı uygulamasından bir bildirim alır ve bunu uygulama ön plana çıkarmak için dokunur ek olarak, anlık görüntü de başlatma ekranı davranan beri güncel olması gerekir:

[ ![](background-tasks-images/update09.png "Kullanıcı uygulamasından bir bildirim alır ve bunu uygulama ön plana çıkarmak için dokunur")](background-tasks-images/update09.png)

Kullanıcı bir watchOS uygulaması ile etkileşime bu yana bir saatten daha uzun olması durumunda varsayılan durumuna döndürmek kuramaz. Varsayılan durumu farklı uygulamalara farklı anlamlara ve bir uygulama tasarımını bağlı olarak, bu varsayılan durumu hiç sahip olmayabilir.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

WatchOS 3'de, Apple izleme bağlantı arka planda yenileme API yeni üzerinden ile tümleşik olan `WKWatchConnectivityRefreshBackgroundTask`. WatchOS uygulama arka planda çalıştığı sırada bu yeni özelliği kullanarak, iPhone uygulama yeni veri izleme uygulama karşılığı için iletebilirsiniz:

[ ![](background-tasks-images/update10.png "WatchOS uygulama arka planda çalıştığı sırada bir iPhone uygulama izleme uygulama karşılığı için yeni veri sunabilir")](background-tasks-images/update10.png)

Olası itme başlatan, uygulama bir dosya göndermek veya iPhone uygulamasını kullanıcı bilgilerini güncelleştirme bağlamında Apple Watch uygulama arka planda çağırır.

Ne zaman izleme uygulama woken aracılığıyla bir `WKWatchConnectivityRefreshBackgroundTask` , iPhone uygulamadan verileri almak için standart API yöntemlerini kullanmanız gerekecektir.

[ ![](background-tasks-images/update11.png "WKWatchConnectivityRefreshBackgroundTask veri akışı")](background-tasks-images/update11.png)

1. Oturum etkinleştirdi emin olun.
2. Yeni izleme `HasContentPending` özellik değeri olduğu sürece `true`, uygulama hala işlemek için veri içeriyor. Tüm verileri işleme tamamlanana kadar olarak daha önce uygulama görev tutun.
3. İşlenecek başka veri yok olduğunda (`HasContentPending = false`), sisteme döndürmek için Tamamlanan görevin işaretleyin. Bunu yapmak başarısız bir kilitlenme raporu kaynaklanan uygulamanın ayrılan arka plan çalışma zamanı tüketebilir.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>Arka plan API yaşam döngüsü

Tüm yeni arka plan görevleri API parçasını birlikte yerleştirme, etkileşimleri tipik bir dizi aşağıdaki gibi görünür:

[ ![](background-tasks-images/update12.png "Arka plan API yaşam döngüsü")](background-tasks-images/update12.png)

1. İlk olarak, watchOS uygulama bir arka plan olması uykuya geçme için bazı noktası olarak gelecekte görev zamanlar.
2. Uygulama sistem tarafından woken ve bir görev gönderilir.
3. Uygulamayı hangi iş gerekli görevin işler.
4. Sonuç olarak görev işleme uygulama daha fazla arka plan görevlerinin kullanarak daha fazla içerik indirme gibi daha fazla iş gelecekte tamamlanması zamanlamanız gerekebilir bir `NSUrlSession`.
5. Uygulama görevi tamamlandı işaretler ve sisteme döndürür.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>Kaynakları sorumlu bir şekilde kullanma

WatchOS uygulama sorumlu bir şekilde bu ekosistem içinde kendi boşaltma sistemin paylaşılan kaynaklar sınırlayarak davranmasını önemlidir.

Aşağıdaki senaryoyu göz atın:

[ ![](background-tasks-images/update13.png "Kendi boşaltma sistemin paylaşılan kaynaklar watchOS uygulama sınırlar")](background-tasks-images/update13.png)

1. Kullanıcı watchOS uygulama 1: 00'da başlatılır.
2. Uygulama uyandırır ve bir saat 2: 00'dan en yeni içerik indirmek için bir görev zamanlar.
3. 13: 50'te kullanıcı verileri ve kullanıcı Arabirimi şu anda güncelleştirme olanak sağlayan uygulama yeniden açar.
4. Görev uyku modundan çıkarma uygulama 10 dakika içinde tekrar yapmasına izin vermek yerine uygulama görevi daha sonra 2: 50'e bir saatte çalışacak şekilde yeniden zamanla.

Her uygulama farklı olsa da, Apple desenlerini kullanımı, bu gösterildiği gibi sistem kaynaklarının korunmasına yardımcı olmak için yukarıdaki bulma önerir.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>Arka plan görevleri uygulama

Örnek amacıyla, bu belgede futbol puanları kullanıcıya raporları sahte MonkeySoccer Spor uygulama kullanır. 

Aşağıdaki tipik kullanım senaryosu göz atın:

[ ![](background-tasks-images/update14.png "Tipik kullanım senaryosu")](background-tasks-images/update14.png)

Kullanıcının sık kullanılan futbol takım 9:00 PM 19:00: 00'dan büyük bir eşleşme uygulama puanı düzenli olarak kontrol edilmesi kullanıcıya beklemeniz gerekir ve bir 30 dakikalık güncelleştirme aralığı karar yürütüyor.

1. Kullanıcı uygulamayı açar ve 30 dakika sonra arka planda güncelleştirmesi için bir görev zamanlar. Arka plan API yalnızca bir tür arka plan görevi, belirli bir zamanda çalışıyor olmasını sağlar.
2. Uygulama görev alır ve kendi veri ve kullanıcı Arabirimi güncelleştirir, ardından zamanlamaları için başka bir görev 30 dakika sonra arka plan. Başka bir arka plan görevi zamanlamak Geliştirici hatırlıyor veya uygulama hiçbir zaman daha fazla güncelleştirmeleri almak için yeniden Uyandı önemlidir.
3. Yeniden uygulama görev alır ve kendi veri güncelleştirmeleri, kullanıcı Arabiriminde güncelleştirir ve başka bir arka plan görev 30 dakika sonra zamanlar.
4. Aynı işlemi yeniden tekrarlar.
5. Görev alınan son arka plan ve uygulamayı yeniden güncelleştirir, verileri ve kullanıcı Arabirimi. Bu Sonuç Skoru olduğu için yeni bir arka plan yenilemesi zamanlama değil. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>Arka plan güncelleştirmesi planlama

Yukarıdaki senaryoda verildiğinde, MonkeySoccer uygulama için bir arka plan güncelleştirme zamanlamak için aşağıdaki kodu kullanabilirsiniz:

```csharp
private void ScheduleNextBackgroundUpdate ()
{
    // Create a fire date 30 minutes into the future
    var fireDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

    // Create 
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("LastActiveDate"), NSDate.FromTimeIntervalSinceNow(0));
    userInfo.Add (new NSString ("Reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleBackgroundRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Yeni bir oluşturur `NSDate` gelecekteki olduğunda içine 30 dakika uygulama Uyandı ister ve oluşturur bir `NSMutableDictionary` istenen görev ayrıntılarını tutmak için. `ScheduleBackgroundRefresh` Yöntemi `SharedExtension` zamanlanmış görevini istemek için kullanılır.

Sistem döndürülecek bir `NSError` istenen görev zamanlama işleyemezse.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>Güncelleştirme işlemi

Ardından, daha yakından puanı güncelleştirmek için gereken adımları gösteren 5 dakikalık göz atın:

[ ![](background-tasks-images/update15.png "Puan güncelleştirmek için gereken adımları gösteren 5 dakikalık penceresi")](background-tasks-images/update15.png)

1. 19:30:02: 00, uygulama sistem tarafından uyandırdı ve güncelleştirme arka plan görevi verilir. İlk önceliği, sunucudan son puanları almaktır. Bkz: [bir NSUrlSession zamanlama](#Scheduling-a-NSUrlSession) aşağıda.
2. 7:30:05 uygulama özgün görev tamamlandığında, sistem uyku için uygulamaya koyar ve istenen veri arka planda karşıdan yüklemeye devam eder.
3. Sistem karşıdan yükleme tamamlandığında, indirilen bilgi işleyebilmek için uygulama uyandırmak için yeni bir görev oluşturur. Bkz: [arka plan görevleri işleme](#Handling-Background-Tasks) ve [Yükleme tamamlanıyor işleme](#Handling-the-Download-Completing) aşağıda. 
4. Uygulama güncelleştirilmiş bilgileri kaydeder ve Tamamlanan görevin işaretler. Ancak, bu işlem işlemek için bir anlık görüntü görev zamanlama Apple öneren Geliştirici şu anda, uygulamanın kullanıcı arabiriminde güncelleştirme isteği olabilir. Bkz: [bir anlık görüntü güncelleştirme zamanlama](#Scheduling-a-Snapshot-Update) aşağıda.
5. Uygulama anlık görüntü görevi alır, kendi kullanıcı arabirimini güncelleştirir ve Tamamlanan görevin işaretler. Bkz: [bir anlık görüntü güncelleştirme işleme](#Handling-a-Snapshot-Update) aşağıda.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>Bir NSUrlSession planlama

Aşağıdaki kod, en son puanlarını indirme zamanlamak için kullanılabilir:

```csharp
private void ScheduleURLUpdateSession ()
{
    // Create new configuration
    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.example.urlsession");

    // Create new session
    var backgroundSession = NSUrlSession.FromConfiguration (configuration);

    // Create and start download task
    var downloadTask = backgroundSession.CreateDownloadTask (new NSUrl ("https://example.com/gamexxx/currentScores.json"));
    downloadTask.Resume ();
}
```

Yeni bir oluşturur ve yapılandırır `NSUrlSession`, yeni bir yükleme oluşturmak için bu oturumu kullanır kullanarak görev `CreateDownloadTask` yöntemi. Çağırır `Resume` oturumu başlatmak için görev yükleme yöntemi.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>İşleme arka plan görevleri

Geçersiz kılma tarafından `HandleBackgroundTasks` yöntemi `WKExtensionDelegate`, uygulama gelen arka plan görevleri işleyebilir:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
        #endregion

        ...
        
        #region Public Methods
        public void CompleteTask (WKRefreshBackgroundTask task)
        {
            // Mark the task completed and remove from the collection
            task.SetTaskCompleted ();
            PendingTasks.Remove (task);
        }
        #endregion 

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request
            foreach (WKRefreshBackgroundTask task in backgroundTasks) {
                // Is this a background session task?
                var urlTask = task as WKUrlSessionRefreshBackgroundTask;
                if (urlTask != null) {
                    // Create new configuration
                    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

                    // Create new session
                    var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

                    // Keep track of all pending tasks
                    PendingTasks.Add (task);
                } else {
                    // Ensure that all tasks are completed
                    task.SetTaskCompleted ();
                }
            }
        }
        #endregion
        
        ...
    }
}
```

`HandleBackgroundTasks` Yöntemi döngüleri tüm görevleri sistem uygulama gönderdi (içinde `backgroundTasks`) arama bir `WKUrlSessionRefreshBackgroundTask`. Bulunması durumunda oturumun kümeyle ve ekler bir `NSUrlSessionDownloadDelegate` Yükleme tamamlanıyor işlemek için (bkz [karşıdan yükleme Tamamlanıyor işleme](#Handling-the-Download-Completing) aşağıda):

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

Bir koleksiyona ekleyerek tamamlanana kadar görevde bir tanıtıcı tutar:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

Uygulamaya gönderilen görevlerin tümüne şu anda işlenmekte olan herhangi bir görev için tamamlanması gereken tam işaretlemeniz:

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>Yükleme tamamlanıyor işleme

Aşağıdaki MonkeySoccer uygulamanın kullandığı `NSUrlSessionDownloadDelegate` Yükleme tamamlanıyor işlemek ve istenen verileri işlemek için temsilci:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class BackgroundSessionDelegate : NSUrlSessionDownloadDelegate
    {
        #region Computed Properties
        public ExtensionDelegate WatchExtensionDelegate { get; set; }

        public WKRefreshBackgroundTask Task { get; set; }
        #endregion

        #region Constructors
        public BackgroundSessionDelegate (ExtensionDelegate extensionDelegate, WKRefreshBackgroundTask task)
        {
            // Initialize
            this.WatchExtensionDelegate = extensionDelegate;
            this.Task = task;
        }
        #endregion

        #region Override Methods
        public override void DidFinishDownloading (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, NSUrl location)
        {
            // Handle the downloaded data
            ...

            // Mark the task completed
            WatchExtensionDelegate.CompleteTask (Task);

        }
        #endregion
    }
}
```

Hazırlarken, onu bir tanıtıcı hem de tutar `ExtensionDelegate` ve `WKRefreshBackgroundTask` , kökenli onu. Bu geçersiz kılmaları `DidFinishDownloading` Yükleme tamamlanıyor işlemek için yöntem. Daha sonra kullanır `CompleteTask` yöntemi `ExtensionDelegate` bildirmek için görev onun tamamlandı ve beklemedeki görevlerin koleksiyondan kaldırın. Bkz: [arka plan görevleri işleme](#Handling-Background-Tasks) üstünde.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>Bir anlık görüntü güncelleştirme planlama

Aşağıdaki kod, kullanıcı Arabirimi ile en son puanları güncelleştirmek için bir anlık görüntü görev zamanlamak için kullanılabilir:

```csharp
private void ScheduleSnapshotUpdate ()
{
    // Create a fire date of now
    var fireDate = NSDate.FromTimeIntervalSinceNow (0);

    // Create user info dictionary
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
    userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleSnapshotRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Olduğu gibi `ScheduleURLUpdateSession` yöntemi yukarıdaki oluşturduğu yeni bir `NSDate` zaman uygulama Uyandı ister ve oluşturur için bir `NSMutableDictionary` istenen görev ayrıntılarını tutmak için. `ScheduleSnapshotRefresh` Yöntemi `SharedExtension` zamanlanmış görevini istemek için kullanılır.

Sistem döndürülecek bir `NSError` istenen görev zamanlama işleyemezse.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>Bir anlık görüntü güncelleştirme işleme

Anlık görüntü görevi işlemek için `HandleBackgroundTasks` yöntemi (bkz [işleme arka plan görevleri](#Handling-Background-Tasks) yukarıda) şu şekilde görünür şekilde değiştirilir:

```csharp
public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
{
    // Handle background request
    foreach (WKRefreshBackgroundTask task in backgroundTasks) {
        // Take action based on task type
        if (task is WKUrlSessionRefreshBackgroundTask) {
            var urlTask = task as WKUrlSessionRefreshBackgroundTask;

            // Create new configuration
            var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

            // Create new session
            var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

            // Keep track of all pending tasks
            PendingTasks.Add (task);
        } else if (task is WKSnapshotRefreshBackgroundTask) {
            var snapshotTask = task as WKSnapshotRefreshBackgroundTask;

            // Update UI
            ...

            // Create a expiration date 30 minutes into the future
            var expirationDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

            // Create user info dictionary
            var userInfo = new NSMutableDictionary ();
            userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
            userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

            // Mark task complete
            snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
        } else {
            // Ensure that all tasks are completed
            task.SetTaskCompleted ();
        }
    }
}
```

Yöntemi, işlenmekte olan görev türü için sınar. Eğer öyleyse bir `WKSnapshotRefreshBackgroundTask` görev erişim kazanır:

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

Yöntem kullanıcı arabirimini güncelleştirir sonra oluşturur bir `NSDate` anlık görüntü eski olduğunda sistem bildirmek için. Oluşturduğu bir `NSMutableDictionary` işaretleri anlık görüntü görevi bu bilgilerle tamamlandı ve yeni bir anlık görüntü açıklamak için kullanıcı bilgileri ile:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

Ayrıca, bu da anlık görüntü görev uygulama (ilk parametresinde) varsayılan durumuna vermeyen bildirir. Varsayılan durumu kavramına sahip uygulamalar her zaman ayarlamalıdır bu özellik `true`.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>Verimli çalışma

Yukarıdaki örnekte kendi puanları verimli çalışma ve yeni watchOS kullanarak güncelleştirmek için MonkeySoccer uygulama sürdü beş dakikalık penceresinin 3 arka plan görevleri görüldüğü gibi uygulamayı yalnızca 15 saniye toplam için etkin olan: 

[ ![](background-tasks-images/update16.png "Uygulamanın yalnızca 15 saniye cinsinden toplam etkin")](background-tasks-images/update16.png)

Bu uygulamaya hem kullanılabilir Apple Watch kaynakları hem de pil ömrünün sahip etkisini azaltır ve ayrıca daha iyi izleme üzerinde çalışan diğer uygulamalarla çalışma yazmasına izin verir.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>Zamanlama nasıl çalışır

WatchOS 3 uygulama ön planda olsa da, her zaman çalıştırın ve işleme gibi veri güncelleştirme gerekli herhangi bir türde yapabilir veya kendi kullanıcı Arabirimi yeniden boyutlandırmaya üzere zamanlandı. Uygulama arka plan içine geçtiğinde, genellikle sistem tarafından askıya alınır ve tüm çalışma zamanı işlemleri durdu. 

Uygulama arka planda olsa da, belirli bir görevi hızlı bir şekilde çalıştırmak için sistem tarafından hedeflenen. Bu nedenle, watchOS 2'de, sistem geçici olarak bir arka plan uygulaması uzun görünüm bildirimini işleme gibi şeyler veya uygulamanın olası güncelleştirmek için Uyandırma. WatchOS 3'de, bir uygulama arka planda çalıştırabilirsiniz birkaç yeni yolu vardır.

Sistem uygulama arka planda olsa da, bazı sınırlamalar getirir:

- Belirli bir görevi tamamlamak için birkaç saniye yalnızca verilir. Sistem, yalnızca geçirilen süre, ancak de ayrıca ne kadar CPU gücü uygulama bu sınırı çıkarmaya tüketen dikkate alır.
- Kendi sınırlarını aşıyor herhangi bir uygulama, aşağıdaki hata kodlarıyla sonlandırılacak:
    - **CPU** - 0xc51bad01
    - **Zaman** -0xc51bad02
- Sistem, arka plan görevi gerçekleştirmek için app istedi türüne göre farklı sınırlar zorunlu tuttukları. Örneğin, `WKApplicationRefreshBackgroundTask` ve `WKURLSessionRefreshBackgroundTask` görevleri arka plan görevlerini diğer türleri üzerinde biraz daha uzun çalışma zamanları verilmiştir.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>Zorluklar ve uygulama güncelleştirmeleri

Yeni arka plan Apple watchOS 3 eklediği görevlere ek olarak, bir watchOS uygulamanın zorluklar nasıl ve ne zaman uygulama arka plan güncelleştirmeleri alır üzerinde bir etkisi olabilir.

Zorluklar bir bakışta yararlı bilgiler sağlayan küçük visual öğelerdir. Seçilen gözlem yüz bağlı olarak, kullanıcı bir izleme yüz watchOS 3 izleme uygulamada tarafından sağlanan bir veya daha fazla olası ile özelleştirmek için özelliğine sahiptir.

Kullanıcı kendi izleme yüz üzerinde uygulamanın zorluklar içeriyorsa, güncelleştirilmiş aşağıdaki uygulama verir yararları:

- Uygulama bir hazır-için başlatılan içinde tutmak sistem neden olduğu deneme arka planda uygulamayı başlatmak için durumu, tutar, bu, bellek ve çok zaman güncelleştirmek için sağlar.
- Zorluklar günde en az 50 itme güncelleştirmeleri sağlanır.

Her zaman geliştirici, kendi izleme yüz yukarıda listelenen nedenlerden dolayı eklemeden içine kullanıcı ikna için ilgi çekici zorluklar uygulamalarını için oluşturmak ister.

WatchOS 2'de, zorluklar bir uygulama çalışma zamanı sırasında arka planda alınan birincil yolu yoktu. WatchOS 3'de, olası uygulama hala saat başına birden çok güncelleştirme almaya güvence altına, ancak kullanabilirsiniz `WKExtensions` kendi zorluklar güncelleştirmek için daha fazla çalışma zamanı istemek için.

Olası bağlı iPhone uygulamadan güncelleştirmek için kullanılan aşağıdaki kodu göz atın:

```csharp
using System;
using WatchConnectivity;
using UIKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
...

private void UpdateComplication ()
{

    // Get session and the number of remaining transfers
    var session = WCSession.DefaultSession;
    var transfers = session.RemainingComplicationUserInfoTransfers;

    // Create user info dictionary
    var iconattrs = new Dictionary<NSString, NSObject>
        {
            {new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0)},
            {new NSString ("reason"), new NSString ("UpdateScore")}
        };

    var userInfo = NSDictionary<NSString, NSObject>.FromObjectsAndKeys (iconattrs.Values.ToArray (), iconattrs.Keys.ToArray ());

    // Take action based on the number of transfers left
    if (transfers < 1) {
        // No transfers left, either attempt to send or inform
        // user of situation.
        ...
    } else if (transfers < 11) {
        // Running low on transfers, only send on important updates
        // else conserve for a significant change.
        ...
    } else {
        // Send data
        session.TransferCurrentComplicationUserInfo (userInfo);
    }
}
```

Kullandığı `RemainingComplicationUserInfoTransfers` özelliği `WCSession` kaç yüksek öncelikli uygulama aktarır görmek için kalan gün ve ardından bu sayıya göre alır eylemi için. Uygulama üzerinde aktarımları azalmaya başladığında, bu küçük güncelleştirmeler göndermeye bekletir ve önemli bir değişiklik olduğunda yalnızca bilgi gönderin.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>Zamanlama ve yerleştirme

WatchOS 3'de, Apple, kullanıcıların sık kullanılan uygulamalarını sabitleyin ve hızlı bir şekilde erişmesine yuva ekledi. Kullanıcının Apple Watch yan düğmesine bastığında sabitlenmiş uygulama anlık görüntü Galerisi görüntülenir. Kullanıcı istediğiniz uygulamayı bulun ve ardından anlık görüntü çalışan uygulamanın arabirimi ile değiştirerek başlatmak için uygulama dokunun için sola veya sağa doğru çekin.

[ ![](background-tasks-images/dock01.png "Yerleştirme")](background-tasks-images/dock01.png)

Sistem, düzenli aralıklarla uygulamanın UI anlık görüntüleri alır ve bu anlık görüntülerin belgeleri doldurmak için kullanır. watchOS uygulama içeriği ve kullanıcı Arabirimi bu anlık görüntü oluşturulduğunda önce güncelleştirme olanağı sağlar.

Yuva sabitlenmiş uygulamaları aşağıdaki bekleyebilirsiniz:

- Bunların en az bir saat başına güncelleştirilmiş alır. Bu, bir uygulama görev yenileyin ve bir anlık görüntü görev içerir.
- Güncelleştirme bütçe tüm yuva uygulamalar arasında dağıtılır. Bu nedenle daha az uygulamalar her uygulama alacak daha olası güncelleştirmeler kullanıcı sabitlenmiş.
- Uygulamayı hızla yuvasından seçildiğinde sürdürecek şekilde uygulama bellekte tutulacak.

Kullanıcı çalıştırdı son uygulama kabul _en son kullanılan_ uygulama ve yuva son yuvasında kaplar. Buradan var. kullanıcı yuva kalıcı olarak sabitlemek seçebilirsiniz. Tüm diğer sık kullanılan uygulama kullanıcı Yuva zaten sabitlenmiş gibi en son kullanılan kabul edilir.

> [!IMPORTANT]
> **Not:** yalnızca giriş ekranına eklenen uygulamalar değil verilir düzenli bir zamanlama. Normal zamanlama ve arka plan almaya güncelleştirmeleri, bir uygulama _gerekir_ yuva eklenebilir.

Uygulama için Önizleme ve başlatma görüntüleri olarak çalıştıklarının beri bu belgede daha önce belirtildiği gibi anlık görüntüleri watchOS 3 çok önemlidir. Bir yuva uygulamasında kullanıcı kapatır değilse, tam ekrana genişletin, ön girin ve çalıştırma, anlık görüntünün güncel zorunludur şekilde başlatın.

Sistem, yeni bir anlık görüntüsünü uygulamanın kullanıcı Arabirimi gerektiği zaman karar zamanlar olabilir. Bu durumda, anlık görüntü isteği uygulamanın çalışma zamanı bütçe karşı sayar değil. Aşağıdaki sistem anlık görüntü isteği tetikler:

- Olası zaman çizelgesi güncelleştirme.
- Bir uygulamanın bildirim ile kullanıcı etkileşimi.
- Ön arka plan duruma geçiş.
- Arka plan durumda olan bir saat sonra böylece uygulama varsayılan durumuna döndürebilir.
- İlk watchOS önyüklendiğinde.

<a name="Best-Practices" />

## <a name="best-practices"></a>En İyi Yöntemler 

Apple arka plan görevleri ile çalışırken aşağıdaki en iyi yöntemler önerir:

- Uygulama güncelleştirilmesi gerekiyor gibi genellikle zamanlayın. Uygulama her çalıştığında, gelecekteki ihtiyaçlarına yeniden değerlendirmek ve bu zamanlamanın gerektiği gibi ayarlayın.
- Bir güncelleştirme gerçekten gerekli olana kadar sistem Yenile bir arka plan görevi gönderir ve uygulamayı bir güncelleştirme gerektirmez, iş erteleyin.
- Bir uygulama için kullanılabilen tüm çalışma zamanı olanakları göz önünde bulundurun:
    - Yerleştirme ve ön plan etkinleştirme.
    - Bildirimler.
    - Olası güncelleştirmeler.
    - Arka plan yenilemeleri.
- Kullanım `ScheduleBackgroundRefresh` gibi genel amaçlı arka plan çalışma zamanı için:
    - Sistem bilgileri için yoklanıyor.
    - Zamanlama gelecekteki `NSURLSessions` istek arka plan verileri. 
    - Bilinen zaman geçişleri.
    - Tetikleyici olası güncelleştirmeler.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>Anlık görüntü en iyi uygulamalar

Anlık görüntü güncelleştirme ile çalışırken aşağıdaki önerilerden Apple yapar:

- Önemli bir içerik değişiklik olduğunda yalnızca, örneğin, gerekli olduğunda anlık görüntüleri geçersiz.
- Yüksek yoğunlukta anlık görüntüyü geçersiz kılma kaçının. Örneğin, bir zamanlayıcı uygulama saniyede anlık görüntü güncelleştirme döndürmemelidir, Zamanlayıcı sonlandığında yalnızca yapılmalıdır.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>Uygulama veri akışı

Apple veri akışı ile çalışmak için şunları öneririz:

[ ![](background-tasks-images/update17.png "Uygulama verileri Akış Diyagramı")](background-tasks-images/update17.png)

Bir dış olay (örneğin, izleme Connectivity) uygulama uyandırır. Bu, veri (uygulamaların geçerli durumunu temsil eden) modeli güncelleştirmek için uygulama zorlar. Sonuç olarak veri modeli değişiklik uygulama kendi zorluklar güncellemeniz gerekir istek yeni bir anlık görüntü, büyük olasılıkla bir arka plan başlangıç `NSURLSession` daha fazla veri çekmek ve daha fazla arka plan zamanlamak için yeniler.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>Uygulama yaşam döngüsü

Yuva ve sık kullanılan uygulamalar için kullanıcıların çok daha fazla uygulamalar arasında taşıma Apple düşündüğü, PIN yeteneği nedeniyle genellikle daha sonra watchOS 2 ile menülerin. Sonuç olarak, uygulama bu değişikliği işlemek ve ön ve arka plan durumlar arasında hızlıca taşımak hazır olmanız gerekir.

Apple aşağıdaki önerileri vardır:

- Uygulama mümkün olan en kısa sürede ön plan etkinleştirme girme bağlı tüm arka plan görev tamamlandığında emin olun.
- Tüm ön plan iş tamamlanmasını sağlamak çağırarak arka girmeden önce `NSProcessInfo.PerformExpiringActivity`.
- Bir uygulamayı düzgün bir özelliğini sınamak için gerektiği kadar yenileyebilmesi watchOS Simulator bir uygulamayı test edilirken görev bütçeleri hiçbiri zorlanır.
- Uygulamayı yayımlamadan önce kendi bütçeleri geçmiş iTunes Bağlan çalışmadığından emin olmak için gerçek Apple Watch donanımda her zaman sınayın.
- Apple, Apple Watch şarj üzerinde test ve hata ayıklama sırasında tutma önerir.
- Hem soğuk başlatarak ve uygulama sürdürme throughly sınanan emin olun.
- Tüm uygulama görevler tamamlanıp tamamlanmadığını doğrulayın.
- En iyi ve en kötü test etmek için yuva sabitlenmiş uygulama sayısı değişir durumda senaryoları.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, Apple watchOS ve nasıl bir izleme uygulaması güncel tutmak için kullanılabilmesi için yapılan geliştirmeler kapsamına. İlk olarak, tüm yeni kapsanan arka plan görevi Apple watchOS 3 ekledi. Ardından, arka plan API yaşam döngüsü ve arka plan görevleri, bir Xamarin watchOS uygulamasında uygulamak nasıl ele. Son olarak, nasıl zamanlama works ele ve bazı en iyi uygulamalar vermiş oldunuz.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
