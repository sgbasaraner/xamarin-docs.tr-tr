---
title: iOS veri erişimi
description: Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinim vardır. Veri miktarını trivially küçük olmadığı sürece, bu genellikle bir veritabanı ve veri katmanı veritabanı erişimi yönetmek üzere uygulamada gerektirir. iOS "yerleşik" SQLite veritabanı altyapısı vardır ve depolamak ve veri almak için erişim Xamarin'ın platform tarafından basitleştirilmiştir. Bu belge, bir SQLite veritabanına erişmek gösterilmiştir.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 47f2567d81f61568aad639330dc5133856e31936
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="ios-data-access"></a>iOS veri erişimi

_Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinim vardır. Veri miktarını trivially küçük olmadığı sürece, bu genellikle bir veritabanı ve veri katmanı veritabanı erişimi yönetmek üzere uygulamada gerektirir. iOS "yerleşik" SQLite veritabanı altyapısı vardır ve depolamak ve veri almak için erişim Xamarin'ın platform tarafından basitleştirilmiştir. Bu belge, bir SQLite veritabanına erişmek gösterilmiştir._

Xamarin.iOS veritabanı erişimi API'leri gibi destekler:

-  ADO.NET çerçevesi.
-  Kitaplık SQLite NET 3. taraf.

Bu kılavuz SQLite veritabanlarına Xamarin.iOS uygulamalarınıza erişmek için ADO.NET ve SQLite.NET ayarlamak nasıl açıklayan önce veritabanları üst düzey bir genel bakış genel sağlar. 

Bu belge kodda çoğunluğu tamamen platformlar arası ve iOS veya Android üzerinde değişiklik yapılmadan çalışır. Ele alınan iki örnek uygulamaları şunlardır:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – basit veri işlemleri Yazar sonuçları bir metin görüntülemek denetimi;
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – veri işlemleri listeler ve basit veri yapısı düzenlemeleri küçük çalışma uygulamasına tümleştirir.

Her iki örnek çözüm iOS ve Android örnek uygulama projeleri içerir.

Xamarin.Forms uygulamalar için okuma [veritabanlarıyla çalışma](~/xamarin-forms/app-fundamentals/databases.md) Xamarin.Forms ile PCL Kitaplığı'nda SQLite ile nasıl çalışılacağını açıklar.

## <a name="sections"></a>Bölümler

-  [Giriş](introduction.md)
-  [Yapılandırma](configuration.md)
-  [SQLite.NET ORM Kullanma](using-sqlite-orm.md)
-  [ADO.NET Kullanma](using-adonet.md)
-  [Bir Uygulamadaki Verileri Kullanma](using-data-in-an-app.md)


## <a name="summary"></a>Özet

Bu bölümde ele alınan Xamarin.iOS SQLite veritabanı altyapısı kullanılarak veri erişimi. Veritabanı "doğrudan" ADO.NET sözdizimi kullanılarak erişilebilir veya SQLite.NET ORM içerir ve veri işlemleri C# ' de gerçekleştirebilirsiniz.

Şu iki örnek gözden: bir metin alanı ve içeren basit bir uygulama çıktıları oluşturma, okuma, çok basit veri erişim kodu içeren bir güncelleştirme ve silme işlevlerinin. Biz de iş parçacığı oluşturma ve önceden girilmiş bir SQLite veritabanı ile uygulamanızı oluşturmak nasıl açıklanır.

Platformlar arası veri erişimi ek örneklerini görmek için bizim [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) örnek olay incelemesi.

## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarif](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
