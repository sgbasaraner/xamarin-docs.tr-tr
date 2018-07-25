---
title: Xamarin.iOS uygulamaları, verileri depolamaya giriş
description: Bu belge, veri depolama alanı Xamarin.iOS uygulamasının çeşitli yöntem anlatılmaktadır ve SQLite avantajları hakkında belirli bilgiler sağlar.
ms.prod: xamarin
ms.assetid: B1994468-FD06-4FD9-96B3-FCEBB13A972A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: c5324d7e7daa8fbefde1776743dbdc595463fe33
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241155"
---
# <a name="introduction-to-data-storage-in-xamarinios-apps"></a>Xamarin.iOS uygulamaları, verileri depolamaya giriş

## <a name="when-to-use-a-database"></a>Bir veritabanını kullanmak ne zaman

Mobil cihazları depolama ve işleme özelliklerini artan sırada telefonlar ve tabletler hala Masaüstü lag &amp; dizüstü ortaklarınıza. Bu nedenle uygulamanızın veri depolama mimarisi planlamak için bazı zaman ayırdığınız yerine değer yalnızca doğru yanıt her zaman bir veritabanı olduğunu varsayarak olacaktır. Bir dizi gibi farklı gereksinimleri karşılayacak farklı seçenekler vardır:

-  **Tercihler** – iOS veri basit anahtar-değer çiftlerini depolamak için yerleşik bir mekanizma sunar. Basit kullanıcı ayarlarını veya verileri (örneğin, kişiselleştirme bilgisinin) küçük parçaları depoluyorsanız bu tür bilgileri depolamak için platformun yerel özellikleri kullanın. İOS için de bu veriler, hem yedekleme ve eşitleme için birden çok cihazı olan kullanıcıların iCloud eşitleme yararlanabilirsiniz.
-  **Metin dosyaları** – kullanıcı girişini veya önbelleklerini indirilen içerik (örn.) HTML) doğrudan dosya sistemi üzerinde depolanabilir. Dosyaları düzenlemek ve veri bulmanıza yardımcı olması için uygun bir dosya adlandırma kuralını kullanın.
-  **Seri veri dosyalarını** – kalıcı nesneler XML veya JSON dosya sisteminde. .NET framework serileştirme ve seri durumundan nesneleri kolay hale kitaplıkları içerir. Veri dosyalarını düzenlemek için uygun adlar kullanın.
-  **Veritabanı** – SQLite veritabanı altyapısı kullanılabilir iOS ve sorgu, sıralama veya aksi halde yönetmek için ihtiyacınız olan yapılandırılmış verileri depolamak için yararlıdır. Veritabanı depolaması, veri ile pek çok özellik listeleri için uygundur.
-  **Görüntü dosyaları** – ikili verileri veritabanında bir mobil cihazda depolanmasını mümkün olsa da, bunları doğrudan dosya sistemi saklamanız önerilir. Gerekirse, görüntünün diğer verilerle ilişkilendirmek için bir veritabanı dosya adlarını depolayabilirsiniz. Büyük resimler veya çok sayıda görüntü ile ilgilenirken, kullanıcının tüm depolama alanı tüketilmesini önlemek artık gerekli olmayan dosyaları silen bir önbelleğe alma stratejisi planlama iyi bir uygulamadır.


Bir veritabanı uygulamanız için doğru depolama mekanizması varsa, bu belgenin geri kalanında Xamarin platformunda SQLite kullanmayı açıklar.

## <a name="advantages-of-using-a-database"></a>Bir veritabanı kullanmanın avantajları

Mobil uygulamanızda bir SQL veritabanı kullanmanın avantajları vardır:

-  SQL veritabanları, yapılandırılmış verileri verimli depolama sağlar.
-  Belirli veri karmaşık sorgular ile ayıklanabileceği.
-  Sorgu sonuçlarını sıralayabilirsiniz.
-  Sorgu sonuçları toplanabilir.
-  Mevcut veritabanı becerilerini geliştiricilere, veritabanı ve veri erişim kodu tasarım bilgilerini kullanabilir.
-  Sunucu bileşeni, bağlı bir uygulama için veri modelinden mobil uygulamada (tamamen veya kısmen) yeniden kullanılabilir.


## <a name="sqlite-database-engine"></a>SQLite veritabanı altyapısı

SQLite mobil platformları için Apple tarafından benimsenmiştir bir açık kaynak veritabanı altyapısıdır. Geliştiricilerin yararlanmak ek bir iş olduğundan SQLite veritabanı altyapısı için iOS yerleşiktir. SQLite için platformlar arası Mobil Geliştirme için uygundur:

-  Veritabanı altyapısı, küçük, hızlı ve kolay taşınabilir.
-  Bir veritabanını tek bir mobil cihazları yönetmek kolay olan dosyaya depolanır.
-  Dosya biçimi platformlar arasında kolayca kullanılabilir: olmadığını 32 veya 64-bit ve büyük veya küçük-endian sistemler.
-  Standart SQL92 çoğunu uygular.


SQLite, küçük ve daha hızlı olacak şekilde tasarlandığından, kullanımı üzerinde bazı uyarılar vardır:

-  Bazı OUTER JOIN söz dizimi desteklenmiyor.
-  Yeniden adlandırma yalnızca tablo ve ADDCOLUMN desteklenir. Şemanızı başka değişiklikler gerçekleştirilemiyor.
-  Görünümler, salt okunurdur.


SQLite hakkında daha fazla Web sitesinde - edinebilirsiniz [SQLite.org](http://SQLite.org) - ancak SQLite Xamarin ile kullanmak gereken tüm bilgileri bu belgede yer alan ve örnekleri ilişkili. Tüm iOS sürümleri için SQLite veritabanı altyapısı yerleşiktir.
Bu bölümde ele alınmamaktadır olsa da, SQLite, ayrıca Windows Phone ve Windows uygulamalarını kullanılabilir.

## <a name="windows-and-windows-phone"></a>Windows ve Windows Phone

Bu platformlar bu belgede ele alınmayan rağmen SQLite de Windows platformlarında kullanılabilir.
Daha fazla bilgi [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) ve [Tasky Pro](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/building_cross_platform_applications/case_study%3A_tasky) case incelemeleri ve gözden geçirme [Tim Heuer'ın blog](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).



## <a name="related-links"></a>İlgili bağlantılar

- [DataAccess Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
