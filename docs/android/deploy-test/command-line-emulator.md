---
title: Komut satırı öykünücüsü
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: b1ca1c2b441a9c9ca26de5668f318312bea156a3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762077"
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

Burada Android sitesinde ek parametreler hakkında daha fazla bilgi bulabilirsiniz- [http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html)
