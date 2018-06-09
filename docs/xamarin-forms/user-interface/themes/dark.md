---
title: Xamarin.Forms koyu tema
description: Bu makalede bir uygulamada Xamarin.Forms koyu tema kullanma açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 1fc329f506afde04b0dc59dc637d999865aafbe1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245930"
---
# <a name="xamarinforms-dark-theme"></a>Xamarin.Forms koyu tema

![](~/media/shared/preview.png "Bu API şu anda önizlemede değil")

> [!NOTE]
> Temalar Xamarin.Forms 2.3 Önizleme sürümü gerektirir. Denetleme [sorun giderme ipuçları](~/xamarin-forms/user-interface/themes/index.md) hatalar oluşursa.

Koyu Tema kullanmak için:

## <a name="1-add-nuget-packages"></a>1. Nuget paketleri ekleme

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2. Kaynak sözlük ekleme

İçinde **App.xaml** dosyası ekleme yeni bir özel `xmlns` tema ve ardından temanın kaynakları uygulamanın kaynak sözlüğü ile birleştirilmiş emin olun.
XAML dosyası örneği aşağıda verilmiştir:

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:dark="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Dark">
    <Application.Resources>
        <ResourceDictionary MergedWith="dark:DarkThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Yük tema sınıfları

Bu izleyin [adım sorun giderme](~/xamarin-forms/user-interface/themes/index.md) iOS ve Android uygulaması projeleri ve gerekli kodu ekleyin.

## <a name="4-use-styleclass"></a>4. StyleClass kullanın

Düğmeler ve etiketler bunları üreten biçimlendirme birlikte koyu tema bir örneği burada verilmiştir.

[![](dark-images/dark-theme-sml.png "Düğmeler ve etiketler koyu tema")](dark-images/dark-theme.png#lightbox "düğmeler ve etiketler koyu tema")

```xaml
<StackLayout Padding="20">
    <Button Text="Button Default" />
    <Button Text="Button Class Default" StyleClass="Default" />
    <Button Text="Button Class Primary" StyleClass="Primary" />
    <Button Text="Button Class Success" StyleClass="Success" />
    <Button Text="Button Class Info" StyleClass="Info" />
    <Button Text="Button Class Warning" StyleClass="Warning" />
    <Button Text="Button Class Danger" StyleClass="Danger" />
    <Button Text="Button Class Link" StyleClass="Link" />

    <Button Text="Button Class Default Small" StyleClass="Small" />
    <Button Text="Button Class Default Large" StyleClass="Large" />
</StackLayout>
```

[Yerleşik sınıfları tam listesi](~/xamarin-forms/user-interface/themes/index.md) hangi stilleri bazı ortak denetimleri için kullanılabilir olduğunu gösterir.
