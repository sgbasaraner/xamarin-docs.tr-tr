---
title: Xamarin.Forms metin stilleri
description: Bu makalede açıklanır Xamarin.Forms uygulamalarında stil metin öğreneceksiniz. Stilleri çok kez tanımlanmış ve çok sayıda görünümleri tarafından kullanılan, ancak bir stil yalnızca tek bir görünüm ile kullanılabilir.
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 73aa3115e92d1e3954f5ae3eb8dcb84abf9d9efb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998774"
---
# <a name="xamarinforms-text-styles"></a>Xamarin.Forms metin stilleri

_Xamarin.Forms içinde stil metin_

Stilleri, etiketler, girdileri ve düzenleyicileri görünümünü ayarlamak için kullanılabilir. Stilleri çok kez tanımlanmış ve çok sayıda görünümleri tarafından kullanılan, ancak bir stil yalnızca tek bir görünüm ile kullanılabilir.
Stilleri verilebilir bir `Key` ve seçmeli olarak belirli bir denetimin kullanılarak uygulanan `Style` özelliği.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Yerleşik stilleri

Xamarin.Forms içeren birkaç [yerleşik](xref:Xamarin.Forms.Device.Styles) sık karşılaşılan senaryolara yönelik stilleri:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Yerleşik stil uygulamak için kullanma `DynamicResource` stilini belirtmek için işaretleme uzantısı:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# ' ta yerleşik stilleri seçildiği yer `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "Cihaz stilleri örneği")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Özel stilleri

Ayarlayıcıları stilleri oluşur ve ayarlayıcılar özelliklerini oluşur ve özelliklerini değerleri olarak ayarlanır.

C# ' ta boyutu 30 kırmızı metni içeren bir etiket için özel bir stil şu şekilde tanımlanır:

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

XAML içinde:

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

Kaynakları (tüm stilleri dahil) içinde tanımlandığına dikkat edin `ContentPage.Resources`, daha tanıdık bir eşdüzeyi olduğu `ContentPage.Content` öğesi.

![](styles-images/customstyle.png "Özel stilleri örneği")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Stil Uygulama

Bir stil oluşturulduktan sonra eşleşen görünüm uygulanabilir kendi `TargetType`.

XAML içinde sağlayarak özel stilleri görünümlerine uygulanan kendi `Style` özelliğiyle bir `StaticResource` gerekli stili başvuran işaretleme uzantısı:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

C# ' ta stilleri ya da doğrudan bir görünüme uygulanan veya için eklenebilir ve sayfanın alınan `ResourceDictionary`. Doğrudan eklemek için:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Ekleme ve sayfa almak için `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Erişilebilirlik ayarlarını yanıt gerektiği için yerleşik stilleri farklı şekilde uygulanır. XAML, yerleşik stilleri uygulamak için `DynamicResource` işaretleme uzantısı kullanılır:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C# ' ta yerleşik stilleri seçildiği yer `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Erişilebilirlik

Erişilebilirlik tercihlerini saygı daha kolay hale getirmek için yerleşik stilleri yok. Bir kullanıcı Erişilebilirlik tercihlerini uygun şekilde ayarlaması durumunda yerleşik stilleri birini kullanırken, yazı tipi boyutlarını otomatik olarak artırır.

Aşağıdaki örnek, aynı sayfa etkin ve devre dışı erişilebilirlik ayarlarla yerleşik stilleri ile biçimlendirilmiş görünümlerinin göz önünde bulundurun:

Devre dışı:

![](styles-images/pre-access.png "Cihaz stilleri ile erişilebilirlik devre dışı bırakıldı")

Etkin:

![](styles-images/post-access.png "Cihaz stilleri Etkin Erişilebilirlik")

Erişilebilirlik için uygulamanızın içinde herhangi bir metin ile ilgili stilleri için yerleşik stilleri temeli olarak kullanılır ve stilleri tutarlı bir şekilde kullanıyorsanız emin olun. Bkz: [stilleri](~/xamarin-forms/user-interface/styles/index.md) genişletme ve genel stilleri ile çalışma hakkında daha fazla bilgi.


## <a name="related-links"></a>İlgili bağlantılar

- [12. Bölüm Xamarin.Forms ile mobil uygulamalar oluşturma](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
- [Metin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Stil](xref:Xamarin.Forms.Style)
