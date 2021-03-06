---
title: Xamarin.iOS uygulamaları dokunma işleme
description: Bu belge bağlantılar kılavuzlara dokunma, çok dokunma, hareketleri ve Xamarin.iOS uygulama 3B dokunma ile çalışmayı açıklar.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: eb8dce8b13345c13a6f95ae7784bd135e7d1f1f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784168"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Xamarin.iOS uygulamaları dokunma işleme

Diğer mobil platformları gibi iOS bir touch işlemek için çeşitli yöntemler vardır. Çok dokunma destekleyebilir — ekranında iletişim birçok noktaları — ve karmaşık hareketleri. Bu kılavuz, bazı kavramlar ve dokunma ve hareket iOS uygulama particularities tanıtır.

iOS yalıtır touch veri `UITouch` bir dizi aracılığıyla uygulamaları için kullanılabilir hale getirilir sınıfı `UIResponder` yöntemleri. Uygulamalar alt sınıflarının bu yöntemleri geçersiz kılmak `UIView` ve `UIViewController`, her ikisi de devralınmalıdır `UIResponder`.

Touch veri yakalamayı yanı sıra iOS hareketleri içine rötuşları desenlerini yorumlanması için sağlar. Bu hareketi tanıyıcıları sırayla döndürme görüntü veya bırakma sayfasının gibi uygulamaya özgü komutlar yorumlamak için kullanılabilir. iOS zengin koleksiyonu minimum eklenen kod ile ortak hareketleri işlemek için sınıflar sağlar.

Rötuşları ve hareketi tanıyıcıları arasında seçim kafa karıştırıcı bir olabilir. Bu kılavuz, genel olarak, tercih hareketi tanıyıcıları verilmesi gerektiğini önerir. Hareketi tanıyıcıları sorunları ve daha iyi saklama büyük ayrılması sağlayan ayrık sınıflar uygulanır. Bu, mantığı yazılan kod miktarını en aza farklı görünümleri arasında paylaşmak basitleştirir.

Ancak, alt düzey dokunma işleme kullanın ve hatta finger-paint programı oluşturmak için birden çok parmakları izlemek için gerektiğinde zamanlar vardır.

## <a name="sections"></a>Bölümler

-  [iOS’ta Dokunma](touch-in-ios.md)
-  [İzlenecek yol: İOS dokunma kullanma](ios-touch-walkthrough.md)
-  [Çok Dokunmalı İzleme](touch-tracking.md)

Bu kılavuz, dokunmatik giriş iOS içinde görevi görür. 3B dokunma ve Haptic geri bildirim iOS kullanma hakkında daha fazla bilgi için iOS 9 ve 10 sırasıyla aşağıdaki kılavuzları için lütfen bakın de tanıtılan:

* [3D Touch](~/ios/platform/3d-touch.md)
* [Dokunmatik Geri Bildirim Sağlama](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>İlgili bağlantılar

- [iOS Touch başlatın (örnek)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS son Touch (örnek)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [FingerPaint (örnek)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
