---
title: MacOS Mojave ile çalışmaya başlama
description: Bu belge, derleme macOS Xamarin.Mac Mojave uygulamalarla kadar Kurulum açıklar. Xcode 10 indirin ve Mac için Visual Studio güncelleştirme işleminin nasıl açıklanır
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831351"
---
# <a name="getting-started-with-macos-mojave"></a>MacOS Mojave ile çalışmaya başlama

![Önizleme](~/media/shared/preview.png)

> [!WARNING]
> Xamarin'in macOS Mojave desteği, hatalar içerebilir, özelliği tamamlamak değil ve değişebilir Önizleme aşamasında olan.
> Yalnızca deneme kullanın.

> [!NOTE]
> Daha fazla bilgi için okuma [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/) için Xamarin Önizleme sürümü.

Bu belge, derleme macOS Xamarin.Mac Mojave uygulamalarla kadar Kurulum açıklar. Xcode 10 indirin ve Mac için Visual Studio güncelleştirme işleminin nasıl açıklanır

## <a name="download-and-install"></a>İndirme ve yükleme

1. **En yeni Xcode 10 beta yükleme** – kayıtlı Apple geliştiriciler, indirin ve Xcode 10'dan en son sürümünü yükleyin [Apple Developer Portal'a](https://developer.apple.com/download/).

   > [!NOTE]
   > MacOS Mojave SDK'sını Xcode 10 ile dağıtılır.

2. **Xcode 10 çalıştıran** – 10 güncelleştiriliyor ve; Mac için Visual Studio çalıştıran önce çalıştırmak Xcode, Xamarin gerektiren bazı araçları yükler.

3. **Mac için Visual Studio güncelleştirme** – yönergeleri [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin Önizleme yüklemek için.

4. _(isteğe bağlı)_  **En yeni macOS Mojave beta Mac bilgisayarınıza yüklemek** – yeni tanıtılan macOS Mojave API'leri, kayıtlı Apple geliştiriciler can kullanan Xamarin.Mac uygulamaları test etmek için [indirme](https://developer.apple.com/download/) ve yükleme en yeni macOS Mojave Geliştirici beta.

   > [!TIP]
   > Uygulamanızı herhangi bir yeni macOS Mojave API'leri kullanmıyor olsa bile, macOS Mojave (Xcode 10 ile yüklü) SDK'sı ile derleyin ve her şeyin beklendiği gibi çalıştığından emin olmak için test emin olun. Bir uygulama yeni bir API çağrısı değil, macOS Mojave SDK'sı ile derleyin ve, Mac işletim sistemi yükseltme yapmadan test edebilirsiniz.

   > [!IMPORTANT]
   > Mac için macOS Mojave oluşturmak ve yeni macOS Mojave API'leri çağıran Xamarin.Mac uygulamaları test etmek için yükseltmeden önce:
   > - Okuma [Apple'nın sürüm notları](https://developer.apple.com/download/) işletim sistemi güncelleştirmesi.
   > - Okuma [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/) için Xamarin Önizleme sürümü. Bu ilk önizleme yeni macOS Mojave AppKit API'leri (örneğin, koyu modu) için bağlamaları içerip içermediğini unutmayın.

## <a name="related-links"></a>İlgili bağlantılar

- [Xcode 10 indirin](https://developer.apple.com/download/)
- Xamarin Önizleme [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/)
