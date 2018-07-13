---
title: Xamarin.Forms içinde açık stiller
description: Açık bir stil denetimleri için stil özelliklerini ayarlayarak seçmeli olarak uygulanan biridir. Bu makalede, bir Xamarin.Forms uygulamasında açık stiller kullanma açıklanmaktadır.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: fba00120ed9f5c74bec7622ae1914c43533e8579
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998576"
---
# <a name="explicit-styles-in-xamarinforms"></a>Xamarin.Forms içinde açık stiller

_Açık bir stil denetimleri için stil özelliklerini ayarlayarak seçmeli olarak uygulanan biridir._

## <a name="creating-an-explicit-style-in-xaml"></a>XAML içinde açık bir stil oluşturma

Bildirmek için bir [ `Style` ](xref:Xamarin.Forms.Style) sayfa düzeyinde bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) sayfası ve ardından bir veya daha fazla bilgi için eklenmelidir `Style` bildirimleri dahil edilebilir `ResourceDictionary`. A `Style` yapılan *açık* bildiriminden vererek bir `x:Key` tanımlayıcı bir anahtar veren özniteliği `ResourceDictionary`. *Açık* stilleri sonra uygulanmalıdır için özel görsel öğeleri ayarlayarak kendi [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özellikleri.

Aşağıdaki örnekte gösterildiği kod *açık* stilleri bir sayfanın içinde XAML içinde bildirilen `ResourceDictionary` ve sayfanın uygulanan [ `Label` ](xref:Xamarin.Forms.Label) örnekleri:

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

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Üç tanımlar *açık* sayfanın uygulanan stiller [ `Label` ](xref:Xamarin.Forms.Label) örnekleri. Her `Style` metni yazı tipi boyutu ve yatay ve dikey düzen seçeneklerini de ayarlanırken farklı bir renkte göstermek için kullanılır. Her `Style` farklı bir uygulanan `Label` ayarlayarak onun [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) kullanarak özelliklerini `StaticResource` işaretleme uzantısı. Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

[![](explicit-images/explicit-styles.png "Açık stiller örnek")](explicit-images/explicit-styles-large.png#lightbox "açık stiller örneği")

Ayrıca, en son [ `Label` ](xref:Xamarin.Forms.Label) sahip bir [ `Style` ](xref:Xamarin.Forms.Style) uygulanmış, ancak ayrıca geçersiz kılar [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) farklı özelliğini`Color`değeri.

### <a name="creating-an-explicit-style-at-the-control-level"></a>Denetim düzeyi açık bir stil oluşturma

Oluşturmaya ek olarak *açık* sayfa düzeyinde stilleri, bunlar da oluşturulabilir denetimi düzeyinde aşağıdaki kod örneğinde gösterildiği gibi:

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

Bu örnekte, *açık* [ `Style` ](xref:Xamarin.Forms.Style) örnekleri atanmış [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) koleksiyonunu [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) denetimi. Stiller denetim ve alt öğeleri için uygulanabilir.

Bir uygulamanın içinde stilleri oluşturma hakkında bilgi için [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), bkz: [genel stiller](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-explicit-style-in-c35"></a>C'de açık bir stil oluşturma&#35;

[`Style`](xref:Xamarin.Forms.Style) bir sayfanın örnekleri eklenebilir [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) oluşturarak yeni bir C# koleksiyonu [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ve ardından ekleyerek `Style` için örnekler `ResourceDictionary`gösterildiği Aşağıdaki kod örneği:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
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
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Üç Oluşturucu tanımlar *açık* sayfanın uygulanan stiller [ `Label` ](xref:Xamarin.Forms.Label) örnekleri. Her *açık* [ `Style` ](xref:Xamarin.Forms.Style) eklenir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) kullanarak [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) yöntemini belirten bir `key` başvurmak için dize `Style` örneği. Her `Style` farklı bir uygulanan `Label` ayarlayarak kendi [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özellikleri.

Ancak, kullanarak herhangi bir avantaj sağlamaz yoktur bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) burada. Bunun yerine, [ `Style` ](xref:Xamarin.Forms.Style) örnekleri doğrudan atanabilir [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) gerekli görsel öğelerin özelliklerini ve `ResourceDictionary` aşağıda gösterildiği şekilde Kaldırılabilir kod örneği:

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

Üç Oluşturucu tanımlar *açık* sayfanın uygulanan stiller [ `Label` ](xref:Xamarin.Forms.Label) örnekleri. Her `Style` metni yazı tipi boyutu ve yatay ve dikey düzen seçeneklerini de ayarlanırken farklı bir renkte göstermek için kullanılır. Her `Style` farklı bir uygulanan `Label` ayarlayarak onun [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özellikleri. Ayrıca, en son `Label` sahip bir `Style` uygulanmış, ancak ayrıca geçersiz kılar `TextColor` başka bir özellik `Color` değeri.

## <a name="summary"></a>Özet

A [ `Style` ](xref:Xamarin.Forms.Style) yapılan *açık* bildiriminden vererek bir `x:Key` özniteliği ve ardından seçmeli olarak denetimlere ayarlayarak uygulamadan kendi [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özellikleri.



## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Basit stiller (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Ayarlayıcı](xref:Xamarin.Forms.Setter)
