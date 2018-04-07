---
title: Yapılandırma Yöneticisi'nde devre dışı onay kutularını dağıtma
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: c0f116565a2741c62a00ed2a255cfde8c57b8569
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Yapılandırma Yöneticisi'nde devre dışı onay kutularını dağıtma

Xamarin 3.5 itibaren Xamarin.iOS projeleri dağıtılan otomatik olarak tuşuna her **Başlat** araç çubuğu düğmesi veya çekme **hata ayıklama > hata ayıklamayı Başlat** menü öğesi. Yine istenen Xamarin.iOS uygulaması projesi olarak ayarla gerekir **başlangıç projesi** önce çalıştırmadan önce bu ya da komutları.

Bu nedenle, **dağıtma** onay kutularını kasıtlı olarak Visual Studio Yapılandırma Yöneticisi'nde Xamarin.iOS projeleri için devre dışı:

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager bir Xamarin.iOS projesi Xamarin 3.5 için devre dışı 'Dağıt' onay kutusunu gösteren")

Bu değişiklik Xamarin.iOS uygulaması projesi dağıtmak için ayarlanmadı Xamarin (3.3 ve önceki sürümleri) eski sürümleri görünebilir olan bir hata ortadan kaldırır:

![](deploy-checkboxes-images/error.png "Hata iletişim kutusu: Proje iPhoneApp1 başlatılabilir önce dağıtılmaları gerekir. Çözüm Yapılandırma Yöneticisi'nde dağıtılacak projesinin seçili olduğundan emin olun.")
