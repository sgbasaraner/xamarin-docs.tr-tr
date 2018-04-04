---
title: Bildirimler
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 1a681c2bda941d8fe015a8d4da8b99f4d85e441b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="notifications"></a>Bildirimler

Bunları içeren iOS uygulaması destekliyorsa, izleme uygulamaları bildirimleri alabilirsiniz. Olduğundan bunu yapmanız için yerleşik bildirimini işleme *gerek* ancak aşağıda açıklanan ek bildirim desteği eklemek için bildirim davranışını özelleştirmek istediğiniz ve sonra Görünüm okuyabilirsiniz.

Başvurmak [iOS bildirimleri](~/ios/platform/user-notifications/deprecated/index.md) belge çözümünüzdeki iOS uygulamasına bildirim desteği ekleme hakkında daha fazla bilgi.

## <a name="creating-notification-controllers"></a>Bildirim denetleyicileri oluşturma

Şeridinde bildirimleri denetleyicileri bunları tetikleme ü özel bir türüne sahip. Yeni bir sürüklediğinizde **bildirim arabirimi denetleyicisinin** film şeridi bağlı segue otomatik olarak gerekir:

![](notifications-images/notification-storyboard1.png "Bağlı bir segue ile yeni bir bildirim arabirimi denetleyicisi")

Ne zaman bildirim ü seçili özelliklerini düzenleyebilirsiniz:

![](notifications-images/notification-storyboard2.png "Bildirim ü seçili")

Denetleyici özelleştirdikten sonra bu örnekten WatchKitCatalog gibi görünebilir:

![](notifications-images/notifications-segue.png "Bildirim özellikleri")


Bildirim iki tür vardır:

- **Kısa Görünüm** -sistem tarafından tanımlanan kaydırılamayan statik görünüm.

- **Uzun Görünüm** - kaydırılabilir, özelleştirilebilir bir görünüm tarafından tanımlanan! Daha basit ve statik bir sürüm ve daha karmaşık bir dinamik sürüm belirtilebilir.

### <a name="short-look-notification-controller"></a>Kısa görünüm bildirim denetleyicisi

Kısa arama kullanıcı Arabirimi, yalnızca uygulama simge, uygulama adı ve bildirim başlığı dize oluşur.

Kullanıcı bildirimi yoksaymaz, sistem daha fazla bilgi sağlayan bir uzun görünüm bildirim otomatik olarak geçiş yapar.


### <a name="long-look-notification-controller"></a>Uzun görünüm bildirim denetleyicisi

İşletim sistemi bir dizi etkene göre statik veya dinamik görünüm görüntülenip görüntülenmeyeceğini belirler. Statik bir arabirimi ve isteğe bağlı olarak için bildirimler için dinamik bir arabirim de içerir.

#### <a name="static"></a>Statik

Statik Görünüm basit ve görüntülemek hızlı olması gerekir.

![](notifications-images/notification-static.png "Statik Görünüm")

#### <a name="dynamic"></a>Dinamik

Dinamik görünüm daha fazla veri görüntülemek ve daha fazla etkileşim sağlar.

![](notifications-images/notification-dynamic.png "Dinamik Görünüm")


## <a name="generating-notifications"></a>Bildirim oluşturma

Bildirimler, uzak bir sunucudan gelebilir ([Apple anında iletilen bildirim servisi](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), veya APNS) veya iOS uygulama yerel olarak oluşturulabilir.

Başvurmak [iOS bildirimleri izlenecek](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) yerel bildirimler oluşturmak nasıl bir örnek için ve [WatchNotifications örnek](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/) çalışan bir örnek için.

Yerel bildirimler olmalıdır `AlertTitle` üzerinde Apple Watch - görüntülenecek ayarlamak `AlertTitle` dize kısa görünüm arabiriminde görüntülenir. Hem `AlertTitle` ve `AlertBody` bildirimler listesinde; görüntülenir ve `AlertBody` uzun görünüm arabiriminde görüntülenir.

Bu ekran gösterir `AlertTitle` bildirimleri listede görüntülenen ve `AlertBody` uzun görünüm arabiriminde görüntülenen (kullanarak [örnek koduna](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)):

![](notifications-images/watch-notificationslist-sml.png "Bu ekran bildirimler listesinde görüntülenen AlertTitle gösterir") ![ ] (notifications-images/watch-notificationcontroller-sml.png "uzun görünüm arabiriminde görüntülenen AlertBody")

## <a name="testing-notifications"></a>Bildirimleri test etme

Bildirimleri (yerel ve uzak) yalnızca düzgün test bir aygıtta bunlar kullanılarak benzetimi ancak bir **.json** iOS simülatörü'nü dosyasında.

### <a name="testing-on-apple-watch"></a>Apple Watch üzerinde test etme

Bir Apple Watch bildirimlerini test edilirken unutmayın [Apple belgeleri](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) aşağıdaki durumları:

> Uygulamanızın yerel veya uzak bildirimler birini kullanıcının iPhone üzerinde geldiğinde, iOS bu bildirim iPhone veya Apple Watch görüntülenip görüntülenmeyeceğini belirler.

Bu olguya alluding, iOS bir bildirim iPhone veya izleme görünüp görünmeyeceğini belirler. Bir bildirim alındığında eşleştirilmiş iPhone etkinse, bildirim iPhone üzerinde görüntülenecek olasıdır ve *değil* saate yönlendirilir.

Watch'unuzda bildirimi görünür emin olmak için (bir kez güç düğmesine basıldığında) iPhone ekranı kapatabilir veya, uyku moduna geçmesine izin. Eşleştirilmiş gözlem aralığında, gücü ve bileğinizi üzerinde yıpranmış, bildirim var. yönlendirilir ve (bir Zarif eşlik) izleme görünür.

### <a name="testing-on-the-ios-simulator"></a>İOS simülatörü'nü test etme

*Gerekir* iOS Simulator'da bildirim modu test edilirken bir test JSON yükü sağlayın. Yolun kümesinde **özel yürütme bağımsız** Visual Studio'daki Mac için

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio, ek seçenekler görüntülenir, bir izleme uzantısı olarak ayarlandığında **başlangıç projesi**.
İzleme uzantısı projeye sağ tıklayın ve seçin **çalıştırmak ile > özel parametreler...** :
    
[![](notifications-images/runwith-customparams-sml.png "Özel özellikler ile çalıştırma")](notifications-images/runwith-customparams.png#lightbox)
    
Bu açılır **yürütme bağımsız değişkenleri** içeren pencere bir **WatchKit** sekmesi. Seçin **bildirim** ve bir JSON yükü sağlamak tuşuna basarak **yürütme** benzeticisinde izleme uygulamasını başlatmak için:
    
[![](notifications-images/runwith-execargs-sml.png "Bildirim yükü varsayılan seçin")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sınama bildirim yükü Visual Studio sağ düzenlemek için izleme uzantısı ayarlamak için **proje özellikleri**. Git **hata ayıklama** bölüm ve bir bildirim JSON dosyası (bunu otomatik olarak listeler projeye dahil tüm JSON dosyaları) listeden seçin.
    
[![](notifications-images/runwith-execargs-sml-vs.png "Bildirim JSON dosyasını seçin")](notifications-images/runwith-execargs-vs.png#lightbox)

İzleme uzantısı olduğunda **başlangıç projesi**, aşağıda gösterildiği gibi Visual Studio ek seçenekler görüntülenir. Aşağıdakilerden birini seçin **bildirim** izleme uygulamayı başlatmak için seçenekleri **bildirim** modu (Özellikleri penceresinde seçili JSON dosyası kullanarak):
    
![](notifications-images/runwith-vs.png "Cihaz menüsü")

-----

Varsayılan bildirim denetleyicisi simulator'da varsayılan iş yükü JSON dosyasının ile sınarken şöyle:

![](notifications-images/notification-debug-sml.png "Bir örnek bildirimi")

Kullanmak da mümkündür [komut satırı](~/ios/watchos/troubleshooting.md#command_line) iOS simülatörü'nü başlatmak için.

### <a name="example-notification-payload"></a>Örnek bildirim yükü

İçinde [izleme Seti katalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) örnek var olan bir örnek iş yükü JSON dosyasının **NotificationPayload.json** (aşağıda listelenen).

```csharp
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```



## <a name="related-links"></a>İlgili bağlantılar

- [WatchNotifications (yerel bildirimleri) (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)
- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Apple'nın izleme Seti bildirimleri belgeleri](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
