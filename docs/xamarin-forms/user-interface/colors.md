---
title: Renkleri
description: "Xamarin.Forms bir esnek platformlar arası renk sınıf sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 69ab69efd60c486255164987795db1937b36b97d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="colors"></a>Renkleri

_Xamarin.Forms bir esnek platformlar arası renk sınıf sağlar._

Bu makalede çeşitli şekillerde tanıtılır `Color` sınıfı Xamarin.Forms içinde kullanılabilir.

`Color` Sınıfı, bir renk örneği oluşturma yöntemleri sayısını sağlar

-  **Adlandırılmış renkleri** -genel adlı-dahil olmak üzere renklerin koleksiyonu `Red`, `Green`, ve `Blue`.
-  **FromHex** -dize değeri HTML, örneğin "00FF00" kullanılan sözdizimi benzer. Alfa karakterler ("CC00FF00") ilk çifti olarak isteğe bağlı olarak belirtilebilir.
-  **FromHsla** -ton, Doygunluk ve parlaklığını `double` değerlerle isteğe bağlı alfa değeri (0.0-1.0).
-  **FromRgb** -kırmızı, yeşil ve mavi `int` değerleri (0-255).
-  **FromRgba** -kırmızı, yeşil, mavi ve alfa `int` değerleri (0-255).
-  **FromUint** -tek bir ayarla `double` değerini temsil eden **argb**.

Atanan bazı örnek renkler, işte `BackgroundColor` farklı varyasyonları izin verilen sözdizimi kullanılarak bazı etiketleri:

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

Bu renkler her platformda gösterilir. Son renk - fark `Accent` -blue-ish renk iOS ve Android; için bu değeri Xamarin.Forms tarafından tanımlanır. Windows Phone üzerinde `Accent` kırmızı gösterildiği *, bu cihaz için kullanıcı tarafından seçilen Aksan rengi olduğundan*; kullanıcının tercihlerini bağlı olarak bu değer değişiklikleri.

 [ ![Renk demo](colors-images/colors-sml.png "renk Demo")](colors-images/colors.png "renk Tanıtımı")

## <a name="colordefault"></a>Color.Default

Kullanım `Default` bir renk değerini geri platform varsayılan (her platform için her bir özellik, farklı bir temel alınan rengi temsil ettiğini Anlama) ayarlayın (veya yeniden ayarlamak için).

Geliştiriciler ayarlamak için bu değeri kullanabilirsiniz bir `Color` özelliği ancak gereken **değil** (bunlar tüm ayarlamak için -1), bileşen RGB değerleri için bu örnek sorgu.

## <a name="colortransparent"></a>Color.Transparent

Temizlemek için rengini ayarlayın.

## <a name="coloraccent"></a>Color.Accent

Windows Phone üzerinde kullanıcı tarafından seçilen tamamlayıcı renk budur. İyi Windows Phone uygulamaları kendi stil bir parçası olarak yerel bir görünüm sağlamak için bunu kullanın.

İOS ve Android cihazlarda Bu örnek, varsayılan arka plan üzerinde görünür, ancak varsayılan metin rengi ile aynı değil karşıt bir renk ayarlanır.

## <a name="additional-methods"></a>Ek yöntemleri

`Color` örnekleri yeni renkler oluşturmak için kullanılan ek yöntemler şunlardır:

-  **AddLuminosity** -yeni bir renk tarafından sağlanan delta parlaklığını değiştirerek döndürür.
-  **WithHue** -ton sağlanan değer ile değiştirerek yeni bir renk döndürür.
-  **WithLuminosity** -parlaklığını sağlanan değer ile değiştirerek yeni bir renk döndürür.
-  **WithSaturation** -Doygunluk sağlanan değer ile değiştirerek yeni bir renk döndürür.
-  **MultiplyAlpha** -yeni bir renk tarafından sağlanan alfa değeri çarparak alfa değiştirerek döndürür.

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Bu kod parçacığını kullanan `Device.RuntimePlatform` seçmeli olarak rengini ayarlamak için özellik bir `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>XAML kullanma

Renkleri de kolayca tanımlanmış renk adları veya burada gösterilen onaltılık Beyanları kullanarak XAML'de başvurulabilir:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

## <a name="summary"></a>Özet

Xamarin.Forms `Color` sınıfı platformu algılayan renk başvuruları oluşturmak için kullanılır. Paylaşılan kod ve XAML de kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [Bağlanabilir Seçici (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
