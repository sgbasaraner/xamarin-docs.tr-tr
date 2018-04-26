---
title: Sistem Gereksinimleri
description: Xamarin kullanmak için ön koşullar
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 08/28/2017
ms.openlocfilehash: ed9992eb162b57cd9c0dd1bc9f4abda4235bac12
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="system-requirements"></a>Sistem Gereksinimleri

_Xamarin kullanmak için ön koşullar_

Platform hedef iOS için Apple ve Google SDK Xamarin ürünleri kullanır veya bizim sistem gereksinimleri kendilerininkini eşleşecek şekilde Android. Bu sayfayı sistem uyumluluk Xamarin platform ve önerilen geliştirme ortamını ve SDK sürümleri için özetlenmektedir.

- [Geliştirme ortamları](#devenv)
- [macOS gereksinimleri](#mac)
- [Windows gereksinimleri](#windows)

Ziyaret [yükleme yönergeleri](#install) yazılım ve gerekli SDK'ları alma hakkında daha fazla bilgi.

<a name="devenv" />

## <a name="development-environments"></a>Geliştirme ortamları

Bu tabloda, hangi platformlarda farklı geliştirme aracı & işletim sistemi birleşimlerine oluşturulabilir gösterilmektedir:

[!include[](~/cross-platform/includes/development-environment.md)]


> [!NOTE]
> Windows bilgisayarlarına olmalıdır iOS için geliştirmeye bir [Mac bilgisayara ağ üzerinde erişilebilir](~/ios/get-started/installation/windows/connecting-to-mac/index.md), uzak derleme ve hata ayıklama için. Bu durum, ayrıca bir Mac bilgisayarda bir Windows VM içinde çalışan Visual Studio yüklüyse çalışır.

<a name="mac" />

## <a name="macos-requirements"></a>macOS gereksinimleri

Xamarin geliştirme için bir Mac bilgisayar kullanarak, aşağıdaki yazılım/SDK sürümleri gerektirir. İşletim sistemi sürümünüzü denetleyin ve yönergeleri izleyin [Xamarin yükleyici](#install).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode yüklenebilir (ve güncelleştirilmiş) üzerinde [developer.apple.com](https://developer.apple.com/xcode/download/) veya Mac App Store aracılığıyla.

### <a name="testing--debugging-on-macos"></a>Sınama & macOS üzerinde hata ayıklama

Xamarin mobil uygulamaları test ve hata ayıklama (Xamarin.Mac uygulamaları doğrudan geliştirme bilgisayarda; sınanabilir için fiziksel cihazlara USB üzerinden dağıtılabilir Apple Watch uygulamaları ilk eşleştirilmiş iPhone dağıtılır).

[!include[](~/cross-platform/includes/macos-testing.md)]


<a name="windows" />

## <a name="windows-requirements"></a>Windows gereksinimleri

Xamarin geliştirme için bir Windows bilgisayar kullanarak, aşağıdaki yazılım/SDK sürümleri gerektirir.
İşletim sistemi sürümünüzü denetleyin (ve onaylayın, kullanmadığınız bir *Express* Visual Studio - Öyleyse sürümünü göz önünde bulundurun güncelleştirme bir *topluluk* edition).
Visual Studio 2015 ve 2017 yükleyicileri Xamarin otomatik olarak yüklemek için bir seçenek içerir.

[!include[](~/cross-platform/includes/windows-requirements.md)]


> [!NOTE]
>
>* Visual Studio için Xamarin tüm Visual Studio 2015 veya 2017 (Community, Professional ve Enterprise) destekler.
>
>* Evrensel Windows Platformu (UWP) Xamarin.Forms uygulamaları geliştirmek için Visual Studio 2015 veya 2017 ile Windows 10 gerektirir.


### <a name="testing--debugging-on-windows"></a>Sınama ve Windows hata ayıklama

Xamarin mobil uygulamalar, test ve hata ayıklama (iOS cihazları Mac bilgisayara, Visual Studio çalıştıran bilgisayarda değil'e bağlı olması gerekir) için fiziksel cihazlara USB üzerinden dağıtılabilir.

[!include[](~/cross-platform/includes/windows-testing.md)]


> [!NOTE]
>
>* [Windows Phone 8.1 öykünücüsü indirme](https://www.microsoft.com/download/details.aspx?id=43719).
>* Windows Phone 10 öykünücüsü Visual Studio 2015 UWP SDK ile birlikte gelir.

<a name="install" />

## <a name="installation-instructions"></a>Yükleme Yönergeleri

En son Xamarin sürüm macOS için yüklenebilir [xamarin.com/download](http://xamarin.com/download). Windows için izleyin [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) yükleme yönergeleri.

Bizim geçerli ürün sürümleri tam listesi edinilebilir [geçerli sürümleri sayfa](http://developer.xamarin.com/releases/current/). Bu sayfa ayrıca ayrı ayrı ürün sürümleri (ve sürüm notları bağlantılar) bizim beta ve alfa kanalları için özetlenir.

Belirli [yükleme](~/cross-platform/get-started/installation/index.md) kullanılabilir burada olan her platform için yönergeler:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

Ayrıca ek bilgiler var. hakkında [Xamarin.Forms gereksinimleri ve desteklenen platformlar](~/xamarin-forms/get-started/installation.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin indirin](https://xamarin.com/download/)
- [Geçerli sürümler](https://developer.xamarin.com/releases/current/)
