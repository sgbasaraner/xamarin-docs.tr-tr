---
title: Windows yükleme
description: Bu kılavuzda Windows Visual Studio için Xamarin.Android yükleme için adımlar açıklanır ve Xamarin.Android ilk Xamarin.Android uygulamanızı oluşturmak için yapılandırma konusunda açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/17/2018
ms.openlocfilehash: ca88159e8bcbcd4665e29b4ad8df9ffe00cfec67
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2018
---
# <a name="windows-installation"></a>Windows yükleme

_Bu kılavuzda Windows Visual Studio için Xamarin.Android yükleme için adımlar açıklanır ve Xamarin.Android ilk Xamarin.Android uygulamanızı oluşturmak için yapılandırma konusunda açıklanmaktadır._


## <a name="overview"></a>Genel Bakış

Xamarin ile artık bulunduğundan hiçbir ek Visual Studio'ya'nin tüm sürümlerinde maliyet ve ayrı bir lisansı gerektirmez, Visual Studio yükleyicisi Xamarin.Android araçları karşıdan yükleyip kullanabilirsiniz.
(El ile yükleme ve Xamarin.Android önceki sürümleri için gerekli adımları lisans artık gerekli değildir.) Bu kılavuzda, şunları öğreneceksiniz:

-   Java Geliştirme Seti, Android SDK ve Android NDK için özel konumlar yapılandırma konusunda.

-   İndirme ve yükleme ek Android SDK Bileşenleri Android SDK Yöneticisi'ni başlatmak nasıl.

-   Nasıl bir Android cihaz veya öykünücü hata ayıklama ve test için hazırlanır.

-   İlk Xamarin.Android uygulaması projenizi oluşturma

Bu kılavuz sonuna bir çalışma olacaktır Xamarin.Android yüklemesi tümleşik Visual Studio'ya ve ilk Xamarin.Android uygulamanızı oluşturmaya başlamak için hazır olacaktır.

## <a name="installation"></a>Yükleme

Windows Visual Studio ile kullanmak için Xamarin yükleme konusunda ayrıntılı bilgi için bkz: [Windows yükleme](~/cross-platform/get-started/installation/windows.md) Kılavuzu.


## <a name="configuration"></a>Yapılandırma

Xamarin.Android uygulamaları geliştirmek için Java Geliştirme Seti (JDK) ve Android SDK'sını kullanır. Yükleme sırasında Visual Studio yükleyicisi bu araçlar varsayılan konumlarına yerleştirir ve geliştirme ortamı uygun yolu yapılandırmayla yapılandırır. Görüntüleyin ve bu konumları tıklayarak değişiklik **Araçlar > Seçenekler > Xamarin > Android ayarları**:

![Xamarin Android ekran görüntüsü, ayarları iletişim kutusu](windows-images/07-settings.png)

Çoğu kullanıcı için bu varsayılan konumları daha fazla değişiklik yapılmadan çalışır. Ancak, (örneğin, Java JDK, Android SDK veya NDK farklı bir konumda yüklüyse) Visual Studio bu araçlar için özel konumlar yapılandırmak isteyebilirsiniz. Tıklatın **değiştirmek** değiştirmek istediğiniz bir yolu yanında, ardından yeni konuma gidin.

Xamarin.Android kullanan [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), API düzeyi 24 için geliştirme veya daha büyük olduğunda gerekli olduğu (JDK 8 de destekler API düzeylerini 24'den önceki). Kullanmaya devam edebilirsiniz [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özellikle geliştirme veya önceki bir sürümünü kullanıyorsanız.

> [!IMPORTANT]
> Xamarin.Android JDK 9 desteklemez.


### <a name="android-sdk-manager"></a>Android SDK Yöneticisi

Android Android çeşitli sürümleri arasında uygulama uyumluluğunu belirlemek için birden çok Android API düzeyi ayarlarını kullanır (Android API düzeyleri hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md)).
Hedeflemek istediğiniz hangi Android API düzeylerini bağlı olarak ek Android SDK bileşenlerini yükleyip gerekebilir. Ayrıca, isteğe bağlı araçların ve Android SDK'SINDA sağlanan öykünücüsü görüntüleri yüklemeniz gerekebilir. Bunu yapmak için kullanın **Android SDK Manager**. Başlatabilirsiniz **Android SDK Manager** tıklayarak **Araçlar > Android > Android SDK Manager**:

[![Android SDK Yöneticisi'ni başlatmak nasıl](windows-images/08-sdk-manager-sml.png)](windows-images/08-sdk-manager.png#lightbox)

Varsayılan olarak, Visual Studio Google Android SDK Yöneticisi'ni yükler:

![Google Android SDK Manager'ın ekran örnek](windows-images/09-google-sdk-manager.png)

Android SDK Araçları Paketi sürümü 25.2.3 kadar sürümlerini yüklemek için Google Android SDK Yöneticisi'ni kullanabilirsiniz. Ancak, Android SDK Araçları paketini daha sonraki bir sürümünü kullanmanız gerekiyorsa, Visual Studio (Visual Studio marketten kullanılabilir) için Xamarin Android SDK Manager Eklentisi yüklemeniz gerekir. Google'nın tek başına SDK Manager sürüm Android SDK Araçları paketini 25.2.3 kullanım için bu gereklidir. 

Xamarin Android SDK Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz: [Android SDK Kurulum](~/android/get-started/installation/android-sdk.md).

### <a name="google-android-emulator"></a>Google Android öykünücüsü

[Google Android öykünücüsü](https://developer.android.com/studio/run/emulator) geliştirmek ve bir Xamarin.Android uygulaması sınamak için yararlı aracı olabilir. Örneğin, tablet gibi bir fiziksel aygıt geliştirilmesi sırasında kullanıma hazır olmayabilir ya da bir geliştirici kodu kaydetmeden önce bilgisayarlarında bazı tümleştirme testleri çalıştırmak isteyebilirsiniz.

Bir bilgisayarı bir Android cihazında öykünen aşağıdaki bileşenleri içerir:

* **Google Android öykünücüsü** &ndash; dayalı bir öykünücü budur [QEMU](https://www.qemu.org/) Geliştirici iş istasyonunda çalıştıran sanallaştırılmış bir cihaz oluşturur.
* **Bir öykünücü görüntünüzün** &ndash; bir _öykünücü görüntünüzün_ bir şablon veya donanım ve işletim sistemi, sanallaştırılmış amaçlanmıştır belirtimi. Örneğin, bir öykünücü görüntünüzün Google Play Hizmetleri yüklü Android 7.0 çalıştıran X içinde Nexus 5 için donanım gereksinimlerini tanımlamak. Başka bir öykünücü görüntünüzün özel Android 6.0 çalıştıran 10" tablo olabilir.
* **Android sanal cihazı (AVD)** &ndash; bir _Android sanal cihazı_ öykünücüsü görüntüden oluşturulan benzetilmiş bir Android cihazı. Çalıştıran ve Android uygulamalarını test ederken Xamarin.Android Android belirli AVD başlangıç öykünücüsü, Başlat, APK yükleyin ve ardından uygulamayı çalıştırın.

Önemli bir iyileştirme x86 üzerinde geliştirme tabanlı bilgisayarlarda zaman performans x86 için en iyi duruma getirilir özel öykünücüsü görüntülerini kullanarak elde edilebilir mimarisi ve iki sanallaştırma teknolojilerini biri:

1. Microsoft'un Hyper-V &ndash; Windows 10 Nisan güncelleştirme çalıştıran bilgisayarlarda kullanılabilir.
2. Intel'in donanım hızlandırılmış yürütme Yöneticisi'ni (HAXM) &ndash; x86 üzerinde kullanılabilir OS X, macOS veya Windows eski sürümünü çalıştıran bilgisayarlar.

Hyper-V ve HAXM, Google Android öykünücüsü hakkında daha fazla bilgi için lütfen bkz [Android öykünücüsü donanım hızlandırmasını](~/android/get-started/installation/android-emulator/hardware-acceleration.md) Kılavuzu.

> [!NOTE]
> Önceki Windows sürümlerinde HAXM Hyper-V ile uyumlu değil. Bu senaryoda ya da gerekli [Hyper-V devre dışı](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#disabling-hyper-v) veya x86 olmayan yavaş öykünücüsü görüntüleri kullanmak için en iyi duruma getirme.


<a name="device" />

### <a name="android-device"></a>Android cihazı

Test etmek için kullanılacak fiziksel bir Android cihazınız varsa, geliştirme kullanım için ayarlamak için iyi bir zamandır budur. Bkz: [ayarlamak yukarı geliştirme için cihazı](~/android/get-started/installation/set-up-device-for-development.md) geliştirme için Android Cihazınızı yapılandırmak için ardından bunu çalıştırmak ve Xamarin.Android uygulamalarında hata ayıklama için bilgisayarınıza bağlayın.


## <a name="create-an-application"></a>Uygulama oluşturma

Xamarin.Android yüklediyseniz, Visual Studio çalıştırmanızı sağlayan yeni bir proje oluşturun. Tıklatın **Dosya > Yeni > Proje** uygulamanızı oluşturmaya başlamak için:

![Yeni bir proje oluşturma](windows-images/10-new-project.png)

İçinde **yeni proje** iletişim kutusunda **Android** altında **şablonları** tıklatıp **Android uygulaması** sağ bölmede. Uygulamanız için bir ad girin (aşağıdaki ekran görüntüsünde, uygulama adlı **Uygulamam**), ardından **Tamam**:

[![Yeni Proje penceresinin Ekran iletişim kutusunda, boş bir Android uygulaması oluşturma](windows-images/11-first-app-sml.w157.png)](windows-images/11-first-app.w157.png#lightbox)

İşte bu kadar! Artık Android uygulamaları oluşturmak için Xamarin.Android kullanmaya hazırsınız!


## <a name="summary"></a>Özet

Bu makalede, ayarlama ve Xamarin.Android platform Windows üzerinde yükleme öğrendiniz (isteğe bağlı) özel Java JDK ve Android SDK yükleme konumları ile Visual Studio yapılandırma konusunda ek Android SDK yüklemek için SDK Yöneticisi'ni başlatmak nasıl bileşenleri, bir Android cihaz veya öykünücü ayarlama ve nasıl ilk uygulamanızı oluşturmaya başlayın.

Sonraki adım göz atın yapmaktır [Hello, Android](~/android/get-started/hello-android/index.md) öğreticileri çalışan bir Xamarin.Android uygulaması oluşturmayı öğrenin.


## <a name="related-links"></a>İlgili bağlantılar

- [Visual Studio indirme](https://www.visualstudio.com/vs/)
- [Xamarin için Visual Studio araçlarını yükleme](~/cross-platform/get-started/installation/windows.md)
- [Sistem Gereksinimleri](~/cross-platform/get-started/requirements.md)
- [Android SDK Kurulumu](~/android/get-started/installation/android-sdk.md)
- [Google Android Emulator](~/android/get-started/installation/android-emulator/index.md)
- [Aygıtı geliştirme için ayarlama](~/android/get-started/installation/set-up-device-for-development.md)
- [Uygulamaların Android öykünücüsünde çalıştırın](https://developer.android.com/studio/run/emulator#Requirements)
