---
title: Nasıl içerik sağlayıcıları çalışma
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: b4c674176be5af09d6b780d79923737a364d1591
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="how-content-providers-work"></a>Nasıl içerik sağlayıcıları çalışma

İki sınıf dahil edilen bir `ContentProvider` etkileşim:

- **ContentProvider** &ndash; standart bir şekilde bir veri kümesi sunan bir API uygular. Sorgu, INSERT, Update ve Delete ana yöntemler şunlardır.

- **ContentResolver** &ndash; iletişim kuran bir statik proxy bir `ContentProvider` herhangi birinden aynı uygulama içinde veya başka bir uygulama verilerine erişmek için.

Bir içerik sağlayıcı normalde bir SQLite veritabanı tarafından desteklenir, ancak kod tüketen temel SQL hakkında bir şey bilmek gerekmez, API anlamına gelir. Sorguları Sabitleri (veri yapılarını bağımlılıkları azaltmak üzere), sütun adları başvurmak için kullanarak bir URI yapılır ve bir `ICursor` üzerinden yinelemek kullanıcı kodu döndürdü.


## <a name="consuming-a-contentprovider"></a>Bir ContentProvider kullanma

`ContentProviders` içinde kayıtlı bir URI üzerinden işlevleri kullanıma **AndroidManifest.xml** uygulamasının veriler yayımlar. Burada URI ve sunulan veri sütunları veri bağlamayı kolaylaştırmak için sabit olarak kullanılabilir bir kuralı yok. Android yerleşik `ContentProviders` tüm kolaylık sınıfları veri yapısında başvuru sabitleri sağlamak [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/) ad alanı.



### <a name="built-in-providers"></a>Yerleşik sağlayıcılar

Android sunar çok çeşitli sistem ve kullanıcı verilerini kullanarak erişimi `ContentProviders`:

- *Tarayıcı* &ndash; yer işaretleri ve tarayıcı geçmişi (iznine gerek duyar `READ_HISTORY_BOOKMARKS` ve/veya `WRITE_HISTORY_BOOKMARKS`).

- *CallLog* &ndash; son çağrıları yapılan veya aygıtla aldı.

- *Kişiler* &ndash; ayrıntılı listesinden kişiler, telefonlar, fotoğraf ve grupları dahil olmak üzere kullanıcının kişi bilgileri.

- *MediaStore* &ndash; kullanıcı cihazının içeriğini: ses (albümleri, sanatçılar, türler, çalma), (küçük resimleri dahil) görüntüler ve görüntü.

- *Ayarları* &ndash; sistem genelinde aygıt ayarları ve tercihleri.

- *UserDictionary* &ndash; Tahmine dayalı metin girişi için kullanılan kullanıcı tanımlı sözlüğün içeriğini.

- *Sesli posta* &ndash; sesli posta iletilerinin geçmişi.



## <a name="classes-overview"></a>Sınıfları genel bakış

İle çalışırken kullanılan birincil sınıfların bir `ContentProvider` burada gösterilir:

[![İçerik sağlayıcı uygulama ve tüketim uygulama etkileşimleri sınıf diyagramı](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

Bu diyagramda `ContentProvider` sorguları uygular ve diğer uygulamaların veri bulmak için kullandığı URI'si kaydeder. `ContentResolver` İçin 'proxy' gibi davranır `ContentProvider` (sorgu, INSERT, Update ve Delete yöntemleri). `SQLiteOpenHelper` Tarafından kullanılan verileri içeren `ContentProvider`, ancak bunu doğrudan uygulamaları kullanan gösterilmez.
`CursorAdapter` Tarafından döndürülen imleç geçirir `ContentResolver` görüntülemek için bir `ListView`. `UriMatcher` Sorguları işlerken URI'ler ayrıştıran bir yardımcı sınıfıdır.

Her sınıf amacı, aşağıda belirtilmiştir:

- **ContentProvider** &ndash; veri göstermek için bu Özet sınıfın yöntemleri uygulayın. Diğer sınıflar ve sınıf tanımına eklenen URI öznitelik aracılığıyla uygulamalar için API kullanılabilir hale getirilir.

- **SQLiteOpenHelper** &ndash; yardımcı uygulama tarafından sunulan SQLite veri deposu `ContentProvider`.

- **UriMatcher** &ndash; kullanım `UriMatcher` içinde `ContentProvider` içeriği sorgulamak için kullanılan URI yönetmenize yardımcı olmak için uygulama.

- **ContentResolver** &ndash; kod tüketen kullanan bir `ContentResolver` erişmek için bir `ContentProvider` örneği. İki sınıf birlikte verilerinin kolayca uygulamalar arasında paylaşılmasına izin vererek, işlemler arası iletişim sorunların dikkatli olun. Kod kullanan hiçbir zaman oluşturur bir `ContentProvider` sınıf açıkça; bunun yerine, verileri tarafından sunulan bir URI dayalı bir imleç tarafından erişilen `ContentProvider` uygulama.

- **CursorAdapter** &ndash; kullanım `CursorAdapter` veya `SimpleCursorAdapter` aracılığıyla erişilen verileri görüntülemek için bir `ContentProvider`.

`ContentProvider` API, veriler üzerinde işlemler, çeşitli gibi gerçekleştirmesine olanak sağlar:

-  Listeleri veya tek tek kayıtları döndürülecek veri sorgu.
-  Tek tek kayıtları değiştirin.
-  Yeni kayıtlar ekleyin.
-  Kayıtları silin.

Bu belge, bir sistem tarafından sağlanan kullanan bir örnek içeriyor `ContentProvider`, özel bir uygulayan basit bir salt okunur örnek yanı sıra `ContentProvider`.

