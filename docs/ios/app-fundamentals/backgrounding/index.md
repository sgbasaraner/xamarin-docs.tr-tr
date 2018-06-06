---
title: Xamarin.iOS içinde backgrounding
description: Arka plan işleme veya backgrounding başka bir uygulama ön planda çalıştığı sırada görevleri arka planda uygulamaları izin vererek işlemidir. Bu kılavuz, arka plan iOS işlemleri giriş olarak görev yapar.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b22f3ef3276129f7f46c23cc1d06666f151f5ac4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783548"
---
# <a name="backgrounding-in-xamarinios"></a>Xamarin.iOS içinde backgrounding

_Arka plan işleme veya backgrounding başka bir uygulama ön planda çalıştığı sırada görevleri arka planda uygulamaları izin vererek işlemidir. Bu kılavuz, arka plan iOS işlemleri giriş olarak görev yapar._

Mobil uygulamalarda backgrounding temelde masaüstünde görevli geleneksel kavramı farklıdır. Masaüstü makineler ekran Gayrimenkul, güç ve bellek gibi bir uygulama için kullanılabilir kaynakları çeşitli sahiptir. Yan yana çalıştırma ve kullanıcı kalır ve kullanılabilir uygulamaları geçerlidir. Bir mobil cihazda kaynakları çok daha sınırlıdır. Birden fazla uygulama küçük bir ekranda göster zordur ve tam hızda birkaç uygulama çalıştıran pil boşaltmak. Backgrounding uygulamalar da gerçekleştirmek için ihtiyaç arka plan görevleri çalıştırmak için gereken kaynakları vermiş ve foregrounded uygulama ve cihaz esnek bulundurmama arasında sabit bir güvenlik açığı olur. İOS ve Android backgrounding için hazırlar sahip, ancak bunlar çok farklı şekilde işler.

İOS, backgrounding bir uygulama durumu tanınır ve uygulamaları arka plan durumunu uygulama ve kullanıcıya ve bu moddan davranışına göre taşınır. iOS de bilinen arka plan gerekli uygulama, bir tür olarak işletim önemli bir görevi tamamlamak için işletim sistemi kez isteyen dahil olmak üzere arka planda çalıştırmak için bir uygulama bağlantı kabloları çeşitli seçenekler sunar ve bir uygulamanın içerik yenileniyor aralıkları.

Bu kılavuz ve izlenecek yollar eşlik, biz arka planda uygulama görevleri öğrenin olacak. Biz temel kavramları ve en iyi yöntemleri kapsar ve arka planda konumu güncelleştirmelerini alan bir gerçek dünya uygulaması oluşturmada size adım.

## <a name="contents"></a>İçindekiler

1.  [iOS’ta Arka Planda İşlemeye Giriş](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Uygulama Yaşam Döngüsü Tanıtımı](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [iOS Arka Planda İşleme Teknikleri](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [İzlenecek Yollar: iOS’ta Arka Planda İşleme](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [iOS Arka Planda İşlemeRehberi](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Özet

Bu kılavuzda, iOS arka plan işleme yapılması farklı yollarını sunar. Biz iOS uygulama durumları ele ve uygulama yaşam döngüsü iOS oynadığı backgrounding rol inceledi. Ayrıca, nasıl biz tek tek görevler veya iOS arka planda çalışması için tüm uygulamaları kaydolabilir öğrendiniz. Son olarak, güncelleştirmeleri arka planda gerçekleştirmek uygulamalar oluşturarak İos'ta backgrounding bizim anlamak güçlendirilmiş.



## <a name="related-links"></a>İlgili bağlantılar

- [Android backgrounding](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (örnek)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Konum (örnek)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Basit arka plan Aktarım (örnek)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS arka plan yürütme](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
