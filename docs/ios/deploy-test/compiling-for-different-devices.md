---
title: "Farklı aygıtlar için derleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3B259248-887E-3E4F-E09C-7AD28C2A8CEE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 13838215b32abe49a5fe07b04088bc4216250844
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="compiling-for-different-devices"></a>Farklı aygıtlar için derleme

Yürütülebilir dosyanın derleme özelliklerini projenin yapılandırılabilir **iOS yapı** proje adına sağ tıklayıp için gözatma bulunan özellikler sayfası **Seçenekleri > iOS yapı** içinde Mac için Visual Studio ve **özellikleri** Visual Studio'da:

[[ide name="xs"]]

[ ![](compiling-for-different-devices-images/image1.png "Projeleri iOS yapı Özellikler sayfası")](compiling-for-different-devices-images/image1.png) 

[[/ide]] 

[[ide name="vs"]]

[ ![](compiling-for-different-devices-images/image1a.png "Projeleri iOS yapı Özellikler sayfası")](compiling-for-different-devices-images/image1a.png)

[[/ide]]

Yapılandırma seçeneklerle yanı sıra kullanıcı arabirimini, komut satırı seçenekleri için kendi kümesi de geçirebilirsiniz [Xamarin.iOS Oluşturma Aracı (mtouch)](~/ios/deploy-test/mtouch.md).

[http://iossupportmatrix.com/](http://iossupportmatrix.com/) tüm gerekli cihazlar, mimari ve iOS sürümleri dahil emin olmak için kullanılan yardımcı bir kaynaktır.

 <a name="SDK_Options" />


## <a name="sdk-options"></a>SDK seçenekleri

Mac için Visual Studio SDK ilgili iki önemli özelliklerini yapılandırmanıza olanak sağlar: yazılımınız ve dağıtım hedef (veya en düşük gerekli iOS sürüm) oluşturmak için kullanılan iOS SDK sürümü.

İOS **SDK sürümü** seçeneği sağlar kullanım farklı bir Apple sürümleri yayımlanan SDK, bu derleyicileri, linkers ve bu derleme sırasında başvuruda bulunmalıdır kitaplıklarına Xamarin.iOS yönlendirir. 

**Dağıtım hedef** ayarını, gerekli en düşük sürümü, uygulamanızın üzerinde çalışacağı işletim sistemi seçmek için kullanılır. Bu, projenizin Info.plist dosyasında ayarlanır. Uygulamanızı çalıştırmak için gereken tüm API'leri en düşük sürümü seçmeniz gerekir.

Genel olarak, Xamarin.iOS API SDK'ın en son sürümde kullanılabilir tüm yöntemlerini gösterir ve gerektiğinde, biz işlevselliği çalışma zamanında kullanılabilir olup olmadığını algılamak izin kolaylık özelliklerini sağlayın (örneğin, `UIDevice.UserInterfaceIdiom` ve `UIDevice.IsMultitaskingSupported` her zaman çalışması Xamarin.iOS üzerinde biz arka planda tüm iş yapın).

 <a name="Linking" />


## <a name="linking"></a>Bağlama

Ayrılmış sayfamıza bakın [bağlayıcı](~/ios/deploy-test/linker.md) nasıl bağlayıcı, yürütülebilir dosyalar boyutunu azaltmanıza yardımcı hakkında daha fazla bilgi ve etkili bir şekilde kullanmayı öğrenin.

 <a name="Code_Generation_Engine" />


## <a name="code-generation-engine"></a>Kod oluşturma altyapısı

Xamarin.iOS 4.0 ile başlayarak, Xamarin.iOS için iki kod oluşturma arka uçlarını vardır. Normal Mono kod oluşturma altyapısı ve biri üzerinde LLVM en iyi duruma getirme derleyici tabanlı. Her motor kendi Artıları ve eksileri vardır.

Hızla yineleme olanak sağlayacak şekilde genellikle, geliştirme sürecinde, büyük olasılıkla Mono kod oluşturma altyapısı kullanır. Yayın derlemeleri ve AppStore dağıtımı için LLVM kod oluşturma altyapıya geçin ve isteyeceksiniz.

Mono altyapısı makinelerinden uzun derleme sürelerini, arka uç altyapısı en iyi duruma getirme LLVM daha hızlı ve daha sıkı kodu oluşturur.

Bu iOS Mac veya Visual Studio için Visual Studio'da derleme seçenekleri etkinleştirebilirsiniz.

[ ![](compiling-for-different-devices-images/image2.png "LLVM etkinleştirme")](compiling-for-different-devices-images/image2.png)

[ ![](compiling-for-different-devices-images/image2a.png "LLVM etkinleştirme")](compiling-for-different-devices-images/image2a.png)

 <a name="ARMV7_and_ARMV7s_support" />


## <a name="architecture-support"></a>Mimari desteği

<a name="armv6-discontinued" />

### <a name="armv6-xamarinios-discontinued-support-for-armv6-with-v810"></a>ARMv6 (Xamarin.iOS dönüştürülmesine ARMv6 desteği v8.10 ile)

- iPhone (original), 3G
- iPod 1, 2. nesil

### <a name="armv7"></a>ARMv7

- iPhone 3GS, 4, 4S
- iPad 1, 2, 3, Mini
- iPod 3, 4, 5 oluşturma

### <a name="armv7s"></a>ARMv7s

- iPhone 5
- iPhone 5c
- iPad 4

Yalnızca ARMv7s işlemci hedefliyorsanız, oluşturulan kodu biraz daha hızlı olacaktır, ancak fat ikili paketi birden fazla yürütülebilir dosyaları içeren bir derleme sürece artık ARMv7 veya ARMv6 sistemlerde çalışır.

### <a name="arm64-xamarinios-started-supporting-arm64-in-v86"></a>ARM64 (Xamarin.iOS başlatılan ARM64 içinde v8.6 destekleme)

- iPhone 5s
- iPhone SE
- iPhone 6, 6 Plus
- iPhone 6s, 6s Plus
- iPhone 7, 7 Plus
- iPhone 8, 8 Plus
- iPhone X
- iPad Air
- iPad Air 2
- iPad Mini 2, 3, 4
- iPad Pro (Tümü)

Uygulama deposuna gönderilen her türlü derleme 64 bit desteği içermesi gerekir, bu tarafından belirlenen bir gereksinimdir dikkat edin [Apple](https://developer.apple.com/news/?id=12172014b). Ayrıca, iOS 11 yalnızca 64 bit uygulamalarını destekler.

 <a name="ARM_Thumb_Support" />


### <a name="arm-thumb-2-support"></a>ARM Flash-2 desteği

Flash, ARM işlemcileri tarafından kullanılan daha kısa bir yönerge kümesidir. Flash destek sağlayarak daha yavaş yürütme sürelerinin ödün verme pahasına yürütülebilir dosyanızın boyutunu azaltabilirsiniz. Flash ARMv7 ve ARMv7s desteklenir.

 <a name="Conditional_framwork_useage" />


## <a name="conditional-framework-usage"></a>Koşullu Framework kullanımı

Projeniz kullanmak istiyorsa, bazı özelliklerin daha yeni iOS serbest, bazı yeni çerçevesinde koşullu yararlanmayı gerekebilir. İOS 4.0 veya yukarısını çalıştırırken IAD kullanır, ancak hala 3.x cihazları desteklemek bu prime örneği isteyen. Bunu gerçekleştirmek için "zayıf" IAD framework karşı bağlamanız gerekir bilmeniz Xamarin.iOS izin gerekir. Zayıf bağlamaları framework, isteğe bağlı ilk saat framework sınıfından gereklidir yalnızca yüklü olduğundan emin olun.

Bunu yapmak için aşağıdaki adımları izlemesi gerekir:

-  Açık, **proje seçenekleri** gidin **iOS yapı** bölmesi.
-  Ekleme `'-gcc_flags "-weak_framework iAd"'` için **ek seçenekler** her yapılandırması üzerinde zayıf bağlantı istiyor:


[ ![](compiling-for-different-devices-images/image3.png "Ek Seçenekler")](compiling-for-different-devices-images/image3.png)


Buna ek olarak, burada mevcut iOS eski sürümlerinde çalışan türlerinin kullanımınızı koruma sağlamak gerekir. Bunu gerçekleştirmek için çeşitli yöntemler vardır, ancak biri ayrıştırma `UIDevice.CurrentDevice.SystemVersion`.



## <a name="related-links"></a>İlgili bağlantılar

- [Linker](~/ios/deploy-test/linker.md)
- [Dış - iOS destek matrisi](http://iossupportmatrix.com/)
