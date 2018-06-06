---
title: Xamarin uygulamaları'da erişilebilirlik
description: Bu belge çeşitli ipuçları için erişilebilir uygulamalar oluşturulmasını sağlar. Örneğin, büyük yazı tipleri, yüksek karşıtlık, kendiliğinden açıklayıcı arabirimleri ve daha fazlası hakkında öneriler içerir.
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 97cd3655ac47a017d9590e1890b93d74f10a9c34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780310"
---
# <a name="accessibility-in-xamarin-apps"></a>Xamarin uygulamaları'da erişilebilirlik

_Uygulamalarınızı en geniş olası hedef kitle tarafından kullanışlı olduğundan emin olun_

Erişilebilirlik gibi büyük türü, yüksek karşıtlık yakınlaştırma, işletim sistemi görüntü ve giriş Yardım özellikleri işe uygulama kullanıcı arabirimleri tasarlama, ekran okuma (okuma), visual veya haptic görüş yardımlar kavramı başvuruyor ve diğer giriş yöntemleri.

İOS, Android ve Windows gibi masaüstü ve mobil platformlarda sağlayan yerleşik erişilebilir uygulamalar gibi oluşturma, geliştiricilerin API'lerinde [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) ve [Apple'nın VoiceOver](http://www.apple.com/accessibility/ios/voiceover/).

## <a name="platform-specific-apis"></a>Platforma özgü API'leri

Bu belgedeki yönergeleri uygulamak için her platform tarafından sağlanan API'leri kullanın:

- [**Android erişilebilirlik**](~/android/app-fundamentals/accessibility.md)
- [**iOS erişilebilirlik**](~/ios/app-fundamentals/accessibility.md)
- [**OS X erişilebilirlik**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>Erişilebilirlik denetim listesi

Uygulamalarınızı en geniş izleyici erişilebilir olduğundan emin olmak için aşağıdaki ipuçlarını izleyin. Kullanıma [Android erişilebilirlik sınama denetim listesi](http://developer.android.com/training/accessibility/testing.html) ve [Apple'nın erişilebilirlik sayfasına](http://www.apple.com/accessibility/) ek bilgi için.

### <a name="support-large-fonts-and-high-contrast"></a>Büyük yazı tipleri ve yüksek karşıtlık desteği

Cmdlet'e kod denetim boyutları oluşturmamaya özen gösterin ve bunun yerine, daha büyük yazı tipi boyutlarını uyum sağlayacak şekilde yeniden boyutlandırabilirsiniz düzenleri tercih eder.
Renk düzenleri okunabilir olmasını sağlamak için yüksek karşıtlık modunda sınayın.

### <a name="make-the-user-interface-self-describing"></a>Kullanıcı arabirimi kendiliğinden açıklayıcı olun

Açıklayıcı metni ve ekran API'leri her platformda okuma ile uyumlu olan ipuçları ile kullanıcı arabiriminizi tüm öğeleri etiketleyin.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>Görüntüleri ve simgeleri bir alternatif metin açıklaması sahip olduğundan emin olun

Resimler ve uygulama kullanıcı arabirimi (örneğin, düğmeler veya örneğin durum göstergelerini) parçası olan simgeler ile erişilebilir bir açıklama etiketli.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>Görsel ağaç aklınızda erişilebilir Gezinti ile tasarlama

Uygun düzen denetimleri kullanın veya diğer giriş yöntemleri kullanarak denetimleri arasında gezinme böylece dokunmatik ekran kullanma olarak aynı mantıksal akış API'leri izler.

Gereksiz öğeler ekran okuyucular (dekoratif görüntüleri veya zaten örneğin erişilebilir, alanlar için etiketleri) dışında bırakın.

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>Tek başına ses veya renk yardımlar hakkında güvenmeyin

İlerleme durumunu, tamamlanma veya başka bir duruma tek göstergesi ses veya renk değişikliği olduğu durumlar kaçının. Da NET bir görsel ipuçlarıyla (ses ve yalnızca öğrenmeyi rengini) dahil etmek veya belirli erişilebilirlik göstergeleri eklemek için kullanıcı arabirimi tasarlayın.

Renkleri seçerken, kullanıcılar için renk görme engeli ile ayırt etmek sabit bir palet kaçınmaya çalışın.

### <a name="captioning-for-video-text-for-audio"></a>Video, Ses metni için açıklamalı alt yazı

Video içeriği ve ses içeriği için okunabilir bir komut dosyası için resim yazıları sağlayın. Ses veya video içeriği hızını ayarlama denetimleri sağlar ve bu birimde sağlamak yararlıdır ve Yürüt/Duraklat düğmeleri bulabilir ve kolaydır.

### <a name="localize"></a>Yerelleştirme

Erişilebilirlik açıklamaları burada uygulamanın birden çok dili destekleyen yerelleştirilmesi olabilir ve gerekir.



## <a name="related-links"></a>İlgili bağlantılar

- [Android erişilebilirlik](~/android/app-fundamentals/accessibility.md)
- [iOS erişilebilirlik](~/ios/app-fundamentals/accessibility.md)
- [OS X erişilebilirlik](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms erişilebilirlik](~/xamarin-forms/app-fundamentals/accessibility/index.md)
