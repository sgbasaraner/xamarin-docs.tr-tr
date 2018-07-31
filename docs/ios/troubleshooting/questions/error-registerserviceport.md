---
title: RegisterServicePort ile iOS Designer hatası
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 9edcc822b170c3463908b9f5fb1db8b798346e3e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350712"
---
# <a name="ios-designer-error-with-registerserviceport"></a>RegisterServicePort ile iOS Designer hatası

## <a name="sample-error"></a>Örnek hata
> System.AggregateException: Bir veya daha fazla hata oluştu.---> System.SystemException: RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): döndürülen Çekirdek:-308 (-308): sonlanmış (IPC/MIG) sunucusu

## <a name="explanation"></a>Açıklama
Hatalarla `RegisterServicePort` ve yukarıda benzer hata iletileri gibi yaygın bir casus yazılım/kötü amaçlı yazılım bilgisayarda sorun. Lütfen göz önünde bulundurun [açıklaması bu hatayı rapor](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) bağlantısını yanı sıra daha fazla bilgi için [Apple forum tartışmasına](https://discussions.apple.com/thread/5596008) olası bulaşma kaldırma. 

Sorunu tanılamaya yardımcı olmak üzere, macOS uygulamayı açın **konsol** ve içindeki her dosya silme **kullanıcı tanılama raporları** bölümü [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy). Mac için Visual Studio'yu başlattıktan sonra Tasarımcı kullanmayı deneyin. Lütfen tasarımcıyı başlatmak başarısız olursa yeni günlük dosyalarını bu bölümünde görünüyorsa, bunlar bizim analiz etmek için kaydedin.  

Lütfen bu dosyayı denetlemek için en önemli şey olduğunu unutmayın: 
> /usr/lib/libimckit.dylib

Yukarıdaki sonuçları ne olursa olsun, bu dosya varsa, yukarıda sözü edilen casus yazılım/kötü amaçlı yazılım sorunu bilgisayarınızda mevcuttur.  

Aşağıdaki bağlantıda Bu casus yazılım/kötü amaçlı yazılım kaldırmak için adımı vardır: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

