---
title: Xamarin.iOS Windows yükleme
description: Bu belge, bir Mac yapı konağı ve çifti Windows Xamarin.iOS geliştirme için Mac ayarlama bir Windows makinesine ayarlama açıklar.
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: 2bff37aba9b961b7308bf261377951dc96bd8e34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786070"
---
# <a name="installing-xamarinios-on-windows"></a>Xamarin.iOS Windows yükleme

_Bu makalede Windows makine kurun açıklar ve bir Mac yapı Xamarin.iOS geliştirme için ana bilgisayar._

## <a name="overview"></a>Genel Bakış

Visual Studio 2017 Windows ile Xamarin.iOS uygulamaları oluşturmak için ihtiyacınız:
 
-  Visual Studio yüklü 2017 Windows makineyle. Bu, bir fiziksel veya sanal makine olabilir.
    - [Windows sistem gereksinimleri](~/cross-platform/get-started/requirements.md#windows-requirements)
    
-  Bir ağ üzerinden erişilebilen Mac Apple'nın derleme araçları ve Xamarin.iOS ile ayarlayın. Visual Studio 2017 bu makinede yerel iOS uygulamaları derlemek için gerekli olan Apple'nın derleme araçları kullanmak için bir ağ bağlantısı üzerinden erişir. 
    - [Mac sistem gereksinimleri](~/cross-platform/get-started/requirements.md#macos-requirements)

## <a name="setup"></a>Kurulum

Visual Studio 2017 içinde Xamarin.iOS geliştirme için ayarlanmış almak için şu adımları izleyin:

1. Windows Kurulumu (Visual Studio 2017 yükleme)

    Bir tek başına veya sanal makinede Visual Studio 2017 Community, Professional ve Enterprise sürümleri ile Xamarin.iOS çalışır.
    
    - [Visual Studio 2017 yükleme](~/cross-platform/get-started/installation/windows.md).

2. Mac (Xcode yükleyin ve Mac için Visual Studio) ayarlama

    Yapı, hata ayıklama ve dağıtım için iOS uygulamaları imzalamak için Visual Studio 2017 hem Apple Geliştirici Araçları (Xcode) yapılandırılmış bir Mac yapı ana bilgisayar için ağ erişimi olması gerekir ve Xamarin.iOS.

    - [Xcode Mac uygulama Mağazası'ndan yükleyip](https://itunes.apple.com/us/app/xcode/id497799835?mt=12). 
    - [Mac için Visual Studio yükleme](https://docs.microsoft.com/visualstudio/mac/installation), hangi de yükler Xamarin.iOS.

    > [!NOTE] 
    > Mac için Visual Studio yüklememeyi tercih ediyorsanız, başlayarak [Visual Studio 2017 sürüm 15,6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Visual Studio 2017 yazılım Xamarin.iOS uygulamaları oluşturmak için gereken Mac yapı konak otomatik olarak yapılandırabilir . Daha fazla bilgi için bkz: [otomatik Mac sağlama](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning).

3. Mac çiftine (Visual Studio 2017 mac'e bağlanma)

    Visual Studio Mac iOS derleme araçları kullanmak 2017 için iki makine bir ağ üzerinden bağlanmanız gerekir.

    - [Mac Kılavuzu'na çift okuma](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="summary"></a>Özet

Bu makalede Windows makine ve onun ilişkili Mac yapı konağı Xamarin.iOS geliştirme için nasıl ayarlanacağı açıklanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

- [Visual Studio için Xamarin.iOS’a Giriş](introduction-to-xamarin-ios-for-visual-studio.md)
- [Visual Studio 2017 yapılandırma](config-options.md)
- [Cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md)
