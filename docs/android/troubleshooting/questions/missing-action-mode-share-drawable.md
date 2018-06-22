---
title: "Android.Support.v7.AppCompat - verilen adı ile eşleşen hiçbir kaynak bulundu: attr 'android: actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 07655587642c3e1aa94d035e76f6f6758340546d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774902"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - verilen adı ile eşleşen hiçbir kaynak bulundu: attr 'android: actionModeShareDrawable'

1. En son ek özellikler ve bunun yanı sıra Android 5.0 (API 21) SDK aracılığıyla Android SDK Yöneticisi'ni yüklediğinizden emin olun.

2. 21 değerine ayarlanmış compileSdkVersion ile uygulamanızı derlediğiniz emin olun. İsteğe bağlı olarak, targetSdkVersion 21 de ayarlayabilirsiniz.

3. API 19 gibi önceki bir sürümünü gerektirir, lütfen Nuget sayfasında bulunan ilgili sürümünü karşıdan yükleyin:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*Not*: el ile bu Paket Yöneticisi konsolu yüklerseniz, Xamarin.Android.Support.v4 aynı sürümünü de yüklediğinizden emin olun

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Yığın taşması başvurusu: [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>Ayrıca Bkz.

- [Hangi Android SDK paketlerini yüklemeliyim?](~/android/troubleshooting/questions/install-android-sdk-packages.md)

