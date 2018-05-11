---
title: Neden Visual Studio my yapı içinde başvurulan kitaplığı proje içermeyen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 8a35c8e0cd03d5ff07405647b08275ef9ca3758d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Neden Visual Studio my yapı içinde başvurulan kitaplığı proje içermeyen

Visual Studio kullanan **Configuration Manager** projeleri bir çözümde belirli bir yapı veya dağıtım yapılandırmada otomatik olarak dahil edildiğini belirlemek için.

Başvurulan kitaplığı projesi ile oluşturulan bazı şablonlar zaten yapılandırmada dahil başvurulan kitaplık olur; ancak Aksi durumda elle ayarlanması gerekir.

## <a name="how-to-use-the-configuration-manager"></a>Yapılandırma Yöneticisi'ni kullanma

1. Açık **Yapı > Yapılandırma Yöneticisi**
2. Örneğin, özelleştirmek için yapılandırmayı seçin **hata ayıklama | iPhone**
3. Eklemek istediğiniz projeleri için onay kutularını seçin.

> [!NOTE]
> Gri kutuları otomatik olarak işlenir ve herhangi bir değişiklik gereksinim döndürmemelidir.

Bu adımları ekran kaydı: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
