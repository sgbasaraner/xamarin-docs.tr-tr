---
title: Android SDK Araçları yapılan değişiklikler
description: Android SDK yüklü API düzeylerini ve AVDs nasıl yönettiğini değişiklikler.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: b0d9458238c4b3ac9ceeeb7d7ce4e2ca8b0b6de3
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732872"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK Araçları yapılan değişiklikler

_Android SDK yüklü API düzeylerini ve AVDs nasıl yönettiğini değişiklikler._

## <a name="changes-to-android-sdk-tooling"></a>Android SDK Araçları yapılan değişiklikler

En son Android SDK Araçları sürümlerinde, Google varolan AVD ve SDK yöneticileri lehinde kaldıran yeni CLI (komut satırı arabirimi) araç kaldırdı. **Android** program kaldırıldı ve Google GUI (grafik kullanıcı arabirimi) yöneticileri Mac ve Xamarin için Visual Studio Araçları'nın eski sürümleri için Visual Studio artık Android SDK Araçları 25.2.5 sürümü çalışmaz. Örneğin, kullanılmaya çalışılıyor **android** program komut satırı aracılığıyla aşağıdaki gibi hata iletisine neden olur:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

Aşağıdaki bölümler Android SDK ve Android SDK 25.3.0 kullanarak Android sanal aygıtların nasıl yönetileceğini açıklar ve daha sonra.

### <a name="ui-tools"></a>UI araçları

Visual Studio ve Mac için Visual Studio şimdi devam etmeyen Google GUI tabanlı yöneticileri Xamarin donanımlarının sağlar:

-   Android SDK Araçları, platformlar ve Xamarin.Android uygulamaları geliştirmek için gereken diğer bileşenleri indirmek için kullanacağınız [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md) yerine eski Google SDK Yöneticisi.

-   Oluşturma ve Android sanal cihaz yapılandırmak için kullanın [Android Aygıt Yöneticisi'ni](~/android/get-started/installation/android-emulator/device-manager.md) yerine eski Google öykünücüsü yöneticisi.

Bu araçlar Google GUI tabanlı için işlevsel olarak eşdeğerdir yöneticileri bunlar değiştirin.

### <a name="cli-tools"></a>CLI araçlarını

Alternatif olarak, yönetmek ve Öykünücüler ve Android SDK güncelleştirmek için CLI araçlarını kullanabilirsiniz. Aşağıdaki programlar artık Android SDK Araçları için komut satırı arabirimi oluşturan:

#### <a name="sdkmanager"></a>sdkmanager

**Eklenmiş:** Android SDK Araçları 25.2.3 (Kasım 2016) ve üzeri.

Adlı yeni bir program var. **sdkmanager** içinde **Araçlar/bin** klasör, Android SDK'ın. Bu araç, komut satırında Android SDK korumak için kullanılır. Bu aracı kullanmayla ilgili daha fazla bilgi için bkz: [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Eklenmiş:** Android SDK Araçları 25.3.0 (Mart 2017) ve üzeri.

Adlı yeni bir program var. **avdmanager** içinde **Araçlar/bin** klasör, Android SDK'ın. Bu araç, Google Android öykünücüsü AVDs korumak için kullanılır. Bu aracı kullanmayla ilgili daha fazla bilgi için bkz: [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Önceki sürüme indirme

Düşürmek, **Android SDK Araçları** Android SDK'ın önceki bir sürümünü yükleme sürümünün [Android Geliştirici Web sitesi](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Eski GUI kullanarak

Özgün GUI çalıştırarak kullanmaya devam edebilirsiniz **android** içinde programı, **Araçları** klasörü üzerinde olduğu sürece **Android SDK Araçları** sürüm **25.2.5**  veya daha düşük.


## <a name="related-links"></a>İlgili bağlantılar

- [Android SDK Kurulumu](~/android/get-started/installation/android-sdk.md)
- [Android cihaz Yöneticisi](~/android/get-started/installation/android-emulator/device-manager.md)
- [Android API düzeylerini anlama](~/android/app-fundamentals/android-api-levels.md)
- [Sürüm Notları (Google) SDK Araçları](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
