---
title: watchOS platform özellikleri
description: Bu belge bağlantıları çeşitli kılavuzlara Apple Pay, bildirimler, zorluk, proaktif öneriler, etkinlik uygulamaları ve daha fazlası gibi watchOS platform özellikleri açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: 09200ba5968838edf829b30a50a8ad0f4a3ab3aa
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39030684"
---
# <a name="watchos-platform-features"></a>watchOS platform özellikleri

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[watchOS 5’e Giriş](introduction-to-watchos5/index.md)

Bu belge, Xamarin ile watchOS uygulamaları oluştururken kullanılabilecek watchOS 5'deki yeni ve güncelleştirilmiş özellikler üst düzey bir genel bakış sağlar.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4’e Giriş](introduction-to-watchos4.md)

Bu belge, eklenen ve güncelleştirilen watchOS 4 içinde özellikleri üst düzey bir genel bakış sağlar.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3’e Giriş](introduction-to-watchos3/index.md)

Bu makalede, watchOS 3 yeni ve güncelleştirilmiş API'leri açıklar.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay geliştirmeleri](~/ios/watchos/platform/apple-pay.md)

WatchOS 3'de, PassKit framework Apple Watch üzerinde çalışan uygulamalar için güvenli, uygulama içi Ödemeler (of, fiziksel mal ve Hizmetler) için destek izin verecek şekilde genişletildi.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Arka plan görevleri](~/ios/watchos/platform/background-tasks.md)

watchOS 3 uygulama açmadan önce kullanıcının gereken içeriğe sahip olduğundan emin olmakta bilgilerini güncelleştirmek için kullanabileceğiniz birden fazla arka plan görevleri tanıtır.

## <a name="notificationsnotificationsmd"></a>[Bildirimler](notifications.md)

Watch uygulaması işleme özel bir bildirim sağlamak öğrenin.

## <a name="complicationscomplicationsmd"></a>[Zorluk Grubu](complications.md)

Saat kadranı hakkında güncel verileri görüntülemek için karmaşıklık desteği eklendi.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Proaktif öneriler](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 sağlayan proaktif olarak kullanıcıya bilgi göstermek uygulama bağlamları verilir. Bu özelliği desteklemek için [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) artık `MapItem` diğer uygulamalar tarafından daha sonra kullanmak için konum bilgileri sağlayan uygulama olanak tanıyan özellik.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Hızlı etkileşim teknikleri](~/ios/watchos/platform/quick-interaction-techniques.md)

Sağlayarak Hızlı Kullanıcı etkileşimlerine, ilgi çekici Apple Watch uygulamaları ve zorluklar oluşturmak için gereklidir. Yeni watchOS 3 için Apple hareket tanıyıcılar, dijital Dama yapma ve yeni kullanıcı bildirimi ve gezinti teknikleri erişim için destek eklendi. Bu, eklenen desteğiyle SceneKit hem SpriteKit, birlikte izin kolayca hızlı ve esnek glanceable, zengin arabirimler oluşturmak Geliştirici.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Etkinlik uygulama geliştirmeleri](~/ios/watchos/platform/workout-apps.md)

Yeni uygulamanız üzerinde Apple Watch arka planda çalıştırma olanağı, watchOS 3 için etkinlik ilgili. Bu özelliği etkinleştirmek (ve HealthKit verilere erişim için), uygulama içermelidir `WKBackgroundModes` anahtarını `Info.plist` değeri dosyasıyla `workout-processing`.
