---
title: İOS 9 giriş
description: Bu makalede Xamarin.iOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve iOS 9 kullanılabilir özellikler sunar.
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 10ed9154b92e6f13dd71f83cf4fed47585dc795f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30781278"
---
# <a name="introduction-to-ios-9"></a>İOS 9 giriş

_Bu makalede Xamarin.iOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve iOS 9 kullanılabilir özellikler sunar._

![](images/ios9-sml.png "İOS 9 logosu")

Apple, iOS 9 var olan özellikleri birçok geliştirmelerin yanı sıra, birkaç yeni API'ler ve Hizmetleri ekledi.

## <a name="3d-touch"></a>3D Touch

İOS 9 iPhone 6s ve iPhone 6s yeni 3B dokunma Ayrıca, iOS uygulamalarınızı baskısı hassas hareketleri ekler. 3B ile dokunma, iPhone uygulama artık yalnızca kullanıcı cihazın ekran değecek söyleyin yapabiliyor Ayrıca kullanıcı exerting ne kadar baskısı mantıklı ve farklı baskısı düzeylere yanıt.

3B Touch uygulamanız için aşağıdaki özellikleri sağlar:

- **Baskı duyarlılık** - uygulamalar şimdi ölçmek nasıl sabit veya ışık kullanıcı bu bilgileri ekran ve Al avantajlarından temas. Örneğin, bir boyama uygulama kalın veya kullanıcının ekran nasıl sabit temas thinner göre çizgi yapabilir.
- **Gözat ve Pop** -uygulamanız, geçerli bağlam dışında gitmek zorunda kalmadan kendi verilerle etkileşimli kullanıcı artık olanak tanır. Ekranda sabit basarak yapabilir *Peek* (bir ileti Önizleme gibi) ilginizi oldukları öğesindeki. Daha zor basarak yapabilir *Pop* öğenin içine.
- **Hızlı Eylemler** -düşünüyorsanız, hızlı gibi eylemleri, kullanıcı bir masaüstü uygulamasının bir öğeyi tıklattığında açılan bağlamsal menüleri. Hızlı Eylemler kullanarak, ortak, hızlı ve kolay erişim kısayolların işlevlere uygulamanızda iOS cihazında giriş ekranı simgesinden ekleyebilirsiniz.

Daha fazla bilgi için lütfen bkz bizim [3B dokunma giriş](~/ios/platform/3d-touch.md) Kılavuzu.

## <a name="app-transport-security"></a>Uygulama taşıma güvenliği

Yeni iOS 9, uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulamanız arasındaki güvenli bağlantılar zorlar. ATS tüm Internet iletişimlerini güvenli bağlantı en iyi uygulamalar için uygun böylece uygulamanız veya bunu kullanan bir kitaplığı ile doğrudan hassas bilgilerin yanlışlıkla açığa önleme sağlar.

Varsayılan olarak tüm bağlantılar kullanarak iOS 9 ve OS X 10.11 sürümünü (El Capitan) için oluşturulan uygulamaların ATS etkin olduğundan [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) veya [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) tabi olacaktır ATS güvenlik gereksinimleri. Bağlantılarınızı bu gereksinimi karşılamıyorsa, bir özel durum ile başarısız olur.

ATS hakkında daha fazla bilgi için lütfen bkz bizim [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md) Kılavuzu.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>İPad için çoklu

İOS 9 Apple belirli iPad donanım üzerinde aynı anda çalışan iki uygulamalar için çoklu destek eklemiştir. Sonuç olarak, Xamarin.iOS uygulamaları artık herhangi bir anda yalnızca uygulamayı oldukları veya tam ekran veya cihazın kaynaklara erişime sahip oldukları kabul edilebilir.

İPad için çoklu aşağıdaki özellikler desteklenmez:

- **Slayt üzerinden** -kullanıcının bir slayt şu anda çalışan ana uygulama yaklaşık % 25 kapsayan paneli (ya da dil yöne bağlı ekran sağ veya sol tarafındaki) kullanıma ikinci bir iOS uygulaması bir geçici olarak çalıştırmasına izin verir. Slayt üzerinde yalnızca bir iPad Pro, iPad hava, iPad hava 2, iPad Mini 2, iPad Mini 3 ya da iPad Mini 4 kullanılabilir.
- **Bölünmüş Görünüm** -desteklenen iPad donanım (iPad hava 2, iPad Mini 4 ve iPad Pro yalnızca), kullanıcı ikinci bir uygulama seçin ve çalıştırın yan yana bölünmüş ekran modu şu anda çalışan uygulamaya sahip. Kullanıcı, her uygulama kapladığı ana ekran yüzdesini denetleyebilir.
- **Resim içinde resim** - video içeriği kayıttan yürütmek, video artık şu anda iOS cihaz üzerinde çalışan diğer uygulamalar üzerinden gezinen taşınabilir ve yeniden boyutlandırılabilir penceresinde yürütülebilir uygulamalar için. Kullanıcı boyutunu ve konumunu bu penceresinin üzerinde tam denetime sahiptir. Resim içinde resim, yalnızca bir iPad Pro, iPad hava, iPad hava 2, iPad Mini 2, iPad Mini 3 ya da iPad Mini 4 kullanılabilir.

İOS 9 yeni görevli yeteneklerini hakkında daha fazla bilgi için lütfen bkz bizim [iPad için çoklu](~/ios/platform/multitasking.md) Kılavuzu.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>Yeni kişiler ve kişi UI çerçeveleri

İOS 9 başlanmasıyla, iki yeni çerçeveler Apple yayımladı [kişiler](https://developer.xamarin.com/api/namespace/Contacts/) ve [ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/), var olan adres defteri değiştirin ve adres defteri UI çerçeveleri 8 ve önceki iOS tarafından kullanılır.

Bu yeni, nesne yönelimli çerçeveleri aşağıdakileri sağlar:

- **Kişiler** – kullanıcının kişi bilgilerine erişim Xamarin.iOS sağlar. Çoğu uygulamalar yalnızca salt okunur erişim gerektirdiğinden bu framework iş parçacığı güvenli, salt okunur erişim için optimize edilmiştir.
- **ContactsUI** – sağlar Xamarin.iOS UI öğelerini görüntülemek için düzenleme, seçin ve iOS cihazlarda kişileri oluşturun.

Daha fazla bilgi için bizim [kişiler ve kişi UI](~/ios/platform/contacts.md) belgeleri.


## <a name="new-search-apis"></a>Yeni arama API'leri

Arama iOS 9 Xamarin.iOS uygulamanızı içinde bilgilere erişmek için yeni yollar sağlamak için genişletilmiştir. Yeni arama API'leri kullanarak, uygulamanızın içerik Spotlight ve Safari arama sonuçları, iletim ve Siri anımsatıcıları ve öneriler aracılığıyla aranabilir yapabilirsiniz. Bu etkinlikler ve uygulamanızda ayrıntılı bilgi için kullanıcıların hızlı erişim sağlar.

Ayrıca, yeni arama API'leri, önceki arama uygulaması deneyimi olmadan, uygulamanızda arama tümleştirmek kolaylaştırır. Bu nedenle, Apple genellikle bir iOS 9 uygulamanın içeriği Evrensel aranabilir uygulama arama'yı kullanarak yapmak için birkaç saat sürer talepleri.

Daha fazla bilgi için lütfen bkz bizim [arama geliştirmeleri](~/ios/platform/search/index.md) belgeleri.

## <a name="new-stack-view"></a>Yeni bir yığın görünümü

Yığın Görünümü denetimi ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/)) dinamik olarak iOS cihaz yönlendirmesini ve ekran boyutuna yanıt otomatik düzeni ve boyutu sınıfları (yatay veya dikey) subviews yığınını yönetmek için gücünü yararlanır.

Yığın Görünümü denetimi kullanarak, bir kullanıcı arabirimi önemli ölçüde azalır düzene iş miktarı gereklidir. Bir yığın görünüme bağlı tüm subviews düzenini yönetilen eksen, dağıtım, hizalama ve aralığı gibi tanımlanmış Geliştirici özellikleri göre otomatik olarak.

Daha fazla bilgi için lütfen bkz bizim [yığın görünümü giriş](~/ios/user-interface/controls/uistackview.md) belgeleri.


## <a name="collection-view-changes"></a>Koleksiyon görünümü değişiklikleri

İOS 9, koleksiyon görünümü içinde ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) destekler şimdi sürükleyin kutunun dışında öğelerinin yeni bir varsayılan hareketi tanıyıcı ve birkaç yeni destek yöntemleri ekleyerek yeniden sıralama.

Bu yeni yöntemleri kullanarak kolayca koleksiyon görünümünüzde Sürükle sipariş uygulamak ve sipariş işleminin herhangi aşaması sırasında öğeleri görünümünü özelleştirme seçeneğiniz vardır.

İOS 9 için koleksiyon görünümü değişiklikler hakkında daha fazla bilgi için lütfen bkz bizim [koleksiyon görünümü değişikliklerini](~/ios/user-interface/controls/uicollectionview.md) Kılavuzu.

## <a name="game-enhancements"></a>Oyun geliştirmeleri

İOS 9 Apple, oyun grafikleri ve ses Xamarin.iOS uygulamanızı uygulamak kolaylaştıran oyun API'ları teknolojik yenilikleri yapmıştır. Bunlar, her iki üst düzey çerçeveler ve geliştirilmiş hızı ve alt düzey geliştirmeleri ile grafik yeteneklerini iOS cihazın GPU gücünü gücünden aracılığıyla geliştirme kolaylığı içerir.

Bu seçenek, GameplayKit, ReplayKit, Model g/ç, MetalKit ve Metal performans gölgelendiriciler birlikte Metal, SceneKit ve SpriteKit yeni, Gelişmiş özellikleri içerir.

Daha fazla bilgi için lütfen bkz bizim [oyun geliştirmeleri](~/ios/platform/gaming/index.md) belgeleri.

## <a name="homekit-framework-changes"></a>HomeKit Framework değişiklikleri

[HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) iOS 8, sunulan framework Kurulum ve bir Xamarin.iOS uygulaması'ndan (otomatik ışık, kilit ve Garaj kapı ayrıcalığına gibi) çeşitli etkin HomeKit Donatılar denetleme olanağı sağlar. Kolay kurulum ve yapılandırma olmaya ek olarak, HomeKit Donatılar sözlü Siri komutları denetlenebilir.

İOS 9'da, Apple kurulumu daha kolay hale, desteklenen ve (örneğin, bir donatıyı iCloud aracılığıyla uzaktan denetleme) daha fazla aksesuar etkileşimleri sağlanan Donatılar türlerini genişletilmiş.

Daha fazla bilgi için bizim [HomeKit giriş](~/ios/platform/homekit.md), [HomeKitIntro iOS örnek uygulaması](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) ve Apple'nın [HomeKit](https://developer.apple.com/homekit/) belgeleri.

## <a name="handoff-framework-changes"></a>İletim Framework değişiklikleri

İletim (sürekliliği olarak da bilinir) Apple, iOS 8 ve OS X Yosemite (10.10) tarafından kullanıcının cihazlarını (iOS veya Mac) birinde bir etkinlik başlangıç ve aynı etkinliğin başka cihazlarını (kullanıcının iClou tarafından tanımlandığı gibi çalışmaya devam etmek için bir yol olarak sunulmuştur d hesap için).

İletim Gelişmiş arama özellikleri iOS 9 ayrıca yeni, desteklemek için genişletilmiştir. Daha fazla bilgi için lütfen bkz bizim [arama geliştirmeleri](~/ios/platform/search/index.md) belgeleri. İletim kullanma hakkında daha fazla bilgi için lütfen bkz bizim [iletimi giriş](~/ios/platform/handoff.md) belgeleri.

## <a name="new-extension-points"></a>Yeni Uzantı Noktaları

İOS 8'de, Apple uzantıları sunulan — işletim sistemi tarafından standart bağlamlarda gibi bildirim merkezi kullanıcı klavye istediğinde veya fotoğraf düzenlerken sunulan kitaplıkları.

İOS 9 Apple uzantısı desteği birkaç yeni sağlayarak mu genişlettiğini _uzantı noktaları_ kullanım ilkeleri tanımlayın ve içinde belirli bir alanda şu şekilde çalışmak için API'ler sağlar:

- **Yeni ses birim uzantısı noktası** – diğer ses birim ana bilgisayar uygulamalarını (örneğin, GarageBand) içinde kullanmak için ses efektler, müzik Gereçleri, ses oluşturucuları, vb. sağlamak için bu uzantı noktasını kullanın. Bu uzantı noktası da satmak sayesinde _ses birimleri_ (ses eklentiler) App Store'dan.
- **Yeni dizin Bakım Uzantısı noktası** — uygulama verileri bir uygulama denemesine gerek kalmadan yeniden dizin oluşturmaya desteklemek için bu uzantı noktasını kullanın.
- **Yeni ağ uzantı noktaları** (bunlar Apple özel izinler gerektirir):
    - **Uygulama Proxy sağlayıcısı uzantısı** — özel saydam istemci-tarafı ağ proxy uygulamak için bu uzantı noktasını kullanın.
    - **Veri sağlayıcısı filtre / filtre denetim sağlayıcısı uzantısı** -dinamik ağ içerik cihaz üzerinde filtre uygulamak için bu uzantı noktaları kullanın.
    - **Paket tünel sağlayıcısı uzantısı** — bu uzantı Noktaya Tünel Protokolü istemci-tarafı özel bir VPN uygulamak için kullanın.
- **Yeni Safari uzantı noktaları**:
    - **İçerik engelleme uzantısı** — bu uzantı noktası kullanıcının Web'e göz atarken görüntülenmeyecek engellenen içerik listesini tanımlamak için kullanın.
    - **Bağlantılar uzantısı Paylaşılan** — bu uzantı noktası Safari'nın paylaşılan bağlantılar uygulamanızın içeriğin görüntülemeyi etkinleştirmek için kullanın.

Daha fazla bilgi için lütfen bkz bizim [uzantıları giriş](~/ios/platform/extensions.md) ve Apple'nın [uygulama uzantısı Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) belgeleri.

## <a name="keychain-enhancements"></a>Anahtarlık geliştirmeleri

İOS 9'da, Apple Anahtarlık güvenli Enclave ve daha fazla öğe koruma seçenekleri için yeni bir şifreleme anahtar türü şu şekilde sağlamak için geliştirilmiştir:

- Parmak izi veritabanı değiştirildiğinde geçersiz kılan Anahtarlık öğeleri yeni bir Touch ID kısıtlama.
- Touch ID veya geçiş kodu ile erişim denetimi listesi girdileri yalnızca oluşturulmasına izin ver yeni kısıtlamaları.
- Kimlik doğrulama sunucusundan ayrı çağrılacak izin veren yeni bir kimlik doğrulama bağlam `SecItem` çağrıları.
- Denetim listesi entropi (uygulama parolası seçeneğini kullanarak) için uygulama tarafından sağlanan Anahtarlık öğesi şifreleme erişin.
- Oluşturma ve anahtarları güvenli enclave içinde kullanma desteği (aracılığıyla `kSecAttrTokenIDSecureEnclave` özniteliği).

Daha fazla bilgi için lütfen bkz bizim [Touch ID giriş](~/ios/platform/touchid.md) belgeleri.


## <a name="right-to-left-language-support"></a>Sağdan sola dil desteği

İOS 9'da, ters çevrilmiş kullanıcı arabirimi çok daha kolay her zamankinden sağdan sola yazılan diller için tam destek sağlayarak sunan Apple yaptı. Buna aşağıdakiler dahildir:

- Standart [Uıkit](https://developer.xamarin.com/api/namespace/UIKit/) denetimleri otomatik olarak sağdan sola iOS aygıtları yerel ayarlar ve dil ayarlarınızı temel alan çevir.
- [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) sınıfı, belirli bir görünüm ne zaman nasıl görünmelidir tanımlamanıza izin öznitelikleri çevrilmiş sağdan sola sağlar.
- Kullanarak bir görüntü program aracılığıyla Çevir olanağı [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/) özelliği [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/) sınıfı.

Daha fazla bilgi için lütfen Apple'nın bkz [destekleyen sağdan sola dilleri](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) belgeleri.



## <a name="additional-framework-changes"></a>Ek Framework değişiklikleri

Biz yukarıda ele aldıktan önemli değişiklikler ek olarak, değişiklikler ve iyileştirmeler iOS 9 aşağıdakiler de dahil olmak üzere birkaç varolan çerçeveyi Apple yaptı:

- AV Foundation çerçevesi
- AVKit Framework
- CloudKit Framework
- Foundation çerçevesi
- İletim Framework
- HealthKit Framework
- HomeKit Framework
- Yerel kimlik doğrulama çerçevesi
- MapKit Framework
- PassKit Framework
- Safari Hizmetleri çerçevesi
- Uıkit Framework

Daha fazla bilgi için lütfen bkz bizim [ek iOS 9 Framework değişiklikleri](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) belgeleri.

## <a name="deprecated-apis-and-functions"></a>Kullanım dışı API'ları ve İşlevler

Apple aşağıdaki API'leri ve iOS 9 işlevlerde kullanımdan kaldırmıştır:

- **Adres Defteri & Adres Defteri UI** -bu API'leri ile iletişime geçin ve ilgili kişi UI çerçeveleri tarafından yerini almıştır. Daha fazla bilgi için bizim [kişiler ve kişi UI](~/ios/platform/contacts.md) belgeleri.
- **CBCentralManager** - `RetrievePeripherals` ve `RetrieveConnectedPeripherals` yöntemlerinin `CBCentralManager` sınıfı iOS 9 kaldırıldı. Bu yöntemleri çağrılırken bir uygulama bir donatıyı eşleştirme zaman çökmesine veya uygulama başlatma neden olur.
- **FetchAllChanges** - `FetchAllChanges` , `CKFetchRecordChangesOperation` sınıfı amorti ve iOS 9 kaldırılacak.
- **Media Player** -Media Player framework iOS 9 bırakılmıştır. Bunun yerine AVKit veya AV Foundation API'leri kullanın.

Apple'nın belirli API deprecations tam bir listesi için bkz: [iOS 9.0 API Diffs](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) belgeleri.

## <a name="ios-9-sample-apps"></a>iOS 9 örnek uygulamaları

Bazı sahibiz [iOS 9 özgü örnekleri](https://developer.xamarin.com/samples/ios/iOS9/) başlamak için:

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

Ayrıca bu örnekleri (yardımcı Mac OS X sürümlerinin yakında!) iOS bölümlerini denetleyin:

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [3B dokunmatik giriş](~/ios/platform/3d-touch.md)
- [Uygulama Aktarım Güvenliği](~/ios/app-fundamentals/ats.md)
- [iPad için Çoklu Görev Gerçekleştirme](~/ios/platform/multitasking.md)
- [Kişiler ve kişileri kullanıcı Arabirimi](~/ios/platform/contacts.md)
- [Yeni arama API'leri](~/ios/platform/search/index.md)
- [Görünüm yığın giriş](~/ios/user-interface/controls/uistackview.md)
- [Koleksiyon görünümü değişiklikleri](~/ios/user-interface/controls/uicollectionview.md)
- [Oyun geliştirmeleri](~/ios/platform/gaming/index.md)
- [HomeKit giriş](~/ios/platform/homekit.md)
- [İletim giriş](~/ios/platform/handoff.md)
- [Ek iOS 9 Çerçeve Değişiklikleri](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Sorun giderme](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [Yeni iOS 9.0 nedir](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Xamarin.iOS uygulamaları iOS9 (video) güncelleştiriliyor](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
