---
title: Nasıl hangi kitaplıkları bir PCL desteklenir görüntüleyebilirsiniz?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.openlocfilehash: 87f65ba2cff2d5990c32aa142f97766a76d6ba05
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919546"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Nasıl hangi kitaplıkları bir PCL desteklenir görüntüleyebilirsiniz?

- Altında çeşitli PCL Hedef platformlar tarafından desteklenen çeşitli özelliklere genel bakış bulabilirsiniz *desteklenen özellikler* bu sayfayı kısmı: [http://msdn.microsoft.com/library/gg597391.aspx](https://msdn.microsoft.com/library/gg597391.aspx)

- Başka bir seçenek kullanmaktır [.NET taşınabilirlik Çözümleyicisi](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) varolan kitaplığınızın PCL profiline dönüştürülebilir olup olmadığını değerlendirmek için.

- Kullanabileceğinize gerçek profili içeriğine gözatmak için üçüncü bir olasılık olabilir. Profil 78 örneğin kullanarak, burada geçebilir: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` ve içindeki tüm derlemelerde görüntüleyin.

Hangi yöntemi seçtiğiniz, lütfen bazı işlevler NuGet ve Microsoft BCL kitaplığı aracılığıyla indirilmesi gerektiğini unutmayın.
