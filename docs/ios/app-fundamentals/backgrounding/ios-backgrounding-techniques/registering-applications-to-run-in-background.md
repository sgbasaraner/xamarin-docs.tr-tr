---
title: Arka planda çalışan uygulamaları kaydetme
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 58ee68f765372094d68c3cf30ed6a631a67fe313
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="registering-applications-to-run-in-the-background"></a>Arka planda çalışan uygulamaları kaydetme

Bazı uygulamalar, ancak uygulamanın sürekli bağlı GPS aracılığıyla kullanıcı için yol tarifi alma gibi önemli, uzun süre çalışan görevleri gerçekleştirmek için çağrıldıysa olanlar için arka plan ayrıcalıkları çalışır için tek tek görevler kaydediliyor? Bunlar gibi uygulamaları bilinen arka plan gerekli uygulamaları yerine kaydedilmesi gerekir.

Bir uygulamayı kaydetme, iOS için uygulama arka planda görevleri gerçekleştirmek için gereken özel ayrıcalıklar verilmesi gerektiğini işaret eder.

## <a name="application-registration-categories"></a>Uygulama kayıt kategorileri

Kayıtlı uygulamaları birkaç kategoriye ayrılır:

-  **Ses** -müzik çalar ve ses içerikli çalışan diğer uygulamalar kaydedilebilir devam etmek için uygulama artık ön planda olduğunda bile ses çalma. Bu kategorideki bir uygulamayı play ses veya yükleme sırasında arka planda dışında her şeyi kullanmaya çalışırsa, iOS, sona erdirir.
-  **VoIP** -arka planda ses işleme tutmak için ses uygulamalar için aynı ayrıcalıklarının get Internet protokolü üzerinden ses (VoIP) uygulamaları. Gereken şekilde kendi bağlantılarını Canlı tutmak için bunları güç VoIP Hizmetleri yanıtlamak için de kullanılabilir.
-  **Dış Donatılar ve Bluetooth** -Bluetooth cihazları ve diğer dış donanım Donatılar ile iletişim kurmak için gereken uygulamalar için ayrılmış, bu kategoriler altında kayıt donanıma bağlı kalın yazmasına izin verir.
-  **Newsstand** -A Newsstand uygulama içeriği arka planda eşitlemeye devam edebilir.
-  **Konum** - uygulamalar GPS kullanın veya ağ konumu veri gönderebilir ve konum güncelleştirmeleri arka planda alabilirsiniz.
-  **Getirme (iOS 7 +)** - arka planda getirmeye ayrıcalıkları uygulamaya döndüğünüzde kullanıcı güncelleştirilmiş içerikle sunan düzenli aralıklarla yeni içerik için bir sağlayıcı denetlemek için kayıtlı uygulama.
-  **Uzak bildirimler (iOS 7 +)** -uygulama bir sağlayıcıdan bildirimleri almak için kaydolun ve kullanıcı uygulamayı açar önce devre dışı bir güncelleştirme kazandırın için bildirim kullanın. Bildirimleri anında iletme bildirimleri formunda gelen veya uygulama sessizce uyandırmak için kabul.


Uygulamaları ayarlayarak kaydedilebilir **gerekli arka plan modları** uygulamanın özelliğinde *Info.plist*. Uygulamanın gerektirdiği gibi sayıda kategorilerde kaydedebilirsiniz:

 [![](registering-applications-to-run-in-background-images/bgmodes.png "Arka plan modlarını ayarlama")](registering-applications-to-run-in-background-images/bgmodes.png#lightbox)

Uygulama için arka plan konumu güncelleştirmeleri kaydetme için adım adım yönergeler için bkz: [arka plan konumu izlenecek](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md).

## <a name="application-does-not-run-in-background-property"></a>Uygulama arka plan özelliğinde çalışmaz

Ayarlanabilir başka bir özellik *Info.plist* olan *uygulama arka planda çalışmıyor*, veya `UIApplicationExitsOnSuspend` özelliği:

 [![](registering-applications-to-run-in-background-images/plist.png "Çalıştıran arka plan devre dışı bırakma")](registering-applications-to-run-in-background-images/plist.png#lightbox)

Bu Geliştirici tarafında yalnızca değiştirilebilir dışında iOS 7 + kapalı uygulama arka plan yenileme ayarını tam aynı etkiye sahiptir ve yukarıda ve iOS 4 için kullanılabilir. Uygulama arka hemen girdikten sonra askıya alınır ve herhangi bir işlem yapmak mümkün olmaz.

Uygulamanızı arka plan işlemi, beklenmeyen davranışları önlemek yardımcı olarak işlemek üzere tasarlanmamıştır, bu özelliği kullanın.

## <a name="background-fetch-and-remote-notifications"></a>Arka planda getirmeye ve uzak bildirimler

Arka planda getirmeye ve uzak bildirimler iOS 7 sunulan özel kayıt kategoriler bulunur. Bu kategoriler uygulamaların bir sağlayıcıdan yeni içerik almak ve arka planda güncelleştirmesine izin verin. Sonraki bölümde fetch ve daha ayrıntılı uzak bildirimler inceler ve aynı zamanda bir uygulamanın iOS 6 arka planda güncelleştirmek için anlamına gelir gibi konumu tanıma sunar.
