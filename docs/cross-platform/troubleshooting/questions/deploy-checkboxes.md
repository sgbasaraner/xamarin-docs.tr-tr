---
title: Yapılandırma Yöneticisi'nde devre dışı onay kutularını dağıtma
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: ab825ba4d28ca8768e5c633fc3779828638a498d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Yapılandırma Yöneticisi'nde devre dışı onay kutularını dağıtma

Xamarin 3.5 itibaren Xamarin.iOS projeleri dağıtılan otomatik olarak tuşuna her **Başlat** araç çubuğu düğmesi veya çekme **hata ayıklama > hata ayıklamayı Başlat** menü öğesi. Yine istenen Xamarin.iOS uygulaması projesi olarak ayarla gerekir **başlangıç projesi** önce çalıştırmadan önce bu ya da komutları.

Bu nedenle, **dağıtma** onay kutularını kasıtlı olarak Visual Studio Yapılandırma Yöneticisi'nde Xamarin.iOS projeleri için devre dışı:

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager bir Xamarin.iOS projesi Xamarin 3.5 için devre dışı 'Dağıt' onay kutusunu gösteren")

Bu değişiklik Xamarin.iOS uygulaması projesi dağıtmak için ayarlanmadı Xamarin (3.3 ve önceki sürümleri) eski sürümleri görünebilir olan bir hata ortadan kaldırır:

![](deploy-checkboxes-images/error.png "Hata iletişim kutusu: Proje iPhoneApp1 başlatılabilir önce dağıtılmaları gerekir. Çözüm Yapılandırma Yöneticisi'nde dağıtılacak projesinin seçili olduğundan emin olun.")
