---
title: Sistem Gereksinimleri
description: Bu belge, Mac ve Windows bilgisayarlarda Xamarin ile uygulama oluşturmaya ilişkin sistem gereksinimleri listeler. Ayrıca, yükleme yönergeleri için bağlantıları.
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: conceptdev
ms.author: crdun
ms.date: 07/24/2018
ms.openlocfilehash: 6d16f01965b6b3bcba35cf14d4000f53a4400653
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241984"
---
# <a name="system-requirements"></a>Sistem Gereksinimleri

' % S'Platformu hedef iOS için Apple ve Google SDK Xamarin ürünleri kullanır veya bizim sistem gereksinimleri theirs eşleşecek şekilde Android. Bu sayfa Xamarin platformunu ve önerilen geliştirme ortamı ve SDK sürümleri için sistemi uyumluluğunu özetler.

- [Geliştirme ortamları](#devenv)
- [macOS gereksinimleri](#mac)
- [Windows gereksinimleri](#windows)

Ziyaret [yükleme yönergeleri](#install) yazılım ve gerekli SDK'ları alma hakkında daha fazla bilgi.

<a name="devenv" />

## <a name="development-environments"></a>Geliştirme ortamları

Bu tabloda, hangi platformları ile farklı bir geliştirme aracı ve işletim sistemi birleşimleri oluşturulabilir gösterilmektedir:

[!include[](~/cross-platform/includes/development-environment.md)]


> [!NOTE]
> İçin Windows bilgisayarlarda olmalıdır iOS geliştirme için bir [Mac bilgisayar ağda erişilebilir](~/ios/get-started/installation/windows/connecting-to-mac/index.md), uzak derleme ve hata ayıklama. Bu durum, ayrıca bir Mac bilgisayarda bir Windows sanal makine içinde çalışan bir Visual Studio varsa çalışır.

<a name="mac" />

## <a name="macos-requirements"></a>macOS gereksinimleri

Xamarin geliştirme için bir Mac bilgisayar kullanarak, aşağıdaki yazılım/SDK sürümleri gerektirir. İşletim sistemi sürümünüzü kontrol edin ve yönergelerini izleyin [Xamarin yükleyici](#install).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode yüklenebilir (ve güncelleştirilmiş) üzerinde [developer.apple.com](https://developer.apple.com/xcode/download/) veya Mac App Store aracılığıyla.

### <a name="testing--debugging-on-macos"></a>Sınama ve macOS üzerinde hata ayıklama

Xamarin mobil uygulamaları için test ve hata ayıklama (Xamarin.Mac uygulamaları doğrudan geliştirme bilgisayarında; test edilebilir fiziksel cihaz USB aracılığıyla dağıtılabilir Apple Watch uygulamaları ilk eşleştirilmiş İphone'a dağıtılır).

[!include[](~/cross-platform/includes/macos-testing.md)]

<a name="windows" />

## <a name="windows-requirements"></a>Windows gereksinimleri

Xamarin geliştirme için Windows bilgisayar kullanarak, aşağıdaki yazılım/SDK sürümleri gerektirir.
İşletim sistemi sürümünüzü kontrol edin (ve onaylayın, kullanmadığınız bir *Express* - Öyleyse, Visual Studio sürümü için güncelleştiriliyor göz önünde bulundurun bir *topluluk* sürümü).
Visual Studio 2017 yükleyicisi Xamarin otomatik olarak yüklemek için bir seçenek içerir (**.NET ile Mobil Geliştirme**).

[!include[](~/cross-platform/includes/windows-requirements.md)]

> [!NOTE]
>
>- Visual Studio için Xamarin, tüm Visual Studio 2017 (Community, Professional ve Enterprise) destekler.
>
>- Evrensel Windows Platformu (UWP) için Xamarin.Forms uygulamaları geliştirmek için Visual Studio 2017 ile Windows 10 gerektirir.

### <a name="testing--debugging-on-windows"></a>Sınama ve Windows üzerinde hata ayıklama

Xamarin mobil uygulamaları, test ve hata ayıklama (iOS cihazlar Mac bilgisayara, Visual Studio çalıştıran bilgisayarın bağlanması gerekir) için USB aracılığıyla fiziksel cihazlara dağıtılabilir.

[!include[](~/cross-platform/includes/windows-testing.md)]

<a name="install" />

## <a name="installation-instructions"></a>Yükleme Yönergeleri

MacOS için en yeni Xamarin sürümü yüklenebilir [xamarin.com/download](http://xamarin.com/download). Windows için izleyin [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) yükleme yönergeleri.

Bizim geçerli ürün sürümleri tam listesi edinilebilir [geçerli sürümler sayfasından](http://developer.xamarin.com/releases/current/). Bu sayfa ayrıca tek tek ürün sürümleri (ve bağlantılar için sürüm notları) bizim beta ve alfa kanalları özetlenmektedir.

Belirli [yükleme](~/cross-platform/get-started/installation/index.md) her platform için yönergeleri burada bulabilirsiniz:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

Ayrıca ek bilgiler var. hakkında [Xamarin.Forms gereksinimleri ve desteklenen platformlar](~/xamarin-forms/get-started/installation.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin'i indirin](https://visualstudio.microsoft.com/xamarin/)
- [Geçerli sürümler](https://developer.xamarin.com/releases/current/)
