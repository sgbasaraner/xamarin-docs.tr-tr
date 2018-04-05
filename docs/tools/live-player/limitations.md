---
title: Sınırlamalar
description: Xamarin Canlı Player bazı sınırlamaları
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: 43699e77bc8b7365c6d5f7fbf27e19a945e69e22
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="limitations"></a>Sınırlamalar

![Önizleme özelliği](~/media/shared/preview.png)

## <a name="device-requirements"></a>Cihaz gereksinimleri
Xamarin Canlı oynatıcı uygulaması aşağıdaki cihazları destekler:

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 veya üzeri.
- ARM-v7a, ARM v8a, ARM64-v8a, x 86 veya x86_64 işlemci.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 veya üzeri.
- ARM64 işlemci.

-----

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki öğeler de dahil olmak üzere Xamarin Canlı Player çalıştırabilirsiniz şeyler bazı sınırlamalar vardır:

### <a name="xamarinforms"></a>Xamarin.Forms
- Özel oluşturucu desteklenmez.
- Efektleri desteklenmez.
- Özel denetimler özel bağlanabilir özellikleri ile desteklenmez.
- Katıştırılmış kaynakları desteklenmez (IE. görüntüleri veya başka kaynaklara bir PCL katıştırma).
- Üçüncü taraf MVVM çerçeveler (örn. desteklenmez Prizma, Mvvm arası, Mvvm hafif, vb.).
- İOS varlık kataloglar desteklenmez.

### <a name="other-project-types"></a>Diğer proje türleri
- Canlı Player yerel Android veya (kullanan Android XML veya film şeritleri için kullanıcı arabirimi) iOS projeler için tasarlanmamıştır.

### <a name="misc"></a>Çeşitli
- Sınırlı yansıma desteği (şu anda SQLite ve Json.NET gibi bazı yaygın NuGets etkiler). Diğer NuGets hala desteklenmiyor olabilir.
- Bazı sistem sınıfları kılınamaz (örneğin, bir alt sınıfı uygulayamaz).
- Sağlama gerektiren bazı platform özellikleri (ancak bu Fotoğraf Galerisi erişim gibi ortak işlemleri için yapılandırılmış) Xamarin Canlı Player uygulamasında çalışamaz.
- Derleme adımları ve özel hedefleri göz ardı edilir. Örneğin, Fody, Refit, AutoFac ve AutoMapper gibi araçlar dahil edilemez.
- F # projeleri Android desteklenmiyor ve sınırlı destek iOS
- Özel genel sınıfları ve arabirimleri ile Gelişmiş senaryolar desteklenmiyor.

Lütfen ek sorunları hakkında rapor [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>İlgili bağlantılar

- [Sorun giderme](~/tools/live-player/troubleshooting.md)
- [Kurulum](~/tools/live-player/install.md)
