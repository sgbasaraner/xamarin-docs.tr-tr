---
title: "TvOS 9 giriş"
description: "Bu makalede Xamarin.tvOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve tvOS 9 kullanılabilir özellikler sunar."
ms.topic: article
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 55e83658e09bc7e5c12bb3ef3f508497651ec46c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-tvos-9"></a>TvOS 9 giriş

_Bu makalede Xamarin.tvOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve tvOS 9 kullanılabilir özellikler sunar._


Apple yeniden tasarlanan, Dokunmatik özellikli uzak bir özelliğe sahip olan, yeni tvOS işletim sistemi (iOS 9 bağlı olarak) çalıştıran Apple TV donanım 4 nesil yayımladı.

İlk kez, zengin, modern uygulamalar oluşturmak ve bunları Apple TV's yerleşik uygulama Mağazası'ndaki yazma ve serbest bırakma iTunes App kullanarak iOS için uygulama deneyimi için benzer bir işlem aracılığıyla yayın sağlayarak geliştirici, Apple TV platforma tvOS açar Deposu.

İle Xamarin.iOS geliştirme hakkında bilginiz varsa tvOS geçişi oldukça basit bulmanız gerekir. API ve özelliklerin çoğu aynıdır, ancak birçok ortak API'leri (WebKit gibi) kullanılamaz. Ayrıca, çalışma ile Siri uzaktan dayalı dokunmatik iOS cihazları mevcut olmayan bazı tasarım zorluklar oluşturur.

Bu kılavuz, Xamarin.tvOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve tvOS 9 kullanılabilir özellikler giriş verecektir. Apple'nın tvOS hakkında daha fazla bilgi için lütfen bkz [yeni Apple TV için geliştirme](https://developer.apple.com/tvos/) belgeleri.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>Desteklenen ve desteklenmeyen özellikleri

Apple TV çalıştıran tvOS uygulamaları aşağıdaki yetenekleri ve özellikleri desteklenen bulunmaktadır:

 - Uygulama grupları
 - Arka plan modları
 - Veri koruma
 - Oyun Merkezi
 - Oyun denetleyicileri
 - iCloud
 - Uygulama içi satın alma
 - Anahtarlık paylaşımı

Aşağıdaki özellikler ve yetenekler desteklenmez:

 - Apple Pay
 - Uygulama korumalı alan
 - İlişkili etki alanları
 - HealthKit
 - HomeKit
 - Uygulamalar Arası Ses
 - Eşlemeleri
 - Kişisel VPN
 - Anında iletme bildirimleri
 - Wallet
 - Kablosuz Aksesuar Yapılandırması

Lütfen bakın bizim [desteklenen derlemeleri](~/ios/tvos/internals/assemblies.md) ve [desteklenen çerçeveler](~/ios/tvos/internals/frameworks.md) daha fazla bilgi için.

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV donanımı

Yeni Apple TV aşağıdaki donanım belirtimlere sahiptir:

 - 64-bit A8 işlemci
 - 32GB ya da 64GB depolama alanı
 - 2GB RAM
 - 10/100Mbps Ethernet
 - WiFi 802.11a/b/g/n/ac
 - 1080p çözümleme
 - HDMI
 - USB C (ve için bağlantı noktası developer yalnızca tanılama kullanım)
 - Yeni Siri uzaktan veya Apple TV (bölgeye göre) uzaktan

### <a name="siri-remote"></a>Siri uzaktan

Bölgeye göre sağlanan Apple TV uzak bir ya da yapılandırmalarında gelir: Siri uzaktan veya Apple TV uzak.

Siri uzaktan aşağıdaki ülkede şu anda kullanılabilir değil:

 - Avustralya
 - Kanada
 - Fransa
 - Almanya
 - Japonya
 - İspanya
 - Birleşik Krallık
 - Amerika Birleşik Devletleri

Tüm diğer ülkeler Apple TV varsayılan arama ekranında aramak için metin girişi ile getirir arama düğmesiyle Siri düğmesi değiştiren uzak alırsınız:

[![](tvos9-images/remote02.png "Siri uzaktan")](tvos9-images/remote02.png#lightbox)

Daha fazla bilgi için lütfen bkz bizim [Siri uzak ve Bluetooth denetleyicileri](~/ios/tvos/platform/remote-bluetooth.md) belgeleri.

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV sağlama

Yalnızca iOS için geliştirme gibi yeni tvOS, geliştirme ve ekip üyeliğini ve imzalama zaten oluşturulmuş kimliklerle Apple temel dağıtım için uygun sağlama profili gerektirecektir.

Uygun sağlama da iCloud KVS veya CloudKit veri depoları gibi tvOS özelliklerine erişmek gereklidir. Lütfen bakın bizim [kaynakları ve veri depolama](~/ios/tvos/app-fundamentals/resources-data-storage.md) iCloud Xamarin.tvOS uygulamalarınızda destekleme hakkında bilgi için.

Sağlama profilleri oluşturulur ve Xamarin.iOS uygulamalarla çalışma olarak aynı şekilde yüklü. Bu nedenle, lütfen bizim iOS bakın [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) daha fazla ayrıntı için belgeleri.

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV uygulamalar

Yeni Apple TV donanım ve tvOS 9 iki tür uygulamaları destekler: Geleneksel ve istemci-sunucu uygulamaları.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>Geleneksel uygulamalar

Geleneksel uygulamalar Apple TV uygulama Mağazası'ndan satın alınan ve doğrudan aygıta yüklenir. Bu uygulamalar, oyunlar, yardımcı programları veya Xamarin.iOS uygulamaları aynı çerçeveler ve teknikler kullanılarak geliştirilen medya uygulamaları olabilir.

Apple TV uygulamaları 200 MB maksimum boyuta sahip ve isteğe bağlı kaynakları kullanarak içerik gerekenden 2 GB indirebilirsiniz. Lütfen bakın bizim [kaynakları ve veri depolama](~/ios/tvos/app-fundamentals/resources-data-storage.md) daha fazla bilgi için.

Bkz: bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md) Xamarin.tvOS kullanarak tvOS uygulamaları geliştirmek için gereken kavramlar ve araçları ile tanımak için.

<a name="Summary" />

### <a name="client-server-apps"></a>İstemci-sunucu uygulamaları

Geleneksel yüklü uygulamalar yanı sıra Apple TV, web tabanlı istemci-sunucu medya web teknolojileri (HTTPS, XML ve JavaScript) kullanan uygulamalar oluşturmak kolaylaştırır. Apple'nın TVML biçimlendirme dili kullanarak kullanıcı arabirimi tasarlayın ve JavaScript TVMLKit kullanarak uygulamanın davranışları tanımlamak için kullanın.

Daha fazla bilgi için lütfen Apple'nın bkz [Apple TV işaretleme dili başvurusu](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [TVJS Framework başvurusu](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076), [TVMLKit Framework başvurusu](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), [hakkında HTTP canlı akış](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) ve [HLS yazma belirtimi Apple TV için](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) belgeleri.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>Kullanıcı arabirimi sorunları

İOS veya OS X aksine, Apple TV dokunmatik veya doğrudan seçin ve bir uygulama veya içeriği ile etkileşim kurmak kullanıcı izin fare yok. Bunun yerine, kullanıcı yeni Siri uzaktan veya Bluetooth oyun denetleyicileri bir uygulamanın kullanıcı arabiriminde gezinme. Daha fazla bilgi için lütfen bkz bizim [Siri uzak ve Bluetooth denetleyicileri](~/ios/tvos/platform/remote-bluetooth.md) belgeleri.

Ayrıca, genel kullanıcı deneyimi iOS veya tek kullanıcı deneyimleri olma eğilimindedir Mac uygulamaları büyük ölçüde farklıdır. Apple TV ile kullanıcı deneyimleri burada birkaç kişiye tek bir uygulamayı ve birbirleri ile etkileşim kanepenizde üzerinde durduğunu doğası gereği daha sosyal olma eğilimindedir. Başarılı bir Apple TV Uygulama Deneyimi (yeni bir uygulama veya mevcut bir bağlantı noktası oluşturma) tasarlamak için bu değişiklikleri dikkate alınması gerekir. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>Odak ve Parallax görüntüleri ile çalışma

Yukarıda belirtildiği gibi Xamarin.tvOS uygulamanızdaki kullanıcıların arabirimiyle burada bunlar cihazın ekran görüntülerinde ancak dolaylı olarak gelen Siri uzaktan kullanarak yer arasında dokunun, doğrudan iOS ile etkileşim değil. Sunmak ve bu kullanıcı etkileşimi işlemek için bir temel odak modeli Apple TV kullanır. 

Odak değiştikçe Zarif animasyonları ve etkileri (örneğin, görüntüleri Parallax etkisi) odağa sahip kullanıcı arabirimi öğesi açıkça tanımlamak için kullanılır.

Kullanıcı Siri uzak yavaş, döngüsel bir hareketi yaparsa odaklanmış öğesi bu taşıma yanıta gerçek zamanlı sway. Sway gerçekleştiği sırada aydınlatılmış bir görünüm görünmek görünmesini yüzeyini yapmadan görüntüsünü uygulanır. Verilen bir miktar kaldıktan sonra herhangi bir odak içerik karartır ve Focused öğesi daha da büyük büyüyecektir.

Daha fazla bilgi için lütfen bkz bizim [gezinti ve odak çalışma](~/ios/tvos/app-fundamentals/navigation-focus.md) ve [simgeleri ve görüntüleri çalışma](~/ios/tvos/app-fundamentals/icons-images.md) belgeleri.

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>Giriş ekranı

Apple TV giriş ekranı yüklenir ve kullanıcı tercihleri erişmek için bir yol sağlayan tüm uygulamalar gösterilir:

[![](tvos9-images/home01.png "Giriş ekranı")](tvos9-images/home01.png#lightbox)

Kullanıcı bir kılavuz Siri bir uygulama seçin ve başlatmak için odak kullanarak uzaktan üzerinde dokunma hareketleri kullanarak uygulama simgeleri gider. Uygulama simgesi ilk olası kullanıcınız harika izlenim için fırsat ve uygulamanızın amacı bir bakışta iletişim kurmanız gerekir.

Her uygulama, küçük ve büyük bir uygulama simgesini sürümü sağlamanız gerekir. Uygulama yüklendiğinde Apple TV giriş ekranında küçük simge kullanılır. Büyük sürüm App Store tarafından kullanılır. Büyük uygulama simge küçük simge sürüm Görünüm ve yapısını taklit etmelidir.

Daha fazla bilgi için lütfen bkz bizim [simgeleri ve görüntüleri çalışma](~/ios/tvos/app-fundamentals/icons-images.md) belgeleri.

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>Üst raf

Kullanıcının Apple TV giriş ekranında üst satırda Xamarin.tvOS uygulamanızı yerleştirdiğini, uygulamanızı kullanıcı tarafından seçildiğinde büyük bir üst raf görüntü görüntülenir. Bu görüntü, uygulamanızı özelliklerini vurgulamak veya içeriği doğrudan bağlantılar sağlar.

[![](tvos9-images/topshelf01.png "Üst raf")](tvos9-images/topshelf01.png#lightbox)

Üst raf görüntü ya da tek bir statik sağlanabilir `.png` veya `.lsr` dosyası veya dinamik olarak oluşturulabilir çalışma zamanında odaklanabilir öğeleri tek bir satır.

Statik bir üst raf görüntü yerine dinamik bir satır veya odaklanabilir öğeler veya başlıkları kaydırma dinamik kümesi içerebilir. Bu dinamik stili her ikisi de, uygulama veya atlama en fazla kullanılan özelliklerini içine tarafından sağlanan içeriği Vurgula olanak tanır.

Daha fazla bilgi için lütfen bkz bizim [simgeleri ve görüntüleri çalışma](~/ios/tvos/app-fundamentals/icons-images.md) belgeleri ve Apple'nın [TVServices Framework başvurusu](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) sağlamak için uygulamanıza bir üst raf uzantısı ekleme hakkında daha fazla bilgi için dinamik üst raf içerik.






## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin (video) ile tvOS için uygulama oluşturma](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
