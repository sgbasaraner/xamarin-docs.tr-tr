---
title: Genel stiller
description: Stilleri genel uygulamanın kaynak sözlüğüne ekleyerek kullanılabilir hale getirilebilir. Bu sayfalar veya denetimleri boyunca stilleri yinelenmesini önlemek için yardımcı olur.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: a505720e5fef8fe9e9ef82d03e53370210772f45
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="global-styles"></a>Genel stiller

_Stilleri genel uygulamanın kaynak sözlüğüne ekleyerek kullanılabilir hale getirilebilir. Bu sayfalar veya denetimleri boyunca stilleri yinelenmesini önlemek için yardımcı olur._

## <a name="creating-a-global-style-in-xaml"></a>XAML'de genel bir stil oluşturma

Varsayılan olarak, bir şablondan oluşturulan tüm Xamarin.Forms uygulamaların kullanmak **uygulama** uygulamak için sınıf [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) bir alt kümesi. Bildirmek için bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) uygulama düzeyinde uygulamanın [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) XAML, varsayılan kullanılarak **uygulama** XAML ile sınıfı değiştirildi **Uygulama** sınıfı ve ilişkili arka plan kodu. Daha fazla bilgi için bkz: [uygulama sınıfıyla çalışan](~/xamarin-forms/app-fundamentals/application-class.md).

Aşağıdaki örnekte gösterildiği kod bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) uygulama düzeyinde bildirilen:

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

Bu [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) tek bir tanımlar *açık* stili `buttonStyle`, hangi görünümünü ayarlamak için kullanılacak [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) örnekleri. Ancak, genel stiller olabilir *açık* veya *örtük*.

Aşağıdaki kod örneğinde XAML uygulama sayfası gösterilir `buttonStyle` sayfanın için [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) örnekleri:

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

Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

[![](application-images/application-styles-1.png "Genel stiller örnek")](application-images/application-styles-1-large.png#lightbox "genel stiller örneği")

Bir sayfanın içinde stilleri oluşturma hakkında bilgi için [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), bkz: [açık stilleri](~/xamarin-forms/user-interface/styles/explicit.md) ve [örtülü stiller](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="overriding-styles"></a>Stilleri geçersiz kılma

Stilleri görünüm hiyerarşinin alt düzeylerindeki yukarı yüksek tanımlanan önceliklidir. Örneğin, bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) ayarlayan [ `Button.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/) için `Red` uygulamayı düzeyi ayarlar bir sayfa düzeyi stili tarafından geçersiz kılınacaktır `Button.TextColor` için`Green`. Benzer şekilde, sayfa düzeyi stili denetim düzeyi stili tarafından geçersiz kılınacaktır. Ayrıca, varsa `Button.TextColor` doğrudan bir denetim özelliği bu sürer öncelik herhangi stillerin ayarlanmadı. Bu öncelik aşağıdaki kod örneğinde gösterilmiştir:

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

Özgün `buttonStyle`, uygulama düzeyinde tanımlanan, tarafından geçersiz kılınır `buttonStyle` sayfa düzeyinde tanımlanan örneği. Ayrıca, sayfa düzeyi stili denetim düzeyi tarafından geçersiz kılınmış `buttonStyle`. Bu nedenle, [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) örnekleri aşağıdaki ekran görüntülerinde gösterildiği gibi mavi metinle görüntülenir:

[![](application-images/application-styles-2.png "Stilleri örnek geçersiz kılma")](application-images/application-styles-2-large.png#lightbox "stilleri örnek geçersiz kılma")

## <a name="creating-a-global-style-in-c35"></a>C'de genel bir stil oluşturma&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örnek uygulamanın eklenebilir [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) yeni oluşturarak koleksiyonu C# [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)ve ardından ekleyerek `Style` için örnekler `ResourceDictionary`, olarak Aşağıdaki kod örneğinde gösterildiği:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,   Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Tek bir oluşturucu tanımlar *açık* uygulamak için stil [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) örnek uygulama boyunca. *Açık* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örnekleri eklenir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) kullanarak [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) yöntemi, bir belirtme`key`başvurmak için dize `Style` örneği. `Style` Örneği ardından uygulanabilir uygulamadaki doğru türde herhangi bir denetim. Ancak, genel stiller olabilir *açık* veya *örtük*.

Aşağıdaki kod örneği, bir C# sayfa uygulama gösterir `buttonStyle` sayfanın için [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) örnekleri:

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

`buttonStyle` Uygulanan [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ayarlayarak örnekleri kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özelliklerini ve görünümünü denetler `Button` örnekleri.

## <a name="summary"></a>Özet

Stilleri kullanılabilir hale getirebilir genel olarak uygulamanın ekleyerek [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Bu sayfalar veya denetimleri boyunca stilleri yinelenmesini önlemek için yardımcı olur.



## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Temel stilleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [stili](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Ayarlama](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
