---
title: Xamarin.Forms açık tema
description: Bu makalede, bir uygulamada Xamarin.Forms açık tema kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 7f40e375d653acec60f8848627234ab46fcce8de
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842758"
---
# <a name="xamarinforms-light-theme"></a>Xamarin.Forms açık tema

![](~/media/shared/preview.png "Bu API, şu anda Önizleme aşamasındadır")

> [!NOTE]
> Temalar Xamarin.Forms 2.3 önizleme sürümünü gerektirir. Denetleme [sorun giderme ipuçları](~/xamarin-forms/user-interface/themes/index.md) hatalar oluşursa.

Açık Tema kullanmak için:

## <a name="1-add-nuget-packages"></a>1. Nuget paketleri Ekle

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2. İçin kaynak sözlüğü Ekle

İçinde **App.xaml** dosyası özel bir ekleme `xmlns` temanın ve ardından temanın kaynakları uygulamanın kaynak sözlüğü ile birleştirilmiş emin olun.
Örnek bir XAML dosyası aşağıda gösterilmiştir:

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:light="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light">
    <Application.Resources>
        <ResourceDictionary MergedWith="light:LightThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Yük tema sınıfları

İzleyin [adım sorun giderme](~/xamarin-forms/user-interface/themes/index.md) ve iOS ve Android uygulaması projelerinde gerekli kodu ekleyin.

## <a name="4-use-styleclass"></a>4. StyleClass kullanın

Düğmeler ve etiketler bunları üreten işaretleme yanı sıra açık tema örneği aşağıda verilmiştir.

[![](light-images/light-theme-sml.png "Düğmeler ve etiketler açık tema")](light-images/light-theme.png#lightbox "düğmeler ve etiketler açık tema")

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
