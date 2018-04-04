---
title: XAML standart (Önizleme) denetimleri
description: Xamarin.Forms XAML standart önizlemede araştırmaya nasıl
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 2fc7fb9581f344e0d54bd9f690d334eda78cc97a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-standard-preview-controls"></a>XAML standart (Önizleme) denetimleri

![Önizleme](~/media/shared/preview.png)

Bu sayfada, eşdeğer kendi Xamarin.Forms denetimi yanında Önizleme'de kullanılabilir XAML standart denetimler listelenir.

Ayrıca yeni özellik ve numaralandırma adlarını XAML standart denetimleri listesi verilmiştir.

## <a name="controls"></a>Denetimler

|Xamarin.Forms|XAML standart|
|--- |--- |
|Çerçeve|Kenarlık|
|Picker|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|Etiketle|TextBlock|
|Giriş|TextBox|
|Anahtar|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Özellikleri ve numaralandırmaları

|Güncelleştirilmiş özellikleriyle Xamarin.Forms denetimleri|Xamarin.Forms özelliği ya da enum|XAML standart eşdeğeri|
|--- |--- |--- |
|Düğme, giriş, etiket, DatePicker, düzenleyici, SearchBar, TimePicker|TextColor|Ön plan|
|VisualElement|BackgroundColor|Arka plan *|
|Picker, Button|BorderColor, OutlineColor|BorderBrush|
|Düğme|BorderWidth|BorderThickness|
|ProgressBar|İlerleme durumu|Değer|
|Düğme, giriş, etiket, düzenleyici, SearchBar, aralık, yazı tipi|FontAttributesBold, italik yok|FontStyleItalic, Normal|
|Düğme, giriş, etiket, düzenleyici, SearchBar, aralık, yazı tipi|FontAttributes|FontWeights *Bold, Normal|
|InputView|KeyboardDefault, Url, sayı, telefon, metin, sohbet, e-posta|InputScopeNameValue * varsayılan, Url, sayı, TelephoneNumber, metin, sohbet, EmailNameOrAddress|
|StackPanel|StackOrientation|Yönlendirme *|

> [!IMPORTANT]
> İşaretli öğeleri * geçerli önizlemede eksik

## <a name="related-links"></a>İlgili bağlantılar

- [Önizleme NuGet](https://aka.ms/xf-xamlstandard-nuget)
