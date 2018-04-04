---
title: Dağıtım ve Test Etme
description: Sabitlemeyi ve dağıtım kılavuzları
ms.prod: xamarin
ms.assetid: 2DBF3BF9-79E7-4E24-AF26-E34C972B0169
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 515ea8e63f8309c46a7d802af1daafcb0c483762
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="deployment-and-testing"></a>Dağıtım ve Test Etme

Bu bölümde yanı sıra bir uygulama dağıtmak için test etmek için kullanılan konuları kapsar. Burada konuların hata ayıklama, sınayıcılar ve uygulama mağazası bir uygulamanın nasıl yayımlanacağını dağıtım için kullanılan araçları gibi nesnelerdir.


##  <a name="app-distributioniosdeploy-testapp-distributionindexmd"></a>[Uygulama Dağıtımı](~/ios/deploy-test/app-distribution/index.md)

Bu makalede, yapılandırma, yapı ve de dahil olmak üzere çeşitli farklı yollarla dağıtım için bir Xamarin.iOS uygulaması yayımlama gösterilmektedir:

- [App Store Dağıtımı](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Şirket İçi (Kurumsal) Dağıtım](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Geçici Dağıtım](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)

##  <a name="ipa-deploymentiosdeploy-testapp-distributionipa-supportmd"></a>[IPA dağıtımı](~/ios/deploy-test/app-distribution/ipa-support.md)

Geçici ve kurumsal dağıtımlar sınama veya şirket içi kullanıcılara dağıtılabilir paketleri oluşturmak geliştiriciler izin verir. Bu belge, iTunes kullanarak iOS cihazına eşitlenen bir IPA oluşturma açıklanmaktadır.

## <a name="provisioningprovisioningindexmd"></a>[Sağlama](provisioning/index.md)

Bu kılavuzlar dizi kod imzalama ve özellik listeleri ve uygulamanızın uygulama hizmetleri için sağlama ile çalışma gibi essentials sağlama kapsar. 

## <a name="wireless-deploymentwireless-deploymentmd"></a>[Kablosuz Dağıtım](wireless-deployment.md)

 Xcode 9 dağıtmak ve uygulamanızın hatalarını ayıklama istediğiniz her zaman için donanım aygıtlarınızın sahip olmak yerine bir iOS cihazı veya Apple TV için bir ağ üzerinden dağıtma seçeneği sunulmuştur. Bu özellik şu anda önizlemede değil.

##  <a name="testflightiosdeploy-testtestflightmd"></a>[TestFlight](~/ios/deploy-test/testflight.md)

TestFlight artık Apple'nın ait ve beta Xamarin.iOS uygulamalarınızı test etmek için birincil yoludur. Bu makalede iTunes Connect ile çalışmaya, uygulamanızı karşıya TestFlight işleminin – tüm adımlarda size yol gösterir.

##  <a name="debugging-in-xamariniosiosdeploy-testdebugging-in-xamarin-iosmd"></a>[Xamarin.iOS içinde hata ayıklama](~/ios/deploy-test/debugging-in-xamarin-ios.md)

Visual Studio ve Visual Studio Mac IDE için hem iOS simülatörü ve iOS aygıtlarında Xamarin.iOS uygulamalarında hata ayıklama desteğini içerir. Bu makalede, hata ayıklayıcıyı kullanma ve bunun yanı sıra desteklediği çeşitli seçeneklerinin nasıl yapılandırılacağı gösterilmektedir.


##  <a name="touchunitiosdeploy-testtouchunitmd"></a>[Touch.Unit](~/ios/deploy-test/touch.unit.md)

Bu belge, Xamarin.iOS projelerinizi için birim testleri oluşturmak açıklar.
Birim testi Xamarin.iOS ile yapılır, hem iOS içeren Touch.Unit framework kullanarak test Çalıştırıcısı yanı sıra değiştirilmiş bir sürümünü [NUnitLite](http://www.nunitlite.com/) birim testleri yazma için tanıdık bir API kümesi sağlayan.



##  <a name="using-instruments-to-detect-native-leaks-using-markheapiosdeploy-testusing-instruments-to-detect-native-leaks-using-markheapmd"></a>[Yerel MarkHeap kullanarak sızıntıları algılama gereçlerine kullanma](~/ios/deploy-test/using-instruments-to-detect-native-leaks-using-markheap.md)

Bu makalede Instruments herhangi bir iOS aygıtı ve herhangi bir Xamarin.iOS uygulaması üzerinde nasıl kullanılacağı açıklanmaktadır. Ayrıca simulator profili uygulamalarda nasıl bakar.



##  <a name="walkthrough---using-apples-instrument-tooliosdeploy-testwalkthrough-apples-instrumentmd"></a>[İzlenecek Yol - Apple’ın İşaretleme Aracını Kullanma](~/ios/deploy-test/walkthrough-apples-instrument.md)

Bu makalede Apple'nın Instruments aracının Xamarin ile oluşturulan bir iOS uygulaması bellek sorunları tanılamak için nasıl kullanılacağı anlatılmaktadır. Araçları başlatın, yığın anlık görüntülerini almak ve bellek büyüme çözümlemek nasıl gösterir. Ayrıca, Instruments görüntülemek ve bellek soruna neden tam satır kod sabitleme için nasıl kullanılacağını gösterir.

##  <a name="linking-on-ioslinkermd"></a>[İos'ta bağlama](linker.md)

Bağlayıcı ayarlarını ve kullanım değiştirmek için nasıl erişileceği en küçük olası uygulama paketi sağlamak için nasıl çalıştığını açıklar.

##  <a name="xamarinios-performanceperformancemd"></a>[Xamarin.iOS Performance](performance.md)

Xamarin.iOS ile oluşturulan uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir.

##  <a name="mtouchmtouchmd"></a>[mtouch](mtouch.md)

Notlar ve mtouch.exe, projenizin iOS tarafından kullanılabilir bir uygulamaya derlemeler komut satırı aracı hakkında bilgi.

## <a name="ios-build-mechanicsios-build-mechanicsmd"></a>[iOS derleme mekanizması](ios-build-mechanics.md)

Uygulamalarınızı süreyi bu kılavuzda araştırır ve tüm yapı yapılandırmalarını için daha hızlı çalıştırılacağı yöntemlerinin nasıl kullanılacağını oluşturur.