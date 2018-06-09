---
title: Xamarin.Forms stili devralma
description: Stilleri çoğaltma azaltın ve yeniden etkinleştirmek için diğer stilleri devralabilirsiniz. Bu makalede, bir Xamarin.Forms uygulaması stili devralma gerçekleştirme açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: aff47769fad065e03de4c62af1be1d67b903eb0a
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245100"
---
# <a name="style-inheritance-in-xamarinforms"></a>Xamarin.Forms stili devralma

_Stilleri çoğaltma azaltın ve yeniden etkinleştirmek için diğer stilleri devralabilirsiniz._

## <a name="style-inheritance-in-xaml"></a>XAML'de stili devralma

Stil devralma ayarlayarak gerçekleştirilir [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) varolan bir özellik [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/). XAML'de ayarlayarak bu elde edilen `BasedOn` özelliğine bir `StaticResource` önceden oluşturulmuş başvuran biçimlendirme uzantısı `Style`. C# ' ta bu ayarlayarak sağlanır `BasedOn` özelliğine bir `Style` örneği.

Bir taban stil devral stilleri içerebilir [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) yeni özellikleri için örnek veya bunları temel stili stilleri geçersiz kılmak için kullanın. Ayrıca, bir taban stil devral stilleri aynı türde veya temel stili tarafından hedeflenen türü türeyen bir tür hedeflemesi gerekir. Örneğin, bir taban stil hedefleri, [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) temel stili temel alan stilleri örnekleri, hedef `View` örneği veya öğesinden türetilen türler `View` gibi sınıfı [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ve [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) örnekleri.

Aşağıdaki kodda *açık* stil devralma XAML sayfası içinde:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`baseStyle` Hedefleri [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) örnekleri ve ayarlar [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ve [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özellikleri. `baseStyle` Doğrudan herhangi denetimlere ayarlı değil. Bunun yerine, `labelStyle` ve `buttonStyle` ondan, ek bağlanabilirse özellik değerlerinin devralır. `labelStyle` Ve `buttonStyle` sonra uygulanan [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekleri ve [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ayarlayarak örneği, kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özellikleri. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Örtülü bir stil açık bir stili elde edilebilir, ancak bir açık stili örtülü bir stil türetilemez.

### <a name="respecting-the-inheritance-chain"></a>Devralma zincirini uyarak

Stil stilleri aynı düzeyde ya da yukarıdaki yalnızca devralınmalıdır görünüm hiyerarşideki. Bunun anlamı:

- Uygulama düzeyi kaynağı yalnızca diğer uygulama düzeyi kaynaklardan devralabilirsiniz.
- Sayfa düzeyi kaynak uygulama düzeyi kaynakları ve sayfa düzeyinde kaynaklar devralabilirsiniz.
- Bir denetim düzeyi kaynak uygulama düzeyi kaynakları, sayfa düzeyi kaynakları ve diğer denetim düzeyi kaynakları devralabilirsiniz.

Bu devralma zincirini aşağıdaki kod örneğinde gösterilmiştir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Bu örnekte, `labelStyle` ve `buttonStyle` denetim düzeyi, kaynaklardır sırada `baseStyle` sayfa düzeyi kaynaktır. Ancak, `labelStyle` ve `buttonStyle` devralınmalıdır `baseStyle`, mümkün değil `baseStyle` devralınacak `labelStyle` veya `buttonStyle`, ilgili konumlarına hiyerarşisini görüntüleme için son.

## <a name="style-inheritance-in-c35"></a>C stili devralma&#35;

Eşdeğer C# sayfaya, burada [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örnekleri doğrudan atanır [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) gerekli denetimlerin özelliklerini, aşağıdaki kod örneğinde gösterilir:

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center    },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal    }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value =    Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

`baseStyle` Hedefleri [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) örnekleri ve ayarlar [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ve [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özellikleri. `baseStyle` Doğrudan herhangi denetimlere ayarlı değil. Bunun yerine, `labelStyle` ve `buttonStyle` ondan, ek bağlanabilirse özellik değerlerinin devralır. `labelStyle` Ve `buttonStyle` sonra uygulanan [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örnekleri ve [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ayarlayarak örneği, kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özellikleri.

## <a name="summary"></a>Özet

Stilleri çoğaltma azaltın ve yeniden etkinleştirmek için diğer stilleri devralabilirsiniz. Stil devralma ayarlayarak gerçekleştirilir [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) varolan bir özellik [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/).


## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Temel stilleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [stili](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Ayarlama](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
