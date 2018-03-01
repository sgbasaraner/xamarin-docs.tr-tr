---
title: "Giriş"
ms.topic: article
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: f270248ad3b43d7a4c818b29390b3391d89ddd14
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction"></a>Giriş

## <a name="when-to-use-a-database"></a>Bir veritabanını kullanmak ne zaman

Mobil cihazların depolama ve işleme özelliklerini artan sırada telefonlar ve tabletler hala Masaüstü öteleme &amp; dizüstü ortaklarınıza. Bu nedenle, uygulamanız için veri depolama mimarisi planlamak için bazı zaman ayırdığınız yerine değer yalnızca bir veritabanı her zaman doğru yanıt olduğunu varsayarak olacaktır. Gibi farklı gereksinimlere uyacak farklı seçenekler vardır:

-  **Tercihler** – iOS veri basit anahtar-değer çiftlerini depolamak için yerleşik bir mekanizma sunar. Basit kullanıcı ayarlarını veya veri (örneğin, kişiselleştirme bilgileri) küçük parçalarını depolanıyorsa platformun yerel özelliklerini bu tür bilgileri depolamak için kullanabilirsiniz. İOS için de bu veriler, hem yedekleme ve eşitleme için birden çok cihazlara sahip kullanıcılar için iCloud eşitleme yararlanabilirsiniz.
-  **Metin dosyaları** – kullanıcı girişi veya önbelleklerini indirilen içeriği (ör.) HTML) doğrudan dosya sistemi üzerinde depolanabilir. Dosyaları düzenlemenize ve verileri bulmanıza yardımcı olmak için uygun dosya adlandırma kuralını kullanın.
-  **Seri veri dosyalarını** – nesneleri kalıcı hale XML veya JSON dosya sisteminde. .NET framework seri hale getirme ve seri durumundan nesneleri kolay hale kitaplıklarını içerir. Veri dosyalarını düzenlemek için uygun adlar kullanın.
-  **Veritabanı** – SQLite veritabanı altyapısı kullanılabilir iOS ve sorgu, sıralama veya aksi halde işlemek için gereken yapılandırılmış verileri depolamak için kullanışlıdır. Veritabanı depolama pek çok özellik verilerle listesi için uygundur.
-  **Görüntü dosyaları** – bir mobil cihazda veritabanındaki ikili verileri depolamak mümkün olsa da, bunları doğrudan dosya sistemi içinde depolamanız önerilir. Gerekirse diğer verilerle görüntü ilişkilendirmek için bir veritabanı dosya adlarını depolayabilirsiniz. Görüntülerin büyük veya çok sayıda görüntü ile ilgilenirken, kullanıcının tüm depolama alanı kullanan önlemek için artık gerekli olmayan dosyaları siler bir önbelleğe alma stratejisi planlama iyi bir uygulamadır.


Bir veritabanı, uygulamanız için uygun depolama mekanizmasını ise, bu belgenin geri kalanında Xamarin platformunda SQLite kullanmayı açıklar.

## <a name="advantages-of-using-a-database"></a>Bir veritabanını kullanmanın yararları

Mobil uygulamanızda bir SQL veritabanını kullanmanın avantajları vardır:

-  SQL veritabanları, yapılandırılmış verilerin verimli depolama izin verin.
-  Belirli veri ile karmaşık sorgular ayıklanabilir.
-  Sorgu sonuçları sıralanabilir.
-  Sorgu sonuçları kümelenebilir.
-  Var olan veritabanı becerileri geliştiricilere veritabanı ve veri erişim kodu tasarlamak için bilgilerini kullanabilir.
-  Sunucu bileşeni bağlı bir uygulama, veri modelinden mobil uygulamada (tamamen veya kısmen) yeniden kullanılabilir.


## <a name="sqlite-database-engine"></a>SQLite veritabanı altyapısı

SQLite Apple tarafından kendi mobil platform için benimsenen bir açık kaynak veritabanı altyapısıdır. Bu yüzden, yararlanmak geliştiricilere yönelik ek bir iş SQLite veritabanı altyapısı için iOS yerleşiktir. SQLite için platformlar arası Mobil Geliştirme için uygundur:

-  Veritabanı altyapısı küçük, hızlı ve kolay taşınabilir.
-  Bir veritabanı tek bir mobil aygıtları yönetmek kolay olan dosyaya depolanır.
-  Platformlar arası dosya biçimi kullanımı kolay: olup olmadığını 32 veya 64-bit ve büyük veya küçük-endian sistemleri.
-  Standart SQL92 çoğunu uygular.


SQLite küçük ve hızlı olması için tasarlandığından, kendi kullanımda bazı uyarılar vardır:

-  Bazı OUTER JOIN sözdizimi desteklenmiyor.
-  Yeniden Adlandır yalnızca tablo ve ADDCOLUMN desteklenir. Şemanızı başka değişiklikler gerçekleştiremezsiniz.
-  Görünümler salt okunurdur.


Web sitesinde - SQLite hakkında daha fazla bilgiyi [SQLite.org](http://SQLite.org) - ancak SQLite Xamarin ile kullanmak gereken tüm bilgiler bu belgede yer alan ve örnekleri ilişkilendirilmiş. SQLite veritabanı altyapısı, tüm iOS sürümleri için yerleşiktir.
Bu bölümde kapsanmayan rağmen SQLite, aynı zamanda Windows Phone ve Windows uygulamalarını kullanılabilir.

## <a name="windows-and-windows-phone"></a>Windows ve Windows Phone

Bu platformlar bu belgede ele alınmayan rağmen SQLite Windows platformlarında de kullanılabilir.
Daha fazla okuma [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) ve [Tasky Pro](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/building_cross_platform_applications/case_study%3A_tasky) durumda olay incelemeleri ve gözden [Tim Heuer'ın blogu](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).



## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarif](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
