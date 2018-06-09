---
title: XAML standart (Önizleme)
description: Bu makalede, Xamarin.Forms XAML standart önizlemede ile araştırmaya açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 61e0fa2587ce9a8794dbd32ff9de1f13da857342
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245800"
---
# <a name="xaml-standard-preview"></a>XAML standart (Önizleme)

![Önizleme](~/media/shared/preview.png)

XAML standart Xamarin.Forms, denemeniz için şu adımları izleyin:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Karşıdan [NuGet paketini buradan Önizleme](https://aka.ms/xf-xamlstandard-nuget).
2. Ekleme **Xamarin.Forms.Alias** Xamarin.Forms .NET standart ve platform projeleriniz için NuGet paketi.
3. Paketiyle başlatma `Alias.Init()`
4. Ekleme bir `xmlns:a` başvurusu `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML'de türlerini kullanın - bkz [denetimleri başvuru](controls.md) daha fazla bilgi için.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Karşıdan [NuGet paketini buradan Önizleme](https://aka.ms/xf-xamlstandard-nuget).
2. Ekleme **Xamarin.Forms.Alias** Xamarin.Forms .NET standart ve platform projeleriniz için NuGet paketi.
3. Paketiyle başlatma `Alias.Init()`
4. Ekleme bir `xmlns:a` başvurusu `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML'de türlerini kullanın - bkz [denetimleri başvuru](controls.md) daha fazla bilgi için.

-----

Aşağıdaki XAML bir Xamarin.Forms kullanılan bazı XAML standart denetimler gösteren `ContentPage`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage 
    xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"
    x:Class="XAMLStandardSample.ItemsPage" 
    Title="{Binding Title}" x:Name="BrowseItemsPage">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Clicked="AddItem_Clicked" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <a:StackPanel>
            <ListView x:Name="ItemsListView" ItemsSource="{Binding Items}" VerticalOptions="FillAndExpand" HasUnevenRows="true" RefreshCommand="{Binding LoadItemsCommand}" IsPullToRefreshEnabled="true" IsRefreshing="{Binding IsBusy, Mode=OneWay}" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout Padding="10">
                                <a:TextBlock Text="{Binding Text}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemTextStyle}" FontSize="16" />
                                <a:TextBlock Text="{Binding Description}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemDetailTextStyle}" FontSize="13" />
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </a:StackPanel>
    </ContentPage.Content>
</ContentPage>
```

> [!NOTE]
> Xmlns gerektiren `a:` önektir XAML standart denetimlerinde geçerli Önizleme sınırlandırılmasıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [Önizleme NuGet](https://aka.ms/xf-xamlstandard-nuget)
- [Denetimler Başvurusu](controls.md)
