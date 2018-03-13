---
title: "NUnit 2.6.4 yükleme NuGet kullanma"
description: "Bu kılavuz NUnit 2.6.4 NUnit 3.0 düşürmek alınmaktadır NuGet kullanma."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7683F2B8-7FDF-48C4-8E7D-649D4D4E79F0
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: b7f42d6e36638bf5c7e98b9363295e37997ee067
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="installing-nunit-264-using-nuget"></a>NUnit 2.6.4 yükleme NuGet kullanma

_Bu kılavuz NUnit 2.6.4 NUnit 3.0 düşürmek alınmaktadır NuGet kullanma._

Mac veya Xamarin.UITest kullanarak kullanılmalı için testleri Visual Studio'da yazma geliştiricilerinin [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4) NUnit 3.0 veya daha yüksek Mac veya Xamarin.UITest için Visual Studio ile uyumlu değil. Mac veya Xamarin.UITests NUnit 3.0 için Visual Studio ile birim testleri çalıştırma denemesi başarısız olur.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu kılavuz NUnit 2.6.4 yüklemek nasıl ele alınacaktır Mac için Visual Studio için NuGet kullanma Gerekirse, aşağıdaki adımları NUnit 3.0 da kaldırılır.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu kılavuz NUnit 2.6.4 NUnit 3.0 düşürmek nasıl ele alınacaktır NuGet Visual Studio 2015'te kullanma.

-----

## <a name="requirements"></a>Gereksinimler

Bu kılavuz, bir mobil uygulama projesi ve bir test projesi ile varolan bir çözümü olduğunu varsayar.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="installing-nunit-264-in-visual-studio-for-mac"></a>Mac için Visual Studio yükleme NUnit 2.6.4

Aşağıdaki adımlar nasıl NUnit 2.6.4 yükleneceğini açıklar.


1. **Paket Yöneticisi'ni açmak** -sağ tıklayın **paketleri** seçip **paketleri Ekle** açılan menüsünden:

    [![](installing-nunit-using-nuget-images/add-packages-xs.png "Paketleri sağ tıklayın ve açılan menüden paketleri Ekle seçin")](installing-nunit-using-nuget-images/add-packages-xs.png#lightbox)
    
1. **Arama `NUnit version:2.6.4`**  -Mac için Visual Studio NUnit 3.0 (gerekirse) kaldırın ve ardından indirir ve NUnit 2.6.4 yükleyin. İçinde **paketleri Ekle** iletişim kutusunda, metin girin `nunit version:2.6.4` içinde **arama** üst sağ köşede yer alan. Seçin **NUnit** tıklayın ve arama sonuçlarını **Paketi Ekle** düğmesi:

    [![](installing-nunit-using-nuget-images/nunit-search-xs.png "Arama sonuçlarından NUnit seçin ve paketi Ekle düğmesini tıklatın")](installing-nunit-using-nuget-images/nunit-search-xs.png#lightbox)


Çözüm panelinde NUnit paketin sürüm numarası incelenerek NUnit 2.6.4 yüklü olduğunu doğrulamak mümkündür:

[![](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png "Çözüm panelinde NUnit paketin sürüm numarası inceleyin.")](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png#lightbox)

## <a name="summary"></a>Özet

Bu kılavuz Paket Yöneticisi konsolu kullanılarak Mac için Visual Studio NUnit 2.6.4 NUnit 3.0 düşürmek açıklanır.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installing-nunit-264-in-visual-studio"></a>Visual Studio'da NUnit 2.6.4 yükleme

Bu bölümde kullanarak odaklanacaktır _NuGet Paket Yöneticisi Konsolu_ NUnit 3.0 kaldırın ve NUnit 2.6.4 yüklemek için Visual Studio 2015'te.


1. **NuGet Paket Yöneticisi Konsolu'ndan** - seçin **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**:

    [![](installing-nunit-using-nuget-images/package-manager-console.png "NuGet Paket Yöneticisi konsolu - seçim araçları NuGet Paket Yöneticisi Paket Yöneticisi konsolunu Başlat")](installing-nunit-using-nuget-images/package-manager-console.png#lightbox)
    
1. **Sürüm NUnit doğrulayın** -isteğe bağlı olarak komutunu çalıştırarak yüklü NUnit sürümünü doğrulayabilirsiniz `Get-Package -Project <UITEST PROJECT>`:

    ```bash
    [PM] Get-Package -Project [TEST PROJECT NAME]
    
        Id                                  Versions                                 ProjectName
        --                                  --------                                 -----------
    NUnit                               {3.0.1}                                  [TEST PROJECT NAME]
    Xamarin.UITest                      {1.2.0}                                  [TEST PROJECT NAME]
    ```

NUnit 3.0 veya üzerini görürseniz, NUnit 2.6.4 düşürmek gerekir.

1. **NUnit 3.0 kaldırma** -kullanım `Uninstall-Package` komutunu NUnit 3.0 kaldırmak için:

        <PM> Uninstall-Package NUnit -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.3.0.1' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Resolving actions to uninstall package 'NUnit.3.0.1'
        Resolved actions to uninstall package 'NUnit.3.0.1'
        Removed package 'NUnit.3.0.1' from 'packages.config'
        Successfully uninstalled 'NUnit.3.0.1' from <TEST PROJECT NAME>

1. **NUnit 2.6.4 yükleme** -yükleme Nunit 2.6.4 ile `Install-Package` aşağıdaki kod parçacığında gösterildiği gibi komutunu:

        <PM> Install-Package NUnit -Version 2.6.4 -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.2.6.4' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Attempting to resolve dependencies for package 'NUnit.2.6.4' with DependencyBehavior 'Lowest'
        Resolving actions to install package 'NUnit.2.6.4'
        Resolved actions to install package 'NUnit.2.6.4'
        Adding package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to 'packages.config'
        Successfully installed 'NUnit 2.6.4' to <TEST PROJECT NAME>
    
## <a name="summary"></a>Özet

Bu kılavuz Paket Yöneticisi konsolu kullanılarak Visual Studio 2015'te NUnit 2.6.4 NUnit 3.0 düşürmek açıklanır.

-----

## <a name="related-links"></a>İlgili bağlantılar

- [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4)
- [NUnit 2.6.4 NuGet paketi](https://www.nuget.org/packages/NUnit/2.6.4)
