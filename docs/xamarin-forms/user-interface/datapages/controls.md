---
title: DataPages denetimleri başvurusu
description: Bu makalede Xamarin.Forms DataPages NuGet paketinin kullanılabilir denetimler sunar.
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: c907d55f09d334e167c831a19f9d0edc4c97732f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38866528"
---
# <a name="datapages-controls-reference"></a>DataPages denetimleri başvurusu

![](~/media/shared/preview.png "Bu API, şu anda Önizleme aşamasındadır")

> [!IMPORTANT]
> DataPages gerektiren bir [Xamarin.Forms tema](~/xamarin-forms/user-interface/themes/index.md) işlemek için başvuru.


Xamarin.Forms DataPages Nuget birkaç veri kaynağına bağlama yararlanabilirsiniz denetimleri içerir.

XAML içinde bu denetimleri kullanmak için ad alanına dahil olun, örneğin `xmlns:pages` aşağıdaki bildirimi:

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

Aşağıdaki örnekler `DynamicResource` çalışmak için projenin kaynak sözlüğünde gerekecektir başvuruları. Derleme ilişkin bir örnek bulunmaktadır bir [özel denetim](#custom)

## <a name="built-in-controls"></a>Yerleşik denetimler

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

`HeroImage` Denetim dört özelliklere sahiptir:

* Metin
* Ayrıntısı
* ImageSource
* En boy

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Android'de HeroImage denetimi") ![](controls-images/heroimage-dark-android.png "Android HeroImage denetimi")

**iOS**

![](controls-images/heroimage-light-ios.png "İos'ta HeroImage denetimi") ![](controls-images/heroimage-dark-ios.png "ios'ta HeroImage denetimi")


<a name="listitem" />

### <a name="listitem"></a>ListItem

`ListItem` Denetimin düzenini yerel iOS benzer ve Android liste veya tablo satırları, ancak bunu ayrıca normal bir görünüm olarak kullanılabilir. Örnekte aşağıdaki kod içinde barındırılan gösterilen bir `StackLayout`, ancak bu verilere bağlı scolling liste denetimlerinde kullanılabilir.

Beş özellik vardır:

* Başlık
* Ayrıntısı
* ImageSource
* PlaceholdImageSource
* En boy

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

Bu ekran görüntüleri Göster `ListItem` iOS ve Android platformlarında açık ve koyu tema kullanma:

**Android**

![](controls-images/listitem-light-android.png "Android'de ListItem denetimi") ![](controls-images/listitem-dark-android.png "Android ListItem denetimi")

**iOS**

![](controls-images/listitem-light-ios.png "İos'ta ListItem denetimi") ![](controls-images/listitem-dark-ios.png "ios'ta ListItem denetimi")


## <a name="custom-control-example"></a>Özel denetim örneği

Bu özel amacı `CardView` yerel Android CardView benzeyecek şekilde denetimidir.

Bu üç özellik içerir:

* Metin
* Ayrıntısı
* ImageSource

Aşağıdaki kod gibi görünür bir özel denetim hedefidir (unutmayın özel `xmlns:local` gereklidir geçerli derlemeyi başvuran):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

Ekran görüntüleri, karşılık gelen yerleşik açık ve koyu Tema renkleri kullanarak aşağıdaki gibi görünmelidir:

**Android**

![](controls-images/cardview-light-android.png "Android'de CardView özel denetim") ![](controls-images/cardview-dark-android.png "Android CardView özel denetim")

**iOS**

![](controls-images/cardview-light-ios.png "İOS özel denetimde CardView") ![](controls-images/cardview-dark-ios.png "ios'ta CardView özel denetim")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>Özel kart görünümü oluşturma

1. [DataView alt sınıfı](#1)
2. [Yazı tipi, Düzen ve kenar boşlukları tanımlayın](#2)
3. [Denetimin çocuklar için stil oluşturma](#3)
4. [Denetim şablonu düzen oluşturma](#4)
5. [Temaya özgü kaynak Ekle](#5)
6. [ControlTemplate CardView sınıfı için ayarlayın](#6)
7. [Bir sayfasına denetim ekleme](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. DataView alt sınıfı

C# öğesinin `DataView` bağlanabilir denetimin özelliklerini tanımlar.

```csharp
public class CardView : DataView
{
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create ("Text", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Text
    {
        get { return (string)GetValue (TextProperty); }
        set { SetValue (TextProperty, value); }
    }

    public static readonly BindableProperty DetailProperty =
        BindableProperty.Create ("Detail", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Detail
    {
        get { return (string)GetValue (DetailProperty); }
        set { SetValue (DetailProperty, value); }
    }

    public static readonly BindableProperty ImageSourceProperty =
        BindableProperty.Create ("ImageSource", typeof (ImageSource), typeof (CardView), null, BindingMode.TwoWay);

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue (ImageSourceProperty); }
        set { SetValue (ImageSourceProperty, value); }
    }

    public CardView()
    {
    }
}
```

<a name="2" />

#### <a name="2-define-font-layout-and-margins"></a>2. Yazı tipi, Düzen ve kenar boşlukları tanımlayın

Denetim Tasarımcısı kullanıcı arabirimi tasarımı özel denetim için bir parçası olarak bu değerleri şekil. Platforma özgü özellikleri gerekli olduğu `OnPlatform` öğe kullanılır.

Bazı değerler başvuran Not `StaticResource`s – bunlar içinde tanımlanabilir [5. adım](#5).

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewBackgroundColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewBackgroundColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewBackgroundColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewShadowSize">
        <On Platform="iOS" Value="2" />
        <On Platform="Android" Value="5" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewCornerRadius">
        <On Platform="iOS" Value="0" />
        <On Platform="Android" Value="4" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3. Denetimin çocuklar için stil oluşturma

Özel denetim içinde kullanılacak alt oluşturmak üzere tanımlanan tüm öğeleri başvurusu:

```xml
<!-- EXPLICIT STYLES (will be Classes) -->
<Style TargetType="Label" x:Key="CardViewTextStyle">
    <Setter Property="FontSize" Value="{ StaticResource CardViewTextFontSize }" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewTextTextColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewTextlMargin }" />
    <Setter Property="HorizontalTextAlignment" Value="Start" />
</Style>

<Style TargetType="Label" x:Key="CardViewDetailStyle">
    <Setter Property="HorizontalTextAlignment" Value="Start" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewDetailTextColor }" />
    <Setter Property="FontSize" Value="{ StaticResource CardViewDetailFontSize }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewDetailMargin }" />
</Style>

<Style TargetType="Image" x:Key="CardViewImageImageStyle">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="Center" />
    <Setter Property="WidthRequest" Value="220"/>
    <Setter Property="HeightRequest" Value="165"/>
</Style>
```

<a name="4" />

#### <a name="4-create-the-control-layout-template"></a>4. Denetim şablonu düzen oluşturma

Özel denetimin görsel tasarım yukarıda tanımlanan kaynakları kullanarak, Denetim şablonunda açıkça bildirilir:

```xml
<!--- CARDVIEW -->
<ControlTemplate x:Key="CardViewControlControlTemplate">
  <StackLayout
    Spacing="0"
    BackgroundColor="{ TemplateBinding BackgroundColor }"
  >

    <!-- CARDVIEW IMAGE -->
    <Image
      Source="{ TemplateBinding ImageSource }"
      HorizontalOptions="FillAndExpand"
      VerticalOptions="StartAndExpand"
      Aspect="AspectFill"
      Style="{ StaticResource CardViewImageImageStyle }"
    />

    <!-- CARDVIEW TEXT -->
    <Label
      Text="{ TemplateBinding Text }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewTextStyle }"
    />


    <!-- CARDVIEW DETAIL -->
    <Label
      Text="{ TemplateBinding Detail }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewDetailStyle }" />

  </StackLayout>

</ControlTemplate>
```

<a name="5" />

#### <a name="5-add-the-theme-specific-resources"></a>5. Temaya özgü kaynak Ekle

Bu, özel bir denetim olduğu için kaynak sözlüğü'nu kullanarak tema eşleşen kaynak ekleyin:

##### <a name="light-theme-colors"></a>Açık Tema renkleri

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Koyu Tema renkleri

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

<a name="6" />

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. ControlTemplate CardView sınıfı için ayarlayın

Son olarak, oluşturulan C# sınıfı olun [1. adım](#1) tanımlı denetim şablonu kullanan [4. adım](#4) kullanarak bir `Style` `Setter` öğesi

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. Bir sayfasına denetim ekleme

`CardView` Denetimi bir sayfaya artık eklenebilir. Aşağıdaki örnek, barındırılan gösterir bir `StackLayout`:

```xaml
<StackLayout Spacing="0">
  <local:CardView
    Margin="12,6"
    ImageSource="{ DynamicResource CardViewImage }"
    Text="CardView Text"
    Detail="CardView Detail"
  />
</StackLayout>

```
