---
title: "iOS görevlerle Backgrounding"
ms.topic: article
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 5e05cf0f13512478b3957070e7fa6329ea84337f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="ios-backgrounding-with-tasks"></a>iOS görevlerle Backgrounding

İos'ta backgrounding gerçekleştirmek için basit backgrounding gereksinimlerinizi görevlere bölme ve arka planda çalışan görevler için yoldur. Görevler altında katı bir zaman sınırı olan ve genellikle yaklaşık 600 saniye (10 dakika) bir uygulamanın iOS 6 arka plana taşındıktan sonra işleme süresini ve 10 dakikadan az iOS 7 + alabilirsiniz.

Arka plan görevleri üç kategoride ayrılabilir:

1.  **Arka plan güvenli görevleri** - herhangi bir yerinde çağrılan sahip olduğu bir görev uygulama uygulama girin arka kesintiye uğramış istemediğiniz.
1.  **DidEnterBackground görevleri** - sırasında çağrılan `DidEnterBackground` temizleme ve durum kaydetme yardımcı olmak üzere uygulama yaşam döngüsü yöntemi.
1.  **Arka plan aktarımları (iOS 7 +)** -özel türde bir arka plan görevi iOS 7 ağ aktarımı gerçekleştirmek için kullanılır. Normal görevleri, arka plan aktarımları önceden belirlenen bir süre sınırı yoktur.


Arka plan için güvenli ve `DidEnterBackground` görevlerdir iOS 6 ve iOS 7, bazı küçük farklar ile kullanmak güvenli. Şimdi bu iki tür görev daha ayrıntılı araştırın.

## <a name="creating-background-safe-tasks"></a>Arka plan güvenli görevler oluşturma

Bazı uygulamalar, uygulamanın durumu değiştirmelisiniz iOS tarafından kesintiye döndürmemelidir görevleri içerir. Bu görevleri kesintiye gelen korumak için bir iOS uzun süre çalışan görevler olarak kaydolmak için yoludur. Bu desen uygulamanızda nerede kesintiye bir görev kullanıcı put uygulama arka plan içine gereken istemediğiniz her yerde kullanabilirsiniz. Bu model için harika bir aday sunucunuza yeni kullanıcının kayıt bilgileri gönderme veya oturum açma bilgilerini doğrulama gibi görevleri olacaktır.

Aşağıdaki kod parçacığını arka planda çalıştırmak için bir görev kaydetme gösterir:

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Bir görev benzersiz bir tanımlayıcı ile kayıt işlemini çiftleri `taskID`ve eşleştirmelerinde sarmalar `BeginBackgroundTask` ve `EndBackgroundTask` çağrıları. Çağrı vermiyoruz tanımlayıcısını oluşturmak için `BeginBackgroundTask` yöntemi `UIApplication` nesne ve uzun süre çalışan görev üzerinde yeni bir iş parçacığı genellikle başlatın. Görev tamamlandığında diyoruz `EndBackgroundTask` ve aynı tanımlayıcıda geçirin. İOS, uygulama sonlandıracak bu önemlidir, çünkü bir `BeginBackgroundTask` çağrısı eşleşen yok `EndBackgroundTask`.

> [!IMPORTANT]
> **Not**: ana iş parçacığı ya da uygulamanın gereksinimlerine bağlı olarak bir arka plan iş parçacığı güvenli arka plan görevleri çalıştırabilir.


## <a name="performing-tasks-during-didenterbackground"></a>DidEnterBackground sırasında görevleri gerçekleştirme

Uzun süre çalışan bir görev arka plan güvenli hale getirme yanı sıra kayıt uygulama arka planda put gibi görevleri kazandırın için kullanılabilir. iOS sağlayan bir olay yönteminde *AppDelegate* adlı bir sınıf `DidEnterBackground` uygulama durumunu Kaydet, kullanıcı verilerini kaydetme ve uygulama arka geçirilmeden önce duyarlı içeriği şifrelemek için kullanılabilir. Bu yöntemden almak için yaklaşık beş saniyede bir uygulama yok veya sonlandırıldı. Bu nedenle, tamamlamak için birden fazla beş saniye sürebilir temizleme görevleri gelen çağrılabilir içinde `DidEnterBackground` yöntemi. Bu görevleri ayrı bir iş parçacığı üzerinde çağrılması gerekir.

Neredeyse aynı, uzun süre çalışan bir görev kaydı işlemidir. Aşağıdaki kod parçacığını bu eylemde gösterilmektedir:

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Geçersiz kılarak başlamak `DidEnterBackground` yönteminde `AppDelegate`, biz bizim göreviyle kaydetmek burada `BeginBackgroundTask` önceki örnekte yaptığımız gibi. Ardından, size yeni bir iş parçacığı oluşturma ve bizim uzun süre çalışan görev gerçekleştirin. Unutmayın `EndBackgroundTask` çağrısı şimdi yapıldığında gelen uzun süre çalışan görev içinde beri `DidEnterBackground` yöntemi zaten döndürdü.

> [!IMPORTANT]
> **Not**: iOS kullanan bir [izleme mekanizması](http://developer.apple.com/library/ios/qa/qa1693/_index.html) uygulamanın UI yanıt verebilir durumda kalmasını sağlamak için. Çok fazla zaman harcayan bir uygulama `DidEnterBackground` kullanıcı Arabiriminde vermemeye. Arka planda çalıştırılacak görevleri kapalı oluşturan verir `DidEnterBackground` zamanında kullanıcı Arabiriminin yanıt verebilir durumda kalmasını ve uygulama sonlandırılması izleme engelleyerek, döndürülecek.


## <a name="handling-background-task-time-limits"></a>İşleme arka plan görevi zaman sınırları

iOS yerleştirir katı sınırları üzerinde uzun bir arka plan görevi çalıştırabilirsiniz nasıl ve `EndBackgroundTask` ayrılan süre içinde çağrı yapılır değil, uygulama sonlandırılacak. Kalan backgrounding süre ve sona erme işleyicileri gerektiğinde kullanarak izleyen tarafından iOS uygulamanın sonlandırma önleyebilirsiniz.

### <a name="accessing-background-time-remaining"></a>Kalan arka plan zaman erişme

Kayıtlı görevler içeren bir uygulama için arka plan taşınsa, kayıtlı görevleri çalıştırmak yaklaşık 600 saniye alırsınız. Görev sahip statik kullanarak tamamlamak için ne kadar süre kontrol edebilirsiniz `BackgroundTimeRemaining` özelliği `UIApplication` sınıfı. Aşağıdaki kod bize bizim arka plan görevi ayrıldı saniye cinsinden zaman verin:

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Sona erme işleyicileri ile uygulama sonlandırma önleme

Erişimi vermiş yanı sıra `BackgroundTimeRemaining` özelliği, iOS üzerinden arka plan zaman aşımı işlemek için normal bir yöntem sunar bir **sona erme işleyici**. Bir isteğe bağlı bir görev için ayrılan süresi dolmak üzere olduğunda, yürütülen kod bloğunu budur. Sona erme işleyici kodu çağırır `EndBackgroundTask` ve uygulama iyi davrandığından ve iOS görev çalıştığında dışında olsa bile uygulama sonlandırma engeller gösterir görev kimliği geçirir. `EndBackgroundTask` sona erme işleyici içinde yanı sıra yürütme normal seyrinde çağrılmalıdır. 

Sona erme işleyici, aşağıda gösterildiği gibi bir lambda ifadesi kullanarak anonim bir işlevi ifade edilir:

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.TimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

Sona erme işleyicileri kodu çalıştırmak gerekli değildir; ancak, bir arka plan görevine sahip bir sona erme işleyicisi her zaman kullanmalısınız.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>İOS 7 + arka plan görevleri

İOS 7 arka plan görevleri ile en büyük değişiklik değil görevlerin nasıl uygulanır, ancak çalıştırdıklarında ' dir.

Ön iOS 7, arka planda çalışan bir görev 600 saniye tamamlamak için olduğunu hatırlayın. Bu sınır için bir neden, arka planda çalışan bir görev cihaz uyanık görev süresince tutmanız şöyledir:

 [![](ios-backgrounding-with-tasks-images/ios6.png "Uygulama uyanık öncesi iOS 7 tutma görev grafiği")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

iOS 7 arka plan işlemesi uzun pil ömrü için optimize edilmiştir. İOS 7'de, backgrounding fırsatçılıktan olur: cihaz uyku ve cihaz telefon görüşmeleri, bildirimler, gelen e-posta ve diğer işlemek için çağrıldığında bunun yerine bunların işlenmesini parçalar gittiğinde cihaz uyanık tutma yerine görevleri Uy Ortak kesintiler. Aşağıdaki diyagramda bir görevin nasıl kopmuş olabilir içine Insight sağlar Yukarı:

 [![](ios-backgrounding-with-tasks-images/ios7.png "Grafik bozuk görevinin sonrası iOS 7 öbekleri")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Çalışma zamanı görevi artık sürekli olmadığından, ağ aktarımı gerçekleştirmek görevleri iOS 7 farklı şekilde ele gerekir. Geliştiriciler kullanmaları `NSURlSession` ağ aktarımları işlemek için API. Sonraki bölümde arka plan aktarımları bir genel bakıştır.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Arka plan aktarımları

İOS 7 arka plan aktarımları omurga yenilikler `NSURLSession` API. `NSURLSession` görevler oluşturma olanak tanır:

1.  Ağ ve aygıt kesintiler içeriğinden aktarın.
1.  Büyük dosyaları yükleme ve indirme ( *arka plan Aktarım Hizmeti* ).


Bunun nasıl çalıştığına daha yakın bir göz atalım.

### <a name="nsurlsession-api"></a>NSURLSession API

 `NSURLSession` bir güçlü içeriği ağ üzerinden aktarılması için bir API'dir. Ağ kesintilerine ve uygulama durumları değişiklikleri üzerinden veri aktarımı işlemek için araçlar sağlar.

`NSURLSession` API sırayla ilgili veri öbekleri ağ üzerinden shuttle için görevler oluşturma, bir veya birkaç oturum oluşturur. Görevler, hızlı ve güvenilir bir şekilde veri aktarmak için zaman uyumsuz olarak çalışır. Çünkü `NSURLSession` olduğu zaman uyumsuz, her oturum sistem ve uygulama bir Aktarım tamamlandığında bilmeniz izin vermek için tamamlama işleyici blok gerektirir.

Ön iOS 7 ve sonrası iOS 7 üzerinde geçerli bir ağ aktarımı gerçekleştirmek için kontrol bir `NSURLSession` kuyruğa aktarımları için kullanılabilir ve düzenli arka plan görevi değilse aktarımı gerçekleştirmek için kullanın:

```csharp
if ([NSURLSession class]) {
  // Create a background session and enqueue transfers
}
else {
  // Start a background task and transfer directly
  // Do NOT make calls to update the UI here!
}
```

> [!IMPORTANT]
> **Not**: iOS 6 arka plan UI güncelleştirmeleri desteklemez ve uygulamayı sonlandıracak gibi iOS 6 uyumlu kodda arka plan Arabiriminden güncelleştirmek için çağrıları yapma kaçının.


`NSURLSession` API zengin bir kimlik doğrulamasını işleyecek, başarısız aktarımlarını yönetmek ve - ancak değil sunucu-tarafı - istemci tarafı hata raporu özellikleri kümesi içerir. İOS 7 görev Kesintiler çalışma zamanı sunulan köprüsü yardımcı olur ve hızlı ve güvenilir bir şekilde büyük dosyaları aktarmak için de destek sağlar. Sonraki bölümde bu ikinci özellik araştırır.

### <a name="background-transfer-service"></a>Arka plan Aktarım Hizmeti

İOS 7 önce yükleyerek veya arka planda dosya indirme güvenilmez. Arka plan görevleri çalıştırmak için sınırlı bir süre alabilir ancak bir dosya aktarım süresini ağ ve dosya boyutuna göre değişir. İOS 7'de, kullandığımız bir `NSURLSession` başarıyla karşıya yükleyin ve büyük dosyaları indirin. Belirli `NSURLSession` büyük dosyaları arka planda ağ aktarımı işleme oturumu türü olarak bilinir *arka plan Aktarım Hizmeti*.

Arka plan Aktarım Hizmeti kullanılarak başlatılan aktarımları işletim sistemi tarafından yönetilir ve kimlik doğrulama ve hataları işlemek için API'ler sağlar. Aktarımları göre rastgele bir zaman sınırı bağımlı olmadığından kullanılabilmesi için karşıya yükleme veya büyük dosyaları karşıdan yüklemek için otomatik güncelleştirme ve arka plan içeriği. Başvurmak [arka plan aktarım izlenecek](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) hizmeti uygulama hakkında ayrıntılar için.

Arka plan Aktarım Hizmeti, genellikle arka plan Fetch veya içeriği arka planda yenileme uygulamaları yardımcı olmak için Uzak bildirimleri ile eşleştirilmiş. Sonraki iki bölümde iOS 6 ve iOS 7 arka planda çalışan tüm uygulamaların kaydetme kavramını tanıtır.

