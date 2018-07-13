---
title: Xamarin.Forms içinde stil devralımı
description: Stilleri azaltmak ve yeniden etkinleştirmek için diğer stilleri devralabilir. Bu makalede, bir Xamarin.Forms uygulamasında stil devralımı gerçekleştirmek açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f8cf3287c6d713d91a0217bd30ca2ee927534aea
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995339"
---
# <a name="style-inheritance-in-xamarinforms"></a>Xamarin.Forms içinde stil devralımı

_Stilleri azaltmak ve yeniden etkinleştirmek için diğer stilleri devralabilir._

## <a name="style-inheritance-in-xaml"></a>XAML içinde stil devralımı

Stil devralımı ayarlayarak gerçekleştirilir [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) mevcut bir özelliği [ `Style` ](xref:Xamarin.Forms.Style). XAML içinde bu ayarlayarak sağlanır `BasedOn` özelliğini bir `StaticResource` önceden oluşturulmuş başvuran işaretleme uzantısı `Style`. C# ' ta bu ayarlayarak sağlanır `BasedOn` özelliğini bir `Style` örneği.

Temel bir stil devralan stilleri içerebilir [ `Setter` ](xref:Xamarin.Forms.Setter) örnekleri için yeni özellikler veya bunları stilleri temel stili geçersiz kılmak için kullanın. Ayrıca, temel bir stil devralan stilleri, aynı türde veya temel stili tarafından hedeflenen türünden türetilen bir türü hedeflemesi gerekir. Örneğin, bir temel stili hedef alıyorsa [ `View` ](xref:Xamarin.Forms.View) örnekleri temel stili tabanlı stilleri hedefleyebilir `View` örnekleri veya öğesinden türetilen türler `View` gibi sınıf [ `Label` ](xref:Xamarin.Forms.Label) ve [ `Button` ](xref:Xamarin.Forms.Button) örnekleri.

Aşağıdaki kodda *açık* devralma de bir XAML sayfası stili:

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

`baseStyle` Hedefleri [ `View` ](xref:Xamarin.Forms.View) örnekler ve ayarlar [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) ve [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özellikleri. `baseStyle` Doğrudan herhangi bir denetim üzerinde ayarlı değil. Bunun yerine, `labelStyle` ve `buttonStyle` kendisinden ek bağlanılabilir özellik değerlerini ayarlama devralır. `labelStyle` Ve `buttonStyle` sonra uygulanır [ `Label` ](xref:Xamarin.Forms.Label) örnekleri ve [ `Button` ](xref:Xamarin.Forms.Button) ayarlayarak örneği, kendi [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özellikleri. Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Örtülü bir stil açık bir stili elde edilebilir, ancak açık bir style'nden bir örtük stil türetilemez.

### <a name="respecting-the-inheritance-chain"></a>Devralma zincirini uyarak

Bir stil yalnızca aynı düzeyde veya üzerinde stilleri öğesinden devralabilir görünümü hiyerarşideki. Bunun anlamı:

- Uygulama düzeyi kaynağı, yalnızca diğer uygulama düzeyi kaynaklardan devralabilir.
- Sayfa düzeyi kaynak uygulama düzeyinde kaynakları ve diğer sayfa düzeyi kaynakları devralabilir.
- Bir denetim düzeyi kaynak uygulama düzeyinde kaynakları, sayfa düzeyi kaynakları ve diğer denetim düzeyi kaynaklarından devralabilir.

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

Bu örnekte, `labelStyle` ve `buttonStyle` denetim düzeyi kaynaklar durumda sırada `baseStyle` sayfa düzeyi kaynaktır. Ancak, `labelStyle` ve `buttonStyle` devralınacak `baseStyle`, mümkün değil `baseStyle` devralınacak `labelStyle` veya `buttonStyle`nedeniyle ilgili konumlarına hiyerarşisini görüntüle.

## <a name="style-inheritance-in-c35"></a>C stili devralma&#35;

Eşdeğer C# sayfaya, burada [ `Style` ](xref:Xamarin.Forms.Style) örnekleri doğrudan atanmış [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) gerekli denetimlerin özelliklerini, aşağıdaki kod örneğinde gösterilmiştir:

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

`baseStyle` Hedefleri [ `View` ](xref:Xamarin.Forms.View) örnekler ve ayarlar [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) ve [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özellikleri. `baseStyle` Doğrudan herhangi bir denetim üzerinde ayarlı değil. Bunun yerine, `labelStyle` ve `buttonStyle` kendisinden ek bağlanılabilir özellik değerlerini ayarlama devralır. `labelStyle` Ve `buttonStyle` sonra uygulanır [ `Label` ](xref:Xamarin.Forms.Label) örnekleri ve [ `Button` ](xref:Xamarin.Forms.Button) ayarlayarak örneği, kendi [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özellikleri.

## <a name="summary"></a>Özet

Stilleri azaltmak ve yeniden etkinleştirmek için diğer stilleri devralabilir. Stil devralımı ayarlayarak gerçekleştirilir [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) mevcut bir özelliği [ `Style` ](xref:Xamarin.Forms.Style).


## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Basit stiller (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Ayarlayıcı](xref:Xamarin.Forms.Setter)
