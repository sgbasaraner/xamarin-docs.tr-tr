---
title: Açık stilleri
description: Açık bir stil denetimlerine stil özellikleri ayarlayarak seçmeli olarak uygulanan biridir.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 53f87fe9dfbf8284055d28fd87bab7bad02c1fd8
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="explicit-styles"></a>Açık stilleri

_Açık bir stil denetimlerine stil özellikleri ayarlayarak seçmeli olarak uygulanan biridir._

## <a name="creating-an-explicit-style-in-xaml"></a>XAML'de açık bir stil oluşturma

Bildirmek için bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) sayfa düzeyinde bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) sayfa ve ardından bir veya daha fazla bilgi için eklenmelidir `Style` bildirimleri dahil edilebilir `ResourceDictionary`. A `Style` yapılır *açık* bildiriminden vererek bir `x:Key` açıklayıcı bir anahtar sağlar özniteliği `ResourceDictionary`. *Açık* stilleri sonra uygulanmalıdır belirli görsel öğelere ayarlayarak kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özellikleri.

Aşağıdaki örnekte gösterildiği kod *açık* stilleri XAML'de bir sayfanın içinde bildirilen `ResourceDictionary` ve sayfanın uygulanan [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekleri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Üç tanımlar *açık* sayfanın uygulanmış olan stiller [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekleri. Her `Style` metin yazı tipi boyutu ve yatay ve dikey düzeni seçenekleri de ayarlanırken farklı bir renk görüntülemek için kullanılır. Her `Style` farklı bir öğeye uygulanan `Label` ayarlayarak kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) kullanarak özelliklerini `StaticResource` biçimlendirme uzantısı. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

[![](explicit-images/explicit-styles.png "Açık stilleri örnek")](explicit-images/explicit-styles-large.png#lightbox "açık stilleri örneği")

Buna ek olarak, en son [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) sahip bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) uygulanan, ancak ayrıca geçersiz kılar [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) farklı özelliğine`Color`değeri.

### <a name="creating-an-explicit-style-at-the-control-level"></a>Açık bir stil denetim düzeyi oluşturma

Oluşturma yanı sıra *açık* stilleri sayfa düzeyinde, bunlar da oluşturulabilir denetim düzeyinde aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Bu örnekte, *açık* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örnekleri atanır [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) koleksiyonu [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) denetim. Stilleri denetimi ve alt öğelerini uygulanabilir.

Uygulamanın içinde stilleri oluşturma hakkında bilgi için [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), bkz: [genel stiller](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-explicit-style-in-c35"></a>C'de açık bir stil oluşturma&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örnekleri bir sayfanın eklenebilir [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) yeni oluşturarak koleksiyonu C# [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)ve ardından ekleyerek `Style` için örnekler `ResourceDictionary`gösterildiği gibi Aşağıdaki kod örneği:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red  }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label { Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Üç Oluşturucusu tanımlar *açık* sayfanın uygulanmış olan stiller [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekleri. Her *açık* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) eklenen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) kullanarak [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) yöntemi, belirten bir `key` başvurmak için dize `Style` örneği. Her `Style` farklı bir öğeye uygulanan `Label` ayarlayarak kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özellikleri.

Bununla birlikte, kullanılması avantajlı yoktur bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) burada. Bunun yerine, [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örnekleri doğrudan atanabilir [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) gerekli görsel öğeler, özelliklerini ve `ResourceDictionary` aşağıda gösterildiği gibi kaldırılabilir kod örneği:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

Üç Oluşturucusu tanımlar *açık* sayfanın uygulanmış olan stiller [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekleri. Her `Style` metin yazı tipi boyutu ve yatay ve dikey düzeni seçenekleri de ayarlanırken farklı bir renk görüntülemek için kullanılır. Her `Style` farklı bir öğeye uygulanan `Label` ayarlayarak kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özellikleri. Buna ek olarak, en son `Label` sahip bir `Style` uygulanan, ancak ayrıca geçersiz kılar `TextColor` başka bir özellik `Color` değeri.

## <a name="summary"></a>Özet

A [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) yapılır *açık* bildiriminden vererek bir `x:Key` özniteliği ve ardından seçmeli olarak denetimlere ayarlayarak uygulamadan kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özellikleri.



## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Temel stilleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [stili](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Ayarlama](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
