---
title: XAML standart (Önizleme) denetimleri
description: Bu makalede Xamarin.Forms içinde kullanılabilir XAML standart denetimler araştırır.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 1b01d0773f0c2150db575875b770957eb6452f41
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245578"
---
# <a name="xaml-standard-preview-controls"></a>XAML standart (Önizleme) denetimleri

![Önizleme](~/media/shared/preview.png)

Bu sayfada, eşdeğer kendi Xamarin.Forms denetimi yanında Önizleme'de kullanılabilir XAML standart denetimler listelenir.

Ayrıca yeni özellik ve numaralandırma adlarını XAML standart denetimleri listesi verilmiştir.

## <a name="controls"></a>Denetimler

|Xamarin.Forms|XAML standart|
|--- |--- |
|Çerçeve|Kenarlık|
|Seçici|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|Etiketle|TextBlock|
|Giriş|TextBox|
|Anahtar|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Özellikleri ve numaralandırmaları

|Güncelleştirilmiş özellikleriyle Xamarin.Forms denetimleri|Xamarin.Forms özelliği ya da enum|XAML standart eşdeğeri|
|--- |--- |--- |
|Düğme, giriş, etiket, DatePicker, düzenleyici, SearchBar, TimePicker|textColor|Ön plan|
|VisualElement|BackgroundColor|Arka plan *|
|Seçici, düğmesi|BorderColor, OutlineColor|BorderBrush|
|Düğme|BorderWidth|BorderThickness|
|ProgressBar|İlerleme durumu|Değer|
|Düğme, giriş, etiket, düzenleyici, SearchBar, aralık, yazı tipi|FontAttributesBold, italik yok|FontStyleItalic, Normal|
|Düğme, giriş, etiket, düzenleyici, SearchBar, aralık, yazı tipi|FontAttributes|FontWeights * kalın, Normal|
|InputView|KeyboardDefault, Url, sayı, telefon, metin, sohbet, e-posta|InputScopeNameValue * varsayılan, Url, sayı, TelephoneNumber, metin, sohbet, EmailNameOrAddress|
|StackPanel|StackOrientation|Yönlendirme *|

> [!IMPORTANT]
> İşaretli öğeleri * geçerli önizlemede eksik

## <a name="related-links"></a>İlgili bağlantılar

- [Önizleme NuGet](https://aka.ms/xf-xamlstandard-nuget)
