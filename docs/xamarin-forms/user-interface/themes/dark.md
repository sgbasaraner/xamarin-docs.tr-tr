---
title: Xamarin.Forms koyu tema
description: Bu makalede, bir uygulamada Xamarin.Forms koyu tema kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 1fc329f506afde04b0dc59dc637d999865aafbe1
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38853251"
---
# <a name="xamarinforms-dark-theme"></a>Xamarin.Forms koyu tema

![](~/media/shared/preview.png "Bu API, şu anda Önizleme aşamasındadır")

> [!NOTE]
> Temalar Xamarin.Forms 2.3 önizleme sürümünü gerektirir. Denetleme [sorun giderme ipuçları](~/xamarin-forms/user-interface/themes/index.md) hatalar oluşursa.

Koyu Tema kullanmak için:

## <a name="1-add-nuget-packages"></a>1. Nuget paketleri Ekle

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2. İçin kaynak sözlüğü Ekle

İçinde **App.xaml** dosyası özel bir ekleme `xmlns` temanın ve ardından temanın kaynakları uygulamanın kaynak sözlüğü ile birleştirilmiş emin olun.
Örnek bir XAML dosyası aşağıda gösterilmiştir:

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

İzleyin [adım sorun giderme](~/xamarin-forms/user-interface/themes/index.md) ve iOS ve Android uygulaması projelerinde gerekli kodu ekleyin.

## <a name="4-use-styleclass"></a>4. StyleClass kullanın

Düğmeler ve etiketler, onları oluşturan işaretleme birlikte koyu tema örneği aşağıda verilmiştir.

[![](dark-images/dark-theme-sml.png "Düğmeler ve etiketler koyu temada")](dark-images/dark-theme.png#lightbox "düğmeler ve etiketler koyu tema")

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
