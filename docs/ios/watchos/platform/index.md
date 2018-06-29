---
title: watchOS platform özellikleri
description: Bu belge bağlantılar çeşitli kılavuzlara Apple Pay, bildirimler, zorluklar, öngörülü öneriler, etkinlik uygulamalar ve daha fazla gibi watchOS platform özellikleri açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 16d10dd69223f404aac7c933302992a1544461e9
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066620"
---
# <a name="watchos-platform-features"></a>watchOS platform özellikleri

Bu belge bağlantılar çeşitli kılavuzlara Apple Pay, bildirimler, zorluklar, öngörülü öneriler, etkinlik uygulamalar ve daha fazla gibi watchOS platform özellikleri açıklanmaktadır.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4’e Giriş](introduction-to-watchos4.md)

Bu belge, eklenen ve güncelleştirilen watchOS 4 özellikleri üst düzey bir genel bakış sağlar.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3’e Giriş](introduction-to-watchos3/index.md)

Bu makalede watchOS 3 yeni ve güncelleştirilmiş API'leri açıklanmaktadır.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay geliştirmeleri](~/ios/watchos/platform/apple-pay.md)

WatchOS 3'de, PassKit framework güvenli, uygulama Ödemeler (fiziksel mal ve hizmet için) Apple Watch üzerinde çalışan uygulamalar için desteği sağlamak için genişletilmiştir.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Arka plan görevleri](~/ios/watchos/platform/background-tasks.md)

watchOS 3 uygulama açmadan önce kullanıcının gerektiren içeriğe sahip sağlama bilgilerini güncelleştirmek için kullanabileceğiniz birkaç arka plan görevleri tanıtır.

## <a name="notificationsnotificationsmd"></a>[Bildirimler](notifications.md)

Gözcü uygulamanızda işleme özel bildirim sağlamak öğrenin.

## <a name="complicationscomplicationsmd"></a>[Zorluk Grubu](complications.md)

Gözcü yüz güncel verileri görüntülemek için olası desteği ekleyin.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Öngörülü önerileri](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 sağlayan önceden içinde kullanıcıya bilgi sunmak uygulama bağlamları verilir. Bu özelliği desteklemek için [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) artık içerir `MapItem` diğer uygulamalar tarafından daha sonra kullanmak için konum bilgilerini sağlayın uygulamanın sağlar özelliği.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Hızlı etkileşim teknikleri](~/ios/watchos/platform/quick-interaction-techniques.md)

Sağlayan hızlı kullanıcı etkileşimleri ilgi çekici Apple Watch uygulamaları ve zorluklar oluşturmak için gereklidir. Yeni watchOS 3 için Apple hareketi tanıyıcıları, dijital Dama yapma ve yeni kullanıcı bildirimi ve gezinti teknikleri erişim için destek eklendi. Bu, SceneKit ve SpriteKit, eklenen desteğinin yanı sıra izin Geliştirici hızlı ve esnek zengin, gözatılabilir arabirimleri kolayca oluşturun.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Etkinlik uygulama geliştirmeleri](~/ios/watchos/platform/workout-apps.md)

Yeni watchOS 3 için etkinlik uygulamalar üzerinde Apple Watch arka planda çalıştırma yeteneğini ilişkili. Bu özelliği etkinleştirmek (ve HealthKit verilere erişmek için), uygulama içermelidir `WKBackgroundModes` anahtarını `Info.plist` değeri dosyasıyla `workout-processing`.
