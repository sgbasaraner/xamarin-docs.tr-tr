---
title: "Nasıl hangi kitaplıkları bir PCL desteklenir görüntüleyebilirsiniz?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: e05b15f0a9af7666d635ad375b6c401e95a44525
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Nasıl hangi kitaplıkları bir PCL desteklenir görüntüleyebilirsiniz?

- Altında çeşitli PCL Hedef platformlar tarafından desteklenen çeşitli özelliklere genel bakış bulabilirsiniz *desteklenen özellikler* bu sayfayı kısmı: [http://msdn.microsoft.com/en-us/library/gg597391.aspx](https://msdn.microsoft.com/en-us/library/gg597391.aspx)

- Başka bir seçenek kullanmaktır [.NET taşınabilirlik Çözümleyicisi](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) varolan kitaplığınızın PCL profiline dönüştürülebilir olup olmadığını değerlendirmek için.

- Kullanabileceğinize gerçek profili içeriğine gözatmak için üçüncü bir olasılık olabilir. Profil 78 örneğin kullanarak, burada geçebilir: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` ve içindeki tüm derlemelerde görüntüleyin.

Hangi yöntemi seçtiğiniz, lütfen bazı işlevler NuGet ve Microsoft BCL kitaplığı aracılığıyla indirilmesi gerektiğini unutmayın.