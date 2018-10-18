---
title: Xamarin Live Player sınırlamaları
description: Bu belgede, Xamarin Live Player sınırlamaları açıklanmaktadır. Bu cihaz gereksinimleri açıklanır, proje türleri ve diğer çeşitli konuların ile çalışır özellikleri.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: aff6990df1b710190f11c2d7fa09c8399e94f8af
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "43780673"
---
# <a name="limitations-of-xamarin-live-player"></a>Xamarin Live Player sınırlamaları

![Önizleme özelliği](~/media/shared/preview.png)

## <a name="device-requirements"></a>Cihaz gereksinimleri

Xamarin Live Player uygulamasını aşağıdaki Android cihazlarına destekler:

- Android 4.2 veya üzeri.
- ARM-v7a, v8a ARM, ARM64 v8a, x 86 veya x86_64 işlemci.

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki öğeler de dahil olmak üzere, Xamarin Live Player çalıştırabilirsiniz şeyler bazı sınırlamalar vardır:

### <a name="ios"></a>iOS

Live Player, iOS için kullanılamıyor.

### <a name="xamarinforms"></a>Xamarin.Forms

- Özel oluşturucular desteklenmez.
- Efektleri desteklenmez.
- Özel denetimler ile özel bağlanabilir özellikler desteklenmez.
- Gömülü kaynaklar desteklenmez (IE. görüntüleri veya başka kaynaklara bir PCL'de ekleme).
- Üçüncü taraf MVVM çerçeveleri (örn. desteklenmez Prism, Mvvm arası, Mvvm Light, vb.).

### <a name="other-project-types"></a>Diğer proje türleri

- Live Player (Android XML için kullanıcı arabirimi kullanan) yerel Android projeleri için tasarlanmamıştır.

### <a name="misc"></a>Çeşitli

- Sınırlı yansıma için destek (şu anda SQLite ve Json.NET gibi bazı popüler Nuget'i etkiler). Diğer Nuget'i hala desteklenmiyor olabilir.
- Bazı sistem sınıfları geçersiz kılınamaz (örneğin, bir alt uygulayamaz).
- Sağlama gerektiren bazı platform özellikleri (ancak bu Fotoğraf Galerisi erişim gibi yaygın işlemleri için yapılandırılmış) Xamarin Live Player uygulamasında çalışmaz.
- Özel hedefler ve derleme adımları göz ardı edilir. Örneğin, Fody, Refit AutoFac ve AutoMapper gibi araçları dahil edilemez.
- F # projelerinde desteklenmez.
- Özel genel sınıfları ve arabirimleri ile Gelişmiş senaryolar desteklenmiyor olabilir.

Kullanım **sorun bildir** içinde [Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017) veya [Mac için Visual Studio](https://docs.microsoft.com/visualstudio/mac/report-a-problem) Xamarin Live Player ile ilgili sorunları bildirmek için.

## <a name="related-links"></a>İlgili bağlantılar

- [Sorun giderme](~/tools/live-player/troubleshooting.md)
- [Kurulum](~/tools/live-player/install.md)
