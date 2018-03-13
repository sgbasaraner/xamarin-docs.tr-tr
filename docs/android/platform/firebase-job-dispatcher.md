---
title: "Firebase iş dağıtıcı"
description: "Bu kılavuz, Google Firebase iş dağıtıcı kitaplığından kullanarak arka plan iş zamanlama açıklanır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: fd5b2f8c758d8e1e9bb9276da96a410c61478d4a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="firebase-job-dispatcher"></a>Firebase iş dağıtıcı

_Bu kılavuz, Google Firebase iş dağıtıcı kitaplığından kullanarak arka plan iş zamanlama açıklanır._

## <a name="firebase-job-dispatcher-overview"></a>Firebase iş dağıtıcı genel bakış

Bir Android uygulaması yanıt vereceğini kullanıcıya tutmak için iyi yollarından biri, karmaşık veya uzun süre çalışan iş arka planda gerçekleştirilir sağlamaktır. Bununla birlikte, arka plan çalışması olumsuz kullanıcı deneyimini aygıtla etkilemez olduğunu önemlidir. 

Örneğin, arka plan işi sorgu için birkaç dakikada değişiklikler belirli bir veri kümesi için bir Web sitesi yoklama. Cihazda felaket niteliğinde bir etkisi olabilir ancak bu zararsız, gibi görünüyor. Uygulama aygıtı Uyandırma, daha yüksek bir güç durumu CPU artırmasını Radyoları destekleyen ağ istekleri yapan ve sonuçlar işlenirken yukarı sona erer. Aygıt hemen gücü kapatın ve düşük güç boşta durumuna döndürmek için kötü alır. Kötü zamanlanmış arka plan çalışması durumunda gereksiz ve aşırı güç gereksinimleri ile cihaz yanlışlıkla tutabilir. Etkili bir şekilde (bir Web sitesi yoklama) bu seeming zararsız etkinlik aygıt daha kısa bir süre içinde kullanılamaz hale getirecek.

Android zaten ancak bunlar hiçbirine kapsamlı bir çözüm arka planda, işi gerçekleştirerek ile yardımcı olmak için çeşitli API'ler sağlar:

* **[Hedefi Hizmetleri](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; çalışması zamanlamak için bir yol sağlar ancak amacı Hizmetleri iş gerçekleştirmek için harika.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager)**  &ndash; bu API'ler yalnızca zamanlanması, ancak gerçekte çalışmayı gerçekleştirmek üzere bir yolunu sağlar çalışmaya izin verir. Ayrıca, AlarmManager yalnızca temel saat kısıtlamaları, belirli bir zamanda veya belirli bir süre geçtikten sonra bir alarm Yükselt anlamına gelir izin verir. 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash; JobSchedule olan işlerini zamanlamak için işletim sistemiyle çalışan harika bir API. Ancak, bunu yalnızca API düzeyi 21 hedef bu Android uygulamaları için veya yüksek kullanılabilir. 
* **[Yayın alıcıları](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; bir Android uygulaması yanıt veya sistem geniş olayları hedefleri olarak işlerini yapmak için yayın alıcıları kurulumu. Ancak, yayın alıcıları işin ne zaman çalıştırılması gerektiğini üzerinden herhangi bir denetimi sağlamıyorsa. Ayrıca Android işletim sistemindeki değişiklikler kısıtlar yayın alıcıları çalışırken ya da yanıt verebilir iş tür. 
* **Google bulut ileti Ağ Yöneticisi'ni** &ndash; , bu, tartışmaya açık bir şekilde, uzun bir süredir akıllıca zamanlama arka plan en iyi şekilde çalışır. Ancak, GCMNetworkManager beri kullanım dışıdır. 

Etkin arka plan iş gerçekleştirmenin iki anahtar özellikler vardır (bazen denir bir _arka plan işi_ veya _iş_):

1. **Akıllıca iş zamanlama** &ndash; uygulamanın iş arka planda, gerçekleştirdiğinde bunu iyi vatandaşı mu olduğunu önemlidir. İdeal olarak, uygulama bir işin çalıştırılması talep değil. Bunun yerine, uygulama koşullar karşılandığında çalıştırmak için çalışan iş çalıştırın ve ardından, zamanlamak için karşılanması gereken koşulları belirtmeniz gerekir. Bu, akıllıca işlerini yapmak Android sağlar. Örneğin, ağ istekleri tüm ek yükü en fazla kullanılmasını ağ ile ilgili sağlamak için aynı anda çalıştırmak için toplu.
2. **İş Kapsüllenen** &ndash; arka plan çalışması gerçekleştirmek için kod bağımsız olarak kullanıcı arabirimi çalıştırmak ve tamamlamak iş başarısız olursa yeniden zamanlamak görece olarak daha kolay bir ayrık bileşeninde kapsüllenmiş Bazı nedenlerden dolayı.

Firebase iş dağıtıcı zamanlama arka plan iş basitleştirmek için akıcı bir API sağlar Google kitaplıktan ' dir. Google bulut Yöneticisi için değiştirme olması amaçlanmıştır. Firebase iş dağıtıcı aşağıdaki API'lerinin oluşur:

* A `Firebase.JobDispatcher.JobService` arka plan iş mantığı ile genişletilmelidir bir Özet sınıf.
* A `Firebase.JobDispatcher.JobTrigger` iş yeniden başlatıldığında bildirir. Bu genellikle bir zaman penceresi örneğin ifade edilir, işi başlatmadan önce en az 30 saniye bekleyin, ancak iş 5 dakika içinde çalışır.
* A `Firebase.JobDispatcher.RetryStrategy` doğru şekilde çalışabilmesi bir işi başarısız olduğunda ne yapılmalı hakkındaki bilgileri içerir. Yeniden deneme stratejisini işi yeniden çalıştırmak denemeden önce beklenecek süreyi belirtir. 
* A `Firebase.JobDispatcher.Constraint` şarj ya da iş çalıştırmadan önce gibi cihaz unmetered bir ağ üzerinde karşılanması gereken bir koşul açıklayan isteğe bağlı bir değerdir.
* `Firebase.JobDispatcher.Job` Bir--tarafından zamanlanan iş birimi için önceki API'leri birleştiren bir API'dir `JobDispatcher`. `Job.Builder` Sınıfı örneği oluşturmak için kullanılan bir `Job`.
* A `Firebasee.JobDispatcher.JobDispatcher` işletim sistemiyle iş zamanlama ve gerekirse işleri iptal etmek için bir yol sağlamak için önceki üç API'lerini kullanır.

Firebase iş dağıtıcı iş zamanlamak için bir Xamarin.Android uygulaması genişleten bir türü kodda sarmalamalıdır `JobService` sınıfı. `JobService` üç yaşam döngüsü yöntemlerine sahiptir, iş ömrü boyunca çağrılabilir:

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; Bu, iş yeri gerçekleşir ve her zaman uygulanmalıdır yöntemidir. Ana iş parçacığı üzerinde çalışır. Bu yöntem döndürülecek `true` varsa kalan, iş veya `false` iş yapıldığında. 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; Bu işin herhangi bir nedenden dolayı durdurulduğunda çağrılır. Döndürme zorunluluğu `true` iş için daha sonra yeniden durumunda.
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; Bu yöntem aldığında çağrılan `JobService` herhangi bir zaman uyumsuz iş bitirdi. 

Bir işi zamanlamak için uygulama örneği bir `JobDispatcher` nesnesi. Ardından, bir `Job.Builder` oluşturmak için kullanılan bir `Job` için sağlanan nesne `JobDispatcher` hangi deneyin ve işini çalışmak üzere zamanlayabilirsiniz.

Bu kılavuz, bir Xamarin.Android uygulamasına Firebase iş dağıtıcı ekleme ve arka plan çalışması zamanlamak için kullanmak üzere nasıl ele alınacaktır.

## <a name="requirements"></a>Gereksinimler

Firebase iş dağıtıcı Android API düzeyi 9 veya üstü gerektirir. Google Play Hizmetleri tarafından sağlanan bazı bileşenleri Firebase iş dağıtıcı kitaplığı kullanır; cihaz Google Play Hizmetleri'nin yüklü olması gerekir.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Xamarin.Android Firebase iş dağıtıcı kitaplıkta kullanma

Firebase iş dağıtıcı ile çalışmaya başlamak için öncelikle eklemek [Xamarin.Firebase.JobDispatcher NuGet paketi](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher/0.6.0-beta1) Xamarin.Android projesi. NuGet Paket Yöneticisi için arama **Xamarin.Firebase.Jobdispatcher** paket.  

Firebase iş dağıtıcı kitaplığı eklendikten sonra oluşturma bir `JobService` sınıfı ve örneği ile çalıştırmak için zamanlama `FirebaseJobDispatcher`.

### <a name="creating-a-jobservice"></a>Oluşturma bir `JobService`

Firebase iş dağıtıcı kitaplığı tarafından gerçekleştirilen tüm iş genişleten bir türü yapılmalıdır `Firebase.JobDispatcher.JobService` soyut sınıf. Oluşturma bir `JobService` oluşturmaya çok benzer bir `Service` Android framework ile: 

1. Genişletme `JobService` sınıfı
2. Alt sınıfla tasarlamanız `ServiceAttribute`. Kesinlikle olmadığında gerekli olsa da, açıkça ayarlamak için önerilir `Name` hata ayıklamaya yardımcı olması için parametre `JobService`. 
3. Ekleme bir `IntentFilter` bildirmek için `JobService` içinde **AndroidManifest.xml**. Bu ayrıca bulun ve çağırma Firebase iş dağıtıcı kitaplığı yardımcı olacak `JobService`.

Aşağıdaki kod basit örneğidir `JobService` bir uygulama için:

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // Note: This runs on the main thread. Anything that takes longer than 16 milliseconds
         // should be run on a seperate thread.
        
        return false; // return false because there is no more work to do.
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>Oluşturma bir `FirebaseJobDispatcher`

Herhangi bir iş zamanlanabilmesi için önce oluşturmak gerekli olan bir `Firebase.JobDispatcher.FirebaseJobDispatcher` nesnesi. `FirebaseJobDispatcher` Zamanlama için sorumlu olduğu bir `JobService`. Aşağıdaki kod parçacığını bir örneğini oluşturmak için bir yoldur `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

Önceki kod parçacığında bulunan `GooglePlayDriver` yardımcı sınıf `FirebaseJobDispatcher` Google Play Hizmetleri'nin zamanlama API'lerinde bazıları cihazda etkileşim. Parametre `context` tüm Android olan `Context`, bir etkinlik gibi. Şu anda `GooglePlayDriver` tek `IDriver` Firebase iş dağıtıcı Kitaplığı'nda uygulama. 

Firebase iş dağıtıcı Xamarin.Android bağlama oluşturmak için bir genişletme yöntemi sağlar bir `FirebaseJobDispatcher` gelen `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Bir kez `FirebaseJobDispatcher` bırakıldı örneği, bu oluşturmak mümkündür bir `Job` ve kod çalıştırmadan `JobService` sınıfı. `Job` Tarafından oluşturulan bir `Job.Builder` nesne ve sonraki bölümde incelenecektir.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Oluşturma bir `Firebase.JobDispatcher.Job` ile `Job.Builder`

`Firebase.JobDispatcher.Job` Sınıftır meta veri şifreleme için sorumlu çalıştırmak gerekli bir `JobService`. A`Job` işi çalıştırmadan önce karşılanması gereken herhangi bir kısıtlama gibi bilgileri içeren `Job` tekrarlanan türde veya çalıştırılacak iş neden olacak hiçbir tetikleyici.  Tam bir en az olarak bir `Job` olmalıdır bir _etiketi_ (projeye tanımlayan benzersiz bir dize `FirebaseJobDispatcher`) ve türünü `JobService` , çalıştırılmalıdır. Firebase iş dağıtıcı örneği `JobService` işin çalıştırma süresi olduğunda.  A `Job` örneği kullanılarak oluşturulan `Firebase.JobDispatcher.Job.JobBuilder` sınıfı. 

Aşağıdaki kod parçacığını oluşturmak nasıl basit bir örnektir bir `Job` Xamarin.Android bağlama işlemini kullanma:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` İş için giriş değerleri üzerinde bazı temel doğrulama denetimleri gerçekleştirir. Bir özel durum için olası bir onu varsa oluşturulacaktır `Job.Builder` oluşturmak için bir `Job`.  `Job.Builder` Oluşturacak bir `Job` aşağıdaki varsayılanlar ile:

* A `Job`'s _ömrü_ (ne kadar süreyle bunu çalıştırmak için zamanlanacak) yalnızca aygıt yeniden başlatılır kadar olan &ndash; cihaz yeniden başlatıldıktan sonra `Job` kaybolur.
* A `Job` değil yinelenen &ndash; yalnızca bir kez çalışacaktır.
* A `Job` olabildiğince çabuk çalıştırmak üzere zamanlanmış.
* İçin varsayılan yeniden deneme stratejisini bir `Job` kullanmaktır bir _üstel geri alma_ (daha fazla ayrıntı bölümünde ele alınan [bir RetryStrategy ayarı](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>Zamanlama bir `Job`

Oluşturduktan sonra `Job`, ile zamanlanması gerekiyor `FirebaseJobDispatcher` , çalıştırılmadan önce. Zamanlama için iki yöntem vardır bir `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Tarafından döndürülen değer `FirebaseJobDispatcher.Schedule` aşağıdaki tamsayı değerlerden biri olacaktır:

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job` Başarıyla zamanlandı.
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; Engelleyen bazı bilinmeyen bir sorun oluştu `Job` zamanlanma gelen.
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; Geçersiz bir `IDriver` kullanıldı veya `IDriver` şekilde ulaşılamadı. 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger` Desteklenmiyordu.
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; Hizmet doğru şekilde yapılandırılmamış veya kullanılamıyor.
 
### <a name="configuring-a-job"></a>Bir proje yapılandırma

Bir işi özelleştirmek kullanılabilir. Bir işi nasıl özelleştirilebilir örnekler aşağıdakileri içerir:

* [Bir projeye parametreleri geçirme](#Passing_Parameters_to_a_Job) &ndash; A `Job` örneğin dosya indirme işini gerçekleştirmek için ek değerler gerektirebilir.
* [Ayarlanmış kısıtlamalar](#Setting_Constraints) &ndash; belirli koşullar karşılandığında yalnızca bir işi çalıştırmak gerekli olabilir. Örneğin, yalnızca çalıştırılan bir `Job` aygıtın ne zaman şarj. 
* [Belirten bir `Job` çalışması gerektiğini](#Setting_Job_Triggers) &ndash; Firebase iş dağıtıcı iş ne zaman çalışmalı bir saat belirtmek üzere uygulamaları sağlar.  
* [Başarısız olan işler için bir yeniden deneme stratejisini bildirme](#Setting_a_RetryStrategy) &ndash; A _stratejisi yeniden deneme_ için kılavuzluk sağlar `FirebaseJobDispatcher` ne ile yapmak `Jobs` tamamlamak başarısız. 

Her Bu konu, aşağıdaki bölümlerde daha incelenecektir.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>Bir iş parametreleri geçirme

Parametreler, işe oluşturarak geçirilir bir `Bundle` ile birlikte geçirilen `Job.Builder.SetExtras` yöntemi:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle` Alanından erişilen `IJobParameters.Extras` özellikte `OnStartJob` yöntemi:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Ayar kısıtlamaları

Kısıtlamaları yardımcı cihazda maliyetler veya pil boşaltma azaltır. `Firebase.JobDispatcher.Constraint` Sınıfı tamsayı değerleri olarak bu kısıtlamaların tanımlar:

* `Constraint.OnUnmeteredNetwork` &ndash; Yalnızca aygıt unmetered bir ağa bağlıyken işi çalıştırın. Bu, kullanıcının verileri ücretlerinin yansıtılmasını engellemek kullanışlıdır.
* `Constraint.OnAnyNetwork` &ndash; Cihazın bağlandığı ne olursa olsun ağdaki işi çalıştırın. İle birlikte belirtilmişse `Constraint.OnUnmeteredNetwork`, bu değer öncelikli olur.
* `Constraint.DeviceCharging` &ndash; Yalnızca aygıt şarj işi çalıştırın.

Kısıtlamalar ile ayarlanır `Job.Builder.SetConstraint` yöntemi: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

#### <a name="setting-job-triggers"></a>Ayar iş Tetikleyicileri

`JobTrigger` İş başlaması gereken hakkında işletim sistemi için kılavuzluk sağlar. A `JobTrigger` sahip bir _penceresi yürütme_ ne zaman zamanlanmış bir süredir tanımlayan `Job` çalıştırmanız gerekir. Yürütme penceresine sahip bir _penceresi başlangıç_ değeri ve bir _son penceresi_ değeri. Başlangıç penceresi cihaz işi çalıştırmadan önce beklemesi gereken saniye sayısını ve son penceresi değer çalıştırmadan önce beklenecek saniye sayısını `Job`. 

A `JobTrigger` ile oluşturulan `Firebase.Jobdispatcher.Trigger.ExecutionWindow` yöntemi.  Örneğin `Trigger.ExecutionWindow(15,60)` işin ne zaman zamanlanmış gelen 15 ila 60 saniye arasında çalışması gereken anlamına gelir. `Job.Builder.SetTrigger` İçin kullanılan yöntemi 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Varsayılan `JobTrigger` bir iş değeri tarafından temsil edilen için `Trigger.Now`, belirten bir işi mümkün olan en kısa sürede zamanlanma sonra çalıştırılması...

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Bir RetryStrategy ayarlama

`Firebase.JobDispatcher.RetryStrategy` Ne kadar bir gecikme cihazı denemeden önce başarısız bir işi yeniden çalıştırmak için kullanması gereken belirtmek için kullanılır. A `RetryStrategy` sahip bir _İlkesi_, tanımlayan hangi süresi temeli algoritması başarısız işi ve hangi iş geçerli zamanın bir pencere belirten bir yürütme penceresi yeniden zamanlamak için kullanılır. Bu _yeniden zamanlama penceresi_ iki değerlerine göre tanımlanır. İlk değer iş kullanılabilirliğiyle önce beklenecek saniye sayısıdır ( _ilk geri Çekilme_ değeri), ve ikinci sayı en fazla iş çalıştırmalısınız önce saniye sayısıdır ( _maksimum geri Çekilme_değeri). 
 
Yeniden deneme ilkelerini iki tür bu int değerlerine göre tanımlanır:

* `RetryStrategy.RetryPolicyExponential` &ndash; Bir _üstel geri alma_ İlkesi artıracaktır ilk geri Çekilme değeri katlanarak her hatasından sonra. Bir işi başarısız, ilk kez iş kullanılabilirliğiyle önce belirtilen _initial aralığı kitaplığı bekleyeceği &ndash; örnek 30 saniyedir. İş başarısız, ikinci kez kitaplık işi çalıştırmak denemeden önce en az 60 saniye bekler. Üçüncü denemesi başarısız olduktan sonra kitaplık 120 saniye bekleyin ve benzeri. Varsayılan `RetryStrategy` Firebase iş dağıtıcı için kitaplığı tarafından temsil edilen `RetryStrategy.DefaultExponential` nesnesi. 30 saniyelik bir ilk geri Çekilme ve en fazla bir geri Çekilme 3600 saniyelik vardır.
* `RetryStrategy.RetryPolicyLinear` &ndash; Bu strateji bir _doğrusal geri Çekilme_ iş çalıştırılacak şekilde zamanlanmasını ayarlamak aralıkları (başarılı olana dek). Doğrusal geri Çekilme mümkün olan en kısa sürede tamamlanmalıdır iş için veya hızlı bir şekilde kendilerini çözümleyecek sorunları için uygundur. Firebase iş dağıtıcı kitaplığı tanımlayan bir `RetryStrategy.DefaultLinear` en az 30 saniyelik bir yeniden zamanlama penceresi olan 3600 saniye için.

Özel bir tanımlamak mümkündür `RetryStrategy` ile `FirebaseJobDispatcher.NewRetryStrategy` yöntemi. Üç parametreleri alır:

1. `int policy` &ndash; _İlkesi_ önceki biri `RetryStrategy` değerleri, `RetryStrategy.RetryPolicyLinear`, veya `RetryStrategy.RetryPolicyExponential`.
2. `int intialBackoffSeconds` &ndash; _İlk geri Çekilme_ işi yeniden çalıştırmak denemeden önce gerekli olan bir gecikme, saniye cinsinden. Bunun için varsayılan değer 30 saniyedir. 
3. `int maximumBackoffSeconds` &ndash; _Maksimum geri Çekilme_ değeri saniye cinsinden gecikme işi yeniden çalıştırmak denemeden önce üst sınırını bildirir. 3600 saniye varsayılan değerdir. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Bir işi iptal ediliyor

Olası zamanlanmamış tüm işleri iptal etmek veya yalnızca tek işi kullanılarak `FirebaseJobDispatcher.CancelAll()` yöntemi veya `FirebaseJobDispatcher.Cancel(string)` yöntemi:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Her iki yöntem bir tamsayı değeri döndürür:

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; İş başarıyla iptal edildi.
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; Hata işi iptal ediliyor engelledi.
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher` Olduğundan geçerli işi iptal edemiyor `IDriver` kullanılabilir.

## <a name="summary"></a>Özet

Bu kılavuz Firebase iş dağıtıcı iş akıllıca arka planda gerçekleştirmek için nasıl kullanılacağı açıklanmıştır. Olarak gerçekleştirilecek iş yalıtılacak nasıl ele alınan bir `JobService` ve nasıl `FirebaseJobDispatcher` ölçütlerle belirtme çalışmanın zamanlamak için bir `JobTrigger` ve olan hataları nasıl işleneceğini bir `RetryStrategy`.


## <a name="related-links"></a>İlgili bağlantılar

- [NuGet üzerinde Xamarin.Firebase.JobDispatcher](https://www.nuget.org/packages/Xamarin.FirebaseJobDispatcher)
- [İş firebase dağıtıcısı github'da](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher Binding](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Akıllı iş planlama](https://developer.android.com/topic/performance/scheduling.html)
- [Android pil ve bellek iyileştirmeleri - Google g/ç 2016 (video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
