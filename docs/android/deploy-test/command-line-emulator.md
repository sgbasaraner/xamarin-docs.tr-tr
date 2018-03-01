---
title: "Komut satırı öykünücüsü"
ms.topic: article
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: b8e8ec3de3afccab15d54f666974af678c1b8c33
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="command-line-emulator"></a>Komut satırı öykünücüsü


## <a name="running-the-android-emulator-from-the-command-line"></a>Android öykünücüsünde komut satırından çalıştırma

Android öykünücüsünde komut satırından çalıştırma etkinleştirmek için Android SDK'sı tarafından sağlanan "öykünücü" aracını kullanabilirsiniz. Bu araç, OS X terminalde veya komut satırından bir Windows makinesinde öykünücü çalıştırmak için kullanılabilir.

Belirli bir Android öykünücüsü başlatmak için android SDK konuma (örneğin, C:\android-sdk-windows\tools) Araçlar dizininden aşağıdaki komutu çalıştırın:

Windows üzerinde

```cmd
emulator.exe -avd NameOfYourEmulator -partition-size 512
```

MacOS üzerinde

```bash
./emulator -avd NameOfYourEmulator -partition-size 512
```

Bölüm boyutu gerek nedeni yeterince alan varsayılan öykünücü boyutu küçüktür gibi öykünücüsünde yüklü Xamarin.Android platform almak öykünücü izin vermektir.

Burada - Android sitesinde ek parametreler hakkında daha fazla bilgi bulabilirsiniz [http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html)
