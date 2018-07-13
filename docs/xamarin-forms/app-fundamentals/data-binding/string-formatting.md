---
title: Xamarin.Forms dize biçimlendirmesi
description: Bu makalede, biçimlendirmek ve dizeleri nesneleri görüntülemek için Xamarin.FOrms veri bağlamaları kullanmayı açıklar. StringFormat bağlamanın .NET standart bir biçimlendirme dizesi bir yer tutucu ayarlayarak elde edilir.
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: a876e81c67b6ec61a2cb29143cb001a7d6160032
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998153"
---
# <a name="xamarinforms-string-formatting"></a>Xamarin.Forms dize biçimlendirmesi

Bazen bir nesnenin ya da değer dize gösterimini görüntülenecek veri bağlamaları kullanmak kullanışlıdır. Örneğin, kullanmak isteyebilirsiniz bir `Label` geçerli değerini görüntülemek için bir `Slider`. Bu veri bağlamada `Slider` kaynak ve hedef `Text` özelliği `Label`.

Dizeleri kodda görüntülerken, statik en güçlü araçtır [ `String.Format` ](xref:System.String.Format(System.String,System.Object)) yöntemi. Biçimlendirilen değerleri yanı sıra diğer metin içerebilir ve biçimlendirme kodları nesnelerin çeşitli türleri için özel bir biçimlendirme dizesi içerir. Bkz: [NET'teki biçimlendirme türleri](/dotnet/standard/base-types/formatting-types/) makale biçimlendirme dizesi hakkında daha fazla bilgi.

## <a name="the-stringformat-property"></a>StringFormat özelliği

Bu özellik veri bağlamaları ile gerçekleştirilen: ayarladığınız [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat) özelliği `Binding` (veya [ `StringFormat` ](xref:Xamarin.Forms.Xaml.BindingExtension.StringFormat) özelliği `Binding` işaretleme uzantısı) için bir Standart .NET biçimlendirme dizesi ile bir yer tutucu:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Biçimlendirme dizesi XAML ayrıştırıcı başka bir XAML işaretleme uzantısı ve küme ayraçlarının değerlendirmesini önlemek amacıyla tek tırnak işareti (kesme işareti) karakterleri tarafından ayrılmış dikkat edin. Aksi takdirde, dize bir çağrıda bir kayan nokta değerini görüntülemek için kullandığınız aynı dize tek tırnak işareti olmayan `String.Format`. Biçimlendirme belirtimini `F2` iki ondalık basamakla görüntülenecek değer neden olur.

`StringFormat` Özelliği yalnızca anlamlı hedef özelliği türü olduğunda `string`, ve bağlama modu `OneWay` veya `TwoWay`. İki yönlü bağlamaları için `StringFormat` kaynaktan hedefe geçirme değerleri için yalnızca geçerlidir.

Sonraki makalede anlatıldığı [bağlama yolu](binding-path.md), veri bağlamaları oldukça karmaşık ve karışık gelebilir. Bu veri bağlamaları hata ayıklarken ekleyebileceğiniz bir `Label` ile XAML dosyasına bir `StringFormat` bazı Ara sonuçlar görüntülenecek. Yalnızca bir nesnenin türü görüntülemek için kullanıyor olsanız bile, bu yararlı olabilir.

**Biçimlendirme dizesi** sayfası gösterilmektedir çeşitli kullanımları `StringFormat` özelliği:

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

Bağlamalar `Slider` ve `TimePicker` biçim belirtimleri kullanımı için belirli Göster `double` ve `TimeSpan` veri türleri. `StringFormat` Metni görüntüler `Entry` görünümü çift tırnak işareti kullanarak biçimlendirme dizesi belirtmek nasıl gösterir `&quot;` HTML varlık.

Sonraki bölüm XAML dosyasında bir `StackLayout` ile bir `BindingContext` kümesine bir `x:Static` statik başvuruları işaretleme uzantısı `DateTime.Now` özelliği. İlk bağlama özelliğe sahip değil:

```xaml
<Label Text="{Binding}" />
```

Bu basitçe görüntüler `DateTime` değerini `BindingContext` varsayılan biçimlendirme ile. İkinci bağlama görüntüler `Ticks` özelliği `DateTime`, diğer iki bağlamaları görüntüleme `DateTime` kendisi ile özel biçimlendirme. Bu bildirimde `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Sola veya sağa süslü ayraçlar, biçimlendirme dizesindeki görüntülenecek ihtiyacınız varsa, bunları bir çift kullanmanız yeterlidir.

Son bölümde kümeleri `BindingContext` değerine `Math.PI` ve varsayılan biçimlendirme ve sayısal biçimlendirme iki farklı tür ile görüntüler.

Üç tüm platformlarda çalışan bir program şöyledir:

[![Dize biçimlendirmesi](string-formatting-images/stringformatting-small.png "dize biçimlendirme")](string-formatting-images/stringformatting-large.png#lightbox "dize biçimlendirmesi")

## <a name="viewmodels-and-string-formatting"></a>Viewmodel'lar ve biçimlendirme dizesi

Kullanırken `Label` ve `StringFormat` ayrıca bir ViewModel hedefi bir görünüm değerini görüntülemek için ya da bağlama görünümünden tanımlayabilirsiniz `Label` veya için ViewModel `Label`. Görünümü ViewModel arasındaki bağlamaları çalıştığını uyguladığından genel olarak, ikinci en iyi yaklaşımdır.

Bu yaklaşım gösterilen **daha iyi renk seçici** olarak aynı ViewModel kullanan örnek **basit Renk Seçici** gösterilen programı [ **bağlama modu** ](binding-mode.md) makale:

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

Artık üç çift vardır `Slider` ve `Label` aynı bağlı öğeleri kaynak özelliğinde `HslColorViewModel` nesne. Tek fark `Label` sahip bir `StringFormat` her görüntülenecek özelliği `Slider` değeri.

[![Daha iyi, seçici renk](string-formatting-images/bettercolorselector-small.png "daha iyi, seçici renk")](string-formatting-images/bettercolorselector-large.png#lightbox "daha iyi, seçici renk")

Geleneksel iki basamaklı onaltılık biçimde RGB (kırmızı, yeşil, mavi) değerleri nasıl görüntüleyebilir merak ediyor olabilirsiniz. Bu tamsayı değerleri doğrudan kullanılabilir olmayan `Color` yapısı. ViewModel içinde renk bileşenlerinin tamsayı değerleri hesaplamak ve bunları özellik olarak kullanıma sunmak için tek bir çözüm olacaktır. Bunları daha sonra biçimlendirebilirsiniz kullanarak `X2` belirtimi biçimlendirme.

Başka bir yaklaşım daha geneldir: yazabileceğiniz bir *bağlama değer dönüştürücü* sonraki makalede anlatıldığı gibi [ **bağlama değeri dönüştürücüleri**](converters.md).

Sonraki makalede, ancak keşfediyor [ **bağlama yolu** ](binding-path.md) daha fazla ayrıntı ve alt özellikleri ve öğeleri koleksiyonlardaki başvurmak için nasıl kullanabileceğinizi gösterir.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölümden Xamarin.Forms kitabı](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
