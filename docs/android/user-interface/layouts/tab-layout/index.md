---
title: Sekmeli düzenleri
description: Android sekmeli düzenleri genel bakış
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/08/2017
ms.openlocfilehash: 53ed5f91583d43839e96388194aea8c0d6ac5315
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34149266"
---
# <a name="tabbed-layouts"></a>Sekmeli düzenleri


## <a name="overview"></a>Genel Bakış

Sekmeleri popüler kullanıcı arabirimi düzeni mobil uygulamalarda, Basitlik ve kullanılabilirlik nedeniyle kümesidir. Bunlar, bir uygulamada çeşitli ekranları arasında gezinmek için tutarlı ve kolay bir yol sağlar. Android birkaç API'nin sekmeli arabirimleri için sahiptir: 

-   **ActionBar** &ndash; bu yeni bir Android 3.0 (API düzeyi 11) tutarlı bir sağlama hedefle tanıtılan API'nin kümesinin parçası olan gezinti ve Görünüm değiştirme arabirimi. Bu geri ile Android 2.2 (API düzeyi 8) için bağlantı noktası kurulmuş [Android destek kitaplığı v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; geçerli, sonraki ve önceki sayfalarında gösteren bir `ViewPager`. `ViewPager` yalnızca aracılığıyla kullanıma [Android destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Hakkında daha fazla bilgi için `PagerTabStrip`, bkz: [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Araç çubuğu** &ndash; `Toolbar` değiştiren bir daha yeni ve daha esnek Eylem çubuğu bileşenidir `ActionBar`. `Toolbar` Android 5.0 Lolipop veya daha sonra kullanılabilir ve ayrıca Android eski sürümleri için kullanılabilir [Android destek kitaplığı v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet paketi. 
    `Toolbar` şu anda Android uygulamaları kullanmak için önerilen eylemi çubuğu bileşendir.
    Daha fazla bilgi için bkz: [araç](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="related-links"></a>İlgili bağlantılar

- [Malzeme tasarım - sekmeler](https://material.io/guidelines/components/tabs.html)- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android desteği kitaplığı v7 uygulama NuGet paketi](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 uygulama kitaplığı](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
