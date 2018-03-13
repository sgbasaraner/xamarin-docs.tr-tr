---
title: "Bir öykünücü üzerinde Android yıpranması hata ayıklama"
description: "Bu makaleler, bir Xamarin.Android yıpranması uygulaması bir öykünücü üzerinde hata ayıklama açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: dc6146670c4b5f7d29491335b4719e032487dcce
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="debug-android-wear-on-an-emulator"></a>Bir öykünücü üzerinde Android yıpranması hata ayıklama

_Bu makaleler, bir Xamarin.Android yıpranması uygulaması bir öykünücü üzerinde hata ayıklama açıklanmaktadır._

## <a name="debug-wear-on-emulator-overview"></a>Öykünücü genel bakış üzerinde yıpranması hata ayıklama

Android takmak uygulamaları geliştirme uygulama ya da fiziksel donanım üzerinde çalışan veya bir öykünücü veya benzetici kullanarak gerektirir. Donanım kullanarak en iyi yaklaşımı, ancak her zaman en kullanışlı değildir. Çoğu durumda, daha basit ve daha düşük maliyetli bir öykünücü aşağıda açıklandığı gibi kullanarak Android takmak donanım benzetimini/benzetmek için olabilir. Yerel Yönetici değilseniz henüz Android takmak uygulamaları dağıtma ve çalıştırma işlemi bilmiyorsanız bkz [Merhaba, takmak](~/android/wear/get-started/hello-wear.md).

## <a name="configure-the-android-sdk-emulator"></a>Android SDK öykünücüsü yapılandırın

Bir öykünücü üzerinde yıpranması uygulamanızı çalıştırmak için Android SDK'sı Android öykünücüsü yükleyip Android takmak için yapılandırmanız gerekir. Genel Android SDK öykünücüsü yükleme ve yapılandırma bilgileri için bkz: [Android SDK öykünücüsü](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Yıpranması sanal cihazı oluşturduğunuzda, bir Android takmak cihaz profilini seçin (gibi **Android yıpranması kare**). Geliştirilmiş performans için yıpranması kullanmak **x86** CPU/ABI Bu örnekte görüldüğü gibi:

[![Örnek yıpranması sanal aygıt yapılandırma](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)


## <a name="launch-the-wear-virtual-device"></a>Yıpranması sanal cihazı başlatma 

Android takmak sanal aygıt oluşturduktan sonra hata ayıklama başlamadan önce IDE aygıtı aşağı açılır menüde seçebilirsiniz. Sanal cihazınız aygıt aşağı açılır kullanılabilir durumda değilse, projenizin bir Android olduğundan emin olun *takmak* uygulama projesi (Android uygulaması projesi değil) ve API düzeyini hedefine aynı API'sine ayarlanır düzey sanal cihazı olarak. Örneğin:

[![Visual Studio aygıt menüde takmak AVD seçme](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Android öykünücüsünde başladıktan sonra Xamarin.Android öykünücüsünü yıpranması uygulamayı dağıtın. Öykünücü ile yapılandırılmış sanal cihaz görüntüsü uygulamayı çalıştırır.

Bu görürseniz şaşırmayın (veya başka bir Interstitial ekran) ilk. Gözcü öykünücüsü başlatılması biraz zaman alabilir: 

![Yalnızca bir dakika öykünücüsü görüntüler izleme...](debug-on-emulator-images/please-wait.png)

Öykünücü çalıştıran kalmış olabilir; kapatmak ve uygulama her çalıştırıldığında yeniden başlatmak gerekli değildir.

 
## <a name="summary"></a>Özet
 
Bu kılavuz, Android SDK öykünücüsü yıpranması geliştirme için yapılandırmak ve hata ayıklama için yıpranması sanal cihazı başlatma anlatılmıştır.
