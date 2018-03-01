---
title: "Bir NuGet paketi nasıl düşürmek?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 98d12bd93c50690909ac902a6f2498bcdb94960f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Bir NuGet paketi nasıl düşürmek?

Mac için Visual Studio & Visual Studio'nun eski sürümlerinin seçme ve bunları otomatik olarak yükleme özellikleri vardır; nasıl çalışır güncelleştirme paketleri benzer. Bu adımları aşağıda açıklanmıştır.

## <a name="visual-studio"></a>Visual Studio
1. Git **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**
2. Proje altındaki ayarlamak **varsayılan proje**
3. Şu sözdizimini kullanın:

    > Install-Package [PackageName]-[sekmesinde sürüm menüsü] sürümü

Ayrıca kopyalayıp paketin NuGet sayfasından tam komutu yapıştırın. Xamarin.Forms örneğin: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Mac için Visual Studio
1. Projenizdeki paketler klasörü sağ tıklatın ve seçin **paketleri Ekle**
2. Searchbar içinde gerekli paketlerinizin aramak için aşağıdaki söz dizimini kullanabilirsiniz:

    `[PackageName] version:*`

### <a name="examples"></a>Örnekler 
- Tüm Xamarin.Forms paketleri listeler: 

    `Xamarin.Forms version:`
- Tüm Xamarin.Forms 1.4.x paketleri listeler: 


    `Xamarin.Forms version:1.4`

*Not: arasında bir boşluk eklerseniz `version:` & sürüm numarası, arama davranır hiçbir sürüm belirtildi ancak gibi.*

