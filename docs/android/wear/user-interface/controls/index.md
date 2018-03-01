---
title: "Android yıpranması denetimleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5B62A5F8-5E55-4B3C-BFC4-E21CDB27C08B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 867246b3784fb244171058c7f4c6ec54092e813b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="android-wear-controls"></a>Android yıpranması denetimleri

Android yıpranması uygulamaları kullanabileceğiniz birçok aynı denetimini zaten dahil olmak üzere normal Android uygulamaları için kullanımda `Button`, `TextView`, görüntü drawables. Düzen denetimleri de dahil olmak üzere `ScrollView`, `LinearLayout`, ve `RelativateLayout` de kullanılabilir.

Android yıpranması özel denetimleri için bu sayfayı bağlantıları [wearable kullanıcı Arabirimi Kitaplığı](https://developer.android.com/training/wearables/apps/layouts.html#UiLibrary) Xamarin projelerine kullanılabilir [Wearable Destek](http://www.nuget.org/packages/Xamarin.Android.Wear/) NuGet paketi. Bu denetimleri aşağıdakileri içerir:

-   **GridViewPager** &ndash; burada kullanıcı kayar aşağı sonra arasında seçim yapmak için iki boyutlu Gezinti arabirimi oluşturma (daha fazla bilgi için bkz: [GridViewPager](~/android/wear/user-interface/controls/gridviewpager.md)):

    ![Bir GridViewPager örnek ekran görüntüsü](images/gridviewpager.png)

Yıpranmasıyla ilişkili uygulamalar için diğer önemli denetimleri içerir:

* `BoxInsetLayout` (bkz [ekran boyutlarıyla çalışma](~/android/wear/screen-sizes.md)),

* `WatchViewStub` (bkz [ekran boyutlarıyla çalışma](~/android/wear/screen-sizes.md)),

* `CardFrame` (bkz [Android oluşturma kartları](https://developer.android.com/training/wearables/ui/cards.html)),

* `CardScrollView` (bkz [Android oluşturma kartları](https://developer.android.com/training/wearables/ui/cards.html)),

* `WearableListView` (bkz [oluşturma Android listeler](https://developer.android.com/training/wearables/ui/lists.html)).


## <a name="related-links"></a>İlgili bağlantılar

- [Android.Support.Wearable belgeleri](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
