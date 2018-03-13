---
title: "Xamarin.Mac uzantısı desteği"
description: "Bu makalede uzantısı desteği Xamarin.Mac sürüm 2.10 (ve büyük) içinde yer almaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 530e53230e9f0dea165b083fa6795558025a293f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="xamarinmac-extension-support"></a>Xamarin.Mac uzantısı desteği

Xamarin.Mac 2.10 Önizleme'de birden çok macOS uzantı noktaları için destek eklendi:

- Bulucu
- Paylaş
- Bugün

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>Sınırlamalar ve bilinen sorunlar

Aşağıdaki sınırlamalar vardır ve Xamarin.Mac uzantılarında geliştirirken oluşabilecek sorunları bildirin:

* Şu anda Mac için Visual Studio'da hata ayıklama desteği yoktur Tüm hata ayıklama aracılığıyla yapılması gerekecek **NSLog** ve **konsol**. Ayrıntılar için aşağıdaki ipuçları bölümüne bakın.
* Uzantıları içerilmelidir bir ana bilgisayar uygulaması olan çalıştırmak bir kez kayıt sistemi ile. İçinde sonra etkinleştirilmelidir **uzantısı** bölümünü **sistem tercihleri**. 
* Uzantı çökmeleri konak uygulama kararlılığını ve garip davranışlara neden olabilir. Özellikle, **Bulucu** ve **Bugün** bölümünü **bildirim Merkezi** "sıkışmış" olur ve yanıt veremez duruma gelebilir. Bu uzantı projelerinde xcode'da de deneyimli bırakıldı ve şu anda Xamarin.Mac için ilgisiz görünür. Genellikle bu sistem günlüğünde görebilirsiniz (aracılığıyla **konsol**, ipuçları Ayrıntılar için bkz:) yinelenen hata iletilerini yazdırma. Bu sorunu gidermek için macOS yeniden görüntülenir.

<a name="Tips" />

## <a name="tips"></a>İpuçları

Aşağıdaki ipuçları Xamarin.Mac uzantıları ile çalışırken yararlı olabilir:

- Hata ayıklama deneyimini Xamarin.Mac hata ayıklama uzantıları şu anda desteklemediğinden, öncelikle yürütülmesine bağlıdır ve `printf` deyimleri ister. Bununla birlikte, uzantıları içinde korumalı alan işlemi, bu nedenle çalıştırmanız `Console.WriteLine` diğer Xamarin.Mac uygulamalarda yaptığı gibi davranacak değil. Çağırma [ `NSLog` doğrudan](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) sistem günlüğüne hata ayıklama iletilerini çıkarır.
- Yakalanmayan Özel durumlar az miktarda yararlı bilgiler sağlayan uzantısı işlemi kilitleniyor **sistem günlüğüne**. Sorunlu kodda kaydırma bir `try/catch` (özel), engelleme `NSLog`kullanıcının önce yeniden atma yararlı olabilir.
- **Sistem günlüğüne** ulaşılabiliyor **konsol** altında uygulama **uygulamaları** > **yardımcı programları**:

    [![](extensions-images/extension02.png "Sistem günlüğü")](extensions-images/extension02.png#lightbox)
- Yukarıda belirtildiği gibi uzantı ana bilgisayar uygulaması çalıştıran sistemiyle kaydeder. Uygulama Paketle silme, kaydı. 
- Bir uygulamanın uzantıları "parazit" sürümlerini kaydedilmişse (bunlar silinebilir şekilde) bunları bulmak için aşağıdaki komutu kullanın: `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>Gözden geçirme ve örnek uygulama

Geliştirici oluşturma ve Xamarin.iOS uzantıları aynı şekilde Xamarin.Mac uzantılarıyla çalışma olduğundan, lütfen bizim [uzantıları giriş](~/ios/platform/extensions.md) daha fazla ayrıntı için belgeleri.

Her bir uzantı türü çalışma örnekleri küçük içeren bir örnek Xamarin.Mac proje bulunabilir [burada](https://developer.xamarin.com/samples/mac/ExtensionSamples/).

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede Xamarin.Max sürüm 2.10 (ve büyük) uygulama uzantıları ile çalışan bir Hızlı Bakış sürdü.

## <a name="related-links"></a>İlgili bağlantılar

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
