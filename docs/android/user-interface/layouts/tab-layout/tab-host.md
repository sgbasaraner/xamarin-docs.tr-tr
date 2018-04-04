---
title: Sekme düzeni TabHost ile
description: Bu makalede bir yüksek düzeyde genel bakış sağlayacak TabHost, daha eski bir API sekmeli düzenleri bir Xamarin.Android uygulaması oluşturmak için kullanılır.
ms.prod: xamarin
ms.assetid: 77B890A4-27A6-41DF-81BA-22C6116A8FB2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: ae5b9020e08575bcd453703f3df14f63b288d2f5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="tab-layout-with-tabhost"></a>Sekme düzeni TabHost ile

_Bu makalede bir yüksek düzeyde genel bakış sağlayacak TabHost, daha eski bir API sekmeli düzenleri bir Xamarin.Android uygulaması oluşturmak için kullanılır._


## <a name="overview"></a>Genel Bakış

> [!NOTE]
> `TabHost` Google tarafından onaylanmaz eski bir API'dır. Geliştiriciler kullanarak sekmeli uygulamaları oluşturmak için kullanmaları [ActionBar](~/android/user-interface/controls/action-bar.md). `ActionBar` Tüm Android sürümünde kullanılabilir. Android 3.0 (API düzeyi 11) ilk sunulmuştur ve Android 2.2 (API düzeyi 8) ve Android 2.3 (API düzey 10) içinde geri alındığını [V7 uygulama Kitaplığı](http://developer.android.com/tools/support-library/features.html#v7-appcompat), Xamarin.Android kullanılabilir olduğu [Xamarin Android desteği Library - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) paket.

`TabHost` Sekmeli kullanıcı interfacesIt oluşturma Xamarin.Android Android 2.2 ve Android 2.3 desteklemesi ve kullanamazsınız uygulamalar için uygundur daha eski, özgün API aranır **ActionBarSherlock**.
Aşağıdaki beş bileşenleri ile ilgili tüm `TabHost` API'si:

-  **TabActivity** &ndash; bu için kapsayıcı görevi gören, özelleştirilmiş bir görünümdür bir `TabHost` (aşağıda açıklanmıştır).

-  **TabHost** &ndash; bu sekmeli UI için kapsayıcıdır. İki alt barındıran: sekme etiketleri listesi ve bir `FrameLayout` sekmesini içeriği görüntüler.

-  **TabWidget** &ndash; bu denetim sekme etiketleri, her bir sekmede için bir tane listesini görüntüler `TabHost` . Her bir sekmede bir `TabHost` tarafından temsil edilen bir `TabHost.TabSpec` nesnesi. Bu nesne her sekme için meta veriler içerir. Kullanıcı bir sekme seçtiğinde `TabHost` seçime uygun sekmeyi görüntüleyerek yanıt verir.

-  **FrameLayout** &ndash; sekmeli UI olmalıdır bir `FrameLayout` içinde yer alan bir `TabHost`. Sekmenin içeriğini görüntülemek için kullanılır.

-  **Etkinlikler veya görünümleri** &ndash; bir etkinlik veya bir görünümde bir sekmeye seçildikten sonra görüntülediği `FrameLayout`.

Aşağıdaki diyagramda, nasıl bu bileşenlerin tümünü birlikte bir ilişki gösterilmektedir:

![TabHost içinde TabWidget içinde çerçevesi düzeni gösteren diyagram](tab-host-images/image03.png)

Sekme içeriğinin etkinlikleri veya görünümler olabilir. Görünümleri nispeten basit ve basit ancak çok sayıda ilgisiz kod co-habitating etkinliğinde neden olabilir. Bu sorunları ve korumak sabit bir bloated sınıfı zayıf ayrımı içinde sonuçlanır. Buna karşılık, etkinlikleri sistem kaynaklarını gerektirir ancak kendi ayrı sınıfında kapsüllenmiş her sekme mantığı ile daha modüler bir yaklaşım için sağlar.


## <a name="summary"></a>Özet

Bu makalede eski üst düzey bileşenlerden açıklandığı `TabHost` Android ve nasıl tüm birlikte ilişkili oldukları için API.



## <a name="related-links"></a>İlgili bağlantılar

- [Eylem Çubuğu](http://developer.android.com/guide/topics/ui/actionbar.html)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabHost.TabSpec](https://developer.xamarin.com/api/type/Android.Widget.TabHost+TabSpec/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [Eylem Çubuğu](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android desteği kitaplığı v7 uygulama NuGet paketi](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 uygulama kitaplığı](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
