---
title: Neden Visual Studio my yapı içinde başvurulan kitaplığı proje içermeyen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: cb9b3689ab6a12d99f9694583cd0fd50a6f5c72c
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
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
