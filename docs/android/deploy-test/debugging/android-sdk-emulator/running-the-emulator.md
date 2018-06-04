---
title: Google Android öykünücüsü çalıştırma
description: Google Android öykünücüsü uygulamanızla hata ayıklama
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 0f4a8dc2ad581dac71f76d8dd1de09534b63abaa
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732313"
---
# <a name="running-the-google-android-emulator"></a>Google Android öykünücüsü çalıştırma

_Bu kılavuzda, bir hata ayıklama ve uygulamanızı test etmek için Google Android öykünücüsü sanal cihazı başlatma öğreneceksiniz._

## <a name="using-a-pre-configured-virtual-device"></a>Önceden yapılandırılmış bir sanal cihaz kullanımıyla

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio görünür önceden yapılandırılmış sanal cihazlar cihaz açılır menüde içerir. Örneğin, aşağıdaki Visual Studio 2017 ekran görüntüsünde, birkaç önceden yapılandırılmış sanal cihaz kullanılabilir:

-   **Visual Studio\_android 23\_arm\_telefon**

-   **Visual Studio\_android 23\_arm\_tablet**

-   **Visual Studio\_android 23\_x86\_telefon** 

-   **Visual Studio\_android 23\_x86\_tablet** 

[![Sanal cihazlar](running-the-emulator-images/win/01-virtual-devices-sml.png)](running-the-emulator-images/win/01-virtual-devices.png#lightbox)

Genellikle, seçeceğiniz **Visual Studio\_android 23\_x86\_telefon** sınamak ve telefon uygulama hatalarını ayıklamak için sanal cihazı. Bu önceden yapılandırılmış sanal cihazlar birini gereksinimlerinizi karşılayıp karşılamadığını (yani, uygulamanızın hedefine API düzeyi eşleşir) geçin [öykünücü başlatma](#launching) uygulamanızı öykünücüde çalıştırmaya başlamak için. (Android API düzeyleriyle hakkında bilgi sahibi değilseniz, bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).)

Xamarin.Android projenizi kullanılabilir sanal cihazlar ile uyumsuz bir hedef çerçeve düzeyi kullanıyorsanız, aşağı açılan menüsünün altında kullanılamaz sanal aygıtlarını listeler **desteklenmeyen aygıtları**. Örneğin, aşağıdaki proje ayarlamak bir hedef Framework sahip **Android 7.1 Nougat (API 25)**, ile uyumsuz olduğu **Android 6.0** Bu örnekte listelenen sanal aygıtlar:

[![Uyumsuz sanal cihaz](running-the-emulator-images/win/02-incompatible-level-sml.png)](running-the-emulator-images/win/02-incompatible-level.png#lightbox)

Tıklayabilirsiniz **Minimum Android hedef değiştirmek** proje değiştirmek için kullanıcının Minimum Android sürümü böylece kullanılabilir sanal cihazlar API düzeyini eşleşir. Alternatif olarak, kullanabileceğiniz [Android Aygıt Yöneticisi'ni](~/android/get-started/installation/android-emulator/device-manager.md) API hedef destekleyen yeni sanal aygıtlar düzeyi oluşturmak için.
Sanal cihazlar için yeni bir API düzeyi yapılandırmadan önce bu API düzeyi için karşılık gelen sistem görüntüleri önce yüklemeniz gerekir (bkz [Xamarin.Android için Android SDK'sı ayarı](~/android/get-started/installation/android-sdk.md)).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio aygıt açılır menüde görünür önceden yapılandırılmış sanal aygıtları içerir. Örneğin, aşağıdaki ekran görüntüsünde, iki önceden yapılandırılmış sanal cihazlar kullanılabilir:

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![Sanal cihazlar](running-the-emulator-images/mac/01-virtual-devices-sml.png)](running-the-emulator-images/mac/01-virtual-devices.png#lightbox)

Genellikle, seçeceğiniz **Android\_hızlandırılmış\_x86** sınamak ve telefon uygulama hatalarını ayıklamak için sanal cihazı. Bu sanal önceden yapılandırılmışsa aygıt gereksinimlerinize uygun (yani, uygulamanızın hedefine API düzeyi eşleşir) geçin [öykünücü başlatma](#launching) uygulamanızı öykünücüde çalıştırmaya başlamak için. (Android API düzeyleriyle hakkında bilgi sahibi değilseniz, bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="editing-virtual-devices"></a>Sanal cihazlar düzenleme

Sanal cihazlar değiştirin (veya yenilerini oluşturun), kullanmalısınız [Android Aygıt Yöneticisi'ni](~/android/get-started/installation/android-emulator/device-manager.md).


<a name="launching" />

## <a name="launching-the-emulator"></a>Öykünücü başlatma

Visual Studio üstüne seçmek için kullanılan bir açılır menü olduğundan **hata ayıklama** veya **sürüm** modu. Seçme **hata ayıklama** uygulama başladıktan sonra öykünücüsü içinde çalışan uygulama işlemi eklemek hata ayıklayıcı neden olur. Seçme **sürüm** modunu (ancak hala çalıştırabilirsiniz hata ayıklama için uygulama ve kullanım günlük deyimleri) hata ayıklayıcı devre dışı bırakır. Aygıt açılır menüsünden sanal cihazı seçtikten sonra seçin **hata ayıklama** veya **sürüm** modu, ardından uygulamayı çalıştırmak için Oynat düğmesini tıklatın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Hata ayıklama ve yayın modları, Yürüt düğmesine](running-the-emulator-images/win/17-debug-release-sml.png)](running-the-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Hata ayıklama ve yayın modları, Yürüt düğmesine](running-the-emulator-images/mac/16-debug-release-sml.png)](running-the-emulator-images/mac/16-debug-release.png#lightbox)

-----

Öykünücü başladıktan sonra Xamarin.Android öykünücüsünü uygulamayı dağıtın. Öykünücü ile yapılandırılmış sanal cihaz görüntüsü uygulamayı çalıştırır. Google Android öykünücüsü, örnek bir ekran görüntüsü aşağıda görüntülenmektedir. Bu örnekte, öykünücü adlı boş bir uygulamayı çalıştıran **Uygulamam**:

![Boş bir uygulaması çalıştıran öykünücüsü](running-the-emulator-images/emulator-running.png)

Öykünücü çalıştıran kalabilir: kapatmak için gerekli değildir ve bekleme süresi, uygulama her zaman yeniden başlatıldığında. Bir Xamarin.Android uygulaması öykünücüsünde ilk çalıştırıldığında uygulama tarafından izlenen targetted API düzeyi için paylaşılan Xamarin.Android çalışma zamanı yüklendi. Çalışma zamanı yükleme birkaç dakika sürebilir, bu nedenle lütfen bekleyin. Yalnızca ilk Xamarin.Android uygulaması öykünücüsünü dağıtıldığında çalışma zamanı yükleme kurulur &ndash; yalnızca uygulama öykünücüsünü kopyaladığınızdan sonraki dağıtımları daha hızlı.

## <a name="quick-boot"></a>Hızlı önyükleme

Google Android öykünücüsü daha yeni sürümleri dahildir adlı bir özelliği _hızlı önyükleme_ , yalnızca birkaç saniye içinde öykünücü başlatır. Öykünücü kapattığınızda, böylece yeniden başlatıldığında, hızlı bir şekilde bu durumundan geri yüklenebilir sanal cihaz durumunun bir anlık görüntüsünü alır.
Bu özelliğe erişmek için aşağıdakiler gerekir:

-   Android öykünücüsünde sürüm 27.0.2 veya daha yenisi
-   Android SDK Araçları sürüm 26.1.1 veya daha yenisi

SDK Araçları ve öykünücüsü yukarıda listelenen sürümleri yüklendiğinde, hızlı önyükleme özelliği varsayılan olarak etkindir. 

Bir anlık görüntüsü henüz oluşturulmamış olduğundan sanal cihazın ilk soğuk önyükleme hızlı geliştirme olmadan gerçekleşir:

![Soğuk önyükleme ekran görüntüsü](running-the-emulator-images/cold-boot.png)

Öykünücü dışına çıktığınızda hızlı önyükleme anlık görüntüde öykünücü durumunu kaydeder:

![Kapatma işlemi durumu kaydediyor](running-the-emulator-images/saving-state.png)

Öykünücü yalnızca öykünücü kapalı durumu geri yüklemesi nedeniyle sonraki sanal cihaz başlatılır çok daha hızlıdır.

![Yeniden başlatma durumuna yükleniyor](running-the-emulator-images/loading-state.png)

Google Android öykünücüsü kullanma hakkında daha fazla bilgi için aşağıdaki Android Geliştirici konulara bakın:

-   [Ekranda gezinme](https://developer.android.com/studio/run/emulator.html#navigate)

-   [Öykünücüde temel görevleri gerçekleştirme](https://developer.android.com/studio/run/emulator.html#tasks)

-   [Genişletilmiş denetimler, ayarları ve Yardım ile çalışma](https://developer.android.com/studio/run/emulator.html#extended)

-   [Öykünücü ile hızlı önyükleme çalıştırın](https://developer.android.com/studio/run/emulator#quickboot)

