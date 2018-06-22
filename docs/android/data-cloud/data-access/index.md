---
title: Xamarin.Android Data Access
description: Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinim vardır. Veri miktarını trivially küçük olmadığı sürece, bu genellikle bir veritabanı ve veri katmanı veritabanı erişimi yönetmek üzere uygulamada gerektirir.  Android 'yerleşik' SQLite veritabanı altyapısı vardır ve depolamak ve veri almak için erişim Xamarin'ın platform tarafından basitleştirilmiştir. Bu belge, platformlar arası şekilde bir SQLite veritabanına erişmek gösterilmiştir.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 508a8f54723bfdd30b1c8ea8d4b6c1d422ae24e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763256"
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android Data Access

_Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinim vardır. Veri miktarını trivially küçük olmadığı sürece, bu genellikle bir veritabanı ve veri katmanı veritabanı erişimi yönetmek üzere uygulamada gerektirir.  Android 'yerleşik' SQLite veritabanı altyapısı vardır ve depolamak ve veri almak için erişim Xamarin'ın platform tarafından basitleştirilmiştir. Bu belge, platformlar arası şekilde bir SQLite veritabanına erişmek gösterilmiştir._

## <a name="data-access-overview"></a>Veri erişimine genel bakış

Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinim vardır. Veri miktarını trivially küçük olmadığı sürece, bu genellikle bir veritabanı ve veri katmanı veritabanı erişimi yönetmek üzere uygulamada gerektirir. Android her iki "yerleşik" Sqlite veritabanı altyapısı vardır ve verilere erişim SQLite veri sağlayıcısı ile birlikte gelen Xamarin'ın platform tarafından basitleştirilmiştir.

Xamarin.Android veritabanı erişimi API'leri gibi destekler:

-  ADO.NET çerçevesi.
-  Kitaplık SQLite NET 3. taraf.

Bu bölümdeki kod çoğunluğu tamamen platformlar arası ve iOS veya Android üzerinde değişiklik yapılmadan çalışır. Ele alınan iki örnek uygulamaları şunlardır:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; basit veri işlemleri Yazar sonuçları bir metin görüntülemek denetimi;

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; veri işlemleri listeler ve basit veri yapısı düzenlemeleri küçük çalışma uygulamasına tümleştirir.

Her iki örnek çözüm iOS ve Android örnek uygulama projeleri içerir.

Xamarin.Forms uygulamalar için okuma [veritabanlarıyla çalışma](~/xamarin-forms/app-fundamentals/databases.md) Xamarin.Forms ile PCL Kitaplığı'nda SQLite ile nasıl çalışılacağını açıklar.

Bu bölümdeki konular, data access veritabanı altyapısı olarak SQLite kullanarak Xamarin.Android tartışın. "Doğrudan" ADO.NET sözdizimini kullanarak veritabanı erişilebilir veya SQLite.NET ORM içerir ve veri işlemleri C# ' de gerçekleştirebilirsiniz.

İki örnek geçirilene: bir metin alanı ve içeren basit bir uygulama çıktıları oluşturma, okuma, çok basit veri erişim kodu içeren bir güncelleştirme ve silme işlevlerinin. Parçacıkları ve önceden girilmiş bir SQLite veritabanı ile uygulamanızı oluşturmak nasıl da açıklanır.

Platformlar arası veri erişimi ek örneklerini görmek için bizim [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) örnek olay incelemesi.


## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android veri tarif](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
