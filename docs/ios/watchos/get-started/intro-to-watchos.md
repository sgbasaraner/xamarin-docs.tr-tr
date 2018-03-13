---
title: "WatchOS giriş"
description: "WatchOS çözüm yapısını ve kısıtlamaları'na genel bakış"
ms.topic: article
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 2276b67fc29f2752e4b178168a12e6e980b788d0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-watchos"></a>WatchOS giriş

> [!NOTE]
> **Not:** kullanıma [watchOS 3 giriş](~/ios/watchos/platform/introduction-to-watchos3/index.md) en son özelliklere genel bakış.

## <a name="about-watchos"></a>WatchOS hakkında

WatchOS uygulama çözüm 3 proje sahiptir:

- **Uzantı izleme** – izleme uygulama kodunu içeren bir projesi.
- **Uygulama izleme** – kullanıcı arabirimi film şeridi ve kaynaklar içerir.
- **iOS üst uygulaması** – bu uygulama normal iPhone uygulamadır. Uzantı ve izleme uygulama teslimi için kullanıcının izleme iPhone uygulamada paketlenmiştir.

WatchOS 1 uygulamalardaki uzantısı'nda kod iPhone üzerinde çalışır – Apple Watch etkili bir şekilde bir dış görüntülenir. watchOS 2 ve 3 uygulamalar tamamen Apple Watch üzerinde çalıştırın. Bu fark, aşağıdaki çizimde gösterilmiştir:

[ ![](intro-to-watchos-images/arch-sml.png "Bu diyagramda watchOS 1 ve 2 (ve büyük) watchOS arasındaki fark gösterilir")](intro-to-watchos-images/arch.png#lightbox)

WatchOS'ın hangi sürümü hedeflenen bağımsız olarak, Mac'ın çözüm Pad için Visual Studio'da eksiksiz bir çözüm şunun gibi görünür:

[![](intro-to-watchos-images/projectstructure-sml.png "Çözüm paneli")](intro-to-watchos-images/projectstructure.png#lightbox)

*Üst uygulama* bir watchOS normal iOS uygulaması çözümüdür. Bu yalnızca bir görünür olan çözümü projedir **Telefonda**. Bu uygulama için kullanım örnekleri öğreticileri, yönetim ekranları ve orta katman filtreleme, cacheing, vb. içerir. Ancak, yüklemek ve izleme uygulama/uzantısı olmadan çalıştırmak kullanıcı için olası **hiç** üst uygulama açmadan böylece tek seferlik başlatma veya Yönetim için çalıştırmak için üst uygulama gerekiyorsa gerekir, izleme programı kullanıcıya bildirmek için uygulama/uzantısı.

Üst uygulama izleme uygulama ve uzantı sunar ancak bunlar farklı sanal çalıştırın.

Paylaşılan uygulama grubu üzerinden veya statik işlev aracılığıyla verileri paylaşabilmeleri 1 watchOS üzerinde `WKInterfaceController.OpenParentApplication`, tetiklemek `UIApplicationDelegate.HandleWatchKitExtensionRequest` üst uygulamanızın yöntemi `AppDelegate` (bkz [üst uygulama ile birlikte çalışma](~/ios/watchos/app-fundamentals/parent-app.md)).

Gözcü bağlantı framework üst uygulamayla iletişim kurmak için kullanılan watchOS 2 veya sonraki sürümü üzerinde kullanarak `WCSession` sınıfı.

## <a name="application-lifecycle"></a>Uygulama yaşam döngüsü

Öğesinin bir alt kümesi izleme uzantısı'nda `WKInterfaceController` sınıfı, her film şeridi Sahne için oluşturulur.

Bunlar `WKInterfaceController` sınıflar benzer `UIViewController` iOS programlama nesneleri ancak aynı düzeyde görünümüne erişime sahip değil.
Örneği için dinamik olarak denetimlere ekleyemez veya UI yeniden yapılandırabilirsiniz.
, Ancak gizleme ve denetimleri ortaya ve bazı denetimler ile bunların boyutu, saydamlık ve görünüm seçeneklerini değiştirin.

Yaşam döngüsü bir `WKInterfaceController` nesnesi aşağıdaki çağrıları içerir:

- [Uyanık](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/) : Bu yöntem, başlatma çoğunu gerçekleştirmeniz gerekir.
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : kısa bir süre içinde izleme uygulama kullanıcıya görünür önce çağrılır. Şu anda son başlatılmasını gerçekleştirmek, başlangıç animasyonları, vb. için bu yöntemi kullanın.
- Bu noktada, izleme uygulama görünür ve kullanıcı girişi ve izleme uygulamanın görünen uygulama mantığınızın başına güncelleştirme yanıtlama uzantısı başlar.
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/) sonra izleme uygulama, kullanıcı tarafından kapatıldığında, bu yöntem çağrılır. Bu yöntem döndükten sonra kullanıcı arabirimi denetimlerini zamana kadar değiştirilemez `WillActivate` olarak adlandırılır. İPhone bağlantı bozuk ise, bu yöntem aynı zamanda çağrılır.
- Uzantı devre dışı bırakıldı sonra programınıza erişilemiyor. Zaman uyumsuz işlevleri bekleyen **almayacak** çağrılabilir. Gözcü Seti uzantıları arka plan modları işlemleri kullanamazsınız. Program kullanıcı tarafından yeniden ancak uygulama işletim sistemi tarafından sonlandırıldı değil, çağrılan ilk yöntemi olacaktır `WillActivate`.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "Uygulama yaşam döngüsüne genel bakış")

## <a name="types-of-user-interface"></a>Kullanıcı arabirim türü

Kullanıcı izleme uygulamanızla olabilir etkileşim üç tür vardır.
Tüm özel alt sınıflarını kullanarak programlanmış `WKInterfaceController`, daha önce ele alınan yaşam döngüsü dizisi Evrensel uygular (bildirimleri ile alt sınıflarını programlanmış `WKUserNotificationController`, kendisi alt sınıfıdır `WKInterfaceController`):

### <a name="normal-interaction"></a>Normal etkileşimi

İzleme uygulaması/uzantısı etkileşim çoğunluğu ile alt sınıflarını olacağından `WKInterfaceController` izleme uygulamanızın görünümlerde karşılık gelecek şekilde yazma **Interface.storyboard**. Bu ayrıntılı olarak ele alınmıştır [yükleme](~/ios/watchos/get-started/installation.md) ve [Başlarken](~/ios/watchos/get-started/index.md) makaleleri.
Aşağıdaki resimde bir kısmı gösterilmiştir [izleme Seti katalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) örnek'ın Film şeridi. Burada gösterilen her Sahne için karşılık gelen bir özel olduğundan `WKInterfaceController` (`LabelDetailController`, `ButtonDetailController`, `SwitchDetailController`, vs.) uzantısı projesinde.

![](intro-to-watchos-images/scenes.png "Normal etkileşim örnekleri")

### <a name="notifications"></a>Bildirimler

[Bildirimleri](~/ios/watchos/platform/notifications.md) önemli bir kullanım durumu için Apple Watch şunlardır. Hem yerel hem de uzak bildirimler desteklenir. Bildirimleri ile etkileşim kısa ve uzun Görünüm adlı iki aşamada gerçekleşir.

Kısa görünen kısaca görüntülenir ve izleme uygulama simgesi, adını ve Başlık Göster (ile belirtildiği gibi `WKInterfaceController.SetTitle`).

Bir sistem tarafından sağlanan uzun konum birleştirir **Kanat** alan ve kapatma düğmesini özel film şeridi tabanlı içeriğe sahip.

`WKUserNotificationInterfaceController` genişletir `WKInterfaceController` yöntemleriyle `DidReceiveLocalNotification` ve `DidReceiveRemoteNotification`.
Bildirim olaylarına tepki vermek için bu yöntemleri geçersiz kılın.

Bildirim kullanıcı Arabirimi tasarımı ile ilgili daha fazla bilgi için başvurmak [Apple Watch İnsan Arabirimi yönergelerine](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "Örnek bildirimleri")

## <a name="screen-sizes"></a>Ekran boyutlarına

Apple Watch iki yüz boyutları vardır: 38mm, 42mm, hem 5:4 ekran oranı ve Retina görüntüleme. Kullanışlı boyutlarının şunlardır:

- 38 mm: 136 x 170 mantıksal piksel (272 x 340 fiziksel piksel cinsinden)
- 42 mm: 156 x 195 mantıksal piksel (312 x 390 fiziksel piksel cinsinden).

Kullanım `WKInterfaceDevice.ScreenBounds` hangi ekranda izleme uygulamanızı çalıştığını belirlemek için.

Genellikle, metin ve düzeni tasarımınızı daha kısıtlı 38 mm ekranla geliştirmek ve sonra ölçeği daha kolay olur.
Daha büyük ortamı ile başlatırsanız, ölçekleme çirkin çakışma veya metin kesilmesi neden olabilir.

Daha fazla bilgi edinin [ekran boyutlarıyla çalışma](~/ios/watchos/app-fundamentals/screen-sizes.md).


## <a name="limitations-of-watchos"></a>WatchOS sınırlamaları

WatchOS watchOS uygulamaları geliştirirken dikkat edilmesi gereken bazı sınırlamalar vardır:

- Apple Watch aygıtları depolama sınırlı - büyük dosyaları (ör. yüklemeden önce kullanılabilir alan unutmayın Ses veya film dosyaları).

- Birçok watchOS [denetimleri](~/ios/watchos/user-interface/index.md) analogues Uıkit içinde vardır, ancak farklı sınıflar (`WKInterfaceButton` yerine `UIButton`, `WKInterfaceSwitch` için `UISwitch`, vs.) ve bunların Uıkit karşılaştırıldığında yöntemleri sınırlı sayıda eşdeğerleri. Ayrıca, watchOS gibi bazı denetimler vardır `WKInterfaceDate` (bir tarih ve saati görüntülemek için) bu Uıkit sahip değildir.

  - Bildirimleri izleme yalnızca için veya yalnızca (ne tür bir kullanıcının yönlendirme üzerinden denetim Apple tarafından açıklandı değil) iPhone yönlendiremezsiniz.

Başka bir bilinen sınırlamalar / sık sorulan sorular:

- 3. taraf özel izleme yüzeyleri Apple izin vermez.

- İTunes bağlı telefonda denetlemek izleme izin API'leri özeldir.


## <a name="further-reading"></a>Daha Fazla Bilgi

Apple belgelerine bakın:

* [Gözcü Seti için geliştirme](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [Kit programlama kılavuzu izleyin](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Apple Watch İnsan Arabirimi yönergelerine](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>İlgili bağlantılar

- [watchOS 3 katalog (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [watchOS 1 katalog (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Kurulum ve yükleme](~/ios/watchos/get-started/installation.md)
- [İlk izleme uygulama video](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple için izleme paketi Kılavuzu geliştirme](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Apple'nın WatchKit ipuçları](https://developer.apple.com/watchkit/tips/)
- [watchOS 3’e Giriş](~/ios/watchos/platform/introduction-to-watchos3/index.md)
