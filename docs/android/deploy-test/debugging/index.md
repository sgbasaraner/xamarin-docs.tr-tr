---
title: Cihazlar ve Öykünücüler Xamarin.Android hata ayıklama
description: Test ve Xamarin.Android uygulamanızın hatalarını ayıklama
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 4b51b10965f3f6e77420d657bc0f7f7d25fc6569
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="debugging"></a>Hata Ayıklama

Bu bölümde, bir Xamarin.Android uygulaması veya Öykünücüler cihazlarda hata ayıklamak nasıl anlatılmaktadır.
## <a name="debugging-overview"></a>Hata Ayıklamaya Genel Bakış

Android uygulamaları geliştirme uygulama ya da fiziksel donanım üzerinde çalışan veya bir öykünücü kullanarak gerektirir. Donanım kullanarak en iyi yaklaşımı, ancak her zaman en kullanışlı değildir. Çoğu durumda, daha basit ve daha düşük maliyetli aşağıda açıklanan Öykünücüler birini kullanarak Android donanım benzetimini/benzetmek için olabilir.


### <a name="android-sdk-emulatorandroiddeploy-testdebuggingandroid-sdk-emulatorindexmd"></a>[Android SDK Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

Bu makaleler, Android SDK'sı ile sağlanan varsayılan öykünücü kullanacak şekilde açıklanmaktadır. Bu öykünücüsü Windows için Visual Studio ve Mac için Visual Studio için kullanılabilir

### <a name="visual-studio-android-emulatorandroiddeploy-testdebuggingvisual-studio-android-emulatormd"></a>[Visual Studio Android Emulator](~/android/deploy-test/debugging/visual-studio-android-emulator.md)

Bu makalede, hata ayıklama ve Visual Studio 2015 yerleşik Android öykünücüsünü kullanarak Xamarin.Android uygulamanızı test etme açıklanmaktadır. Visual Studio 2015 kullanıyorsanız ve özel cihaz profillerini gerekmez bu öykünücü iyi bir seçimdir.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Bir cihazda hata ayıklama](~/android/deploy-test/debugging/debug-on-device.md)

Bu makalede, fiziksel bir Android cihazı Xamarin.Android uygulaması için doğrudan ya da Visual Studio veya Visual Studio veya Mac üzerinden dağıtılabilir şekilde yapılandırmak gösterilmektedir

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Android Hata Ayıklama Günlüğü](~/android/deploy-test/debugging/android-debug-log.md)

Yaygın eli geliştiriciler kullandığınız kendi uygulamalarında hata ayıklamasını kullanarak `Console.WriteLine`. Ancak, Android gibi mobil platformda yoktur konsol yok. Android cihazları, uygulamaları yazarken kullanmak için gereken büyük olasılıkla günlük sağlar. Bu bazen olarak adlandırılır **logcat** nedeniyle bunu almak için yazdığınız komutu. Bu makalede nasıl kullanılacağını açıklar **logcat**.

> [!WARNING]
> Unutmayın **Xamarin Android Player** kullanım dışı bırakıldı. Daha fazla bilgi için bkz: [bu blog gönderisine Duyuruda](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/).
