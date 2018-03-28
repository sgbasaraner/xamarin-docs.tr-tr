---
title: Android SDK Araçları yapılan değişiklikler
description: Android SDK yüklü API düzeylerini ve AVDs nasıl yönettiğini değişiklikler.
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: a16aa3704d9e0a63cfabde4b620452e7e2a5bf57
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="changes-to-the-android-sdk-tooling"></a>Android SDK Araçları yapılan değişiklikler

_Android SDK yüklü API düzeylerini ve AVDs nasıl yönettiğini değişiklikler._

## <a name="changes-to--android-sdk-tooling"></a>Android SDK Araçları yapılan değişiklikler

Modern Android SDK Araçları sürümlerinde, Google varolan AVD ve SDK yöneticileri yeni CLI (komut satırı arabirimi) araç düşünülerek kullanılmaz kaldırdı. Eski **android** program kaldırıldı ve GUI (grafik kullanıcı arabirimi) yöneticileri Mac ve Visual Studio için Xamarin eski sürümleri için Visual Studio artık Android SDK Araçları 25.2.5 sürümü çalışmaz.


![Visual Studio'da Android IDE menüsü](sdk-cli-tooling-changes-images/android-ide-menu.png)

Kullanılmaya çalışılıyor **android** program komut satırı aracılığıyla aşağıdaki gibi hata iletisine neden olur:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

Bu nedenle yönetmek ve Öykünücüler ve Android SDK güncelleştirmek için CLI araçlarını kullanmanız gerekir.

### <a name="cli-tools"></a>CLI araçlarını

Aşağıdaki programlar artık Android SDK Araçları için komut satırı arabirimi oluşturan:

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
- [Android API düzeylerini anlama](~/android/app-fundamentals/android-api-levels.md)
- [Sürüm Notları (Google) SDK Araçları](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
