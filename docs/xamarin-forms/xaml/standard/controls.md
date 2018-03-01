---
title: "XAML standart (Önizleme) denetimleri"
description: "Xamarin.Forms XAML standart önizlemede araştırmaya nasıl"
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 3f30a77975a9f42380ecf7efd73426763ec83ef0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview-controls"></a>XAML standart (Önizleme) denetimleri

![Önizleme](~/media/shared/preview.png)

Bu sayfada, eşdeğer kendi Xamarin.Forms denetimi yanında Önizleme'de kullanılabilir XAML standart denetimler listelenir.

Ayrıca yeni özellik ve numaralandırma adlarını XAML standart denetimleri listesi verilmiştir.

## <a name="controls"></a>Denetimler

<table style="width:300px">
  <tr><th>Xamarin.Forms</th><th>XAML standart</th></tr>
  <tr><td>Çerçeve</td><td>Kenarlık</td></tr>
  <tr><td>Picker</td><td>ComboBox</td></tr>
  <tr><td>ActivityIndicator</td><td>ProgressRing</td></tr>
  <tr><td>StackLayout</td><td>StackPanel</td></tr>
  <tr><td>Etiketle</td><td>TextBlock</td></tr>
  <tr><td>Giriş</td><td>TextBox</td></tr>
  <tr><td>Anahtar</td><td>ToggleSwitch</td></tr>
  <tr><td>ContentView</td><td>UserControl</td></tr>
</table>

## <a name="properties-and-enumerations"></a>Özellikleri ve numaralandırmaları

<table>
  <tr><th>Xamarin.Forms<br/>Güncelleştirilmiş özellikleriyle denetimleri</th><th>Xamarin.Forms<br/>Özellik veya numaralandırma</th><th>XAML standart<br/>Eşdeğer</th></tr>
  <tr><td>Düğme, giriş, etiket, DatePicker, düzenleyici, SearchBar, TimePicker</td><td>TextColor</td><td>Ön plan</td></tr>
  <tr><td>VisualElement</td><td>BackgroundColor</td><td><i>Arka plan *</i></td></tr>
  <tr><td>Picker, Button</td><td>BorderColor, OutlineColor</td><td>BorderBrush</td></tr>
  <tr><td>Düğme</td><td>BorderWidth</td><td>BorderThickness</td></tr>
  <tr><td>ProgressBar</td><td>İlerleme durumu</td><td>Değer</td></tr>
  <tr><td>Düğme, giriş, etiket, düzenleyici, SearchBar, aralık, yazı tipi</td><td>FontAttributes<br/>Kalın, italik yok</td><td>FontStyle<br/>İtalik, Normal</td></tr>
  <tr><td>Düğme, giriş, etiket, düzenleyici, SearchBar, aralık, yazı tipi</td><td>FontAttributes</td><td><i>FontWeights *</i><br/>Kalın, Normal</td></tr>
  <tr><td>InputView</td><td>Klavye<br/>Varsayılan, Url, sayı, telefon, metin, sohbet, e-posta</td><td><i>InputScopeNameValue *</i><br/>Varsayılan, Url, sayı, TelephoneNumber, metin, sohbet, EmailNameOrAddress</td></tr>
  <tr><td>StackPanel</td><td>StackOrientation</td><td><i>Yönlendirme *</i></td></tr>
</table>

> [!IMPORTANT]
> İşaretli öğeleri * geçerli önizlemede eksik


## <a name="related-links"></a>İlgili bağlantılar

- [Önizleme NuGet](https://aka.ms/xf-xamlstandard-nuget)
