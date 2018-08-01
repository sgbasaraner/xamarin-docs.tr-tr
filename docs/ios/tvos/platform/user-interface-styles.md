---
title: Xamarin stillerde tvOS kullanıcı arabirimi
description: Bu makalede, açık yer almaktadır ve koyu UI temalar, Apple tvOS için 10 ve nasıl Xamarin.tvOS uygulamada uygulanacağı ekledi.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 43bfac29acb8b465fd1f3cdfd53c7664adeae18f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789177"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>Xamarin stillerde tvOS kullanıcı arabirimi

_Bu makalede, açık yer almaktadır ve koyu UI temalar, Apple tvOS için 10 ve nasıl Xamarin.tvOS uygulamada uygulanacağı ekledi._

tvOS tüm yapı içinde Uıkit denetler otomatik olarak koyu hem ışık kullanıcı arabirimi tema destekler uyarlamak için artık 10 kullanıcının tercihlerini temel. Ayrıca, geliştirici kullanıcının seçtiği temasını temel alan kullanıcı Arabirimi öğeleri el ile ayarlayabilirsiniz ve belirli bir tema geçersiz kılabilirsiniz.

<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>Yeni kullanıcı arabirimi stilleri hakkında

Yukarıda belirtildiği gibi tvOS tüm yapı içinde Uıkit denetler otomatik olarak koyu hem ışık kullanıcı arabirimi tema destekler uyarlamak için artık 10 kullanıcının tercihlerini temel.

Kullanıcı Bu tema giderek geçebilirsiniz **ayarları** > **genel** > **Görünüm** ve arasında geçiş yapma **açık**  ve **koyu**:

[![](user-interface-styles-images/theme01.png "Ayarları uygulama")](user-interface-styles-images/theme01.png#lightbox)

Zaman **koyu** tema seçildiğinde, tüm kullanıcı arabirimi öğeleri koyu arka plan üzerinde açık metin geçer:

[![](user-interface-styles-images/theme02.png "Koyu tema")](user-interface-styles-images/theme02.png#lightbox)

Kullanıcı tema herhangi bir zamanda geçiş seçeneği yoktur ve bu nedenle Apple TV bulunduğu geçerli etkinliği veya günün saatini göre yapabilecek.

Varsayılan tema ışık UI tema olduğu ve tvOS koyu Tema yararlanmak için 10 değiştirildiğinde sürece varolan tvOS uygulamalardan hala açık tema, kullanıcının tercihlerini bağımsız olarak kullanır. TvOS 10 uygulama aynı zamanda geçerli tema geçersiz kılmak ve her zaman açık veya koyu tema bazılarını veya tümünü kullanıcı Arabiriminde için silebilir.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>Açık ve koyu Temalar uygulamasını kullanma

Bu özelliği desteklemek için Apple için yeni bir API ekledi `UITraitCollection` sınıfı ve bir tvOS uygulaması gerekir katılımı koyu görünümünü desteklemek için (bir ayar aracılığıyla kendi `Info.plist` dosyası).

Katılımı açık ve koyu tema desteği için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Info.plist` dosyayı düzenlemek için açın.
2. Seçin **kaynak** görünümünden (düzenleyicisinin alt).
3. Yeni bir anahtar ekleyin ve bunu `UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "UIUserInterfaceStyle anahtarı")](user-interface-styles-images/theme03.png#lightbox)
4. Ayarlanan tür bırakın `String` ve değeri girin `Automatic`: 

    [![](user-interface-styles-images/theme04.png "Otomatik girin")](user-interface-styles-images/theme04.png#lightbox)
5. Değişiklikleri dosyaya kaydedin.

Üç olası değerler için `UIUserInterfaceStyle` anahtarı:

- **Açık** -her zaman açık Tema kullanmak için tvOS uygulamanın UI zorlar.
- **Koyu** -tvOS uygulamanın UI koyu tema her zaman kullanmaya zorlar.
- **Otomatik** -kullanıcının tercihlerini ayarlarında temel açık ve koyu tema arasında geçiş yapar. Tercih edilen ayar budur.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>Uıkit tema desteği

TvOS uygulama kullanıyorsa, standart, yerleşik `UIView` denetimleri, bunlar otomatik olarak UI tema Geliştirici müdahalesi olmadan yanıtlar tabanlı.

Ayrıca, `UILabel` ve `UITextView` select UI temasına bağlı renklerine otomatik olarak değiştirir:

- Metin açık tema siyah olacaktır.
- Metin koyu tema beyaz olacaktır.

Geliştirici hiç metin rengi el ile (ya da film şeridi veya kod) değişirse, UI temasına bağlı renk değişikliklerini işlemek için sorumlu olacaktır.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>Yeni Bulanıklaştırma etkileri

Apple tvOS 10 uygulamada açık ve koyu Temalar desteklemek için iki yeni Ölçeklendirilmelidir efektleri ekledi. Bu yeni efektler kullanıcının şu şekilde seçtiği UI temasına bağlı Bulanıklaştırma otomatik olarak ayarlar:

- `UIBlurEffectStyleRegular` -Kullanan bir açık ölçeklendirilmelidir açık tema ve bir koyu koyu tema ölçeklendirilmelidir.
- `UIBlurEffectStyleProminent` -Açık tema içinde çok açık bir bulanıklaştırma ve koyu tema, bir çok koyu Bulanıklaştırma kullanır.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>Ayırdedici nitelik koleksiyonları ile çalışma

Yeni `UserInterfaceStyle` özelliği `UITraitCollection` sınıfı olacaktır ve şu anda seçili UI tema almak için kullanılan bir `UIUserInterfaceStyle` enum, şu değerlerden biri:

- **Açık** -ışık UI tema seçilidir.
- **Koyu** -koyu UI tema seçilidir.
- **Belirtilmeyen** -View gösterilmez henüz, ekran için geçerli UI tema bilinmiyor şekilde.

Ayrıca, ayırdedici nitelik koleksiyonları tvOS 10 aşağıdaki özelliklere sahiptir:
 
- Görünüm proxy göre özelleştirilebilir `UserInterfaceStyle` , bir verilen `UITraitCollection` görüntüleri gibi şeyler değiştirme veya renk temasını temel alan öğesi.
- TvOS uygulama geçersiz kılarak ayırdedici nitelik koleksiyonu değişiklikleri işleyebildiğinden `TraitCollectionDidChange` yöntemi bir `UIView` veya `UIViewController` sınıfı.

> [!IMPORTANT]
> Xamarin.tvOS erken Önizleme tvOS 10 için tam olarak desteklemeyen `UIUserInterfaceStyle` için `UITraitCollection` henüz. Tam destek gelecekteki bir sürümde eklenecek.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>Temasına bağlı görünümünü özelleştirme

Görünüm proxy'sini desteklemek için kullanıcı arabirimi öğeleri, görünümlerini kendi ayırdedici nitelik koleksiyonunun UI tema göre ayarlanabilir. Bu nedenle, belirli bir kullanıcı Arabirimi öğesi için açık tema için tek bir renk ve koyu tema için başka bir renk Geliştirici belirtebilirsiniz.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> Ne yazık ki, Xamarin.tvOS Önizleme tvOS 10 için tam olarak desteklemeyen `UIUserInterfaceStyle` için `UITraitCollection`, bu tür özelleştirme henüz kullanılabilir değil. Tam destek gelecekteki bir sürümde eklenecek.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>Doğrudan tema değişikliklere yanıt verme

Geliştirici gerektirir, geçersiz kılabilirsiniz UI öğesinin görünümünü üzerinde derin denetim temel seçilen UI temayı `TraitCollectionDidChange` yöntemi bir `UIView` veya `UIViewController` sınıfı.

Örneğin:

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);
    
    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="overriding-a-trait-collection"></a>Ayırdedici nitelik koleksiyonu geçersiz kılma

TvOS uygulama tasarımını bağlı olarak, belirli bir kullanıcı arabirimi öğesi ayırdedici nitelik koleksiyonu geçersiz kılmak ve sahip her zaman belirli bir kullanıcı Arabirimi Tema kullanmak için geliştirici gerektiği zaman zamanlar olabilir.

Bu yapılabilir kullanarak `SetOverrideTraitCollection` yöntemi `UIViewController` sınıfı. Örneğin:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

Daha fazla bilgi için lütfen bkz [nitelikler](~/ios/user-interface/storyboards/unified-storyboards.md) ve [geçersiz kılma özellikleri](~/ios/user-interface/storyboards/unified-storyboards.md) bölümlerini bizim [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) belgeleri.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>Ayırdedici nitelik koleksiyonlar ve film şeritleri

TvOS 10, çok sayıda kullanıcı Arabirimi öğeleri açık ve koyu tema haberdar olmanız ve bir uygulamanın film şeridi ayırdedici nitelik koleksiyonlarına yanıt verecek şekilde ayarlayabilirsiniz. Film şeridi Xcode'nın arabirimi Oluşturucu geçici bir çözüm olarak düzenlenmesi gerekir böylece tvOS 10 geçerli Xamarin.tvOS erken önizlemesi arabirimi Tasarımcısı'nda henüz, bu özelliği desteklemiyor.

Ayırdedici nitelik koleksiyonu desteğini etkinleştirmek için aşağıdakileri yapın:

1. Film şeridi dosya üzerinde sağ **Çözüm Gezgini** seçip **birlikte Aç** > **Xcode arabirimi Oluşturucu**: 

    [![](user-interface-styles-images/theme05.png "Xcode arabirimi Oluşturucu ile Aç")](user-interface-styles-images/theme05.png#lightbox) 
2. Ayırdedici nitelik koleksiyonu desteğini etkinleştirmek için geçiş **dosya denetçisi** ve denetleyin **kullanım ayırdedici nitelik Çeşitlemeler** özelliğinde **arabirimi Oluşturucu belge** bölümü: 

    [![](user-interface-styles-images/theme06.png "Ayırdedici nitelik koleksiyonu desteğini etkinleştir")](user-interface-styles-images/theme06.png#lightbox)
3. Ayırdedici nitelik Çeşitlemeler kullanma değişikliğini onaylayın: 

    [![](user-interface-styles-images/theme07.png "Ayırdedici nitelik Çeşitlemeler uyarı kullanın")](user-interface-styles-images/theme07.png#lightbox)
4. Film şeridi dosyasındaki değişiklikleri kaydedin.

Apple tvOS film şeritleri arabirimi Oluşturucu düzenlerken aşağıdaki yeteneklerini ekledi:

* Geliştirici UI temada göre kullanıcı arabirimi öğeleri farklı varyasyonları belirleyebilir **özniteliği denetçisi**:
    
    * Şimdi çeşitli özelliklere sahip bir **+** hangi UI tema belirli bir sürümü eklemek için tıklattığınız yanlarında: 

        [![](user-interface-styles-images/theme08.png "UI tema belirli bir sürümü ekleme")](user-interface-styles-images/theme08.png#lightbox) 
    
    * Geliştirici yeni bir özellik belirtin veya tıklatın **x** düğmesini kaldırmak için: 

        [![](user-interface-styles-images/theme09.png "Yeni bir özellik belirtin veya kaldırmak için x düğmesini tıklatın.")](user-interface-styles-images/theme09.png#lightbox)
* Geliştirici açık veya koyu temayı arabirimi oluşturucu içinde UI tasarımında önizlemesini görüntüleyebilirsiniz:
    
    * Tasarım yüzeyine alt geçerli UI tema geçiş yapmak Geliştirici sağlar: 

        [![](user-interface-styles-images/theme10.png "Tasarım yüzeyine alt")](user-interface-styles-images/theme10.png#lightbox)
        
    * Yeni temayı arabirimi Oluşturucusu'nda görüntülenir ve ayırdedici nitelik koleksiyon belirli ayarlamaları görüntülenir: 

        [![](user-interface-styles-images/theme11.png "Arabirim Oluşturucusu'nda görüntülenen tema")](user-interface-styles-images/theme11.png#lightbox)

Ayrıca, Simulator tvOS şimdi kolayca tvOS uygulama hata ayıklama sırasında açık ve koyu tema arasında geçiş yapmak Geliştirici izin vermek için bir klavye kısayolu vardır. Kullanım **komutu-Shift-D** klavye açık ve koyu arasında geçiş yapmak için dizisi.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede ele alınan açık ve koyu UI temalar, Apple tvOS için 10 ve nasıl Xamarin.tvOS uygulamada uygulanacağı ekledi.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
