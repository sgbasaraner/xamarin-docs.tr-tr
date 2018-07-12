---
title: Xamarin.Forms renkleri
description: Xamarin.Forms, esnek bir platformlar arası renk sınıf sağlar. Bu makalede, renk sınıfı ve nasıl kullanılacağını tarafından sağlanan işlevselliği açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 1017f108d6808155cac84e98a811a30d09afa134
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986089"
---
# <a name="colors-in-xamarinforms"></a>Xamarin.Forms renkleri

_Xamarin.Forms, esnek bir platformlar arası renk sınıf sağlar._

Bu makalede çeşitli yollar sunar `Color` sınıfı Xamarin.Forms içinde kullanılabilir.

`Color` Sınıfı bir dizi bir renk örneği oluşturmak için yöntem sağlar

-  **Adlandırılmış renkler** -ortak adlı-renkleri koleksiyonunu `Red`, `Green`, ve `Blue`.
-  **FromHex** -dize değeriyle benzer şekilde, HTML, örneğin "00FF00" kullanılan sözdizimi. Alfa, isteğe bağlı olarak ilk karakter ("CC00FF00") çifti olarak belirtilebilir.
-  **FromHsla** -ton, Doygunluk ve parlaklık `double` değerlerle isteğe bağlı bir alfa değeri (0,0-1.0).
-  **FromRgb** -kırmızı, yeşil ve mavi `int` değerlerini (0-255).
-  **FromRgba** -kırmızı, yeşil, mavi ve alfa `int` değerlerini (0-255).
-  **FromUint** -tek bir kümesi `double` değerini temsil eden **argb**.

Bazı örnek atanan renkler, işte `BackgroundColor` farklı çeşitleri izin verilen söz dizimi kullanarak bazı etiketler:

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

Bu renklerin her platformda gösterilmektedir. Son rengini - fark `Accent` -iOS ve Android için blue-ish bir renk bu değer Xamarin.Forms tarafından tanımlanır.

 [![Renk tanıtım](colors-images/colors-sml.png "renk tanıtım")](colors-images/colors.png#lightbox "renk Tanıtımı")

## <a name="colordefault"></a>Color.Default

Kullanım `Default` platform varsayılan (her bir özellik için her platformda farklı bir temel rengin temsil ettiğini Anlama) bir renk değeri ayarlayın (veya yeniden ayarlamak için).

Geliştiriciler bu değeri ayarlamak için kullanabilir bir `Color` özelliği ancak gereken **değil** Bu örneği (bunlar tüm ayarlamak için -1), bileşen RGB değerleri için sorgu.

## <a name="colortransparent"></a>Color.Transparent

Temizlemek için rengini ayarlayın.

## <a name="coloraccent"></a>Color.Accent

İOS ve Android'de Bu örneği için varsayılan arka plan üzerinde görünür ancak varsayılan metin rengi ile aynı değil renklerden bir renk ayarlanır.

## <a name="additional-methods"></a>Ek yöntemleri

`Color` örnekler, yeni renkleri oluşturmak için kullanılan ek yöntemler şunlardır:

-  **AddLuminosity** -tarafından sağlanan delta parlaklık değiştirerek yeni bir renk döndürür.
-  **WithHue** -hue sağlanan değer ile değiştirerek yeni bir renk döndürür.
-  **WithLuminosity** -parlaklık sağlanan değer ile değiştirerek yeni bir renk döndürür.
-  **WithSaturation** -doygunluğu sağlanan değer ile değiştirerek yeni bir renk döndürür.
-  **MultiplyAlpha** -sağlanan alfa değeri ile çarparak alfa değiştirerek yeni bir renk döndürür.

## <a name="implicit-conversions"></a>Örtük dönüşümler

Arasında örtük dönüştürme `Xamarin.Forms.Color` ve `System.Drawing.Color` türleri gerçekleştirilebilir:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Bu kod parçacığı kullandığı `Device.RuntimePlatform` seçmeli olarak rengini ayarlamak için özellik bir `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>XAML kullanma

Renkleri, tanımlanmış bir rengi adları veya burada gösterilen onaltılık ifadeleri kullanarak XAML içinde de bir kolayca başvurulabilir:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> XAML derleme kullanırken, renk adlarını büyük küçük harf ve küçük harflerle yazılabilir. XAML derleme hakkında daha fazla bilgi için bkz: [XAML derleme](~/xamarin-forms/xaml/xamlc.md).

## <a name="summary"></a>Özet

Xamarin.Forms `Color` sınıfı platformu algılayan renk başvuru oluşturmak için kullanılır. Paylaşılan kod ve XAML içinde kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [Bağlanabilir Seçici (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
