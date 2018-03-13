---
title: "Araç Çubuğu"
description: "Araç, varsayılan Eylem çubuğu daha fazla esneklik sağlayan bir eylem çubuğu bileşenidir: uygulamada herhangi bir yere yerleştirilebilir, boyutuna değiştirilebilir ve uygulamanın temayı farklı bir renk şeması kullanabilirsiniz. Ayrıca, her uygulama ekran birden çok araç çubukları olabilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 30b1cb280c2817f55d73e10ff8b4d7942011bf2c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="toolbar"></a>Araç Çubuğu

_Araç, varsayılan Eylem çubuğu daha fazla esneklik sağlayan bir eylem çubuğu bileşenidir: uygulamada herhangi bir yere yerleştirilebilir, boyutuna değiştirilebilir ve uygulamanın temayı farklı bir renk şeması kullanabilirsiniz. Ayrıca, her uygulama ekran birden çok araç çubukları olabilir._

 
## <a name="overview"></a>Genel Bakış

Herhangi bir Android etkinliği temel tasarım öğesi bir *Eylem çubuğu*. Eylem çubuğunda kullanılan gezinme, arama, menüleri ve Android uygulaması marka UI bileşendir. Android 5.0 Lolipop, eylem çubuğundaki önce Android sürümlerde (olarak da bilinen *uygulama çubuğunu*) bu işlevselliği sağlamak için önerilen bir bileşen. 

`Toolbar` (Android 5.0 Lolipop içinde sunulan) pencere öğesi zorlayıcı bir eylem çubuğu arabirimi Genelleştirme &ndash; Eylem çubuğu değiştirmek için tasarlanmıştır. `Toolbar` Bir uygulama düzende herhangi bir kullanılabilir ve bir eylem çubuğu çok daha özelleştirilebilir. Aşağıdaki ekran görüntüsünde özelleştirilmiş gösterilmektedir `Toolbar` bu Kılavuzu'nda oluşturulan örnek: 

[![Örnek ekran görüntüsü bir araç çubuğu düzenleyin, kaydedin ve menü öğeleri taşması](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

Arasında önemli bazı farklar vardır `Toolbar` ve Eylem çubuğu: 

-   A `Toolbar` kullanıcı arabiriminde herhangi bir yere yerleştirilebilir.

-   Birden çok araç çubukları aynı ekranda görüntülenebilir.

-   Parçaları kullanılıyorsa, her parça kendi olabilir `Toolbar`. 

-   A `Toolbar` kısmi genişliğini ekran span şekilde yapılandırılabilir. 

-   Çünkü `Toolbar` bağlı değilse etkinliğe ilişkin penceresi dekorasyonu için renk düzenini görsel olarak ayrı renk düzenini sahip olabilir. 

-   Eylem çubuğunda aksine `Toolbar` simge soldaki içermez. Sağdaki menülerini daha az alan kullanın. 

-   `Toolbar` Yüksekliği ayarlanabilir. 

-   İçindeki diğer görünümlere eklenebilir `Toolbar`. 

A `Toolbar` bir veya daha fazla aşağıdaki öğeleri içerebilir: 

-   Gezinti düğmesi

-   Markalı logo görüntüsü

-   Başlık ve alt başlığı

-   Özel görünümler

-   Eylem menüsü

-   Taşma menüsü

Google [malzeme tasarım yönergeleri](https://material.google.com/) uygulamalar farklı bir görünüm (yerine yalnızca bir uygulama simgesi ve/veya başlık bağlı olan) vermek için bu öğeler yararlanarak önerir. 

Bu kılavuz, en yaygın kullanılan kapsar. `Toolbar` senaryolar:

-   Bir etkinliğin varsayılan eylem çubuğunda değiştirerek bir `Toolbar`. 

-   İkinci bir ekleme `Toolbar` aktiviteye.

-   Kullanarak **Android destek kitaplığı v7 uygulama** kitaplığı (olarak adlandırılan *uygulama* bu kılavuzun geri kalanında) dağıtmak için `Toolbar` Android önceki sürümlerinde. 

 
 
## <a name="requirements"></a>Gereksinimler

`Toolbar` Android 5.0 Lolipop (API 21) ve sonraki sürümlerinde kullanılabilir. Android hedefleme Android 5.0'den önceki sürümleri, kullanın [Android destek kitaplığı v7 uygulama](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), geriye dönük olarak uyumlu sağlayan `Toolbar` destekleyen bir NuGet paketi. 
[Araç çubuğu Uyumluluk](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) bu kitaplığının nasıl kullanıldığını açıklar. 




## <a name="related-links"></a>İlgili bağlantılar

- [Lolipop araç çubuğu (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [Uygulama araç çubuğu (örnek)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
