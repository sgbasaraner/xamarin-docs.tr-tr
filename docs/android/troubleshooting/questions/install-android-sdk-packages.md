---
title: "Hangi Android SDK paketleri yüklediğimde?"
ms.topic: article
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/02/2018
ms.openlocfilehash: e2b0736a9ccc4dde5c1dcdf2d99f527247040560
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="which-android-sdk-packages-should-i-install"></a>Hangi Android SDK paketleri yüklediğimde?

Android SDK'sını yükleme geliştirmek için tüm minimum gerekli paketleri otomatik olarak içermez. Tek tek Geliştirici farklılık, ancak aşağıdaki paketleri genellikle Xamarin.Android ile geliştirmek için gerekli olacaktır:

## <a name="tools"></a>Araçlar

SDK Yöneticisi'nde Araçlar klasöründen en yeni araçları yükleyin:

- Android SDK Araçları
- Android SDK platformunuzun Araçlar
- Android SDK derleme araçları

## <a name="android-platforms"></a>Android platformları

Minimum & Hedef olarak ayarladığınızdan Android sürümleri için "SDK Platform" yükleyin. 

Örnekler:

- Hedef API 23
- Minimum API 23

Yalnızca SDK Platform için API 23 yüklemeniz gerekir

- Hedef API 23
- Minimum API 15

SDK'sı platformlar API 15 ve 23 yüklemeniz gerekir. Not (Bu API düzeylerini backporting olsa bile,) en az ve hedef arasındaki API düzey yüklemeniz gerekmez.

## <a name="system-images"></a>Sistem görüntüleri
Yalnızca bunlar Google sayfası Giden kutusu Android öykünücüsünü kullanmak istiyorsanız gereklidir. 

- [Varsayılan öykünücü yapılandırma](~/android/get-started/installation/android-emulator/index.md)

- [Varsayılan öykünücü hızlandırmak nasıl](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Ek Özellikler
Android SDK ek özellikler genellikle gerekli değildir; Ancak, kullanım örneği bağlı olarak gerekli olabileceğinden bunları haberdar olmanız kullanışlıdır.

## <a name="further-reading"></a>Daha Fazla Bilgi
Aşağıdaki kılavuz Bu seçenekler kapsayan ve farklı paketler hakkında daha fazla ayrıntı SDK girmeyeceğini manager mevcut olan: [Android SDK Manager Kurulumu Kılavuzu](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

