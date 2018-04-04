---
title: Temalar
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: c128e6130f0d8c2ba076a82ac85bda5aadea1ee9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="themes"></a>Temalar

![](~/media/shared/preview.png "Bu API şu anda önizlemede değil")

Xamarin.Forms Temalar geliştikçe 2016 duyurdu ve müşterilerin deneyin ve geri bildirim sağlamak önizleme olarak kullanılabilir.

Bir tema ekleyerek bir Xamarin.Forms uygulaması eklenir **Xamarin.Forms.Theme.Base** Nuget paketini, (ör. belirli bir temayı tanımlayan ek bir paket Xamarin.Forms.Theme.Light) veya başka bir yerel tema uygulama için tanımlanmış olması.

Başvurmak [açık tema](light.md) ve [koyu tema](dark.md) sayfaları için bir uygulama eklemek veya kullanıma konusunda yönergeler için [örnek özel tema](custom.md).

**Önemli:** adımları izlemeniz gereken [tema derlemeler (aşağıda) yükleme](#loadtheme) iOS bazı Demirbaş kod ekleyerek `AppDelegate` ve Android `MainActivity`. Bu, gelecekteki Önizleme sürümünde geliştirilmiş.


## <a name="control-appearance"></a>Denetim Görünüm

[Açık](light.md) ve [koyu](dark.md) Temalar hem standart denetimler için belirli bir görsel görünümünü tanımlayın. Uygulamanın kaynak sözlüğüne bir tema ekledikten sonra standart denetimlerin görünümünü değiştirir.

Aşağıdaki XAML biçimlendirme bazı ortak denetimler gösterir:

```xaml
<StackLayout Padding="40">
    <Label Text="Regular label" />
    <Entry Placeholder="type here" />
    <Button Text="OK" />
    <BoxView Color="Yellow" />
    <Switch />
</StackLayout>
```

Bu ekran görüntüleri bu denetimlerle göster:

* Tema uygulandı
* Açık tema (tema yok sahip olmak için yalnızca küçük farklılıklar)
* Koyu tema

![](images/standard-none-sml.png "Tema olmadan denetimler") ![ ] (images/standard-light-sml.png "açık tema denetimleriyle") ![ ] (images/standard-dark-sml.png "koyu tema denetimleriyle")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

`StyleClass` Özelliği, bir görünümün görünüm bir tema tarafından sağlanan bir tanımı göre değiştirilmesine izin verir.

[Açık](light.md) ve [koyu](dark.md) Temalar hem tanımlamak için üç farklı görünümler bir `BoxView`: `HorizontalRule`, `Circle`, ve `Rounded`. Bu biçimlendirme üç farklı gösterir `BoxView`s uygulanan farklı stil sınıflarıyla:

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

Bu açık ve koyu aşağıdaki gibi işler:

![](images/boxview-light-sml.png "Açık tema StyleClass ile BoxView") ![ ] (images/boxview-dark-sml.png "BoxView koyu tema StyleClass ile")

<a name="builtin" />

## <a name="built-in-classes"></a>Yerleşik sınıfları

Otomatik olarak stil oluşturma yanı sıra ortak ışık denetler ve koyu Temalar şu anda destek ayarlayarak uygulanabilir aşağıdaki sınıfları `StyleClass` bu denetimlerinde:

**BoxView**

* HorizontalRule
* Daire
* Yuvarlanmış

**Görüntü**

* Daire
* Yuvarlanmış
* Küçük resim

**Düğme**

* Varsayılan
* birincil
* Başarılı
* Bilgi
* Uyarı
* Tehlike
* Bağlantı
* Küçük
* Büyük

**Etiket**

* Üstbilgi
* Alt Başlığı
* Gövde
* Bağlantı
* Ters


## <a name="troubleshooting"></a>Sorun giderme

<a name="loadtheme" />

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Dosya veya derleme 'Xamarin.Forms.Theme.Light' ya da bağımlılıklarından biri yüklenemedi

Önizleme sürümünde Temalar çalışma zamanında yük mümkün olmayabilir. İlgili projelere bu hatayı düzeltmek için aşağıdaki kodu ekleyin.

**iOS**

İçinde **AppDelegate.cs** sonra aşağıdaki satırları ekleyin `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

İçinde **MainActivity.cs** sonra aşağıdaki satırları ekleyin `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```


## <a name="related-links"></a>İlgili bağlantılar

- [ThemesDemo örnek](https://github.com/xamarin/xamarin-forms-samples/tree/master/Themes/ThemesDemo)
