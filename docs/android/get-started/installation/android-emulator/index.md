---
title: Android öykünücüsünde Kurulumu
description: Android öykünücüsünde farklı cihaz benzetimini yapmak için yapılandırmaları çeşitli çalıştırabilirsiniz. Bu kılavuz, uygulamanızı test etmek için Android öykünücüsünde hazırlamak nasıl açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: f281227ae6ee17548e9c4653d52c7ae6d2bfff2d
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935041"
---
# <a name="android-emulator-setup"></a>Android öykünücüsünde Kurulumu

_Bu kılavuz, uygulamanızı test etmek için Android öykünücüsünde hazırlamak nasıl açıklanmaktadır._


## <a name="overview"></a>Genel Bakış

Android öykünücüsünde farklı cihaz benzetimini yapmak için yapılandırmaları çeşitli çalıştırabilirsiniz. Her yapılandırma adlı bir _sanal aygıt_. Dağıtma ve uygulamanızı öykünücü üzerinde test Nexus veya piksel telefonu gibi fiziksel bir Android cihazı benzetim önceden yapılandırılmış veya özel sanal cihazı seçin.

Aşağıda listelenen bölümleri en yüksek performans için Android öykünücüsü hızlandırmak nasıl, Android Aygıt Yöneticisi'ni oluşturmak ve sanal aygıtların özelleştirmek için nasıl kullanılacağı ve bir sanal cihaz profil özelliklerini özelleştirmek nasıl açıklanmaktadır. Ayrıca, bir sorun giderme bölümü, ortak öykünücüsü sorunlar ve geçici çözümler açıklanmaktadır.

## <a name="sections"></a>Bölümler

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Donanım hızlandırma öykünücüsü performansı](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

En yüksek Android öykünücüsü performansı için bilgisayarınız hazırlamak nasıl.
Android öykünücüsünde donanım hızlandırmasını şekilde basımı karşılamayacak kadar yavaş olabilir çünkü öykünücü kullanmadan önce bilgisayarınızda donanım hızlandırmasını etkinleştirmenizi öneririz.

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Sanal cihaz Android cihaz Yöneticisi ile yönetme](~/android/get-started/installation/android-emulator/device-manager.md)

Android Aygıt Yöneticisi'ni oluşturmak ve sanal aygıtların özelleştirmek için nasıl kullanılacağını.

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Android sanal cihazı özelliklerini düzenleme](~/android/get-started/installation/android-emulator/device-properties.md)

Sanal cihazı profil özelliklerini düzenlemek için Android Aygıt Yöneticisi'ni kullanma

### <a name="android-emulator-troubleshootingandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[Sorun giderme android öykünücüsü](~/android/get-started/installation/android-emulator/troubleshooting.md)

Bu makalede, en yaygın uyarı iletilerini ve Android öykünücüsünde çalıştırılırken oluşan sorunları, geçici çözümler ve ipuçları ile birlikte açıklanmaktadır.

Android öykünücüsünde yapılandırdıktan sonra bkz: [Android öykünücüsünde hata ayıklama](~/android/deploy-test/debugging/debug-on-emulator.md) öykünücü başlatma ve test ve hata ayıklama, uygulamanız için kullanma hakkında bilgi için.


> [!NOTE]
> Android SDK Araçları sürüm itibariyle **26.0.1** ve daha sonra Google lehinde kaldıran yeni (komut satırı arabirimi) CLI araçlarını varolan AVD/SDK yöneticileri için destek kaldırdı. Bu kullanımdan değişikliği nedeniyle Xamarin SDK/cihaz yöneticileri artık Google SDK/cihaz yöneticileri yerine Android araçları 26.0.1 için kullanılır ve daha sonra. Xamarin SDK Yöneticisi hakkında daha fazla bilgi için bkz: [Xamarin.Android için Android SDK'sı ayarı](~/android/get-started/installation/android-sdk.md).

