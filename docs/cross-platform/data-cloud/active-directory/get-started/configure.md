---
title: Adım 2. Mobil uygulama için hizmet erişimi yapılandırma
description: Bu belge, Azure Active Directory tarafından güvenli hale getirilmiş bir Azure uygulamaya erişimi olan bir Xamarin uygulaması sağlamak açıklar.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 2a9baab9215ae2d30e4daf6800a116c95165da42
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780106"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Adım 2. Mobil uygulama için hizmet erişimi yapılandırma

Tüm kaynak örn web uygulaması, web hizmeti, vb. her gerektiğinde Azure Active Directory tarafından korunması, kayıtlı olması gerekir. Tüm güvenli uygulamalar veya hizmetler altında görülebilir **uygulamaları** sekmesi. Burada, mobil uygulamadan erişilebilir ve ona erişmesini sağlamak için gereken uygulama seçebilirsiniz.

1. Üzerinde **yapılandırma** sekmesinde, bulmak **diğer uygulamalara izinler** bölümü:

  ![](configure-images/2.1-configure.png "Diğer uygulamaları bölümüne izinleri yapılandırma sekmesinde bulun")

2.  Tıklayın **uygulama ekleme** düğmesi. Sonraki ekranda açılan Azure Active Directory tarafından güvenli hale getirilen tüm uygulamaların listesi görmeniz gerekir. Mobil uygulamayı erişilmesi gereken uygulamaları seçin.

  ![](configure-images/2.2-add-application.png "Mobil uygulamayı erişilmesi gereken uygulamaları seçin")

3. Uygulamayı seçtikten sonra bir kez daha yeni eklenen uygulamada seçin **diğer uygulamalara izinler** bölümünde ve uygun hakları verin.

  ![](configure-images/2.3-permissions.png "Uygulamayı seçtikten sonra bir kez daha diğer uygulamaları bölüm için izinleri yeni eklenen uygulamayı seçin ve uygun hakları verin")

4. Son olarak, **kaydetmek** yapılandırma. Bu hizmetleri artık mobil uygulamalarda kullanılabilir olması gerekir!



## <a name="related-links"></a>İlgili bağlantılar

- [Microsoft NativeClient örnek](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
