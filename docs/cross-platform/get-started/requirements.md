---
title: Sistem Gereksinimleri
description: Bu belge, hem Mac hem de Windows bilgisayarlarda Xamarin ile uygulamalar geliştirmek için sistem gereksinimlerini listeler. Ayrıca yükleme yönergelerine bağlantılar içerir.
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: conceptdev
ms.author: crdun
ms.date: 07/24/2018
ms.openlocfilehash: 422eb24b86ba14ff4e5362db8aeec5775fab5833
ms.sourcegitcommit: aa16f267c59725cc88bd84b049544ecfbec297ac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43263513"
---
# <a name="system-requirements"></a>Sistem gereksinimleri

Xamarin ürünleri, sistem gereksinimlerimizin Apple ve Google sistem gereksinimleriyle eşleşmesi için iOS ve Android’i hedeflemek üzere Apple ve Google’daki platform SDK’larına dayalıdır. Bu sayfa Xamarin platformu için sistem uyumluluğunun yanı sıra önerilen geliştirme ortamı ve SDK sürümlerinin ana hatlarını verir.

Yazılımı ve gerekli SDK’ları edinme hakkında daha fazla bilgi için [yükleme yönergelerine](#installation-instructions) göz atın.

## <a name="development-environments"></a>Geliştirme ortamları

Bu tablo farklı geliştirme aracı ve işletim sistemi birleşimleriyle hangi platformların oluşturulabileceğini gösterir:

[!include[](~/cross-platform/includes/development-environment.md)]

> [!NOTE]
> Windows bilgisayarlarda iOS’a yönelik geliştirme yapmak amacıyla uzaktan derleme ve hata ayıklama için [ağda erişilebilir olan bir Mac bilgisayar](~/ios/get-started/installation/windows/connecting-to-mac/index.md) bulunmalıdır. Ayrıca Mac bilgisayardaki bir Windows VM üzerinde çalışan Visual Studio da kullanılabilir.

## <a name="macos-requirements"></a>macOS gereksinimleri

Xamarin geliştirmesine yönelik bir Mac bilgisayar kullanmak için aşağıdaki yazılım/SDK sürümleri gerekir. İşletim sistemi sürümünüzü denetleyin ve [Xamarin yükleyicisi](#installation-instructions) yönergelerini izleyin.

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode, [developer.apple.com](https://developer.apple.com/xcode/download/) adresinden veya Mac App Store aracılığıyla yüklenebilir (ve güncelleştirilebilir).

### <a name="testing--debugging-on-macos"></a>macOS’ta test etme ve hata ayıklama

- Xamarin mobil uygulamaları, test etme ve hata ayıklama için USB aracılığıyla fiziksel cihazlara dağıtılabilir (Apple Watch uygulamaları önce eşleştirilmiş iPhone’a dağıtılır).
- Xamarin.Mac uygulamaları doğrudan geliştirme bilgisayarında test edilebilir.

[!include[](~/cross-platform/includes/macos-testing.md)]

> [!WARNING]
> Yaklaşan Xamarin.Mac 4.8 sürümü yalnızca macOS 10.9 veya sonraki sürümleri destekleyecektir.
> Xamarin.Mac’in önceki sürümleri macOS 10.7 veya sonraki sürümleri destekler ancak bu eski macOS sürümleri TLS 1.2’yi destekleyecek yeterli TLS altyapısını barındırmaz. macOS 10.7 veya macOS 10.8’i hedeflemek için Xamarin.Mac 4.6 veya önceki sürümleri kullanın.

## <a name="windows-requirements"></a>Windows gereksinimleri

Xamarin geliştirmesine yönelik bir Windows bilgisayar kullanmak için aşağıdaki yazılım/SDK sürümleri gerekir.
İşletim sisteminizi denetleyin (ve Visual Studio’nun *Express* sürümünü kullanmadığınızdan emin olun. Kullanıyorsanız *Community* sürümüne yükseltmeyi düşünün).
Visual Studio 2017 yükleyicisi Xamarin’i otomatik olarak yükleme seçeneğini içerir (**.NET ile mobil geliştirme**).

[!include[](~/cross-platform/includes/windows-requirements.md)]

> [!NOTE]
> - Visual Studio için Xamarin tüm Visual Studio 2017 sürümlerini (Community, Professional ve Enterprise) destekler.
> - Evrensel Windows Platformu (UWP) için Xamarin.Forms uygulamaları geliştirmek amacıyla Windows 10 ile Visual Studio 2017 gerekir.

### <a name="testing--debugging-on-windows"></a>Windows’da test etme ve hata ayıklama

Xamarin mobil uygulamaları test etme ve hata ayıklama için USB aracılığıyla veya kablosuz olarak fiziksel aygıtlara dağıtılabilir (iOS cihazları Visual Studio’yu çalıştıran bilgisayara değil Mac bilgisayara bağlı olmalıdır).

[!include[](~/cross-platform/includes/windows-testing.md)]

## <a name="installation-instructions"></a>Yükleme yönergeleri

macOS için en son Xamarin sürümü [xamarin.com/download](http://xamarin.com/download) adresinden indirilebilir. Windows için, [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) yükleme yönergelerini izleyin.

Geçerli ürün sürümlerimizin tam listesi [geçerli sürümler sayfasında](http://developer.xamarin.com/releases/current/) bulunabilir. Bu sayfada ayrıca beta ve alfa kanallarımızdaki bireysel ürün sürümleri (ve sürüm notlarına bağlantılar) belirtilmiştir.

Her platform için [yükleme](~/cross-platform/get-started/installation/index.md) yönergeleri burada bulunabilir:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

Ayrıca [Xamarin.Forms gereksinimleri ve desteklenen platformlar](~/xamarin-forms/get-started/installation.md) hakkında ek bilgiler de bulunur.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin’i indirin](https://visualstudio.microsoft.com/xamarin/)
- [Geçerli Sürümler](https://developer.xamarin.com/releases/current/)
