---
title: watchOS Platform özellikleri
description: Bu belge bağlantılar çeşitli kılavuzlara Apple Pay, bildirimler, zorluklar, öngörülü öneriler, etkinlik uygulamalar ve daha fazla gibi watchOS platform özellikleri açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 8ad4dc52c3bca0f54adb64bb97acaa23aeb1e590
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791299"
---
# <a name="watchos-platform-features"></a>watchOS Platform özellikleri

_WatchOS uygulamalarında dahil etmek için Apple Watch özgü özellikler._

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay geliştirmeleri](~/ios/watchos/platform/apple-pay.md)

WatchOS 3'de, PassKit framework güvenli, uygulama Ödemeler (fiziksel mal ve hizmet için) Apple Watch üzerinde çalışan uygulamalar için desteği sağlamak için genişletilmiştir.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Arka Plan Görevleri](~/ios/watchos/platform/background-tasks.md)

watchOS 3 uygulama açmadan önce kullanıcının gerektiren içeriğe sahip sağlama bilgilerini güncelleştirmek için kullanabileceğiniz birkaç arka plan görevleri tanıtır.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4’e Giriş](introduction-to-watchos4.md)

WatchOS 4 yeni özellikler.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3’e Giriş](introduction-to-watchos3/index.md)

Bu makalede, tüm yeni ve değiştirilen API'ler watchOS 3 kullanılabilir Xamarin geliştiriciler için tanıtılır.

##  <a name="notificationsnotificationsmd"></a>[Bildirimler](notifications.md)

Gözcü uygulamanızda işleme özel bildirim sağlamak öğrenin.

##  <a name="complicationscomplicationsmd"></a>[Zorluk Grubu](complications.md)

Gözcü yüz güncel verileri görüntülemek için olası desteği ekleyin.


## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Proaktif Öneriler](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 sağlayan önceden içinde kullanıcıya bilgi sunmak uygulama bağlamları verilir. Bu özelliği desteklemek için [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) artık içerir `MapItem` diğer uygulamalar tarafından daha sonra kullanmak için konum bilgilerini sağlayın uygulamanın sağlar özelliği.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Hızlı Etkileşim Teknikleri](~/ios/watchos/platform/quick-interaction-techniques.md)

Sağlayan hızlı kullanıcı etkileşimleri ilgi çekici Apple Watch uygulamaları ve zorluklar oluşturmak için gereklidir. Yeni watchOS 3 için Apple hareketi tanıyıcıları, dijital Dama yapma ve yeni kullanıcı bildirimi ve gezinti teknikleri erişim için destek eklendi. Bu, SceneKit ve SpriteKit, eklenen desteğinin yanı sıra izin Geliştirici hızlı ve esnek zengin, gözatılabilir arabirimleri kolayca oluşturun.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Antrenman Uygulaması Geliştirmeleri](~/ios/watchos/platform/workout-apps.md)

Yeni watchOS 3 için etkinlik uygulamalar üzerinde Apple Watch arka planda çalıştırma yeteneğini ilişkili. Bu özelliği etkinleştirmek (ve HealthKit verilere erişmek için), uygulama içermelidir `WKBackgroundModes` anahtarını `Info.plist` değeri dosyasıyla `workout-processing`.
