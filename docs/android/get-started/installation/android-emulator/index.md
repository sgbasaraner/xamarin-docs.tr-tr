---
title: "Android öykünücüsünde Kurulumu"
description: "Bu bölümde, uygulamanızı test etmek için Android SDK öykünücüsü hazırlama açıklar. En yüksek performans için öykünücüsü hızlandırmak nasıl açıklar ve bir öykünücü yöneticisini oluşturmak ve sanal aygıtların özelleştirmek için nasıl kullanılacağını gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 55f5cf22718713fdcf11c49e0993f47c2f5a6f1d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="android-emulator-setup"></a>Android öykünücüsünde Kurulumu

_Bu bölümde, uygulamanızı test etmek için Android SDK öykünücüsü hazırlama açıklar. En yüksek performans için öykünücüsü hızlandırmak nasıl açıklar ve bir öykünücü yöneticisini oluşturmak ve sanal aygıtların özelleştirmek için nasıl kullanılacağını gösterir._


## <a name="overview"></a>Genel Bakış

Google Android SDK öykünücüsü farklı cihaz benzetimini yapmak için yapılandırmaları çeşitli çalıştırabilirsiniz. Bu yapılandırmalar her biri olarak oluşturulan bir _sanal aygıt_. Bu kılavuzda daha iyi performans için Android öykünücüsü hızlandırmak ve Xamarin Android öykünücü yöneticisini veya eski Google öykünücü yöneticisini sanal cihaz oluşturmak için nasıl kullanılacağını öğreneceksiniz.


> [!NOTE]
> Android SDK Araçları sürüm itibariyle **26.0.1** ve daha sonra Google lehinde kaldıran yeni (komut satırı arabirimi) CLI araçlarını varolan AVD/SDK yöneticileri için destek kaldırdı. Bu kullanımdan değişikliği nedeniyle Xamarin SDK/cihaz yöneticileri artık Google SDK/öykünücü yöneticileri yerine Android araçları 26.0.1 için kullanılır ve daha sonra. (Xamarin SDK Yöneticisi hakkında daha fazla bilgi için bkz: [Android SDK Kurulum](~/android/get-started/installation/android-sdk.md)).


## <a name="sections"></a>Bölümler

### <a name="hardware-accelerationandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Donanım Hızlandırma](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

En yüksek Android SDK öykünücüsü performansı için bilgisayarınız hazırlamak nasıl. Android SDK öykünücüsü donanım hızlandırmasını şekilde basımı karşılamayacak kadar yavaş olabileceğinden, Android SDK öykünücüsü kullanmadan önce bilgisayarınızda donanım hızlandırmasını etkinleştirmenizi öneririz.

### <a name="xamarin-android-device-managerandroidget-startedinstallationandroid-emulatorxamarin-device-managermd"></a>[Xamarin Android cihaz Yöneticisi](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

Xamarin Android Aygıt Yöneticisi'ni oluşturmak ve Android SDK öykünücüsü sanal cihazlar özelleştirmek için nasıl kullanılacağını. **Xamarin Android Aygıt Yöneticisi'ni**, şu anda Önizleme'de, eski Google öykünücü yöneticisini değiştirmek için tasarlanmıştır. Android Oreo 8.0 veya üstünü hedefliyorsanız, Google öykünücü yöneticisini yerine Xamarin Android Aygıt Yöneticisi'ni kullanmanız gerekir.

### <a name="google-emulator-managerandroidget-startedinstallationandroid-emulatorgoogle-emulator-managermd"></a>[Google Öykünücü Yöneticisi](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

Eski Google öykünücü yöneticisini oluşturmak ve Android SDK öykünücüsü sanal cihazlar özelleştirmek için nasıl kullanılacağını. Google Android öykünücüsü Android SDK Araçları sürüm 25.2.5 kalan tarafından özgün Google Öykünücüsü Yöneticisi'ni ile çalıştırmak veya alt devam edebilirsiniz.

Android SDK öykünücüsü yapılandırdıktan sonra bkz: [Android SDK öykünücüsü](~/android/deploy-test/debugging/android-sdk-emulator/index.md) öykünücü başlatma ve test ve hata ayıklama, uygulamanız için kullanma hakkında bilgi için.
