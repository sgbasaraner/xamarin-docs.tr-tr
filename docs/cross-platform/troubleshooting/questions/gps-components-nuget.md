---
title: Birleştirme Google Play Services bileşenleri ile Nuget'i
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 3f5c5f75ae1c7a44537afa59ff4a15d54b1df50b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351489"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Birleştirme Google Play Services bileşenleri ile Nuget'i

## <a name="history"></a>Geçmiş

Orada kullanılan birkaç Google Play Services bileşenleri ve NuGet paketleri olarak:

-   Google Play Hizmetleri (Froyo)
-   Google Play Hizmetleri (Gingerbread)
-   Google Play Hizmetleri (ICS)
-   Google Play Hizmetleri (JellyBean)
-   Google Play Hizmetleri (KitKat)

Google aslında yalnızca iki gemileri .jar dosyalarını Google Play Hizmetleri için:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

Sunduğumuz araç düzgün bir şekilde anlatın olmadı çünkü tutarsızlık varolan `aapt.exe` API düzeyi en fazla kaynak için belirli bir uygulamanın kullanılacak olan. Bu geliyordu, Google Play Hizmetleri (KitKat) bağlama Gingerbread gibi daha düşük bir API düzeyi kullanarak çalıştık, derleme hataları aldık.

## <a name="unifying-google-play-services"></a>Google Play Hizmetleri altyapınızı kurun

Xamarin.Android daha yeni sürümlerinde, biz artık söyleyin `aapt.exe` Bu sorun bizim için kaybolduktan şekilde kullanmak için hangi en fazla kaynak sürümü.

Diğer bir deyişle, Zencefilli kurabiye/ICS/JellyBean / (farklı .jar dosyasını tamamen olduğundan ancak yine de ayrı bir bağlama için Froyo ihtiyacımız) KitKat için ayrı paketler olması için gerçek bir neden yoktur.

Geliştiriciler için işleri kolaylaştırmak için size şimdi bizim bileşenleri ile Nuget'i birleşik iki paketlere:

-   Google Play Hizmetleri (Froyo) (bağlar `google-play-services-froyo.jar`)
-   Google Play Hizmetleri (bağlar `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Hangisinin kullanılmalıdır?

Neredeyse her durumda, Google Play Hizmetleri kullanılmalıdır. (Froyo) paketini kullanmak üzere yalnızca etkin bir şekilde Froyo hedeflediğiniz varsa nedenidir. Froyo gibi küçük bir yüzdesine cihazlar üzerinde olduğundan bu ayrı .jar dosyasını Google'dan var. tek nedeni, kendilerine bu .jar dosyasını dondurulmuş, desteklenmeyen anlık görüntüsünü Google Play Hizmetleri, bu nedenle, desteğini durduracak karar verdiniz.

### <a name="note-about-gingerbread"></a>Not Gingerbread hakkında

Gingerbread varsayılan olarak destek parça yok ve bu nedenle, bazı bağlama sınıflarda Gingerbread cihazda çalışma zamanında bir uygulamada kullanılamaz. Sınıfların `MapFragment` Zencefilli kurabiye üzerinde çalışmaz ve kendi destek değişken bunun yerine kullanılması gereken `SupportMapFragment`. Bunu kullanmak için bilmeniz gereken geliştiriciler için hazır. Bu uyumsuzluk, Google Play Hizmetleri belgelerinde Google tarafından belirtilir.

### <a name="what-happens-to-the-old-componentsnugets"></a>Eski bileşenleri/NuGet için ne olacak?

Artık gerekli olmadığından, aşağıdaki bileşenleri/Nuget'i devre dışı bırakılmış/Delisted sunuyoruz:

-   Google Play Hizmetleri (Gingerbread)
-   Google Play Hizmetleri (JellyBean)
-   Google Play Hizmetleri (KitKat)

Varolan _Google Play Hizmetleri (ICS)_ bileşeni/Nuget adlandırıldı _Google Play Hizmetleri_ ve ileride güncel tutulur. Devre dışı bırakılmış/Delisted paketlerden birini başvuran tüm projeler bunu kullanmak için güncelleştirilmesi gerekir.

Devre dışı bileşenlerin hala var ve bunlar yine de, bunları bozmayı önlemek için başvurulan projeler için geri yüklenebilen olmalıdır. Benzer şekilde delisted NuGet paketlerini hala mevcut ve geri yüklenebilir. Kullanıcılar bundan sonra güncelleştirilmez.
