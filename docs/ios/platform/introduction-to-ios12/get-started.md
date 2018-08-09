---
title: İOS 12, 12 tvOS ve watchOS 5 ile çalışmaya başlama
description: Bu belge, en fazla 12 yapı iOS, tvOS 12 ve Xamarin ile watchOS 5 uygulamaları Kurulum açıklar. Bu, Xcode 10 indirin ve Mac ve Visual Studio 2017 için Visual Studio'yu güncelleştirmek nasıl ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/07/2018
ms.openlocfilehash: cb84ddc646933d253ca72fe8f9f581364aab698b
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615180"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>İOS 12, 12 tvOS ve watchOS 5 ile çalışmaya başlama

![Önizleme](~/media/shared/preview.png)

> [!WARNING]
> Xamarin'in iOS 12, 12 tvOS ve watchOS 5 SDK'ları Xcode 10 ile dağıtılmış desteği hataları içerebilir, özelliği anlamına gelir tamamlamak ve değişebilir Önizleme aşamasında olan. Yalnızca deneme kullanın.

Bu belge Xcode 10 ile yayımlanan API'leri çağırmak Xamarin uygulamaları oluşturmak için ayarlama yapma açıklar. Bu, Xcode 10 indirin ve Mac ve Visual Studio 2017 için Visual Studio'yu güncelleştirmek nasıl ele alınmaktadır.

## <a name="download-and-install"></a>İndirme ve yükleme

1. **En yeni Xcode 10 beta yükleme** – kayıtlı Apple geliştiriciler, indirin ve Xcode 10'dan en son sürümünü yükleyin [Apple Developer Portal'a](https://developer.apple.com/download/).

2. **Xcode 10 çalıştıran** – çalıştırma Xcode, Xamarin araçları güncelleştiriliyor ve bazı yüklerken, Mac veya Visual Studio 2017 için Visual Studio çalıştıran önce 10 gerektirir.

3. **Mac ve Visual Studio 2017 için Visual Studio güncelleştirme** – yönergelerini izleyin [blog yayın](https://releases.xamarin.com/preview-release-xcode-10-beta-5/) Xamarin Önizleme yüklemek için.

4. _(isteğe bağlı)_  **En son iOS beta iOS cihazlarınıza yükleyebilir** – Xcode 10, kayıtlı Apple geliştiriciler can sunulan API'leri kullanan uygulamalar, cihaz test için [indirme](https://developer.apple.com/download) ve en son sürümünü yükleyin Geliştirici betalar cihazlarından.

   > [!TIP]
   > Uygulamanızı herhangi bir yeni API'ler kullanmıyor olsa bile, en yeni Xcode 10 SDK'ları ile derleyin ve her şeyin beklendiği gibi çalıştığından emin olmak için test emin olun. Bir uygulama yeni bir API çağrısı değil, bu yeni SDK'ları ile derleyin ve yeni işletim sistemine henüz yükseltilmemiş cihazlarda test edin.
   >
   > Cihazlarınız için en son işletim sistemi yükseltme, Xamarin test edebileceğiniz Apple'dan serbest önce emin olun:
   >
   > - Okuma [Apple'nın sürüm notları](https://developer.apple.com/download/) işletim sistemi güncelleştirmeleri.
   > - Xamarin Önizleme okuma [sürüm blog gönderisini](https://releases.xamarin.com/preview-release-xcode-10-beta-5/).

## <a name="related-links"></a>İlgili bağlantılar

- [Xcode 10 indirin](https://developer.apple.com/download/)
- Xamarin Önizleme [sürüm blog gönderisi](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
