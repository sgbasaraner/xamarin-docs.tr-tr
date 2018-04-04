---
title: iOS RegisterServicePort hatasıyla Tasarımcısı
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 62d4a06c62bffb23566f4f59f42a0c980f417c45
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS RegisterServicePort hatasıyla Tasarımcısı

## <a name="sample-error"></a>Örnek hata
> System.AggregateException: Bir veya daha fazla hata oluştu---> System.SystemException: RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): çekirdek döndürdü:-308 (-308): eniden (IPC/MIG) sunucusu

## <a name="explanation"></a>Açıklama
Hatalarla `RegisterServicePort` ve yukarıdaki benzer hata iletileri gibi yaygın olarak casus yazılım/kötü amaçlı yazılım bilgisayardaki ile ilgili bir sorun. Lütfen düşünün [bu hata raporu yorum](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) bağlantısını yanı sıra daha fazla bilgi için [Apple forum tartışmasına](https://discussions.apple.com/thread/5596008) olası bulaşmasını konusunda. 

Sorunu tanılamalarına yardımcı olması için macOS uygulamayı açmak **konsol** ve her dosya içine silme **kullanıcı tanılama raporları** bölüm [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy). Daha sonra Mac için Visual Studio'yu başlatın ve Tasarımcısı'nı kullanmayı deneyin. Lütfen yeni günlük dosyalarını Tasarımcısı'nı başlatmak başarısız oldu sonra bu bölümde görünmüyorsa, bize çözümlemek bunları kaydedin.  

Lütfen bu dosyayı denetlemek için gereken en önemli şey olduğuna dikkat edin: 
> /usr/lib/libimckit.dylib

Yukarıdaki sonuçları olsun, bu dosya varsa, daha önce bahsedilen casus yazılım/kötü amaçlı yazılım sorunu bilgisayarınızda mevcuttur.  

Aşağıdaki bağlantıda Bu casus yazılım/kötü amaçlı yazılımı kaldırmak için adımı vardır: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

