---
title: Bir NuGet paketi eski sürümünü nasıl yükleyebilirim?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 72fdf7246b148fa95ea312284957072ecda47121
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351222"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Bir NuGet paketi eski sürümünü nasıl yükleyebilirim?

Mac için Visual Studio ve Visual Studio, paketlerin daha eski sürümlerini seçme ve bunları otomatik olarak yükleme özellikleri sahiptir; nasıl çalıştığını güncelleştirme paketleri benzer. Bu adımlar aşağıda açıklanmıştır.

## <a name="visual-studio"></a>Visual Studio
1. Git **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**
2. Altında projesini **varsayılan proje**
3. Bu söz dizimini kullanın:

    > Install-Package [PackageName]-[sürüm menüsünden için sekmesinde] sürümü

Ayrıca kopyalayıp paketin NuGet sayfasından tam komutu yapıştırın. Xamarin.Forms için örnek: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Mac için Visual Studio
1. Projenizde, paketleri klasörü sağ tıklatın ve seçin **paketleri Ekle**
2. Searchbar içinde gerekli paketleri aramak için aşağıdaki söz dizimini kullanabilirsiniz:

    `[PackageName] version:*`

### <a name="examples"></a>Örnekler 
- Tüm Xamarin.Forms paketlerini listeler: 

    `Xamarin.Forms version:`
- Tüm Xamarin.Forms 1.4.x paketlerini listeler: 

    `Xamarin.Forms version:1.4`

*Not: arasına boşluk eklerseniz `version:` & sürüm numarası, arama davranır sürüm belirtildi ancak.*

