---
title: Xamarin.iOS arka planda işleme
description: Arka planda işleme veya işlerken arka plan uygulamaları başka bir uygulama ön planda çalışırken görevleri arka planda izin vererek işlemidir. Bu kılavuz, iOS arka plan giriş olarak görev yapar.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2018
ms.openlocfilehash: 73344b790bf6d4719d9a92cfa9146578dffe04e9
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350774"
---
# <a name="backgrounding-in-xamarinios"></a>Xamarin.iOS arka planda işleme

_Arka planda işleme veya işlerken arka plan uygulamaları başka bir uygulama ön planda çalışırken görevleri arka planda izin vererek işlemidir. Bu kılavuz, iOS arka plan giriş olarak görev yapar._

Mobil uygulamalarda arka planda işleme temelde geleneksel masaüstü çok görevli kavramını farklıdır. Masaüstü makinelerinden çeşitli ekran gerçek boyutunuzu, güç ve bellek gibi bir uygulamada kullanılabilir kaynaklara sahip. Yan yana çalıştırmak ve yüksek performanslı kalır ve kullanılabilir uygulamalardır. Mobil cihazda, kaynakları çok daha sınırlıdır. Birden fazla uygulama küçük bir ekranda göstermek zordur ve tam hızda birkaç uygulama çalıştıran pil boşaltma. Arka planda işleme uygulamaları da gerçekleştirmek için gereksinim duydukları arka plan görevleri çalıştırma kaynakları vererek ve foregrounded uygulama ve cihaz duyarlı tutma arasında sabit güvenliğinin aşılmasına neden olur. Hem iOS hem de Android için arka planda işleme hükümlerine sahip, ancak bunlar çok farklı yollarla işlemek.

İOS, arka planda işleme bir uygulama durumu kabul edilir ve uygulamaları arka plan durumunu uygulama ve kullanıcı içine ve dışına davranışına göre taşınır. iOS Ayrıca, bilinen arka planda gerektiğinde uygulama türü çalışan önemli bir görevi tamamlamak için işletim sistemi kez isteyen dahil olmak üzere arka planda çalıştırılacak bir uygulama bağlama çeşitli seçenekler sunar ve bir uygulamanın içerik yenileniyor aralıkları.

Bu kılavuzu ve izlenecek yollar eşlik eden, uygulama görevleri arka planda gerçekleştirmek öğrenmek için kullanacağız. Biz temel kavramları ve en iyi uygulamaları kapsar ve ardından arka planda konum güncelleştirmeleri alan gerçek dünya uygulaması oluşturma işlemi adım adım.

## <a name="contents"></a>İçindekiler

1.  [iOS’ta Arka Planda İşlemeye Giriş](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Uygulama Yaşam Döngüsü Tanıtımı](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [iOS Arka Planda İşleme Teknikleri](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [İzlenecek Yollar: iOS’ta Arka Planda İşleme](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [iOS Arka Planda İşlemeRehberi](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Özet

Bu kılavuzda, ios'ta arka plan işlemi yapmanın farklı yöntemleri kullanıma sunduk. Biz iOS uygulama durumlarını kapsamında ve uygulama yaşam döngüsü iOS oynadığı arka planda işleme rol incelenir. Ayrıca, nasıl biz tek tek görevler veya iOS arka planda çalışması için tüm uygulamaları kaydettirilemedi öğrendiniz. Son olarak, arka planda güncelleştirmeleri gerçekleştiren uygulamalar oluşturarak İos'ta arka planda işleme bizim anlamak güçlendirilmiş.



## <a name="related-links"></a>İlgili bağlantılar

- [Android'de arka planda işleme](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (örnek)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Konum (örnek)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Basit arka plan Aktarım (örnek)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS arka planda yürütme](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
