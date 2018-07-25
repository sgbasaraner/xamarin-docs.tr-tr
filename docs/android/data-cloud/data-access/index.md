---
title: Xamarin.Android veri erişimi
description: Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinimi vardır. Veri miktarı basit bir şekilde küçük olmadığı sürece, bu genellikle bir veritabanı ve uygulamanın veritabanı erişimi yönetmek için veri katmanında gerektirir.  Android SQLite Veritabanı Altyapısı 'yerleşik' var ve Xamarin'in platformu tarafından veri depolama ve alma için erişim basitleştirilmiştir. Bu belge, platformlar arası şekilde bir SQLite veritabanından erişmeye gösterilmektedir.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0ee89728395134f26972ebefa28ca96925b98c84
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241100"
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android veri erişimi

_Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinimi vardır. Veri miktarı basit bir şekilde küçük olmadığı sürece, bu genellikle bir veritabanı ve uygulamanın veritabanı erişimi yönetmek için veri katmanında gerektirir.  Android SQLite Veritabanı Altyapısı 'yerleşik' var ve Xamarin'in platformu tarafından veri depolama ve alma için erişim basitleştirilmiştir. Bu belge, platformlar arası şekilde bir SQLite veritabanından erişmeye gösterilmektedir._

## <a name="data-access-overview"></a>Veri erişimine genel bakış

Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinimi vardır. Veri miktarı basit bir şekilde küçük olmadığı sürece, bu genellikle bir veritabanı ve uygulamanın veritabanı erişimi yönetmek için veri katmanında gerektirir. "Yerleşik" Sqlite veritabanı altyapısı hem Android var ve verilere erişim SQLite veri sağlayıcısı ile birlikte gelen Xamarin'in platforma göre basitleştirilmiştir.

Xamarin.Android veritabanı erişimi API'leri gibi destekler:

-  ADO.NET çerçevesi.
-  Kitaplık SQLite NET 3. taraf.

Bu bölümdeki kod çoğunu tamamen platformlar arası ve iOS veya Android üzerinde değişiklik yapılmadan çalışır. Ele alınan iki örnek uygulama vardır:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; basit veri işlemleri Yazar sonuçları bir metin denetimi; görüntüleyin

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; veri işlemleri listeler ve basit veri yapısı düzenlemeleri küçük çalışan bir uygulama tümleştirilir.

Her iki örnek çözüm, iOS ve Android örnek uygulama projeleri içerir.

Xamarin.Forms uygulamaları için okuma [veritabanlarıyla çalışma](~/xamarin-forms/app-fundamentals/databases.md) Xamarin.Forms ile PCL kitaplığında SQLite ile nasıl çalışılacağını açıklar.

Bu bölümdeki konular, veri erişimi Xamarin.Android SQLite veritabanı altyapısı olarak kullanma açıklanmaktadır. Veritabanı "doğrudan" ADO.NET sözdizimi kullanılarak erişilebilir veya SQLite.NET ORM içerir ve C# ' de veri işlemleri gerçekleştirebilirsiniz.

İki örneği gözden geçirilebilir: çıkış metni alanına ve içeren basit bir uygulama oluşturma, okuma, çok basit veri erişim kodu içeren bir güncelleştirme ve silme işlevi. Parçacıkları ve çekirdeğini önceden doldurulmuş bir SQLite veritabanından uygulamanızla nasıl da bahsedilmiştir.

Platformlar arası veri erişim diğer örnekler için sunduğumuz [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) incelemesi.


## <a name="related-links"></a>İlgili bağlantılar

- [DataAccess Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android veri tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
