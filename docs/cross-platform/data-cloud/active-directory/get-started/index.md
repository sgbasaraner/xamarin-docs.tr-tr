---
title: Azure Active Directory
description: "Bir uygulamayı Azure Active Directory kullanmak için kaydetme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: bf39b394903c3322ee617289ffe583a22a39fb20
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
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
