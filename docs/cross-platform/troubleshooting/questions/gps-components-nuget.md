---
title: Google Play birleştirin bileşenleri ve NuGet Hizmetleri
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.openlocfilehash: cfd417f4fc01b07b4334259c45472eb24b73abd8
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Google Play birleştirin bileşenleri ve NuGet Hizmetleri

### <a name="history"></a>Geçmiş

Var. birkaç Google Play Hizmetleri bileşenleri ve NuGet paketleri olması için kullanılır:

-   Google Play hizmetlerini (Froyo)
-   Google Play hizmetlerini (Zencefilli kurabiye)
-   Google Play (ICS) Hizmetleri
-   Google Play hizmetlerini (JellyBean)
-   Google Play hizmetlerini (KitKat)

Google gerçekte yalnızca gelir iki .jar dosyaları Google Play Hizmetleri için:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

Bizim araçları düzgün söyleyin oldu çünkü tutarsızlık var `aapt.exe` maksimum kaynak API düzeyini belirli bir uygulamanın için kullanılacak oluştu. Bu anlamına gelir, Google Play Hizmetleri'nin (KitKat) bağlama Zencefilli kurabiye gibi daha düşük bir API düzeyine kullanarak denedik, derleme hataları aldık.

### <a name="unifying-google-play-services"></a>Google Play hizmetlerini birleştirin

Şimdi biz Xamarin.Android daha yeni sürümlerinde söyleyin `aapt.exe` bu sorunu bize kaybolduktan şekilde kullanmak için hangi en fazla kaynak sürümü.

Yani, Zencefilli kurabiye/ICS/JellyBean / (farklı .jar dosyasına tamamen olduğundan ancak hala ayrı bir bağlama için Froyo ihtiyacımız) KitKat için ayrı paketler için gerçek bir neden yoktur.

Geliştiriciler için işleri kolaylaştırmak için biz şimdi bizim bileşenleri ve NuGet birleşik iki paketlere:

-   Google Play Hizmetleri (Froyo) (bağlar `google-play-services-froyo.jar`)
-   Google Play Hizmetleri (bağlar `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Hangisinin kullanılsın mı?

Neredeyse her durumda, Google Play Hizmetleri'nin kullanılmalıdır. (Froyo) paketini kullanmak için yalnızca, etkin olarak Froyo hedeflediğiniz varsa nedenidir. Froyo gibi küçük bir yüzdesi aygıtlar üzerinde olduğundan bu ayrı .jar dosya Google var tek nedeni, kendilerini bunlar bu .jar dosyasına Google Play Hizmetleri'nin dondurulmuş, desteklenmeyen bir anlık görüntüdür şekilde, destekleme durdurmaya karar verdiniz.

### <a name="note-about-gingerbread"></a>Not Zencefilli kurabiye hakkında

Zencefilli kurabiye varsayılan olarak destek parça yok ve bu nedenle, bazı bağlama sınıflarda Zencefilli kurabiye aygıtta çalışma zamanında bir uygulamada kullanılabilir olmaz. Benzer sınıflar `MapFragment` Zencefilli kurabiye üzerinde çalışmaz ve bunların destek değişken yerine kullanılmalıdır `SupportMapFragment`. Bunu kullanılacak öğrenmek için geliştirici için hazır. Bu uyumsuzluk Google Play Hizmetleri belgelerinde Google tarafından belirtilir.

### <a name="what-happens-to-the-old-componentsnugets"></a>Eski bileşenleri/NuGet için ne olur?

Artık gerekli olmadığından, aşağıdaki bileşenleri/NuGets devre dışı bırakılmış/Delisted sahibiz:

-   Google Play hizmetlerini (Zencefilli kurabiye)
-   Google Play hizmetlerini (JellyBean)
-   Google Play hizmetlerini (KitKat)

Varolan _Google Play Hizmetleri'nı (ICS)_ bileşen/Nuget adlandırılmıştır _Google Play Hizmetleri_ ve ileride güncel tutulacak. Devre dışı bırakılmış/Delisted paketlerden birini başvuran tüm projeleri bunu kullanacak şekilde güncelleştirilmesi.

Devre dışı bileşenler hala var ve bunlar yine de, bunları yeni önlemek için başvurulan projeleri için geri yüklenebilen olmalıdır. Benzer şekilde delisted NuGet paketlerini hala var ve geri yüklenebilir. Bunlar ileride güncelleştirilmez.
