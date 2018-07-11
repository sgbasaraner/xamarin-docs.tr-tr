---
title: Xamarin Live Player sınırlamaları
description: Bu belgede, Xamarin Live Player sınırlamaları açıklanmaktadır. Bu cihaz gereksinimleri açıklanır, proje türleri ve diğer çeşitli konuların ile çalışır özellikleri.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815288"
---
# <a name="limitations-of-xamarin-live-player"></a>Xamarin Live Player sınırlamaları

![Önizleme özelliği](~/media/shared/preview.png)

## <a name="device-requirements"></a>Cihaz gereksinimleri
Xamarin Live Player uygulamasını aşağıdaki cihazları destekler:

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 veya üzeri.
- ARM-v7a, v8a ARM, ARM64 v8a, x 86 veya x86_64 işlemci.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 veya üstünde.
- ARM64 işlemci.

-----

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki öğeler de dahil olmak üzere, Xamarin Live Player çalıştırabilirsiniz şeyler bazı sınırlamalar vardır:

### <a name="xamarinforms"></a>Xamarin.Forms

- Özel oluşturucular desteklenmez.
- Efektleri desteklenmez.
- Özel denetimler ile özel bağlanabilir özellikler desteklenmez.
- Gömülü kaynaklar desteklenmez (IE. görüntüleri veya başka kaynaklara bir PCL'de ekleme).
- Üçüncü taraf MVVM çerçeveleri (örn. desteklenmez Prism, Mvvm arası, Mvvm Light, vb.).
- Varlık katalogları ios'ta desteklenmez.

### <a name="other-project-types"></a>Diğer proje türleri

- Live Player, yerel Android veya (Android XML veya film şeritleri için kullanıcı arabirimi kullanan) iOS projeleri için tasarlanmamıştır.

### <a name="misc"></a>Çeşitli

- Sınırlı yansıma için destek (şu anda SQLite ve Json.NET gibi bazı popüler Nuget'i etkiler). Diğer Nuget'i hala desteklenmiyor olabilir.
- Bazı sistem sınıfları geçersiz kılınamaz (örneğin, bir alt uygulayamaz).
- Sağlama gerektiren bazı platform özellikleri (ancak bu Fotoğraf Galerisi erişim gibi yaygın işlemleri için yapılandırılmış) Xamarin Live Player uygulamasında çalışmaz.
- Özel hedefler ve derleme adımları göz ardı edilir. Örneğin, Fody, Refit AutoFac ve AutoMapper gibi araçları dahil edilemez.
- F # projeleri, Android'de desteklenmez ve sınırlı iOS desteği
- Özel genel sınıfları ve arabirimleri ile Gelişmiş senaryolar desteklenmiyor olabilir.

Ek sorunları raporlamak lütfen [bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>İlgili bağlantılar

- [Sorun giderme](~/tools/live-player/troubleshooting.md)
- [Kurulum](~/tools/live-player/install.md)
