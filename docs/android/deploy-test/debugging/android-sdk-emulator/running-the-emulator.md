---
title: Google Android öykünücüsü çalıştırma
description: Google Android öykünücüsü uygulamanızla hata ayıklama
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0e290b24c0d7a98b1abaf647fe76e56867042645
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="running-the-google-android-emulator"></a>Google Android öykünücüsü çalıştırma

Bu kılavuzda, hata ayıklama ve uygulamanızı test etme için Google Android öykünücüsü sanal bir aygıt başlatmak öğreneceksiniz.

## <a name="using-a-pre-configured-virtual-device"></a>Önceden yapılandırılmış bir sanal cihaz kullanımıyla

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio görünür önceden yapılandırılmış sanal cihazlar cihaz açılır menüde içerir. Örneğin, aşağıdaki Visual Studio 2017 ekran görüntüsünde, birkaç önceden yapılandırılmış sanal cihaz kullanılabilir:

-   **Visual Studio\_android 23\_arm\_telefon**

-   **Visual Studio\_android 23\_arm\_tablet**

-   **Visual Studio\_android 23\_x86\_telefon** 

-   **Visual Studio\_android 23\_x86\_tablet** 

[![Sanal cihazlar](running-the-emulator-images/win/01-virtual-devices-sml.png)](running-the-emulator-images/win/01-virtual-devices.png#lightbox)

Genellikle, seçeceğiniz **Visual Studio\_android 23\_x86\_telefon** sınamak ve telefon uygulama hatalarını ayıklamak için sanal cihazı. Bu önceden yapılandırılmış sanal cihazlar birini gereksinimlerinizi karşılayıp karşılamadığını (yani, uygulamanızın hedefine API düzeyi eşleşir) geçin [öykünücü başlatma](#launching) uygulamanızı öykünücüde çalıştırmaya başlamak için. (Android API düzeyleriyle hakkında bilgi sahibi değilseniz, bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).)

Xamarin.Android projenizi kullanılabilir sanal cihazlar ile uyumsuz bir hedef çerçeve düzeyi kullanıyorsanız, aşağı açılan menüden altında kullanılamaz sanal cihazlar listesinde **desteklenmeyen aygıtları**. Örneğin, aşağıdaki proje ayarlamak bir hedef Framework sahip **Android 7.1 Nougat (API 25)**, ile uyumsuz olduğu **Android 6.0** varsayılan olarak sağlanan sanal aygıtlar:

[![Uyumsuz sanal cihaz](running-the-emulator-images/win/02-incompatible-level-sml.png)](running-the-emulator-images/win/02-incompatible-level.png#lightbox)

Tıklayabilirsiniz **Minimum Android hedef değiştirmek** proje değiştirmek için kullanıcının Minimum Android sürümü böylece kullanılabilir sanal cihazlar API düzeyini eşleşir. Alternatif olarak, kullanabileceğiniz **Android Emulator Manager** API hedef destekleyen yeni sanal cihazları daha sonra açıklandığı gibi düzeyi oluşturmak için [sanal aygıtları yapılandırma](#virtualdevice). Sanal cihazlar için yeni bir API düzeyi yapılandırmadan önce bu API düzeyi için karşılık gelen sistem görüntüleri önce yüklemelisiniz &ndash; bu sonraki bölümde anlatılmıştır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio aygıt açılır menüde görünür önceden yapılandırılmış sanal aygıtları içerir. Örneğin, aşağıdaki ekran görüntüsünde, iki önceden yapılandırılmış sanal cihazlar kullanılabilir:

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![Sanal cihazlar](running-the-emulator-images/mac/01-virtual-devices-sml.png)](running-the-emulator-images/mac/01-virtual-devices.png#lightbox)

Genellikle, seçeceğiniz **Android\_hızlandırılmış\_x86** sınamak ve telefon uygulama hatalarını ayıklamak için sanal cihazı. Bu sanal önceden yapılandırılmışsa aygıt gereksinimlerinize uygun (yani, uygulamanızın hedefine API düzeyi eşleşir) geçin [öykünücü başlatma](#launching) uygulamanızı öykünücüde çalıştırmaya başlamak için. (Android API düzeyleriyle hakkında bilgi sahibi değilseniz, bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="creating-custom-virtual-devices"></a>Özel sanal cihaz oluşturma

Özel sanal cihaz oluşturmak için Xamarin Android Aygıt Yöneticisi'ni veya eski Google öykünücüsü Android SDK'ın bir parçası olan Yöneticisi'ni kullanmanız gerekir. Oluşturma ve sanal aygıtların özelleştirme hakkında daha fazla bilgi için bkz: [Xamarin Android Aygıt Yöneticisi'ni](~/android/get-started/installation/android-emulator/xamarin-device-manager.md).
Eski Google öykünücü yöneticisini kullanmayı tercih ederseniz, bkz: [Google öykünücü yöneticisini](~/android/get-started/installation/android-emulator/google-emulator-manager.md).

İçin Android 8.0 Oreo geliştiriyorsanız, Xamarin Android Aygıt Yöneticisi'ni kullanmanız gerektiğini unutmayın.

<a name="launching" />

## <a name="launching-the-emulator"></a>Öykünücü başlatma

IDE üstüne seçmek için kullanılan bir açılır menü olduğundan **hata ayıklama** veya **sürüm** modu. Seçme **hata ayıklama** öykünücüsü içinde çalışan uygulama işlemi için hata ayıklayıcı ekler. 

Aygıt açılır menüsünden sanal cihazı seçtikten sonra şunlardan birini seçin **hata ayıklama** veya **sürüm** modu, ardından **yürütmek** uygulamayı çalıştırmak için düğmesi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Hata ayıklama ve yayın modları, Yürüt düğmesine](running-the-emulator-images/win/17-debug-release-sml.png)](running-the-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Hata ayıklama ve yayın modları, Yürüt düğmesine](running-the-emulator-images/mac/16-debug-release-sml.png)](running-the-emulator-images/mac/16-debug-release.png#lightbox)

-----

Android öykünücüsünde başladıktan sonra Xamarin.Android öykünücüsünü uygulamayı dağıtın. Öykünücü ile yapılandırılmış sanal cihaz görüntüsü uygulamayı çalıştırır. Google Android öykünücüsü, örnek bir ekran görüntüsü aşağıda gösterilen (öykünücü adlı boş bir uygulamayı çalıştıran **Uygulamam**):

![Boş bir uygulaması çalıştıran öykünücüsü](running-the-emulator-images/emulator-running.png)

Öykünücü çalıştıran kalmış olabilir; kapatmak ve uygulama her çalıştırıldığında yeniden başlatmak gerekli değildir. Bir Xamarin.Android uygulaması öykünücüsünde ilk çalıştırıldığında uygulama tarafından izlenen targetted API düzeyi için paylaşılan Xamarin.Android çalışma zamanı yüklendi. Çalışma zamanı yükleme birkaç dakika sürebilir, bu nedenle lütfen bekleyin. Yalnızca ilk Xamarin.Android uygulaması öykünücüsünü dağıtıldığında çalışma zamanı yükleme kurulur &ndash; yalnızca uygulama öykünücüsünü kopyaladığınızdan sonraki dağıtımları daha hızlı.

Google Android öykünücüsü kullanma hakkında daha fazla bilgi için aşağıdaki Android Geliştirici konulara bakın:

-   [Ekranda gezinme](https://developer.android.com/studio/run/emulator.html#navigate)

-   [Öykünücüde temel görevleri gerçekleştirme](https://developer.android.com/studio/run/emulator.html#tasks)

-   [Genişletilmiş denetimler, ayarları ve Yardım ile çalışma](https://developer.android.com/studio/run/emulator.html#extended)

