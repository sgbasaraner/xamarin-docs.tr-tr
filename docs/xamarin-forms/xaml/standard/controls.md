---
title: XAML standart (Önizleme) denetimleri
description: Bu makalede Xamarin.Forms içinde kullanılabilir olan XAML standart denetimleri keşfediyor.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 1b01d0773f0c2150db575875b770957eb6452f41
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38843949"
---
# <a name="xaml-standard-preview-controls"></a>XAML standart (Önizleme) denetimleri

![Önizleme](~/media/shared/preview.png)

Bu sayfada, eşdeğer Xamarin.Forms Denetim yanı sıra Önizleme sürümünde kullanılabilir olan XAML standart denetimleri listelenir.

Yeni özellik ve sabit listesi adları XAML standart denetimleri listesi yoktur.

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

|Güncelleştirilmiş özellikleriyle Xamarin.Forms denetimleri|Xamarin.Forms özelliği veya sabit listesi|XAML standart eşdeğeri|
|--- |--- |--- |
|Düğme, giriş, etiket, DatePicker, düzenleyici, SearchBar, TimePicker|TextColor|Ön plan|
|VisualElement|BackgroundColor|Arka plan *|
|Seçici, düğme|BorderColor, OutlineColor|BorderBrush|
|Düğme|BorderWidth|BorderThickness|
|ProgressBar|İlerleme durumu|Değer|
|Düğme, giriş, etiket, düzenleyici, SearchBar, yayılma, yazı tipi|FontAttributesBold, italik, hiçbiri|FontStyleItalic, Normal|
|Düğme, giriş, etiket, düzenleyici, SearchBar, yayılma, yazı tipi|FontAttributes|FontWeights * kalın, Normal|
|InputView|KeyboardDefault, Url, sayı, telefon, metin, sohbet, e-posta|InputScopeNameValue * varsayılan, Url, sayı, TelephoneNumber, metin, sohbet, EmailNameOrAddress|
|StackPanel|StackOrientation|Yönlendirme *|

> [!IMPORTANT]
> Simgesiyle işaretli öğeler * geçerli önizlemede eksik

## <a name="related-links"></a>İlgili bağlantılar

- [Önizleme NuGet](https://aka.ms/xf-xamlstandard-nuget)
