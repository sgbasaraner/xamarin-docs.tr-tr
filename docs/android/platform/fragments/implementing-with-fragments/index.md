---
title: Parçaları ile uygulama
description: Android 3.0 parçaları sunmuştur. Parçalar, farklı boyutlardaki ekranlarda çalıştırılabilecek uygulama yazma sürecinin karmaşıklığına yanıt vermek için kullanılan, kendi içindeki modüler bileşenlerdir. Bu makalede parçaları Xamarin.Android uygulamaları geliştirmek için nasıl kullanılacağı ve önceden Android 3.0 cihazlarda parçaları desteklemek nasıl anlatılmaktadır.
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 81f1f992de450ee62c4c1d2e80da858b024be594
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="implementing-with-fragments"></a>Parçaları ile uygulama

_Android 3.0 parçaları sunmuştur. Parçalar, farklı boyutlardaki ekranlarda çalıştırılabilecek uygulama yazma sürecinin karmaşıklığına yanıt vermek için kullanılan, kendi içindeki modüler bileşenlerdir. Bu makalede parçaları Xamarin.Android uygulamaları geliştirmek için nasıl kullanılacağı ve önceden Android 3.0 cihazlarda parçaları desteklemek nasıl anlatılmaktadır._


## <a name="overview"></a>Genel Bakış

Bu bölümde, biz Shakespeare'nın yürütür ve her seçili play tekliften listesini görüntüleyen bir uygulama oluşturmak nasıl adım geçireceğiz. Böylece biz bizim UI bileşenleri tek bir yerde tanımlamak ancak bunları farklı form faktörlerine kullanmak uygulamamıza parçaları kullanın. Örneğin, aşağıdaki ekran görüntüleri 10" Tablet yanı sıra bir telefon üzerinde çalışan uygulama göstermektedir:

[![Tablet ve telefon üzerinde çalışan örnek uygulama ekran görüntüleri](images/intro-screenshot-sml.png)](images/intro-screenshot.png#lightbox)

Bu bölümde aşağıdaki konuları ele alınacaktır:

- **Parçaları oluşturma** &ndash; Shakespeare'nın yürütür listesini görüntülemek için bir parçası ve her play tekliften görüntülemek için başka bir parça nasıl oluşturulacağını gösterir.

- **Farklı ekran boyutlarına destekleyen** &ndash; gösterir nasıl düzen büyük ekran boyutlarına yararlanmak için uygulama.

- **Android destek paketi kullanarak** &ndash; Android destek paketi uygular ve ardından Android eski sürümleri çalışmasına izin vererek uygulama etkinlikler için bazı küçük değişiklikler yapar.


## <a name="requirements"></a>Gereksinimler

Bu kılavuzda Xamarin.Android 4.0 veya üstünü gerektirir. Bu ayrıca parçaları belgelerinde Android destek paketi yüklemek için gerekli olacaktır.


## <a name="introduction"></a>Giriş

Bu bölümde yapı örnekte etkinlikler listesi yüklenirken, kullanıcının seçimini yanıt veya seçili play teklif görüntüleme mantığı içermez. Bu mantık tek tek parçaları bulunmaktadır.
Bu mantığı parçaları kendilerini yerleştirerek, biz her etkinlik için farklı mantığı yazmak zorunda kalmadan bir etkinlik veya birden çok etkinliklerle küçük ekranlar ile büyük ekranlar desteklemek için uygulamanın iş akışı bozulabilir. Tablet, her iki parçada bir etkinlikte olacaktır. Bir telefonda parçaları farklı etkinlikleri barındırılan.

Bu uygulama, aşağıdaki bölümleri içerir:

 **MainActivity** – birini veya her ikisini parçalar ekran boyutuna bağlı olarak görüntüler. Etkinlik Başlangıç budur.

 **TitlesFragment** – alınacağı kullanıcı seçebilir Shakespeare'nın yürütür listesini görüntüler.

 **DetailsFragment** – seçili play tekliften görüntüler.

 **DetailsActivity** – barındırır ve DetailsFragment görüntüler.
Bu etkinlik, küçük ekranlar, telefonlar gibi aygıtlarla tarafından kullanılır.



## <a name="related-links"></a>İlgili bağlantılar

- [FragmentsWalkthrough (örnek)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Tasarımcı genel bakış](~/android/user-interface/android-designer/index.md)
- [Xamarin.Android örnekleri: bal peteği Galerisi](https://developer.xamarin.com/samples/HoneycombGallery/)
- [Parçaları uygulama](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Destek Paketi](http://developer.android.com/sdk/compatibility-library.html)
