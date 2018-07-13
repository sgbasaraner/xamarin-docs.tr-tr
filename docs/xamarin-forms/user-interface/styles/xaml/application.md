---
title: Xamarin.Forms içinde genel stiller
description: Stilleri genel olarak uygulamanın kaynak sözlüğüne ekleyerek kullanılabilir hale getirilebilir. Bu çoğaltma stil sayfaları veya denetimler arasında kaçınmaya yardımcı olur.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: e7b2a37b868ea03ca626ffd2dcddb006a235b0cc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995407"
---
# <a name="global-styles-in-xamarinforms"></a>Xamarin.Forms içinde genel stiller

_Stilleri genel olarak uygulamanın kaynak sözlüğüne ekleyerek kullanılabilir hale getirilebilir. Bu çoğaltma stil sayfaları veya denetimler arasında kaçınmaya yardımcı olur._

## <a name="creating-a-global-style-in-xaml"></a>XAML içinde genel bir stil oluşturma

Varsayılan olarak, bir şablondan oluşturulan tüm Xamarin.Forms uygulamaları kullanın **uygulama** uygulamak için sınıfı [ `Application` ](xref:Xamarin.Forms.Application) öğesinin alt sınıfı. Bildirmek için bir [ `Style` ](xref:Xamarin.Forms.Style) içinde uygulamanın uygulama düzeyinde [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) kullanarak XAML, varsayılan **uygulama** ile bir XAML sınıf değiştirildi **Uygulama** sınıfı ve ilişkili arka plan kod. Daha fazla bilgi için [uygulama sınıfıyla çalışan](~/xamarin-forms/app-fundamentals/application-class.md).

Aşağıdaki kod örnekte gösterildiği bir [ `Style` ](xref:Xamarin.Forms.Style) uygulama düzeyinde bildirilen:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Bu [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) tek bir tanımlar *açık* stili `buttonStyle`, hangi görünümünü ayarlamak için kullanılacak [ `Button` ](xref:Xamarin.Forms.Button) örnekleri. Ancak, genel stiller olabilir *açık* veya *örtük*.

Aşağıdaki kod örneği bir XAML sayfası uygulama gösterir `buttonStyle` sayfanın için [ `Button` ](xref:Xamarin.Forms.Button) örnekleri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

[![](application-images/application-styles-1.png "Genel stiller örnek")](application-images/application-styles-1-large.png#lightbox "genel stiller örneği")

Stilleri bir sayfanın içinde oluşturma hakkında bilgi için [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), bkz: [açık stiller](~/xamarin-forms/user-interface/styles/explicit.md) ve [örtük stilleri](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="overriding-styles"></a>Stilleri geçersiz kılma

Görünüm hiyerarşide daha düşük stilleri ayarlama yüksek tanımlanmış önceliklidir. Örneğin, bir [ `Style` ](xref:Xamarin.Forms.Style) ayarlayan [ `Button.TextColor` ](xref:Xamarin.Forms.Button.TextColor) için `Red` uygulamayı düzeyini ayarlayan bir sayfa düzeyi stil tarafından geçersiz kılınır `Button.TextColor` için`Green`. Benzer şekilde, sayfa düzeyi stili bir denetim düzeyi stili tarafından geçersiz kılınır. Ayrıca, varsa `Button.TextColor` doğrudan bir denetim özelliği, bu götürür öncelik stilleri üzerinde ayarlanmış. Bu öncelik aşağıdaki kod örneğinde gösterilmiştir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Özgün `buttonStyle`, uygulama düzeyinde tanımlanan, tarafından geçersiz kılınır `buttonStyle` sayfa düzeyinde tanımlanan örneği. Ayrıca, sayfa düzeyi stili denetim düzeyi tarafından geçersiz kılındı `buttonStyle`. Bu nedenle, [ `Button` ](xref:Xamarin.Forms.Button) örnekleri aşağıdaki ekran görüntülerinde gösterildiği mavi metinle görüntülenir:

[![](application-images/application-styles-2.png "Stilleri örnek geçersiz kılma")](application-images/application-styles-2-large.png#lightbox "stilleri örnek geçersiz kılma")

## <a name="creating-a-global-style-in-c35"></a>C'de genel bir stil oluşturma&#35;

[`Style`](xref:Xamarin.Forms.Style) uygulamanın örnekleri eklenebilir [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) oluşturarak yeni bir C# koleksiyonu [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ve ardından ekleyerek `Style` için örnekler `ResourceDictionary`, olarak Aşağıdaki kod örneğinde gösterilen:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Tek bir oluşturucu tanımlar *açık* stil uygulamak için [ `Button` ](xref:Xamarin.Forms.Button) örnek uygulama boyunca. *Açık* [ `Style` ](xref:Xamarin.Forms.Style) örnekleri eklenir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) kullanarak [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) yöntemi, bir belirtme`key`başvurmak için dize `Style` örneği. `Style` Örneği ardından uygulanabilir uygulamadaki doğru türde herhangi bir denetim. Ancak, genel stiller olabilir *açık* veya *örtük*.

Aşağıdaki kod örneği, bir C# sayfa uygulama gösterir `buttonStyle` sayfanın için [ `Button` ](xref:Xamarin.Forms.Button) örnekleri:

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

`buttonStyle` Uygulanan [ `Button` ](xref:Xamarin.Forms.Button) ayarlayarak örnekleri kendi [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özellikleri ve görünümünü denetler `Button` örnekleri.

## <a name="summary"></a>Özet

Stilleri kullanılabilir hale genel olarak uygulamanın ekleyerek [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Bu çoğaltma stil sayfaları veya denetimler arasında kaçınmaya yardımcı olur.



## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Basit stiller (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Ayarlayıcı](xref:Xamarin.Forms.Setter)
