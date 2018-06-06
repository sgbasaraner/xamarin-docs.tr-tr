---
title: Xamarin.iOS veri erişimi
description: Bu belge bağlantılar kılavuzlara Xamarin.iOS uygulamasının yerel veritabanlarıyla çalışmayı açıklar. Bağlantılı içeriği SQLite.NET, ADO.NET ve daha fazlasını açıklanır.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: a986ea9931f62497e5a6863c84bd4041983d66d9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784582"
---
# <a name="xamarinios-data-access"></a>Xamarin.iOS veri erişimi

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
