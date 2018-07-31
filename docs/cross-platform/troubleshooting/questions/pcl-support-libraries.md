---
title: Nasıl bir PCL'de hangi kitaplıkların desteklenen görüntüleyebilir miyim?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.date: 07/27/2018
ms.openlocfilehash: 7e1017baf7daed68b5e55319a9ce13a4a2df5f2e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351476"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Nasıl bir PCL'de hangi kitaplıkların desteklenen görüntüleyebilir miyim?

- Altında çeşitli PCL Hedef platformlar tarafından desteklenen çeşitli özelliklere genel bakış bulabilirsiniz *desteklenen özellikler* bu sayfayı bir kısmı: [https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- Başka bir seçenek kullanmaktır [.NET Portability Analyzer](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) mevcut kitaplığınızı PCL profil dönüştürülüp dönüştürülemeyeceğini değerlendirmek için.

- Kullanabileceğiniz gerçek profili içeriğini göz atmak için üçüncü bir olasılıktır. Buraya gidin profili 78 örneğin kullanarak: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` ve içindeki tüm derlemeleri görüntüleyin.

Hangi yöntemi seçtiğiniz, lütfen bazı işlevler NuGet ve Microsoft BCL kitaplığı indirilmesi gerektiğini unutmayın.
