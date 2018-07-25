---
title: Android veri depolamaya giriş
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2017
ms.openlocfilehash: 26576fe31919822237022572a4e490cf6fc19d65
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241451"
---
# <a name="introduction"></a>Giriş

## <a name="when-to-use-a-database"></a>Bir veritabanını kullanmak ne zaman

Mobil cihazları depolama ve işleme özelliklerini artan sırada telefonlar ve tabletler hala Masaüstü ve dizüstü karşılıkları lag. Bu nedenle uygulamanızın veri depolama mimarisi planlamak için bazı zaman ayırdığınız yerine değer yalnızca doğru yanıt her zaman bir veritabanı olduğunu varsayarak olacaktır. Bir dizi gibi farklı gereksinimleri karşılayacak farklı seçenekler vardır:

-  **Tercihler** – Android veri basit anahtar-değer çiftlerini depolamak için yerleşik bir mekanizma sunar. Basit kullanıcı ayarlarını veya verileri (örneğin, kişiselleştirme bilgisinin) küçük parçaları depoluyorsanız bu tür bilgileri depolamak için platformun yerel özellikleri kullanın.
-  **Metin dosyaları** – kullanıcı girişini veya önbelleklerini indirilen içerik (örn.) HTML) doğrudan dosya sistemi üzerinde depolanabilir. Dosyaları düzenlemek ve veri bulmanıza yardımcı olması için uygun bir dosya adlandırma kuralını kullanın.
-  **Seri veri dosyalarını** – kalıcı nesneler XML veya JSON dosya sisteminde. .NET framework serileştirme ve seri durumundan nesneleri kolay hale kitaplıkları içerir. Veri dosyalarını düzenlemek için uygun adlar kullanın.
-  **Veritabanı** – Android platformunda SQLite veritabanı altyapısı kullanılabilir ve depolamak yararlı yapılandırılmış sorgu, sıralama veya aksi halde işlemek için gereken verileri. Veritabanı depolaması, veri ile pek çok özellik listeleri için uygundur.
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

SQLite mobil platformları için Google tarafından benimsenmiştir bir açık kaynak veritabanı altyapısıdır. Geliştiricilerin yararlanmak ek bir iş olduğundan SQLite veritabanı altyapısı her iki işletim sistemleri içinde yerleşiktir. SQLite için platformlar arası Mobil Geliştirme için uygundur:

-  Veritabanı altyapısı, küçük, hızlı ve kolay taşınabilir.
-  Bir veritabanını tek bir mobil cihazları yönetmek kolay olan dosyaya depolanır.
-  Dosya biçimi platformlar arasında kolayca kullanılabilir: olmadığını 32 veya 64-bit ve büyük veya küçük-endian sistemler.
-  Standart SQL92 çoğunu uygular.


SQLite, küçük ve daha hızlı olacak şekilde tasarlandığından, kullanımı üzerinde bazı uyarılar vardır:

-  Bazı OUTER JOIN söz dizimi desteklenmiyor.
-  Yeniden adlandırma yalnızca tablo ve ADDCOLUMN desteklenir. Şemanızı başka değişiklikler gerçekleştirilemiyor.
-  Görünümler, salt okunurdur.


SQLite hakkında daha fazla Web sitesinde - edinebilirsiniz [SQLite.org](http://SQLite.org) - ancak SQLite Xamarin ile kullanmak gereken tüm bilgileri bu belgede yer alan ve örnekleri ilişkili. SQLite veritabanı altyapısının içinde Android Android 2'den desteklenen.
Bu bölümde ele alınmamaktadır olsa da, SQLite, ayrıca Windows Phone ve Windows uygulamalarını kullanılabilir.

## <a name="windows-and-windows-phone"></a>Windows ve Windows Phone

Bu platformlar bu belgede ele alınmayan rağmen SQLite de Windows platformlarında kullanılabilir.
Daha fazla bilgi [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) ve [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) case incelemeleri ve gözden geçirme [Tim Heuer'ın blog](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).


## <a name="related-links"></a>İlgili bağlantılar

- [DataAccess Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android veri tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
