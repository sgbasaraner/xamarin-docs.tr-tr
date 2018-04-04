---
title: Azure Active Directory
description: Bir uygulamayı Azure Active Directory kullanmak için kaydetme
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ff076aa894f7f406c79d2a19b6055d2feeb99bd0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Bir uygulamayı Azure Active Directory kullanmak için kaydetme_

Azure Active Directory dosyaları, bağlantılar ve çalışanların sistemlerine oturum açın veya kendi e-postaları denetlemek için kullandığı aynı kuruluş hesabı kullanarak Web API'leri gibi güvenli kaynaklara geliştiricilerin sağlar.

Azure Active Directory ile doğrulanabilir mobil uygulamaları geliştirme üç adımdan oluşur.
İlk iki adım genellikle, kullanmayı planladığınız hangi hizmetlerin bağımsız olarak aynıdır. Üçüncü adım her hizmet türü için farklı olur:

  1. [Azure Active Directory ile kaydetme](~/cross-platform/data-cloud/active-directory/get-started/register.md) üzerinde *windowsazure.com* portal, ardından
  2. [Hizmetleri Yapılandırma](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Hizmetleri kullanarak mobil uygulamaları geliştirin.

Erişmek için farklı Hizmetleri örnekleri şunlardır:

- [Grafik API'si](~/cross-platform/data-cloud/active-directory/graph.md)
- Web API
- Office365


## <a name="conclusion"></a>Sonuç

Yukarıdaki adımları kullanarak mobil uygulamalarınızı Azure Active Directory karşı kimlik doğrulaması yapabilir. Active Directory Authentication Library (ADAL) kod, daha az satırlara sahip kodu çoğunu tutarken kolaylaşır aynı ve böylece paylaşılabilir platformlarında kolaylaştırarak.



## <a name="related-links"></a>İlgili bağlantılar

- [Microsoft NativeClient örnek](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
