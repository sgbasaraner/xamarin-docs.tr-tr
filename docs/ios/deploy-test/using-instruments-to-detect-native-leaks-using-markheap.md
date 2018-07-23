---
title: İşaretlemeler ile Xamarin.iOS uygulamalarının profil oluşturma
description: Bu belge, bir cihaz veya simülatör üzerinde yüklü Xamarin.iOS uygulamasının profilini çıkarmak için Apple'nın Instruments uygulama kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 9b6168eba91a87af88891b9e07e3dd395301cc48
ms.sourcegitcommit: 021027b78cb2f8061b03a7c6ae59367ded32d587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39182214"
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>İşaretlemeler ile Xamarin.iOS uygulamalarının profil oluşturma

Xcode **Instruments** profilini bir cihaz veya simülatör Xamarin.iOS uygulamaları için kullanılabilir bir araçtır. Mono, onun Just-ın-Time kullanan kod ve araçları derlemek için modeli değil yorumlama bu tür verilerin de çıkış araçları kullanan simülatör tabanlı uygulamalardan çalışmak zor olabilir.
Bu sorun nedeniyle, bu kılavuz, geliştirici uygulama araçları çıkış bu belgedeki yorumlamak için kullanma hakkında odaklanacaktır.

## <a name="requirements"></a>Gereksinimler

Xcode araçları yalnızca bir Mac üzerinde çalışır

## <a name="opening-the-instruments-app"></a>Instruments uygulama açma

Cihazı seçin ve araçları uygulamayı çalıştırın:

1. Xamarin.iOS projesi Mac için Visual Studio'da açın.
2. Seçin **hata ayıklama | iPhone** yapılandırma.
3. Bir iOS cihazı bilgisayara bağlayın.
4. İçinde **çalıştırma** menüsünde **cihaza Yükle** . Uygulamayı şimdi oluşturulan ve cihaza karşıya yüklendi.
5. İçinde **Araçları** menüsünde **başlatma Instruments**.


Araçları, şimdi açın ve aşağıdaki iletişim kutusunu görüntüle:

 [![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "Profil şablonu seçme")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png#lightbox)

Seçmek için tıklatın **ayırmaları** şablonu. Yalnızca bu makalede ele alınmaktadır ancak diğer şablon geçerli **ayırmaları** profil şablonu.

Ardından, cihaz ve uygulama penceresinin en üstünde menüsünü kullanarak seçin:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "Cihaz ve uygulama seçin")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png#lightbox)

İOS cihazı seçilecek pencerenin üst kısmındaki menüde ve profili oluşturulacak uygulamayı yanında seçilmelidir (**MemoryDemo** yukarıdaki ekran görüntüsünde).

Cihaz altındaki menüde listede yoksa, kontrol **konsol** uygulama cihaza dağıtıldığında görüntülenebilecek hata iletileri için Mac için Visual Studio'da. Ayrıca, cihaz Xcode Düzenleyicisi aracılığıyla geliştirme için sağlanan emin olun.

Tıklayın **Seç** düğmesi ve sonraki ekranda görünmelidir:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "Profil oluşturma arabirimi")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png#lightbox)

Profil oluşturmayı başlatmak için Kaydet düğmesine (sol üst kırmızı daire) tıklayın.

Aşağıdaki ekran görüntüsü kullanarak profil oluşturma örneği gösterir **Instruments**:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "Profil oluşturma araçları kullanma örneği")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png#lightbox)

## <a name="summary"></a>Özet

Bu kılavuz, Mac için Visual Studio içinden iOS uygulama izlemek için Xcode Araçları'nı başlatmak gösterildi Devam [Instruments izlenecek](~/ios/deploy-test/walkthrough-apples-instrument.md) araçlarını kullanarak bir bellek sorunu tanılamak nasıl bir örneği.

## <a name="related-links"></a>İlgili bağlantılar

- [Instruments gözden geçirme](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Xamarin.iOS çöp toplama (blog gönderisi)](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)
