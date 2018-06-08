---
title: Stilleri giriş
description: Stilleri özelleştirilmek üzere görsel öğelerin görünümünü sağlar. Stilleri belirli bir türü için tanımlı ve o türde kullanılabilir özelliklerinin değerlerini içerir.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: f878641f9266da74d61eda8a85d4e16de193be0e
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847972"
---
# <a name="introduction-to-styles"></a>Stilleri giriş

_Stilleri özelleştirilmek üzere görsel öğelerin görünümünü sağlar. Stilleri belirli bir türü için tanımlı ve o türde kullanılabilir özelliklerinin değerlerini içerir._

Xamarin.Forms uygulamalar genellikle özdeş bir görünüme sahip birden çok denetim içerir. Örneğin, bir uygulama birden çok olabilir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) aşağıdaki XAML kod örneğinde gösterildiği gibi aynı yazı tipi seçenekleri ve Düzen Seçenekleri sahip örnekleri:

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

Aşağıdaki kod örneği, C# ' de oluşturulan eşdeğer sayfaya gösterir:

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

Her [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) örneği tarafından görüntülenen metni görünümünü denetlemek için aynı özellik değerlerini sahip `Label`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

[![](introduction-images/no-styles.png "Etiket görünüm stilleri olmadan")](introduction-images/no-styles-large.png#lightbox "etiket görünüm stilleri olmadan")

Her denetim görünümünü ayarlama yinelenen olabilir ve hataya. Bunun yerine, bir stil görünümünü tanımlayan ve gerekli denetimleri uygulanan oluşturulabilir.

## <a name="creating-a-style"></a>Stil oluşturma

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Sınıfı grupları bir koleksiyona özellik değerlerinin birden çok görsel öğe örneklerine uygulanabilir bir nesne. Bu tekrarlayan biçimlendirme azaltılmasına yardımcı olur ve daha kolay değiştirilecek bir uygulama görünümünü sağlar.

Stilleri öncelikle XAML tabanlı uygulamalar için tasarlanmış olsa da, bunlar ayrıca C# ' ta oluşturulabilir:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) XAML'de oluşturulan örnekleri tanımlanmış genellikle bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) için atanan [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) bir denetim koleksiyonu sayfasında veya [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) uygulama koleksiyonu.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) C# ' de oluşturulan örnek, genellikle sayfanın sınıfı veya genel olarak erişilebilir bir sınıf tanımlanır.

Nerede tanımlamalı seçerek bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) nerede kullanılabilir etkiler:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Denetim ve alt denetim düzeyinde tanımlanan örnekleri yalnızca uygulanabilir.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) sayfa düzeyinde tanımlanan örnekleri yalnızca sayfa ve alt öğelerini uygulanabilir.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) uygulama düzeyinde tanımlanan örnekleri uygulama genelinde uygulanabilir.

Her [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örneğini içeren bir veya daha fazla koleksiyonu [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) her nesneleri `Setter` sahip bir [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) ve [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/). `Property` İçin uygulanan stil öğesi bağlanabilirse özelliği adıdır ve `Value` özelliğine uygulanan değerdir.

Her [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örneği olabilir *açık*, veya *örtük*:

- Bir *açık* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örneği belirterek tanımlanmış bir [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) ve bir `x:Key` değeri ve hedef öğenin 'olarakayarlama[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özelliğine `x:Key` başvuru. Hakkında daha fazla bilgi için *açık* stilleri bkz [açık stilleri](~/xamarin-forms/user-interface/styles/explicit.md).
- Bir *örtük* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) örneği yalnızca belirterek tanımlanmış bir [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/). `Style` Örneği sonra otomatik olarak uygulanacak bu türdeki tüm öğeleri. Not o alt sınıflarının `TargetType` otomatik olarak olmadığı `Style` uygulanır. Hakkında daha fazla bilgi için *örtük* stilleri bkz [örtülü stiller](~/xamarin-forms/user-interface/styles/implicit.md).

Oluştururken bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/), [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) özelliği gereklidir her zaman. Aşağıdaki örnekte gösterildiği kod bir *açık* stili (Not `x:Key`) XAML'de oluşturuldu:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Uygulanacak bir `Style`, hedef nesne olmalıdır bir [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) eşleşen [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) özellik değerinin `Style`aşağıdaki XAML kod örneğinde gösterildiği gibi:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Stilleri görünüm hiyerarşinin alt düzeylerindeki yukarı yüksek tanımlanan önceliklidir. Örneğin, bir [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) ayarlayan [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) için `Red` uygulamayı düzeyi ayarlar bir sayfa düzeyi stili tarafından geçersiz kılınacaktır `Label.TextColor` için`Green`. Benzer şekilde, sayfa düzeyi stili denetim düzeyi stili tarafından geçersiz kılınacaktır. Ayrıca, varsa `Label.TextColor` doğrudan bir denetim özelliği bu önceliklidir herhangi stillerin ayarlanmadı.

Bu bölümdeki makaleleri göstermek ve oluşturmak ve uygulamak açıklanmaktadır *açık* ve *örtük* stiller, genel stiller oluşturma, devralma, stil nasıl stil değişiklikleri çalışma zamanında yanıt ve nasıl Xamarin.Forms dahil-yerleşik stilleri kullanın.

> [!NOTE]
> **StyleId nedir?**
>
> Xamarin.Forms 2.2 önce [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) UI testi ve Pixate gibi tema altyapılarındaki tanımlaması için bir uygulamada tek tek öğeleri tanımlamak için kullanılan özellik. Ancak, Xamarin.Forms 2.2 anlatılmıştır [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) yerini aldı özelliği [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) özelliği. Daha fazla bilgi için bkz: [otomatikleştirmek Xamarin.Forms Xamarin.UITest ve Test bulut ile test](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md).

## <a name="summary"></a>Özet

Xamarin.Forms uygulamalar genellikle özdeş bir görünüme sahip birden çok denetim içerir. Her denetim görünümünü ayarlama yinelenen olabilir ve hataya. Bunun yerine, stiller, denetimin görünümünü özelleştirme gruplandırma ve denetim türünde kullanılabilen ayarları özellikler tarafından oluşturulabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [stili](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Ayarlama](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
