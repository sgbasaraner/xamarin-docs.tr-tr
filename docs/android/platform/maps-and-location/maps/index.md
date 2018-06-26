---
title: Google Haritalar ve konum Xamarin.Android ile kullanma
description: Bu makalede eşlemeleri ve konumu ile Xamarin.Android nasıl kullanılacağı açıklanmaktadır. Google eşlemeleri Android API V2 kullanarak doğrudan yerleşik eşlemeleri uygulamanın yararlanarak her şeyi içerir.
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: a861e43152870933ba684bf693a1bd3d3ac5bd0b
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935379"
---
# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Google Haritalar ve konum Xamarin.Android ile kullanma

_Bu makalede eşlemeleri ve konumu ile Xamarin.Android nasıl kullanılacağı açıklanmaktadır. Google eşlemeleri Android API V2 kullanarak doğrudan yerleşik eşlemeleri uygulamanın yararlanarak her şeyi içerir._

## <a name="maps-overview"></a>MAPS genel bakış

Eşleme, mobil cihazlara bulunabilen tamamlama teknolojilerdir. Masaüstü bilgisayarlar ve dizüstü bilgisayarlar yerleşik konumu tanıma sahip olma eğilimi gösterir yok. Öte yandan, mobil aygıtları bulmak ve değişen konum bilgilerini görüntülemek için bu tür uygulamalar kullanın. Android cihazda kullanılabilir konum donanım kullanarak haritalar konum verileri görüntüler güçlü, yerleşik teknolojiye sahiptir. Bu makalede, dahil olmak üzere Xamarin.Android altında eşlemeleri uygulamaları sunmak sahip olan bir dizi yer almaktadır: 

-  Yerleşik kullanarak hızlı bir şekilde eşleme işlevselliği eklemek için uygulama eşler.
-  Bir haritanın görüntü denetlemek için haritalar API'si ile çalışıyor.
-  Grafik yer paylaşımları eklemek için çeşitli teknikler kullanma.

Bu bölümdeki konular, çok çeşitli eşleme özelliklerini kapsar.
İlk olarak, bunlar Android'ın yerleşik eşlemeleri uygulama yararlanır ve bir konum panoramik Sokak görünümünü görüntülemek açıklanmaktadır. Ardından bunlar grafik yer paylaşımları ekleme yanı sıra haritalar API'si eşleme özelliklerini hem nasıl denetim konumu ve bir harita görünümünü kapsayan doğrudan bir uygulama içinde birleştirmek için nasıl kullanılacağı açıklanmaktadır.


## <a name="related-links"></a>İlgili bağlantılar

- [MapsAndLocationDemo_v3 (örnek)](https://developer.xamarin.com/samples/monodroid/MapsAndLocationDemo_v3/)
- [Etkinlik Yaşam Döngüsü](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Google Haritalar API Anahtarını Alma](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Amaçlar listesi: Google uygulamaları Android cihazlarda çağırma](http://developer.android.com/guide/appendix/g-app-intents.html)
- [Konum ve eşlemeleri](http://developer.android.com/guide/topics/location/index.html)
