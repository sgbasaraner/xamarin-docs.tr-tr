---
title: Google Android öykünücüsü Kurulumu
description: Google Android öykünücüsü farklı cihaz benzetimini yapmak için yapılandırmaları çeşitli çalıştırabilirsiniz. Bu kılavuz, uygulamanızı test etmek için Android öykünücüsünde hazırlamak nasıl açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: e5ba2cc23ea9751ca60644d3eb5b7e3f31bbb6bb
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732537"
---
# <a name="google-android-emulator-setup"></a>Google Android öykünücüsü Kurulumu

_Bu kılavuz, Google Android öykünücüsü uygulamanızı test etmek için hazırlamak nasıl açıklanmaktadır._


## <a name="overview"></a>Genel Bakış

Google Android öykünücüsü farklı cihaz benzetimini yapmak için yapılandırmaları çeşitli çalıştırabilirsiniz. Her yapılandırma adlı bir _sanal aygıt_. Dağıtma ve uygulamanızı öykünücü üzerinde test Nexus veya piksel telefonu gibi fiziksel bir Android cihazı benzetim önceden yapılandırılmış veya özel sanal cihazı seçin.

Aşağıda listelenen bölümleri en yüksek performans için Google Android öykünücüsü hızlandırmak nasıl, Android Aygıt Yöneticisi'ni oluşturmak ve sanal aygıtların özelleştirmek için nasıl kullanılacağı ve bir sanal cihaz profil özelliklerini özelleştirmek nasıl açıklanmaktadır. Ayrıca, bir sorun giderme bölümü, ortak kurulum sorunlarını ve geçici çözümler açıklanmaktadır.

## <a name="sections"></a>Bölümler

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Donanım hızlandırma öykünücüsü performansı](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

En yüksek Android öykünücüsü performansı için bilgisayarınız hazırlamak nasıl.
Google Android öykünücüsü donanım hızlandırmasını şekilde basımı karşılamayacak kadar yavaş olabileceğinden, bu öykünücüsü kullanmadan önce bilgisayarınızda donanım hızlandırmasını etkinleştirmenizi öneririz.

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Sanal cihaz Android cihaz Yöneticisi ile yönetme](~/android/get-started/installation/android-emulator/device-manager.md)

Android Aygıt Yöneticisi'ni oluşturmak ve sanal aygıtların özelleştirmek için nasıl kullanılacağını.

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Android sanal cihazı özelliklerini düzenleme](~/android/get-started/installation/android-emulator/device-properties.md)

Bir Android sanal cihazı profil özelliklerini düzenlemek için Android Aygıt Yöneticisi'ni kullanma

### <a name="troubleshooting-emulator-setup-problemsandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[Öykünücü kurulum sorunlarını giderme](~/android/get-started/installation/android-emulator/troubleshooting.md)

Android öykünücüsünde ayarlarken oluşabilir Android Aygıt Yöneticisi'ni sorunlarını tanılamak ve düzeltmek nasıl.


Android öykünücüsünde yapılandırdıktan sonra bkz: [Google Android öykünücüsü ile hata ayıklama](~/android/deploy-test/debugging/android-sdk-emulator/index.md) öykünücü başlatma ve test ve hata ayıklama, uygulamanız için kullanma hakkında bilgi için.


> [!NOTE]
> Android SDK Araçları sürüm itibariyle **26.0.1** ve daha sonra Google lehinde kaldıran yeni (komut satırı arabirimi) CLI araçlarını varolan AVD/SDK yöneticileri için destek kaldırdı. Bu kullanımdan değişikliği nedeniyle Xamarin SDK/cihaz yöneticileri artık Google SDK/cihaz yöneticileri yerine Android araçları 26.0.1 için kullanılır ve daha sonra. Xamarin SDK Yöneticisi hakkında daha fazla bilgi için bkz: [Android SDK Kurulum](~/android/get-started/installation/android-sdk.md).

