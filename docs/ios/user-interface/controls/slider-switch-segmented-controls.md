---
title: "Kaydırıcılar, anahtarlar ve bölümlenmiş denetimleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 282a4cb59545703c5172f8747cb5b633e7b648dc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="sliders-switches-and-segmented-controls"></a>Kaydırıcılar, anahtarlar ve bölümlenmiş denetimleri

<a name="Sliders" />


## <a name="sliders"></a>Kaydırıcılar

Kaydırıcı denetimi basit bir sayısal değer bir aralık içinde seçilmesine olanak sağlar. Denetim 0 ile 1 arasında bir değer varsayılan olarak, ancak bu sınırları özelleştirilebilir.

 [![](slider-switch-segmented-controls-images/image25a.png "Kaydırıcı")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Aşağıdaki ekran Tasarımcısı'nda düzenlenebilir özellikleri gösterir:

 [![](slider-switch-segmented-controls-images/image26a.png "Kaydırıcı özellikleri")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Bu değerleri kodda şu anda seçili değer görüntülemek için bir işleyici yukarı kablolama dahil olmak üzere, aşağıda gösterildiği gibi ayarlayabileceğiniz bir `UILabel` denetimi:

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
> Şimdilik bir [hata](http://stackoverflow.com/a/19496179) neden `ThumbTint` çalışma zamanında beklendiği gibi işlenmeyebilir. Aşağıdaki kod satırını ekleyin **önce** yukarıdaki kodu geçici bir çözüm olarak. [[Kaynak](http://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Geçersiz kılınır, ancak bunu yerleştirilir emin olmak gibi herhangi bir görüntü kullanabilirsiniz _içinde_ kaynakları dizin ve kodunuzda çağrılır.

<a name="Switch" />

## <a name="switch"></a>Anahtar

iOS kullanan `UISwitch` bir Boole değeri, temsil ettiği diğer platformlarda radyo düğmesi giriş olarak. Kullanıcı denetimi taşıyarak işleyebileceğiniz *Flash* arasında **açık/kapalı** konumlar.

 [![](slider-switch-segmented-controls-images/image28a.png "geçiş")](slider-switch-segmented-controls-images/image28a.png#lightbox)

İçindeki anahtar görünümünü özelleştirilebilir **özellikleri paneli** varsayılan durumunu denetlemenize olanak tanıyan, Designer **açık/kapalı TINT** renkleri ve bir **açık/kapalı görüntü**. Bu, aşağıdaki resimde gösterilmiştir:

 [![](slider-switch-segmented-controls-images/image29a.png "Anahtar Özellikler")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Anahtar özelliklerini da kodda ayarlanabilir, örneğin aşağıdaki kodu varsayılan değerini anahtarıyla gösterecektir `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Bölümlenmiş denetimleri

Bölümlenmiş bir denetim, az sayıda seçenekleri ile etkileşime girmesine izin vermek için düzenli bir yoludur. Yatay yerleştirilmeden ve her segment ayrı bir düğme olarak işlev görür. Tasarımcı kullanırken, bölümlenmiş denetimi altında bulunabilir **araç > denetimleri**ve aşağıdaki görüntü gibi görünmelidir:

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Bölümlenmiş denetimi")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Aşağıda gösterildiği gibi tasarım yüzeyine ayrı ayrı seçilecek her segment için benzersiz bir özelliği Designer sağlar:

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Bölümlenmiş denetimi")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

Bu daha kesin olarak her segment özelliklerini denetlemek için kullanılacak özellikler paneli sağlar. Düzenlenebilir özellikler aşağıdaki ekran görüntüsünde görebilirsiniz:

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Bölümlenmiş denetimi")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Bölümlenmiş denetim stilini iOS7 içinde kullanım dışı ve bu nedenle, bir iOS7 uygulama bu seçeneklerinde ayarlama hiçbir etkisi olmaz unutulmamalıdır.

## <a name="related-links"></a>İlgili bağlantılar

- [Denetimleri (örnek)](https://developer.xamarin.com/samples/Controls/)
- [Uyarı denetleyicisi](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
