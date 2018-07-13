---
title: Xamarin.Forms stilleri giriş
description: Stilleri özelleştirilmiş görsel öğelerin görünümünü sağlar. Stilleri, belirli bir tür için tanımlanan ve o türde mevcut olan özelliklerin değerlerini içerir.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 8f84c960f17f56fce2a1bba143a215ce930f6f4e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996116"
---
# <a name="introduction-to-xamarinforms-styles"></a>Xamarin.Forms stilleri giriş

_Stilleri özelleştirilmiş görsel öğelerin görünümünü sağlar. Stilleri, belirli bir tür için tanımlanan ve o türde mevcut olan özelliklerin değerlerini içerir._

Xamarin.Forms uygulamaları genellikle özdeş bir görünüme sahip birden fazla denetim içerir. Örneğin, bir uygulamanın birden çok olabilir [ `Label` ](xref:Xamarin.Forms.Label) aşağıdaki XAML kod örneğinde gösterildiği gibi aynı yazı tipi seçenekleri ve düzen seçeneklerini sahip örnekleri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Aşağıdaki kod örneği, oluşturulan C# ' de eşdeğer sayfaya gösterir:

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

Her [ `Label` ](xref:Xamarin.Forms.Label) örneği tarafından görüntülenen metni görünümünü denetlemek için aynı özellik değerlerini sahip `Label`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

[![](introduction-images/no-styles.png "Etiket stilleri olmadan Görünüm")](introduction-images/no-styles-large.png#lightbox "etiket stilleri olmadan görünümü")

Tek tek her denetimin görünümünü ayarı yinelenen olabilir ve hataya açık alanlardır. Bunun yerine, bir stil görünümünü tanımlar ve sonra gerekli denetimlere uygulanan oluşturulabilir.

## <a name="creating-a-style"></a>Bir stil oluşturma

[ `Style` ](xref:Xamarin.Forms.Style) Sınıfı gruplarını koleksiyona, birden çok görsel öğe örnekleri için uygulanabilir bir nesnesi özellik değerleri. Bu durum, yinelenen biçimlendirme azaltmaya yardımcı olur ve daha kolay değiştirilecek bir uygulama görünüm sağlar.

Stilleri öncelikle XAML tabanlı uygulamalar için tasarlanmış olsa da, bunlar ayrıca C# ' ta oluşturulabilir:

- [`Style`](xref:Xamarin.Forms.Style) XAML içinde oluşturulan örnekleri tanımlanmış genellikle bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) için atanan [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) bir denetim koleksiyonunu sayfasında veya [ `Resources` ](xref:Xamarin.Forms.Application.Resources) uygulama koleksiyonu.
- [`Style`](xref:Xamarin.Forms.Style) C# ' ta oluşturulan örnekleri genellikle sayfanın sınıfı ya da genel olarak erişilebilen sınıfı olarak tanımlanır.

Tanımlama yeri seçme bir [ `Style` ](xref:Xamarin.Forms.Style) burada kullanılabileceği etkiler:

- [`Style`](xref:Xamarin.Forms.Style) Denetim düzeyinde tanımlanan örnekleri yalnızca denetime ve alt öğeleri için uygulanabilir.
- [`Style`](xref:Xamarin.Forms.Style) sayfa düzeyinde tanımlanan örnekleri yalnızca sayfanın ve alt öğeleri için uygulanabilir.
- [`Style`](xref:Xamarin.Forms.Style) uygulama düzeyinde tanımlanan örnekleri uygulama genelinde uygulanabilir.

Her [ `Style` ](xref:Xamarin.Forms.Style) örneği içeren bir koleksiyon bir veya daha fazla [ `Setter` ](xref:Xamarin.Forms.Setter) her ile nesne `Setter` sahip bir [ `Property` ](xref:Xamarin.Forms.Setter.Property) ve [`Value`](xref:Xamarin.Forms.Setter.Value). `Property` Bağlanılabilir özellik için uygulanan stil öğenin adıdır ve `Value` özelliğine uygulanan değerdir.

Her [ `Style` ](xref:Xamarin.Forms.Style) örneği olabilir *açık*, veya *örtük*:

- Bir *açık* [ `Style` ](xref:Xamarin.Forms.Style) örneği belirterek tanımlanmış bir [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) ve `x:Key` değeri ve hedef öğenin ayarlayarak[ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özelliğini `x:Key` başvuru. Hakkında daha fazla bilgi için *açık* stil, bakın [açık stiller](~/xamarin-forms/user-interface/styles/explicit.md).
- Bir *örtük* [ `Style` ](xref:Xamarin.Forms.Style) örneği yalnızca belirterek tanımlanmış bir [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType). `Style` Örneği sonra otomatik olarak uygulanacak bu türdeki tüm öğeler için. Not, alt sınıflara ayıran `TargetType` otomatik olarak sahip olmayan `Style` uygulanır. Hakkında daha fazla bilgi için *örtük* stil, bakın [örtük stilleri](~/xamarin-forms/user-interface/styles/implicit.md).

Oluştururken bir [ `Style` ](xref:Xamarin.Forms.Style), [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) özelliği gereklidir her zaman. Aşağıdaki kod örnekte gösterildiği bir *açık* stili (Not `x:Key`) XAML içinde oluşturulan:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Uygulanacak bir `Style`, hedef nesne olmalıdır bir [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) eşleşen [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) özelliği değerinin `Style`aşağıdaki XAML kod örneğinde gösterildiği gibi:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Görünüm hiyerarşide daha düşük stilleri ayarlama yüksek tanımlanmış önceliklidir. Örneğin, bir [ `Style` ](xref:Xamarin.Forms.Style) ayarlayan [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) için `Red` uygulamayı düzeyini ayarlayan bir sayfa düzeyi stil tarafından geçersiz kılınır `Label.TextColor` için`Green`. Benzer şekilde, sayfa düzeyi stili bir denetim düzeyi stili tarafından geçersiz kılınır. Ayrıca, varsa `Label.TextColor` doğrudan bir denetim özelliği, bu önceliklidir stilleri üzerinde ayarlanmış.

Bu bölümdeki makaleleri göstermek ve oluşturma ve uygulama açıklayan *açık* ve *örtük* stiller, genel stiller oluşturma, devralma, stil nasıl zamanında stili değişikliklerine yanıt verme ve Xamarin.Forms içinde bulunan yerleşik stilleri kullanma.

> [!NOTE]
> **Styleıd nedir?**
>
> Xamarin.Forms 2.2 önce [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) özelliği, bir uygulama kimliği, UI testi ve tema altyapısı Pixate gibi tek tek öğeleri tanımlamak için kullanıldı. Ancak, Xamarin.Forms 2.2 tanıttı [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) yerini aldı özelliği [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) özelliği. Daha fazla bilgi için [otomatikleştirmek Xamarin.Forms Xamarin.UITest ve Test Cloud ile test](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md).

## <a name="summary"></a>Özet

Xamarin.Forms uygulamaları genellikle özdeş bir görünüme sahip birden fazla denetim içerir. Tek tek her denetimin görünümünü ayarı yinelenen olabilir ve hataya açık alanlardır. Bunun yerine, stiller, denetimin görünümünü özelleştirme gruplandırma ve denetim türünde kullanılabilir ayarları özellikleri tarafından oluşturulabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Stil](xref:Xamarin.Forms.Style)
- [Ayarlayıcı](xref:Xamarin.Forms.Setter)
