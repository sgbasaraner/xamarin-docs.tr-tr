---
title: "Açık tema"
ms.topic: article
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 9918c7e54f443477006ad40931c24d814f7171ea
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="light-theme"></a>Açık tema

![](~/media/shared/preview.png "Bu API şu anda önizlemede değil")

> [!NOTE]
> Temalar Xamarin.Forms 2.3 Önizleme sürümü gerektirir. Denetleme [sorun giderme ipuçları](~/xamarin-forms/user-interface/themes/index.md) hatalar oluşursa.

Açık Tema kullanmak için:

## <a name="1-add-nuget-packages"></a>1. Nuget paketleri ekleme

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2. Kaynak sözlük ekleme

İçinde **App.xaml** dosyası ekleme yeni bir özel `xmlns` tema ve ardından temanın kaynakları uygulamanın kaynak sözlüğü ile birleştirilmiş emin olun.
XAML dosyası örneği aşağıda verilmiştir:

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

Bu izleyin [adım sorun giderme](~/xamarin-forms/user-interface/themes/index.md) iOS ve Android uygulaması projeleri ve gerekli kodu ekleyin.

## <a name="4-use-styleclass"></a>4. StyleClass kullanın

Düğmeler ve etiketler bunları üreten biçimlendirme birlikte açık tema içinde bir örneği burada verilmiştir.

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

