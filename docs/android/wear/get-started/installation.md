---
title: Kurulum ve yükleme
description: Bu makalede, bilgisayar ve cihazlar Android takmak geliştirme için hazırlamak için gereken yapılandırma ayrıntılarını ve yükleme adımları anlatılmaktadır. Bu makalenin sonundaki, Mac ve/veya Microsoft Visual Studio için Visual Studio'ya Xamarin.Android yıpranması yüklemesi tümleşik çalışma gerekir ve ilk Xamarin.Android yıpranması uygulamanızı oluşturmaya başlamak için hazır olması.
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 86ed62075f5c39eaf7b0e348020f4a9361764727
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="setup-and-installation"></a>Kurulum ve yükleme

_Bu makalede, bilgisayar ve cihazlar Android takmak geliştirme için hazırlamak için gereken yapılandırma ayrıntılarını ve yükleme adımları anlatılmaktadır. Bu makalenin sonundaki, Mac ve/veya Microsoft Visual Studio için Visual Studio'ya Xamarin.Android yıpranması yüklemesi tümleşik çalışma gerekir ve ilk Xamarin.Android yıpranması uygulamanızı oluşturmaya başlamak için hazır olması._

## <a name="requirements"></a>Gereksinimler

Aşağıdaki Android takmak Xamarin tabanlı uygulamalar oluşturmak için gereklidir:

-   **Visual Studio veya Mac için Visual Studio** &ndash; , Visual Studio, Visual Studio 2015 Professional kullanarak ya da daha sonra gerekli değilse.

-   **Xamarin.Android** &ndash; Xamarin.Android 4.17 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-   **Android SDK** -Android SDK 5.0.1 (API 21) veya sonrası Android SDK Yöneticisi aracılığıyla yüklü olmalıdır.

-   **Java Geliştirme Seti** &ndash; Xamarin Android geliştirme gerektirir [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) API düzeyi 24 için geliştirme veya daha büyük olduğunda (JDK 1.8 de destekler API düzeylerini 24'den önceki).

Kullanmaya devam edebilirsiniz [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özellikle geliştirme veya önceki bir sürümünü kullanıyorsanız.

> [!IMPORTANT]
> Xamarin.Android JDK 9 desteklemez.

## <a name="installation"></a>Yükleme

Xamarin.Android yükledikten sonra böylece oluşturmak ve Android takmak uygulamaları sınamak hazır aşağıdaki adımları gerçekleştirin: 

1.  Gerekli Android SDK ve araçlarını yükler.
2.  Bir test cihazı yapılandırın.
3.  Android takmak ilk uygulamanızı oluşturma.

Bu adımlar aşağıdaki bölümlerde açıklanmaktadır.


### <a name="install-android-sdk-and-tools"></a>Android SDK ve araçlarını yükleme 

Başlatma **Android SDK Manager**: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Visual Studio'da Android SDK Yöneticisi'ni başlatmak nasıl](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Mac için Visual Studio Android SDK Yöneticisi'ni başlatmak nasıl](installation-images/xs/sdk-menu.png)

-----


Aşağıdaki Android SDK ve araçlarının yüklü olduğundan emin olun:

* Android SDK Araçları v 24.0.0 veya daha yüksek ve
* Android 4.4W (API20) veya
* Android 5.0.1 (API21) ya da daha yüksek.

En son SDK ve araçlarının yüklü yoksa, gerekli SDK Araçları indirme *ve* API BITS (bunları bulmak için bir bit kaydırmanız gerekebilir &ndash; API seçimi aşağıda gösterilmiştir): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Örnek SDK Manager Android 5.0.1 etkinleştirme ekran bileşenleri](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Örnek SDK Manager Android 4.4 ve 5.0.1 etkinleştirme ekran bileşenleri](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>Yapılandırma

Kullanmadan önce uygulamanızı test etme, bir Android takmak öykünücü ya da gerçek bir Android takmak cihaz yapılandırmanız gerekir. 


### <a name="android-wear-emulator"></a>Android yıpranması öykünücüsü

Bir Android takmak öykünücü kullanmadan önce bir Android takmak Android sanal cihazı (AVD) kullanarak yapılandırmalısınız **Google öykünücü yöneticisini**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Visual Studio'dan Android öykünücüsü yöneticisini başlatmak nasıl](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Mac için Visual Studio'dan Android öykünücü yöneticisini başlatmak nasıl](installation-images/xs/emulator-menu.png)

-----

Bir Android takmak öykünücü ayarlama hakkında daha fazla bilgi için bkz: [bir öykünücü üzerinde Android takmak hata ayıklama](~/android/wear/deploy-test/debug-on-emulator.md).


### <a name="android-wear-device"></a>Android yıpranması cihaz

Android takmak Smartwatch gibi bir Android takmak cihazınız varsa, bu aygıttaki bir öykünücü kullanmak yerine uygulama ayıklayabilirsiniz. Yıpranması aygıtla geliştirme hakkında daha fazla bilgi için bkz: [takmak bir aygıtta hata ayıklama](~/android/wear/deploy-test/debug-on-device.md).


## <a name="create-your-first-android-wear-app"></a>İlk Android yıpranması uygulamanızı oluşturma

İzleyin [Merhaba, takmak](~/android/wear/get-started/hello-wear.md) ilk izleme uygulamanızı oluşturmak için yönergeleri.


## <a name="packaging-your-app"></a>Uygulamanızı paketleme

Android yıpranması uygulamalar her zaman bir yardımcı Android telefon uygulaması ile dağıtılır. 

Ana Android uygulamanız için bir başvuru olarak Android takmak uygulamanızı eklediğinizde otomatik olarak bir Android takmak proje olduğu varsayılır ve gerekli tüm XML ve meta verileri sizin için oluşturur. Ek olarak, uygulamalarınızı Google Play kolayca sevk edebilir böylece paket ve sürüm numaraları eşleştiğini doğrular. 

Yıpranması uygulamaları paketleme hakkında daha fazla bilgi için bkz: [paketle birlikte çalışma](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>İlgili bağlantılar

- [SkeletonWear (örnek)](https://developer.xamarin.com/samples/SkeletonWear/)
