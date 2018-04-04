---
title: Kaynak sözlükleri
description: XAML, birden çok kez kullanılabilir nesne tanımları kaynaklardır. ResourceDictionary tek bir konumda tanımlanmış ve bir Xamarin.Forms uygulaması yeniden kullanılabilir kaynaklar sağlar. Bu makalede nasıl oluşturulacağını ve ResourceDictionary tüketebilir ve kaynak sözlüklerindeki birleştirmek nasıl açıklanmaktadır.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/17/2017
ms.openlocfilehash: 37e38f2c4297d5acd148a7e6d1bec5569fe8c51c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="resource-dictionaries"></a>Kaynak sözlükleri

_XAML, birden çok kez kullanılabilir nesne tanımları kaynaklardır. ResourceDictionary tek bir konumda tanımlanmış ve bir Xamarin.Forms uygulaması yeniden kullanılabilir kaynaklar sağlar. Bu makalede nasıl oluşturulacağını ve ResourceDictionary tüketebilir ve kaynak sözlüklerindeki birleştirmek nasıl açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) bir Xamarin.Forms uygulaması tarafından kullanılan kaynakları için bir depodur. İçinde depolanan genel kaynakları bir `ResourceDictionary` dahil [stilleri](~/xamarin-forms/user-interface/styles/index.md), [denetim şablonları](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [veri şablonları](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), renklerini ve dönüştürücüleri.

XAML'de kaynakları tanımlanan bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) alınır ve kullanarak öğelerine uygulanan `StaticResource` biçimlendirme uzantısı. C# ' ta kaynakları içinde tanımlanan bir `ResourceDictionary` alınır ve dize tabanlı bir dizin oluşturucu kullanarak öğelerine uygulanır. Ancak, çok az kullanmanın avantajı yoktur bir `ResourceDictionary` C# ' ta kaynaklar olarak kolayca doğrudan atanabilir görsel öğe özelliklerini için önce bunları almaya gerek kalmadan bir `ResourceDictionary`.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Oluşturma ve ResourceDictionary Kullanma

Kaynakları tanımlanabilir bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) için bağlı [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) koleksiyonu sayfasının veya denetiminin ya da [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) koleksiyonu uygulamayı. Nerede tanımlamalı seçerek bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) nerede kullanılabilir etkiler:

- Kaynakları bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) tanımlanan denetim düzeyi yalnızca denetlemek ve alt öğelerini uygulanabilir.
- Kaynakları bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) tanımlanan sayfanın düzeyi yalnızca sayfa ve alt öğelerini uygulanabilir.
- Kaynakları bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) tanımlanan uygulamayı düzeyi uygulama genelinde uygulanabilir.

Aşağıdaki XAML kod örnek bir uygulama düzeyinde tanımlanan kaynaklara gösterir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Bu [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) üç tanımlar [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) kaynakları ve [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) kaynak. XAML oluşturma hakkında daha fazla bilgi için `App` sınıfı için bkz: [uygulama sınıfı](~/xamarin-forms/app-fundamentals/application-class.md).

Her kaynak kullanılarak belirtilen bir anahtara sahip `x:Key` açıklayıcı bir anahtar sağlar özniteliği `ResourceDictionary`. Anahtar bir kaynaktan almak için kullanılan [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) tarafından `StaticResource` denetim içinde tanımlanan ek kaynaklara düzeyi gösteren aşağıdaki XAML kod örneğinde gösterildiği gibi biçimlendirme uzantısı `ResourceDictionary`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

İlk [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örneği alır ve tüketir `LabelPageHeadingStyle` uygulama düzeyinde tanımlanan kaynak [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), ikinci ile `Label` örneği Alma ve tüketen `LabelNormalStyle` kaynağı denetim düzeyi tanımlı `ResourceDictionary`. Benzer şekilde, [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) örneği alır ve tüketir `NormalTextColor` uygulama düzeyinde tanımlanan kaynak `ResourceDictionary`ve `MediumBoldText` kaynağı denetim düzeyi tanımlı `ResourceDictionary`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

[![](resource-dictionaries-images/screenshots-sml.png "ResourceDictionary kaynakları tüketen")](resource-dictionaries-images/screenshots.png#lightbox "ResourceDictionary kaynakları kullanma")

> [!NOTE]
> Tek bir sayfaya belirli kaynaklar kaynakları sonra yerine uygulama başlangıcında bir sayfa tarafından istendiğinde ayrıştırılır bir uygulama düzeyi kaynak sözlük, bu nedenle dahil döndürmemelidir. Daha fazla bilgi için bkz: [uygulama kaynak sözlük boyutunu küçültmek](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Kaynakları geçersiz kılma

Zaman [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) kaynakları paylaşmak `x:Key` öznitelik değerleri daha düşük görünüm hiyerarşi içinde tanımlanan kaynaklara önceliklidir yukarı yüksek tanımlanan. Örneğin, ayarlama `PageBackgroundColor` kaynağa `Blue` uygulamayı düzeyi sayfa düzeyi tarafından geçersiz kılınacaktır `PageBackgroundColor` kaynak kümesine `Yellow`. Benzer şekilde, bir sayfa düzeyi `PageBackgroundColor` Kaynak Denetim düzeyine göre geçersiz `PageBackgroundColor` kaynak. Bu öncelik aşağıdaki XAML kod örneği tarafından gösterilmiştir:

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

Özgün `PageBackgroundColor` ve `NormalTextColor` örnekleri, uygulama düzeyinde tanımlanan tarafından geçersiz kılınır `PageBackgroundColor` ve `NormalTextColor` sayfa düzeyinde tanımlanan örnekleri. Bu nedenle, sayfa arka plan rengi mavi olur ve aşağıdaki ekran görüntülerinde gösterildiği gibi metin sayfasında sarı olur:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "ResourceDictionary kaynakları geçersiz kılma")](resource-dictionaries-images/overridding-screenshots.png#lightbox "ResourceDictionary kaynakları geçersiz kılma")

Ancak, dikkat edin arka plan çubuğunu [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) hala sarı çünkü [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) özelliği değerine ayarlanmış `PageBackgroundColor` kaynağı uygulamada tanımlı düzey [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).

## <a name="merged-resource-dictionaries"></a>Birleştirilmiş Kaynak Sözlükleri

Birleştirilmiş kaynak sözlüklerindeki birleştiren bir veya daha fazla [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) başka bir örneği. Bu ayar gerçekleştirilir `ResourceDictionary.MergedDictionaries` uygulama, sayfa ya da denetim düzeyi birleştirilmiş bir veya daha fazla kaynak sözlüklerindeki özelliğine `ResourceDictionary`.

> [!IMPORTANT]
> [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) De türüne sahip bir [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) tek bir birleştirme için kullanılan özellik `ResourceDictionary` başka bir alana sahip `ResourceDictionary` değeriolarakbelirtilen`MergedWith`geçerli birleştirilen özelliği `ResourceDictionary` örneği. Aracılığıyla birleştirilirken `MergedWith` özelliği, geçerli herhangi bir kaynağa `ResourceDictionary` paylaşan `x:Key` öznitelik değerlerini kaynaklarla `ResourceDictionary` birleştirilecek, değiştirilecek. Ancak, `MergedWith` özelliği Xamarin.Forms gelecekteki bir sürümde kullanım. Bu nedenle, kullanılacak önermiştir `MergedDictionaries` birleştirmek için özellik `ResourceDictionary` örnekleri.

Aşağıdaki XAML kodu örnekte gösterildiği bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) adlı `MyResourceDictionary` tek bir kaynak içerir:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

`MyResourceDictionary` herhangi bir uygulama, sayfa veya denetim düzeyi birleştirilmiş `ResourceDictionary`. Aşağıdaki XAML kod örneği, bir sayfa düzeye birleştirilen gösterir `ResourceDictionary` kullanarak `MergedDictionaries` özelliği:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Birleştirilmiş zaman [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) kaynakları paylaşmak aynı `x:Key` , aşağıdaki kaynak öncelik Xamarin.Forms öznitelik değerleri kullanır:

1. Kaynak sözlüğe yerel kaynaklar.
1. Aracılığıyla birleştirilmiş kaynak sözlüğü bulunan kaynaklar [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) özelliği.
1. Aracılığıyla birleştirilen kaynak sözlüklerindeki içerdiği kaynaklar `MergedDictionaries` koleksiyon, bunlar içinde listelenen sırayla `MergedDictionaries` özelliği.

> [!NOTE]
> Kaynak sözlüklerindeki arama olabilir bir pkı'ya yoğun görevi birden çok, bir uygulama içeriyorsa, büyük kaynak sözlük. Bu nedenle, her bir uygulama sayfasını yalnızca sayfaya gereksiz arama önlemek için uygun kaynak sözlüklerindeki kullandığından emin olun.

## <a name="summary"></a>Özet

Bu makalede açıklanan oluşturma ve kullanma hakkında bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)ve kaynak sözlüklerindeki birleştirme. A `ResourceDictionary` tek bir konumda tanımlanmış ve bir Xamarin.Forms uygulaması yeniden kullanılabilir kaynaklar sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Kaynak sözlüklerindeki (örnek)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
