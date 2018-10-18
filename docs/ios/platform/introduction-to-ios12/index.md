---
title: İOS 12 giriş
description: Bu belge üst düzey hangi Xamarin'in preview için sürüm C# bağlamaları sağlar. bazı iOS 12 API açıklamasını sağlar.
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/08/2018
ms.openlocfilehash: 81375e8c66e5504604d0d4cb3be34afd58f4269d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615154"
---
# <a name="introduction-to-ios-12"></a>İOS 12 giriş

Bu belge üst düzey hangi Xamarin'in preview için sürüm C# bağlamaları sağlar. bazı iOS 12 API açıklamasını sağlar.

Xamarin ile iOS 12 uygulamalar oluşturmaya başlamak için bkz: [Başlangıç Kılavuzu](get-started.md)

## <a name="arkit-2arkit2md"></a>[ARKit 2](arkit2.md)

ARKit dahil iOS ile genişletilmiş gerçeklik çerçevedir. ARKit 2 birden çok bir genişletilmiş gerçeklik sahnede birbiriyle etkileşim olanağı tanıyan nesneleri alanında kalıcı hale getirmek ve daha sonraki bir zamanda döndürülmeleri mümkün kılar ve 2B resim tanıma sağlar ve tanıma izleme ve 3B nesne. iOS 12 Ayrıca, AR hızlı konum, uygulamalarınızda AR modelleri usdz işleyecek bir yol sağlar.

## <a name="siri-shortcutssiri-shortcutsmd"></a>[Siri kısayolları](siri-shortcuts.md)

Siri kısayolları, geliştiricilerin uygulamalarına Siri ile daha derin tümleştirme sağlar. Siri kısayollarıyla kullanıcılar içeriği açmak veya arka plan görevlerini başlatmak için sesli komutları kullanabilir veya bunların aynı görevleri kilit ekranında Siri öneren kısayolları aracılığıyla başlatabilirsiniz.

## <a name="core-ml-2coremlmd"></a>[ML 2 Çekirdek](coreml.md)

2 Çekirdek ML model boyutlandırma ve esnek modelleri aracılığıyla uygulama boyutunu azaltır, yeni bir batch tahmin API ile uygulama performansını artırır ve machine learning'de geliştirmeleri desteklemek için özel modelleri kullanır.

## <a name="notification-improvementsnotificationsindexmd"></a>[Bildirim geliştirmeleri](notifications/index.md)

İOS 12'de, gruplandırılmış bildirimleri, uygulama veya iş parçacığı ile ilgili gruplandırmaları mevcut kullanıcı bildirimleri mümkün kılar. Özet metni hakkında daha fazla bildirim grubu hakkında bilgi sağlar.

Bildirim içerik iOS 12 uzantılarında, özel kullanıcı arabirimleri ve dinamik eylem düğmeleri için izin verir.

## <a name="natural-language-frameworknatural-languagemd"></a>[Doğal dil çerçevesi](natural-language.md)

Doğal dil çerçevesi, çeşitli dil analizi gerçekleştirmek uygulamaları etkinleştirir. Örneğin, bu bölümleri konuşma belirleyin ve bir metin bloğu tarafından temsil edilen bir dil belirleyin.

## <a name="vision-framework"></a>Vision çerçevesi

Vision çerçevesi, yüzleri çeşitli yönleriyle algılayabilir bir geliştirilmiş yüz algılayıcısı içerir. Ayrıca, isteği düzeltmeler belirli Vision çerçevesi algoritması düzeltmesini seçebilirsiniz.

## <a name="photo-and-video-apis"></a>Fotoğraf ve video API'leri

İOS 12'de, dikey ayrılmasını API dikey etkileri örtü – ön dikey görüntü arka planını betimleyen ve çeşitli yansıma efektleri oluşturulmasında yararlı olan bir doğrusal maskesi döndürür. iOS 12 de olmasını sağlar TrueDepth kameradan derinlik verileri gerçek zamanlı bir video efektleri için kullanmak mümkündür.

## <a name="passwords"></a>Parolalar

iOS 12 kullanıcılar ve geliştiricilerin parolaları ile çalışmak için daha kolay hale getirir:

- Parola otomatik doldurma ve otomatik güçlü parolalar otomatik olarak oluşturmak, depolamak ve kaydolmak için gerekli ve bir uygulamaya oturum iOS uygulamalarında güçlü parolalar kullanın mümkün kılar.
- Güvenlik kodu otomatik doldurma olmadan el ile kesme ve yapıştırma veya anımsama SMS tabanlı kimlik doğrulama kodlarını kullanmak mümkün kılar.
- `ASWebAuthenticationSession` Sınıfı, Federasyon kimlik doğrulama hizmetleri ile çalışma işlemini sorunsuz hale getirir.
- Otomatik doldurma kimlik bilgisi sağlayıcı uzantıları kullanıcı adı ve parolaları için oturum açma alanlarını sağlamak parola üçüncü taraf uygulamaları için mümkün hale getirir.

## <a name="healthkit-updates"></a>HealthKit güncelleştirmeleri

tanıtılan iOS 11.3 [sağlık kayıtlarına](https://www.apple.com/healthcare/health-records/), kullanıcıların kendi sistem durumu indirmesine izin veren çeşitli sağlık kurumları bilgileri kaydedin ve iOS cihazlarında görüntüleyebilir. iOS 12 üçüncü taraf uygulamalar bu verilere güvenle erişmesini sağlayan API'ler ekler.

## <a name="imessage-app-presentation-contexts"></a>iMessage uygulama sunu bağlamları

İOS 12'de, normal iMessage uygulama olarak veya bir fotoğraf veya video efekti bağlamında çalıştırmak uygulamalara izin ver sunu bağlamı, iMessage uygulamaları destekler.

## <a name="network-framework"></a>Ağ çerçevesi

Ağ framework, ağ yığınını temel `URLSession` iOS uygulamalarında yaygın olarak kullanılan API'ler, artık bir tek başına çerçeve TCP, UDP, TLS, IPv4/IPv6 ve daha fazlası ile çalışmak kolaylaştırır.

## <a name="carplay"></a>CarPlay

İOS 12'de, üçüncü taraf uygulamaları haritaları ve bırakma tarafından Aç Gezinti yönergelerini CarPlay içinde yeni CarPlay çerçevesi kullanılarak teslim edebilirsiniz.

## <a name="deprecations"></a>Bırakılanların

Apple iOS 12 kullanımdan kaldırmıştır:

- OpenGL ES [geliştiriciler teşvik](https://developer.apple.com/ios/whats-new/) Metal benimsemek için.
- [`UIWebView`](https://developer.xamarin.com/api/type/UIKit.UIWebView/), [sunulmasıyla `WKWebView` ](https://developer.apple.com/documentation/webkit/wkwebview?language=objc).

## <a name="related-links"></a>İlgili bağlantılar

- [İOS (Apple) 12 Get hazır](https://developer.apple.com/ios/)
