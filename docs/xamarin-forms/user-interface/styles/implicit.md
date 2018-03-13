---
title: "Örtük stilleri"
description: "Örtülü bir stil stili başvurmak için her denetim gerektirmeden aynı TargetType tüm denetimleri tarafından kullanılan biridir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: b96b306c882eb30aaf8c81604afb9b6a547d715b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="implicit-styles"></a>Örtük stilleri

_Örtülü bir stil stili başvurmak için her denetim gerektirmeden aynı TargetType tüm denetimleri tarafından kullanılan biridir._

## <a name="creating-an-implicit-style-in-xaml"></a>XAML'de örtülü bir stil oluşturma

Bildirmek için bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) sayfa düzeyinde bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) sayfa ve ardından bir veya daha fazla bilgi için eklenmelidir `Style` bildirimleri dahil edilebilir `ResourceDictionary`. A `Style` yapılır *örtük* değil belirterek bir `x:Key` özniteliği. Stil sonra eşleşen görsel öğelere uygulanır `TargetType` tam olarak, ancak türetilmiş öğelere `TargetType` değeri.

Aşağıdaki örnekte gösterildiği kod bir *örtük* stili XAML'de bir sayfanın içinde bildirilen `ResourceDictionary`ve sayfanın uygulanan [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) örnekleri:

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

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Tek bir tanımlar *örtük* sayfanın uygulanan stil [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) örnekleri. `Style` Diğer görünüm seçenekleri de ayarlanırken sarı bir arka plan üzerinde mavi metne görüntülemek için kullanılır. `Style` Sayfanın eklenen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) belirtmeden bir `x:Key` özniteliği. Bu nedenle, `Style` tümüne uygulanır `Entry` bunların eşleşmesi gibi örtük olarak örnekleri [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) özelliği `Style` tam olarak. Ancak, `Style` uygulanmaz `CustomEntry` bir altsınıflanmış olan örneği `Entry`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

[![](implicit-images/implicit-styles.png "Örtülü stiller örnek")](implicit-images/implicit-styles-large.png#lightbox "örtülü stiller örneği")

Ayrıca, dördüncü [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) geçersiz kılmaları [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) ve [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) farklı örtükstilözelliklerini`Color`değerleri.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Denetim düzeyi örtülü bir stil oluşturma

Oluşturma yanı sıra *örtük* stilleri sayfa düzeyinde, bunlar da oluşturulabilir denetim düzeyinde aşağıdaki kod örneğinde gösterildiği gibi:

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

Bu örnekte, *örtük* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) atandığı [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) koleksiyonu [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)denetim. *Örtük* stili, ardından Denetim ve alt öğelerini uygulanabilir.

Uygulamanın içinde stilleri oluşturma hakkında bilgi için [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), bkz: [genel stiller](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>C'de örtülü bir stil oluşturma&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örnekleri bir sayfanın eklenebilir [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) yeni oluşturarak koleksiyonu C# [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)ve ardından ekleyerek `Style` için örnekler `ResourceDictionary`gösterildiği gibi Aşağıdaki kod örneği:

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

Tek bir oluşturucu tanımlar *örtük* sayfanın uygulanan stil [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) örnekleri. `Style` Diğer görünüm seçenekleri de ayarlanırken sarı bir arka plan üzerinde mavi metne görüntülemek için kullanılır. `Style` Sayfanın eklenen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) belirtmeden bir `key` dize. Bu nedenle, `Style` tümüne uygulanır `Entry` bunların eşleşmesi gibi örtük olarak örnekleri [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) özelliği `Style` tam olarak. Ancak, `Style` uygulanmaz `CustomEntry` bir altsınıflanmış olan örneği `Entry`.

## <a name="summary"></a>Özet

Bir *örtük* stili aynı tüm görsel öğeleri tarafından kullanılan bir olduğundan [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), stil başvurmak için her denetim gerektirmeden. A `Style` yapılır *örtük* değil belirterek bir `x:Key` özniteliği. Bunun yerine, `x:Key` öznitelik değeri otomatik olarak olacak [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) özelliği.



## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Temel stilleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Stilleri (örnek) ile çalışma](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [stili](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
