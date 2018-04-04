---
title: Bir ContentProvider kullanma
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b9b6340d4aaf386c7b4be8ebf366589582771be2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="using-a-contentprovider"></a>Bir ContentProvider kullanma

CursorAdapters bir ContentProvider verileri görüntülemek için de kullanılabilir.
ContentProviders diğer uygulamalar tarafından kullanıma sunulan veri erişim izin ver (Android sistem verileri kişiler gibi dahil olmak üzere medya ve Takvim bilgileri).

LoaderManager kullanarak CursorLoader ile bir ContentProvider erişmek için önerilen yöntemdir. LoaderManager Android ana iş parçacığı engelleme görevleri taşımak için 3.0 (API düzeyi 11, bal peteği) sürümünde sunulan ve bir CursorLoader kullanarak verilerin görüntülenmesi için ListView bağlanan önce bir iş parçacığında yüklenmesine izin verir.

Başvurmak [ContentProviders giriş](~/android/platform/content-providers/index.md) daha fazla bilgi için.

