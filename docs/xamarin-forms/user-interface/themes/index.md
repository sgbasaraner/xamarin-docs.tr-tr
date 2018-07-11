---
title: Xamarin.Forms temalar
description: Bu makalede Standart görünümler için belirli visual görünümlerini tanımlamak Xamarin.Forms Temalar tanıtılmaktadır.
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0f49eeba072d6aeb7ead40d5d56d4af9e9bf5e27
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38814713"
---
# <a name="xamarinforms-themes"></a>Xamarin.Forms temalar

![](~/media/shared/preview.png "Bu API, şu anda Önizleme aşamasındadır")

Xamarin.Forms Temalar geliştikçe 2016'da açıkladığımız ve müşterilerin deneyin ve geri bildirim sağlamak için Önizleme olarak kullanılabilir.

Bir tema dahil ederek bir Xamarin.Forms uygulaması eklenir **Xamarin.Forms.Theme.Base** Nuget paketini, (örn. özel bir temayı tanımlayan bir ek paket Xamarin.Forms.Theme.Light) veya başka bir uygulama için yerel bir tema tanımlanabilir.

Başvurmak [açık tema](light.md) ve [koyu tema](dark.md) sayfaları için bunları bir uygulamaya ekleme veya kullanıma konusunda yönergeler [örnek özel tema](custom.md).

**Önemli:** adımlarını izleyin [tema derlemeleri (aşağıda) yükleme](#loadtheme) iOS bazı ortak kod ekleyerek `AppDelegate` ve Android `MainActivity`. Bu, gelecekteki Önizleme sürümünde geliştirilecektir.


## <a name="control-appearance"></a>Denetim Görünüm

[Işık](light.md) ve [koyu](dark.md) Temalar hem standart denetimler için belirli bir görsel görünümünü tanımlayın. Bir tema için uygulamanın kaynak sözlüğü ekledikten sonra standart denetimlerin görünümünü değiştirir.

Aşağıdaki XAML biçimlendirme bazı ortak denetimleri gösterir:

```xaml
<StackLayout Padding="40">
    <Label Text="Regular label" />
    <Entry Placeholder="type here" />
    <Button Text="OK" />
    <BoxView Color="Yellow" />
    <Switch />
</StackLayout>
```

Bu ekran görüntüleri ile bu denetimleri göster:

* Tema uygulandı
* Açık tema (Tema olması için yalnızca farklar)
* Koyu renkli tema

![](images/standard-none-sml.png "Tema olmadan denetimler") ![](images/standard-light-sml.png "açık tema denetimleriyle") ![](images/standard-dark-sml.png "koyu tema denetimleriyle")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

`StyleClass` Özelliği, bir görünümün görünüm tema tarafından sağlanan bir tanımına göre değiştirilmesine izin verir.

[Işık](light.md) ve [koyu](dark.md) Temalar hem tanımlamak için üç farklı görünümlerini bir `BoxView`: `HorizontalRule`, `Circle`, ve `Rounded`. Bu işaretleme üç farklı gösterir `BoxView`uygulanan farklı bir stil sınıflarla s:

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

Bu açık ve koyu şu şekilde işlenir:

![](images/boxview-light-sml.png "Açık tema StyleClass ile BoxView") ![](images/boxview-dark-sml.png "BoxView koyu tema StyleClass ile")

<a name="builtin" />

## <a name="built-in-classes"></a>Yerleşik sınıflar

Otomatik olarak stil yanı sıra ortak denetimleri açık ve koyu temaları şu anda ayarlayarak uygulanabilir aşağıdaki sınıflar Destek `StyleClass` bu denetimleri:

**BoxView**

* HorizontalRule
* Daire
* Yuvarlak

**Görüntü**

* Daire
* Yuvarlak
* Küçük resim

**Düğme**

* Varsayılan
* Birincil
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

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Dosya veya derleme 'Xamarin.Forms.Theme.Light' veya bağımlılıklarından biri yüklenemedi

Önizleme sürümünde, temalar çalışma zamanında yüklemek mümkün olmayabilir. Aşağıda bu hatayı düzeltmek için ilgili projeleri içinde gösterilen kodu ekleyin.

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
