---
title: "İOS 8 giriş"
description: "İOS 8 Apple yeni çerçeveler ve toplayan ve geliştiricilerin zevk aldığı için API'ler sayısız sağlamıştır. Bu kılavuzda Biz bu yeni API'leri getirir ve iOS 8 geliştiricilere ve kullanıcılara nasıl yararlanabilir bakın."
ms.topic: article
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 8a4fabd5cc63434950f4646336b06676f6eb915b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-ios-8"></a>İOS 8 giriş

_İOS 8 Apple yeni çerçeveler ve toplayan ve geliştiricilerin zevk aldığı için API'ler sayısız sağlamıştır. Bu kılavuzda Biz bu yeni API'leri getirir ve iOS 8 geliştiricilere ve kullanıcılara nasıl yararlanabilir bakın._

iOS 7 hangi kullanıcıların ve geliştiricilerin sağ ilk iPhone OS beklenir gelen tüm iOS kullanıcı arabirimi görsel olarak değiştirildi. IOS 8 Bu birçok çerçeveleri geliştiriciler için kullanıcıların kendi iPhone düz oldukları yaşam süreleri neredeyse her açıdan denetlemesine izin veren sağlayarak devam eder. Örneğin sistem durumunu ve uygunluk ile çözümlenebilir *HealthKit*, şifreleri biyometrik kimlik doğrulamasını kullanarak obsolescent *LocalAuthentication*, *uygulama uzantıları*3 taraf uygulamalar arasında bir iletişim kanalı açın ve *HomeKit* gelecekteki bir ev evinizde kapatma olanağı sağlar. 

İOS 7 kullanıcılar delighting hakkında olduysa, iOS 8 tasty bu yeni araçları tam bir dizi delighting geliştiricilere odaklanmıştır. 

Bu kılavuz, yeni API'leri Xamarin.iOS geliştiriciler için tanıtır.  

Bu belgenin sonuna ayrıntılı iOS 8 ' de kullanım birkaç API'lerini de vardır.

## <a name="requirements"></a>Gereksinimler

Mac için Visual Studio iOS 8 uygulamaları oluşturmak için aşağıdakiler gereklidir:

- **Xcode 7 ve iOS 8 veya daha yeni** – Apple'nın en son Xcode ve iOS API gereken yüklenmeli ve geliştirici bilgisayarda yapılandırılmış.
- **Mac için Visual Studio** – Mac için Visual Studio en son sürümünü yüklenmeli ve kullanıcı cihazda yapılandırılmalıdır.
- **iOS 8 aygıtı veya Simulator** – iOS 8 test etmek için en son sürümünü çalıştıran bir iOS cihazı.

## <a name="home-and-leisure"></a>Giriş ve boş

iOS 8 sıkıca düz HomeKit ve HealthKit kullanımı ile ev Kalp içine Apple ve iOS cihaz planlayın Yardım. Bu bölümde, bu yeni çerçeveler nasıl her iki çalışma ve Xamarin.iOS uygulamanıza bunların nasıl tümleştirilebilir ele alacağız.

## <a name="homekit"></a>HomeKit

İPhone Cihazınızı, gereçlerini denetleme teknolojinin yeni bir uygulama değildir; bağlı giriş ürününe bir iOS uygulaması aracılığıyla denetlenebilir. Ancak HomeKit artık bunu bir adım ileri ev Otomasyon cihazlar için ortak bir protokolle yükseltme ve ortak bir API belirli üreticilerine, iHome gibi Philips ve Honeywell kullanılabilir hale getirme alır. Kullanıcıya bu neredeyse her açıdan kendi evden sorunsuz bir şekilde denetleyebilirsiniz anlamına gelir bir uygulama içinde. Bunları Philips ton ampul veya iç içe alarm kullandıkları bilmek önemli değildir. Kullanıcılar aynı zamanda çok sayıda akıllı ev süreçleriyle birlikte "Planda" zincir.

HomeKit ile üçüncü taraf uygulamalar ve Siri Donatılar bulmak ve bunları kendi kişisel ev yapılandırma veritabanına eklemek, düzenleyebilir ve bu verilere hareket ve Donatılar ve bir eylemi gerçekleştirmek için kendi Hizmetleri ile iletişim.

### <a name="configuration"></a>Yapılandırma

Aşağıdaki diyagram HomeKit Donatılar yapılandırmasını temel hiyerarşisini gösterir:

![](introduction-to-ios8-images/image1.png "Bu diyagramda HomeKit Donatılar yapılandırmasını temel hiyerarşisini gösterir")
 
HomeKit ile çalışmaya başlamak için geliştiricilerin kendi sağlama profili seçili HomeKit hizmet sahip olduğundan emin olmak gerekir. Apple için Xcode bir HomeKit simulator eklentisi geliştiricilere sağlanan. Bu bulunabilir [Apple Geliştirici Merkezi](https://developer.apple.com/downloads/index.action)altında `Hardware IO Tools for Xcode`. 

Daha fazla bilgi için lütfen bkz bizim [HomeKit](~/ios/platform/homekit.md) Kılavuzu.

## <a name="healthkit"></a>HealthKit

HealthKit Merkezi, Eşgüdümlü ve güvenli bir veri deposu için sistem durumu ile ilgili bilgiler sağlar iOS 8'de sunulan bir çerçevedir. İşletim sistemi, gizlilik ve güvenlik durumu bilgisi ve sistem durumu uygulama, kullanıcı için bir Pano sağlar. Kullanıcının izni olan uygulamalar okuma ve çok çeşitli sistem durumu bilgisi yazma.

Bu Xamarin.iOS uygulamanızı kullanma hakkında daha fazla bilgi için bkz [HealthKit giriş](~/ios/platform/healthkit.md) Kılavuzu.

## <a name="extending-iphone-functionality"></a>İPhone işlevselliği genişletme
İOS8 ile geliştiriciler kimlerin, uygulama ve artan yetenek üçüncü taraf uygulamalar arasında daha açık iletişim için kullanabileceğini üzerinde daha fazla denetim verilir. Uygulama uzantıları ve belge Seçici gibi özellikleri, uygulamaları Apple'nın ekosistemi nasıl kullanılabileceğini olanaklar dünyası açın.

### <a name="app-extensions"></a>Uygulama uzantıları
Oversimplify için uygulama uzantıları, üçüncü taraf uygulamaların birbirleriyle iletişim kurmak bir yoldur. Yüksek güvenlik standartları korumak ve korumalı uygulamalar bütünlüğünü tut için bu iletişim doğrudan uygulamalar arasında gerçekleşmez. Bunun yerine, onu tarafından gerçekleştirilir bir *uzantısı* ortasında.

Uygulama Uzantı oluşturmanın ilk adımı doğru uzantı noktası tanımlamaktır — bu davranışı ve doğru API'leri kullanılabilirliğini sağlamak için önemlidir. Mac için Visual Studio'da Uygulama Uzantı oluşturmak için varolan bir uygulamaya çözümünüz için yeni bir proje ekleyerek ekleyin.

İçinde **yeni proje** iletişim gidin **C#** > **iOS** > **Unified API**  >   **Uzantıları**aşağıdaki ekran görüntüsünde gösterildiği gibi:

![](introduction-to-ios8-images/image2.png "Yeni bir uzantısı oluşturma")
 
Yeni Proje iletişim kutusu uygulama uzantıları oluşturmak için yedi yeni proje şablonları sağlar ve aşağıda ele alınmıştır. İOS, belge Seçici gibi diğer yeni API'leri uzantılarının çoğu ile ilgili dikkat edin:

- **Eylem** – bu kullanıcılara belirli görevleri gerçekleştiren izin vererek benzersiz özel eylem düğmeleri oluşturmak geliştiriciler sağlar
- **Özel klavye** – Bu, kendi özel bir ekleyerek Apple klavyeler yerleşik aralığına eklemek için geliştiricilere sağlar. Popüler klavye Swype bu iOS kendi klavye getirmek için kullanır.
- **Belge Seçici** – bu kullanıcıların uygulamanın sandbox dışındaki dosyalara erişmesini sağlayan bir belge Seçici görünümü denetleyicisi içerir.
- **Belge Seçici dosya sağlayıcısı** – bu belge seçicisini kullanarak dosyaları için güvenli depolama sağlar.
- **Fotoğraf düzenleme** – bu filtreleri genişletir ve düzenleme zaten kullanıcılara daha fazla denetim ve daha fazla seçenek kendi fotoğraf düzenlerken vermek için fotoğraf uygulamada Apple tarafından sağlanan araçları.
- **Bugün** – bu uygulamaları bildirim merkezi bugün bölümünde pencere öğeleri görüntüleme olanağı sağlar.

Xamarin içinde uygulama uzantıları kullanma hakkında daha fazla bilgi için başvurmak [uygulama uzantıları giriş](~/ios/platform/extensions.md) Kılavuzu.

### <a name="touch-id"></a>Touch ID

Touch ID iOS 7 kullanıcı kimlik doğrulaması için bir araç tanıtılmıştır — bir geçiş kodu benzer. Ancak, cihazın kilidini sonra uygulama mağazası kullanarak kullanarak iTunes ve iCloud anahtarlık yalnızca kimlik doğrulaması için sınırlı 

Şimdi yerel kimlik doğrulaması API'sini kullanarak iOS 8 uygulamalarında kimlik doğrulama mekanizması olarak Touch ID kullanmanın iki yolu vardır. Şu anda yerel kimlik doğrulaması uzaktan kimlik doğrulaması için kullanmak mümkün değil.

İlk olarak, yeni Anahtarlık erişim denetim listeleri (ACL'ler) kullanarak mevcut Anahtarlık Hizmetleri yardımcı olur. Anahtarlık veri kullanıcılar parmak izi başarılı kimlik doğrulaması ile kilidi açılabilir.

İkincisi, LocalAuthentication uygulamanızı yerel olarak kimlik doğrulaması yapmak için iki yöntem sunar. Geliştiriciler kullanması gereken `CanEvaluatePolicy` cihaz Touch ID kabul edebilen olup olmadığını belirlemek için ve ardından `EvaluatePolicy` kimlik doğrulama işlemini başlatmak için.

Touch ID ve bir Xamarin.iOS uygulamasına tümleştirileceği hakkında bilgi edinmek için daha fazla bilgi için bkz [giriş için Touchıd](~/ios/platform/touchid.md) kılavuzları.

### <a name="document-picker"></a>Belge Seçici

Farklı bir uygulamada oluşturulan, almak ve bunları işlemek ve bunları dışarı aktarmak dosyaları açmaya izin vermek için bir kullanıcıların iCloud sürücüsüyle belge Seçici works yeniden. Bu, sezgisel bir iş akışı ve bu nedenle çok daha iyi bir deneyim, kullanıcılar için oluşturur. iCloud eşitleniyor alır bunu bir adım daha fazla — bir uygulamada yapılan değişiklikler ayrıca olması yansıtacak tutarlı bir şekilde tüm cihazlarınızda.

Daha fazla derinliği belge seçicide hakkında bilgi edinin ve bir Xamarin.iOS uygulamasına tümleştirileceği hakkında bilgi edinmek için başvurmalıdır [belge Seçici giriş](~/ios/platform/document-picker.md) Kılavuzu.

### <a name="handoff"></a>İletimi

Daha büyük sürekliliği özelliğinin bir parçası olan iletimi OS X ve iOS tümleştirme doğru bir adım ileri alır. Platformlar arası Airdrop'a, iPhone çağrıları, iPad ve Mac ve iPhone Cihazınızı internet paylaşımı içinde geliştirmeleri SMS alma özelliği içerir.

İletim Yosemite ve iOS 8 ile çalışır ve kullanmak istediğiniz tüm farklı cihazlara günlüğe kaydedilmesini bir iCloud hesabıyla gerektirir. Safari, iWork, haritalar, takvimler ve kişiler de dahil olmak üzere önceden yüklenmiş Apple uygulamaları ile çalışması gerekir.

Daha fazla bilgi için lütfen bkz bizim [Handoff'a](~/ios/platform/handoff.md) Kılavuzu.

## <a name="unified-storyboards"></a>Birleşik film şeritleri
iOS 8 içeren yeni bir kullanıcı arabirimi oluşturmak için mekanizmasının kullanılması daha basit — birleşik film şeridi. Farklı donanım ekran boyutlarına tümünün karşılamak üzere tek film şeridi, hızlı ve esnek görünümleri true "kez tasarlama, birçok kullanma" stilde oluşturulabilir.

Geliştiriciler iOS8 önce kullanılan `UIInterfaceOrientation` dikey ve yatay modlar arasında ayrım yapmak için ve `UIInterfaceIdiom` iOS cihazlar arasında ayrım yapmak için. İOS8 içinde artık ayrı film şeritleri iPhone ve iPad cihazları için oluşturmak için gerekli değildir — yönünü ve cihaz belirlenir kullanarak *boyutu sınıfları*.

Her cihazın bir boyut sınıfı dikey ve yatay eksen tarafından tanımlanır ve iOS 8 boyutu sınıflarda iki tür vardır:

- **Normal** -bu büyük ekran boyutu (örneğin, iPad) veya büyük bir boyuta (örneğin, bir UIScrollView izlenim sunan bir araç içindir
- **Compact** -bu küçük aygıtlar (örneğin, iPhone) içindir. Bu boyut cihaz yönünü dikkate alır.

İki kavramları birlikte kullandıysanız, aşağıdaki diyagramda görüldüğü gibi iki farklı yönleri içinde kullanılabilir farklı olası boyutları tanımlar 2 x 2 kılavuz oluşur:

![](introduction-to-ios8-images/image3.png "İçinde farklı yönler kullanılabilir farklı olası boyutları tanımlar 2 x 2 kılavuz gösteren diyagram")
 
Boyutu sınıfları hakkında daha fazla bilgi için başvurmak [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md).

## <a name="photo-kit"></a>Fotoğraf Seti
Fotoğraf Seti sistem resim kitaplığı sorgulamak ve görüntülemek ve içeriğini değiştirmek için özel kullanıcı arabirimleri oluşturmak için uygulamalarına izin veren yeni bir çerçevedir. Birkaç görüntü ve video varlıklar yanı sıra, Albümler ve klasörler gibi varlıklar koleksiyonu temsil eden sınıfları içerir.

Daha fazla bilgi için lütfen bkz bizim [PhotoKit](~/ios/platform/photokit.md) Kılavuzu.

## <a name="games"></a>Oyunlar

<a name="scenekit" />

### <a name="scene-kit"></a>Sahne Seti

Sahne, 3B Sahne grafik 3B grafik çalışmak basitleştirir API'si setidir. OS X 10.8 ilk sunulmuştur ve iOS 8 şimdi geldi. Sahne Seti'nde derinlikli 3B Görselleştirme ve sıradan 3B oyunlar oluşturma OpenGL uzmanlık gerektirmez. Ortak Sahne grafik kavramlar oluşturma, Sahne Seti hemen OpenGL ve çok 3B eklemek bir uygulama için içerik kolaylaşır OpenGL ES karmaşıklığını soyutlar. Ancak, OpenGL uzmanı varsa, Sahne Seti doğrudan OpenGL ile de bağlamadan harika desteğe sahiptir. Ayrıca fizik gibi 3B grafik tamamlayan çeşitli özellikler içerir ve çok iyi çekirdek animasyon, çekirdek görüntü ve hareketli grafik Seti gibi diğer Apple çerçeveleri ile tümleşir.

Daha fazla bilgi için lütfen bkz bizim [SceneKit](~/ios/platform/gaming/scenekit.md) belgeleri.

<a name="spritekit" />

### <a name="sprite-kit"></a>Sprite Kit

Hareketli grafik Seti, 2B oyun framework Apple, iOS 8 ve OS X Yosemite ilginç bazı yeni özellikler vardır. Bunlar Sahne Seti, gölgelendirici desteği, aydınlatma, gölge, kısıtlamalar, normal eşlem oluşturma ve fizik geliştirmeleri ile tümleştirme içerir. Özellikle, yeni fizik özellikleri çok oyun için gerçekçi efektler eklemek kolaylaştırır.

Daha fazla bilgi için lütfen bkz bizim [SpriteKit](~/ios/platform/gaming/spritekit.md) belgeleri.

## <a name="other-changes"></a>Diğer Değişiklikler
Yukarıda açıklanan iOS 8 önemli değişiklikler yanı sıra Apple ek olarak birçok mevcut çerçeveleri güncelleştirdi. Bunlar, aşağıda açıklanmıştır:

- **[Çekirdek görüntü](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)**  – Apple genişletilmiş kendi görüntü işleme altyapısı dikdörtgen bölgeler algılanması için daha iyi destek ekleyerek ve QR kodları içinde görüntüler. Can Bluestein araştırır bu post başlıklı kendi blog [iOS 8 görüntü algılama](http://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>Kullanım dışı API'leri
İOS 8 yapılan tüm iyileştirmeler ile API'leri sayısı kullanım dışı. Bunlardan bazıları aşağıda açıklanmıştır.

- **[Uıapplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  – uzak bildirimler kayıt için kullanılan özellikler ve yöntemler kullanım dışı. RegisterForRemoteNotificationTypes ve enabledRemoteNotificationTypes bunlar.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  – nitelikler ve boyutu sınıfları yerini arabirimi yönlendirmesini tanımlamak için kullanılan özellikleri ve yöntemleri. Başvurmak [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) bunlar kullanma hakkında daha fazla bilgi için.

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  – bu iOS8 UISearchController tarafından değiştirildi.

## <a name="summary"></a>Özet
Bu makaledeki bazı iOS 8 Apple tarafından sunulan yeni özellikler inceledik.



## <a name="related-links"></a>İlgili bağlantılar

- [UIKitEnhancements (örnek)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [Uygulama uzantıları giriş](~/ios/platform/extensions.md)
- [CloudKit giriş](~/ios/data-cloud/intro-to-cloudkit.md)
- [Belge Seçici giriş](~/ios/platform/document-picker.md)
- [HealthKit giriş](~/ios/platform/healthkit.md)
- [Giriş el ile kamera denetimleri](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Touchıd giriş](~/ios/platform/touchid.md)
- [Birleşik film şeritleri giriş](~/ios/user-interface/storyboards/unified-storyboards.md)
