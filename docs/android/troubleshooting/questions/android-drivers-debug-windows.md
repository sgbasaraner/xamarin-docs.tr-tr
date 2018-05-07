---
title: Android, Windows hata ayıklamak hangi USB sürücüleri gerekiyor mu?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: 77a7f50ab9d8f351dcefcbbdd50e88e18a13645d
ms.sourcegitcommit: c9ebf456e1c6924956bedb13f4ea78ff09f7b1a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Android, Windows hata ayıklamak hangi USB sürücüleri gerekiyor mu?

## <a name="finding-usb-drivers"></a>USB sürücüleri bulma

Android cihazında Windows geliştirirken hata ayıklamak için; uyumlu bir USB sürücü yüklemeniz gerekir. Android SDK Manager burada açıklandığı gibi Nexus cihazlar için destek ekler varsayılan olarak "Google USB sürücüsü" içerir: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Diğer cihazları özellikle aygıt üreticisi tarafından yayımlanan USB sürücüleri gerektirir. Bazı bağlantılar en yaygın üreticileri için bu kılavuzda bulunmaktadır: [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternatifleri

Manfacturer bağlı olarak, gerekli tam USB sürücüsü izlemek zor olabilir. Android uygulamalarını test etme için bazı alternatifleri Android öykünücüsünde veya dış sınaması hizmetleri kullanarak da dahil olmak üzere Windows geliştirmiştir. Bunlardan bazıları şunlardır:

- [Uygulama Merkezi Test](https://docs.microsoft.com/appcenter/test-cloud/) - bulut sınama Hizmetleri gerçek Android cihazları yüzlerce üzerinde çalıştırın.

- [Android için Visual Studio Öykünücüsü](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)

- [Google Android SDK öykünücüsü](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

