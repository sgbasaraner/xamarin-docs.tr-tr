---
title: 64/32-bit platformu hakkında önemli noktalar
description: 32 bit ve 64 bit mimari uygulamanız için hedefleme ilgili önemli noktalar
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 06e65b8a639345af65c6e5d3bbe57d8bb3feffa1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="3264-bit-platform-considerations"></a>64/32-bit platformu hakkında önemli noktalar

İOS ve macOS 32 ve 64-bit uygulamalar geçmişte desteklenen olsa da, Apple 32-bit desteği kullanım dışı kademeli olarak.

İOS 11 itibariyle, 32-bit uygulamalar artık başlatır, ve [uygulama mağazası tüm gönderileri 64-bit desteklemelidir](https://developer.apple.com/news/?id=06282017b).

Ocak 2018 içinde başlangıç [Mac App Store gönderilen yeni uygulamalar, 64-bit desteklemelidir](https://developer.apple.com/news/?id=06282017a), ve mevcut uygulamalar tarafından Haziran 2018 güncelleştirilmesi gerekir.

Xamarin'ın Klasik API (`XamMac.dll` ve `monotouch.dll`) yalnızca 32-bit uygulamalar desteklenir. Ancak, yeni Xamarin.iOS ve Xamarin.Mac uygulamaları kullanan [Unified API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` ve `Xamarin.Mac`) varsayılan olarak, ve bu nedenle hedef 32 ve 64-bit, gerekli olarak kullanabilirsiniz.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS uygulamaları 64-bit etkinleştirme derlemeler

> [!WARNING]
> Bu bölümde, geçmiş nedeniyle ve birleşik API için eski Xamarin.iOS projeleri taşıyın ve 64-bit desteği yardımcı olmak için dahil edilmiştir. Tüm yeni Xamarin.iOS projeleri Unified API ve hedef 64-bit varsayılan olarak kullanır.

Birleşik API'sine dönüştürülen Xamarin.iOS mobil uygulamalar için geliştiriciler el ile yapı ayarları hedef 64-bit güncelleştirmeniz gerekir:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. İçinde **çözüm paneli**, uygulamanın proje açmak için çift **proje seçenekleri** penceresi.
2. Seçin **iOS yapı**.
3. İPhone benzeticisi, içinde **desteklenen mimariler** açılan listesinde, şunlardan birini seçin **x86\_64** veya **i386 + x86\_64**:

   [![Desteklenen mimariler için x86 ayarı\_64 veya i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. Fiziksel cihazlar için kullanılabilir birini seçin **ARM64** birleşimleri:

   [![Desteklenen mimariler ARM64 birleşimleri birine ayarı](Images/Image02.png "ARM64 birleşimleri birine ayarı desteklenen mimariler")](Images/Image02-large.png#lightbox)

5. **Tamam**'ı tıklatın.
6. Temiz bir yapı gerçekleştirin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. İçinde **Çözüm Gezgini**, uygulamanın projesine sağ tıklatın ve **özellikleri**.
2. Seçin **iOS yapı**.
3. Bir iPhone benzeticisi'ı için ayarlamak **desteklenen mimariler** ya da **x86\_64** veya **i386 + x86\_64**: 

   [![Desteklenen mimariler ayarı x86_64 veya i386 + x86\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. Fiziksel cihazlar için kullanılabilir birini seçin **ARM64** birleşimleri:
    
   [![Desteklenen mimariler ARM64 birleşimleri birine ayarı](Images/VS01.png "ARM64 birleşimleri birine ayarı desteklenen mimariler")](Images/VS01-large.png#lightbox)

5. Değişikliklerinizi kaydedin.
6. Temiz bir yapı gerçekleştirin.

-----

ARMv7s iPhone 5 (veya daha büyük) dahil A6 işlemcisi tarafından desteklenir. ARMv7 kodu daha hızlı ve ARMv6 küçükse, yalnızca iPhone 3GS ve sonraki sürümlerinde çalışır ve Apple'nın iPad veya en düşük iOS sürüm 5.0 hedeflerken gereklidir. ARMv6 tüm cihazlarda çalışır ancak artık Xcode 4.5 ve sonraki sürümleriyle birlikte derleyici tarafından desteklenmiyor. 

ARM64 iOS 8 iPhone 6 veya diğer 64-bit cihazları desteklemek için gereklidir ve Apple'nın yeni veya güncelleştirme iTunes App Store'da uygulamalardan çıkma gönderirken gerekir.

Apple için kapsamlı bir görünüm çeşitli iOS cihazları yeteneklerini adresindeki denetleyin [cihaz Uyumluluk](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) belge.

### <a name="64-bit-and-binary-size-increases"></a>64-bit ve ikili boyutunu artırır

Uygulamalar, Apple'nın 64-bit, 32-bit iOS geçişi sırasında 32-bit ve 64 bit donanımda çalıştırmanız gerekir. Bu nedenle, her ikisi de hedeflemek geliştiricilerin Xamarin'ın birleşik API sağlar.

32 bit ve 64 bit mimariyi hedefleme uygulama boyutunu önemli ölçüde artırır. Ancak, bunun nedenle hala eski cihazları desteklerken en iyi duruma getirilmiş kodu çalıştırmak yeni aygıtlar izin verir.

> [!IMPORTANT]
> İTunes App Store'dan iOS uygulama gönderirken aşağıdaki iletisini alırsanız _"uyarı ITMS-9000: 64-bit desteği eksik. 1 Şubat 2015 yeni iOS başlangıç uygulamalarını uygulama mağazasında karşıya 64-bit desteği içermelidir ve iOS 8 SDK'sı, Xcode 6 veya sonraki sürümlerde dahil yerleştirilmiş olabilir. Etkinleştirmek için 64-bit projenizdeki, Xcode yapı ayarı "Standart Mimarisi" varsayılan kullanmanızı öneririz 32-bit ve 64 bit kod ile tek bir ikili oluşturmak için. "_ Desteklenen mimariler kullanılabilir birini geçiş yapmanız **ARM64** (yukarıda gösterildiği gibi) birleşimi, derleme ve yeniden gönderin.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> Mac App Store gönderilen tüm yeni Mac uygulamaları Ocak 2018 başlayarak, 64-bit desteklemesi gerekir. Mevcut Mac App Store uygulamaları ve bunların güncelleştirmelerini 64-bit Haziran 2018 başlangıç desteklemesi gerekir. Bkz: [Apple'nın announcment](https://developer.apple.com/news/?id=06282017a) ve [64-bit Xamarin.Mac uygulamalarınızı güncelleştirmeye açıklayan bir kılavuz](~/cross-platform/macios/32-and-64/mac-64-bit.md).

Çoğu modern Mac bilgisayarlar 32-bit ve 64-bit uygulamaları desteklemez.   MacOS 10.6 (kar Leopard) son işletim sisteminin 32-bit sistemleri üzerinde çalışan oluştu.   Her iki sistemle 2010 destek beri yayınlanan en Mac'ler.

İOS, birçok yeni macOS sürümlerinde kullanıma sunulan yeni çerçeveleri yalnızca 64-bit modunda (CloudKit EventKit, oyun denetleyicisi, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, sosyal, desteklenir ve diğerlerinin yanı sıra MapKit).

Birleşik API izin geliştiricilerin üretmek istedikleri ne tür bir uygulama seçin: 32 bit veya 64-bit.

**32-bit uygulamalar** , 32 bit ve 64-bit Mac bilgisayarlarda çalıştırmak, 32 bit sınırlı bir adres alanına sahip ve 32 bit tüm kitaplıkları istiyorsunuz.

Bu mod, daha küçük bir indirme sahip olmak istiyorsanız 64-bit modunda çalıştırmayın 32-bit bağımlılıkları varsa veya yoksa hiçbir performans avantajı 64 bit taşıma genellikle kullanır.

Bu mod, paylaşmıyor, birçok çerçeveleri macOS Mavericks ve macOS Yosemite kullanılabilir kullanabilmek şekilde sınırlama.

**64-bit uygulamalar** yalnızca 64-bit Mac cihazlarda çalışır.

Mac için bu tercih edilen işleminin kullanımda en Mac'ler bugün 64-bit modunu destekler ve Apple'nın sağladığı çerçeveleri tamamını erişimi moddur.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>64-bit etkinleştirme Xamarin.Mac uygulamaların derlemeler

Xamarin.Mac kullanarak 64 bit bir uygulama oluşturma hakkında daha fazla bilgi için lütfen bkz [64-bit uygulamaları güncelleştirme Xamarin.Mac birleşik](~/cross-platform/macios/32-and-64/mac-64-bit.md) Kılavuzu.

## <a name="related-links"></a>İlgili bağlantılar

- [Klasik vs Unified API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
