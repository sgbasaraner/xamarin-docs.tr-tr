---
title: Kaydırıcılar, anahtarlar ve bölümlenmiş denetimler Xamarin.iOS içinde
description: Bu belge, slaytlar, anahtarlar ve bölümlenmiş denetimler de onlarla iOS Designer hem program aracılığıyla çalışma açıklayan Xamarin.iOS, açıklanır.
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7df79cb6f225326dda6656fa9dfe9534e35f2457
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241997"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Kaydırıcılar, anahtarlar ve bölümlenmiş denetimler Xamarin.iOS içinde

<a name="Sliders" />

## <a name="sliders"></a>Kaydırıcılar

Kaydırıcı denetiminde sayısal bir değerin bir aralık içinde basit bir seçim olanak tanır. Denetimin 0 ile 1 arasında bir değer varsayılan olarak, ancak bu limitler özelleştirilebilir.

 [![](slider-switch-segmented-controls-images/image25a.png "Kaydırıcı")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Aşağıdaki ekran Tasarımcısı'nda düzenlenebilir özelliklerini gösterir:

 [![](slider-switch-segmented-controls-images/image26a.png "Kaydırıcı özellikleri")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Bu değerler bağlantı şu anda seçili değeri görüntülemek için bir işleyici'kurmak dahil olmak üzere, aşağıda gösterildiği gibi kodda ayarlayabilirsiniz bir `UILabel` denetimi:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Kaydırıcıyı ayarlayarak görsel görünümünü özelleştirebilirsiniz

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Özelleştirilmiş kaydırıcıyı şöyle görünür:

 [![](slider-switch-segmented-controls-images/image27a.png "Özel kaydırıcı")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> Şu anda bir [hata](http://stackoverflow.com/a/19496179) neden `ThumbTint` çalışma zamanında beklendiği gibi işlenmeyebilir. Aşağıdaki kod satırını ekleyebilirsiniz **önce** yukarıdaki geçici bir çözüm olarak kodu. [[Kaynak](http://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Geçersiz kılınacak ancak bunu yerleştirilir emin olun, herhangi bir görüntü kullanabilirsiniz _içinde_ kaynaklar dizini ve kodunuzda çağrılır.

<a name="Switch" />

## <a name="switch"></a>Anahtar

iOS kullanan `UISwitch` bir Boole değeri, temsil ettiği diğer platformlarda bir radyo düğmesi giriş. Kullanıcı denetimi hareket ettirerek işleyebilirsiniz *thumb* arasında **açık/kapalı** konumları.

 [![](slider-switch-segmented-controls-images/image28a.png "Anahtar")](slider-switch-segmented-controls-images/image28a.png#lightbox)

İçindeki anahtar görünümünü özelleştirilebilir **özellikler panelinde** varsayılan durumu denetlemenize imkan sağlayacak tasarımcının **açık/kapalı renk tonu** renkleri ve bir **Kapat görüntü**. Bu, aşağıdaki görüntüde gösterilmiştir:

 [![](slider-switch-segmented-controls-images/image29a.png "Anahtar özellikleri")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Kodda anahtarı özelliklerini de ayarlanabilir, örneğin aşağıdaki kod varsayılan değerini bir anahtarla gösterir `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Bölümlenmiş denetimler

Bölümlenmiş bir denetim, kullanıcıların küçük birçok seçenek ile etkileşim kurmasına izin vermek için düzenli bir yoludur. Yatay olarak yerleştirildiği ve her bir kesim ayrı bir düğme olarak çalışır. Altında Tasarımcısı kullanılırken bölümlenmiş denetimi bulunabilir **araç kutusu > denetimleri**ve şu resimdeki gibi görünmelidir:

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Bölümlenmiş denetimi")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Benzersiz bir özellik Tasarımcısı'nın tasarım yüzeyinde tek tek seçilecek her bir kesim için aşağıda gösterildiği gibi sağlar:

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Bölümlenmiş denetimi")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

Bu, her bir kesim özelliklerini daha kesin bir şekilde denetlemek için kullanılan özellikler panelinde sağlar. Düzenlenebilir özellikler aşağıdaki ekran görüntüsünde görebilirsiniz:

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Bölümlenmiş denetimi")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Bölümlenmiş denetim stili iOS7 ' kullanım dışıdır ve bu nedenle, bu seçenekleri iOS7 uygulamada ayarlama hiçbir etkisi olmayacağını unutulmamalıdır.

## <a name="related-links"></a>İlgili bağlantılar

- [Denetimler (örnek)](https://developer.xamarin.com/samples/Controls/)
- [Uyarı denetleyicisi](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
