---
title: Google Licensing Services
ms.topic: article
ms.prod: xamarin
ms.assetid: E96BDCC3-454A-A797-5819-905E2BB1AC41
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/20/2017
ms.openlocfilehash: d4ed2df994ace7f6de5ade78577e759bb811565c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="google-licensing-services"></a>Google Licensing Services

Google Play önce Android uygulamaları kopya Google emin olmak için yalnızca yetkili kullanıcıların Pazar tarafından sağlanan koruma uygulamaları kendi cihazlarındaki çalıştırabilir eski üzerinde dayanıyordu. Kopya koruma mekanizması sınırlamaları uygulama koruması için ideal daha az çözümü yapılan.

Google lisans bu eski kopya koruma mekanizması bir yerini alır.
Google lisans Android uygulamaları, uygulamanın belirli bir aygıtta çalıştırmak için lisanslı değilse belirlemek için sorgulayabilir esnek, güvenli, ağ tabanlı bir hizmet var.

Android uygulamaları Lisans denetlemek için lisans ne sıklıkta denetlemek ne zaman ve lisans sunucusundan yanıt nasıl ele alınacağını üzerinde tam denetime sahip Google lisans esnektir.

Her yanıtı özel olarak Google Play sunucu ve uygulama arasında paylaşılan bir RSA anahtar çifti ile imzalanmış Google lisans güvenli olmamasıdır. Google Play'de Android uygulama içinde katıştırılır ve yanıtları kimliğini doğrulamak için kullanılan geliştiricileri için bir ortak anahtar sağlar. Google Play sunucunun özel anahtarı dahili olarak tutar.

Google lisans uyguladı bir uygulama bir cihazda Google Play uygulaması tarafından barındırılan bir hizmete istekte bulunur. Google Play sonra lisans durumuna sahip yanıt Google lisans sunucusu bu isteği gönderir: 

[![Lisans sunucusu iş akışı diyagramı](google-licensing-services-images/gp-licensing-service-overview.png)](google-licensing-services-images/gp-licensing-service-overview.png#lightbox)

Yukarıdaki diyagramda bu iş akışı gösterilmektedir: 

-   Paket adı uygulamanın sağladığı bir *nonce* (şifreleme doğrulayıcısı) sunucu yanıtı ve yanıtı zaman uyumsuz olarak işlemek bir geri çağırma doğrulamak için kullanılır. 

-   Google Play Google hesabı ve IMSI numarası gibi cihaz kendisini gibi bilgiler sağlar. 

Google lisans hizmeti de bir anahtar (Bu belgenin sonraki bölümlerinde ele alınmıştır) APK genişletme dosyaları bileşenidir. APK genişletme dosyaları indirilir genişletme dosyaları URL'lerini almak için Google lisans hizmetleri kullanan.


## <a name="requirements"></a>Gereksinimler

Google Play satın alınmamış uygulamaları fayda Google lisans Hizmetleri'nden alır. Google Play bir aygıtta yüklü değilse, ardından Hizmetleri Lisansı'nı kullanan uygulamalar yine normal olarak bu cihaz üzerinde çalışır.

Google Play işlevselliği için Internet erişimi gerektirir. Bir uygulama burada cihaz Google Play lisans sunucularına erişimi yok senaryolara yer vermek için lisans önbelleğe alabilir.

Uygulama APK genişletme dosyaları kullandığında boş uygulamalar yalnızca Google lisansı gerektirir.
