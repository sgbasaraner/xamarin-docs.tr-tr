---
title: Profil oluşturma araçları ile Xamarin.iOS uygulamaları
description: Bu belgede bir aygıt veya bir simulator yüklü bir Xamarin.iOS uygulaması profil için Apple'nın Instruments uygulama kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 25129d532af0b146afedf28865649ffc9e38ee17
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785618"
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>Profil oluşturma araçları ile Xamarin.iOS uygulamaları

Xcode **Instruments** çok Xamarin.iOS uygulamaları bir aygıt veya benzetici profil için kullanılan bir araçtır. Mono kullanır, sadece zaman kodu ve araçları derlemek için model değil yorumlama bu tür verilerin de Instruments kullanan simulator tabanlı uygulamaları çıktısını çalışmak zor olabilir.
Bu sorun nedeniyle, bu kılavuz, bu belgedeki Instruments çıktısını yorumlamak için geliştirici uygulama kullanımı hakkında odaklanacaktır.

## <a name="requirements"></a>Gereksinimler

Xcode Instruments yalnızca bir Mac bilgisayar üzerinde çalışır

## <a name="opening-the-instruments-app"></a>Instruments uygulama açma

Cihazı seçin ve Instruments uygulamayı çalıştırın:

1.  Xamarin.iOS projesi Mac için Visual Studio'da açın
2.  Seçin **hata ayıklama | iPhone** yapılandırma.
3.  Bir iOS cihazı bilgisayara bağlayın.
4.  İçinde **çalıştırmak** menüsünde, select **cihaza karşıya** . Uygulamayı şimdi oluşturulur ve cihaza yüklenir.
5.  İçinde **Araçları** menüsünde, select **başlatma Instruments**.


Instruments şimdi açın ve aşağıdaki iletişim kutusu görüntüler:

 [![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "Profil şablonu seçme")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png#lightbox)

Seçmek için tıklatın **ayırmaları** şablonu. Bu makalede yalnızca ele ancak diğer şablonlar geçerli **ayırmaları** profil şablonu.

Ardından, cihaz ve uygulama penceresinin en üstünde menüsünü kullanarak seçin:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "Cihaz ve uygulama seçin")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png#lightbox)

İOS cihazı pencerenin üstündeki menüde seçili ve yanındaki profillerinin oluşturulmasını uygulama seçilmelidir (**MemoryDemo** yukarıdaki ekran görüntüsü).

Cihaz altında menüde listede yoksa, denetleme **konsol** cihaza uygulama dağıtıldığında görüntülenebilecek hata iletileri için Mac için Visual Studio. Ayrıca, cihaz geliştirme Xcode Düzenleyicisi aracılığıyla sağlanan emin olun.

Tıklatın **Seç** düğmesini ve sonraki ekranda görünmelidir:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "Profil oluşturma arabirimi")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png#lightbox)

Profil oluşturmayı başlatmak için (sol üst kırmızı daire) kaydı düğmesini tıklatın.

Aşağıdaki ekran görüntüsü kullanarak profil örneği gösterir **Instruments**:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "Profil oluşturma araçlarını kullanarak bir örnek")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png#lightbox)

## <a name="summary"></a>Özet

Bu kılavuz Xcode Mac için Visual Studio içinde bir iOS uygulaması izlemek için araçları başlatmak nasıl oluşturulacağını gösterir. İle devam [Instruments izlenecek](~/ios/deploy-test/walkthrough-apples-instrument.md) araçlarını kullanarak bir bellek sorunu tanılamak nasıl bir örneği.

## <a name="related-links"></a>İlgili bağlantılar

- [Instruments gözden geçirme](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Xamarin.iOS çöp toplama](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
