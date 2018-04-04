---
title: Ekran boyutlarıyla çalışma
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: 12272b6db10249fb3750edd853ebb01a99586427
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-screen-sizes"></a>Ekran boyutlarıyla çalışma

Android yıpranması aygıtların bir dikdörtgen ya da farklı boyutlarda olabilir yuvarlak bir görüntü olabilir.

![Dikdörtgen ve yuvarlak yıpranması ekran görüntüleri görüntüler](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>Ekran türü tanımlama

Yıpranması destek kitaplığı yardımcı bazı denetimler algılamak ve farklı ekran şekillere gibi uyum sağlar `WatchViewStub` ve `BoxInsetLayout`.

Diğer bazı kitaplığı denetimleri desteklemediğini unutmayın (gibi `GridViewPager`) *otomatik olarak* kendilerini ekran şeklini Algıla ve denetimleri alt aşağıda açıklandığı gibi eklenmesi gerekir.

### <a name="watchviewstub"></a>WatchViewStub

Bkz: [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/) ekran türü algılamak ve her tür için farklı bir düzen görüntülemek nasıl görmek için örnek.

Ana Düzen dosyasını içeren bir `android.support.wearable.view.WatchViewStub` kullanarak dikdörtgen ve yuvarlak ekranları için farklı düzenler başvuran `app:rectLayout` ve `app:roundLayout` öznitelikleri:

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

Çözüm, çalışma zamanında seçili her stili için farklı düzenleri içerir:

![Kaynakları/düzeni altında gösterilen dosyaları](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

Her bir ekran türü için farklı düzenler oluşturmak yerine, dikdörtgen veya yuvarlak ekranlara uyum tek bir görünüm oluşturabilirsiniz.

Bu [Google örnek](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) nasıl kullanılacağını gösterir `BoxInsetLayout` dikdörtgen ve yuvarlak ekranlarda aynı düzeni kullanmak için.


## <a name="wear-ui-designer"></a>UI Tasarımcısı takmak

Xamarin Android Tasarımcı dikdörtgen ve yuvarlak ekranları destekler:

![Xamarin Android Tasarımcısı'nda Android yıpranması kare ekran seçme](screen-sizes-images/design-screen-type.png)

Dikdörtgen stilde tasarım yüzeyi burada gösterilir:

![Dikdörtgen stilde tasarım yüzeyi](screen-sizes-images/design-rect.png) 

Yuvarlak stilde tasarım yüzeyi burada gösterilir:

![Yuvarlak stilde tasarım yüzeyi](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Simulator takmak

**Google öykünücü yöneticisini** her ikisi de türleri ekran aygıt tanımları içerir. Uygulamanızı test etmek için dikdörtgen ve yuvarlak Öykünücüler oluşturabilirsiniz.

![Google Öykünücüsü Yöneticisi'nde gösterilen aygıt tanımları takmak](screen-sizes-images/emulator-devices.png)

Öykünücü şöyle bir dikdörtgen ekranın oluşturulur:

![Öykünücü dikdörtgen ekranı oluşturma](screen-sizes-images/recipe-2.png) 

Bu gibi yuvarlak bir ekranın oluşturulur:

![Öykünücü yuvarlak bir ekran oluşturma](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>Video

[Tam ekran uygulamaları Android yıpranması için](https://www.youtube.com/watch?v=naf_WbtFAlY) gelen [developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).

