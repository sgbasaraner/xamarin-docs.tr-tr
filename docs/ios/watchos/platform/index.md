---
title: "Platform özellikleri"
description: "WatchOS uygulamalarında dahil etmek için Apple Watch özgü özellikler."
ms.topic: article
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 16e779761aa164ea9890547e3baca527a3592021
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="platform-features"></a>Platform özellikleri

_WatchOS uygulamalarında dahil etmek için Apple Watch özgü özellikler._

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay geliştirmeleri](~/ios/watchos/platform/apple-pay.md)

WatchOS 3'de, PassKit framework güvenli, uygulama Ödemeler (fiziksel mal ve hizmet için) Apple Watch üzerinde çalışan uygulamalar için desteği sağlamak için genişletilmiştir.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Arka plan görevleri](~/ios/watchos/platform/background-tasks.md)

watchOS 3 uygulama açmadan önce kullanıcının gerektiren içeriğe sahip sağlama bilgilerini güncelleştirmek için kullanabileceğiniz birkaç arka plan görevleri tanıtır.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[WatchOS 4 giriş](introduction-to-watchos4.md)

WatchOS 4 yeni özellikler.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[WatchOS 3 giriş](introduction-to-watchos3/index.md)

Bu makalede, tüm yeni ve değiştirilen API'ler watchOS 3 kullanılabilir Xamarin geliştiriciler için tanıtılır.

##  <a name="notificationsnotificationsmd"></a>[Bildirimleri](notifications.md)

Gözcü uygulamanızda işleme özel bildirim sağlamak öğrenin.

##  <a name="complicationscomplicationsmd"></a>[Zorluklar](complications.md)

Gözcü yüz güncel verileri görüntülemek için olası desteği ekleyin.


## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Öngörülü önerileri](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 sağlayan önceden içinde kullanıcıya bilgi sunmak uygulama bağlamları verilir. Bu özelliği desteklemek için [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) artık içerir `MapItem` diğer uygulamalar tarafından daha sonra kullanmak için konum bilgilerini sağlayın uygulamanın sağlar özelliği.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Hızlı etkileşim teknikleri](~/ios/watchos/platform/quick-interaction-techniques.md)

Sağlayan hızlı kullanıcı etkileşimleri ilgi çekici Apple Watch uygulamaları ve zorluklar oluşturmak için gereklidir. Yeni watchOS 3 için Apple hareketi tanıyıcıları, dijital Dama yapma ve yeni kullanıcı bildirimi ve gezinti teknikleri erişim için destek eklendi. Bu, SceneKit ve SpriteKit, eklenen desteğinin yanı sıra izin Geliştirici hızlı ve esnek zengin, gözatılabilir arabirimleri kolayca oluşturun.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Etkinlik uygulama geliştirmeleri](~/ios/watchos/platform/workout-apps.md)

Yeni watchOS 3 için etkinlik uygulamalar üzerinde Apple Watch arka planda çalıştırma yeteneğini ilişkili. Bu özelliği etkinleştirmek (ve HealthKit verilere erişmek için), uygulama içermelidir `WKBackgroundModes` anahtarını `Info.plist` değeri dosyasıyla `workout-processing`.
