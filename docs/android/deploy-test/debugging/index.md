---
title: Cihazlar ve Öykünücüler Xamarin.Android hata ayıklama
description: Test ve Xamarin.Android uygulamanızın hatalarını ayıklama
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 1ed6ec57365c5d3a861dd3fd947a2ad195ce5357
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935392"
---
# <a name="debugging"></a>Hata Ayıklama

Bu bölümde, bir Xamarin.Android uygulaması veya Öykünücüler cihazlarda hata ayıklamak nasıl anlatılmaktadır.

## <a name="debugging-overview"></a>Hata Ayıklamaya Genel Bakış

Android uygulamaları geliştirme uygulama ya da fiziksel donanım üzerinde çalışan veya bir öykünücü kullanarak gerektirir. Donanım kullanarak en iyi yaklaşımı, ancak her zaman en kullanışlı değildir. Çoğu durumda, daha basit ve daha düşük maliyetli aşağıda açıklanan Öykünücüler birini kullanarak Android donanım benzetimini/benzetmek için olabilir.

### <a name="debugging-on-the-android-emulatorandroiddeploy-testdebuggingdebug-on-emulatormd"></a>[Android öykünücüsünde hata ayıklama](~/android/deploy-test/debugging/debug-on-emulator.md)

Bu makalede açıklanır nasıl Visual Studio'dan Android öykünücüsünde başlatın ve bir sanal cihaz uygulamanızı çalıştırın.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Bir cihazda hata ayıklama](~/android/deploy-test/debugging/debug-on-device.md)

Bu makalede, fiziksel bir Android cihazı Xamarin.Android uygulaması için doğrudan ya da Visual Studio veya Visual Studio veya Mac üzerinden dağıtılabilir şekilde yapılandırmak gösterilmektedir

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Android Hata Ayıklama Günlüğü](~/android/deploy-test/debugging/android-debug-log.md)

Yaygın eli geliştiriciler kullandığınız kendi uygulamalarında hata ayıklamasını kullanarak `Console.WriteLine`. Ancak, Android gibi mobil platformda yoktur konsol yok. Android cihazları, uygulamaları yazarken kullanmak için gereken büyük olasılıkla günlük sağlar. Bu bazen olarak adlandırılır **logcat** nedeniyle bunu almak için yazdığınız komutu. Bu makalede nasıl kullanılacağını açıklar **logcat**.

> [!WARNING]
> Unutmayın **Xamarin Android Player** kullanım dışı bırakıldı. Daha fazla bilgi için bkz: [bu blog gönderisine Duyuruda](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/). Ayrıca, **Visual Studio Android öykünücüsü** Visual Studio 2017 sürümünden itibaren kullanım dışı bırakıldı.
