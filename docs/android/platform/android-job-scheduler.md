---
title: Android Job Scheduler
description: Bu kılavuz Android iş Zamanlayıcı API'yi kullanarak arka plan çalışması zamanlamak nasıl açıklanır.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: dc72b7e4da330185b00541f923d9c4b64b91bc95
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
ms.locfileid: "30005665"
---
# <a name="android-job-scheduler"></a>Android Job Scheduler

_Bu kılavuz Android iş Zamanlayıcı Android 5.0 (API düzeyi 21) çalıştıran Android cihazlarda kullanılabilir olan API kullanarak arka plan iş zamanlama açıklanır ve daha yüksek._


## <a name="overview"></a>Genel Bakış 

Bir Android uygulaması yanıt vereceğini kullanıcıya tutmak için iyi yollarından biri, karmaşık veya uzun süre çalışan iş arka planda gerçekleştirilir sağlamaktır. Bununla birlikte, arka plan çalışması olumsuz kullanıcı deneyimini aygıtla etkilemez olduğunu önemlidir. 

Örneğin, arka plan işi bir Web sitesi üç veya dört dakikada bir sorgu için belirli bir veri kümesine değişiklikleri yoklama. Pil ömrünün felaket niteliğinde bir etkisi yoktur ancak bu zararsız, gibi görünüyor. Uygulama art arda aygıtı Uyandırma, daha yüksek bir güç durumu CPU kullanımı ile yükseltme, Radyoları güç, ağ istekleri ve sonuçlar işlenirken yapın. Aygıt hemen gücü kapatın ve düşük güç boşta durumuna döndürmek için kötü alır. Kötü zamanlanmış arka plan çalışması durumunda gereksiz ve aşırı güç gereksinimleri ile cihaz yanlışlıkla tutabilir. (Bir Web sitesi yoklama) bu zararsız görünen etkinlik aygıt daha kısa bir süre içinde kullanılamaz hale getirecek.

Android işlemi arka planda gerçekleştirmek ile yardımcı olması için aşağıdaki API'ler sağlar ancak kendi kendilerine bunlar akıllı iş zamanlaması için yeterli değil. 

* **[Hedefi Hizmetleri](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; çalışması zamanlamak için bir yol sağlar ancak amacı Hizmetleri iş gerçekleştirmek için harika.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; bu API'ler yalnızca zamanlanacak ancak aslında çalışmayı gerçekleştirmek üzere bir yolunu sağlar çalışmaya izin verir. Ayrıca, AlarmManager yalnızca temel saat kısıtlamaları, belirli bir zamanda veya belirli bir süre geçtikten sonra bir alarm Yükselt anlamına gelir izin verir. 
* **[Yayın alıcıları](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; bir Android uygulaması yanıt veya sistem genelinde olayları hedefleri olarak işlerini yapmak için yayın alıcıları kurulumu. Ancak, yayın alıcıları işin ne zaman çalıştırılması gerektiğini üzerinden herhangi bir denetimi sağlamıyorsa. Ayrıca Android işletim sistemindeki değişiklikler kısıtlar yayın alıcıları çalışırken ya da yanıt verebilir iş tür. 

Arka plan iş verimli bir şekilde gerçekleştirmek için iki anahtar özellikler vardır (bazen denir bir _arka plan işi_ veya _iş_):

1. **Akıllıca iş zamanlama** &ndash; uygulamanın iş arka planda, gerçekleştirdiğinde bunu iyi vatandaşı mu olduğunu önemlidir. İdeal olarak, uygulama bir işin çalıştırılması talep değil. Bunun yerine, uygulama iş çalıştırmak ve koşullar karşılandığında iş gerçekleştirecek işletim sistemi bu işle zamanlamak için karşılanması gereken koşulları belirtmeniz gerekir. Bu aygıttaki en yüksek verimliliği sağlamak için işi çalıştırmak Android sağlar. Örneğin, ağ istekleri tüm ek yükü en fazla kullanılmasını ağ ile ilgili sağlamak için aynı anda çalıştırmak için toplu.
2. **İş Kapsüllenen** &ndash; arka plan çalışması gerçekleştirmek için kod bağımsız olarak kullanıcı arabirimi çalıştırmak ve tamamlamak iş başarısız olursa yeniden zamanlamak görece olarak daha kolay bir ayrık bileşeninde kapsüllenmiş Bazı nedenlerden dolayı.

Android İş Zamanlayıcısı zamanlama arka plan iş basitleştirmek için akıcı bir API sağlar Android işletim sistemi için yerleşik bir çerçevedir.  Android İş Zamanlayıcısı aşağıdaki türden oluşur:

* `Android.App.Job.JobScheduler` Zamanlama, yürütme ve gerekirse iptal etmek, bir Android uygulama adına işleri için kullanılan bir sistemdir.
* Bir `Android.App.Job.JobService` iş uygulamanın ana iş parçacığı üzerinde çalışacak mantığı ile genişletilmelidir bir Özet sınıf. Bunun anlamı `JobService` zaman uyumsuz olarak gerçekleştirilecek nasıl iş içindir sorumludur.
* Bir `Android.App.Job.JobInfo` nesnesi işin çalışması gereken Android yardımcı olacak ölçütleri tutar.

Android İş Zamanlayıcısı ile çalışması zamanlamak için bir Xamarin.Android uygulaması genişleten bir sınıfı kodda sarmalamalıdır `JobService` sınıfı. `JobService` üç yaşam döngüsü yöntemlerine sahiptir, iş ömrü boyunca çağrılabilir:

* **bool OnStartJob (JobParameters parametreleri)** &ndash; bu yöntem tarafından çağrılır `JobScheduler` iş ve uygulamanın ana iş parçacığı çalışır gerçekleştirmek için. Sorumluluğundadır `JobService` zaman uyumsuz olarak çalışmayı gerçekleştirmek üzere ve `true` varsa kalan, iş veya `false` iş yapıldığında.
    
    Zaman `JobScheduler` bu yöntemi çağırır isteği ve iş sürekliliği için Android gelen wakelock korur. İş tamamlandığında, sorumluluğundadır `JobService` bildirmek için `JobScheduler` çağrı tarafından bu olayının `JobFinished` yöntemi (sonraki bölümde açıklandığı).

* **JobFinished (JobParameters parametreleri, bool needsReschedule)** &ndash; bu yöntem çağrılır `JobService` bildirmek için `JobScheduler` , iş yapılır. Varsa `JobFinished` çağrılmaz, `JobScheduler` gereksiz pil boşaltma neden wakelock kaldırmaz. 

* **bool OnStopJob (JobParameters parametreleri)** &ndash; iş erken Android tarafından durdurulduğunda bu adı verilir. Döndürme zorunluluğu `true` işi (aşağıda ayrıntılı olarak açıklanmıştır) yeniden deneme ölçütlere göre yeniden durumunda.

Belirlemek mümkündür _kısıtlamaları_ veya _Tetikleyicileri_ ne zaman bir iş olabilir veya çalışması gerektiğini denetlemek. Örneğin, böylece yalnızca cihaz şarj veya resim olduğunda bir işi başlatmak için geçen zaman da çalışacak bir işi sınırlamak mümkündür.

Bu kılavuz nasıl uygulanacağını ayrıntılı olarak ele alınacaktır bir `JobService` sınıfı ve onunla zamanlama `JobScheduler`.

## <a name="requirements"></a>Gereksinimler

Android API düzeyi (Android 5.0) 21 Android İş Zamanlayıcısı gerektiren ya da daha yüksek. 

## <a name="using-the-android-job-scheduler"></a>Android iş Zamanlayıcı'yı kullanma

Android JobScheduler API kullanma için üç adım vardır:

1. İş kapsülleyen bir JobService türü uygulayın.
2. Kullanım bir `JobInfo.Builder` oluşturulacak nesne `JobInfo` ölçütlerini tutacak nesne `JobScheduler` işi çalıştırmak için. 
3. Kullanarak iş zamanlama `JobScheduler.Schedule`.

### <a name="implement-a-jobservice"></a>Bir JobService uygulama

Android iş Zamanlayıcı Kitaplığı tarafından gerçekleştirilen tüm iş genişleten bir türü yapılmalıdır `Android.App.Job.JobService` soyut sınıf. Oluşturma bir `JobService` oluşturmaya çok benzer bir `Service` Android framework ile: 

1. Genişletme `JobService` sınıfı.
2. Alt sınıfla tasarlamanız `ServiceAttribute` ve `Name` paket adı ve sınıf adını oluşan bir dize parametresi (aşağıdaki örneğe bakın).
3. Ayarlama `Permission` özelliği `ServiceAttribute` dizeye `android.permission.BIND_JOB_SERVICE`.
4. Geçersiz kılma `OnStartJob` yöntemi, çalışmayı gerçekleştirmek için kod ekleme. Android işi çalıştırmak için uygulamanın ana iş parçacığı üzerinde bu yöntemi çağırır. Birkaç milisaniye uygulama engelleme önlemek için bir iş parçacığında gerçekleştirilmelidir daha uzun sürer çalışın.
5. İş tamamlandığında, `JobService` çağırmalısınız `JobFinished` yöntemi. Bu yöntem nasıl `JobService` söyler `JobScheduler` çalışma yapılır. Çağrılacak hatası `JobFinished` sonuçlanır `JobService` gereksiz taleplerini pil ömrünün kısaltmayı cihazda koyma. 
6. Ayrıca geçersiz kılmak için iyi bir fikirdir `OnStopJob` yöntemi. Bu yöntem, tamamlandı ve sağlar önce işi kapatılıyor, Android tarafından çağrılır `JobService` düzgün tüm kaynaklarını silmek için bir fırsat ile. Bu yöntem döndürmelidir `true` işi yeniden zamanlamak gerekli değilse veya `false` işi yeniden çalıştırmak için desireable değilse.
   

Aşağıdaki kod basit örneğidir `JobService` bazı iş zaman uyumsuz olarak gerçekleştirmek için bir uygulama için TPL kullanma:

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Bir işi zamanlamak için bir iş tanımı oluşturma

Xamarin.Android uygulamaları değil örneği bir `JobService` doğrudan, bunun yerine geçirirler bir `JobInfo` nesnesine `JobScheduler`. `JobScheduler` İstenen örneği `JobService` zamanlama ve çalıştırma nesne `JobService` meta verilerde göre `JobInfo`. A `JobInfo` nesnesi, aşağıdaki bilgileri içermelidir:

* **JobId** &ndash; bu bir `int` işe tanımlamak için kullanılan değer `JobScheduler`. Bu değer yeniden var olan tüm işleri güncelleştirir. Değeri uygulama için benzersiz olmalıdır. 
* **JobService** &ndash; Bu parametre bir `ComponentName` türü açıkça tanımlayan, `JobScheduler` bir işi çalıştırmak için kullanmanız gerekir. 
  

Bu uzantı metodu nasıl oluşturulduğunu gösteren bir `JobInfo.Builder` bir Android ile `Context`, bir etkinlik gibi:

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob andsets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creats a JobInfo object.
```

Güçlü Android İş Zamanlayıcısı ne zaman bir iş çalıştırır veya ne altında bir işi koşulları çalışabilir denetleme olanağı özelliğidir. Aşağıdaki tabloda bazı yöntemlere açıklar `JobInfo.Builder` bir işin ne zaman etkilemek uygulama izin ver:  


|  Yöntem | Açıklama   |
|---|---|
| `SetMinimumLatency`  | Önce bir iş uyulması bir gecikme (milisaniye cinsinden) çalışması gerektiğini belirtir. |
| `SetOverridingDeadline`  | İş bildiren bu süresi (milisaniye cinsinden) geçmeden önce çalıştırmanız gerekir. |
| `SetRequiredNetworkType`  | Bir iş için ağ gereksinimleri belirtir. |
| `SetRequiresBatteryNotLow` | Aygıt kullanıcıya "Düşük pil" uyarısı görüntülerken değil, iş yalnızca çalıştırabilirsiniz. |
| `SetRequiresCharging` | Pil şarj olduğunda iş yalnızca çalıştırabilirsiniz. |
| `SetDeviceIdle` | Cihaz meşgul olduğunda iş çalıştırılır. |
| `SetPeriodic` | İş düzenli olarak çalışması gerektiğini belirtir. |
| `SetPersisted` | İş perisist aygıt yeniden başlatmalar arasında olmalıdır. | 


`SetBackoffCriteria` Hakkında bazı yönergeler sağlar uzun `JobScheduler` bir işi yeniden çalıştırmak denemeden önce beklemesi gerekir. Geri Çekilme ölçütleri iki bölümü vardır: kullanılması gereken bir gecikme milisaniye (varsayılan değeri olan 30 saniye) ve geri alma türü (bazen denir _geri Çekilme İlkesi_ veya _yeniden deneme ilkesi_) . İçinde iki ilke kapsüllenmiş `Android.App.Job.BackoffPolicy` enum:

* `BackoffPolicy.Exponential` &ndash; Üstel geri alma İlkesi ilk geri Çekilme değeri katlanarak her hatasından sonra artacaktır. Bir işi başarısız, ilk kez kitaplık işi – örnek 30 saniye kullanılabilirliğiyle önce belirtilen ilk aralığı bekler. İş başarısız, ikinci kez kitaplık işi çalıştırmak denemeden önce en az 60 saniye bekler. Üçüncü denemesi başarısız olduktan sonra kitaplık 120 saniye bekleyin ve benzeri. Varsayılan değer budur.
* `BackoffPolicy.Linear` &ndash; İş için zamanlanmasını doğrusal bir geri Çekilme bu stratejidir belirlenen aralıklarla (başarılı olana dek) çalıştırın. Doğrusal geri Çekilme mümkün olan en kısa sürede tamamlanmalıdır iş için veya hızlı bir şekilde kendilerini çözümleyecek sorunları için uygundur. 

Daha fazla ayrıntı oluşturmak için bir `JobInfo` nesne okuyun, [Google belgelerine `JobInfo.Builder` sınıfı](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>İş tanımı aracılığıyla bir iş parametreleri geçirme

Parametreler, işe oluşturarak geçirilir bir `PersistableBundle` ile birlikte geçirilen `Job.Builder.SetExtras` yöntemi:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle` Alanından erişilen `Android.App.Job.JobParameters.Extras` özelliğinde `OnStartJob` yöntemi bir `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>Bir iş zamanlaması

Bir işi zamanlamak için bir Xamarin.Android uygulaması için bir başvuru alır `JobScheduler` sistem hizmeti ve çağrı `JobScheduler.Schedule` yöntemiyle `JobInfo` önceki adımda oluşturulan nesne. `JobScheduler.Schedule` iki tamsayı değerlerden biriyle hemen döndürür:

* **JobScheduler.ResultSuccess** &ndash; iş zamanlanmış başarıyla yüklendi. 
* **JobScheduler.ResultFailure** &ndash; iş zamanlanamadı. Bu durum genellikle çakışan tarafından kaynaklanır `JobInfo` parametreleri.

Bu kod, bir iş zamanlaması ve zamanlama girişimi sonuçları kullanıcı bildiren bir örnektir:

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
}
else
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
}
```
 
### <a name="cancelling-a-job"></a>Bir işi iptal ediliyor

Olası zamanlanmamış tüm işleri iptal etmek veya yalnızca tek işi kullanılarak `JobsScheduler.CancelAll()` yöntemi veya `JobScheduler.Cancel(jobId)` yöntemi:

```csharp
// Cancel all jobs
jobSchduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Özet

Bu kılavuz, Android İş Zamanlayıcısı akıllıca iş arka planda gerçekleştirmek için nasıl kullanılacağı açıklanmıştır. Olarak gerçekleştirilecek iş yalıtılacak nasıl ele alınan bir `JobService` ve nasıl kullanılacağını `JobScheduler` ölçütlerle belirtme çalışmanın zamanlamak için bir `JobTrigger` ve olan hataları nasıl işleneceğini bir `RetryStrategy`.

## <a name="related-links"></a>İlgili bağlantılar

- [Akıllı iş planlama](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API reference](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Pro JobScheduler ile gibi işlerini zamanlama](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android pil ve bellek iyileştirmeleri - Google g/ç 2016 (video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert - Xamarin University](https://www.youtube.com/watch?v=aSjBBPYjelE)