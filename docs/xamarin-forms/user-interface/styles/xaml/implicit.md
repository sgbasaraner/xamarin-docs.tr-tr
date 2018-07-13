---
title: Xamarin.Forms içinde örtük stilleri
description: Örtülü bir stil stili başvurmak için her denetim gerektirmeden aynı TargetType tüm denetimler tarafından kullanılan biridir.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 277be51c242521f52e9b1e162226ae8137e7b133
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995520"
---
# <a name="implicit-styles-in-xamarinforms"></a>Xamarin.Forms içinde örtük stilleri

_Örtülü bir stil stili başvurmak için her denetim gerektirmeden aynı TargetType tüm denetimler tarafından kullanılan biridir._

## <a name="creating-an-implicit-style-in-xaml"></a>XAML içinde örtülü bir stil oluşturma

Bildirmek için bir [ `Style` ](xref:Xamarin.Forms.Style) sayfa düzeyinde bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) sayfası ve ardından bir veya daha fazla bilgi için eklenmelidir `Style` bildirimleri dahil edilebilir `ResourceDictionary`. A `Style` yapılan *örtük* değil belirterek bir `x:Key` özniteliği. Stil ardından eşleşen görsel öğelere uygulanacak `TargetType` tam olarak, ancak olmayan türetilmiş öğeler `TargetType` değeri.

Aşağıdaki kod örnekte gösterildiği bir *örtük* stili bir sayfanın içinde XAML içinde bildirilen `ResourceDictionary`ve sayfanın uygulanan [ `Entry` ](xref:Xamarin.Forms.Entry) örnekleri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Tek bir tanımlar *örtük* sayfanın uygulanan stil [ `Entry` ](xref:Xamarin.Forms.Entry) örnekleri. `Style` Diğer görünüm seçenekleri de ayarlanırken sarı bir arka plan üzerinde mavi metin görüntülemek için kullanılır. `Style` Sayfanın eklenen [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) belirtmeden bir `x:Key` özniteliği. Bu nedenle, `Style` tümüne uygulanan `Entry` eşleşecek şekilde örtük olarak örnekler [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) özelliği `Style` tam olarak. Ancak, `Style` uygulanmaz `CustomEntry` bir alt sınıflanan olan örneği `Entry`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

[![](implicit-images/implicit-styles.png "Örtük stiller örnek")](implicit-images/implicit-styles-large.png#lightbox "örtük stilleri örneği")

Ayrıca, dördüncü [ `Entry` ](xref:Xamarin.Forms.Entry) geçersiz kılmalar [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) ve [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) farklı örtükstilözellikleri`Color`değerleri.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Denetim düzeyi örtülü bir stil oluşturma

Oluşturmaya ek olarak *örtük* sayfa düzeyinde stilleri, bunlar da oluşturulabilir denetimi düzeyinde aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Bu örnekte, *örtük* [ `Style` ](xref:Xamarin.Forms.Style) atandığı [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) koleksiyonunu [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)denetimi. *Örtük* stili, ardından Denetim ve alt öğeleri için uygulanabilir.

Bir uygulamanın içinde stilleri oluşturma hakkında bilgi için [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), bkz: [genel stiller](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>C'de örtülü bir stil oluşturma&#35;

[`Style`](xref:Xamarin.Forms.Style) bir sayfanın örnekleri eklenebilir [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) oluşturarak yeni bir C# koleksiyonu [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ve ardından ekleyerek `Style` için örnekler `ResourceDictionary`gösterildiği Aşağıdaki kod örneği:

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

Tek bir oluşturucu tanımlar *örtük* sayfanın uygulanan stil [ `Entry` ](xref:Xamarin.Forms.Entry) örnekleri. `Style` Diğer görünüm seçenekleri de ayarlanırken sarı bir arka plan üzerinde mavi metin görüntülemek için kullanılır. `Style` Sayfanın eklenen [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) belirtmeden bir `key` dize. Bu nedenle, `Style` tümüne uygulanan `Entry` eşleşecek şekilde örtük olarak örnekler [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) özelliği `Style` tam olarak. Ancak, `Style` uygulanmaz `CustomEntry` bir alt sınıflanan olan örneği `Entry`.

## <a name="summary"></a>Özet

Bir *örtük* stili, aynı tüm görsel öğeleri tarafından kullanılan bir [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), stili başvurmak için her denetim gerektirmeden. A `Style` yapılan *örtük* değil belirterek bir `x:Key` özniteliği. Bunun yerine, `x:Key` öznitelik değerini otomatik olarak olacak [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) özelliği.



## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Basit stiller (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Ayarlayıcı](xref:Xamarin.Forms.Setter)
