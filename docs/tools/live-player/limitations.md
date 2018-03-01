---
title: "Sınırlamalar"
description: "Xamarin Canlı Player bazı sınırlamaları"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: d551c0a82fb8f970ce5a00ec9e64ac7f49c81a44
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>Sınırlamalar

![Önizleme özelliği](~/media/shared/preview.png)

## <a name="device-requirements"></a>Cihaz gereksinimleri
Xamarin Canlı oynatıcı uygulaması aşağıdaki cihazları destekler:

### <a name="ios"></a>iOS
- iOS 9.0 veya üzeri.
- ARM64 işlemci.
- Denetleme [App Store](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?mt=8) desteklenen aygıtların listesi.

### <a name="android"></a>Android
- Android 4.2 veya üzeri.
- ARM-v7a, ARM v8a, ARM64-v8a, x 86 veya x86_64 işlemci.


## <a name="limitations"></a>Sınırlamalar

Aşağıdaki öğeler de dahil olmak üzere Xamarin Canlı Player çalıştırabilirsiniz şeyler bazı sınırlamalar vardır:

### <a name="android"></a>Android
- Android kullanıcı arabirimleri AXML dosyalarıyla tasarlanmış şu anda desteklenmemektedir.

### <a name="ios"></a>iOS
- Bazı iOS film şeridi özellikler desteklenmez.
- iOS XIB dosyaları desteklenmez.

### <a name="xamarinforms"></a>Xamarin.Forms
- Özel oluşturucu desteklenmez.
- Efektleri desteklenmez.
- Özel denetimler özel bağlanabilir özellikleri ile desteklenmez.
- Katıştırılmış kaynakları desteklenmez (IE. görüntüleri veya başka kaynaklara bir PCL katıştırma).

### <a name="misc"></a>Çeşitli
- Sınırlı yansıma desteği (şu anda SQLite ve Json.NET gibi bazı yaygın NuGets etkiler). Diğer NuGets hala desteklenmektedir.
- Bazı sistem sınıfları kılınamaz (örneğin, bir alt sınıfı uygulayamaz).
- Sağlama gerektiren bazı platform özellikleri (ancak bu Fotoğraf Galerisi erişim gibi ortak işlemleri için yapılandırılmış) Xamarin Canlı Player uygulamasında çalışamaz.
- Derleme adımları ve özel hedefleri göz ardı edilir. Örneğin, Fody gibi araçlar dahil edilemez.

> [!WARNING]
> **Not:** Xamarin Canlı Player Xamarin Studio ile çalışmaz

Lütfen ek sorunları hakkında rapor [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>İlgili bağlantılar

- [Sorun giderme](~/tools/live-player/troubleshooting.md)
- [Kurulum](~/tools/live-player/install.md)
