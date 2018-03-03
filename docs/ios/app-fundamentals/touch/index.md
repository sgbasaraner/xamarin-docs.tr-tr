---
title: Dokunma
description: "Dokunmatik ekranlar bugünün aygıtların çoğu üzerinde kullanıcıların hızlı ve verimli şekilde doğal ve sezgisel şekilde cihazlarla etkileşime girmesine izin. Bu etkileşimi yalnızca basit dokunma algılama sınırlı değildir – hareketleri de kullanmak da mümkündür. Örneğin, tutarak yakınlaştırma hareketi kullanıcı yakınlaştırmak veya uzaklaştırmak iki parmakları ekran parçası çimdik tarafından bu – çok yaygın bir örneği bulunmaktadır. Bu kılavuz, dokunma ve iOS hareketleri inceler."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A17FD28-313F-4AAC-B82B-3847B4D64A88
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: 8f6c26048bc0ece0d64acf069151ff1d67403ccc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="touch"></a>Dokunma

_Dokunmatik ekranlar bugünün aygıtların çoğu üzerinde kullanıcıların hızlı ve verimli şekilde doğal ve sezgisel şekilde cihazlarla etkileşime girmesine izin. Bu etkileşimi yalnızca basit dokunma algılama sınırlı değildir – hareketleri de kullanmak da mümkündür. Örneğin, tutarak yakınlaştırma hareketi kullanıcı yakınlaştırmak veya uzaklaştırmak iki parmakları ekran parçası çimdik tarafından bu – çok yaygın bir örneği bulunmaktadır. Bu kılavuz, dokunma ve iOS hareketleri inceler._


Diğer mobil platformları gibi iOS bir touch işlemek için çeşitli yöntemler vardır. Çok dokunma destekleyebilir — ekranında iletişim birçok noktaları — ve karmaşık hareketleri. Bu kılavuz, bazı kavramlar ve dokunma ve hareket iOS uygulama particularities tanıtır.

iOS yalıtır touch veri `UITouch` bir dizi aracılığıyla uygulamaları için kullanılabilir hale getirilir sınıfı `UIResponder` yöntemleri. Uygulamalar alt sınıflarının bu yöntemleri geçersiz kılmak `UIView` ve `UIViewController`, her ikisi de devralınmalıdır `UIResponder`.

Touch veri yakalamayı yanı sıra iOS hareketleri içine rötuşları desenlerini yorumlanması için sağlar. Bu hareketi tanıyıcıları sırayla döndürme görüntü veya bırakma sayfasının gibi uygulamaya özgü komutlar yorumlamak için kullanılabilir. iOS zengin koleksiyonu minimum eklenen kod ile ortak hareketleri işlemek için sınıflar sağlar.

Rötuşları ve hareketi tanıyıcıları arasında seçim kafa karıştırıcı bir olabilir. Bu kılavuz, genel olarak, tercih hareketi tanıyıcıları verilmesi gerektiğini önerir. Hareketi tanıyıcıları sorunları ve daha iyi saklama büyük ayrılması sağlayan ayrık sınıflar uygulanır. Bu, mantığı yazılan kod miktarını en aza farklı görünümleri arasında paylaşmak basitleştirir.

Ancak, alt düzey dokunma işleme kullanın ve hatta finger-paint programı oluşturmak için birden çok parmakları izlemek için gerektiğinde zamanlar vardır.

## <a name="sections"></a>Bölümler

-  [İOS dokunma](touch-in-ios.md)
-  [İzlenecek yol: İOS dokunma kullanma](ios-touch-walkthrough.md)
-  [İzleme çok dokunma](touch-tracking.md)

Bu kılavuz, dokunmatik giriş iOS içinde görevi görür. 3B dokunma ve Haptic geri bildirim iOS kullanma hakkında daha fazla bilgi için iOS 9 ve 10 sırasıyla aşağıdaki kılavuzları için lütfen bakın de tanıtılan:

* [3D Touch](~/ios/platform/3d-touch.md)
* [Haptic geribildirim sağlama](~/ios/user-interface/ios-ui/haptic-feedback.md)



## <a name="related-links"></a>İlgili bağlantılar

- [iOS Touch başlatın (örnek)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS son Touch (örnek)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [FingerPaint (örnek)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
