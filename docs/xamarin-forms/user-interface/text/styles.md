---
title: Stilleri
description: Xamarin.Forms stili metin
ms.topic: article
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: d1aadd808ea18023ee0a22259ddb36c0a8daa802
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="styles"></a>Stilleri

_Xamarin.Forms stili metin_


Stilleri etiketleri, girişleri ve düzenleyiciler görünümünü ayarlamak için kullanılabilir. Stilleri kez tanımlanmış ve çok sayıda görünümleri tarafından kullanılan, ancak bir stil yalnızca bir tür görünümlerle kullanılabilir.
Stilleri verilebilir bir `Key` ve seçmeli olarak belirli bir denetimin kullanılarak uygulanan `Style` özelliği.

Bu makalede aşağıdaki konuları içerir:

- **[Yerleşik stilleri](#Built-In_Styles)**  &ndash; stili metin tabanlı görünümlere uygulamanızı boyunca yerleşik stilleri kullanın.
- **[Özel stiller](#Custom_Styles)**  &ndash; yerleşik seçenekleri yeterli olmadığı durumlarda özel stiller tanımlayın.
- **[Stilleri uygulama](#Applying_Styles)**  &ndash; görünümlerinize özel ve yerleşik stilleri uygulayın.
- **[Erişilebilirlik](#Accessibility)**  &ndash; metin erişilebilirlik ayarlarını dikkate alır emin olun.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Yerleşik stilleri

Xamarin.Forms içeren birkaç [yerleşik](http://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) stilleri ortak senaryolar için:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Yerleşik stilleri birini uygulamak için kullanmak `DynamicResource` biçimlendirme uzantısı stilini belirlemek için:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# ' ta yerleşik stilleri seçildiği yer `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "Cihaz stilleri örneği")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Özel stiller

Stilleri ayarlayıcıları oluşur ve özellik değerleri olarak ayarlanır ve ayarlayıcılar özelliklerini oluşur.

C# ' ta boyutu 30 kırmızı metinle bir etiket için özel bir stil şu şekilde tanımlanır:

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

XAML'de:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

Kaynaklar (tüm stilleri dahil) içinde tanımlanan Not `ContentPage.Resources`, daha tanıdık eşdüzey olduğu `ContentPage.Content` öğesi.

![](styles-images/customstyle.png "Özel stiller örneği")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Stil Uygulama

Stil oluşturulduktan sonra tüm görünüm eşleşen uygulanabilir kendi `TargetType`.

XAML'de sağlayarak özel stiller görünümlerine uygulanan kendi `Style` özelliği ile bir `StaticResource` gerekli stili başvuran biçimlendirme uzantısı:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

C# ' ta stilleri ya da doğrudan bir görünüme uygulanan veya için eklenebilir ve sayfanın alınan `ResourceDictionary`. Doğrudan eklemek için:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Ekleme ve sayfanın almak için `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Erişilebilirlik ayarlarını yanıt gerektiğinden yerleşik stiller farklı uygulanır. XAML yerleşik stilleri uygulamak için `DynamicResource` biçimlendirme uzantısı kullanılır:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# ' ta yerleşik stilleri seçildiği yer `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Erişilebilirlik

Erişilebilirlik tercihleri saygı kolaylaştırmak için yerleşik stilleri yok. Bir kullanıcı erişilebilirlik tercihlerine uygun şekilde ayarlarsa yerleşik stilleri birini kullanırken, yazı tipi boyutlarını otomatik olarak artacaktır.

Aşağıdaki örnek etkin ve devre dışı erişilebilirlik ayarlarla yerleşik stilleri ile biçimlendirilmiş görünümleri aynı sayfasının göz önünde bulundurun:

Devre dışı:

![](styles-images/pre-access.png "Erişilebilirlik devre dışı aygıt stilleri")

Etkin:

![](styles-images/post-access.png "Etkin Erişilebilirlik ile cihaz stilleri")

Erişilebilirlik emin olmak için uygulamanızın içinde metin ilgili stilleri için yerleşik stilleri temeli olarak kullanılır ve stiller tutarlı bir şekilde kullanıyorsanız emin olun. Bkz: [stilleri](~/xamarin-forms/user-interface/styles/index.md) genişletme ve genel stilleri ile çalışma hakkında daha fazla bilgi.


## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 12 Xamarin.Forms ile mobil uygulamaları oluşturma](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [stili](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
