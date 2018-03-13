---
title: "Arka planda bir uygulamayı güncelleştirme"
ms.topic: article
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f4a18bf8f35d1a6c615c819ea90433d1eb123422
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="updating-an-application-in-the-background"></a>Arka planda bir uygulamayı güncelleştirme

Arka plan yenilemesi askıya alınmış bir uygulamayı Uyandırma veya çalışmıyor ve yeni içerikle güncelleştirme işlemidir. iOS içeriği arka planda yenileme için üç seçenek sunar:

1.  *Bölge izleme* ve *önemli konum değişiklikleri hizmeti* -konum algılayan API'ler tetikleyici arka plan kullanıcının konumuna göre değişiklikleri güncelleştirir. Bu API'leri dikkatli olun iOS 6 konum tabanlı olmayan uygulamalar, içeriği yenilemek için diğer seçeneklerinin kullanılabildiği değil kullanılabilir.
1.  *Arka plan Fetch (iOS 7 +)* -zamana bağlı bir yaklaşım yenileme *kritik olmayan* güncelleştirmeleri içeriği *sık* .
1.  *Uzak bildirimler (iOS 7 +)* -anında iletme bildirimleri alma uygulamaları bildirimleri arka plan içerik yenilemeleri tetiklemek için kullanabilirsiniz. Bu yöntem ile güncelleştirmek için kullanılan *önemli, zamana duyarlı* güncelleştirmeleri içeriği *düzensiz aralıklarla hataya* .


Aşağıdaki bölümlerde bu seçeneklerin temel kavramları kapsar.

## <a name="region-monitoring-and-significant-location-changes"></a>Bölge izleme ve önemli konumu değişiklikleri

iOS özellikleri backgrounding ile iki konum algılayan API'ler sağlar:

1.  *Bölge izleme* sınırlarla bölgeler ayarlama ve kullanıcı girdiğinde veya bir bölgede çıktığında cihazın uyku modundan işlemidir. Bölgeler döngüsel ve değişen boyutu olabilir. Kullanıcı bir bölge sınırı kestiği, cihaz olayı genellikle bir bildirim tetikleme veya bir görevi devre dışı oluşturan işlemek için uyandırmak. Bölge izleme GPS gerektirir ve pil ve veri kullanımı artırır.
1.  *Önemli konum değişiklikleri hizmeti* daha basit, güç tasarrufu seçenek cep telefonu Radyoları olan aygıtlar için kullanılabilir. Cihaz hücre towers geçtiğinde önemli konumu değişikliklerini dinleyen bir uygulamanın bildirilir. Bu hizmet askıya alındı veya işten çıkarılan uygulama uyandırmak için kullanılabilir ve yeni içerik arka planda denetleme olanağı sağlar. İle eşleştirilmiş sürece arka plan etkinliği yaklaşık 10 saniye için sınırlı bir [arka plan görevi](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) .


Bir uygulama konumu gerekmez `UIBackgroundMode` bu konum algılayan API'leri kullanılacak. İOS aygıtı kullanıcının konumunu değişikliklerinden uyandırıldığında çalıştırılabilen görevleri türlerini izlemez olduğundan, bu API'leri bir geçici çözüm iOS 6 arka planda içeriği güncelleştirmek için sağlar. *Arka plan güncelleştirmeleri konum temelli API'leri ile tetikleme aygıt kaynaklardaki çizer ve neden bir uygulama konumlarına erişmesi anlamadığınız kullanıcılar karışıklığa neden olabilir göz önünde bulundurmanız*. Arka plan API'leri konumu zaten kullanmadığınız uygulamalarda işlemleri bölge izleme veya konum yapılan önemli değişiklikler uygularken dikkatli olun.

Arka plan işleme için konum izleme kullanan uygulamalar kullanıma bir kusur iOS 6: bir uygulamanın gereksinimlerini bir arka plan gerekli kategoriye sığmıyorsa backgrounding seçenekleri sınırlıdır. İki yeni API'lerin girişiyle *arka plan Fetch* ve *uzak bildirimler*, iOS 7 (ve büyük) daha fazla backgrounding fırsatı sağlar. Sonraki iki bölümde bu yeni API'ler sunar.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>Arka planda getirmeye (iOS 7 ve daha sonraki sürümleri)

İOS 6'da, kullanıcıların önceden gördüğünüz içerikle kısaca sunan yeni içerik yüklemek için süre ön girerek bir uygulama gerekiyor. Arka planda getirmeye izin verir, yeni verileri yüklemek uygulamalar *önce* kullanıcı uygulamayı başlatır ve en güncel içerikle kullanıcı sağlayın.

Arka planda getirmeye uygulamak için düzenleme *Info.plist* ve denetleme **arka plan modlarını etkinleştir** ve **arka planda getirmeye** onay kutularını:

 [![](updating-an-application-in-the-background-images/fetch.png "Info.plist düzenleyin ve arka plan modlarını etkinleştir ve arka plan Fetch onay kutularını işaretleyin")](updating-an-application-in-the-background-images/fetch.png#lightbox)

İleri ' `AppDelegate`, geçersiz kılma `FinishedLaunching` minimum fetch aralığını ayarlamak için yöntem. Bu örnekte, genellikle yeni içerik almak nasıl karar OS sağlar:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

Son olarak, geçersiz kılarak fetch gerçekleştirmek `PerformFetch` yönteminde `AppDelegate`ve içinde geçen bir *tamamlama işleyici*. Tamamlama işleyici alır temsilcisidir bir `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

Güncelleştirme içeriği de bittiğinde, biz uygun durumu tamamlama işleyiciyle çağırarak bilmeniz OS olanak tanır. iOS tamamlama işleyici durumu için üç seçenek sunar:

1.  `UIBackgroundFetchResult.NewData` -Yeni içerik getirildi ve uygulamanın güncelleştirilmiş çağrılır.
1.  `UIBackgroundFetchResult.NoData` -Getirme için yeni içerik kurulmadan, ancak içerik kullanılabilir olduğunda çağrılır.
1.  `UIBackgroundFetchResult.Failed` -Fetch geçtikleri bulamadı hata işleme için yararlı, bu denir.


Arka plan Fetch kullanan uygulamalar, arka plan Arabiriminden güncelleştirmek için çağrıları yapabilirsiniz. Uygulama kullanıcı oturum açtığında, kullanıcı Arabirimi en fazla tarih ve yeni içerik görüntüleme olacaktır. Kullanıcı uygulamayı yeni içerik olduğunda görebilmek için bu uygulamanın uygulama değiştirici anlık görüntüyü güncelleştirir.

> [!IMPORTANT]
> **Not**: bir kez `PerformFetch` olduğu olarak adlandırılan, uygulaması yaklaşık olarak 30 saniye yeni içerik indirme kazandırın ve tamamlanma işleyici blok çağırmak için vardır. Uygulama bu çok uzun sürerse sonlandırılır. Arka plan getirme ile kullanmayı _arka plan Aktarım Hizmeti_ medya veya diğer büyük dosyaları yüklerken.


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

Yukarıdaki örnek kodda biz genellikle minimum fetch aralığı ayarlayarak yeni içerik almak nasıl karar OS izin `BackgroundFetchIntervalMinimum`. iOS için fetch aralığı üç seçenek sunar:

1.  `BackgroundFetchIntervalNever` -Hiçbir zaman yeni içerik almak için sistem söyleyin. Ne zaman kullanıcı oturum açmadı gibi bazı durumlarda, getirme kapalı etkinleştirmek için bunu kullanın. Fetch aralığı için varsayılan değer budur. 
1.  `BackgroundFetchIntervalMinimum` -Kullanıcı desenleri, pil ömrünün, veri kullanımı ve diğer uygulamalar gereksinimlerine göre genellikle getirmek nasıl karar sistem sağlar.
1.  `BackgroundFetchIntervalCustom` -Bir uygulama içeriğinin ne sıklıkta güncelleştirilir biliyorsanız, uygulamanın hangi sırasında engellenir yeni içerik getirme her getirmeden sonra "uyku" aralığı belirtebilirsiniz. Bu aralığı yukarı olduğunda sistem içeriği getirme zamanı belirler.


Her ikisi de `BackgroundFetchIntervalMinimum` ve `BackgroundFetchIntervalCustom` öğesinden zamanlamak için sistemde kullanır. Bu aralık, tek tek kullanıcının alışkanlıklarınıza yanı sıra cihazın gereksinimlerine uyarlama dinamik bir işlemdir. Uygulama her açışlarında Örneğin, bir kullanıcı bir uygulama her sabah denetler ve içerik başka bir denetimleri her saat iOS sağlayacak hem kullanıcılar için güncel olduğundan.

Arka planda getirmeye sık kritik olmayan içerikle güncelleştirme uygulamalar için kullanılmalıdır. Kritik güncelleştirmelere sahip uygulamalar için Uzak bildirimler kullanılmalıdır. Uzak bildirimler arka plan Fetch temel alır ve aynı tamamlama işleyici paylaşın. Biz uzak bildirimlerini filtrelerle İleri dalın.

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>Uzak bildirimler (iOS 7 ve daha sonraki sürümleri)

Anında iletme bildirimleri olan bir sağlayıcıdan bir aygıt tarafından yolu, gönderilen JSON iletileri *Apple anında iletilen bildirim servisi (APNs)*.

İOS 6'da, bir gelen anında iletme bildirimleri ilgi çekici bir şey bir uygulamada gerçekleştirilmedi kullanıcıyı uyarmak için sistem söyler. Bildirime tıklayarak uygulamayı askıya alındı veya işten çıkarılan durumdan çeker ve uygulama içeriği güncelleştirme başlayın. iOS 7 (ve büyük) arka planda içeriği güncelleştirmek için bir fırsat uygulamaları vererek sıradan anında iletme bildirimleri genişletir *önce* böylece kullanıcı uygulamayı açın ve yeni içerikle sunulan kullanıcı bildirme hemen.

Uzak bildirimleri uygulamak için düzenleme *Info.plist* ve denetleme **arka plan modlarını etkinleştir** ve **uzak bildirimler** onay kutularını:

 [![](updating-an-application-in-the-background-images/remote.png "Arka plan modlarını etkinleştir ve Uzaktan bildirimler için arka plan modunu ayarlama")](updating-an-application-in-the-background-images/remote.png#lightbox)

Ardından, ayarlayın `content-available` anında iletme bildirimi kendisini 1 bayrağı. Bu uyarı görüntülenmeden önce yeni içerik almak için bilmeniz uygulama sağlar:

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

İçinde *AppDelegate*, geçersiz kılma `DidReceiveRemoteNotification` kullanılabilir içeriği için bildirim yükü denetleyin ve uygun tamamlama işleyici blok çağırmak için yöntem:

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

Uzak bildirimler için uygulamanın işlevselliğini önemlidir içerikle sık güncelleştirmeler için kullanılmalıdır. Xamarin uzak bildirimleri hakkında daha fazla bilgi için bkz: [iOS anında iletme bildirimleri](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) Kılavuzu.

> [!IMPORTANT]
> **Not**: Uzak Bildirimlerde güncelleştirme mekanizması arka plan Fetch dayalı olan, uygulama yeni içerik indirme kazandırın ve bildirim alma sonra 30 saniye içinde tamamlama işleyici blok çağrısı veya iOS olur çünkü Uygulama sonlanır. Uzak bildirimlerle eşleştirme göz önünde bulundurun _arka plan Aktarım Hizmeti_ ortam veya diğer büyük dosyaları arka planda yüklenirken.


### <a name="silent-remote-notifications"></a>Sessiz uzak bildirimler

Uzak bildirimler güncelleştirmeleri uygulamalarının bilgilendirin ve yeni içerik getirme kapalı kazandırın yönelik basit bir yöntem olsa da burada bir şey değişti kullanıcıya bildirmek için gerekli değil durumlar vardır. Örneğin, bir kullanıcı senkronize için bir dosya gösterirse, biz dosya güncelleştirmeleri her zaman bildirmek gerek yoktur. Dosya senkronize şaşırtıcı bir olay değildir ve kullanıcının hemen ilgilenilmesi gereken yapar. Kullanıcılar yalnızca dosyasının kullanıcıların açtıklarında güncel olmasını bekler.

Yukarıdakine gibi durumlarda, iOS anında iletme bildirimleri sessizce - diğer bir deyişle, bir uyarı gönderilmesini sağlar. Normal bir bildirim sessiz bir oturum açmak için uyarı bildirim yükü kaldırın:

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>Oran sınırları

Normal ve sessiz bildirimleri Geliştirici açısından büyük birbirinden sessiz iter oranı sınırlı olmasıdır. Gönderme oranı çok yüksek alırsa APNs sessiz iter teslim aygıta geciktirir. Bu uygulamalar çok fazla sessiz bildirimleri ile aygıt kaynaklarını boşaltın yok sağlamaktır.

Ancak, APNs sessiz bildirimleri "Paketle" normal uzaktan bildirim veya tutma yanıt yanında sağlayacaktır. Normal bildirimleri oranı sınırlı olduğundan, bunlar cihaza APNs anında yukarı saklı sessiz bildirimler için tarafından Aşağıdaki diyagramda gösterildiği gibi kullanılabilir:

 [![](updating-an-application-in-the-background-images/silent.png "Normal bildirimleri cihaza APNs anında saklı sessiz bildirimler için tarafından bu diyagramda gösterildiği gibi kullanılabilir")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> **Not**: Apple uygulama gerektirir ve let APNs zamanlama kendi teslim sessiz anında iletme bildirimleri göndermek için geliştiricilere önerir.


Bu bölümde, biz bir arka plan gerekli kategoriye uymayan görevleri çalıştırmak için içerik arka planda yenileme için çeşitli seçenekler ele. Şimdi, bu API'leri eylem bazıları görelim.

 [Sonraki: Bölüm 4 - iOS Backgrounding izlenecek yollar](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
