---
title: "Sekmeli düzenleri"
description: "Android sekmeli düzenleri genel bakış"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/23/2017
ms.openlocfilehash: 02a425c8276524accc088b53c1099e7c2e28d828
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="tabbed-layouts"></a>Sekmeli düzenleri


## <a name="overview"></a>Genel Bakış

Sekmeleri popüler kullanıcı arabirimi düzeni mobil uygulamalarda, Basitlik ve kullanılabilirlik nedeniyle kümesidir. Bunlar, bir uygulamada çeşitli ekranları arasında gezinmek için tutarlı ve kolay bir yol sağlar. Android birkaç API'nin sekmeli arabirimleri için sahiptir: 

-   **TabHost** &ndash; sekmeli kullanıcı arabirimleri oluşturmak için özgün API budur. A `TabHost` pencere öğesi bir düzene eklenir ve sekmeli görünümleri için kapsayıcı görevi görür. Bu API beri kullanım ve onun kullanılması önerilmez. 

-   **ActionBar** &ndash; bu yeni bir Android 3.0 (API düzeyi 11) tutarlı bir sağlama hedefle tanıtılan API'nin kümesinin parçası olan gezinti ve Görünüm değiştirme arabirimi. Bu geri ile Android 2.2 (API düzeyi 8) için bağlantı noktası kurulmuş [Android destek kitaplığı v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; geçerli, sonraki ve önceki sayfalarında gösteren bir `ViewPager`. `ViewPager` yalnızca aracılığıyla kullanıma [Android destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Hakkında daha fazla bilgi için `PagerTabStrip`, bkz: [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Araç çubuğu** &ndash; `Toolbar` değiştiren bir daha yeni ve daha esnek Eylem çubuğu bileşenidir `ActionBar`. `Toolbar` Android 5.0 Lolipop veya daha sonra kullanılabilir ve ayrıca Android eski sürümleri için kullanılabilir [Android destek kitaplığı v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet paketi. 
    `Toolbar` şu anda Android uygulamaları kullanmak için önerilen eylemi çubuğu bileşendir.
    Daha fazla bilgi için bkz: [araç](~/android/user-interface/controls/tool-bar/index.md). 


Bu API'nin görsel olarak çok farklı olan ve birbirleri ile uyumlu değildir. Aşağıdaki ekran görüntüsü gösterildiği `TabHost` ve `ActionBar` yan yana: 

![TabHost ekran görüntüleri sol ve sağ taraftaki ActionBar](images/image01.png)

Bu uyumsuz API'nin önemli UI değişiklikler nedeniyle Android 3.0 (API düzeyi 11) bu yana mevcut. Bu UI değişiklikleri biri [tasarım deseni çubuğu eylem](http://www.androidpatterns.com/uap_pattern/action-bar), bir uygulamadaki gezinti ve anahtar işlevsellik kolay erişim sağlamak bir desen amaçlar. `ActionBar` API bu desenin desteklenmesi için sunulmuştur. 

`ActionBar` API daha basit ve tartışmaya açık bir şekilde daha iyi bir kullanıcı deneyimi sağlar. Android 2.2 geri bağlantı noktalı ve Xamarin.Android uygulamaları için tercih edilen bir seçimdir. 

`TabHost` API Android tüm sürümleri arasında uyumlu ancak kullanmak için daha fazla çaba gerektirir ve geçerli tutarlı değil [Android UI yönergeleri](http://developer.android.com/design/index.html). Geliştiriciler bu API'yi kullanarak önerilmez ve Xamarin.Android uygulamaları için daha yeni ActionBar favour. 



## <a name="actionbarsherlock"></a>ActionBarSherlock

ActionBar API'nin backported Android 2.2, ActionBar API yeni görünüm ve yapısını istedi, ancak bir üçüncü taraf kitaplık kullanabilirsiniz geliştiriciler için önce [ActionBarSherlock](http://actionbarsherlock.com). Android destek kitaplığı uzantısı backport için Eylem çubuğu tasarım deseni Android 2.x için tasarlanmış ActionBarSherlock olur. Android 3.0 veya daha yüksek çalıştırırken ActionBarSherlock yerel otomatik olarak kullanacağı `ActionBar` Android tarafından sağlanan API. Android eski sürümleri, Görünüm ve yapısını daha yeni benzetimini yapacak özel bir düzeyi kullanacağınız `ActionBar` API. [ActionBarSherlock bileşen](https://www.nuget.org/packages/xamstore-XamarinActionBarSherlock/) ActionBarSherlock bir Xamarin.Android uygulamasına ekleme kolay hale getirir. 



## <a name="related-links"></a>İlgili bağlantılar

- [TabHost’a Genel Bakış](tab-host.md)
- [TabHost gözden geçirme](~/android/user-interface/layouts/tab-layout/creating-a-tabbed-ui.md)
- [Eylem Çubuğu](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android desteği kitaplığı v7 uygulama NuGet paketi](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 uygulama kitaplığı](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
