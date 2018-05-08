---
title: Uygulama parçaları - gözden geçirme
description: Bu makalede parçaları Xamarin.Android uygulamaları geliştirmek için nasıl kullanılacağını aracılığıyla anlatılmaktadır.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 92c68298d7abd2570efd89e12d7cfb6364e90972
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="implementing-fragments---walkthrough"></a>Uygulama parçaları - gözden geçirme

_Parçaları ekran boyutlarına çeşitli aygıtlarla hedef Android uygulamalarını karmaşıklığını adres yardımcı olabilecek kendi içinde bulunan, modüler bileşenleridir. Bu makalede oluşturmak ve parçaları Xamarin.Android uygulamaları geliştirirken kullanmak nasıl aracılığıyla anlatılmaktadır._

## <a name="overview"></a>Genel Bakış

Bu bölümde, nasıl oluşturulacağı ve bir Xamarin.Android uygulaması parçaları kullanmak size rehberlik. Bu uygulama bir listede William Shakespeare tarafından birkaç yürütür başlıklarını görüntüler. Kullanıcı bir play başlığında dokunur, uygulama bu play tekliften ayrı bir etkinlikte görüntülenir:

[![Android telefonla dikey modunda çalışan uygulama](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Telefon modu yatay olarak döndürüldüğünde Uygulama görünümünü değiştirir: her iki listesi yürütür ve tırnak işaretleri aynı etkinlik içindeki görüntülenir. Bir Kullan seçildiğinde, teklif aynı etkinlik görünümünde olacaktır:

[![Android telefonla yatay modunda çalışan uygulama](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Son olarak, uygulama tablet çalıştırıyorsa:

[![Android tablet üzerinde çalışan uygulama](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

Bu örnek uygulama kolayca farklı form faktörlerine ve çok az kod değişiklikleri yönleriyle parçaları kullanarak uyarlayabilirsiniz ve [alternatif düzenleri](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources).

Uygulama verileri olarak dize diziler C# uygulamasında sabit kodlanmış olan iki dize dizisi yer alır. Dizi her veri kaynağı için bir parçası olarak hizmet verecektir.  Bir dizi bazı yürütür Shakespeare tarafından adını tutacak ve diğer dizi bu play tekliften tutacaktır. Uygulama başlatıldığında play adlarında görüntülenir bir `ListFragment`. Kullanıcı tıkladığında bir play'de `ListFragment`, uygulama teklif görüntüleyen başka bir etkinliği başlar.

İki düzenleri, biri için dikey ve yatay modu için bir uygulama için kullanıcı arabirimi içerir. Çalışma zamanında Android cihaz yönünü yüklemek için hangi yerleşimi göre belirler ve bu düzen işlemek için etkinliği sağlar. Tüm kullanıcı tıklamalarına yanıt ve verileri görüntülemek için mantığı parçalanma yer alır. Uygulamasında etkinlikler parçaları barındıracak kapsayıcılar olarak mevcut.

Bu kılavuzda iki kılavuzlara ayrıntılarıyla. [İlk bölümü](./walkthrough.md) uygulama çekirdek bölümlerinin odaklanır. İki parça ve iki etkinlik birlikte düzenleri (dikey modu için en iyi duruma getirilmiş) tek bir dizi oluşturulacak:

1. `MainActivity` &nbsp; Bu uygulama için başlangıç etkinliği var.
1. `TitlesFragment` &nbsp; Bu parça William Shakespeare tarafından yazılmış yürütür başlıklarını listesini görüntüler. Tarafından barındırılacak `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` başlayacak `PlayQuoteActivity` yanıt olarak bir oyunda seçerek kullanıcı `TitlesFragment`.
1. `PlayQuoteFragment` &nbsp; Bu parça yürütme tekliften William Shakespeare göre görüntüler. Tarafından barındırılacak `PlayQuoteActivity`.

[İkinci bölümü bu kılavuzun](./walkthrough-landscape.md) , her iki parçada ekranda görüntülenir (yatay modu için en iyi duruma getirilmiş) bir alternatif düzenini ekleme ele alınacaktır. Ayrıca, böylece uygulamanın davranışını eşzamanlı olarak ekranda görüntülenen parça sayısı Uyarlanır bazı küçük kod değişiklikleri koda yapılacaktır.

## <a name="related-links"></a>İlgili bağlantılar

- [FragmentsWalkthrough (örnek)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Tasarımcı genel bakış](~/android/user-interface/android-designer/index.md)
- [Parçaları uygulama](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Destek Paketi](http://developer.android.com/sdk/compatibility-library.html)
