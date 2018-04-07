---
redirect_url: /xamarin/cross-platform/troubleshooting/questions/
title: Xamarin ile Visual Studio 2017 Sürüm Adayı kullanabilir miyim?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8E752F36-F73A-4EFC-9F82-4E18FDE1C9E2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: cfad562bfbfbc3985efa6252aa8eb9b6559fcc41
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Xamarin ile Visual Studio 2017 Sürüm Adayı kullanabilir miyim?

## <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Xamarin ile Visual Studio 2017 Sürüm Adayı kullanabilir miyim?

Evet. Xamarin ile Visual Studio 2017 Sürüm Adayı yararlanamaz. Bununla birlikte, lütfen şu anda unutmayın, Visual Studio 2017 RC Xamarin yükleme halinde Visual Studio 2015/2013 yüklü Xamarin önceki tüm sürümlerini kaldırır. Visual Studio 2017 için Xamarin Xamarin ile aynı zamanda için Visual Studio 2015/2013 Visual Studio Visual Studio yükleyicisi sistem kullanımı doğru MSI paketlemesi çıktığınızda taşıma 2017 sonucu olarak yüklenebilir.

Takım şu anda bu beklenen bir davranış atlama yolları aranırken biz geliştirme ortamlarını gereksinimlerine göre seçmelerini öneriyoruz. 

> [!IMPORTANT]
> Xamarin.iOS projeleri oluşturuluyorsa, eşleştirilmiş Mac sisteminizdeki Mac için Visual Studio Xamarin.iOS kanal aynı sürümde olduğundan emin olun.

## <a name="how-do-i-install-xamarin-to-visual-studio-2017-release-candidate"></a>Visual Studio 2017 Sürüm Adayı'na nasıl Xamarin yüklensin mi?

### <a name="installing-during-the-visual-studio-2017-rc-installer"></a>Visual Studio 2017 RC yükleyicisi sırasında yükleme

* Seçin **Xamarin** bir parçası olarak yeni bileşen **Visual Studio yükleyicisi**

  [![](visualstudio-2017-rc-images/install1-sml.png "Visual Studio 2017 RC yükleyici ekranı")](visualstudio-2017-rc-images/install1-orig.png#lightbox)

Bu Xamarin.iOS ve xamarin Android geliştirme için Visual Studio uzantısı yükler.

### <a name="installing-or-reinstalling-xamarin-in-an-existing-installation-of-visual-studio-2017-rc"></a>Visual Studio 2017 RC olan bir yüklemesini yükleme veya yeniden yüklemeyi Xamarin

#### <a name="using-the-visual-studio-installer"></a>Visual Studio Yükleyicisi'ni kullanarak:

1. Visual Studio yükleyicisi uygulama arayın

  [![](visualstudio-2017-rc-images/reinstall1-sml.png "Visual Studio yükleyicisi uygulama için arama sonuçları")](visualstudio-2017-rc-images/reinstall1-orig.png#lightbox)

2. Seçin: bir. **.NET (Önizleme) ile Mobil Geliştirme** iş yüklerini sekmesinde veya

  [![](visualstudio-2017-rc-images/reinstall2-sml.png "VS yükleyici iş yükleri sekmesini") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** içinde **bileşenleri tek tek** sekmesi

  [![](visualstudio-2017-rc-images/reinstall3-sml.png "VS yükleyici bileşenleri sekmesi")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)

#### <a name="using-the-visual-studio-installer-within-visual-studio"></a>Visual Studio içinde Visual Studio Yükleyicisi'ni kullanarak:
1. Visual Studio 2017 başlangıç sayfasına gidin
2. Tıklayın **fazla proje şablonlarını** altında **yeni proje** bölümü

    [![](visualstudio-2017-rc-images/reinstall4-sml.png "Visual Studio Başlangıç sayfası")](visualstudio-2017-rc-images/reinstall4-orig.png#lightbox)
3. Tıklayın `Open Visual Studio Installer` sol bölmede

    [![](visualstudio-2017-rc-images/reinstall5-sml.png "Yeni Proje ekranı")](visualstudio-2017-rc-images/reinstall5-orig.png#lightbox)
4. Seçin:
    
    a. **.NET (Önizleme) ile Mobil Geliştirme** iş yüklerini sekmesinde veya

    [![](visualstudio-2017-rc-images/reinstall2-sml.png "VS yükleyici iş yükleri sekmesini") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** içinde **bileşenleri tek tek** sekmesi

    [![](visualstudio-2017-rc-images/reinstall3-sml.png "VS yükleyici bileşenleri sekmesi")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)
