---
title: "Geriye dönük uyumluluk Android destek paketi ile sağlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/12/2017
ms.openlocfilehash: 670ec465843bbe819b41a53fff71b01ab78b0059
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Geriye dönük uyumluluk Android destek paketi ile sağlama

Parçaları yararlılığı olacaktır olmadan geriye dönük uyumluluk önceden Android 3.0 (API düzeyi 11) cihazlarla sınırlı. Bu yeteneği sağlamak için Google sunulan [destek kitaplığı](http://developer.android.com/sdk/compatibility-library.html) (başlangıçta adlı *Android uyumluluk kitaplığı* zaman yayımlanan) hangi backports bazı daha yeni sürümlerinden API'leri Android Android eski sürümleri için. Bu Android destek Android 2.3.3 Android 1.6 (API düzey 4) çalıştıran cihazlar sağlayan paketidir. (API düzey 10).

> [!NOTE]
> Yalnızca `ListFragment` ve `DialogFragment` Android destek paketi üzerinden kullanılabilir. Alt sınıfların, gibi diğer hiçbiri parçalara `PreferenceFragment,` Android destek paketinde desteklenir. Önceden Android 3.0 uygulamalarında çalışmaz. 


## <a name="adding-the-support-package"></a>Destek paketi ekleme

Android destek paketi bir Xamarin.Android uygulaması için otomatik olarak eklenmez. Xamarin sağlar [Android destek kitaplığı v4 NuGet paketi](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) bir Xamarin.Android uygulamasına destek kitaplıkları ekleme basitleştirmek için. Destek paketleri uygulamayı dahil, bir Xamarin.Android dahil etmek için [Android destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Xamarin.Android projenize bileşen aşağıdaki ekran görüntüsünde gösterildiği gibi: 

[![Android destek kitaplığı ekran v4 paketini projeye eklenen](providing-backwards-compatibility-images/02.png)](providing-backwards-compatibility-images/02.png#lightbox)

Bu adımları gerçekleştirdikten sonra parça Android önceki sürümlerinde kullanmak mümkün olur. Parça API'ları aşağıdaki istisnalar dışında bu önceki sürümlerinde, aynı anda çalışır: 

-   **Minimum Android Version değiştirmeniz** &ndash; uygulamanın artık Android 3.0 veya üzerini, aşağıda gösterildiği gibi hedef gerekir: 

    [![Uygulama özellikleri altında ayarlanmasına Minimum Android ekran görüntüsü, hedef](providing-backwards-compatibility-images/03.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **FragmentActivity genişletme** &ndash; parçaları barındırma etkinlikleri öğesinden şimdi devralmalıdır `Android.Support.V4.App.FragmentActivity` , değil nesneden `Android.App.Activity` . 

-   **Ad alanları güncelleştirme** &ndash; devralınmalıdır sınıfları `Android.App.Fragment` şimdi öğesinden devralmalıdır `Android.Support.V4.App.Fragment` . Using kaldırmak deyimi " `using Android.App;` " kaynak kodunu üstündeki dosya ve bunların yerine " `using Android.Support.V4.App` ". 

-   **SupportFragmentManager kullanmak** &ndash; `Android.Support.V4.App.FragmentActivity` kullanıma sunan bir `SupportingFragmentManager` bir başvuru almak için kullanılması gereken özellik `FragmentManager` . Örneğin: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Yerinde bu değişikliklerle Android 1.6 veya 2.x yanı sıra bal peteği ve dondurma Sandwich parça tabanlı bir uygulama çalıştırmak mümkün olacaktır. 


## <a name="related-links"></a>İlgili bağlantılar

- [Android desteği kitaplığı v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
