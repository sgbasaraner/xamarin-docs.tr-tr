---
title: İOS arka planda işlemeye giriş
description: 'Bu belge iOS arka planda işleme açıklar: uygulama durumları, uygulama yaşam döngüsü yöntemleri ve arka plan uygulama yenileme.'
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/24/2018
ms.openlocfilehash: 621a2d25a8b800ad95be2c8b71a5e6cdf6abca21
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351209"
---
# <a name="introduction-to-backgrounding-in-ios"></a>İOS arka planda işlemeye giriş

iOS arka plan işlemleri çok sıkı bir şekilde düzenler ve uygulamak için üç bir yaklaşım sunar:

-  **Arka plan görevi kaydettirin** -bir uygulamanın önemli bir görevi tamamlamak gerekiyorsa, uygulama arka planda hareket ettiğinde görev kesme değil için iOS isteyebilir. Örneğin, bir uygulama son kullanıcı olarak oturum veya son büyük dosya indirme gerekebilir.
-  **Arka planda gerektiğinde uygulaması olarak kayıt** -bir uygulamayı bilinen, bir uygulamanın arka planda işleme belirli gereksinimleri belirli bir tür olarak da kaydedebilirsiniz *ses* , *VoIP* ,  *Dış aksesuar* , *Newsstand* , ve *konumu* . Bu uygulamaları sürekli arka plan kayıtlı uygulama türünde parametre içinde olan görevleri gerçekleştirdiği sürece ayrıcalıkları işleme izin verilir.
-  **Arka plan güncelleştirmelerin** -uygulamalar ile arka plan güncelleştirmeleri tetikleyebilir *bölge izleme* veya dinleniyor *konumu yapılan önemli değişiklikler* . İOS 7 itibariyle, uygulamaları da arka plan kullanarak içeriği güncelleştirmek için kaydedebilir *arka planda getirme* veya *uzak bildirimler* .


## <a name="application-states-and-application-delegate-methods"></a>Uygulama durumları ve uygulama temsilci yöntemleri

İOS arka plan kodu derinlerine biz önce size bir iOS uygulaması yaşam döngüsü nasıl arka planda işleme etkiler anlamanız gerekir.

İOS uygulama yaşam döngüsü, uygulama durumları ve bunlar arasında taşımak için yöntemleri oluşan bir koleksiyondur. Uygulamanın, kullanıcı davranışını ve uygulamanın backgrounding gereksinimlerinize göre durumlar arasında geçiş yapar. Taşıma tarafından Aşağıdaki diyagramda gösterilmiştir:

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Uygulama durumları ve uygulama temsilci yöntemleri diyagramı")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **Çalışmıyor** -uygulama henüz cihazda başlatıldı değil.
-  **Çalışan/etkin** -uygulama ekran üzerinde ve bir ön planda kodu yürütülüyor.
-  **Etkin olmayan** -gelen bir telefon görüşmesi, metin veya diğer kesinti uygulama kesintiye uğrar.
-  **Backgrounded** -uygulama arka planda taşır ve arka plan kod yürütmeye devam eder.
-  **Askıya alındı** - uygulama arka planda çalıştırmak için herhangi bir kod yok veya tüm kod tamamladıysa, uygulama olacak *askıya alındı* işletim sistemine göre. Askıya alınan bir uygulamanın işlem canlı olarak tutulur, ancak uygulama bu durumda herhangi bir kod yürütmek düzenleyemedi.
-  **İade değil çalıştırma/sonlandırması yükünü (Rare)** - bazen, uygulamanın işlem yok ve uygulamaya döndürür *çalışmıyor* durumu. Düşük bellek durumlarda böyle veya kullanıcı, uygulamayı el ile sona erer.


Giriş çok görevli destek itibaren iOS nadiren boşta uygulamaları sonlandırır ve bunun yerine, işlemlerinin tutar *askıya alındı* bellekte. Uygulamayı hızla kullanıcı, bir sonraki açışında başlatan uygulamanın işlem Canlı kalmasını sağlar. Ayrıca uygulamalar taşıyabilir serbestçe geldiğini *askıya alındı* geri durum *Backgrounded* sistem kaynaklarını çizim olmadan durum. Bu özellik, cihazın uyku moduna güncelleştirme içeriği ve kullanıcı etkileşimi olmadan arka doğrudan gittiğinde arka plan görevlerini duraklatmak uygulamaları etkinleştirme yeni API'ler ile iOS 7 yararlanan. Bunu yeni API'ler ele [iOS arka planda işleme teknikleri](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Uygulama yaşam döngüsü yöntemleri

Uygulama durumu değiştiğinde, iOS uygulama olay yöntemleri aracılığıyla bildirir. `AppDelegate` sınıfı:

-  `OnActivated` -Bu uygulama başlatıldığında ilk zaman çağrılır ve her zaman uygulama önplana gelir. Bu uygulama her açıldığında çalıştırmak için gereken kod için yerdir.
-  `OnResignActivation` -Kullanıcı bir kesinti gibi bir metin ya da telefon aramasına alırsa, bu yöntem adı ve uygulama geçici olarak devre dışı bırakılır. Kullanıcının telefon araması kabul edin, uygulama arka plana gönderilir.
-  `DidEnterBackground` -Uygulama backgrounded durumuna girdiğinde çağrılır, bu yöntem için olası sonlandırma hazırlamak için yaklaşık beş saniyeden uygulamaya verilir. Bu süre, kullanıcı verileri ve görevler kaydedin ve önemli bilgileri ekrandan kaldırmak için kullanın.
-  `WillEnterForeground` -Bir kullanıcı backgrounded veya askıya alınmış bir uygulamaya döndürür ve önplana başlatır `WillEnterForeground` uğradığında çağrılır. Bu ön sırasında kaydedilmiş herhangi bir durumu yeniden veriyle doldurulurken uygulamanın hazırlamak için süredir `DidEnterBackground` .  `OnActivated` Bu yöntem hemen tamamlandıktan sonra çağrılır.
-  `WillTerminate` -Uygulama kapatılır ve kendi işlemi yok. Çok görevli bellek düşükse veya el ile sona erer backgrounded uygulama kullanıcı cihaz veya işletim sistemi sürümü mevcut değilse bu yöntem yalnızca adı. Sonlandırılan askıya alınmış uygulamalar değil çağırır unutmayın `WillTerminate` .


Aşağıdaki diyagram, uygulamanın nasıl durumları gösterir ve yaşam döngüsü yöntemlerini bir araya getireceğinizi:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "Bu diyagram nasıl uygulama durumları gösterir ve yaşam döngüsü yöntemlerini bir araya getireceğinizi")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>İos'ta arka planda işlemeye için kullanıcı denetimleri

iOS 7 backgrounded uygulamanın durumu hakkında daha fazla denetime kullanıcılara vermek için çeşitli özellikler eklendi. Uygulama yaşam döngüsünü hem uygulama değiştirici hem de uygulamayı arka planda yenileme ayarını etkiler.

### <a name="app-switcher"></a>Uygulama değiştirici

Uygulama iOS 7 ' sunulan bir önemli denetim özelliğini değiştiricisidir. Çift dokunarak onu başlatan **giriş** düğmesini ve işlemlerini Canlı olan uygulamalar gösterir:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Uygulama değiştiriciyi kullanarak uygulamalar arasında taşıma")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

Uygulama değiştiriciyi kullanarak, kullanıcılar tüm backgrounded ve askıya alınan uygulamaları anlık görüntüler kaydırabilirsiniz. Bir uygulama dokunarak önplana başlatır. Yukarı çekerek uygulama, işlem sonlandırılıyor arka planda kaldırır. Biz uygulama değiştirici yakından sürer [iOS uygulama yaşam döngüsü Tanıtımı](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) sonraki bölümde.

> [!IMPORTANT]
> Uygulama değiştirici backgrounded ve askıya alınmış uygulamalar arasında bir farklılık göstermez.



### <a name="background-app-refresh-settings"></a>Arka plan uygulama yenileme ayarları

iOS 7 uygulamaları için arka planda işleme dışında iyileştirilmiş olanak tanıyarak kullanıcı denetime uygulama yaşam döngüsünü artırır [arka plan işlemesi için kayıtlı](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *Bu arka plan görevleri çalıştırmanın uygulamaların engellemez*.

Kullanıcılar, giderek bu ayarı değiştirebilirsiniz <span class="uiitem">ayarlar > Genel > uygulama arka planda yenileme</span> ve seçilen bir uygulamaya backgrounding ayrıcalıklarını düzenleme. Uygulama arka planda yenileme, off olarak ayarlanırsa, uygulama arka plan girildikten sonra hemen askıya ve herhangi bir arka plan işleme yapmasını engelledi:

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Arka plan uygulama yenileme ayarları")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Geliştiriciler, arka planda yenileme uygulama durumunu kontrol edebilirsiniz `BackgroundRefreshStatus` API. Örneğin, başvurmak [arka planda Yenileme ayarı denetlemek tarif](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting).

İOS uygulama yaşam döngüsü ve uygulama yaşam döngüsünü denetleyen özellikler hakkındaki temel bilgileri ele aldığımız. Ardından, eylem iOS uygulama yaşam döngüsü görelim.

