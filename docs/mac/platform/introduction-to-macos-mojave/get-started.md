---
title: MacOS Mojave ile çalışmaya başlama
description: Bu belge, yapı macOS Xamarin.Mac Mojave uygulamalarla kadar ayarlanmış almak açıklar. Xcode 10 karşıdan yükleme ve Mac için Visual Studio güncelleştirme açıklanır
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067197"
---
# <a name="getting-started-with-macos-mojave"></a>MacOS Mojave ile çalışmaya başlama

![Önizleme](~/media/shared/preview.png)

> [!WARNING]
> Xamarin'ın macOS Mojave desteği şu anda bu hatalar içerebilir, özelliği tamamlamak değil ve değişebilir anlamına gelir önizlemede değil.
> Yalnızca deneme için kullanın.

> [!NOTE]
> Daha fazla bilgi için okuma [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/) için Xamarin Önizleme sürümü.

Bu belge, yapı macOS Xamarin.Mac Mojave uygulamalarla kadar ayarlanmış almak açıklar. Xcode 10 karşıdan yükleme ve Mac için Visual Studio güncelleştirme açıklanır

## <a name="download-and-install"></a>İndirme ve yükleme

1. **En son Xcode 10 beta yükleme** – kayıtlı Apple geliştiriciler indirip yükleyebileceğiniz Xcode 10'dan en son sürümünü [Apple Geliştirici Portalı](https://developer.apple.com/download/).

   > [!NOTE]
   > MacOS Mojave SDK Xcode 10 ile dağıtılır.

2. **Xcode 10 çalıştıran** – 10 güncelleştirme ve Mac için; Visual Studio çalıştıran önce çalıştırmak Xcode Xamarin gerektiren bazı araçlarını yükler.

3. **Mac için Visual Studio güncelleştirme** –'ndaki yönergeleri izleyin [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin Önizleme yüklemek için.

4. _(isteğe bağlı)_  **Mac'inizde son macOS Mojave beta yükleme** – yeni sunulan macOS Mojave API'leri, kayıtlı Apple geliştiriciler can kullanan Xamarin.Mac uygulamaları test etmek için [karşıdan](https://developer.apple.com/download/) ve yükleme en son macOS Mojave Geliştirici beta.

   > [!TIP]
   > Uygulamanızı herhangi yeni macOS Mojave API'leri kullanmıyor olsa bile macOS Mojave (Xcode 10 ile yüklenen) SDK'sı ile oluşturmanıza ve her şeyin beklendiği gibi çalıştığından emin olmak için test emin olun. Bir uygulama yeni bir API çağrısı değil, Mojave SDK macOS ile derlenir ve Mac'ın işletim sistemi yükseltme yapmadan sınayın.

   > [!IMPORTANT]
   > Mac macOS oluşturmak ve yeni macOS Mojave API çağrısı Xamarin.Mac uygulamaları sınamak için Mojave yükseltmeden önce:
   > - Okuma [Apple'nın sürüm notları](https://developer.apple.com/download/) işletim sistemi güncelleştirmesi.
   > - Okuma [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/) için Xamarin Önizleme sürümü. Bu ilk önizleme yeni macOS Mojave AppKit API'leri (örneğin, koyu modu) için bağlamaları içermediğine dikkat edin.

## <a name="related-links"></a>İlgili bağlantılar

- [Xcode 10 indirin](https://developer.apple.com/download/)
- Xamarin Önizleme [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/)
