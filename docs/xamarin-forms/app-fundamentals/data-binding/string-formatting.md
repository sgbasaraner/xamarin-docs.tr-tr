---
title: Biçimlendirme dizesi
description: Biçimlendirmek ve dize olarak nesneleri görüntülemek için veri bağlamaları kullanın
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 4e143f650c3cde7577def1a95e53b207608a088a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="string-formatting"></a>Biçimlendirme dizesi

Bazen bir nesneye veya değeri dize gösterimini görüntülemek üzere veri bağlamaları kullanmak uygundur. Örneğin, kullanmak istediğiniz bir `Label` geçerli değerini görüntülemek için bir `Slider`. Bu veri bağlamada `Slider` kaynak ve hedef `Text` özelliği `Label`.

Dizeleri kodda görüntülerken, statik en güçlü araçtır [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) yöntemi. Biçimlendirilen değerleri yanı sıra başka bir metin içerebilir ve biçimlendirme kodları çeşitli nesneleri belirli biçimlendirme dizesini içerir. Bkz: [.NET biçimlendirme türleri](/dotnet/standard/base-types/formatting-types/) biçimlendirme dizesi hakkında daha fazla bilgi için makalenin.

## <a name="the-stringformat-property"></a>StringFormat özelliği

Bu özellik veri bağlamaları devredilir: ayarladığınız [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) özelliği `Binding` (veya [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.StringFormat/) özelliği `Binding` biçimlendirme uzantısı) için bir biçimlendirme dizesi bir yer tutucu ile standart .NET:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Biçimlendirme dizesi başka bir XAML biçimlendirme uzantısı olarak süslü ayraçlar değerlendirmesini kaçının XAML ayrıştırıcısı yardımcı olması için tek tırnaklı (kesme) karakter tarafından sınırlandırılır dikkat edin. Aksi takdirde, dize tek tırnak karakteri, çağrıda bir kayan nokta değerini görüntülemek için kullandığınız aynı dizedir olmadan `String.Format`. Biçimlendirme belirtimini `F2` değeri ile iki ondalık basamak görüntülenmesine neden olur.

`StringFormat` Özelliği yalnızca anlamlı hedef özelliği türü olduğunda `string`, ve bağlama modu `OneWay` veya `TwoWay`. İki yönlü bağlamaları için `StringFormat` yalnızca kaynak sunucudan hedef geçirme değerleri için geçerli değil.

Sonraki makalede göreceğiniz gibi [bağlama yolu](binding-path.md), oldukça karmaşık ve karışık veri bağlamaları haline gelir. Bu veri bağlamaları hata ayıklama sırasında ekleyebileceğiniz bir `Label` ile XAML dosyasına bir `StringFormat` bazı Ara sonuçları görüntülemek için. Yalnızca bir nesnenin türü görüntülemek için kullandığınız olsa bile, yararlı olabilir.

**Biçimlendirme dizesi** sayfa birkaç kullanımları gösterilmektedir `StringFormat` özelliği:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="DataBindingDemos.StringFormattingPage"
             Title="String Formatting">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Blue" />
                <Setter Property="HeightRequest" Value="2" />
                <Setter Property="Margin" Value="0, 5" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <Slider x:Name="slider" />
        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The slider value is {0:F2}'}" />

        <BoxView />

        <TimePicker x:Name="timePicker" />
        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time,
                              StringFormat='The TimeSpan is {0:c}'}" />

        <BoxView />

        <Entry x:Name="entry" />
        <Label Text="{Binding Source={x:Reference entry},
                              Path=Text,
                              StringFormat='The Entry text is &quot;{0}&quot;'}" />

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:DateTime.Now}">
            <Label Text="{Binding}" />
            <Label Text="{Binding Path=Ticks,
                                  StringFormat='{0:N0} ticks since 1/1/1'}" />
            <Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
            <Label Text="{Binding StringFormat='The long date is {0:D}'}" />
        </StackLayout>

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:Math.PI}">
            <Label Text="{Binding}" />
            <Label Text="{Binding StringFormat='PI to 4 decimal points = {0:F4}'}" />
            <Label Text="{Binding StringFormat='PI in scientific notation = {0:E7}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Bağlantılarına `Slider` ve `TimePicker` biçim belirtimleri kullanımını göstermek için belirli `double` ve `TimeSpan` veri türleri. `StringFormat` Metinden görüntüler `Entry` görünümü gösterilmektedir kullanarak biçimlendirme dizesinde çift tırnak işaretleri belirleme konusunda `&quot;` HTML varlığı.

XAML dosyası sonraki bölümünde bir `StackLayout` ile bir `BindingContext` kümesine bir `x:Static` statik başvuran biçimlendirme uzantısı `DateTime.Now` özelliği. İlk bağlama hiçbir özelliklere sahiptir:

```xaml
<Label Text="{Binding}" />
```

Bu basitçe görüntüler `DateTime` değerini `BindingContext` varsayılan biçimlendirmeye sahip. İkinci bağlama görüntüler `Ticks` özelliği `DateTime`, diğer iki bağlamaları görüntülemek `DateTime` belirli biçimlendirme kendisiyle. Bu bildirimde `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Biçimlendirme dizesi içinde sola veya sağa süslü ayraçlar görüntülemek gerekiyorsa, yalnızca bir çiftinden kullanın.

Son bölümü kümeleri `BindingContext` değerine `Math.PI` varsayılan biçimlendirme ve sayısal biçimlendirme iki farklı türleri ile görüntüler.

Aşağıda, tüm üç platformlarında çalışan program verilmiştir:

[![Dize biçimlendirme](string-formatting-images/stringformatting-small.png "dize biçimlendirme")](string-formatting-images/stringformatting-large.png#lightbox "dize biçimlendirme")

## <a name="viewmodels-and-string-formatting"></a>ViewModels ve biçimlendirme dizesi

Kullanırken `Label` ve `StringFormat` bir ViewModel hedef aynı zamanda bir görünüm değerini görüntülemek için ya da bağlama görünümünden tanımlayabilirsiniz `Label` veya ViewModel `Label`. ViewModel ve görünüm arasında bağlamaları çalıştığınız uyguladığından genel olarak, ikinci en iyi yaklaşımdır.

Bu yaklaşım gösterilen **daha iyi renk seçici** olarak aynı ViewModel kullanan örnek **basit Renk Seçici** gösterilen program [ **bağlama modu** ](binding-mode.md) makale:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.BetterColorSelectorPage"
             Title="Better Color Selector">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Artık üç çiftlerini olan `Slider` ve `Label` aynı bağlı öğeleri kaynak özelliğinde `HslColorViewModel` nesnesi. Tek fark `Label` sahip bir `StringFormat` her görüntülenecek özellik `Slider` değeri.

[![Daha iyi, seçici renk](string-formatting-images/bettercolorselector-small.png "daha iyi, seçici renk")](string-formatting-images/bettercolorselector-large.png#lightbox "daha iyi, seçici renk")

Geleneksel iki basamaklı onaltılık biçimde RGB (kırmızı, yeşil, mavi) değerleri nasıl görüntüleneceğini merak ediyor olabilirsiniz. Bu tamsayı değerleri kullanılabilir doğrudan olmayan `Color` yapısı. Renk bileşenleri ViewModel içinde tamsayı değerleri hesaplamak ve özellikleri olarak göstermek için bir çözüm olacaktır. Ardından bunları biçimlendirebilirsiniz kullanarak `X2` belirtimi biçimlendirme.

Başka bir yaklaşım daha genel: yazabileceğiniz bir *bağlama değer dönüştürücüsü* sonraki makalesinde ele alındığı gibi [ **bağlama değer dönüştürücüler**](converters.md).

Sonraki makalede ancak ele [ **bağlama yolu** ](binding-path.md) daha ayrıntılı ve alt özellikleri ve öğeleri koleksiyonlarda başvurmak için nasıl kullanabileceğinizi gösterir.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama gösterileri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
