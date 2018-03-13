---
title: "Komut satırı öykünücüsü"
ms.topic: article
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 01ae4e1477ff5a05a5690ef24ed266b73f862748
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
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
