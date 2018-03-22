---
title: "İOS içinde Backgrounding giriş"
ms.topic: article
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c0369fe52897a2557a92fd56ebcd816b8427faf7
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-backgrounding-in-ios"></a>İOS içinde Backgrounding giriş

iOS arka plan işlemleri çok sıkı bir şekilde düzenler ve uygulamak için üç yaklaşım sunar:

-  **Arka plan görevi kaydettirin** -uygulamanın önemli bir görevi tamamlamak gerekirse, uygulama arka plan içine taşındığında görev kesme değil iOS sorabilirsiniz. Örneğin, bir uygulama bir kullanıcı oturum son veya büyük bir dosya indirme son gerekebilir.
-  **Bir arka plan gerekli uygulaması olarak kayıt** -bir uygulamayı bilinen, uygulama belirli backgrounding gereksinimleri, belirli bir tür olarak da kaydedebilirsiniz *ses* , *VoIP* ,  *Dış aksesuar* , *Newsstand* , ve *konumu* . Bu uygulamalar sürekli arka plan kayıtlı uygulama türü parametreleri içinde görevleri gerçekleştirdiğiniz sürece ayrıcalıkları işlemleri izin verilir.
-  **Arka plan güncelleştirmelerin** -uygulamaları ile arka plan güncelleştirmeleri tetikleyebilir *bölge izleme* veya dinleme *konumu yapılan önemli değişiklikler* . İOS 7 itibariyle uygulamaları da arka plan kullanarak içeriği güncelleştirmek için kaydedebilir *arka plan Fetch* veya *uzak bildirimler* .


## <a name="application-states-and-application-delegate-methods"></a>Uygulama durumları ve uygulama temsilci yöntemleri

Arka plan iOS işlemleri için kod ıntune'un biz önce size bir iOS uygulaması yaşam döngüsü nasıl backgrounding etkiler anlamanız gerekir.

İOS uygulama yaşam döngüsü, uygulama durumları ve bunlar arasında taşıma yöntemleri koleksiyonudur. Uygulamanın kullanıcı davranışını ve uygulamanın backgrounding gereksinimlerine göre durumları arasında geçiş yapar. Taşıma tarafından Aşağıdaki diyagramda gösterilmiştir:

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Uygulama durumları ve uygulama temsilci yöntemlerini diyagramı")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **Çalışmıyor** -uygulama henüz cihazda başlatıldı değil.
-  **Çalışan/etkin** -uygulama ekranda ve bir kod ön planda yürütüyor.
-  **Etkin olmayan** -uygulama gelen telefon aramasını, metin veya diğer kesinti tarafından kesildi.
-  **Backgrounded** -uygulama arka plan içine taşır ve arka plan kod yürütmeye devam eder.
-  **Askıya alındı** - uygulama arka planda çalıştırmak için herhangi bir kod yok ya da tüm kod tamamladıysa, uygulama olacaktır *askıya* işletim sistemi tarafından. Askıya alınan bir uygulama işlemi Canlı tutulur, ancak bu durumda herhangi bir kod yürütemedi bir uygulamadır.
-  **İade değil çalışan/sonlandırma için (Rare)** - bazen, uygulamanın işlem yok ve uygulama döndürür *çalışmıyor* durumu. Düşük bellek durumlarda aşması veya kullanıcı, uygulamayı el ile sona erer.


' Den itibaren görevli destek giriş, iOS nadiren boşta uygulamaları sonlandırır ve bunun yerine süreçlerinin tutar *askıya* bellekte. Bir uygulama işlemi canlı tutma uygulamayı hızlı bir şekilde kullanıcı, bir sonraki açılışına başlatan sağlar. Aynı zamanda uygulamaları taşıyabilirsiniz serbestçe öğesinden gelir *askıya* uygulamasına geri durum *Backgrounded* sistem kaynaklarını çizim olmadan durum. iOS 7 bu özellik Aygıt uyku moduna güncelleştirme içeriğini doğrudan ve kullanıcı etkileşimi olmadan arka gittiğinde arka plan görevleri duraklatmak uygulamalar sağlayan yeni API ile yararlanan. Yeni API'leri ele alınacaktır [iOS Backgrounding teknikleri](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Uygulama yaşam döngüsü yöntemleri

Bir uygulama durumu değiştiğinde, iOS uygulamanın olay yöntemleri aracılığıyla bildirir. `AppDelegate` sınıfı:

-  `OnActivated` -Bu uygulama başlatıldığında, ilk kez denir ve her zaman uygulamanın ön alana gelir. Bu uygulamayı her açıldığında çalıştırmak için gereken kodu yerleştirilecek yerdir.
-  `OnResignActivation` -Kullanıcı bir metin veya telefon görüşmesi gibi bir kesinti alırsa, bu yöntem çağrılır ve uygulama geçici olarak devre. Kullanıcının telefon araması kabul etmelidir, uygulama arka plana gönderilir.
-  `DidEnterBackground` -Bu yöntem uygulama backgrounded durumuna girdiğinde çağrılır, olası sonlandırma için hazırlamak için yaklaşık beş saniyede bir uygulama sağlar. Bu süre kullanıcı verileri ve görevler kaydetmek ve ekranından önemli bilgileri kaldırmak için kullanın.
-  `WillEnterForeground` -Kullanıcı backgrounded veya askıya alınmış bir uygulamaya döndürür ve ön alana başlatır `WillEnterForeground` çağrılır. Bu ön sırasında kaydedilen herhangi bir durum yeniden tarafından uygulamanın hazırlamak için zamandır `DidEnterBackground` .  `OnActivated` Bu yöntem hemen tamamlandıktan sonra çağrılır.
-  `WillTerminate` -Uygulama kapatılır ve kendi işlemi yok. Çoklu bellek düşükse ya da kullanıcı el ile backgrounded uygulama ererse cihaz veya işletim sistemi sürümü kullanılabilir değilse, bu yöntem yalnızca çağrılır. Sonlandırıldı askıya alınmış uygulamaları değil çağıracak Not `WillTerminate` .


Aşağıdaki diyagram nasıl uygulama durumları gösterir ve yaşam döngüsü yöntemlerini bir araya getireceğinizi:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "Bu diyagramda nasıl uygulama durumları gösterilir ve yaşam döngüsü yöntemlerini bir araya getireceğinizi")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>Kullanıcı denetimleri için iOS Backgrounding

iOS 7 uygulamanın backgrounded durumu hakkında daha fazla denetime kullanıcılara vermek için çeşitli özellikler sunar. Uygulama değiştirici ve uygulama arka planda Yenileme ayarı uygulama yaşam döngüsü etkiler.

### <a name="app-switcher"></a>Uygulama değiştirici

Uygulama değiştirici iOS 7 sunulan, önemli denetim bir özelliktir. Çift dokunarak başlatılan **giriş** düğmesine tıklayın ve işlemlerini Canlı olan uygulamalar gösterir:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Uygulama değiştirici kullanarak uygulamalar arasında taşıma")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

Uygulama değiştirici kullanarak, kullanıcıların tüm backgrounded ve askıya alınmış uygulamaları anlık görüntüleri gezinebilirsiniz. Bir uygulama dokunarak ön alana başlatır. Yukarı geçirmeyi uygulamayı, işlem sonlandırılıyor arka plandan kaldırır. Biz uygulama değiştirici yakından sürer [iOS uygulama yaşam döngüsü Demo](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) sonraki bölümde.

> [!IMPORTANT]
> Uygulama değiştirici backgrounded ve askıya alınmış uygulamalar arasında bir fark göstermez.



### <a name="background-app-refresh-settings"></a>Arka plan uygulama yenileme ayarları

iOS 7, kullanıcıların uygulamaları için backgrounding dışında opt izin vererek uygulama yaşam döngüsü üzerinden kullanıcı denetimini artırır [arka plan işleme için kayıtlı](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *Bu uygulamaların arka plan görevleri çalışmasını engellemez*.

Kullanıcılar giderek bu ayarı değiştirebilir <span class="uiitem">ayarlar > Genel > uygulama arka plan yenileme</span> ve seçilen bir uygulamaya backgrounding ayrıcalıklarını düzenleme. Uygulama arka plan yenileme off olarak ayarlanırsa, uygulama arka plan girildikten sonra hemen askıya ve tüm arka plan işleme engel engelledi:

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Arka plan uygulama yenileme ayarları")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Geliştiriciler, ile arka planda yenileme uygulama durumunu denetleyebilirsiniz `BackgroundRefreshStatus` API. Bir örnek için bkz [denetleme arka plan yenileme ayarı tarif](https://developer.xamarin.com/recipes/ios/multitasking/check_background_refresh_setting/).

Biz iOS uygulama yaşam döngüsü ve uygulama yaşam döngüsü denetleme özellikleri temelleri ele. Ardından, eylem iOS uygulama yaşam döngüsü görelim.

