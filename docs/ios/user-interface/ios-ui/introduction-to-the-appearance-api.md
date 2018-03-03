---
title: "Görünüm API"
description: "iOS, uygulama bu denetimin tüm örneklerine değişikliği uygulanmasını görsel özelliği ayarları statik sınıf düzeyinde yerine ayrı ayrı nesneleri uygulamanıza olanak sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 6d2a454665691c028fe8307940a5662a98ab9c98
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="appearance-api"></a>Görünüm API

_iOS, uygulama bu denetimin tüm örneklerine değişikliği uygulanmasını görsel özelliği ayarları statik sınıf düzeyinde yerine ayrı ayrı nesneleri uygulamanıza olanak sağlar._

Bu işlev içinde Xamarin.iOS statik sunulur `Appearance` destekleyen tüm Uıkit denetimlerinde özellik. Görsel görünümünü (ton renk ve arka plan görüntüsü olarak özellikleri gibi) bu nedenle kolayca uygulamanızı tutarlı bir görünüm kazandırmak için özelleştirilebilir. Görünüm API iOS 5 sunulmuştur ve iOS 9 bazı kısımları kullanım dışı olsa da hala bazı stil ve Xamarin.iOS uygulamaları tema efektleri elde etmek için iyi bir yoludur.

## <a name="overview"></a>Genel Bakış

iOS uygulamanızı uygulanmasını istediğiniz marka uygun standart denetimleri yapmak için birçok Uıkit denetimlerin görünümünü özelleştirmenize olanak tanır.

Özel bir görünüm uygulamak için iki farklı yolu vardır:

- **Bir denetim örneği üzerinde doğrudan** – araç çubukları, gezinti çubukları, düğmeler ve kaydırıcılar dahil olmak üzere birçok denetimlere renk, arka plan resmi ve başlık konumunun (yanı sıra diğer bazı öznitelikler) ayarlayabilirsiniz.

- **Varsayılan görünüm statik özelliği ayarlamak** – her denetim için özelleştirilebilir öznitelikler aracılığıyla sunulur `Appearance` statik özelliği. Özelleştirmeler için bu özellikleri uygulamak özelliği ayarladıktan sonra oluşturulan bu türdeki herhangi bir denetimi için varsayılan olarak kullanılır.

Görünüm örnek uygulaması, bu ekran görüntülerinde gösterildiği gibi tüm üç yöntemi gösterilmektedir:

 [ ![](introduction-to-the-appearance-api-images/appearance01.png "Görünüm örnek uygulaması üçünü gösterir")](introduction-to-the-appearance-api-images/appearance01.png)

İOS 8 itibariyle görünüm proxy TraitCollections için genişletilmiştir.
 `AppearanceForTraitCollection` Varsayılan görünüm üzerinde belirli ayırdedici nitelik koleksiyonu ayarlamak için kullanılabilir. Daha fazla bilgiyi bu konuda [film şeritleri giriş](~/ios/user-interface/storyboards/unified-storyboards.md) Kılavuzu.


## <a name="setting-appearance-properties"></a>Görünüm özelliklerini ayarlama

İlk ekranda statik görünüm sınıf düğmeleri ve bu gibi sarı/turuncu öğeler stilini belirlemek için kullanılır:

```csharp
// Set the default appearance values
UIButton.Appearance.TintColor = UIColor.LightGray;
UIButton.Appearance.SetTitleColor(UIColor.FromRGB(0,127,14), UIControlState.Normal);

UISlider.Appearance.ThumbTintColor = UIColor.Red;
UISlider.Appearance.MinimumTrackTintColor = UIColor.Orange;
UISlider.Appearance.MaximumTrackTintColor = UIColor.Yellow;

UIProgressView.Appearance.ProgressTintColor = UIColor.Yellow;
UIProgressView.Appearance.TrackTintColor = UIColor.Orange;
```

Yeşil öğe stillerini şu şekilde de ayarlanır `ViewDidLoad` varsayılan değerleri geçersiz kılan yöntemi ve *Görünüm* statik sınıf:

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Xamarin.Forms içinde UIAppearance kullanma

Görünüm API durumlarda kullanışlı olabilir [iOS uygulaması stil oluşturma](~/xamarin-forms/platform/ios/theme.md#uiappearance) Xamarin.Forms çözümlerinde. Birkaç satır `AppDelegate` oluşturmak zorunda kalmadan belirli bir renk şeması uygulamak için yardımcı sınıfı bir [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).


### <a name="custom-themes-and-uiappearance"></a>Özel temalar ve UIAppearance

iOS arabirimi denetimleri "konulu" kullanarak kullanıcının çok sayıda görsel nitelikler verir *UIAppearance* aynı görünüme sağlamak için belirli bir denetim tüm örneklerini zorlamak için API'ler. Bu, çok sayıda kullanıcı arabirimi denetim sınıfları, tek tek örneklerini denetimi değil, bir görünüm özelliği olarak sunulur. Üzerinde statik bir görüntü özellik ayarlama `Appearance` özelliği, uygulamanızdaki bu türdeki tüm denetimler etkiler.

Kavram daha iyi anlamak için bir örneği göz önünde bulundurun.

Belirli bir değiştirmek için `UISegmentedControl` macenta TINT sağlamak için Biz bu şekilde bizim ekranında belirli denetim başvuru `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

Alternatif olarak, değer Tasarımcısı özellikleri defterinde ayarlayın: 

[ ![](introduction-to-the-appearance-api-images/propertiespadtint.png "Özellikler paneli TINT")](introduction-to-the-appearance-api-images/propertiespadtint.png)

Aşağıdaki resimde bu TINT 'sg1' adlı denetiminde ayarlayan gösterilmektedir.

 [ ![](introduction-to-the-appearance-api-images/image53.png "Bireysel denetim TINT ayarlama")](introduction-to-the-appearance-api-images/image53.png)

Böylece biz yerine statik ayarlayabilirsiniz birçok denetimleri bu şekilde ayarlamak için tamamen verimsiz olacaktır `Appearance` sınıf özelliği. Bu kodda gösterilir:

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

Aşağıdaki görüntü iki parçalı denetimleri için macenta ayarlamak görünümle şimdi gösterilmektedir:

 [ ![](introduction-to-the-appearance-api-images/image54.png "Görünüm denetim TINT ayarlama")](introduction-to-the-appearance-api-images/image54.png)

`Appearance` özellikleri ayarlanmalıdır erken uygulama yaşam döngüsündeki gibi AppDelegate içinde 's `FinishedLaunching` olayı veya etkilenen denetimleri görüntülenmeden ViewController.


Başvurmak [görünüm API giriş](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) daha ayrıntılı bilgi için.


## <a name="related-links"></a>İlgili bağlantılar

- [Görünüm (örnek)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [UIAppearance Protokolü başvurusu](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Xamarin.Forms görünümü](~/xamarin-forms/platform/ios/theme.md#uiappearance)
