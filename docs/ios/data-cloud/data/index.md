---
title: Xamarin.iOS veri erişimi
description: Bu belge bağlantıları kılavuzlara bir Xamarin.iOS uygulaması içinde yerel veritabanlarıyla çalışmayı açıklar. Bağlantılı içeriği SQLite.NET, ADO.NET ve diğer açıklanır.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 017118a74528718ea99fe218f443b8df83d7c52e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241285"
---
# <a name="xamarinios-data-access"></a>Xamarin.iOS veri erişimi

Xamarin.iOS veritabanı erişimi API'leri gibi destekler:

-  ADO.NET çerçevesi.
-  Kitaplık SQLite NET 3. taraf.

Bu kılavuz, veritabanları üst düzey bir genel bakış SQLite veritabanlarına Xamarin.iOS uygulamalarınıza erişmek için ADO.NET ve SQLite.NET ayarlamak nasıl açıklayan önce genel sağlar. 

Bu belgedeki kod çoğunu tamamen platformlar arası ve iOS veya Android üzerinde değişiklik yapılmadan çalışır. Ele alınan iki örnek uygulama vardır:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – basit veri işlemleri Yazar sonuçları bir metin denetimi; görüntüleyin
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – veri işlemleri listeler ve basit veri yapısı düzenlemeleri küçük çalışan bir uygulama tümleştirilir.

Her iki örnek çözüm, iOS ve Android örnek uygulama projeleri içerir.

Xamarin.Forms uygulamaları için okuma [veritabanlarıyla çalışma](~/xamarin-forms/app-fundamentals/databases.md) Xamarin.Forms ile PCL kitaplığında SQLite ile nasıl çalışılacağını açıklar.

## <a name="sections"></a>Bölümler

-  [Giriş](introduction.md)
-  [Yapılandırma](configuration.md)
-  [SQLite.NET ORM Kullanma](using-sqlite-orm.md)
-  [ADO.NET Kullanma](using-adonet.md)
-  [Bir Uygulamadaki Verileri Kullanma](using-data-in-an-app.md)

## <a name="summary"></a>Özet

Bu bölümde SQLite veritabanı altyapısı olarak kullanarak Xamarin.iOS veri erişimi açıklanmıştır. Veritabanı, "doğrudan" ADO.NET söz dizimi kullanılarak erişilebilir veya SQLite.NET ORM içerir ve C# ' de veri işlemleri gerçekleştirebilirsiniz.

Şu iki örneği gözden: çıkış metni alanına ve içeren basit bir uygulama oluşturma, okuma, çok basit veri erişim kodu içeren bir güncelleştirme ve silme işlevi. Ayrıca iş parçacığı oluşturma ve önceden doldurulmuş bir SQLite veritabanından uygulamanızla temel nasıl ele almıştık.

Platformlar arası veri erişim diğer örnekler için sunduğumuz [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) incelemesi.

## <a name="related-links"></a>İlgili bağlantılar

- [DataAccess Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
