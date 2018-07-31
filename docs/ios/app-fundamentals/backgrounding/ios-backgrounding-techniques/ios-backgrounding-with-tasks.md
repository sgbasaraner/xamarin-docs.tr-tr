---
title: iOS ile görevleri arka planda işleme
description: Bu belgenin arka plan görevlerini uygulamanın arka planda yerleştirildikten sonra uzun süre çalışan görevleri gerçekleştirmek için nasıl kullanılacağını açıklar.
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 9d304ee64e7716413febc475e721f5eb39043109
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351544"
---
# <a name="ios-backgrounding-with-tasks"></a>iOS ile görevleri arka planda işleme

İos'ta arka planda işleme gerçekleştirmek için en basit yolu, backgrounding gereksinimlerinizi görevlere ayırma ve görevleri arka planda çalışmaya sağlamaktır. Görevleri, katı bir zaman sınırı olan ve tipik olarak yaklaşık 600 saniye (10 dakika) bir uygulama, iOS 6 arka plana taşındıktan sonra işleme süresi ve 10 dakikadan az iOS 7 + alın.

Arka plan görevleri, üç kategoriye bölünmüştür:

1.  **Güvenli arka plan görevleri** - dünyanın her yerinde çağrılan bir görev olduğu uygulama uygulama girmeniz gereken arka plan kesintiye istemediğiniz.
1.  **DidEnterBackground görevleri** - sırasında çağrılan `DidEnterBackground` temizleme ve durum kaydetme yardımcı olmak için uygulama yaşam döngüsü yöntemi.
1.  **Arka plan aktarımları (iOS 7 +)** -iOS 7 ağ aktarımı gerçekleştirmek için özel bir arka plan görev türü kullanılır. Normal görevlerden farklı olarak, arka plan aktarımları önceden belirlenen bir süre sınırı yoktur.


Arka plan için güvenli ve `DidEnterBackground` görevleridir hem iOS 6 ve iOS 7, bazı küçük farklar ile kullanmak üzere güvenli. Şimdi bu iki tür görevleri daha ayrıntılı inceleyin.

## <a name="creating-background-safe-tasks"></a>Güvenli arka plan görevleri oluşturma

Bazı uygulamalar, uygulama durumu değiştiğinde iOS tarafından kesintiye olmamalıdır görevleri içerir. Bu görevler kesintiye gelen korumak için bir iOS uzun soluklu görevlerin olarak kaydetmek için yoludur. Bu düzen uygulamanızda nerede kesintiye bir görev kullanıcı put uygulamanın arka planda gereken istemediğiniz her yerde kullanabilirsiniz. Bu düzen için harika bir aday sunucunuza yeni bir kullanıcının kayıt bilgilerini göndermeye ve oturum açma bilgilerinin doğrulanması gibi görevleri olacaktır.

Aşağıdaki kod parçacığı, arka planda çalıştırılacak bir görev kaydetme gösterir:

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Kayıt işlemine benzersiz bir tanımlayıcıya sahip bir görevi çiftlerini `taskID`ve ardından eşleşen sarmalar `BeginBackgroundTask` ve `EndBackgroundTask` çağrıları. Tanımlayıcısını oluşturmak için size bir çağrı yapmak `BeginBackgroundTask` metodunda `UIApplication` nesne ve uzun süre çalışan görev genellikle yeni bir iş parçacığı başlatın. Görev tamamlandığında diyoruz `EndBackgroundTask` ve aynı tanımlayıcıda geçirin. Bu önemlidir, iOS, uygulama sonlandırılır çünkü bir `BeginBackgroundTask` çağrı eşleşen bir yok `EndBackgroundTask`.

> [!IMPORTANT]
> Güvenli arka plan görevleri, ana iş parçacığı veya bir arka plan iş parçacığı, uygulama gereksinimlerine bağlı olarak çalıştırabilirsiniz.


## <a name="performing-tasks-during-didenterbackground"></a>Sırasında DidEnterBackground görevlerini gerçekleştirme

Uzun süre çalışan görevleri arka plan güvenli hale getirmenin yanı sıra uygulamanın arka planda put gibi görevleri başlatmasını kaydı kullanılabilir. iOS sağlayan bir olay yönteminde *AppDelegate* adlı sınıf `DidEnterBackground` uygulama durumunu Kaydet, kullanıcı verilerini kaydedin ve uygulamanın arka planda girmeden önce hassas içerik şifrelemek için kullanılabilir. Bu yöntemden yaklaşık olarak beş saniyede bir uygulamaya sahip veya sonlandırıldı. Bu nedenle, tamamlamak için birden fazla beş saniye sürebilir temizleme görevleri gelen çağrılabilir içinde `DidEnterBackground` yöntemi. Bu görevlerin ayrı bir iş parçacığında çağrılmalıdır.

Neredeyse aynı, uzun süre çalışan bir görev kaydetme işlemidir. Aşağıdaki kod parçacığı, bunu eylem göstermektedir:

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Geçersiz kılarak başlamak `DidEnterBackground` yönteminde `AppDelegate`, biz bizim göreviyle kaydetmeden burada `BeginBackgroundTask` önceki örnekte yaptığımız gibi. Ardından, size yeni bir iş parçacığı oluşturma ve müşterilerimizin uzun süre çalışan görev gerçekleştirin. Unutmayın `EndBackgroundTask` çağrı artık yapılır gelen uzun süre çalışan görev içinde beri `DidEnterBackground` yöntemi zaten döndürdü.

> [!IMPORTANT]
> iOS kullanan bir [İzleyici mekanizması](http://developer.apple.com/library/ios/qa/qa1693/_index.html) bir uygulamanın UI duyarlı kaldığından emin olmak için. Uygulamanın içinde çok fazla zaman harcadığını `DidEnterBackground` Arabiriminde yanıt vermemeye başlıyor. Arka planda çalıştırılacak görevleri devre dışı başlatılmadan sağlayan `DidEnterBackground` UI esnek tutma ve uygulama sonlandırma gelen izleme engelleyerek zamanında, döndürülecek.


## <a name="handling-background-task-time-limits"></a>İşleme arka plan görev süre sınırı

iOS yerleştirir katı sınırları üzerinde uzun bir arka plan görevini çalıştırabilirsiniz nasıl ve `EndBackgroundTask` ayrılan süre içinde çağrı yapılır değil, uygulama sonlandırılacak. Kalan süre arka planda işleme ve sona erme işleyicileri gerektiğinde kullanarak izleyen tarafından uygulama sonlandırılıyor iOS önleyebilirsiniz.

### <a name="accessing-background-time-remaining"></a>Kalan arka plan zaman erişme

Kayıtlı görevleri ile bir uygulama arka plana taşıdıysanız, kayıtlı görevleri çalıştırmak yaklaşık 600 saniye alırsınız. Biz görev içeriyor'using static tamamlanması ne kadar süre denetleyebilirsiniz `BackgroundTimeRemaining` özelliği `UIApplication` sınıfı. Aşağıdaki kod bize bizim arka plan görevi kalan saniye cinsinden süre verecektir:

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Sona erme işleyicilerle kaçınarak uygulamayı sonlandırma

Erişim verme yanı sıra `BackgroundTimeRemaining` özelliği, iOS, arka plan zaman aşımı ile işlemek için normal bir yol sağlar bir **sona erme işleyici**. Bu görev için ayrılan süre dolmak üzere olduğunda yürütülen kodu isteğe bağlı bir bloğudur. Sona erme işleyicisinde kodu çağıran `EndBackgroundTask` ve uygulamayı da davrandığını ve iOS görev çalıştığında dışında olsa bile uygulama sonlandırma engeller gösterir görev kimliği, geçirir. `EndBackgroundTask` sona erme işleyici içinde yanı sıra normal seyrinde yürütme çağrılmalıdır. 

Sona erme işleyici, aşağıda gösterildiği gibi bir lambda ifadesi kullanarak anonim bir işlev ifade edilir:

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

Sona erme işleyicileri kodu çalıştırmak gerekli değildir; ancak, bir arka plan görevine sahip her zaman bir sona erme işleyicisi kullanmanız gerekir.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>İOS 7 + arka plan görevleri

İOS 7 arka plan görevleri ile ilgili en büyük değişiklik görevleri nasıl uygulanmaz, ancak çalıştırdıklarında değildir.

Öncesi iOS 7, arka planda çalışan bir görev 600 saniye tamamlanması olduğunu hatırlayın. Arka planda çalışan bir görev cihaz uyanık görevin süresi boyunca engelleneceği, bu sınır için bir neden verilmiştir:

 [![](ios-backgrounding-with-tasks-images/ios6.png "Uygulama uyanık öncesi iOS 7 tutma görev grafiği")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

iOS 7 arka plan işlemesi uzun pil ömrü için optimize edilmiştir. İOS 7'de, arka planda işleme fırsatçı olur: cihaz uyku ve cihazın telefon görüşmeleri, bildirimler, gelen e-posta ve diğer işlemek için çağrıldığında bunun yerine bunların işlenmesini öbekler halinde gittiğinde cihaz uyanık tutmak yerine görevleri Uy Ortak kesintiler. Aşağıdaki diyagramda bir görevin nasıl kopabilir Öngörüler sağlar. ayarlama:

 [![](ios-backgrounding-with-tasks-images/ios7.png "Graf bozuk görevin chunks sonrası iOS 7")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Çalışma zamanı görevi, artık sürekli olmadığı için iOS 7 ' ağ aktarımı gerçekleştirmek görevleri farklı işlenmesi gerekir. Geliştiriciler kullanmaları `NSURlSession` ağ aktarımları işlemek için API. Sonraki bölümde, arka plan aktarımları bir genel bakıştır.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Arka plan aktarımları

Temel arka plan aktarımları iOS 7'deki yenilikler `NSURLSession` API. `NSURLSession` görevler oluşturmak sağlıyor:

1.  Ağ ve cihaz kesintiler aracılığıyla içerik aktarın.
1.  Büyük dosyaları yükleme ve indirme ( *arka plan Aktarım Hizmeti* ).


Bunun nasıl çalıştığını daha yakından göz atalım.

### <a name="nsurlsession-api"></a>Nsurlsession'ı API

 `NSURLSession` güçlü bir API ağ üzerinden içerik aktarmak için ' dir. Bu, ağ kesintileri ve uygulama durumlarını değişiklikleri üzerinden veri aktarımı işlemek için araçları kümesi sağlar.

`NSURLSession` API sırayla, ilgili veri blokları ağında shuttle için görevler oluşturabilir, bir veya birden fazla oturum oluşturur. Görevler zaman uyumsuz olarak hızlı ve güvenilir bir şekilde veri aktarmak için çalıştırın. Çünkü `NSURLSession` olduğu zaman uyumsuz, her oturum sistem ve uygulama bir Aktarım tamamlandığında bilmeniz izin vermek için bir tamamlama işleyicisi blok gerektirir.

Öncesi iOS 7 ve sonrası iOS 7 geçerli bir ağ aktarımı gerçekleştirmek için denetleme bir `NSURLSession` kuyruğa aktarımları için kullanılabilir ve düzenli arka plan görevi değilse aktarımı gerçekleştirmek için kullanın:

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
> İOS 6, arka plan UI güncelleştirmeleri desteklemez ve uygulamayı sona erdirir arka planda iOS 6 uyumlu kod, kullanıcı Arabiriminden güncelleştirmek için çağrıları yapmaktan kaçının.


`NSURLSession` API'yi içeren zengin bir özellikler kimlik doğrulamasını gerçekleştirmesini başarısız aktarımlarını yönetmek ve - ancak değil sunucu-tarafı - istemci tarafı hataları bildirin. İOS 7 görev Kesintiler çalışma zamanı sunulan köprüsü yardımcı olur ve büyük dosyaları hızlı ve güvenilir bir şekilde aktarmak için de destek sağlar. Sonraki bölümde bu ikinci bir özelliği keşfediyor.

### <a name="background-transfer-service"></a>Arka plan Aktarım Hizmeti

İOS 7 önce yükleme veya arka planda dosya indirme güvenilmez. Arka plan görevleri çalıştırmak için sınırlı bir süre yararlanabilirsiniz, ancak dosya boyutu ve ağ ile bir dosya aktarım süresini değişir. İOS 7'de, kullandığımız bir `NSURLSession` başarıyla karşıya yüklemek ve büyük dosyaları indirmek için. Belirli `NSURLSession` ağ aktarımı büyük dosyaları arka planda işleme oturum türü olarak bilinen *arka plan Aktarım Hizmeti*.

Aktarımları arka plan aktarım hizmetini kullanarak tarafından başlatılan işletim sistemi tarafından yönetilir ve kimlik doğrulaması ve hataları işlemek için API'leri sağlar. Aktarımları göre rastgele bir zaman sınırı bağımlı olmadığından kullanılabilmesi için karşıya yükleme veya büyük dosyaları indirmek için otomatik güncelleştirme içeriği arka plan ve daha fazlasını. Başvurmak [arka plan aktarım izlenecek](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) hizmeti uygulama hakkında ayrıntılar için.

Arka plan Aktarım Hizmeti genellikle arka planda getirme veya uygulamaların arka planda içeriğini yenilemek amacıyla uzak bildirimler ile eşleştirilir. Sonraki iki bölümden biz iOS 6 ve iOS 7 arka planda çalıştırılacak tüm uygulamaları kaydetme kavramını sunar.

